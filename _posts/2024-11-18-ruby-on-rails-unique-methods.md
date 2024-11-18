---
layout: home
title: "Ruby On Rails Unique Methods"
date: 2024-11-18
categories: "Ruby On Rails"
tags: [Ruby On Rails, ROR, Unique, Tips, Methods]
image: 'https://github.com/user-attachments/assets/e3c9ed94-2d22-4df8-8634-b537d595e687'
---

# 🚀 **25 Unique Ruby on Rails Methods You Need to Know!** ✨

Ruby on Rails is renowned for its elegance and simplicity, providing developers with powerful tools and methods to build applications quickly and efficiently. While some methods are commonly known, Rails also packs a bunch of unique and lesser-known gems that can save you hours of work! Let’s dive into **25 unique Ruby on Rails methods** every developer should know, complete with explanations and examples. 🛠️

![test](https://github.com/user-attachments/assets/e3c9ed94-2d22-4df8-8634-b537d595e687)

---

## 1. **`present?` and `blank?`** 🤔  
Determine if an object is present or empty. These are much smarter than `nil?` and work for strings, arrays, hashes, and more.  
```ruby
name = "Ruby on Rails"
puts name.present? # true
puts "".blank?     # true
```

---

## 2. **`squish`** 🧹  
Removes all leading, trailing, and excessive inner whitespace from a string. Perfect for cleaning up messy input!  
```ruby
messy_string = "  Too    much   space!  "
puts messy_string.squish # "Too much space!"
```

---

## 3. **`pluck`** 🌱  
Fetch specific fields directly from the database without loading full objects. It’s a game-changer for performance optimization.  
```ruby
User.pluck(:name) # ["Alice", "Bob", "Charlie"]
```

---

## 4. **`in_groups_of`** 📦  
Split an array into groups of a specified size. Handy for pagination or displaying items in a grid.  
```ruby
[1, 2, 3, 4, 5, 6].in_groups_of(2) # [[1, 2], [3, 4], [5, 6]]
```

---

## 5. **`where.not`** 🚫  
Exclude records from a query. Simplifies writing complex queries!  
```ruby
User.where.not(admin: true) # All non-admin users
```

---

## 6. **`try`** 🛡️  
Call a method on an object if it exists, avoiding `NoMethodError`.  
```ruby
user = nil
puts user.try(:name) # nil (no error!)
```

---

## 7. **`deep_merge`** 🔗  
Merge two hashes recursively. Perfect for configurations or nested data structures.  
```ruby
hash1 = { a: { b: 1 } }
hash2 = { a: { c: 2 } }
puts hash1.deep_merge(hash2) # { a: { b: 1, c: 2 } }
```

---

## 8. **`time_ago_in_words`** ⏳  
Convert a timestamp into a human-readable relative time. Great for activity feeds!  
```ruby
time = 5.minutes.ago
puts time_ago_in_words(time) # "5 minutes ago"
```

---

## 9. **`compact`** 🗑️  
Remove `nil` values from an array or hash effortlessly.  
```ruby
[1, nil, 2, nil, 3].compact # [1, 2, 3]
```

---

## 10. **`enum`** 🗂️  
Define enumerable values for a field. It’s cleaner and more semantic than using plain integers.  
```ruby
class Post < ApplicationRecord
  enum status: { draft: 0, published: 1 }
end

Post.published # All published posts
```

---

## 11. **`to_sentence`** 🖊️  
Format an array into a human-readable sentence.  
```ruby
["Alice", "Bob", "Charlie"].to_sentence # "Alice, Bob, and Charlie"
```

---

## 12. **`find_each`** 🔄  
Efficiently iterate over large ActiveRecord collections in batches to avoid memory issues.  
```ruby
User.find_each(batch_size: 100) { |user| puts user.name }
```

---

## 13. **`before_action`** 🎯  
Execute specific logic before a controller action. A Rails classic!  
```ruby
class UsersController < ApplicationController
  before_action :set_user, only: [:show, :edit]
  
  def set_user
    @user = User.find(params[:id])
  end
end
```

---

## 14. **`safe_constantize`** 🛡️  
Convert a string to a constant safely.  
```ruby
"User".safe_constantize # User
"InvalidClass".safe_constantize # nil (no error!)
```

---

## 15. **`as_json`** 📊  
Customize how objects are converted to JSON.  
```ruby
class User < ApplicationRecord
  def as_json(options = {})
    super(only: [:id, :name])
  end
end
```

---

## 16. **`content_tag`** 🏷️  
Dynamically create HTML tags in your views.  
```ruby
content_tag(:div, "Hello World!", class: "greeting") 
# <div class="greeting">Hello World!</div>
```

---

## 17. **`reverse_merge`** 🔄  
Merge a hash with defaults only if keys are missing.  
```ruby
options = { a: 1 }.reverse_merge(b: 2) # { a: 1, b: 2 }
```

---

## 18. **`find_or_create_by`** ✨  
Find or create a record in one go.  
```ruby
User.find_or_create_by(email: "test@example.com")
```

---

## 19. **`truncate`** ✂️  
Truncate a string to a specified length, perfect for summaries or previews.  
```ruby
"Ruby on Rails is amazing!".truncate(10) # "Ruby on..."
```

---

## 20. **`uniq`** 🌟  
Remove duplicate elements from an array.  
```ruby
[1, 2, 2, 3].uniq # [1, 2, 3]
```

---

## 21. **`alias_attribute`** 🪄  
Create a new name for an existing attribute.  
```ruby
class User < ApplicationRecord
  alias_attribute :full_name, :name
end
```

---

## 22. **`cache`** ⚡  
Use caching in views for performance.  
```erb
<% cache @user do %>
  <%= @user.name %>
<% end %>
```

---

## 23. **`helper_method`** 🛠️  
Make a controller method accessible in views.  
```ruby
class ApplicationController < ActionController::Base
  helper_method :current_user
  
  def current_user
    @current_user ||= User.find(session[:user_id])
  end
end
```

---

## 24. **`sanitize`** 🧼  
Clean input to prevent XSS attacks.  
```ruby
sanitize("<script>alert('hacked!')</script>") # ""
```

---

## 25. **`reload`** 🔄  
Reload an object from the database.  
```ruby
user = User.find(1)
user.reload # Refresh the user record
```

---

### 🏁 **Conclusion**  
These **25 unique methods** are just a glimpse of the power Rails puts at your fingertips. Mastering them will not only make you a faster developer but also a smarter one! 💡

So, which method are you adding to your toolkit today? Let us know in the comments! 🚀
