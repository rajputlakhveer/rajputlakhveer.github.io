---
layout: home
title: "Decorators Presenters and Commands in Ruby on Rails"
date: 2024-12-17
categories: "Ruby on Rails"
tags: [Ruby, Ruby On Rails. ROR, Decorators, Presenters, Commands]
image: 'https://github.com/user-attachments/assets/ca174eb3-e424-4c4c-884c-a00d008af6b4'
---

# 🚀 Mastering Decorators, Presenters, and Commands in Ruby on Rails

Ruby on Rails (RoR) is a framework that thrives on clean and maintainable code. But have you ever wondered how to better separate concerns, handle presentation logic, or encapsulate business logic efficiently? Today, we dive into **Decorators**, **Presenters**, and **Commands** — powerful patterns that will elevate your Rails applications! 🌟

![Untitled design (4)](https://github.com/user-attachments/assets/ca174eb3-e424-4c4c-884c-a00d008af6b4)

---

## 🎭 Decorators: Adding Behavior without Changing Objects

A **Decorator** is used to add extra behavior to an object dynamically, without modifying its original class. It’s particularly useful for organizing view-related logic.

### Example:

```ruby
# app/decorators/user_decorator.rb
class UserDecorator < SimpleDelegator
  def full_name
    "#{first_name} #{last_name}"
  end

  def formatted_join_date
    created_at.strftime("%B %d, %Y")
  end
end

# Usage in Controller
@user = User.find(params[:id])
@decorated_user = UserDecorator.new(@user)
```

### Hack:
- Use the `Draper` gem for a more streamlined approach to creating decorators. It integrates seamlessly with Rails:

```ruby
# Gemfile
gem 'draper'
```

```ruby
class UserDecorator < Draper::Decorator
  delegate_all

  def display_name
    object.name.upcase
  end
end

# Usage
@user = User.find(params[:id]).decorate
```

### Best Use:
- View-specific logic like formatting dates, names, or conditional display content.

---

## 🎨 Presenters: Focus on Presentation Layer

Presenters are similar to Decorators but with a specific focus on enhancing the presentation logic. They act as a middle layer between your models and views, often combining multiple models into one cohesive interface.

### Example:

```ruby
# app/presenters/order_presenter.rb
class OrderPresenter
  def initialize(order)
    @order = order
  end

  def total_price
    @order.items.sum(&:price) + shipping_cost
  end

  def shipping_cost
    @order.expedited? ? 10 : 5
  end
end

# Usage in Controller
@order = Order.find(params[:id])
@presenter = OrderPresenter.new(@order)
```

### Hack:
- Use the `view_context` in your Presenter for direct access to Rails helpers, making it even more flexible:

```ruby
def formatted_total_price
  view_context.number_to_currency(total_price)
end
```

### Best Use:
- When you need to abstract complex presentation logic from the view.

---

## 🛠️ Commands: Encapsulate Business Logic

The **Command** pattern encapsulates a business operation in a single class, making it reusable, testable, and easy to maintain. Commands are ideal for performing tasks that might otherwise clutter your models or controllers.

### Example:

```ruby
# app/commands/create_order_command.rb
class CreateOrderCommand
  def initialize(user, cart)
    @user = user
    @cart = cart
  end

  def execute
    Order.create!(user: @user, total_price: @cart.total_price)
  end
end

# Usage in Controller
command = CreateOrderCommand.new(current_user, current_cart)
@order = command.execute
```

### Hack:
- Chain commands together for workflows using a service object:

```ruby
# app/services/order_service.rb
class OrderService
  def initialize(user, cart)
    @user = user
    @cart = cart
  end

  def place_order
    command = CreateOrderCommand.new(@user, @cart)
    notify_user(command.execute)
  end

  private

  def notify_user(order)
    UserMailer.order_confirmation(order).deliver_later
  end
end
```

### Best Use:
- Encapsulating domain logic that doesn’t belong in controllers or models, like workflows or API interactions.

---

## ⚡ Quick Hacks for Efficient Usage

1. **Use Decorators** for enhancing objects with reusable, view-specific methods.
2. **Leverage Presenters** when combining data from multiple models into a single, clean interface for views.
3. **Apply Commands** to separate complex operations from models and controllers.
4. **Naming Matters** — Choose meaningful names for Decorators, Presenters, and Commands to clearly indicate their purpose.
5. **Testing Made Easy** — Since these patterns separate concerns, they’re easier to unit test individually. 🧪

---

## 🌟 Conclusion

By integrating **Decorators**, **Presenters**, and **Commands** into your Ruby on Rails projects, you’ll write cleaner, more maintainable code. These patterns help you stay organized, improve testability, and keep your application logic decoupled. 🚀

What are your favorite patterns in Rails? Let us know in the comments! ✨

