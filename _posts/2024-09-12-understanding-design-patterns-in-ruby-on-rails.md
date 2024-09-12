---
layout: home
title: "Understanding Design Patterns in Ruby on Rails"
date: 2024-09-12
categories: "Ruby on Rails"
tags: [Ruby, Design Patterns, Ruby on Rails, Development]
image: '/assets/images/rails.jpg'
---

## Understanding Design Patterns in Ruby on Rails: A Practical Guide with Examples 🚀

Design patterns are reusable solutions to common problems in software design. They provide a way to structure your code effectively and efficiently. In Ruby on Rails, utilizing these patterns can lead to cleaner, more maintainable code. Let's explore some of the most commonly used design patterns and see how you can implement them in your Rails applications. 🛠️

### 1. Singleton Pattern 🦄

![Description of the image]({{ '/assets/images/singleton.png' | relative_url }})

**What is it?**  
The Singleton Pattern ensures that a class has only one instance and provides a global point of access to it.

**When to use?**  
Use it when you need exactly one instance of a class and you want to control access to it.

**Example in Rails:**  
Imagine you have a service class that manages configurations and you want to make sure there's only one instance of this service.

```ruby
class ConfigurationManager
  include Singleton

  def initialize
    @configurations = {}
  end

  def set(key, value)
    @configurations[key] = value
  end

  def get(key)
    @configurations[key]
  end
end

# Usage
config_manager = ConfigurationManager.instance
config_manager.set(:api_key, '12345')
puts config_manager.get(:api_key) # Output: 12345
```

### 2. Factory Method Pattern 🏭

![Description of the image]({{ '/assets/images/factory.png' | relative_url }})

**What is it?**  
The Factory Method Pattern defines an interface for creating objects but allows subclasses to alter the type of objects that will be created.

**When to use?**  
Use it when you need to create objects that are part of a family of related or dependent objects.

**Example in Rails:**  
Suppose you have different types of notifications, and you want a way to create them based on the type.

```ruby
class NotificationFactory
  def self.create(type)
    case type
    when :email
      EmailNotification.new
    when :sms
      SmsNotification.new
    else
      raise "Unknown notification type"
    end
  end
end

# Usage
notification = NotificationFactory.create(:email)
notification.send_message("Hello World!")
```

### 3. Observer Pattern 👀

![Description of the image]({{ '/assets/images/obsever.png' | relative_url }})

**What is it?**  
The Observer Pattern defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

**When to use?**  
Use it when you want to automatically notify and update other objects when one object’s state changes.

**Example in Rails:**  
In Rails, Active Record models have built-in observer support.

```ruby
class UserObserver < ActiveRecord::Observer
  def after_create(user)
    # Notify user about account creation
    UserMailer.welcome_email(user).deliver_now
  end
end

# In your config/application.rb
config.active_record.observers = :user_observer
```

### 4. Strategy Pattern 🎯
.
![Description of the image]({{ '/assets/images/strategy.png' | relative_url }})

**What is it?**  
The Strategy Pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. It allows the algorithm to vary independently from the clients that use it.

**When to use?**  
Use it when you have a family of algorithms and want to choose one at runtime.

**Example in Rails:**  
Consider a service that processes payments differently based on the payment method.

```ruby
class PaymentProcessor
  def initialize(strategy)
    @strategy = strategy
  end

  def process(payment)
    @strategy.process(payment)
  end
end

class CreditCardPayment
  def process(payment)
    # Process credit card payment
  end
end

class PaypalPayment
  def process(payment)
    # Process PayPal payment
  end
end

# Usage
processor = PaymentProcessor.new(CreditCardPayment.new)
processor.process(payment)
```

### 5. Decorator Pattern 🎨

**What is it?**  
The Decorator Pattern attaches additional responsibilities to an object dynamically. It provides a flexible alternative to subclassing for extending functionality.

**When to use?**  
Use it when you need to add responsibilities to objects dynamically without affecting other objects.

**Example in Rails:**  
Enhancing user notifications with additional features like logging or custom formatting.

```ruby
class NotificationDecorator
  def initialize(notification)
    @notification = notification
  end

  def send_message(message)
    log_message(message)
    @notification.send_message(message)
  end

  private

  def log_message(message)
    # Log message to a file or system
  end
end

# Usage
notification = NotificationDecorator.new(EmailNotification.new)
notification.send_message("Hello World!")
```

### 6. Adapter Pattern 🔄

**What is it?**  
The Adapter Pattern allows incompatible interfaces to work together. It acts as a bridge between two incompatible interfaces.

**When to use?**  
Use it when you need to integrate new functionality into an existing system without changing its code.

**Example in Rails:**  
Suppose you have an old notification system and a new one, and you want to adapt the new system to work with the old interface.

```ruby
# Old interface
class OldNotificationSystem
  def send_old_notification(message)
    # send notification
  end
end

# New interface
class NewNotificationSystem
  def send_new_notification(message)
    # send notification
  end
end

# Adapter
class NotificationAdapter
  def initialize(new_system)
    @new_system = new_system
  end

  def send_old_notification(message)
    @new_system.send_new_notification(message)
  end
end

# Usage
old_system = OldNotificationSystem.new
adapter = NotificationAdapter.new(NewNotificationSystem.new)
adapter.send_old_notification("Hello World!")
```

### 7. Command Pattern 📜

**What is it?**  
The Command Pattern encapsulates a request as an object, thereby allowing for parameterization of clients with queues, requests, and operations.

**When to use?**  
Use it when you need to issue requests to objects without knowing the operation being requested or the receiver of the request.

**Example in Rails:**  
Implementing a system where users can queue up and execute commands.

```ruby
class Command
  def execute
    raise NotImplementedError
  end
end

class LightOnCommand < Command
  def initialize(light)
    @light = light
  end

  def execute
    @light.turn_on
  end
end

class Light
  def turn_on
    puts "Light is on"
  end
end

# Invoker
class RemoteControl
  def initialize
    @command = nil
  end

  def set_command(command)
    @command = command
  end

  def press_button
    @command.execute
  end
end

# Usage
light = Light.new
light_on = LightOnCommand.new(light)
remote = RemoteControl.new
remote.set_command(light_on)
remote.press_button # Output: Light is on
```

### 8. Proxy Pattern 🔐

**What is it?**  
The Proxy Pattern provides a surrogate or placeholder for another object to control access to it.

**When to use?**  
Use it when you need to control access to an object, for instance, to add a layer of security or manage resource-intensive operations.

**Example in Rails:**  
Adding caching to a service that performs resource-intensive operations.

```ruby
class ExpensiveService
  def perform_operation
    # Perform a time-consuming operation
  end
end

class CachedServiceProxy
  def initialize
    @cache = {}
    @service = ExpensiveService.new
  end

  def perform_operation
    @cache[:result] ||= @service.perform_operation
  end
end

# Usage
proxy = CachedServiceProxy.new
proxy.perform_operation # Only calls the expensive operation once
```

### 9. Template Method Pattern 📋

**What is it?**  
The Template Method Pattern defines the skeleton of an algorithm in a base class but lets subclasses override specific steps of the algorithm without changing its structure.

**When to use?**  
Use it when you have a fixed algorithm but want to allow for variations in certain steps.

**Example in Rails:**  
Creating a generic report generator with specific steps that can be customized.

```ruby
class ReportGenerator
  def generate_report
    setup
    fetch_data
    format_data
    output_report
  end

  def setup
    # Setup report generation
  end

  def fetch_data
    raise NotImplementedError
  end

  def format_data
    raise NotImplementedError
  end

  def output_report
    raise NotImplementedError
  end
end

class SalesReport < ReportGenerator
  def fetch_data
    # Fetch sales data
  end

  def format_data
    # Format sales data
  end

  def output_report
    # Output sales report
  end
end

# Usage
report = SalesReport.new
report.generate_report
```

### 10. Mediator Pattern 🕊️

**What is it?**  
The Mediator Pattern defines an object that encapsulates how a set of objects interact. It promotes loose coupling by keeping objects from referring to each other explicitly.

**When to use?**  
Use it when you want to reduce dependencies between communicating objects.

**Example in Rails:**  
Managing interactions between different components of a form.

```ruby
class FormMediator
  def initialize(form)
    @form = form
    @form.on_input_change { validate_form }
  end

  def validate_form
    if @form.input_valid?
      @form.enable_submit_button
    else
      @form.disable_submit_button
    end
  end
end

class Form
  def on_input_change(&block)
    @input_change_callback = block
  end

  def input_valid?
    # Validate form input
  end

  def enable_submit_button
    # Enable submit button
  end

  def disable_submit_button
    # Disable submit button
  end

  def input_changed
    @input_change_callback.call
  end
end

# Usage
form = Form.new
mediator = FormMediator.new(form)
form.input_changed # Mediator will validate form and enable/disable submit button accordingly
```

### Conclusion 🌟

Design patterns are like blueprints for solving common design problems. By implementing these patterns in Ruby on Rails, you can create more scalable, maintainable, and efficient code. Experiment with these patterns to find the best solutions for your specific needs, and don’t hesitate to mix and match them to suit your project's requirements.

Feel free to dive deeper into each pattern and let me know if you need more details or have other patterns in mind! 💡
