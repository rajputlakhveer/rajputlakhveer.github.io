---
layout: home
title: "Standardized Code"
date: 2024-09-23
categories: "Code Better"
tags: [Standardized Code, SOLID, SLAP, SRP, DRY]
image: 'https://github.com/user-attachments/assets/fa2182b4-54e0-4ca6-9062-d0322fb849c9'
---

### All Basic Principles for Standardized Code: Code Like a Pro with Examples 🚀

In the world of software development, writing clean, standardized, and professional code isn't just an ideal—it's essential. Following coding principles not only improves readability but also makes debugging easier, ensures maintainability, and promotes team collaboration. In this blog, we'll explore the key principles every developer should follow to write code like a pro! 💻

![1](https://github.com/user-attachments/assets/fa2182b4-54e0-4ca6-9062-d0322fb849c9)

---

### 1. **DRY (Don’t Repeat Yourself)** 📝

**Principle**: Avoid code duplication. Every piece of knowledge should have a single, unambiguous representation within a system.

**Example**:
Instead of repeating similar blocks of code, extract them into a function:

```ruby
# Bad: Repeated logic
def calculate_discount(price)
  discount = price * 0.1
  price - discount
end

def apply_discount_to_cart(cart_items)
  cart_items.each do |item|
    discount = item.price * 0.1
    item.price -= discount
  end
end

# Good: DRY principle
def apply_discount(price)
  discount = price * 0.1
  price - discount
end

def apply_discount_to_cart(cart_items)
  cart_items.each do |item|
    item.price = apply_discount(item.price)
  end
end
```

This makes the code easier to maintain, as changes to the discount logic only need to be updated in one place.

---

### 2. **KISS (Keep It Simple, Stupid)** 🤓

**Principle**: Simplicity is key. Write code that is as simple as possible for others to understand.

**Example**:
Instead of writing complex logic, break it down into smaller, more digestible parts:

```ruby
# Bad: Complex and hard-to-read logic
def calculate_total(order)
  total = order.items.map { |item| item.price * item.quantity }.sum
  total += order.shipping_cost if order.eligible_for_shipping?
  total - (order.has_discount? ? order.discount_amount : 0)
end

# Good: Simple and readable logic
def calculate_item_total(order)
  order.items.map { |item| item.price * item.quantity }.sum
end

def apply_shipping_cost(total, order)
  total + (order.eligible_for_shipping? ? order.shipping_cost : 0)
end

def apply_discount(total, order)
  total - (order.has_discount? ? order.discount_amount : 0)
end

def calculate_total(order)
  total = calculate_item_total(order)
  total = apply_shipping_cost(total, order)
  apply_discount(total, order)
end
```

Breaking the function down into smaller pieces enhances readability and reduces the complexity.

---

### 3. **YAGNI (You Aren’t Gonna Need It)** ❌

**Principle**: Don’t write code for features that are not currently needed. Focus on what is required now.

**Example**:
```ruby
# Bad: Overcomplicating for features not needed
def create_order(items, shipping_address, billing_address, apply_membership_discount, include_gift_wrapping)
  # some logic for membership discounts and gift wrapping (not yet needed)
end

# Good: Keep it simple for now
def create_order(items, shipping_address, billing_address)
  # only the necessary logic
end
```

Avoid adding extra parameters or logic for features that aren’t being used yet.

---

### 4. **SOLID Principles** 🧱

The SOLID principles provide a foundation for writing scalable and maintainable code. Here’s a brief summary of each:

#### **S**: **Single Responsibility Principle (SRP)**
Each class or function should only have one job.

**Example**:
```ruby
# Bad: Multiple responsibilities in one class
class Order
  def calculate_total
    # logic to calculate total
  end

  def print_invoice
    # logic to print invoice
  end
end

# Good: Separation of responsibilities
class Order
  def calculate_total
    # logic to calculate total
  end
end

class InvoicePrinter
  def print(order)
    # logic to print invoice
  end
end
```

#### **O**: **Open/Closed Principle (OCP)**
Classes should be open for extension but closed for modification.

**Example**:
```ruby
# Bad: Modifying the original class to add new functionality
class ShippingCalculator
  def calculate_cost(order)
    if order.expedited?
      # logic for expedited shipping
    else
      # logic for standard shipping
    end
  end
end

# Good: Extending functionality through inheritance or modules
class ShippingCalculator
  def calculate_cost(order)
    # default shipping logic
  end
end

class ExpeditedShippingCalculator < ShippingCalculator
  def calculate_cost(order)
    # expedited shipping logic
  end
end
```

#### **L**: **Liskov Substitution Principle (LSP)**
Objects of a subclass should be replaceable with objects of a superclass without affecting the correctness of the program.

#### **I**: **Interface Segregation Principle (ISP)**
Clients should not be forced to depend on methods they do not use.

#### **D**: **Dependency Inversion Principle (DIP)**
High-level modules should not depend on low-level modules. Both should depend on abstractions.

---

### 5. **Separation of Concerns (SoC)** 🎯

**Principle**: Divide the code into distinct sections, where each section addresses a separate concern.

**Example**:
```ruby
# Bad: Database access and business logic mixed together
class Product
  def self.fetch_available_products
    Product.where(available: true)
  end

  def calculate_discounted_price
    # logic to calculate discounted price
  end
end

# Good: Separate database logic and business logic
class Product
  def calculate_discounted_price
    # logic to calculate discounted price
  end
end

class ProductRepository
  def self.fetch_available_products
    Product.where(available: true)
  end
end
```

Here, the database logic is moved to a separate class, making the code more modular.

---

### 6. **Write Meaningful Names** 🔠

**Principle**: Always use meaningful variable and method names that clearly describe their purpose.

**Example**:
```ruby
# Bad: Unclear variable names
def calculate(a, b)
  c = a * b
  c
end

# Good: Descriptive variable names
def calculate_area(length, width)
  area = length * width
  area
end
```

Well-named variables and methods make your code self-explanatory.

---

### 7. **Test-Driven Development (TDD)** 🧪

**Principle**: Write tests before the actual code to ensure all code is functional and covered by tests.

**Example**:
```ruby
# Test (written first)
def test_calculate_discount
  product = Product.new(price: 100)
  assert_equal 90, product.calculate_discount(10)
end

# Code
class Product
  def calculate_discount(percent)
    price - (price * percent / 100)
  end
end
```

TDD helps in building robust and bug-free code.

---

### Conclusion 🎉

Mastering these basic principles will significantly improve the quality of your code, making it clean, maintainable, and professional. Whether you're a beginner or a seasoned pro, applying principles like DRY, KISS, and SOLID will ensure that you’re writing code that’s easy to understand and work with. By following these guidelines, you'll not only level up your coding skills but also make a positive impact on your team and projects.

Happy Coding! 😎💻
