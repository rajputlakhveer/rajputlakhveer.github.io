---
layout: home
title: "Ruby on Rails Production Setup"
date: 2026-08-06
categories: "Ruby On Rails"
tags: [Programming, Ruby On Rails, Software Engineer, Software Developer, Deployment, Production]
image: 'https://github.com/user-attachments/assets/f5dfd935-6b16-431b-95f2-16bd70d60af9'
---

# 🚀 Ruby on Rails Production Setup: The Complete Guide to Deploying Secure, Fast & Scalable Rails Applications 🌍

> **"Anyone can make a Rails app work locally. Great engineers make it reliable in production."**

Building an application is only **30% of the journey**. The remaining **70%** lies in deploying, monitoring, securing, scaling, and maintaining it in production.

Many developers deploy a Rails application successfully but encounter issues such as:

❌ Downtime during deployment
❌ Slow response times
❌ Memory leaks
❌ Database bottlenecks
❌ Security vulnerabilities
❌ Lost logs
❌ Broken assets
❌ SSL issues
❌ Background job failures

<img width="864" height="1821" alt="ChatGPT Image Aug 6, 2026, 09_51_57 PM" src="https://github.com/user-attachments/assets/f5dfd935-6b16-431b-95f2-16bd70d60af9" />

This guide covers **everything you should know before taking your Ruby on Rails application to production**, from infrastructure planning to monitoring and scaling.

---

# 📖 Table of Contents

1. Production Mindset
2. Infrastructure Planning
3. Choosing a Cloud Provider
4. Server Setup
5. Ruby Installation
6. PostgreSQL Setup
7. Redis Setup
8. Web Server
9. Application Server
10. Reverse Proxy
11. SSL Configuration
12. Environment Variables
13. Rails Credentials
14. Asset Pipeline
15. Active Storage
16. Background Jobs
17. Cron Jobs
18. Logging
19. Monitoring
20. Error Tracking
21. Performance Optimization
22. Security Checklist
23. Scaling
24. Deployment Strategies
25. Backup Strategy
26. Production Mistakes
27. Production Checklist

---

# 🎯 1. Production Mindset

Production is different from development.

Development focuses on:

* Writing features
* Debugging
* Rapid iteration

Production focuses on:

* Stability
* Security
* Performance
* Reliability
* Scalability

Think like a DevOps engineer.

---

# ☁️ 2. Choose Infrastructure

Popular choices include:

| Platform     | Best For          |
| ------------ | ----------------- |
| AWS EC2      | Full control      |
| AWS ECS      | Containers        |
| AWS EKS      | Kubernetes        |
| DigitalOcean | Small apps        |
| Render       | Easy deployment   |
| Railway      | Side projects     |
| Fly.io       | Fast deployment   |
| Heroku       | Simplicity        |
| Hatchbox     | Rails-specific    |
| Kamal        | Docker deployment |

---

# 🖥️ 3. Server Requirements

Typical stack:

```
Ubuntu 24.04 LTS

Ruby

PostgreSQL

Redis

Nginx

Puma

Node

Yarn/Bun

ImageMagick

Git

Certbot
```

---

# 💎 4. Install Ruby Properly

Recommended:

```
mise
```

or

```
rbenv
```

Avoid:

❌ System Ruby

Reason:

* Easier upgrades
* Version management
* Multiple projects

---

# 🗄️ 5. PostgreSQL Configuration

Production database tips:

✅ Enable connection pooling

```
pool: 20
```

Enable:

* WAL
* Backups
* Replication
* Auto Vacuum

Indexes are critical.

Avoid:

```
SELECT *
```

Instead:

Select only required columns.

---

# ⚡ 6. Redis

Redis powers:

* Sidekiq
* Action Cable
* Cache
* Sessions

Keep Redis separate from PostgreSQL.

---

# 🌐 7. Nginx

Nginx handles:

* SSL
* Static assets
* Compression
* Reverse proxy
* Load balancing

Flow:

```
Internet

↓

Nginx

↓

Puma

↓

Rails
```

---

# 🐆 8. Puma

Puma is the default Rails web server.

Configure:

Workers

Threads

Example:

```
WEB_CONCURRENCY=2

RAILS_MAX_THREADS=5
```

Too many workers can exhaust RAM.

---

# 🔒 9. SSL

Always enable HTTPS.

Use:

```
Let's Encrypt
```

Auto renew certificates.

Enable:

```
HTTP → HTTPS redirect
```

---

# 🔑 10. Secrets Management

Never store secrets in Git.

Use:

Rails Credentials

```
config/credentials.yml.enc
```

or

Environment Variables.

Store:

* API keys
* Secret Key Base
* Database passwords
* AWS keys

---

# 📂 11. Active Storage

Storage options:

Development

```
Local Disk
```

Production

```
Amazon S3
Cloudflare R2
Google Cloud Storage
Azure Blob
```

Never keep uploads only on the application server if you plan to scale horizontally.

---

# 🎨 12. Asset Pipeline

Precompile assets:

```
rails assets:precompile
```

Enable:

```
config.public_file_server.enabled
```

Compress:

* CSS
* JS
* Images

Use fingerprinting.

---

# ⚙️ 13. Background Jobs

Never perform long tasks in controllers.

Use:

* Sidekiq
* Solid Queue
* GoodJob
* Delayed Job

Examples:

* Email
* PDF generation
* Notifications
* Reports
* Image processing

---

# 📅 14. Cron Jobs

Use:

Whenever gem

or

Linux Cron

Tasks:

* Cleanup
* Reports
* Backups
* Notifications

---

# 📊 15. Logging

Log everything important.

Examples:

```
Request

Response

Errors

Background Jobs

Authentication

Payments
```

Use:

```
lograge
```

to simplify logs.

Rotate logs regularly.

---

# 🚨 16. Error Tracking

Use:

✅ Sentry

✅ Bugsnag

✅ Honeybadger

Receive alerts instantly.

---

# 📈 17. Monitoring

Monitor:

CPU

RAM

Disk

Redis

Database

Queue

Response Time

Uptime

Tools:

* Grafana
* Prometheus
* Datadog
* New Relic
* AppSignal

---

# 🚀 18. Performance Optimization

## Cache

Use:

Fragment Cache

Russian Doll Cache

Low-Level Cache

Redis Cache

---

## Eager Loading

Avoid:

```
N+1 Queries
```

Use:

```
includes
```

---

## Database

Always add indexes.

Example:

```
add_index :users, :email
```

---

## Pagination

Never load:

```
100000 rows
```

Use:

```
Pagy

Kaminari
```

---

## Compression

Enable:

```
gzip

brotli
```

---

## CDN

Serve assets via:

Cloudflare

Fastly

AWS CloudFront

---

# 🛡️ 19. Security

Enable:

### Force SSL

```
config.force_ssl = true
```

---

### Secure Headers

```
X-Frame-Options

CSP

HSTS
```

---

### CSRF Protection

Rails enables it by default.

Don't disable it.

---

### Strong Parameters

Never trust user input.

---

### SQL Injection

Use:

```
where(id: params[:id])
```

Avoid:

```
"SELECT * FROM users WHERE id=#{params[:id]}"
```

---

### Brute Force Protection

Use:

Rack Attack

---

### Authentication

Use:

Devise

Auth0

JWT

---

### Authorization

Use:

Pundit

CanCanCan

---

# 📦 20. Backups

Daily:

Database

Weekly:

Files

Monthly:

Full snapshot

Always test restore.

A backup you cannot restore is not a backup.

---

# 📈 21. Scaling

Vertical Scaling

Increase:

* CPU
* RAM

Horizontal Scaling

Increase:

Multiple servers

Add:

Load Balancer

Shared Redis

Shared Database

Shared Storage

---

# 🚀 22. Deployment Strategies

## Blue-Green Deployment

Old Server

↓

Switch Traffic

↓

New Server

Zero downtime.

---

## Rolling Deployment

Update:

Server 1

↓

Server 2

↓

Server 3

---

## Canary

Deploy to:

5%

Monitor

Deploy to:

100%

---

# ⚠️ Common Production Mistakes

## 1.

Running

```
RAILS_ENV=development
```

---

## 2.

Hardcoding secrets

---

## 3.

No SSL

---

## 4.

No backups

---

## 5.

No monitoring

---

## 6.

Running migrations during peak traffic without planning

---

## 7.

No indexes

---

## 8.

Huge ActiveRecord queries

---

## 9.

Memory leaks

---

## 10.

Ignoring logs

---

## 11.

Single server with no redundancy

---

## 12.

No health checks

---

## 13.

No rate limiting

---

## 14.

Uploading files locally

---

## 15.

Blocking requests with long jobs

---

# 🧰 Recommended Production Stack

| Category        | Recommendation                   |
| --------------- | -------------------------------- |
| OS              | Ubuntu LTS                       |
| Ruby            | mise or rbenv                    |
| Database        | PostgreSQL                       |
| Cache           | Redis                            |
| App Server      | Puma                             |
| Reverse Proxy   | Nginx                            |
| Background Jobs | Sidekiq or Solid Queue           |
| Object Storage  | Amazon S3 / Cloudflare R2        |
| CDN             | Cloudflare                       |
| Monitoring      | Grafana + Prometheus / AppSignal |
| Error Tracking  | Sentry                           |
| Deployment      | Kamal, Capistrano, or Hatchbox   |
| SSL             | Let's Encrypt                    |
| Authentication  | Devise                           |
| Authorization   | Pundit                           |
| Pagination      | Pagy                             |
| Logging         | Lograge                          |

---

# 📋 Ultimate Rails Production Checklist

## Infrastructure

* ✅ Ubuntu LTS installed
* ✅ Firewall (UFW) configured
* ✅ SSH keys configured
* ✅ Automatic security updates enabled
* ✅ Time synchronization (NTP) configured

## Application

* ✅ `RAILS_ENV=production`
* ✅ `SECRET_KEY_BASE` configured
* ✅ Rails credentials encrypted
* ✅ Assets precompiled
* ✅ Database migrated
* ✅ Health check endpoint added
* ✅ Environment variables validated

## Database

* ✅ PostgreSQL optimized
* ✅ Proper indexes added
* ✅ Connection pool configured
* ✅ Automated backups scheduled
* ✅ Restore process tested

## Web & App Server

* ✅ Nginx configured
* ✅ Puma workers tuned
* ✅ Gzip/Brotli enabled
* ✅ HTTP → HTTPS redirect
* ✅ Static assets served efficiently

## Security

* ✅ Force SSL enabled
* ✅ Content Security Policy configured
* ✅ HSTS enabled
* ✅ Secure cookies enabled
* ✅ Rack::Attack or equivalent rate limiting
* ✅ Strong Parameters enforced
* ✅ Dependencies scanned for vulnerabilities

## Background Processing

* ✅ Sidekiq/Solid Queue running
* ✅ Redis monitored
* ✅ Cron jobs scheduled
* ✅ Failed jobs retry strategy configured

## Monitoring & Operations

* ✅ Centralized logging
* ✅ Error tracking (Sentry/Honeybadger)
* ✅ Uptime monitoring
* ✅ Metrics dashboard (CPU, RAM, DB, Redis)
* ✅ Alerts configured

## Scaling & Reliability

* ✅ Shared object storage
* ✅ CDN enabled
* ✅ Load balancer (if multiple instances)
* ✅ Zero-downtime deployment strategy
* ✅ Rollback plan documented

---

# 🎯 Final Thoughts

A successful Rails deployment isn't just about getting an application online—it's about building a platform that is **secure, observable, resilient, and easy to maintain**. By combining proven Rails practices with robust infrastructure, continuous monitoring, disciplined deployments, and regular backups, you can confidently serve users at any scale.

Remember this progression:

> **Build → Secure → Optimize → Monitor → Scale → Automate**

Following this lifecycle will help you avoid common production pitfalls and ensure your Rails application remains fast, reliable, and ready for growth.
