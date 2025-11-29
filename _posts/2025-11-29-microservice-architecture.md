---
layout: home
title: "Microservice Architecture"
date: 2025-11-29
categories: "Software Architecture"
tags: [Microservices, Software Architecture, Software Engineer, Software Development, DevOps, Programming]
image: 'https://github.com/user-attachments/assets/50b521e4-a255-4e52-abc8-4c7aa7b4a12e'
---

# 🚀 Microservice Architecture — Build Scalable Apps Step-by-Step (with examples, pitfalls & checklist)

Microservices turn a huge, monolithic app into a team-friendly, independently deployable system of small services. This blog explains the **terminology**, core **concepts**, a **practical step-by-step guide** (with a working example using containers), **common mistakes to avoid**, and a handy **checklist** so you can ship confidently. Let’s go! 💥

<img width="1536" height="1024" alt="ChatGPT Image Nov 29, 2025, 11_53_42 PM" src="https://github.com/user-attachments/assets/50b521e4-a255-4e52-abc8-4c7aa7b4a12e" />

---

# ✅ Why microservices?

* Scale parts of your system independently (scale the payment service, not the whole app). 📈
* Teams can own services, choose appropriate tech stacks, and deploy independently. 👥
* Fault isolation — a failure in one service is less likely to take everything down. 🛡️

But microservices add complexity — networking, deployment, observability and distributed data. Use them when benefits outweigh the operational cost.

---

# 🧰 Key Terminology (quick cheat sheet)

* **Service** — an independently deployable app that performs one business capability (e.g., `user-service`, `order-service`).
* **API / HTTP contract** — how services talk (REST, gRPC, GraphQL). 🔌
* **API Gateway** — single entry point that routes, authenticates, rate-limits requests.
* **Service Discovery** — how services find each other (DNS, Consul, Kubernetes Service). 🔍
* **Load Balancer** — distributes traffic across instances. ⚖️
* **Circuit Breaker** — prevents cascading failures (Hystrix, resilience patterns). 🛑
* **Saga** — pattern to manage distributed transactions (compensating actions). 🔁
* **Event Bus / Message Broker** — async comms (RabbitMQ, Kafka). 🔔
* **Observability** — logs, metrics, traces for debugging (Prometheus, Grafana, Jaeger). 📊
* **CI/CD** — continuous integration and deployments for services (GitHub Actions, Jenkins, GitLab CI). 🔁
* **Container & Orchestration** — Docker, and Kubernetes for running services at scale. 🐳➡️☸️

---

# 🧩 Core Concepts & Design Principles

1. **Bounded Context** — each service owns data & logic for a specific business area.
2. **Single Responsibility** — keep services small and focused.
3. **Decentralized Data** — each service has its own datastore to avoid schema coupling. 🗄️
4. **API Contracts & Versioning** — maintain backwards compatibility; version your APIs. 🔁
5. **Prefer async for long-running tasks** — use events/queues to decouple. ⏳
6. **Idempotency** — make endpoints safe to retry. ♻️
7. **Observability by default** — instrument services for logs/metrics/traces. 🔍
8. **Infrastructure as Code** — automate deployments, not manual steps. 🧱

---

# 🛠️ Step-by-Step Guide — Hands-on Example (Rails API services + Docker Compose)

We'll build a minimal example with:

* `user-service` (Rails API)
* `product-service` (Rails API)
* `api-gateway` (NGINX or a tiny Express / Rails gateway)
* `postgres` DBs for services
* `docker-compose` to run locally

> Note: you can substitute Rails with Node/Python/Go depending on team preference.

---

## 1) Design & define boundaries

Example responsibilities:

* `user-service`: user signup, profile, auth (or just user data if auth is centralised)
* `product-service`: product catalog, search
* Each service owns its DB → `users_db`, `products_db`.

---

## 2) Create two simple Rails API apps (local development)

Commands (run for each service folder):

```bash
# create user-service (Rails API only)
rails new user-service --api -d postgresql
cd user-service
rails g scaffold User name:string email:string
# configure database.yml to use ENV vars for DB_HOST/DB_USER/DB_PASS
```

Same for `product-service`:

```bash
rails new product-service --api -d postgresql
cd product-service
rails g scaffold Product name:string price:decimal
```

Important: In production, you’d likely separate auth into its own service or use a third-party identity provider (Auth0, Keycloak).

---

## 3) Dockerize each service

**Example `Dockerfile` for a Rails API (user-service):**

```dockerfile
FROM ruby:3.2-slim

# system deps
RUN apt-get update -qq && apt-get install -y nodejs postgresql-client build-essential

WORKDIR /app
COPY Gemfile* ./
RUN bundle install --jobs 4

COPY . .
ENV RAILS_ENV=production
# Precompile assets if any (API apps generally don't)
CMD ["bundle", "exec", "puma", "-C", "config/puma.rb"]
```

Make sure `database.yml` uses ENV variables (`ENV['DB_HOST']`, `ENV['DB_USER']`, `ENV['DB_PASS']`, `ENV['DB_NAME']`).

---

## 4) docker-compose for local testing

`docker-compose.yml` (simplified):

```yaml
version: "3.8"
services:
  user-db:
    image: postgres:15
    environment:
      POSTGRES_DB: users_db
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - user-db-data:/var/lib/postgresql/data

  product-db:
    image: postgres:15
    environment:
      POSTGRES_DB: products_db
      POSTGRES_USER: product
      POSTGRES_PASSWORD: password
    volumes:
      - product-db-data:/var/lib/postgresql/data

  user-service:
    build: ./user-service
    depends_on:
      - user-db
    environment:
      DB_HOST: user-db
      DB_NAME: users_db
      DB_USER: user
      DB_PASS: password
      RAILS_ENV: development
    ports:
      - "3001:3000"

  product-service:
    build: ./product-service
    depends_on:
      - product-db
    environment:
      DB_HOST: product-db
      DB_NAME: products_db
      DB_USER: product
      DB_PASS: password
      RAILS_ENV: development
    ports:
      - "3002:3000"

  api-gateway:
    image: nginx:stable
    volumes:
      - ./gateway/nginx.conf:/etc/nginx/nginx.conf:ro
    ports:
      - "8080:80"
    depends_on:
      - user-service
      - product-service

volumes:
  user-db-data:
  product-db-data:
```

**Example `gateway/nginx.conf`** routes `/users` to user-service and `/products` to product-service by proxy_pass.

---

## 5) Service communication patterns

* **Synchronous (HTTP)** — gateway -> user-service -> product-service if needed. Use REST/gRPC. Simple but couples latency/failures.
* **Asynchronous (events)** — product-service emits `product.created` event to a message broker; interested services consume. Good for decoupling.

For local dev, start with HTTP and add a message broker (RabbitMQ/Kafka) as you evolve.

---

## 6) Database & migrations with Docker Compose

Run migrations in each service container:

```bash
docker-compose build
docker-compose up -d
# run migrations inside the rails container (example)
docker-compose exec user-service rails db:create db:migrate
docker-compose exec product-service rails db:create db:migrate
```

(You can script this into your compose / entrypoint for automation.)

---

## 7) Health checks & readiness probes

Expose a `/health` endpoint that returns 200 when the service is healthy (DB connected, essential subsystems OK). Later map these to Kubernetes readiness/liveness probes.

---

## 8) Logging, metrics & tracing (observability)

* Expose structured logs (JSON) → centralized logging (ELK/EFK, Loki). 📝
* Export metrics to Prometheus (app exposes `/metrics`). 📈
* Add distributed tracing (OpenTelemetry → Jaeger) to follow requests across services. 🔎

Instrument early — debugging distributed systems without traces is painful.

---

## 9) CI/CD pipeline

* Build & test service image per PR.
* Run contract tests and integration tests.
* Push images to registry (Docker Hub, ECR).
* Deploy via Helm/Kubernetes manifests or managed platform. 🚀

---

## 10) Deploy to Kubernetes (optional basic manifest example)

A very simple `Deployment` + `Service` for `user-service`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: user-service
spec:
  replicas: 2
  selector:
    matchLabels: { app: user-service }
  template:
    metadata:
      labels: { app: user-service }
    spec:
      containers:
      - name: user-service
        image: yourrepo/user-service:latest
        envFrom:
        - secretRef: { name: user-service-secrets }
        ports:
        - containerPort: 3000
        readinessProbe:
          httpGet:
            path: /health
            port: 3000
          initialDelaySeconds: 10
---
apiVersion: v1
kind: Service
metadata:
  name: user-service
spec:
  selector: { app: user-service }
  ports:
    - port: 80
      targetPort: 3000
```

Use `HorizontalPodAutoscaler` for scaling based on CPU/memory or custom metrics.

---

# ⚠️ Common Mistakes to Avoid

1. **Starting with microservices too early** — pick monolith → modularize → split when needed. ✂️
2. **Sharing a central database across services** — causes tight coupling. ❌
3. **No contracts or API versioning** — breaks clients unexpectedly. 🔨
4. **Poor observability** — no logs/traces/metrics = debugging nightmare. 🔥
5. **Insecure service communication** — no mTLS, no auth between services. Lock it down. 🔐
6. **Tight coupling between services** — avoid sync chains that create single points of latency/failure. 🧩
7. **Ignoring idempotency & retries** — leads to duplicate side effects. 🔁
8. **Ad-hoc deployments** — manual deploys cause drift. Use IaC and CI/CD. ⚙️

---

# ✅ Microservices Checklist (copyable)

* [ ] Bounded contexts defined & responsibilities clear
* [ ] Each service owns its own datastore (no cross-service DB writes)
* [ ] API contract documented (OpenAPI / Swagger) and versioning plan exists
* [ ] Health & readiness endpoints implemented
* [ ] Centralized logging in place (structured logs)
* [ ] Metrics exposed (Prometheus-compatible)
* [ ] Distributed tracing enabled (OpenTelemetry/Jaeger)
* [ ] CI/CD pipeline builds and tests images automatically
* [ ] Automated DB migrations strategy (zero downtime)
* [ ] Circuit breaker / timeout rules applied for remote calls
* [ ] Backoff, retry, and idempotency implemented for retryable operations
* [ ] Authentication & authorization between services (service accounts, mTLS)
* [ ] Secrets management in place (Vault, K8s Secrets)
* [ ] Disaster recovery plan and backups for critical data
* [ ] Resource limits and autoscaling policies defined
* [ ] Load balancer and ingress configured for traffic routing
* [ ] Security scanning for images and dependencies enabled

---

# 🧪 Example: Minimal `/users` endpoint (Rails controller snippet)

`app/controllers/users_controller.rb`:

```ruby
class UsersController < ApplicationController
  def index
    users = User.all
    render json: users
  end

  def create
    user = User.new(user_params)
    if user.save
      # optionally publish event to message broker asynchronously
      render json: user, status: :created
    else
      render json: { errors: user.errors.full_messages }, status: :unprocessable_entity
    end
  end

  private

  def user_params
    params.require(:user).permit(:name, :email)
  end
end
```

Make this API idempotent for operations that can be retried (e.g., by checking request idempotency keys).

---

# 📚 Tools & Tech Suggestions

* Containers: **Docker**
* Local orchestration: **Docker Compose**
* Production orchestration: **Kubernetes (EKS/GKE/AKS)**
* Message brokers: **RabbitMQ**, **Kafka** (for high throughput)
* Observability: **Prometheus**, **Grafana**, **Jaeger**, **ELK/Loki**
* API contracts: **OpenAPI/Swagger**
* CI/CD: **GitHub Actions**, **GitLab CI**, **Jenkins**
* Secrets: **HashiCorp Vault**, cloud provider secrets manager

---

# Final tips (short & spicy) 🌶️

* Start small: split a single module first (e.g., move the catalog out of the monolith).
* Automate everything: builds, tests, deployments, rollbacks.
* Make debugging easy: structured logs + traces + correlation IDs.
* Invest in good developer DX — if developing and running services is painful, teams will avoid best practices.
