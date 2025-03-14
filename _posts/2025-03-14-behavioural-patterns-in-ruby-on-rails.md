---
layout: home
title: "Behavioural Patterns in Ruby on Rails"
date: 2025-03-14
categories: "Ruby On Rails"
tags: [Ruby On Rails, Behavioural Patterns, Design Patterns, Software Enginnering, Code Better]
image: 'https://github.com/user-attachments/assets/d8931841-b5df-4aee-8ace-8ec56bafd1f4'
---

# 🧠 **Master Behavioural Patterns in Ruby on Rails: Write Smarter, Cleaner Code!** 🚀

Behavioural design patterns are all about **how objects interact and communicate** with each other. They help you manage complex workflows, decouple components, and make your code more flexible and maintainable. In this blog, we’ll explore **key behavioural patterns** and show you how to use them effectively in Ruby on Rails. Let’s dive in! 🏊‍♂️

![Behavioral-dp-banner](https://github.com/user-attachments/assets/d8931841-b5df-4aee-8ace-8ec56bafd1f4)

---

## **What Are Behavioural Patterns?**
Behavioural patterns focus on improving communication between objects, making your code more modular and easier to extend. They are especially useful in Rails for managing business logic, handling events, and simplifying complex workflows.

Let’s explore the most important behavioural patterns and how to use them in Rails:

---

## **1. Observer Pattern: The Event Listener 🎧**

### **What is the Observer Pattern?**
The Observer Pattern allows an object (the subject) to notify a list of dependents (observers) about state changes. It’s perfect for decoupling components and handling events.

### **Why Use It in Rails?**
- Keeps your models clean by moving event-handling logic to observers.
- Makes it easy to add or remove observers without modifying the subject.
- Great for sending notifications, logging, or updating related objects.

### **Example: Notifying Users of a New Comment**
Imagine you want to notify users when a new comment is added to a post. Use the Observer Pattern to decouple the notification logic from the `Comment` model.

```ruby
class Comment
  attr_reader :post, :user, :content

  def initialize(post, user, content)
    @post = post
    @user = user
    @content = content
    notify_observers
  end

  private

  def notify_observers
    post.notify_observers(self)
  end
end

class Post
  attr_reader :observers

  def initialize
    @observers = []
  end

  def add_observer(observer)
    observers << observer
  end

  def notify_observers(comment)
    observers.each { |observer| observer.update(comment) }
  end
end

class NotificationObserver
  def update(comment)
    puts "New comment by #{comment.user}: #{comment.content}"
  end
end

# Usage
post = Post.new
post.add_observer(NotificationObserver.new)
Comment.new(post, "Alice", "Great post!")
```

### **Pro Tips 💡**
- Use Rails’ built-in `ActiveSupport::Notifications` for lightweight event handling.
- Combine with background jobs (e.g., Sidekiq) for asynchronous notifications.
- Avoid overusing observers; keep them focused on a single responsibility.

---

## **2. Strategy Pattern: The Flexible Algorithm 🎯**

### **What is the Strategy Pattern?**
The Strategy Pattern allows you to define a family of algorithms, encapsulate each one, and make them interchangeable. It’s ideal for scenarios where you need to switch between different behaviours at runtime.

### **Why Use It in Rails?**
- Makes your code more flexible and extensible.
- Simplifies testing by isolating algorithms.
- Reduces conditional logic in your classes.

### **Example: Payment Processing**
Imagine you have a payment system that supports multiple payment methods (credit card, PayPal, etc.). Use the Strategy Pattern to encapsulate each payment method.

```ruby
class Payment
  attr_reader :strategy

  def initialize(strategy)
    @strategy = strategy
  end

  def process(amount)
    strategy.process(amount)
  end
end

class CreditCardPayment
  def process(amount)
    puts "Processing credit card payment of $#{amount}"
  end
end

class PayPalPayment
  def process(amount)
    puts "Processing PayPal payment of $#{amount}"
  end
end

# Usage
payment = Payment.new(CreditCardPayment.new)
payment.process(100)

payment = Payment.new(PayPalPayment.new)
payment.process(50)
```

### **Pro Tips 💡**
- Use dependency injection to pass strategies into your classes.
- Combine with Rails’ `ActiveSupport::Configurable` for dynamic configuration.
- Use strategies to handle feature toggles or A/B testing.

---

## **3. Command Pattern: The Action Encapsulator 📦**

### **What is the Command Pattern?**
The Command Pattern encapsulates a request as an object, allowing you to parameterize clients with different requests, queue or log requests, and support undoable operations.

### **Why Use It in Rails?**
- Decouples the sender of a request from its executor.
- Simplifies adding new commands without modifying existing code.
- Great for implementing undo/redo functionality or queuing tasks.

### **Example: Undoable Actions**
Imagine you want to implement an undo feature for a text editor. Use the Command Pattern to encapsulate each action.

```ruby
class TextEditor
  attr_accessor :text

  def initialize
    @text = ""
  end
end

class Command
  attr_reader :editor, :backup

  def initialize(editor)
    @editor = editor
  end

  def execute
    raise NotImplementedError
  end

  def undo
    editor.text = backup
  end

  def save_backup
    @backup = editor.text.dup
  end
end

class AddTextCommand < Command
  def initialize(editor, text)
    super(editor)
    @text = text
  end

  def execute
    save_backup
    editor.text += @text
  end
end

# Usage
editor = TextEditor.new
command = AddTextCommand.new(editor, "Hello, world!")
command.execute
puts editor.text # => "Hello, world!"

command.undo
puts editor.text # => ""
```

### **Pro Tips 💡**
- Use commands to implement background jobs or task queues.
- Combine with the **Composite Pattern** to create macro commands.
- Use commands to implement transactional operations.

---

## **4. Chain of Responsibility: The Responsibility Delegator ⛓️**

### **What is the Chain of Responsibility?**
The Chain of Responsibility Pattern allows you to pass a request along a chain of handlers. Each handler either processes the request or passes it to the next handler in the chain.

### **Why Use It in Rails?**
- Decouples the sender of a request from its handlers.
- Simplifies adding or removing handlers.
- Great for middleware, logging, or validation pipelines.

### **Example: Request Validation**
Imagine you want to validate a user registration request. Use the Chain of Responsibility to handle each validation step.

```ruby
class Handler
  attr_accessor :next_handler

  def handle(request)
    if can_handle?(request)
      process(request)
    elsif next_handler
      next_handler.handle(request)
    else
      puts "Request cannot be handled."
    end
  end

  def can_handle?(request)
    raise NotImplementedError
  end

  def process(request)
    raise NotImplementedError
  end
end

class EmailValidator < Handler
  def can_handle?(request)
    request[:email].include?("@")
  end

  def process(request)
    puts "Email is valid."
  end
end

class PasswordValidator < Handler
  def can_handle?(request)
    request[:password].length >= 8
  end

  def process(request)
    puts "Password is valid."
  end
end

# Usage
email_validator = EmailValidator.new
password_validator = PasswordValidator.new
email_validator.next_handler = password_validator

request = { email: "test@example.com", password: "password123" }
email_validator.handle(request)
```

### **Pro Tips 💡**
- Use the Chain of Responsibility for middleware or request processing.
- Combine with Rails’ `ActiveSupport::Callbacks` for reusable hooks.
- Avoid creating long chains; keep them focused and manageable.

---

## **Excited Tips to Improve Code Efficiency and Readability 🌟**
1. **Keep It DRY**: Use patterns to avoid duplicating logic.
2. **Use Meaningful Names**: Name your observers, strategies, and commands descriptively.
3. **Leverage Rails Conventions**: Use Rails’ built-in tools to simplify pattern implementation.
4. **Test Thoroughly**: Write unit tests for your patterns to ensure they behave as expected.
5. **Document Your Patterns**: Add comments or READMEs to explain how and why you’re using these patterns.

---

## **Conclusion: Write Smarter with Behavioural Patterns 🧠**

Behavioural patterns like **Observer**, **Strategy**, **Command**, and **Chain of Responsibility** are powerful tools in your Rails toolkit. They help you manage complex workflows, decouple components, and make your code more flexible and maintainable. Start using these patterns today, and watch your Rails applications soar to new heights! 🚀

---

**What’s Next?**  
Stay tuned for our next blog in the series, where we’ll dive into **Structural Patterns** like Decorator and Adapter! 🎉

---

**Let’s Connect!**  
If you found this blog helpful, share it with your fellow developers and leave a comment below. What’s your favourite behavioural pattern? Let’s discuss! 💬

Happy Coding! 💻✨
