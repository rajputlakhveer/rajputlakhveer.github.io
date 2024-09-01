---
layout: home
title: "Rails 7.2 Release: Everything You Need to Know – Major Features 🚀"
date: 2024-08-30
categories: "Ruby on Rails"
tags: [Rails 7.2, Ruby on Rails, Development]
---

The latest version of Rails is here - **Rails 7.2**! This release brings a host of new features and improvements that will make developing applications even more efficient and enjoyable. Let's explore the major updates, complete with examples to help you get started. 🎉

![Description of the image]({{ '/assets/images/rails.jpg' | relative_url }})

## Development Containers Configuration 🐳

### What It Is:
A development container allows you to set up your entire development environment using Docker. This means you can have a consistent setup across all your projects, making it easier to onboard new developers and manage dependencies.

### What's Included:
- **Redis Container:** Used for caching, Action Cable, and other Redis-dependent features.
- **Database:** You can choose between SQLite, Postgres, MySQL, or MariaDB.
- **Headless Chrome:** Perfect for running system tests without a graphical interface.
- **Active Storage:** Configured to use local disk storage with all preview features enabled.

### How to Use It:
- **Creating a New App:** Run the following command to generate a new Rails application with a development container:
  ```bash
  rails new myapp --devcontainer
  ```
- **Adding to an Existing App:** If you already have a Rails app, you can add a development container configuration by running:
  ```bash
  rails devcontainer
  ```
This will generate a `.devcontainer` folder with all the necessary configuration files like `Dockerfile`, `docker-compose.yml`, and `devcontainer.json`. You can now use Visual Studio Code or other tools that support dev containers to start coding in a fully configured environment. 💻

## Browser Version Guard 🛡️

### What It Is:
Rails 7.2 introduces a feature to specify which browser versions are allowed to access your application. This is useful for ensuring your app only runs on browsers that support the necessary features, thereby avoiding compatibility issues.

### Example:
Let's say you want to block outdated browsers like Internet Explorer and allow only modern versions of Safari, Firefox, and Chrome:
```ruby
class ApplicationController < ActionController::Base
  allow_browser versions: { safari: 16.4, firefox: 121, ie: false }
end
```

You can also add specific rules for individual actions within a controller:
```ruby
class MessagesController < ApplicationController
  allow_browser versions: { opera: 104, chrome: 119 }, only: :show
end
```

If a user tries to access your app with an unsupported browser, they'll see a `406 Not Acceptable` error, and the app will serve a friendly message asking them to update their browser.

## Ruby 3.1 is Now the Minimum Version 💎

### What It Is:
Rails 7.2 requires at least Ruby 3.1 to run. This ensures that your Rails application benefits from the latest Ruby features, security patches, and performance improvements.

### Why It Matters:
Dropping older Ruby versions allows Rails to stay lean and utilize modern Ruby features. It also encourages developers to keep their environments up to date, reducing potential security risks and improving application performance.

### How to Upgrade:
If you're still on an older version of Ruby, you'll need to upgrade to Ruby 3.1 before you can update to Rails 7.2. Here's how you can do it:

1. **Install Ruby 3.1:** Use a version manager like `rbenv` or `rvm` to install Ruby 3.1.
   ```bash
   rbenv install 3.1.0
   ```
2. **Update Your Gemfile:** Update your `Gemfile` to specify the new Ruby version.
   ```ruby
   ruby '3.1.0'
   ```
3. **Bundle Install:** Run `bundle install` to ensure all your gems are compatible with Ruby 3.1.

## Progressive Web Application (PWA) Support 📱

### What It Is:
Rails 7.2 prepares your application for Progressive Web Application (PWA) support by generating default files for the PWA manifest and service worker. These files allow your app to behave like a native mobile app, providing features like offline access, push notifications, and a home screen icon.

### Example:
When you generate a new Rails app, it will automatically include the following files:
- **Manifest:** Defines your app's name, icons, theme colors, and other metadata.
- **Service Worker:** Handles caching and offline access for your app.

These files are served from `app/views/pwa` and can be customized using ERB to dynamically render content.

### How to Use:
You can start by customizing the generated files to fit your app's needs. For example, you can modify the manifest to include your app's logo and theme colors:
```json
{
  "name": "My Rails App",
  "short_name": "RailsApp",
  "icons": [
    {
      "src": "/assets/icon.png",
      "sizes": "192x192",
      "type": "image/png"
    }
  ],
  "start_url": "/",
  "display": "standalone",
  "theme_color": "#000000",
  "background_color": "#ffffff"
}
```

## RuboCop Rules by Default 🔍

### What It Is:
Rails 7.2 includes a set of default RuboCop rules from the `rubocop-rails-omakase` gem. RuboCop is a static code analyzer that helps you write clean, consistent, and idiomatic Ruby code.

### Why It's Useful:
These default rules provide a solid starting point for code styling, especially for teams that haven't established their own style guide. It helps maintain a consistent codebase, making it easier to read and collaborate on.

### How to Customize:
You can customize the RuboCop rules to fit your team's preferences by editing the `.rubocop.yml` file generated in your project. Here's an example of customizing a few rules:
```yaml
Style/StringLiterals:
  EnforcedStyle: double_quotes

Metrics/LineLength:
  Max: 100
```
You can then run RuboCop to analyze your code:
```bash
rubocop
```

## GitHub CI Workflow by Default ✅

### What It Is:
Rails 7.2 automatically adds a GitHub CI workflow file to new applications. This sets up automated testing, linting, and security scanning using GitHub Actions, helping you maintain high code quality.

## Brakeman Installed by Default 🛡️

### What It Is:
Brakeman, a static analysis security tool, is now installed by default in new Rails 7.2 applications. It automatically scans your code for security vulnerabilities and integrates seamlessly with the GitHub CI workflow.

### How to Run Brakeman:
You can run Brakeman manually using:
```bash
brakeman
```
Or let it run automatically on every push to GitHub as part of your CI pipeline. This ensures that potential security issues are caught early in the development process.

## Puma Thread Count Adjustment 🧵

### What It Is:
Rails 7.2 changes the default number of threads for Puma, the default web server for Rails, from 5 to 3. This change is based on real-world experience and helps optimize concurrency and performance.

### Why It Matters:
Reducing the thread count minimizes the time spent waiting for the Global VM Lock (GVL) to release, which can improve request response times in your application.

### How to Customize:
If you want to adjust the thread count, you can modify the `puma.rb` configuration file:
```ruby
threads_count = ENV.fetch("RAILS_MAX_THREADS") { 3 }
threads threads_count, threads_count
```
You can experiment with different values to find the optimal balance for your specific application.

## Job Scheduling within Transactions ⏳

### What It Is:
A common issue in Rails is enqueuing jobs inside transactions, which can lead to errors if the job is processed before the transaction is committed. Rails 7.2 now defers job enqueuing until after the transaction is committed, preventing these errors.

### Example:
Here's how this feature works:
```ruby
Topic.transaction do
  topic = Topic.create
  NewTopicNotificationJob.perform_later(topic)
end
```
In the above example, `NewTopicNotificationJob` won't be enqueued until the transaction is successfully committed. This ensures that your job only runs when the associated database changes are safely saved.

If you want to explicitly control this behavior, you can use the `enqueue_after_transaction_commit` option:
```ruby
NewTopicNotificationJob.set(enqueue_after_transaction_commit: true).perform_later(topic)
```

## Transaction Callbacks 💡

### What It Is:
Rails 7.2 introduces the ability to register callbacks on transactions, even outside of ActiveRecord models. This provides more flexibility in managing actions that should only occur after a transaction is committed or rolled back.

### Example:
Let's say you want to send an email notification only after a transaction commits successfully:
```ruby
Article.transaction do |transaction|
  article.update(published: true)
  transaction.after_commit do
    PublishNotificationMailer.with(article: article).deliver_later
  end
end
```
If the transaction fails, the email won't be sent. This ensures that your code is more robust and predictable.

## YJIT Enabled by Default for Ruby 3.3+ 🚀

### What It Is:
YJIT, the Just-In-Time (JIT) compiler introduced in Ruby, is now enabled by

 default in Rails 7.2 applications running on Ruby 3.3 or later. YJIT can significantly improve the performance of your Rails application by compiling frequently executed code paths into optimized machine code.

### How to Check:
If you're using Ruby 3.3 or later, YJIT will be automatically enabled in your Rails application. You can check the status of YJIT by running:
```ruby
RubyVM::YJIT.runtime_stats
```

If you're using an earlier version of Ruby, consider upgrading to Ruby 3.3 to take advantage of YJIT and other performance enhancements.

## Redesigned Rails Guides 🎨

### What It Is:
Rails 7.2 brings a fresh new design to the Rails Guides, making them cleaner and more user-friendly. The updated design is more consistent with the Rails homepage and simplifies navigation.

### Why It Matters:
The Rails Guides are a vital resource for learning and referencing Rails concepts. The new design makes it easier to find information and provides a more enjoyable reading experience.

## Jemalloc in Default Dockerfile 🧠

### What It Is:
Rails 7.2 includes jemalloc in the default Dockerfile to optimize memory allocation. Jemalloc is a memory allocator that reduces memory fragmentation, which is particularly beneficial for multi-threaded environments like Puma.

### Why It Matters:
Using jemalloc can lead to more efficient memory usage, resulting in lower memory consumption and improved performance for your Rails application.

### How to Use:
If you're using Docker, Rails will automatically use jemalloc. You can also manually add it to your Dockerfile like this:
```dockerfile
RUN apt-get update && apt-get install -y libjemalloc-dev
ENV LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libjemalloc.so.2
```

## Puma-dev Configuration in bin/setup 🖥️

### What It Is:
Puma-dev is a tool that allows you to easily manage multiple Rails applications locally without needing Docker. Rails 7.2 now includes a suggestion to set up puma-dev in your `bin/setup` script.

### Why It's Useful:
If you're working on multiple Rails applications simultaneously, puma-dev simplifies managing them. You can access each app via a custom domain like `myapp.test` instead of managing different ports.

## Conclusion 🎯

Rails 7.2 brings a wealth of new features and improvements that make it easier to develop, test, and deploy your applications. Whether you're excited about the development containers, browser version guards, or the new default RuboCop rules, there's something in this release for everyone.

If you haven't already, upgrade to Rails 7.2 and start exploring these new features. 

Happy coding! 🚀
