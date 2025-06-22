---
layout: home
title: "Ruby on Rails Secret Methods"
date: 2025-06-22
categories: "Ruby On Rails"
tags: [Ruby On Rails, Methods, Coding, Programming, Secrets]
image: 'https://github.com/user-attachments/assets/55d72493-ec8d-420d-b801-1dc68386cc70'
---

# 💎 Ruby on Rails Secrets: Hidden Methods That Will Surprise You! 🚀✨

Ruby on Rails isn’t just famous for its **Convention over Configuration** and productivity — it also hides some **powerful secret methods** 🕵️‍♂️ that many developers don’t discover until years into their career!

Today, I’m pulling back the curtain to reveal some of these hidden gems, explain them with real-world examples, and share bonus “recipes” to make your Rails coding even more magical! 🧙‍♂️✨

![1_iP7tRgSsM7k-osyDTKQyYw](https://github.com/user-attachments/assets/55d72493-ec8d-420d-b801-1dc68386cc70)

Let’s get started! 🚀

---

## 1️⃣ `try` – The Safe Navigator before Safe Navigation

Ever needed to call a method on an object that might be `nil`? You probably write:

```ruby
user && user.name
```

OR use the safe navigation operator (`&.`):

```ruby
user&.name
```

But did you know Rails gave you this power before Ruby itself?

```ruby
user.try(:name)
```

**Example:**

```ruby
user = nil
puts user.try(:name) # => nil (No error!)
```

**Why it’s surprising:**
It lets you chain method calls safely **even on `nil`**, long before Ruby added `&.` in 2.3.

---

## 2️⃣ `presence` – Cleaner than `.blank? ? nil : value`

Imagine you check if a string is blank and fallback:

```ruby
name = user.name
name = nil if name.blank?
```

Rails shortcut: **`presence`**

```ruby
name = user.name.presence || "Guest"
```

**Example:**

```ruby
"".presence      # => nil
"Hello".presence # => "Hello"
```

Perfect for defaults! 🎉

---

## 3️⃣ `in?` – Readable `include?` for Objects

Ever do:

```ruby
[1, 2, 3].include?(value)
```

Rails lets you flip it:

```ruby
value.in? [1, 2, 3]
```

**Example:**

```ruby
1.in? [1, 2, 3]   # => true
5.in? [1, 2, 3]   # => false
```

So natural to read! 📖✅

---

## 4️⃣ `delegate` – One-liner Method Forwarding

Want to forward a method to an associated object? Don’t write manual delegation! Use `delegate`.

**Example:**

```ruby
class User < ApplicationRecord
  has_one :profile
  delegate :bio, to: :profile, allow_nil: true
end

user.bio # Calls user.profile.bio
```

One line, zero boilerplate! ✨

---

## 5️⃣ `tap` + `then` – Chain and Peek

`tap` is a Ruby core method, but in Rails it shines with blocks:

* `tap` lets you do something **without breaking the chain**.
* `then` (a.k.a `yield_self`) lets you transform elegantly.

**Example:**

```ruby
User.new.tap { |u| u.name = "Alice" }.save

value = [1, 2, 3].then { |arr| arr.sum }
# => 6
```

Use `tap` to peek & modify, `then` to transform. 🌀

---

## 6️⃣ `pluck` – Faster than `map`

Instead of:

```ruby
User.all.map(&:name)
```

Use:

```ruby
User.pluck(:name)
```

It fetches **only the columns you need**, directly from SQL — no ActiveRecord objects loaded = ⚡️ Faster and memory-friendly!

---

## 7️⃣ `find_each` – Lazy Batch Processing

Don’t do:

```ruby
User.all.each { |u| process(u) } # Loads all at once 🚫
```

Do this:

```ruby
User.find_each(batch_size: 1000) do |user|
  process(user)
end
```

It loads **1000 records at a time**, perfect for large tables. 🗃️

---

## 8️⃣ `attribute_before_type_cast` – Raw Value Before Casting

Want to see what was originally given before Rails casts it?

```ruby
user = User.new(age: "25")
user.age # => 25 (integer)
user.age_before_type_cast # => "25" (string)
```

Great for debugging! 🧐

---

## 9️⃣ `to_query` – Instantly Build Query Strings

Need a quick URL query?

```ruby
{ name: "Alice", age: 25 }.to_query
# => "name=Alice&age=25"
```

Perfect for dynamic redirects & API calls. 🧩

---

## 🔟 `touch` – Update `updated_at` Fast

Want to bump `updated_at` **without changing other fields**?

```ruby
post.touch
```

It also updates `belongs_to :touch` relationships!

---

## 🎁 **BONUS SECRET RECIPES!**

👉 **`acts_as_list`**
Use the gem or your own version to easily order rows.

👉 **`enum`**
Define status as enum:

```ruby
class Order < ApplicationRecord
  enum status: [:pending, :paid, :shipped]
end

order.paid! # changes status
order.paid? # checks status
```

👉 **`find_or_create_by`**
One-liner to find or create:

```ruby
User.find_or_create_by(email: "alice@example.com")
```

👉 **`update_columns`**
Update attributes **without validations or callbacks** (use carefully!):

```ruby
user.update_columns(name: "Bob")
```

👉 **`silence_warnings`**
Run code ignoring warnings:

```ruby
ActiveSupport::Deprecation.silence do
  # risky code here
end
```

---

## 🚀 **Wrap Up**

Rails is full of these magical shortcuts and hidden helpers! 🌟
Mastering them will save you **time**, reduce boilerplate, and make you feel like a wizard 🧙‍♂️ every time you code.

✨ **Which one surprised you the most?**
💬 Comment below & share your own Rails secrets!

Happy coding, Rails rockstars! 🚀🔥💎

---

## 📌 **Like this? Save it, share it, and follow for more Ruby on Rails magic! 🐾✨**
