---
layout: home
title: "Ruby On Rails Hidden Methods"
date: 2024-10-29
categories: "Ruby On Rails"
tags: [Ruby On Rails, Ruby, Methods, Hidden, Unique, Tips]
image: 'https://github.com/user-attachments/assets/ebb7ed8e-2177-430e-a282-030b8d7662e9'
---

## 🚀 Unveiling Hidden Gems: Lesser-Known Ruby on Rails Methods! 💎

Ruby on Rails is packed with powerful methods that can streamline your coding and enhance your applications. While many developers are familiar with the common methods, some hidden gems often go unnoticed. In this blog, we’ll explore ten hidden methods that can elevate your Ruby on Rails experience, complete with example usage and pro tips. Let's dive in! 🌊

![devise-p1](https://github.com/user-attachments/assets/ebb7ed8e-2177-430e-a282-030b8d7662e9)

### 1. `cattr_accessor` and `mattr_accessor`
These methods create class-level accessors easily, allowing you to define class-wide variables without separate methods.

- **Usage**:
  ```ruby
  class Product
    cattr_accessor :discount_rate
  end

  Product.discount_rate = 0.15
  puts Product.discount_rate # => 0.15
  ```
  
- **Pro Tip**: Use `mattr_accessor` when you want to create mutable class-level variables that can be modified across different instances.

### 2. `composed_of`
This method maps a database column to a custom object, enabling complex data types in your models.

- **Usage**:
  ```ruby
  class User < ApplicationRecord
    composed_of :address, class_name: 'Address', mapping: [%w(address_line_1 line1), %w(address_line_2 line2)]
  end

  user = User.new(address: Address.new(line1: '123 Main St', line2: 'Apt 4B'))
  puts user.address.line1 # => '123 Main St'
  ```
  
- **Pro Tip**: Use `composed_of` when dealing with serialized data that requires specific formatting or manipulation.

### 3. `becomes`
`becomes` allows you to change the type of a model instance dynamically without altering the underlying record.

- **Usage**:
  ```ruby
  class User < ApplicationRecord; end
  class Admin < User; end

  user = User.find(1)
  admin = user.becomes(Admin)
  puts admin.class # => Admin
  ```
  
- **Pro Tip**: This method is useful for scenarios where you need to apply different behavior to the same underlying data, like roles in a system.

### 4. `pluck` with Joins
Using `pluck` with joins optimizes data retrieval by fetching specific columns from multiple tables.

- **Usage**:
  ```ruby
  User.joins(:posts).pluck('users.name, posts.title')
  # This returns an array of arrays with user names and post titles
  ```
  
- **Pro Tip**: Use `pluck` over `select` when you only need a subset of columns, as it directly returns the specified fields without building full ActiveRecord objects.

### 5. `with_options`
This method allows you to set multiple options or validations cleanly, reducing repetition.

- **Usage**:
  ```ruby
  class User < ApplicationRecord
    with_options presence: true do
      validates :name
      validates :email
      validates :password
    end
  end
  ```
  
- **Pro Tip**: This method can be helpful for grouping related validations together, enhancing readability and maintainability.

### 6. `scoping`
Rails offers a way to define temporary scopes for queries using `scoping`, which is particularly useful for complex queries.

- **Usage**:
  ```ruby
  User.scoping do
    # Your scoped queries here
    User.where(active: true).each do |user|
      puts user.name
    end
  end
  ```
  
- **Pro Tip**: Utilize `scoping` to create temporary states or configurations for queries that should not affect the global state.

### 7. `deep_dup`
The `deep_dup` method creates a deep copy of an ActiveRecord object, duplicating all its associations.

- **Usage**:
  ```ruby
  original_post = Post.find(1)
  duplicate_post = original_post.deep_dup
  duplicate_post.title = "Copy of #{original_post.title}"
  duplicate_post.save
  ```
  
- **Pro Tip**: Use `deep_dup` when you need to replicate objects along with their associations without risking data integrity by sharing references.

### 8. `delegate`
`delegate` allows you to delegate method calls to associated objects, making your code DRY (Don't Repeat Yourself).

- **Usage**:
  ```ruby
  class User < ApplicationRecord
    has_one :profile
    delegate :bio, to: :profile
  end

  user = User.find(1)
  puts user.bio # Calls user.profile.bio automatically
  ```
  
- **Pro Tip**: Use `delegate` to simplify your code and improve readability, especially when accessing attributes of associated objects frequently.

### 9. `attr_readonly`
This method makes an attribute read-only, preventing changes after the record is created.

- **Usage**:
  ```ruby
  class Product < ApplicationRecord
    attr_readonly :price
  end

  product = Product.new(price: 100)
  product.save
  product.price = 150 # This will not change the price
  ```
  
- **Pro Tip**: Use `attr_readonly` for fields that should remain constant, such as timestamps or unique identifiers.

### 10. `method_missing`
The `method_missing` method allows you to handle dynamic method calls that aren’t defined, providing flexibility in your code.

- **Usage**:
  ```ruby
  class DynamicFinder
    def method_missing(method, *args)
      if method.to_s.start_with?('find_by_')
        # Custom logic for dynamic finders
        puts "You called #{method} with arguments: #{args.inspect}"
      else
        super
      end
    end
  end

  finder = DynamicFinder.new
  finder.find_by_name("John") # Outputs: You called find_by_name with arguments: ["John"]
  ```
  
- **Pro Tip**: Use `method_missing` sparingly, as it can make your code harder to debug. It's best for cases where dynamic behavior is genuinely necessary.

### 🌟 Conclusion
These hidden methods can significantly enhance your Ruby on Rails applications, making your code cleaner, more efficient, and easier to maintain. By incorporating these methods into your toolkit, you can leverage the full power of Ruby on Rails. Happy coding! 🎉

Feel free to share your favorite hidden methods or experiences with these in the comments below! 😊✨
