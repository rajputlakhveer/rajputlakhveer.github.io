---
layout: home
title: "Ruby On Rails Hacks And Tricks"
date: 2024-09-16
categories: "Ruby On Rails"
tags: [Ruby On Rails, Hacks, Tricks, Tips]
---

# Ruby on Rails Unique Hacks and Tricks Everyone Should Know 🚀

Ruby on Rails (RoR) is known for its ease of use and powerful features, but there are plenty of hidden tricks and hacks that can make your Rails development even smoother. In this blog, we’ll explore some unique Rails tips and techniques to help you write more efficient, maintainable, and clean code. Let’s dive in! 🌟

## 1. **Efficient Batch Processing with `find_each` 📦**

When working with large datasets, loading everything at once can be inefficient. Use `find_each` to process records in batches, reducing memory usage and improving performance.

```ruby
User.find_each(batch_size: 1000) do |user|
  # Process each user
end
```

## 2. **Retrieve Specific Attributes with `pluck` 🔧**

To fetch only the necessary attributes from the database, use `pluck` to avoid loading full ActiveRecord objects.

```ruby
user_ids = User.pluck(:id)
```

This method directly queries the specified columns, which is faster and more memory-efficient.

## 3. **Create Reusable Queries with `scope` 🔍**

Scopes are a great way to encapsulate common queries and keep your code clean and readable.

```ruby
class Post < ApplicationRecord
  scope :published, -> { where(published: true) }
  scope :recent, -> { order(created_at: :desc) }
end

# Usage
Post.published.recent
```

## 4. **Manage States with `enum` 🎛️**

Enums provide a way to map integer values to human-readable strings, simplifying state management.

```ruby
class Order < ApplicationRecord
  enum status: { pending: 0, processing: 1, shipped: 2, delivered: 3 }
end

# Usage
order = Order.new(status: :pending)
order.pending? # true
```

## 5. **Encapsulate Complex Validations with Custom Validators 🛠️**

Custom validators let you encapsulate complex validation logic, making your models cleaner and more maintainable.

```ruby
class CustomValidator < ActiveModel::Validator
  def validate(record)
    unless record.attribute.present?
      record.errors.add(:attribute, "can't be blank")
    end
  end
end

class User < ApplicationRecord
  validates_with CustomValidator
end
```

## 6. **Optimize Test Suites with `let` and `before` Blocks ⚡**

Use `let` and `before` blocks in RSpec to organize and speed up your tests, reducing duplication and improving readability.

```ruby
RSpec.describe User, type: :model do
  let(:user) { User.create(name: "Test User") }

  before do
    # Setup code
  end

  it "has a valid factory" do
    expect(user).to be_valid
  end
end
```

## 7. **Modularize Code with `ActiveSupport::Concern` 📦**

`ActiveSupport::Concern` allows you to create reusable modules for your models or controllers, promoting clean and modular code.

```ruby
module Trackable
  extend ActiveSupport::Concern

  included do
    before_action :track_activity
  end

  def track_activity
    # Tracking code
  end
end

class ApplicationController < ActionController::Base
  include Trackable
end
```

## 8. **Simplify Associations with `delegate` 🗝️**

The `delegate` method can streamline your code by delegating method calls to associated objects.

```ruby
class User < ApplicationRecord
  has_one :profile
  delegate :bio, to: :profile
end

# Usage
user.bio
```

## 9. **Use `after_commit` for Post-Transaction Logic 🏷️**

The `after_commit` callback ensures that code runs only after a transaction has been successfully committed, which is useful for background jobs or external notifications.

```ruby
class Order < ApplicationRecord
  after_commit :notify_customer

  def notify_customer
    # Code to send notification
  end
end
```

## 10. **Batch Processing with `find_in_batches` 🚀**

Similar to `find_each`, `find_in_batches` processes records in batches but gives you more control over batch processing.

```ruby
User.find_in_batches(batch_size: 1000) do |batch|
  batch.each do |user|
    # Process each user
  end
end
```

## 11. **Use `ActiveSupport::Notifications` for Custom Event Handling 📣**

Rails provides a powerful mechanism for subscribing to and broadcasting events using `ActiveSupport::Notifications`. This can be useful for logging, monitoring, or custom event handling.

```ruby
ActiveSupport::Notifications.subscribe('user.created') do |name, start, finish, id, payload|
  puts "User created: #{payload[:user].inspect}"
end

# Triggering the event
ActiveSupport::Notifications.instrument('user.created', user: @user)
```

## 12. **Use `fetch_or_initialize_by` for Safe Object Creation 🛠️**

`fetch_or_initialize_by` allows you to find or initialize an object based on given attributes, ensuring it’s not unnecessarily saved until you explicitly call `save`.

```ruby
user = User.fetch_or_initialize_by(email: 'example@example.com') do |user|
  user.name = 'New User'
end
```

## 13. **Optimize Queries with `includes` for Eager Loading 📚**

When querying associated records, use `includes` to avoid the N+1 query problem by eager loading associations.

```ruby
# Avoid N+1 queries
posts = Post.includes(:comments).where(published: true)
posts.each do |post|
  post.comments.each do |comment|
    # Process comment
  end
end
```

## Conclusion

Ruby on Rails is packed with powerful features and tricks that can significantly enhance your development process. By applying these unique hacks and techniques, you can write more efficient, maintainable, and clean code. Keep exploring Rails to discover even more ways to streamline your workflow and improve your applications. Happy coding! 🎉
