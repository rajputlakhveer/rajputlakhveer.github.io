---
layout: home
title: "Ruby On Rails Code Optimization Shortcuts"
date: 2025-07-09
categories: "Ruby On Rails"
tags: [Ruby on Rails, Code Optimization, Shortcuts, Programming, Coding]
image: 'https://github.com/user-attachments/assets/eb4f3e7e-2d0d-4473-87ab-1d63a211268f'
---

# ⚙️ Level Up Your Ruby on Rails Game: 10+ Code Optimization Shortcuts Only Pros Use 🚀💡

Ruby on Rails is **elegant by default**, but the real magic happens when you know its *hidden shortcuts and smart optimizations*. If you're still writing verbose, repetitive code — you're doing it wrong. 😅

Here’s your cheat sheet to **code smarter, cleaner, and faster** using these **pro-level Rails tricks** — optimized for performance, readability, and development speed. 💻⚡

![0__Jp2pqQ1c_7qtXX_](https://github.com/user-attachments/assets/eb4f3e7e-2d0d-4473-87ab-1d63a211268f)

---

## 🧠 1. `select` Only the Columns You Need (Avoid `SELECT *`)

```ruby
User.select(:id, :email).where(active: true)
```

🔍 **Why:**
Avoids loading unnecessary data into memory — faster DB queries and lighter memory footprint.

✅ **Pro Tip:**
Combine with `.find_each` to process large records efficiently:

```ruby
User.select(:id, :email).find_each(batch_size: 500) do |user|
  # process user
end
```

---

## ⚡ 2. `pluck` Over `.map(&:field)` = Ultra-Fast Fetch

```ruby
User.pluck(:email)
```

🧠 **Why:**
Direct SQL SELECT — skips ActiveRecord object creation = faster execution!

❌ Avoid this:

```ruby
User.all.map(&:email) # Slow & memory heavy
```

✅ **Pro Tip:**

```ruby
User.where(active: true).pluck(:id, :email)
```

---

## 🧙‍♂️ 3. `find_by` Over `where(...).first`

```ruby
User.find_by(email: "demo@example.com")
```

🔥 **Why:**
Less verbose, more intention-revealing, optimized under the hood for early exit.

---

## 🧩 4. Memoization Pattern for Heavy Methods

```ruby
def user_profile
  @user_profile ||= fetch_profile_from_api
end
```

⛓️ **Why:**
Avoids repeated expensive calls. Elegant state caching pattern for instance methods.

✅ **Pro Tip:**
Use for:

* API responses
* DB-intensive lookups
* File reads

---

## 🧼 5. Service Objects + `yield_self`

```ruby
class InvoiceGenerator
  def self.call(order)
    order.yield_self do |o|
      # process invoice
    end
  end
end
```

🛠 **Why:**
Keeps business logic clean, separates concerns, and improves testability.

✅ **Pro Tip:**
Chain-friendly and readable for complex service pipelines.

---

## 📊 6. `scope` for Reusable, Chainable Queries

```ruby
scope :active, -> { where(active: true) }
scope :recent, -> { order(created_at: :desc) }
```

💎 **Why:**
Clean and DRY code. Chaining becomes beautiful:

```ruby
User.active.recent.limit(10)
```

---

## 🔐 7. `before_save if:` Condition for Lean Callbacks

```ruby
before_save :normalize_email, if: -> { email_changed? }

def normalize_email
  self.email = email.strip.downcase
end
```

⚠️ **Why:**
Avoid running unnecessary code — optimize only when needed.

✅ **Pro Tip:**
Use conditional callbacks (`if`, `unless`) instead of burying checks inside methods.

---

## 🚀 8. `update_columns` vs `update` vs `update_attribute`

```ruby
user.update_columns(views_count: user.views_count + 1)
```

⛔ `update` runs validations and callbacks.
✅ `update_columns` skips validations, callbacks — **use only when safe.**

📌 **When to use:**

* Updating counters/logs
* System-generated updates

---

## 🧬 9. JSON Column Access via `store_accessor`

```ruby
store_accessor :preferences, :theme, :notifications_enabled
```

🎯 **Why:**
Access JSON fields like normal attributes — reduces boilerplate!

```ruby
user.theme # => "dark"
user.notifications_enabled = true
```

---

## 🌀 10. Rails Console Ninja Moves

```ruby
app.get '/dashboard'
app.response.body
```

🐱‍👤 **Why:**
Use the `app` object in console to simulate real HTTP requests.

✅ **Pro Tip:**
Use `.helpers`, `.url_helpers`, and `.reload!` like a pro:

```ruby
helpers.number_to_currency(12345.67)
Rails.application.routes.url_helpers.user_path(1)
reload!
```

---

## 🔄 Bonus: Optimize Seeds with `find_or_create_by`

```ruby
User.find_or_create_by(email: 'admin@example.com') do |u|
  u.name = 'Admin'
  u.role = 'superadmin'
end
```

🔥 **Why:**
Avoid duplicate records and make your seed idempotent (safe to run multiple times).

---

## ✨ Final Pro Tips

✅ Use `.includes` or `.eager_load` to prevent N+1 queries
✅ Prefer `delegate` for cleaner model relationships
✅ Cache partials or fragments with `cache` helper
✅ Use `bullet` gem in dev to catch performance issues
✅ Profile slow SQL queries using `rails db:verbose` and `EXPLAIN`

---

## 📌 Conclusion

You don’t need to write *more code* — you need to write **optimized code**.
Rails gives you everything, but it's up to you to use its **power features wisely**. ✨

These shortcuts aren’t just tricks — they’re a philosophy:

> “**Write less. Do more. Stay fast. Stay elegant.**”
