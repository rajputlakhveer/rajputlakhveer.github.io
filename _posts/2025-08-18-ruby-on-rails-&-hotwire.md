---
layout: home
title: "Ruby on Rails & Hotwire"
date: 2025-08-18
categories: "Ruby On Rails"
tags: [Ruby On Rails, Hotwire, Programming, HTML, Wire, Developer]
image: 'https://github.com/user-attachments/assets/4b12e4ee-276b-4949-9fb3-e21860cf1d78'
---

# 🚀 Ruby on Rails + Hotwire: The Future of Modern Web Apps Without Heavy JavaScript

Ruby on Rails has always been about **developer happiness and productivity** 💎. With the introduction of **Hotwire**, Rails made building **fast, dynamic, and interactive web applications** much simpler—without depending heavily on JavaScript frameworks like React or Vue.

Hotwire (short for **HTML Over The Wire**) is a **front-end framework** by Basecamp that powers Rails apps with real-time updates, partial page rendering, and interactivity—while still keeping your codebase simple and mostly in Ruby.

In this blog, let’s break down Hotwire and its components, explore how to use them in Rails, see code examples, and discuss the **best use cases**. 🌟

<img width="964" height="707" alt="1_kJw2DD4pxXNFeOFwICn47A (1)" src="https://github.com/user-attachments/assets/4b12e4ee-276b-4949-9fb3-e21860cf1d78" />

---

## 🔥 What is Hotwire?

Hotwire is made up of **3 main components**:

1. **Turbo** ⚡ – Replaces Rails UJS, speeds up page navigation, and enables partial updates.
2. **Stimulus** 🎛 – A lightweight JavaScript framework for sprinkling interactivity.
3. **Strada** 📱 – Bridges Hotwire with native mobile apps (iOS & Android).

Together, they give you the **power of SPA-like apps**—but with the **simplicity of server-rendered HTML**.

---

## 1️⃣ Turbo: Speed Without the JavaScript Headache ⚡

Turbo is the core of Hotwire and has three flavors:

### 🔹 Turbo Drive

Replaces full-page reloads with **partial updates**.

* Works by intercepting links and form submissions.
* Loads new pages via AJAX and swaps HTML into the DOM.

👉 **Example:**

```erb
<!-- app/views/articles/index.html.erb -->
<%= link_to "Read Article", article_path(article), data: { turbo_frame: "main" } %>

<turbo-frame id="main">
  <!-- Content of the article will load here -->
</turbo-frame>
```

✅ **Feature**: Pages feel like SPAs without writing extra JavaScript.
💡 **Best Use Case**: Faster navigation in blogs, dashboards, or admin panels.

---

### 🔹 Turbo Frames

Allows updating **just a portion of the page** instead of reloading the whole thing.

👉 **Example:**

```erb
<!-- app/views/comments/index.html.erb -->
<turbo-frame id="comments">
  <%= render @comments %>
</turbo-frame>

<%= form_with(model: Comment.new, data: { turbo_frame: "comments" }) do |form| %>
  <%= form.text_field :content %>
  <%= form.submit "Post" %>
<% end %>
```

When a new comment is submitted, only the `comments` frame updates 🎯.

✅ **Feature**: Eliminates unnecessary reloads.
💡 **Best Use Case**: Comment sections, chat messages, notifications.

---

### 🔹 Turbo Streams

Push **real-time updates** to clients via Action Cable (WebSockets).

👉 **Example:**

```erb
# app/views/comments/create.turbo_stream.erb
<turbo-stream action="append" target="comments">
  <template>
    <%= render @comment %>
  </template>
</turbo-stream>
```

Now, when a comment is created, it **instantly appears for all users** connected 🎉.

✅ **Feature**: Live updates with almost no JavaScript.
💡 **Best Use Case**: Live chat apps, collaborative tools, stock dashboards.

---

## 2️⃣ Stimulus: Sprinkling Interactivity 🎛

Stimulus is a small JavaScript framework that works **with server-rendered HTML**. It doesn’t replace Turbo—it **complements** it.

👉 **Example:**

```erb
<!-- app/views/articles/show.html.erb -->
<div data-controller="like">
  <button data-action="click->like#increment">👍 Like</button>
  <span data-like-target="count">0</span>
</div>
```

```js
// app/javascript/controllers/like_controller.js
import { Controller } from "@hotwired/stimulus"

export default class extends Controller {
  static targets = ["count"]

  increment() {
    let count = parseInt(this.countTarget.innerText)
    this.countTarget.innerText = count + 1
  }
}
```

✅ **Feature**: Keeps JS **modular and tied to HTML elements**.
💡 **Best Use Case**: Small UI interactions (likes, toggles, dropdowns, modals).

---

## 3️⃣ Strada: Hotwire for Mobile Apps 📱

Strada (still evolving) helps you build **hybrid apps**—where most views come from Rails via Hotwire, but some interactions integrate with native mobile functionality.

👉 **Example Use Case:**

* A **Rails-powered backend** serving Turbo Streams.
* Native iOS/Android UI (like camera or push notifications) enhanced via Strada.

✅ **Feature**: Shared logic between web + mobile apps.
💡 **Best Use Case**: Startups that want **mobile + web apps quickly** without duplicating effort.

---

## ⚡ Why Use Hotwire in Rails?

* ✅ Less JavaScript – Rails handles most of it.
* ✅ Real-time updates without complex setups.
* ✅ Faster navigation & interaction.
* ✅ Perfect for MVPs and production apps alike.
* ✅ Plays well with **existing JS frameworks** if needed.

---

## 🎯 Conclusion

Hotwire is Rails’ answer to the modern web’s demand for **speed and interactivity**—without the complexity of heavy JavaScript frontends. Whether you’re building a **blog**, a **live chat app**, or a **real-time dashboard**, Hotwire makes development faster, simpler, and still fun 💎.

👉 If you’re a Rails developer, **learning Hotwire is a must in 2025 and beyond!** 🚀

---

✨ **Bonus Tip**: Combine Hotwire with Rails 7’s import maps or esbuild for a blazing-fast developer workflow.
