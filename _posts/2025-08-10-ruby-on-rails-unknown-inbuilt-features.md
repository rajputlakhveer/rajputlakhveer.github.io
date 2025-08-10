---
layout: home
title: "Ruby on Rails Unknown Inbuilt Features"
date: 2025-08-10
categories: "Ruby On Rails"
tags: [Ruby On Rails, Features, Simple, Programming, Software Development]
image: 'https://github.com/user-attachments/assets/b646e281-5dcb-430f-ad49-f50b43f06be2'
---

# 🚀 Ruby on Rails: Unknown Inbuilt Features You Wish You Knew Earlier! 💎💻

Ruby on Rails (RoR) is famous for its *developer happiness*, but behind the scenes, it hides a treasure chest of **lesser-known inbuilt features** that can make your code cleaner, faster, and smarter. 💡

In this blog, we’ll explore some *hidden gems* of Rails — **from magical ActiveRecord tricks to time-saving helpers** — complete with examples and bonus hacks to level up your coding game. 🎯

<img width="1200" height="630" alt="83d09280c8809460222474ef8f7dbc94ee39abc9" src="https://github.com/user-attachments/assets/b646e281-5dcb-430f-ad49-f50b43f06be2" />

---

## 1️⃣ **`.pluck` — Fetch Data Without the Baggage** 🪄

Need only certain column values? Instead of fetching entire ActiveRecord objects (which is slower), use `.pluck`!

```ruby
# Without pluck
User.all.map(&:email)  
# Fetches all data into memory — wasteful 😩

# With pluck
User.pluck(:email)
# Returns array of emails directly from DB 🚀
```

**Why it’s great:** Speeds up queries and reduces memory usage. Perfect for large datasets!

---

## 2️⃣ **`try` — Safe Method Calls Without Conditionals** 🛡️

Avoid long `nil` checks and make your code elegant.

```ruby
# Without try
username = user && user.profile && user.profile.username

# With try
username = user.try(:profile).try(:username)
```

**Hack:** In Rails 5+, you can also use the safe navigation operator:

```ruby
username = user&.profile&.username
```

---

## 3️⃣ **`find_each` — Batch Processing Without Memory Explosions** 📦

When dealing with thousands (or millions) of records, `.each` will load all of them into memory. **Bad idea.**
Use `.find_each` to load them in batches.

```ruby
User.find_each(batch_size: 1000) do |user|
  UserMailer.newsletter(user).deliver_later
end
```

**Why it’s great:** Prevents your app from crashing with *Out of Memory* errors.

---

## 4️⃣ **`.in_batches` — Even More Batch Power** 🏋️‍♂️

Want to update or delete in chunks directly in DB?

```ruby
User.in_batches(of: 500).update_all(active: true)
```

**Hack:** Unlike `find_each`, this doesn’t instantiate objects, so it’s super fast! ⚡

---

## 5️⃣ **`.where.not` — The Anti-Query** 🚫

Rails has a `.not` query builder that makes exclusions simple.

```ruby
User.where.not(status: 'inactive')
```

**Why it’s great:** More readable and less error-prone than writing raw SQL `!=`.

---

## 6️⃣ **`touch` — Auto-Update Timestamps Without Changing Data** ⏰

Need to refresh `updated_at` without changing actual content?

```ruby
post.touch
# updates post.updated_at to current time
```

**Pro Tip:** Great for cache invalidation or triggering ActiveRecord callbacks.

---

## 7️⃣ **`reload` — Refresh From DB Instantly** 🔄

If your object might be stale, reload it directly from DB:

```ruby
user.reload
```

**Why it’s great:** Avoids stale data when another process updates the record.

---

## 8️⃣ **`find_or_create_by` — One-Liner Record Lookup** 🎯

Stop writing boilerplate to check existence before creation.

```ruby
User.find_or_create_by(email: "test@example.com") do |user|
  user.name = "Test User"
end
```

---

## 9️⃣ **Rails Time Helpers — Natural Language Dates** 🗓️

Rails understands plain English for time calculations:

```ruby
3.days.ago       # => 2025-08-07
2.weeks.from_now # => 2025-08-24
```

**Bonus:** Works with hours, months, years, etc.

---

## 🔟 **`.except` & `.slice` — Smart Hash Filtering** 🧹

Perfect for params cleanup.

```ruby
params.except(:password, :token) # removes keys
params.slice(:name, :email)      # keeps only keys
```

---

## 🎁 Bonus Hacks & Tricks

💡 **1. Use `rails db:seed_fu` (via seed-fu gem)** — For idempotent seed data.
💡 **2. Use `Rails.logger.debug`** instead of `puts` for production-friendly logging.
💡 **3. Chain `.select` and `.map` carefully** — Use `.pluck` instead for speed.
💡 **4. Use `delegate` in models** to clean up method forwarding.

```ruby
class Order < ApplicationRecord
  belongs_to :customer
  delegate :name, :email, to: :customer, prefix: true
end

order.customer_name # direct access!
```

💡 **5. Use `.presence` to shorten nil/blank checks:**

```ruby
name = params[:name].presence || "Guest"
```

---

## ✨ Final Thoughts

Rails isn’t just about *conventions over configuration* — it’s about **hidden superpowers** waiting to be unlocked. By mastering these inbuilt features, you’ll write cleaner, faster, and smarter code. 🏆

Next time you’re coding, try using one of these tricks — you might just wonder how you ever lived without it. 🚀
