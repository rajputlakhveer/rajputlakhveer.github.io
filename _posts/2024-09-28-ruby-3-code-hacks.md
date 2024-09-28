---
layout: home
title: "Ruby 3 Code Hacks"
date: 2024-09-11
categories: "Ruby on Rails"
tags: [Ruby, Hacks, Ruby on Rails, Tricks]
image: 'https://github.com/user-attachments/assets/c7ad2739-a766-46d9-90a1-5efc8e826c7d'
---

### 10 Unique Ruby 3 Code Hacks Every Programmer Should Know 🚀

Ruby 3 introduces many exciting features and optimizations that make coding more expressive and efficient. Here are 10 unique code hacks in Ruby 3 that every programmer should know, complete with example usage! 🧑‍💻

![ruby-3-features](https://github.com/user-attachments/assets/c7ad2739-a766-46d9-90a1-5efc8e826c7d)

---

#### 1. **Pattern Matching 🔍**

Pattern matching allows you to deconstruct and match complex data structures like arrays, hashes, and even custom objects with ease. It's an excellent alternative to verbose case-when conditions.

```ruby
case { name: "Alice", age: 30 }
in { name: "Alice", age: age }
  puts "Age is #{age}"  # Age is 30
else
  puts "No match"
end
```

This makes working with data structures more intuitive and concise. 🌟

---

#### 2. **Endless Methods 🛠️**

Endless methods let you define simple methods in a single line without explicitly ending them with `end`. Great for small methods!

```ruby
def greet(name) = "Hello, #{name}!"
puts greet("Ruby")  # Hello, Ruby!
```

A nice way to shorten methods and reduce boilerplate for trivial functionalities! ✨

---

#### 3. **Find Pattern for Arrays 🗂️**

You can deconstruct arrays right in `in` clauses using the `find` pattern. It's perfect for pattern matching complex array structures.

```ruby
array = [1, 2, [3, 4]]

case array
in [1, 2, [a, b]]
  puts "Inner values: #{a}, #{b}"  # Inner values: 3, 4
end
```

This simplifies working with nested arrays! 📦

---

#### 4. **Numbered Block Parameters ➡️**

You can now avoid naming block parameters when the argument is unused or irrelevant, using numbered block parameters like `_1`, `_2`, etc.

```ruby
[1, 2, 3].map { _1 * 2 }  # [2, 4, 6]
```

A neat trick for keeping your code clean and removing unnecessary variable names. 👌

---

#### 5. **One-line Hash to Keyword Arguments Conversion 💡**

In Ruby 3, hashes can directly be passed as keyword arguments without needing the double-splat `**` operator.

```ruby
def info(name:, age:)
  puts "Name: #{name}, Age: #{age}"
end

person = { name: "Bob", age: 25 }
info(person)  # Name: Bob, Age: 25
```

No more extra steps for passing keyword arguments from hashes! 🎯

---

#### 6. **Rightward Assignment Operator ⬅️**

Rightward assignment (`=>`) lets you assign values in a reverse fashion, which is especially useful when using pattern matching.

```ruby
{ a: 1, b: 2 } => { a: x, b: y }
puts x  # 1
puts y  # 2
```

This can clean up your code when working with destructuring and pattern matching! 🧩

---

#### 7. **Enumerable#tally for Frequency Counting 📊**

Want to count the frequency of elements in an array? Ruby 3 introduces `Enumerable#tally`, which simplifies this common task.

```ruby
words = ["apple", "banana", "apple"]
frequency = words.tally
puts frequency  # {"apple" => 2, "banana" => 1}
```

Perfect for quick frequency counting, eliminating the need for manual counting methods! 🔢

---

#### 8. **RBS for Type Checking 📝**

Ruby 3 introduces the Ruby Signature Language (RBS) for static type checking, ensuring type safety in large codebases.

```rbs
class Person
  attr_reader name: String, age: Integer
end
```

RBS files allow you to define types outside of the core Ruby code, making it easier to maintain and validate. 🎯

---

#### 9. **Hash#except: Exclude Keys from Hashes 🚪**

Ruby 3 has added the `Hash#except` method, making it easy to exclude certain keys from a hash without manually modifying the hash.

```ruby
person = { name: "John", age: 25, city: "New York" }
person.except(:age)  # { name: "John", city: "New York" }
```

Great for cleaning up hash data quickly and elegantly. 🧹

---

#### 10. **Improved Performance via JIT Compiler ⚡**

The Just-In-Time (JIT) compiler has been improved in Ruby 3, making your Ruby code run faster without extra effort on your part!

```ruby
def calc
  1000.times do
    Math.sqrt(12345)
  end
end
calc
```

While not a visible "hack," using Ruby 3’s performance boosts out of the box means smoother execution for all your code. 🚀

---

### Conclusion 🎉

Ruby 3 packs a punch with these exciting new code hacks. From pattern matching to type checking, these features not only make your code more concise but also boost its performance and readability. Try them out and let your Ruby code shine! 💎
