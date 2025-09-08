---
layout: home
title: "Error Handling & Logging Best Practices in Rails"
date: 2025-09-08
categories: "Ruby On Rails"
tags: [Ruby On Rails, Programming, Best Practice, Tools, Error Handling, Coding]
image: 'https://github.com/user-attachments/assets/a1a41f45-7c7c-4c1a-a50e-11ef19e5e656'
---

# Silent Failures Are Your App’s Kryptonite — Error Handling & Logging Best Practices in Rails ⚠️🔍

Want fewer 2 a.m. panic calls and faster debugging? This guide gives you practical, production-ready patterns for **error handling** and **logging** in Rails — with examples, config snippets, and the best live tools to use. 🚀

<img width="1906" height="1272" alt="7bdde135-3351-44d8-ad36-717120f46552_slide-1" src="https://github.com/user-attachments/assets/a1a41f45-7c7c-4c1a-a50e-11ef19e5e656" />

---

## Why this matters (short)

* Errors that aren’t **caught**, **reported**, and **contextualized** cost time and users.
* Poor logs = slow fixes. Good logs + alerts = fast remediation and happier users. 😊

---

## Rails built-ins you should know (and use)

### 1) Parameter filtering — **never** log PII

Add sensitive keys to `config.filter_parameters` so Rails masks them in logs and when `inspect` is called on AR objects.

```ruby
# config/initializers/filter_parameter_logging.rb
Rails.application.config.filter_parameters += [
  :password, :credit_card_number, :ssn, :token, :authorization
]
```

Rails’ config provides this for you and is the first line of defense for not leaking secrets. ([Ruby on Rails Guides][1])

---

### 2) Rails error reporter

Rails includes an **error reporter** that wraps request/job execution and lets error-reporting libraries subscribe to unhandled errors. That means Sentry/Honeybadger/etc. can hook in cleanly — and you can also create custom subscribers. ([Ruby on Rails Guides][2])

Example (conceptual):

```ruby
Rails.error.handle(MyCustomError) do
  # code that might raise
end
```

---

## Error-handling best practices (code + patterns)

### Prefer **fail-fast** and **explicit** errors

Create domain-specific exceptions so handlers can react properly:

```ruby
# app/errors/payment_error.rb
class PaymentError < StandardError; end
```

Raise them where appropriate (service objects, not controllers):

```ruby
# app/services/payment_processor.rb
class PaymentProcessor
  def call
    charge = gateway.charge(...)
    raise PaymentError, "card declined" unless charge.success?
    charge
  end
end
```

### Use `rescue_from` in controllers for uniform responses

Centralize HTTP error mapping:

```ruby
# app/controllers/application_controller.rb
class ApplicationController < ActionController::Base
  rescue_from PaymentError, with: :payment_failure
  rescue_from ActiveRecord::RecordNotFound, with: :not_found
  rescue_from StandardError, with: :internal_error

  private

  def payment_failure(err)
    Rails.logger.warn("Payment failed: #{err.message}")
    Sentry.capture_exception(err) if defined?(Sentry)
    render json: { error: 'Payment failed' }, status: :payment_required
  end

  # ...
end
```

### Don’t swallow exceptions silently

Always log and (usually) report — silent failures are the worst. Use structured logging and an error tracker (below).

---

## Background jobs: Sidekiq + retries

Sidekiq automatically retries failed jobs with an exponential backoff and moves exhausted jobs to the “dead” set — but you should still handle expected errors intentionally.

* Use idempotent jobs.
* Use `sidekiq_retries_exhausted` or middleware to notify after retries are done.
* If you have transient external failures, prefer retries; if logic error, fail fast and inspect.

Sidekiq’s retry behavior and hooks are documented in the official docs. ([rubydoc.info][3])

Example hook:

```ruby
class MyWorker
  include Sidekiq::Worker
  sidekiq_options retry: 5

  sidekiq_retries_exhausted do |msg, ex|
    Rails.logger.error("Job exhausted: #{msg['class']} #{msg['args']} - #{ex.message}")
    Sentry.capture_message("Job exhausted: #{msg['class']}", extra: { job: msg })
  end

  def perform(*args)
    # ...
  end
end
```

---

## Logging best practices — make logs actionable

### Use levels correctly

* `debug`: in-depth dev info
* `info`: normal operations (requests, major lifecycle events)
* `warn`: recoverable or unusual events
* `error`: exceptions you caught and are reporting
* `fatal`: process-level crashes

In production prefer `info` or `warn` to avoid noise; `debug` is for local troubleshooting.

### Add context: request\_id, user\_id, etc.

Correlate logs across services by including a request id and user identifiers (non-PII). Use Rails’ `config.log_tags` and `TaggedLogging`.

```ruby
# config/environments/production.rb
config.log_tags = [:request_id, lambda { |req| "user:#{req.session[:current_user_id]}" }]
```

### Structured logs (JSON)

Structured logs are machine-parseable and 10× easier to search/aggregate. Two common approaches in Rails:

1. **Lograge**: convert Rails request logs into single-line, structured logs (JSON or key=value). Great for shipping to log systems. ([GitHub][4])

```ruby
# Gemfile
gem 'lograge'

# config/environments/production.rb
config.lograge.enabled = true
config.lograge.formatter = Lograge::Formatters::Json.new
config.lograge.custom_options = lambda do |event|
  {
    time: event.time.utc.iso8601,
    params: event.payload[:params].except('controller','action'),
    host: event.payload[:host]
  }
end
```

2. Or use a JSON formatter for the standard logger:

```ruby
# config/initializers/json_logger.rb
class JsonLogFormatter < Logger::Formatter
  def call(severity, time, progname, msg)
    {
      level: severity,
      time: time.utc.iso8601,
      message: msg.to_s
    }.to_json + "\n"
  end
end

Rails.logger = ActiveSupport::TaggedLogging.new(Logger.new(STDOUT))
Rails.logger.formatter = JsonLogFormatter.new
```

Lograge is a widely used option for turning Rails’ verbose request logs into compact single-line logs suitable for aggregators. ([GitHub][4])

### Filter what you send

Double-check `filter_parameters` covers any custom keys and be careful with objects that include user data — don’t log full `params` in raw form.

---

## Centralized logging & observability (where to send logs & traces)

### SaaS / APMS (recommended for most teams)

* **Sentry** — excellent Rails integration for error monitoring, breadcrumbs, tracing and alerting; easy install via `sentry-rails`. Great for grouping errors, seeing stack traces and traces across requests. ([Sentry Docs][5])

Quick Sentry setup:

```ruby
# Gemfile
gem 'sentry-rails'

# config/initializers/sentry.rb
Sentry.init do |config|
  config.dsn = ENV['SENTRY_DSN']
  config.breadcrumbs_logger = [:active_support_logger, :http_logger]
  config.traces_sample_rate = 0.05 # sample traces (adjust carefully)
end
```

* **Datadog** — logs, traces & metrics in one place (great if you want APM + logs). Datadog documents how to collect Rails logs and traces and how to tail logs or use the agent. ([Datadog Monitoring][6], [Datadog][7])

### Open-source / self-hosted

* **ELK / OpenSearch (Elasticsearch + Logstash/Filebeat + Kibana)** — flexible and powerful but operationally heavy. Good if you want full ownership and custom pipelines. ([Elastic][8])

### Quick guidance on choosing:

* Need fast setup & great UI: **Sentry** (errors) + **Datadog/LogDNA/Mezmo** (logs) — low ops overhead. ([Sentry Docs][5], [mezmo.com][9])
* Need full control & lower recurring cost: **ELK + beats** (more ops overhead). ([Elastic][8])

---

## Example: end-to-end pattern (service → controller → Sentry + Log)

```ruby
# app/services/order_creator.rb
class OrderCreator
  def initialize(user, params)
    @user = user
    @params = params
  end

  def call
    validate!
    create_order!
  rescue ExternalPaymentGateway::Timeout => e
    # expected transient error — let job retry or handle gracefully
    raise
  rescue => e
    Rails.logger.error("OrderCreator failed: #{e.class} #{e.message}", user_id: @user.id)
    Sentry.capture_exception(e, extra: { user_id: @user.id, params: @params })
    raise
  end
end
```

```ruby
# app/controllers/orders_controller.rb
def create
  OrderCreator.new(current_user, order_params).call
  render json: { ok: true }
rescue PaymentError => e
  render json: { error: e.message }, status: :payment_required
end
```

This combination: **log locally with context**, **report to Sentry**, and **return a friendly HTTP error** is the pattern to aim for.

---

## Practical ops tips & gotchas

* **Log to STDOUT** in containerized environments (`RAILS_LOG_TO_STDOUT=true`) so your platform (Kubernetes/Heroku) can handle collection. (Common gotcha: not sending logs to stdout prevents aggregators from seeing them.) ([Stack Overflow][10])
* **Rotate logs / set retention** to avoid bill shocks.
* **Sampling**: for high-traffic apps, sample traces (APM) to control cost — but always send all error events.
* **Alert tuning**: start with high-priority alerts (new error spike, high error rate, job dead queue) and iteratively refine to reduce noise.
* **Add breadcrumbs** in Sentry to capture the lead-up to an error (DB queries, cache calls, external HTTP) — invaluable for reproducing issues. ([Sentry Docs][5])

---

## Recommended toolset (quick)

* **Error monitoring / grouping**: **Sentry** (excellent Rails integration + tracing). ([Sentry Docs][5])
* **Structured request logs**: **Lograge** (compact single-line request logs). ([GitHub][4])
* **Full-stack observability**: **Datadog** (logs + traces + metrics) or ELK for self-hosted. ([Datadog Monitoring][6], [Elastic][8])
* **Background jobs**: **Sidekiq** (use its retry hooks + monitoring UI). ([rubydoc.info][3])

---

## TL;DR (cheat sheet) ✅

* Add sensitive keys to `config.filter_parameters`. ([Ruby on Rails Guides][1])
* Use `rescue_from` in controllers and explicit rescue + report in services.
* Use **Lograge** or a JSON logger for structured request logs. ([GitHub][4])
* Ship errors to **Sentry** (or Rollbar/Honeybadger) and logs/traces to **Datadog** or ELK. ([Sentry Docs][5], [Datadog Monitoring][6])
* For background jobs, rely on Sidekiq’s retry semantics and hook `sidekiq_retries_exhausted` to notify on exhaustion. ([rubydoc.info][3])
