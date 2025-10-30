---
layout: home
title: "How the Internet Actually Works"
date: 2025-10-30
categories: "Internet"
tags: [Internet, Protocols, Web Servers, Layers, Working, Science]
image: 'https://github.com/user-attachments/assets/347f5343-0d2a-40be-96d6-714e23c67f6c'
---

# 🔌 How the Internet Actually Works — A Deep, Friendly Walkthrough 🚀🌐

Want the whole truth — layers, packets, routing, tools, and every little handshake? Buckle up. I’ll explain the Internet from the metal wires and radio waves all the way to the browser tab that shows cat videos. Expect diagrams, emojis, and practical tools you can use. 🛠️😺

<img width="1024" height="1536" alt="ChatGPT Image Oct 30, 2025, 11_33_30 PM" src="https://github.com/user-attachments/assets/347f5343-0d2a-40be-96d6-714e23c67f6c" />

---

## TL;DR (Quick summary)

The Internet moves data by chopping it into packets, wrapping those packets in protocol headers at each layer (encapsulation), sending them across physical links through switches and routers, using routing protocols (BGP/OSPF) to find paths, and using services like DNS and CDNs to map names to addresses and speed delivery. Transport (TCP/UDP) manages reliability and timing; encryption (TLS) secures it. Tools like `ping`, `traceroute`, `tcpdump`, `wireshark`, `dig`, and `curl` let you inspect and debug it. 🔍

---

# 1) Quick glossary — essential terms you’ll see again

* **Packet / Frame** — unit of data on the network (packet = network layer; frame = data link layer). 📦
* **IP address** — logical address (IPv4 like `192.0.2.5`, IPv6 like `2001:db8::1`). 🌍
* **MAC address** — physical NIC address (e.g., `00:0a:95:9d:68:16`) used on LANs. 🧷
* **Port** — endpoint inside a host (`80` for HTTP, `443` for HTTPS). 🔢
* **Router / Switch** — forwarders: routers move packets between networks, switches forward frames within LANs. ↔️
* **DNS** — maps domain names (example.com) to IP addresses. 🧭
* **NAT** — translates private IPs to public ones (home routers). 🕵️‍♂️
* **BGP / OSPF** — routing protocols for inter-domain (BGP) and intra-domain (OSPF) decisions. 🗺️
* **TLS** — cryptographic layer securing HTTP, email, etc. 🔐
* **CDN** — distributed caching layer (Cloudflare, Akamai) to speed content. 🗃️

---

# 2) Two main reference models: OSI vs TCP/IP

The OSI 7-layer model is a conceptual guide; TCP/IP is what actually gets used.

**OSI (7 layers)** — Application, Presentation, Session, Transport, Network, Data Link, Physical.
**TCP/IP (4 layers)** — Application, Transport, Internet (Network), Link (Data link + Physical).

I’ll often speak in TCP/IP terms because that’s practical: **Link → Internet → Transport → Application**.

---

# 3) Encapsulation — how data looks as it travels

Imagine sending an HTTP request. Each layer adds a header (and sometimes a footer):

```
[ Ethernet Frame Header | IP Header | TCP Header | HTTP Request Body | CRC ]
```

* At the host: application builds HTTP -> hands to TCP -> TCP adds seq/ack -> hands to IP -> IP adds src/dst -> handed to NIC -> NIC frames into Ethernet + checksum -> physical wire.
* At each router: physical → data link processed → IP examined → forwarded → new data link header for next hop.

---

# 4) Step-by-step: what happens when you open `https://example.com` in your browser

I’ll use concrete numbers and show what each system does.

1. **You type URL and press Enter**
   Browser parses URL; determines hostname `example.com` and port `443` (default for HTTPS).

2. **DNS resolution** (to an IP)

   * Browser asks OS resolver → resolver asks recursive DNS resolver (usually ISP) → recursive resolver may query root → TLD → authoritative name server and returns an A/AAAA record (e.g., `93.184.216.34`).
   * CNAMEs may redirect; caches reduce repeated lookups.
     Result: you have server IP and maybe TTL.

3. **Local routing & ARP (if same LAN)**

   * If server IP is not on your subnet, use default gateway.
   * To reach gateway, host resolves gateway’s MAC via ARP (Address Resolution Protocol). ARP: “Who has IP X? Tell MAC Y.” 🧾

4. **TCP three-way handshake (for TCP)**

   * Client -> Server: `SYN` (seq = x)
   * Server -> Client: `SYN-ACK` (seq = y, ack = x+1)
   * Client -> Server: `ACK` (ack = y+1)
     Now the connection is established. This sets initial sequence numbers and window sizes.

5. **TLS handshake (for HTTPS)**

   * ClientHello (protocol versions, cipher suites, client random)
   * ServerHello (chosen cipher, server cert), Certificate sent, maybe ServerKeyExchange
   * Client verifies certificate (chain + CA) and sends KeyExchange (e.g., pre-master secret encrypted with server public key / uses ECDHE)
   * Both derive symmetric keys; send Finished messages.
     Time to encrypt application data.

6. **HTTP request/response**

   * Client sends `GET /` over TLS+TCP (encapsulated).
   * Server processes request, sends back HTTP/2 or HTTP/1.1 response with headers + body.
   * Body may be chunked; server may set caching headers (Cache-Control), cookies, etc.

7. **Connection termination**

   * `FIN` / `ACK` exchange or keep-alives used; for HTTP/1.1 persistent connections may stay open.

8. **Data path across the network**

   * Packets traverse multiple routers. Each router uses the **IP destination** to consult its routing table and forwards the packet out an interface (longest-prefix match). TTL is decremented; if TTL hits 0, router drops packet and returns ICMP Time Exceeded (used by traceroute).

---

# 5) The Physical & Link layers — the wires and switches

* **Physical layer**: copper (Ethernet twisted pair), fiber optics, Wi-Fi (radio waves). Key characteristics: bandwidth, latency, signal encoding. ⚡
* **Data Link layer**: Ethernet frames, MAC addressing, VLANs, switching. Switches learn MAC addresses by reading source MAC of frames and populate a MAC table (CAM).
* **MTU**: Maximum Transmission Unit (common Ethernet MTU = 1500 bytes). Larger packets may be fragmented at IP layer if necessary (IPv4 fragmentation). Fragmentation is expensive — avoid it.

---

# 6) The Internet layer — IP addressing & routing

* **IPv4** (32-bit) and **IPv6** (128-bit). Devices have one or more IPs.
* **Subnetting** determines what IPs are “local” vs across a router.
* **Routing**: Routers hold routing tables with prefixes and next-hops. When a packet arrives, router performs **longest-prefix match** to choose route.

### Routing types

* **Static**: admin sets routes manually.
* **Dynamic**: protocols update routes automatically.

### Important routing protocols

* **BGP (Border Gateway Protocol)** — used between autonomous systems (ASes) on the Internet. Key attributes: AS_PATH, LOCAL_PREF, MED. BGP is policy-based: operators decide which prefixes to advertise and accept. BGP determines the global Internet routes. 🌐
* **OSPF / IS-IS** — interior gateway protocols (IGPs) used inside large networks (AS) to compute shortest paths.
* **RIP, EIGRP** — older protocols (RIP mostly legacy).

BGP basics: ISPs peer and exchange prefixes; path selection uses attributes (local pref, shortest AS_PATH, origin, MED, eBGP/iBGP rules). Misconfigurations can create massive outages.

---

# 7) Transport layer — TCP vs UDP

### TCP (Transmission Control Protocol) — reliable, connection-oriented

* **Sequence numbers**, **ACKs**, **window** (flow control), **congestion control** (slow start, congestion avoidance, fast retransmit, fast recovery).
* **Retransmission timeout (RTO)** based on RTT estimations.
* **SACK** (selective acknowledgement) helps efficient retransmit.
* Common uses: HTTP(S), SMTP, FTP.

### UDP — unreliable, connectionless

* Minimal overhead. Good for DNS queries, VoIP, video streaming (when latency important). Protocols above UDP may implement their own reliability (QUIC does this differently).

### QUIC

* Runs over UDP, incorporates TLS and improved multiplexing and faster connection setup (used by HTTP/3). Not part of classic TCP/IP but increasingly important.

---

# 8) DNS — the Internet’s phonebook

* **Resolver**: usually your ISP / OS cache sends recursive queries.
* **Iterative queries**: resolver goes root → TLD → authoritative.
* **Records**: A, AAAA, CNAME, MX, TXT, NS, SOA, etc.
* TTL controls caching. Remember: DNS is critical — if DNS fails you can’t reach sites by name.

---

# 9) NAT and Firewalls — address translation & security

* **NAT (Network Address Translation)**: home router maps many private IPv4 addresses to one public IP using port translation (PAT). NAT complicates inbound connections, necessitating port forwarding or traversal techniques (STUN/TURN for WebRTC).
* **Firewalls**: control traffic via rules; stateful firewalls track connection states. Important for security but can block legitimate traffic if misconfigured.

---

# 10) Security: TLS, certificates, and PKI

* **TLS** provides encryption, integrity, and (optionally) client auth.
* **Certificates** are issued by CAs; browsers check chain of trust and revocation (OCSP/CRL).
* **Perfect Forward Secrecy (PFS)** achieved via ephemeral key exchanges (ECDHE).
* **HTTPS** = HTTP over TLS on TCP port 443.

---

# 11) Performance helpers: CDNs, caching, and load balancers

* **CDNs** replicate content close to users (edge servers) to reduce latency & offload origin.
* **Reverse proxies & load balancers** distribute requests (round-robin, least-connections, consistent hashing).
* **Caching headers** (`Cache-Control`, `ETag`) instruct browsers/CDNs how to cache resources.

---

# 12) Routing internals — BGP and how the Internet stitches together

* ISPs and large organizations run BGP to advertise IP prefixes. Each AS chooses preferred paths. BGP updates propagate route changes gradually.
* **Peering vs transit**: Peering = two networks exchange traffic directly (often settlement-free), Transit = you pay another ISP to carry your traffic.
* **Internet Exchange Points (IXPs)** are physical locations where networks peer. 🏢

---

# 13) Minute details: checksums, MTU, fragmentation, TTL

* **Checksums**: IP header checksum and transport checks (TCP/UDP) validate integrity of headers and data.
* **MTU fragmentation**: IPv4 can fragment; routers may drop packets if Don’t Fragment (DF) bit set; Path MTU Discovery used to find the largest safe packet size.
* **TTL (Time to Live)**: decremented at each hop; prevents routing loops. Used by `traceroute` because routers send back ICMP Time Exceeded when TTL expires.
* **ICMP**: used for control messages (ping/echo, destination unreachable, time exceeded).

---

# 14) Tools you should know (and how to use them briefly)

* `ping <host>` — test reachability (ICMP echo).
* `traceroute <host>` / `tracert` (Windows) — show path by increasing TTL.
* `mtr <host>` — combines ping + traceroute, shows per-hop loss/latency.
* `dig example.com` / `nslookup` — DNS queries and troubleshooting.
* `curl -v https://example.com` — fetch HTTP responses, show headers.
* `tcpdump -i eth0 host 93.184.216.34` — capture packets on interface (CLI).
* `wireshark` — GUI packet capture and deep analysis (follow TCP stream).
* `netstat` / `ss` — show sockets & connections on your host.
* `ip route`, `ip addr`, `ip neigh` — Linux IP stack configuration and neighbor (ARP) table.
* `iperf3` — performance testing (throughput).
* `nmap` — port scanning and host enumeration.

---

# 15) Example: packet-level view of a single HTTP request (simplified)

Assume client IP `10.0.0.2` (private) via NAT `203.0.113.5` to server `93.184.216.34`. Browser requests `GET /index.html` on port 443.

Encapsulation stages:

1. **Application:** `GET /index.html HTTP/1.1\r\nHost: example.com\r\n...`
2. **Transport (TCP):** header with source port `54321`, dest port `443`, seq/ack numbers.
3. **Network (IP):** `src=10.0.0.2 dst=93.184.216.34`
4. **Data Link (Ethernet):** `src=MAC_client dst=MAC_gateway` — frame on your LAN.
5. **Physical:** bits on wire or radio.

At NAT on your router:

* IP rewritten: `src=203.0.113.5 dst=93.184.216.34`, track mapping `(203.0.113.5:62000)->(10.0.0.2:54321)`. Reply uses NAT mapping to return packet to your device.

---

# 16) Troubleshooting recipe (practical steps)

1. `ping` the destination IP. No reply? Try next step.
2. `traceroute` to see where packets stop.
3. `dig`/`nslookup` to verify DNS resolution.
4. `curl -v` to check HTTP and TLS handshake.
5. `tcpdump` or `wireshark` on client/server to capture packet details.
6. Check local firewall, NAT, and port forwarding rules.
7. Check server logs and load balancer health.
8. For routing issues, inspect routing tables (`ip route`) and BGP sessions (for operators).

---

# 17) Real-world caveats & common gotchas

* **DNS caching** can mask DNS changes (TTL delays).
* **Asymmetric routing**: paths out and back may differ — complicates troubleshooting and stateful firewalls.
* **BGP hijacks / leaks**: accidental/malicious route announcements can redirect traffic.
* **Middleboxes (proxies, NATs, firewalls)** can break protocols that assume end-to-end transparency.
* **MTU issues** cause mysterious connectivity problems (if Path MTU Discovery blocked).
* **TLS certificate problems** (expired, mismatched domain) break HTTPS.

---

# 18) Modern trends briefly (so you’re up to speed)

* **HTTP/2 & HTTP/3 (QUIC)** improve performance via multiplexing and reduced latency.
* **IPv6 adoption** gradually increases to solve IPv4 scarcity and simplify end-to-end addressing.
* **Encryption everywhere** — TLS adoption is nearly universal for the web.
* **Edge computing & CDNs** push compute and caching closer to users.
* **SDN** (Software-Defined Networking) and NFV change how networks are managed (programmable control planes).

---

# 19) Handy diagrams (ASCII) — short & sweet

**End-to-end encapsulation**

```
[App: HTTP] -> [TCP: srcport,dstport,seq] -> [IP: srcIP,dstIP,ttl] -> [Ethernet: srcMAC,dstMAC] -> Physical
```

**Sequence: Browser → DNS → TCP → TLS → HTTP**

```
Browser
  └─DNS lookup────> DNS Resolver → Root → TLD → Auth
  └─TCP SYN────> Server
       ←─SYN-ACK─
  └─ACK─
  └─TLS handshake
  └─Encrypted HTTP GET ──> Server
  └─Encrypted HTTP Response <─ Server
```

---

# 20) Actionable tips & checklist for building/operating networks ✅

* Use IPv6 where possible; don't rely only on NAT.
* Monitor latency and packet loss (use `mtr` for long-term).
* Set sensible TCP timeouts and use connection pooling for HTTP.
* Use CDNs for static assets and TLS termination at the edge for performance.
* Harden BGP with RPKI and prefix filters to reduce hijacks.
* Keep MTU consistent across links or enable Path MTU Discovery properly.
* Use `tcpdump` + `wireshark` to capture suspected packet issues (filter by ip/port to cut noise).

---

## Final note — why this all matters

The Internet is an enormous, cooperative machine: dozens of protocols, countless devices, and human policies (peering agreements) all interacting. A single click triggers DNS, routing policy, multi-stage handshakes, and distributed caching — and yet (usually) it happens in a fraction of a second. Understanding these layers helps you debug problems, optimize performance, and design resilient systems. ⚙️💡
