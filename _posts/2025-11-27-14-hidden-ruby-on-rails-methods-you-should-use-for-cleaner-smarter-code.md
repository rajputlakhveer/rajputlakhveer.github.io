---
layout: home
title: "14 Hidden Ruby on Rails Methods You Should Use for Cleaner Smarter Code"
date: 2025-11-27
categories: "Ruby On Rails"
tags: [Ruby On Rails, Ruby, Methods, Clean Code, Programming, Software Development, Software Engineer]
image: 'https://github.com/user-attachments/assets/27e23549-1c90-4088-be7a-5332fe59358b'
---

# 🌟 **14 Hidden Ruby on Rails Methods You Should Use for Cleaner, Smarter Code!**

Writing clean and elegant code is not just a skill—it’s a *signature* of a great Rails developer. Ruby on Rails hides some real gems 🪙 behind its expressive syntax. Many developers miss these powerful methods that make your code shorter, more readable, and highly maintainable.

In this blog, you’ll discover **14 hidden Rails & Ruby methods** that can instantly level up your code quality—with clear examples.
Plus, you’ll get actionable **clean code tips** at the end.

Let’s unlock them 🔓🚀

<img width="1024" height="1536" alt="ChatGPT Image Nov 27, 2025, 11_29_06 PM" src="https://github.com/user-attachments/assets/27e23549-1c90-4088-be7a-5332fe59358b" />

---

## 🔹 **1. `presence` – Smart Nil or Blank Handling**

Instead of writing long conditional checks, use `presence` to return value only if it’s not blank.

### ❌ Old Way

```ruby
name = params[:name]
name = nil if name.blank?
```

### ✅ Clean Way

```ruby
name = params[:name].presence
```

### ⭐ Why?

Saves time, removes boilerplate, improves readability.

---

## 🔹 **2. `try` – Safe Method Calling**

Avoid `nil` errors when calling methods on possibly nil objects.

### ❌ Old Way

```ruby
user && user.profile && user.profile.city
```

### ✅ Clean Way

```ruby
user.try(:profile).try(:city)
```

### ⭐ Tip

Use `&.` (Safe Navigation Operator) when possible:

```ruby
user&.profile&.city
```

---

## 🔹 **3. `compact_blank` – Remove Empty Values Like a Pro**

Works on arrays and hashes to remove `nil`, `""`, and blank values.

### Example

```ruby
["Ruby", "", nil, "Rails"].compact_blank
# => ["Ruby", "Rails"]
```

### Hash Example

```ruby
{ name: "Raj", age: nil, country: "" }.compact_blank
# => { name: "Raj" }
```

---

## 🔹 **4. `delegate` – Cleaner Model Methods**

Avoid writing forwarding methods manually.

### ❌ Without `delegate`

```ruby
def city
  profile.city
end
```

### ✅ With `delegate`

```ruby
delegate :city, to: :profile
```

### Bonus

```ruby
delegate :city, :state, to: :profile, prefix: true
# => user.profile_city
```

---

## 🔹 **5. `pluck` – Faster DB Queries**

Fetch only required columns.

### Example

```ruby
User.pluck(:email)
# => ["a@mail.com", "b@mail.com"]
```

### Why?

Avoids loading full AR objects → faster, lighter memory usage ⚡

---

## 🔹 **6. `in_batches` – Process Big Data Without Killing RAM**

Perfect for large dataset operations.

```ruby
User.in_batches(of: 1000) do |batch|
  batch.update_all(active: true)
end
```

---

## 🔹 **7. `transform_values` – Clean Hash Transformations**

```ruby
{ a: 1, b: 2 }.transform_values { |v| v * 10 }
# => { a: 10, b: 20 }
```

---

## 🔹 **8. `extract_options!` – Cleaner Arguments in Methods**

Rails automatically pulls hash options from array parameters.

```ruby
def signup(*args)
  options = args.extract_options!
  puts options[:name]
end

signup(:admin, name: "Lakhveer")
```

---

## 🔹 **9. `tap` – Debug-Friendly, Elegant Chaining**

```ruby
user = User.new.tap do |u|
  u.name = "Raj"
  u.age = 25
end
```

### Why?

Super useful for object setup + debugging pipelines.

---

## 🔹 **10. `with_indifferent_access` – Access Hash Keys as String or Symbol**

```ruby
params = { "name" => "Raj" }.with_indifferent_access
params[:name]  # => "Raj"
params["name"] # => "Raj"
```

---

## 🔹 **11. `slice` – Extract Only Needed Keys**

```ruby
params.slice(:email, :password)
# Perfect for whitelisting
```

---

## 🔹 **12. `except` – Remove Unwanted Keys**

```ruby
params.except(:token, :signature)
```

---

## 🔹 **13. `blank?` vs `present?` – Cleaner Boolean Logic**

```ruby
if user.present?
  # clean & expressive
end
```

---

## 🔹 **14. `find_each` – Load Records in Batches Automatically**

Instead of:

```ruby
User.all.each { ... }
```

Use:

```ruby
User.find_each(batch_size: 1000) do |user|
  puts user.email
end
```

Efficient, scalable, production-safe.

---

# 💡 Clean Code Tips Every Rails Developer Should Follow

## ✨ **1. Follow SLAP (Single Level of Abstraction Principle)**

Each method should do only **one thing**.

## ✨ **2. Use Service Objects, Decorators, Presenters**

Keep controllers skinny and models clean.

## ✨ **3. Use Meaningful Names**

Variables like `x`, `data`, `temp` kill readability.

## ✨ **4. Remove Dead Code**

Unused methods, comments, files → clean them regularly.

## ✨ **5. Follow Consistent Style**

Use RuboCop + standardrb for consistency.

## ✨ **6. Avoid Fat Models**

Push logic into modules or service classes.

## ✨ **7. Prefer `early return` over nested conditions**

```ruby
return unless user.present?
```

## ✨ **8. Write Smaller, Focused Methods**

Each method ≤ 10–15 lines for readability.

---

# 🎉 Conclusion

Rails is full of hidden gems 💎 that make your code elegant, readable, and scalable.
By using methods like **presence**, **delegate**, **pluck**, **try**, **compact_blank**, and **tap**, your code becomes more Ruby-ish and clean.

Write less, express more—that’s the Rails magic! 🚀🔥
