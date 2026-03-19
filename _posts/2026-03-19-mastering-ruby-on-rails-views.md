---
layout: home
title: "Mastering Ruby on Rails Views"
date: 2026-03-19
categories: "Ruby On Rails"
tags: [Ruby On Rails, Views, MVC, Techniques, Hidden Features, Optimization, HTML, Gems]
image: 'https://github.com/user-attachments/assets/4f6bcb83-281e-4395-b64f-e8575825e117'
---

# 🎨 Mastering Ruby on Rails Views: Hidden Tricks, Performance Hacks & Gems You MUST Know 🚀

When building a Ruby on Rails application, **Views** are where your users *actually experience your product*. But most developers only scratch the surface — using basic ERB templates without unlocking the true power of Rails Views.

In this blog, we’ll uncover:
✅ Hidden features
✅ Smart tricks
✅ Performance optimizations
✅ Powerful gems
✅ Real-world examples

<img width="1024" height="1536" alt="ChatGPT Image Mar 19, 2026, 11_39_11 PM" src="https://github.com/user-attachments/assets/4f6bcb83-281e-4395-b64f-e8575825e117" />

Let’s transform your views from *basic templates* → *high-performance UI engines* 💡

---

# 🧠 What Are Rails Views (Quick Recap)

Rails Views are responsible for rendering the UI using:

* **ERB (`.html.erb`)**
* **HAML / SLIM**
* **Partials**
* **Layouts**

They follow MVC:
👉 Controller → prepares data
👉 View → displays data

---

# 🔥 1. Hidden Features of Rails Views You Probably Missed

## 🧩 1.1 `content_for` & `yield` Magic

Used for dynamic layouts.

```erb
<% content_for :title do %>
  Dashboard
<% end %>
```

In layout:

```erb
<title><%= yield :title %></title>
```

💡 **Use Case:** Dynamic page titles, scripts, meta tags

---

## 🧩 1.2 `capture` for Reusable Blocks

```erb
<% greeting = capture do %>
  Hello, <%= current_user.name %>
<% end %>

<%= greeting %>
```

💡 Helps build reusable UI snippets dynamically.

---

## 🧩 1.3 `provide` (Cleaner Alternative)

```erb
<% provide(:title, "Home") %>
```

✔ Cleaner than `content_for`

---

## 🧩 1.4 `render` Variations (Super Powerful)

```erb
<%= render @users %>
```

Rails automatically picks `_user.html.erb`

💡 This is called **Collection Rendering** — super efficient!

---

# ⚡ 2. Performance Optimization Techniques

## 🚀 2.1 Fragment Caching (Must Know)

```erb
<% cache @product do %>
  <%= render @product %>
<% end %>
```

💡 Rails stores this block → avoids re-rendering

---

## 🚀 2.2 Russian Doll Caching 🪆

```erb
<% cache @post do %>
  <%= render @post.comments %>
<% end %>
```

💡 Nested caching for complex UI

---

## 🚀 2.3 Avoid N+1 Queries in Views

❌ Bad:

```erb
<% @posts.each do |post| %>
  <%= post.user.name %>
<% end %>
```

✔ Fix in controller:

```ruby
@posts = Post.includes(:user)
```

---

## 🚀 2.4 Use `pluck` Instead of Full Objects

```ruby
User.pluck(:name)
```

💡 Faster than loading entire records

---

## 🚀 2.5 Streaming Views (Advanced ⚡)

```ruby
render stream: true
```

💡 Sends response in chunks → faster perceived load

---

# 🧙 3. Pro-Level View Tricks

## 🎯 3.1 View Helpers for Clean Code

```ruby
def format_price(price)
  number_to_currency(price)
end
```

Use in view:

```erb
<%= format_price(100) %>
```

✔ Keeps views clean and readable

---

## 🎯 3.2 Safe HTML Rendering

```erb
<%= sanitize(user_input) %>
```

⚠️ Avoid:

```erb
<%= raw user_input %>
```

💡 Prevents XSS attacks

---

## 🎯 3.3 Conditional Rendering Shortcut

```erb
<%= render @post if @post.present? %>
```

---

## 🎯 3.4 Dynamic Partial Names

```erb
<%= render "cards/#{@user.role}" %>
```

💡 Super useful for role-based UI

---

# 💎 4. Powerful Gems for Rails Views

## 🌟 4.1 Draper (Decorator Pattern)

👉 Clean separation of logic

```ruby
@user.decorate.full_name
```

✔ Keeps views logic-free

---

## 🌟 4.2 ViewComponent (by GitHub)

Component-based views:

```ruby
render(UserComponent.new(user: @user))
```

💡 Scalable UI architecture

---

## 🌟 4.3 Slim / HAML (Cleaner Syntax)

Slim example:

```slim
h1 Hello #{@user.name}
```

✔ Less code, cleaner UI

---

## 🌟 4.4 Cells (View Models)

```ruby
cell(:comment, @comment)
```

💡 Encapsulates view logic

---

## 🌟 4.5 Phlex (Modern Approach 🚀)

Ruby-based views:

```ruby
class Card < Phlex::HTML
  def template
    h1 "Hello World"
  end
end
```

---

# 🧱 5. Folder Structure Best Practices

```
app/views/
  users/
    index.html.erb
    _user.html.erb
  shared/
    _navbar.html.erb
    _footer.html.erb
```

✔ Use `shared/` for reusable components
✔ Keep partials small & focused

---

# 🧪 6. Real-World Example (Optimized View)

```erb
<% cache @users do %>
  <%= render @users %>
<% end %>
```

Controller:

```ruby
@users = User.includes(:profile)
```

Partial:

```erb
<div class="user-card">
  <h3><%= user.name %></h3>
</div>
```

🔥 Result:

* Faster rendering
* Clean code
* Scalable UI

---

# ⚠️ 7. Common Mistakes to Avoid

❌ Heavy logic in views
❌ Too many partials (over-fragmentation)
❌ Ignoring caching
❌ Using `raw` dangerously
❌ Not optimizing DB queries

---

# 🎯 Final Thoughts

Rails Views are not just templates — they are a **performance layer + UX engine**.

👉 Master these:

* Caching strategies
* Partial rendering
* Helper usage
* Component-based design

And your app will:
🚀 Load faster
🎨 Look cleaner
📈 Scale better

---

# 💬 Powerful Quote

> “First impressions are everything — and in web apps, your Views ARE the first impression.” 💡

---

# 🔥 Bonus Tip

If you want to level up even more:
👉 Combine **Rails Views + React (Hybrid Approach)**
👉 Use **Hotwire (Turbo + Stimulus)** for modern UI without heavy JS
