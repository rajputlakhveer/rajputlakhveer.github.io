---
layout: home
title: "Ruby on Rails Design Patterns"
date: 2025-08-26
categories: "Ruby On Rails"
tags: [Ruby On Rails, Design Patterns, Programming, Software Development, Software Engineering]
image: 'https://github.com/user-attachments/assets/87858b8c-d098-4f61-ab2c-58ec3469d7c4'
---

# 🚀 Ruby on Rails Design Patterns You Must Know for Clean & Scalable Code

When working with **Ruby on Rails (RoR)**, developers often face challenges like keeping code clean, avoiding repetition, and managing complexity as applications grow. That’s where **Design Patterns** step in 👨‍💻.

Design Patterns are **proven solutions** to common problems in software development. They don’t reinvent the wheel but guide us toward structured, reusable, and maintainable code. In this blog, we’ll dive deep into **Rails-specific design patterns**, their use cases, and real examples.

<img width="960" height="600" alt="ruby" src="https://github.com/user-attachments/assets/87858b8c-d098-4f61-ab2c-58ec3469d7c4" />

---

## 1️⃣ **Service Object Pattern** 💼

### 📌 Problem:

Your controllers or models are doing too much—business logic is scattered everywhere.

### 📌 Solution:

Extract business logic into **Service Objects**. This makes controllers thinner and models focused only on persistence.

### ✅ Example:

```ruby
# app/services/user_signup_service.rb
class UserSignupService
  def initialize(user_params)
    @user_params = user_params
  end

  def call
    user = User.new(@user_params)
    if user.save
      WelcomeMailer.send_email(user).deliver_later
      user
    else
      nil
    end
  end
end
```

```ruby
# app/controllers/users_controller.rb
def create
  @user = UserSignupService.new(user_params).call
  if @user
    redirect_to dashboard_path, notice: "🎉 Signup successful!"
  else
    render :new
  end
end
```

👉 Keeps the controller lean and **business logic reusable**.

---

## 2️⃣ **Decorator Pattern** 🎭

### 📌 Problem:

You want to add presentation-related logic (formatting, display) without cluttering models or views.

### 📌 Solution:

Use **Decorators** to wrap models and add UI-specific behavior.

### ✅ Example (with `draper` gem):

```ruby
# app/decorators/user_decorator.rb
class UserDecorator < Draper::Decorator
  delegate_all

  def display_name
    "#{object.first_name} #{object.last_name}".titleize
  end

  def joined_date
    object.created_at.strftime("%B %d, %Y")
  end
end
```

```erb
<!-- In View -->
<p><%= @user.decorate.display_name %></p>
<p>Joined: <%= @user.decorate.joined_date %></p>
```

👉 Keeps models clean while **enhancing UI representation**.

---

## 3️⃣ **Query Object Pattern** 🔍

### 📌 Problem:

Complex queries with ActiveRecord clutter controllers or models.

### 📌 Solution:

Encapsulate queries into **Query Objects** for readability and reusability.

### ✅ Example:

```ruby
# app/queries/active_users_query.rb
class ActiveUsersQuery
  def self.call
    User.where(active: true).where("last_login_at >= ?", 30.days.ago)
  end
end
```

```ruby
# In Controller
@users = ActiveUsersQuery.call
```

👉 Makes **database logic reusable** and easier to maintain.

---

## 4️⃣ **Form Object Pattern** 📝

### 📌 Problem:

You have forms that update multiple models (e.g., User + Profile). Handling validation across them gets messy.

### 📌 Solution:

Use **Form Objects** to encapsulate form-specific validations and persistence.

### ✅ Example:

```ruby
# app/forms/signup_form.rb
class SignupForm
  include ActiveModel::Model

  attr_accessor :user_name, :email, :profile_bio

  validates :user_name, :email, presence: true

  def save
    return false unless valid?
    user = User.create!(name: user_name, email: email)
    Profile.create!(user: user, bio: profile_bio)
  end
end
```

```ruby
# In Controller
form = SignupForm.new(params[:signup])
if form.save
  redirect_to dashboard_path, notice: "Account created ✅"
else
  render :new
end
```

👉 Simplifies **multi-model forms** and keeps controllers DRY.

---

## 5️⃣ **Policy Object Pattern** 🔑

### 📌 Problem:

Authorization logic scattered across controllers and views.

### 📌 Solution:

Extract permission rules into **Policy Objects** (e.g., using `pundit` gem).

### ✅ Example:

```ruby
# app/policies/post_policy.rb
class PostPolicy < ApplicationPolicy
  def update?
    user.admin? || record.user == user
  end
end
```

```ruby
# In Controller
def update
  authorize @post
  @post.update(post_params)
end
```

👉 Centralizes **authorization logic** for better security & clarity.

---

## 6️⃣ **Presenter Pattern** 🎤

### 📌 Problem:

Views need multiple helper methods and formatting. Helpers get bloated.

### 📌 Solution:

Use **Presenter Objects** to represent data in a more structured way.

### ✅ Example:

```ruby
# app/presenters/order_presenter.rb
class OrderPresenter
  def initialize(order)
    @order = order
  end

  def formatted_total
    "$#{'%.2f' % @order.total}"
  end

  def delivery_status
    @order.delivered? ? "🚚 Delivered" : "⏳ In Progress"
  end
end
```

```erb
<!-- In View -->
<p><%= OrderPresenter.new(@order).formatted_total %></p>
<p><%= OrderPresenter.new(@order).delivery_status %></p>
```

👉 A clean way to **keep view logic separate from controllers**.

---

## 7️⃣ **Observer Pattern** 👀

### 📌 Problem:

You want to trigger background tasks (like sending emails or analytics) without bloating model callbacks.

### 📌 Solution:

Use **Observers** (or ActiveSupport::Notifications) to listen to events and respond.

### ✅ Example:

```ruby
# app/observers/user_observer.rb
class UserObserver < ActiveRecord::Observer
  def after_create(user)
    Analytics.track("User created", user_id: user.id)
  end
end
```

👉 Keeps models **focused** while still responding to lifecycle events.

---

# 🎯 Bonus Tips for Perfect Use of Design Patterns

✅ **Keep Controllers Skinny** → Use **Service Objects** and **Query Objects**.
✅ **Keep Models Focused** → Avoid business logic inside models.
✅ **Use the Right Pattern** → Don’t over-engineer. Choose a pattern only when needed.
✅ **Consistency is Key** → Stick to one approach across your app.
✅ **Refactor Early** → Don’t wait until your app gets messy.

---

# ✨ Final Thoughts

Rails provides **conventions** that already reduce boilerplate, but when your app grows, these **Design Patterns** help keep things organized, scalable, and clean.

Adopting **Service Objects, Query Objects, Decorators, and Policies** can **level up your Rails codebase** 💡.

🚀 So next time your controller feels bloated or your model looks messy, remember: *there’s a design pattern for that!*
