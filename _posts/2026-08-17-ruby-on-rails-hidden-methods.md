---
layout: home
title: "Ruby on Rails Hidden Methods"
date: 2026-08-17
categories: "Ruby On Rails"
tags: [Ruby On Rails, Programming, Hidden Methods, Active Record, Action Controller, Optimization, Action View, Software Development]
image: 'https://github.com/user-attachments/assets/841b03d2-9bff-45ae-9559-fd897753bcaa'
---

# 🚀 Ruby on Rails Hidden Methods: The Secret Toolkit for Writing Faster, Cleaner & Smarter Rails Code

Ruby on Rails is famous for making complex things feel simple.

But that simplicity can hide an enormous amount of power.

Most Rails developers know `find`, `where`, `render`, `redirect_to`, and `validates`. Yet Rails contains hundreds of lesser-known methods that can dramatically improve **performance, readability, database efficiency, API design, and maintainability**.

The real Rails skill isn't knowing more syntax.

> **It's knowing which Rails abstraction solves a problem before writing unnecessary code yourself.**

<img width="864" height="1821" alt="ChatGPT Image Aug 17, 2026, 07_40_11 PM" src="https://github.com/user-attachments/assets/841b03d2-9bff-45ae-9559-fd897753bcaa" />

This guide dives deep into some of the most useful "hidden" methods across:

* 🗄️ Active Record
* 🎮 Action Controller
* 🎨 Action View
* 🧠 Active Support
* 💎 Powerful gems that add even more surprising capabilities

> **Note:** Exact APIs can vary slightly between Rails versions. Examples below target modern Rails, with special attention to Rails 7/8-style APIs.

---

# 🗺️ 1. Active Record — Hidden Database Superpowers

Active Record is much more than an ORM that converts rows into Ruby objects.

It can help you control:

* SQL generation
* memory usage
* database round trips
* bulk operations
* locking
* eager loading
* asynchronous queries
* updates without callbacks
* batch processing

Let's explore the methods that often go unnoticed.

---

# 🔍 2. `pick` — Get One Value Without Building an Object

Suppose you only need a user's email.

A common approach is:

```ruby
user = User.find(10)
user.email
```

But Rails creates a full `User` object.

Instead:

```ruby
email = User.where(id: 10).pick(:email)
```

The database essentially performs:

```sql
SELECT email FROM users WHERE id = 10 LIMIT 1;
```

You get:

```ruby
"user@example.com"
```

## Why is this useful?

Because you're telling Rails:

> "I don't need an Active Record object. Give me the value."

You can even retrieve multiple columns:

```ruby
name, email = User.where(id: 10).pick(:name, :email)
```

### 💡 Optimization principle

If you only need **one scalar value**, prefer:

```ruby
pick
```

over:

```ruby
find(...).attribute
```

---

# 📦 3. `pluck` — Skip Object Instantiation

Need thousands of IDs?

Don't do:

```ruby
User.all.map(&:id)
```

That loads Active Record objects into memory.

Use:

```ruby
User.pluck(:id)
```

Result:

```ruby
[1, 2, 3, 4, 5]
```

Multiple columns:

```ruby
User.pluck(:id, :email)
```

Result:

```ruby
[
  [1, "a@example.com"],
  [2, "b@example.com"]
]
```

### 🚀 Real-world example

Instead of:

```ruby
users = User.where(active: true)

emails = users.map(&:email)
```

Use:

```ruby
emails = User.where(active: true).pluck(:email)
```

This can eliminate unnecessary object creation and reduce memory usage significantly.

---

# ⚡ 4. `exists?` — Don't Load Records Just to Check Existence

Bad:

```ruby
if User.where(email: email).first
  ...
end
```

Better:

```ruby
if User.exists?(email: email)
  ...
end
```

Rails can generate an efficient existence query instead of loading a complete record.

You can also use:

```ruby
User.exists?(10)
```

or:

```ruby
User.exists?(id: 10)
```

### ❌ Avoid

```ruby
User.where(email: email).present?
```

### ✅ Prefer

```ruby
User.exists?(email: email)
```

---

# 🎯 5. `find_by` vs `where`

A subtle but important distinction:

```ruby
User.find_by(email: email)
```

returns:

```ruby
User
```

or:

```ruby
nil
```

Whereas:

```ruby
User.where(email: email)
```

returns a relation.

So if you need one record:

```ruby
user = User.find_by(email: email)
```

If you need a collection:

```ruby
users = User.where(active: true)
```

This isn't merely style.

You're communicating the expected cardinality of the query.

---

# 🧨 6. `sole` — Enforce Exactly One Record

This is a fascinating method.

Suppose your business rule says:

> There must be exactly one active configuration.

You could write:

```ruby
config = Configuration.where(active: true).first
```

But this silently accepts multiple records.

Instead:

```ruby
config = Configuration.where(active: true).sole
```

`sole` expects exactly one record.

If there are:

* zero → exception
* one → returns it
* multiple → exception

That's incredibly useful for enforcing assumptions.

```ruby
User.where(email: "test@example.com").sole
```

It effectively turns an implicit assumption into an explicit contract.

---

# 🧠 7. `load` — Explicitly Execute a Relation

Rails relations are lazy.

```ruby
users = User.where(active: true)
```

The SQL may not execute immediately.

You can explicitly load it:

```ruby
users.load
```

Now:

```ruby
users.loaded?
```

returns:

```ruby
true
```

This can be useful when you're deliberately preparing data before subsequent operations.

---

# 🔥 8. `load_async` — Let Rails Fetch Data Asynchronously

Modern Rails provides:

```ruby
users = User.where(active: true).load_async
```

Instead of immediately blocking while waiting for the query, Rails can schedule the query using its asynchronous query infrastructure.

For example:

```ruby
users = User.where(active: true).load_async

orders = Order.where(status: :pending).load_async

# Other Ruby work...

users.each do |user|
  # ...
end
```

When the result is needed, Rails waits for it.

### ⚠️ Important

This isn't a magic performance button.

It is most useful when:

* queries are independent
* database capacity is sufficient
* request concurrency is understood
* the application benefits from overlapping database work

More database concurrency can also mean more database pressure.

---

# 📊 9. `find_each` — Process Millions of Records Safely

Never casually do this:

```ruby
User.all.each do |user|
  process(user)
end
```

If you have 10 million users, you could create a massive memory problem.

Use:

```ruby
User.find_each do |user|
  process(user)
end
```

Rails processes records in batches.

You can control the batch size:

```ruby
User.find_each(batch_size: 1000) do |user|
  process(user)
end
```

---

# 📦 10. `find_in_batches`

Sometimes you actually want the entire batch.

```ruby
User.find_in_batches(batch_size: 1000) do |users|
  send_batch_to_external_service(users)
end
```

This is different from:

```ruby
find_each
```

because `find_each` gives you individual records while `find_in_batches` gives you collections.

---

# 🏭 11. `in_batches`

This becomes particularly powerful for bulk operations.

For example:

```ruby
User.where(active: false).in_batches(of: 1000) do |batch|
  batch.update_all(archived: true)
end
```

You avoid loading every object into Ruby.

Even better, sometimes you can directly chain operations:

```ruby
User.where(active: false)
    .in_batches(of: 1000)
    .update_all(archived: true)
```

This is an excellent pattern for large datasets.

---

# ⚡ 12. `update_all` — Bulk Update Without Objects

Suppose you need to deactivate 100,000 users.

Don't do:

```ruby
User.where(expired: true).find_each do |user|
  user.update(active: false)
end
```

That can generate thousands of SQL statements.

Instead:

```ruby
User.where(expired: true).update_all(active: false)
```

### But there is a critical catch.

`update_all` bypasses:

* validations
* callbacks
* automatic timestamp updates

So:

```ruby
update_all
```

is powerful—but dangerous if your business logic depends on callbacks.

Use it deliberately.

---

# 🛠️ 13. `update_columns`

Sometimes you need to update a single record while intentionally bypassing validations and callbacks.

```ruby
user.update_columns(
  last_seen_at: Time.current
)
```

This can be useful for internal metadata.

But don't use it casually.

If your model has:

```ruby
before_save
after_save
```

those won't run.

---

# 💥 14. `insert_all`

Need to insert thousands of records?

Instead of:

```ruby
records.each do |record|
  User.create!(record)
end
```

consider:

```ruby
User.insert_all([
  { name: "A", email: "a@example.com" },
  { name: "B", email: "b@example.com" },
  { name: "C", email: "c@example.com" }
])
```

This is designed for bulk insertion.

---

# 🔄 15. `upsert_all`

One of the most useful bulk APIs:

```ruby
Product.upsert_all(
  products,
  unique_by: :sku
)
```

It lets the database insert or update records according to a uniqueness constraint.

This is excellent for:

* synchronization jobs
* imports
* external APIs
* data pipelines
* inventory updates

### Architecture lesson

For bulk data operations, let the **database do database work** instead of creating thousands of Ruby objects.

---

# 🔐 16. `with_lock`

Need to safely modify a record under a database transaction and row lock?

```ruby
account.with_lock do
  account.balance -= 100
  account.save!
end
```

This is extremely useful for operations such as:

* balances
* inventory
* counters
* reservations
* financial state

Without proper locking, two concurrent requests can read the same state and overwrite each other's changes.

---

# 🕒 17. `touch`

Sometimes you don't want to save an entire model.

You just want to update timestamps.

```ruby
user.touch
```

Or:

```ruby
user.touch(:last_seen_at)
```

This is useful for cache invalidation and parent-child timestamp tracking.

For example:

```ruby
class Comment < ApplicationRecord
  belongs_to :post, touch: true
end
```

Updating a comment can update the associated post's timestamp.

---

# 🎭 18. `becomes`

This is a less commonly encountered Active Record method.

Suppose STI is involved:

```ruby
class Vehicle < ApplicationRecord
end

class Car < Vehicle
end
```

You can use:

```ruby
vehicle = Vehicle.find(1)

car = vehicle.becomes(Car)
```

This changes the Ruby representation without changing the underlying database record itself.

Use this carefully—it's primarily useful for presentation/modeling scenarios rather than ordinary application logic.

---

# 🎮 19. Action Controller — Hidden Controller APIs

Controllers contain much more than:

```ruby
render
redirect_to
params
```

Let's look at some lesser-known methods.

---

# 🧹 20. `params.expect`

Modern Rails provides a concise way to work with expected parameters.

Conceptually, instead of repeatedly doing:

```ruby
params.require(:user).permit(:name, :email)
```

you can use the newer `expect` API:

```ruby
params.expect(
  user: [:name, :email]
)
```

This is especially valuable because parameter structure and permitted fields are expressed together.

### Why it matters

Strong parameters aren't just about security.

They're an API contract between:

```text
HTTP Request
     ↓
Controller
     ↓
Application
```

---

# 🔙 21. `redirect_back_or_to`

Instead of manually checking the referrer:

```ruby
redirect_to(request.referer || root_path)
```

Rails provides:

```ruby
redirect_back_or_to(root_path)
```

Example:

```ruby
def destroy
  @post.destroy!

  redirect_back_or_to(
    posts_path,
    notice: "Post deleted"
  )
end
```

Cleaner and communicates intent better.

---

# 🧪 22. `performed?`

Ever wondered whether Rails has already rendered or redirected?

Use:

```ruby
performed?
```

Example:

```ruby
return if performed?

render json: { success: true }
```

This can be useful in complex controller flows, although it shouldn't become a substitute for clean control flow.

---

# 🚪 23. `head`

If you don't need a response body, don't send one.

Instead of:

```ruby
render json: {}
```

you can use:

```ruby
head :no_content
```

Or:

```ruby
head :ok
```

Excellent for:

* webhooks
* health checks
* DELETE endpoints
* lightweight APIs

---

# 📡 24. `send_data`

Need to generate a file dynamically?

```ruby
send_data(
  csv_data,
  filename: "users.csv",
  type: "text/csv"
)
```

This is useful for:

* CSV exports
* generated reports
* PDFs
* dynamically generated files

---

# ♻️ 25. `fresh_when` and `stale?`

HTTP caching is often ignored by application developers.

Rails provides helpers such as:

```ruby
fresh_when @product
```

and:

```ruby
if stale?(@product)
  render
end
```

These use HTTP caching semantics such as:

* ETag
* Last-Modified

This can prevent Rails from repeatedly generating identical responses.

### The hidden optimization

Sometimes the fastest Rails request is the request Rails **doesn't have to render**.

---

# 🎨 26. Action View — Rails Template Magic

Rails views have a collection of surprisingly powerful methods.

---

# 🧩 27. `content_for`

Suppose a layout has:

```erb
<head>
  <%= yield :head %>
</head>
```

A specific view can provide:

```erb
<% content_for :head do %>
  <meta name="description" content="Ruby on Rails guide">
<% end %>
```

This allows child templates to inject content into specific layout regions.

---

# 📦 28. `capture`

`capture` lets you capture generated HTML as a string.

Example helper:

```ruby
def card(title)
  content_tag(:div, class: "card") do
    concat content_tag(:h2, title)
    concat capture { yield }
  end
end
```

Then:

```erb
<%= card("Profile") do %>
  <p>Hello Rails!</p>
<% end %>
```

This is extremely useful for reusable view components and helpers.

---

# 🔗 29. `safe_join`

Instead of:

```ruby
links = items.map do |item|
  link_to(item.name, item_path(item))
end

links.join(", ")
```

Rails provides:

```ruby
safe_join(
  items.map { |item| link_to(item.name, item_path(item)) },
  ", "
)
```

This handles HTML safety correctly.

That's important because manually concatenating HTML can easily introduce escaping problems.

---

# 🧹 30. `truncate`

Need to limit text?

```erb
<%= truncate(post.body, length: 100) %>
```

You can customize the omission:

```erb
<%= truncate(
  post.body,
  length: 120,
  omission: "… Read more"
) %>
```

Small method, huge practical value.

---

# 🔁 31. `cycle`

Need alternating classes?

```erb
<% @users.each do |user| %>
  <div class="<%= cycle("odd", "even") %>">
    <%= user.name %>
  </div>
<% end %>
```

This is particularly useful in tables and repeated layouts.

---

# 🎯 32. `link_to_if`

Instead of:

```erb
<% if user.active? %>
  <%= link_to user.name, user_path(user) %>
<% else %>
  <%= user.name %>
<% end %>
```

you can write:

```erb
<%= link_to_if(
  user.active?,
  user.name,
  user_path(user)
) %>
```

Cleaner view logic.

---

# 💾 33. `cache` — Fragment Caching

One of Rails' most important but underused features:

```erb
<% cache @product do %>
  <%= render @product %>
<% end %>
```

Rails stores the rendered fragment and can reuse it.

For collections:

```erb
<%= render partial: "product", collection: @products, cached: true %>
```

This can dramatically reduce view rendering cost.

---

# 🧠 34. Active Support — Rails' Secret Weapon

Active Support is one of the reasons Rails feels so expressive.

It adds functionality to:

* String
* Array
* Hash
* Object
* Numeric
* Time
* Module
* Classes

Some methods are so convenient that developers forget they're Rails extensions.

---

# 🪄 35. `presence`

Instead of:

```ruby
if name.present?
  name
else
  "Anonymous"
end
```

you can write:

```ruby
name.presence || "Anonymous"
```

Even better:

```ruby
username = params[:username].presence || "guest"
```

---

# 🧹 36. `presence_in`

This is particularly useful for whitelisting values.

Instead of:

```ruby
if %w[small medium large].include?(params[:size])
  size = params[:size]
end
```

you can write:

```ruby
size = params[:size].presence_in(
  %w[small medium large]
)
```

If the value isn't allowed, you get `nil`.

Beautifully expressive.

---

# 🔍 37. `in?`

Instead of:

```ruby
%w[admin manager].include?(role)
```

Rails lets you write:

```ruby
role.in?(%w[admin manager])
```

It reads naturally:

> Is this role in these values?

---

# 🧬 38. `deep_dup`

Ruby's `dup` doesn't recursively duplicate nested structures.

Rails provides:

```ruby
original = {
  user: {
    name: "Lakhveer"
  }
}

copy = original.deep_dup
```

Now nested mutable objects are duplicated too.

This is extremely useful when manipulating configuration hashes or nested data.

---

# 🔄 39. `deep_transform_keys`

Suppose you receive:

```ruby
data = {
  "first_name" => "John",
  "profile" => {
    "phone_number" => "123"
  }
}
```

You can recursively transform keys:

```ruby
data.deep_transform_keys(&:to_sym)
```

Result:

```ruby
{
  first_name: "John",
  profile: {
    phone_number: "123"
  }
}
```

---

# 🔧 40. `with`

Rails provides a very elegant temporary context pattern.

Instead of:

```ruby
user.name = "John"
user.save!
user.name = "Original"
```

you can use:

```ruby
user.with(name: "John") do |temporary_user|
  temporary_user.save!
end
```

This is particularly useful when temporarily changing attributes or object state.

---

# 🔗 41. `then`

Ruby already has `then`, but Rails developers can use it beautifully for transformation pipelines.

For example:

```ruby
User.find_by(id: params[:id])
    .then { |user| user&.decorate }
    .then { |user| render_user(user) }
```

Another useful pattern:

```ruby
params[:email]
  .then(&:to_s)
  .then(&:strip)
  .then(&:downcase)
```

This creates readable transformation pipelines.

---

# 🧯 42. `suppress`

Suppose a very specific operation is allowed to fail silently.

Instead of:

```ruby
begin
  cache.delete(key)
rescue Redis::BaseError
end
```

Rails provides:

```ruby
suppress(Redis::BaseError) do
  cache.delete(key)
end
```

But use this carefully.

Silently swallowing exceptions can hide serious problems.

---

# 🧩 43. `delegate`

Instead of repeatedly writing:

```ruby
def company_name
  company.name
end

def company_country
  company.country
end
```

use:

```ruby
delegate :name, :country,
  to: :company,
  prefix: true
```

Now:

```ruby
user.company_name
user.company_country
```

This reduces boilerplate.

---

# 🧠 44. `delegate_missing_to`

This is even more interesting.

Suppose:

```ruby
class UserPresenter
  delegate_missing_to :user

  def initialize(user)
    @user = user
  end

  private

  attr_reader :user
end
```

Methods not found on `UserPresenter` can be delegated to the underlying object.

This can make decorators and presenters dramatically smaller.

---

# 🏗️ 45. `class_attribute`

Need inheritable configuration?

```ruby
class BaseService
  class_attribute :timeout

  self.timeout = 10
end

class PaymentService < BaseService
  self.timeout = 30
end
```

Now:

```ruby
PaymentService.timeout
# => 30
```

This is useful for configurable service objects, policies, clients, and framework-style components.

---

# 🧱 46. `ActiveSupport::Concern`

When Rails code grows, concerns can make reusable behavior much cleaner.

```ruby
module Trackable
  extend ActiveSupport::Concern

  included do
    before_create :generate_tracking_id
  end

  def generate_tracking_id
    self.tracking_id ||= SecureRandom.uuid
  end
end
```

Then:

```ruby
class Order < ApplicationRecord
  include Trackable
end
```

### ⚠️ Don't turn concerns into dumping grounds.

A concern should represent a coherent capability, not "random methods used by several models."

---

# ⏱️ 47. `Benchmark.measure`

Active Support gives you convenient benchmarking tools.

```ruby
require "benchmark"

result = Benchmark.measure do
  User.where(active: true).to_a
end

puts result
```

For production applications, dedicated profiling tools are often better, but this is excellent for quick experiments.

---

# 🧮 48. Time Helpers

Rails' time extensions are famous:

```ruby
2.days.ago
```

```ruby
3.hours.from_now
```

```ruby
1.month.from_now
```

But the deeper benefit is readability.

Compare:

```ruby
Time.current + 172800
```

with:

```ruby
2.days.from_now
```

The second communicates business intent.

---

# 🧠 49. `Time.current` vs `Time.now`

In Rails applications, prefer:

```ruby
Time.current
```

when you're working with application time zones.

Instead of:

```ruby
Time.now
```

Rails' `Time.current` respects the configured application time zone.

This distinction becomes extremely important in distributed applications.

---

# 💎 50. Gems That Give Rails Even More Superpowers

Rails itself is powerful.

But the ecosystem is where things get really interesting.

---

# 🐂 51. Bullet — Find N+1 Queries

One of the most valuable Rails development gems is **Bullet**.

It can identify problems such as:

```ruby
users = User.all

users.each do |user|
  puts user.company.name
end
```

If `company` isn't eager loaded, this can generate:

```text
1 query for users
+
N queries for companies
```

Bullet helps identify these patterns.

Then you can improve it:

```ruby
User.includes(:company)
```

### 🚨 Why it matters

N+1 queries can destroy application performance while everything still appears to work correctly.

---

# 🧪 52. Prosopite — Another N+1 Detective

**Prosopite** is another tool for detecting N+1 queries.

It's particularly useful when you want automatic detection during development and testing.

A strong Rails performance workflow can use tools like:

```text
Bullet / Prosopite
        ↓
Detect N+1
        ↓
Inspect SQL
        ↓
Fix associations
        ↓
Benchmark
```

---

# 📈 53. rack-mini-profiler — See Performance in Development

**rack-mini-profiler** provides request-level profiling information directly while developing your Rails application.

It can help answer:

> Why did this request take 900 ms?

Instead of guessing, inspect:

* SQL time
* view rendering
* application execution
* expensive calls

---

# 🧠 54. memory_profiler — Find Memory Problems

Performance isn't just CPU.

Your application might be fast but consume enormous memory.

Example:

```ruby
report = MemoryProfiler.report do
  User.all.map(&:email)
end

report.pretty_print
```

This can help identify excessive object allocations.

---

# 🔬 55. StackProf — Profile Ruby Code

When Ruby code itself is slow, **StackProf** can help identify where execution time is being spent.

It's particularly useful for:

* CPU-heavy services
* complex transformations
* expensive Ruby loops
* serialization
* background jobs

---

# 🏗️ 56. Strong Migrations — Protect Production Databases

Database migrations can become dangerous at scale.

For example:

```ruby
add_column :users, :status, :string, default: "active"
```

Depending on the database and table size, seemingly harmless schema changes can create locking or performance problems.

**Strong Migrations** warns developers about potentially dangerous migration patterns.

This is especially valuable for production systems with large tables.

---

# 📝 57. Scenic — Database Views in Rails

Sometimes your SQL logic is too complex for ordinary Active Record scopes.

Scenic lets you manage database views through Rails migrations.

For reporting systems, analytics, and complex read models, database views can sometimes be much cleaner than forcing everything into Ruby.

---

# 🗄️ 58. IdentityCache — Avoid Repeated Database Fetches

For read-heavy applications, caching database records can sometimes provide substantial performance improvements.

IdentityCache, associated with Shopify's Rails ecosystem, provides an abstraction for caching Active Record records.

But remember:

> Caching introduces consistency complexity.

Don't add caching before measuring the problem.

---

# 🧭 59. Marginalia — Understand Where SQL Came From

Ever looked at a database query and wondered:

> "Which Rails code generated this?"

Marginalia can annotate SQL queries with application context.

This becomes extremely useful when debugging large Rails applications where dozens of controllers, jobs, and services access the same tables.

---

# 🔥 60. The Real Rails Optimization Formula

Knowing hidden methods isn't enough.

The real optimization process is:

```text
Measure
   ↓
Find bottleneck
   ↓
Understand SQL / allocations
   ↓
Choose correct Rails abstraction
   ↓
Benchmark
   ↓
Monitor in production
```

Don't optimize based purely on assumptions.

---

# 🧠 61. Hidden Method Cheat Sheet

| Problem                          | Better Rails Tool        |
| -------------------------------- | ------------------------ |
| Need one column                  | `pick`                   |
| Need many values                 | `pluck`                  |
| Check existence                  | `exists?`                |
| Exactly one record               | `sole`                   |
| Process millions                 | `find_each`              |
| Process batches                  | `find_in_batches`        |
| Bulk database changes            | `in_batches`             |
| Bulk update                      | `update_all`             |
| Bulk insert                      | `insert_all`             |
| Insert/update bulk data          | `upsert_all`             |
| Safe concurrent modification     | `with_lock`              |
| Update timestamp                 | `touch`                  |
| No-content response              | `head`                   |
| Redirect to previous page        | `redirect_back_or_to`    |
| Check response already performed | `performed?`             |
| HTTP caching                     | `fresh_when`, `stale?`   |
| Capture HTML                     | `capture`                |
| Join HTML safely                 | `safe_join`              |
| Conditional link                 | `link_to_if`             |
| Fragment caching                 | `cache`                  |
| Empty-value fallback             | `presence`               |
| Allowed value check              | `presence_in`            |
| Membership test                  | `in?`                    |
| Deep copy                        | `deep_dup`               |
| Recursive key transformation     | `deep_transform_keys`    |
| Temporary object context         | `with`                   |
| Delegation                       | `delegate`               |
| Missing-method delegation        | `delegate_missing_to`    |
| Inheritable configuration        | `class_attribute`        |
| Reusable model behavior          | `ActiveSupport::Concern` |

---

# 🏆 62. The Five Rules I'd Keep on a Rails Developer's Desk

### Rule #1 — Don't load objects you don't need

Instead of:

```ruby
User.all.map(&:email)
```

consider:

```ruby
User.pluck(:email)
```

---

### Rule #2 — Let the database perform database operations

Instead of:

```ruby
users.find_each do |user|
  user.update!(active: false)
end
```

consider:

```ruby
users.update_all(active: false)
```

when callbacks and validations aren't required.

---

### Rule #3 — Make assumptions explicit

Instead of:

```ruby
config.first
```

if exactly one record is required:

```ruby
config.sole
```

---

### Rule #4 — Optimize after measuring

Don't replace readable code with obscure code merely because it "looks faster."

Benchmark first.

---

### Rule #5 — Know what Rails skips

Methods such as:

```ruby
update_all
update_columns
insert_all
upsert_all
```

can bypass parts of Active Record's normal lifecycle.

That's why they're powerful.

And that's also why they can be dangerous.

---

# 🚀 63. Final Thought

The biggest jump in Rails expertise doesn't happen when you memorize 100 new methods.

It happens when you start recognizing the **intent behind a problem**.

Need a scalar?

```ruby
pick
```

Need a list?

```ruby
pluck
```

Need to know whether something exists?

```ruby
exists?
```

Need exactly one?

```ruby
sole
```

Need to process millions?

```ruby
find_each
```

Need a bulk database operation?

```ruby
insert_all
upsert_all
update_all
```

Need HTTP caching?

```ruby
fresh_when
stale?
```

Need reusable behavior?

```ruby
delegate
ActiveSupport::Concern
```

Need to find performance problems?

```text
Bullet
Prosopite
rack-mini-profiler
StackProf
memory_profiler
```

The Rails philosophy is still incredibly relevant:

> **Write expressive code first. Let the framework and database do the heavy lifting. Measure before optimizing.**

Once you understand these hidden APIs, Rails starts feeling less like a collection of magic methods—and more like a carefully designed performance toolkit. 🛠️💎

**The real Rails superpower isn't writing more code.**

**It's knowing when Rails already has the code you were about to write.** 🚀
