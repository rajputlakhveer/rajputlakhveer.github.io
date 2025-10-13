---
layout: home
title: "Ruby on Rails Coding Mistakes Every Developer Should Avoid"
date: 2025-10-13
categories: "Ruby On Rails"
tags: [Ruby On Rails, Mistakes, Coding, Developer, Programming, MR's]
image: 'https://github.com/user-attachments/assets/f68572ec-8533-4560-a864-a5c9b7f96d0d'
---

# **🚫 Ruby on Rails Coding Mistakes Every Developer Should Avoid 🚦**

Ruby on Rails is an amazing framework — it lets you build powerful web applications quickly and elegantly. But even seasoned Rails developers often fall into common traps that lead to **performance bottlenecks, maintenance nightmares, and bugs** that hide in plain sight 😬.

In this blog, we’ll explore **the most common Rails coding mistakes developers make**, along with **real examples, fixes, and checkpoints** you can use in your **Merge Requests (MRs)** ✅.

<img width="1000" height="420" alt="0_jl94LjFdTv19FBoc (1)" src="https://github.com/user-attachments/assets/f68572ec-8533-4560-a864-a5c9b7f96d0d" />

---

## 🧩 1. Ignoring N+1 Query Problems

### ❌ The Mistake:

Developers often loop through ActiveRecord relations without realizing how many database queries are triggered.

```ruby
# BAD
@users = User.all
@users.each do |user|
  puts user.posts.count
end
```

This runs **1 query for users + N queries for posts**, leading to performance disaster 🚨.

### ✅ The Fix:

Use `includes` or `eager_load` to preload associations efficiently.

```ruby
# GOOD
@users = User.includes(:posts)
@users.each do |user|
  puts user.posts.count
end
```

**Checkpoint for MR ✅:**

* [ ] Did you check for possible N+1 queries using `bullet` gem or logs?
* [ ] Are you preloading related data where needed?

---

## 🧠 2. Fat Models & Skinny Controllers Gone Wrong

### ❌ The Mistake:

Developers often stuff **too much business logic into models**, turning them into “God Objects”.

```ruby
# BAD
class User < ApplicationRecord
  def self.generate_monthly_report
    # Complex report generation logic here
  end
end
```

### ✅ The Fix:

Move such logic to **Service Objects** or **Interactors**.

```ruby
# GOOD
class MonthlyReportGenerator
  def self.call
    # clean logic here
  end
end
```

**Checkpoint for MR ✅:**

* [ ] Are models and controllers under 150 lines?
* [ ] Is there business logic that should live in a service class?

---

## 🧵 3. Missing Background Jobs for Heavy Tasks

### ❌ The Mistake:

Running heavy or long-running tasks (like sending emails or processing files) in controllers.

```ruby
# BAD
def create
  UserMailer.welcome_email(@user).deliver_now
end
```

### ✅ The Fix:

Offload such tasks using **ActiveJob** or **Sidekiq**.

```ruby
# GOOD
def create
  UserMailer.welcome_email(@user).deliver_later
end
```

**Checkpoint for MR ✅:**

* [ ] Are time-consuming operations offloaded to background jobs?
* [ ] Did you ensure retries and job uniqueness where applicable?

---

## 🧱 4. Overusing Callbacks in Models

### ❌ The Mistake:

Callbacks like `after_save`, `before_create` look convenient, but they often lead to **hidden side effects** and **unpredictable behavior**.

```ruby
# BAD
after_save :send_welcome_email
```

### ✅ The Fix:

Use **observers** or **service classes** for side effects.
Keep models focused only on persistence logic.

```ruby
# GOOD
UserCreator.new(user_params).call
```

**Checkpoint for MR ✅:**

* [ ] Are callbacks minimal and clearly justified?
* [ ] Can side effects be extracted into separate services?

---

## 🪄 5. Skipping Validations & Error Handling

### ❌ The Mistake:

Relying on frontend or assumptions instead of ActiveRecord validations.

```ruby
# BAD
user = User.create(name: nil) # silently fails or causes issues later
```

### ✅ The Fix:

Always validate crucial fields and handle exceptions.

```ruby
# GOOD
class User < ApplicationRecord
  validates :name, presence: true
end
```

**Checkpoint for MR ✅:**

* [ ] Are all critical fields validated?
* [ ] Is there rescue logic for possible database or API errors?

---

## 🕸️ 6. Not Using Scopes & Query Objects

### ❌ The Mistake:

Hardcoding query logic throughout your controllers or views.

```ruby
# BAD
@active_users = User.where(active: true).order(created_at: :desc)
```

### ✅ The Fix:

Use **named scopes** for cleaner, reusable code.

```ruby
# GOOD
class User < ApplicationRecord
  scope :active, -> { where(active: true).order(created_at: :desc) }
end
```

**Checkpoint for MR ✅:**

* [ ] Are repeated queries refactored into scopes or Query Objects?
* [ ] Is the query logic reusable and DRY?

---

## 💬 7. Not Leveraging Rails Conventions

### ❌ The Mistake:

Over-customizing naming, routes, or folder structures.

```ruby
# BAD
get '/show_user_profile', to: 'profiles#show'
```

### ✅ The Fix:

Stick to Rails conventions unless you have a strong reason to deviate.

```ruby
# GOOD
resources :profiles, only: [:show]
```

**Checkpoint for MR ✅:**

* [ ] Does the code follow Rails naming conventions?
* [ ] Are routes RESTful and minimal?

---

## 💾 8. Not Indexing Frequently Queried Columns

### ❌ The Mistake:

Forgetting to add indexes leads to **slow queries** and poor scalability.

```ruby
# BAD
add_index :users, :email, unique: true unless needed
```

### ✅ The Fix:

Index columns used in `where`, `joins`, or `order` clauses.

```ruby
# GOOD
add_index :users, :email
add_index :posts, :user_id
```

**Checkpoint for MR ✅:**

* [ ] Are necessary database indexes present?
* [ ] Have you analyzed query performance (EXPLAIN plan)?

---

## 🔒 9. Ignoring Security Best Practices

### ❌ The Mistake:

Interpolating parameters directly into queries or views.

```ruby
# BAD
User.where("email = '#{params[:email]}'")
```

### ✅ The Fix:

Use Rails’ built-in protections.

```ruby
# GOOD
User.where(email: params[:email])
```

And always use `sanitize`, `html_safe`, and `strong_parameters` carefully. 🛡️

**Checkpoint for MR ✅:**

* [ ] Are all parameters sanitized and validated?
* [ ] No direct SQL or unsafe HTML interpolation?

---

## ⚙️ 10. Missing Tests & Linters

### ❌ The Mistake:

Relying only on manual testing or skipping code quality tools.

### ✅ The Fix:

Use:

* **RSpec / Minitest** for testing
* **Rubocop** for linting
* **Brakeman** for security scans
* **SimpleCov** for test coverage

**Checkpoint for MR ✅:**

* [ ] Did you run Rubocop and fix warnings?
* [ ] Is the test coverage above the project minimum?

---

## 🧭 Bonus: Your Merge Request (MR) Checklist 🧾

Here’s a summarized MR checklist to keep your Rails code clean & professional:
✅ Code follows Rails conventions
✅ No N+1 queries (check with Bullet)
✅ Proper validations added
✅ Background jobs for heavy tasks
✅ Business logic in services, not models
✅ Security checks passed
✅ Tests & linters run successfully
✅ Code reviewed for readability & reusability

---

## 💡 Final Thought

Writing Rails code is not just about making things *work* — it’s about making them **maintainable, scalable, and secure**. 🚀
The real craftsmanship lies in writing code that another developer can read, understand, and extend effortlessly.

> “Clean code always looks like it was written by someone who cares.” – Robert C. Martin ✨

So, next time you raise that MR, **pause for a quick checklist review** — your teammates (and future self) will thank you! 🙌
