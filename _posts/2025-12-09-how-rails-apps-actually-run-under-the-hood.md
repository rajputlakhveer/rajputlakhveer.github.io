---
layout: home
title: "How Rails Apps Actually Run Under the Hood"
date: 2025-12-09
categories: "Ruby On Rails"
tags: [Ruby On Rails, Programming, Software Development, Coding, Working, Rails, Request, Response]
image: 'https://github.com/user-attachments/assets/f956faf6-0de2-487b-a2c2-ab9cb0c6ba62'
---

# 🚀 **How Rails Apps *Actually* Run Under the Hood — A Deep Dive for Developers**

Ever wondered what truly happens when you hit **[http://localhost:3000](http://localhost:3000)** in your browser? 🤔
Rails *feels* magical — but in reality, it’s a finely engineered machine powered by dozens of moving parts.

Today, let’s open that machine, see what’s inside, and understand **how a Rails request travels**, what **hidden classes** do the heavy lifting, and how the whole framework works *under the hood*.

<img width="1024" height="1536" alt="ChatGPT Image Dec 9, 2025, 09_47_00 PM" src="https://github.com/user-attachments/assets/f956faf6-0de2-487b-a2c2-ab9cb0c6ba62" />

Let’s begin your journey from **browser → server → your Rails code → back to browser**. ⚙️🔥

---

# 🔥 **🚂 The Journey of a Rails Request (Step-by-Step Story)**

To make it easy, we’ll use an example app:

👉 URL requested:

```
GET /articles/5
```

👉 Expected output:
A page showing article with ID = 5.

Let’s start!

---

# 🧱 **1. The Server Layer — Puma Starts the Engine**

When you run:

```
rails server
```

Rails starts **Puma**, the default multi-threaded web server.

### 🎯 What Puma does:

* Listens for HTTP requests
* Manages threads/pools
* Passes requests to the **Rack** interface

🧩 **Hidden class in action:**

### `Puma::Server`

Handles incoming TCP connections and dispatches them to worker threads.

---

# 🔗 **2. Rack — The Gateway Between Web Server & Rails**

Rails is a **Rack app**, meaning every request passes through the Rack pipeline.

### 🌐 Rack responsibilities:

* Normalizes HTTP request
* Converts request into a Ruby hash
* Calls the Rails app via:

  ```ruby
  call(env)
  ```

🧩 **Hidden classes:**

* `Rack::Handler::Puma`
* `Rack::Lint` (ensures request validity)
* `Rack::Runtime` (adds request time headers)

---

# 🧠 **3. Rails Boot Process — The Framework Wakes Up**

Before your app does anything, Rails boots its full environment.

### 🎯 Key steps:

1. Loads `config/application.rb`
2. Loads Railties (ActiveRecord, ActionPack, ActiveJob…)
3. Runs initializers inside `config/initializers/**/*`
4. Loads routes
5. Prepares the middleware stack

🧩 **Hidden classes:**

* `Rails::Application::Finisher`
  Handles final boot steps like setting up I18n and eager loading.
* `Rails::Initializable`
  Runs initializers across all Railties.

---

# 🧱 **4. Rails Middleware — The Security Shield 🛡️**

Before your controller sees anything, the request passes through **many middleware layers**.

### Example middleware:

* `Rack::Sendfile`
* `ActionDispatch::Static`
* `ActionDispatch::HostAuthorization`
* `ActionDispatch::Cookies`
* `Rack::Session::Cookie`
* `ActionDispatch::RequestId`
* `ActionDispatch::RemoteIp`

🧩 Fun fact: Over **25 middleware** execute before controller logic starts.

🧩 **Unknown but powerful classes:**

* `ActionDispatch::Executor`
* `ActionDispatch::ContentSecurityPolicy::Middleware`
* `ActionDispatch::ServerTiming`

Each plays a hidden but crucial role.

---

# 🗺️ **5. Router — Finding the Right Controller**

The router uses the request path (`/articles/5`) and HTTP method (`GET`) to find a matching route.

### Internally:

Rails uses a **Radix tree**-like matcher for super fast routing.

### Example route:

```ruby
GET /articles/:id → ArticlesController#show
```

🧩 **Hidden class that powers this:**

* `ActionDispatch::Journey::Router`
* `ActionDispatch::Journey::Path::Pattern`

These convert your route definitions into highly optimized matching structures.

---

# 🎬 **6. Controller Processing — Where Your Code Finally Runs**

Rails now creates a controller object:

```
ArticlesController
```

Then runs:

* Filters
* Callbacks
* Params parsing
* Strong Params
* Rendering or Redirecting

### Controller lifecycle:

1. `before_action` → Run checks
2. Action method (e.g., `show`)
3. `render` template
4. `after_action`

🧩 **Unknown classes here:**

* `ActionController::Metal`
  The super bare-bones controller Rails inherits everything from.
* `ActionController::Instrumentation`
  Logs all events for performance debugging.
* `ActionController::Parameters`
  Manages strong params.

---

# 🧩 **7. ActiveRecord — The Heart of Database Access 🧡**

Inside `ArticlesController#show`:

```ruby
@article = Article.find(5)
```

Here’s what actually happens:

### Steps behind the scenes:

1. Query Builder builds SQL
2. Connection pool assigns a DB connection
3. SQL executes via adapter (e.g., PostgreSQL)
4. Result is converted to Ruby objects

🧩 **Important internal classes:**

* `ActiveRecord::Relation`
* `ActiveRecord::ConnectionAdapters::ConnectionPool`
* `ActiveRecord::AttributeSet`
* `ActiveModel::Type::Value` (type casting)

Rails does **a LOT** before returning a simple model object.

---

# 🎨 **8. View Rendering — ERB/Haml/Builders Convert to HTML**

When Rails runs:

```
render "articles/show"
```

### Hidden flow:

1. View lookup path resolver
2. Template reading
3. ERB compiling into Ruby methods
4. Layout handling
5. Response body formatting

🧩 **Key internal classes:**

* `ActionView::Template::Handlers::ERB`
* `ActionView::LookupContext`
* `ActionView::Renderer`

---

# 📦 **9. Response Sent Back — Rack Again**

Rails returns a response triplet:

```
[status_code, headers, response_body]
```

Rack sends it back to Puma → Browser.

Browser displays your HTML.
**Request lifecycle ends.** 🎉

---

# ⚙️ **🧩 Behind-the-Scenes Classes You Rarely Hear About**

Here are some powerful but less-known Rails classes:

### 🔹 `ActiveSupport::Notifications`

Used for internal event tracking & performance monitoring.

### 🔹 `ActiveSupport::Dependencies::ZeitwerkIntegration`

Connected to app auto-loading & constant management.

### 🔹 `ActionDispatch::Request`

Parses cookies, JSON, headers — way more powerful than you think.

### 🔹 `ActionView::PathSet`

Handles template searching logic.

### 🔹 `ActiveModel::Errors`

Manages validation error collection and formatting.

---

# ⚡ **Putting It All Together (Diagrammatic Summary)**

### Request Flow

`Browser → Puma → Rack Middleware → Router → Controller → Model → View → Rack → Browser`

### Example for `/articles/5`

* Puma accepts request
* Rack formats env
* Middleware security checks
* Router finds route
* Controller loads article
* ActiveRecord fetches DB
* View renders
* Response returns

---

# 🌟 **Final Thoughts**

Rails is not magic — it’s **beautiful engineering**.
From middleware to autoloading, from routing engines to view renderers — hundreds of classes work silently to deliver that single page you see.

Understanding these internals makes you a **10× better Ruby on Rails developer** 💥.
