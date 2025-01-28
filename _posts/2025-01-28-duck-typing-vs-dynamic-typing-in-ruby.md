---
layout: home
title: "Duck Typing vs Dynamic Typing in Ruby"
date: 2025-01-28
categories: "Ruby On Rails"
tags: [Duck Typing, Dynamic Typing, Ruby, Ruby On Rails]
image: 'https://github.com/user-attachments/assets/94e5f88c-285d-49b9-8c35-2ca3e2c03149'
---

# 🦆 Duck Typing vs Dynamic Typing in Ruby: Simplifying the Concepts! 🚀

Ruby is a dynamic, flexible, and expressive language, making it a favorite among developers. Two fascinating concepts in Ruby—**Duck Typing** and **Dynamic Typing**—often create confusion, but don't worry! This blog will clarify everything with examples and relatable analogies. Let's dive in! 🌊

![1_6ynorfsEdWBF_zP6YHSyiw](https://github.com/user-attachments/assets/94e5f88c-285d-49b9-8c35-2ca3e2c03149)

---

## 🌟 What is Dynamic Typing?  
Dynamic Typing means **variables do not have a fixed type**. Instead, the type is determined at runtime based on the value assigned to the variable. In simpler terms, Ruby doesn't care about what type of object a variable holds, as long as it can perform the required actions.

### 🛠 Example of Dynamic Typing:
```ruby
x = 10        # x is now an Integer
x = "Hello"   # x is now a String
x = [1, 2, 3] # x is now an Array
```

Here, `x` changes its type from an Integer to a String and then to an Array. No complaints from Ruby—it's happy as long as the operations are valid! 🎉

### 🔥 Advantages of Dynamic Typing:
1. **Flexibility**: You can assign any type to a variable without restrictions.
2. **Conciseness**: No need to declare types explicitly.

### ⚠️ Caution:  
Dynamic Typing can lead to **runtime errors**. For example:
```ruby
x = "Ruby"
puts x + 10 # Error: TypeError (String can't be coerced into Integer)
```

Ruby assumes `x + 10` should work only if `x` is numeric. Hence, runtime checks are crucial.

---

## 🦆 What is Duck Typing?  
The term comes from the phrase:  
*"If it looks like a duck, swims like a duck, and quacks like a duck, then it probably is a duck."*

In Ruby, **Duck Typing** is about an object's behavior rather than its class. If an object responds to a method or action, Ruby doesn't care about its type. If it quacks like a duck, it's treated like a duck! 🦆

### 🛠 Example of Duck Typing:
Imagine a scenario where we have different objects with a `speak` method:

```ruby
class Dog
  def speak
    "Woof! 🐶"
  end
end

class Cat
  def speak
    "Meow! 🐱"
  end
end

class Duck
  def speak
    "Quack! 🦆"
  end
end

def animal_sound(animal)
  puts animal.speak
end

animal_sound(Dog.new) # Output: Woof! 🐶
animal_sound(Cat.new) # Output: Meow! 🐱
animal_sound(Duck.new) # Output: Quack! 🦆
```

Ruby doesn't care if the object is a Dog, Cat, or Duck. As long as it implements the `speak` method, it's valid. 💡

---

## 🤔 Dynamic Typing vs Duck Typing: Key Differences

| Feature           | Dynamic Typing                                    | Duck Typing                                    |
|--------------------|--------------------------------------------------|-----------------------------------------------|
| **Definition**     | Determines variable type at runtime.             | Focuses on object behavior, not type.         |
| **Concern**        | Data type of the variable.                       | Method or behavior an object implements.      |
| **Validation**     | Performed during runtime.                        | Relies on the presence of required methods.   |
| **Error Handling** | Errors can occur when incompatible operations.   | Errors occur if the required behavior is missing. |

---

## 💡 When to Use Duck Typing?  

Duck Typing is useful in **polymorphic behavior** where multiple objects of different classes share common functionality. Examples include:
- Designing APIs.
- Flexible implementations of iterators, parsers, or renderers.

### 🔥 Example: A Payment System
```ruby
class PayPal
  def pay(amount)
    "Paid #{amount} using PayPal. 💸"
  end
end

class CreditCard
  def pay(amount)
    "Paid #{amount} using Credit Card. 💳"
  end
end

def process_payment(payment_method, amount)
  puts payment_method.pay(amount)
end

process_payment(PayPal.new, 100)       # Output: Paid 100 using PayPal. 💸
process_payment(CreditCard.new, 200)  # Output: Paid 200 using Credit Card. 💳
```

As long as the object implements the `pay` method, it's a valid payment method.

---

## 🚩 Pitfalls to Watch For  
1. **Duck Typing Errors**:  
   If an object doesn't implement the required method, you get a `NoMethodError`. Use `.respond_to?(:method_name)` to check.

   ```ruby
   if obj.respond_to?(:speak)
     obj.speak
   else
     puts "This object can't speak! ❌"
   end
   ```

2. **Dynamic Typing Bugs**:  
   Since types aren't enforced, runtime errors are common. Writing **unit tests** is essential in Ruby projects.

---

## 🌈 Conclusion  
Both **Dynamic Typing** and **Duck Typing** are pillars of Ruby's elegance and flexibility. While Dynamic Typing offers freedom with variables, Duck Typing encourages focusing on object behavior rather than class hierarchies. Mastering these concepts will help you write cleaner and more efficient Ruby code! 🚀

Let me know in the comments if you found this helpful! ✨ Happy coding! 💻
