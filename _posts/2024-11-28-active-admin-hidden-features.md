---
layout: home
title: "Active Admin Hidden Features"
date: 2024-11-28
categories: "Ruby On Rails"
tags: [Ruby On Rails, Active Admin, Hidden, Unique, Features]
image: 'https://github.com/user-attachments/assets/5eb74fd0-1740-4ceb-930b-a2029812eee8'
---

# 🎉 Unlocking Hidden Gems in Active Admin for Ruby on Rails 🌟

Active Admin is a powerful tool for building elegant backends for Ruby on Rails applications. While its primary features like resource management and dashboards are well-known, there are some hidden gems and lesser-known features that can supercharge your admin panel! Let’s dive into these unique tricks and features with examples to help you get the most out of Active Admin.

![test](https://github.com/user-attachments/assets/5eb74fd0-1740-4ceb-930b-a2029812eee8)

---

## 1. **Customizing Resource Forms Dynamically** 🌟

Active Admin allows you to dynamically customize forms based on conditions. For instance, let’s say you want to show additional fields only for certain roles:

```ruby
ActiveAdmin.register User do
  form do |f|
    f.inputs "Basic Details" do
      f.input :name
      f.input :email
    end

    if current_user.admin?
      f.inputs "Admin Details" do
        f.input :admin_notes
      end
    end

    f.actions
  end
end
```

This adds role-specific fields, making your admin panel smarter and more context-aware.

---

## 2. **Using Custom Pages for Specialized Actions** 🔗

Need a custom page for a special report or admin action? Active Admin makes this possible with ease:

```ruby
ActiveAdmin.register_page "Monthly Report" do
  content do
    panel "Summary" do
      para "This is a custom page for monthly reports."
    end
  end
end
```

Access this page at `/admin/monthly_report`, and you can add any custom functionality you need!

---

## 3. **Conditional Scopes for Enhanced Filtering** 🔍

Scopes in Active Admin are great for filtering records, but did you know you can make them conditional? Here’s how:

```ruby
ActiveAdmin.register Order do
  scope :all

  scope("High Value Orders") { |orders| orders.where("total_price > ?", 500) }

  scope :today do
    Time.zone.now.hour > 12 ? Order.where(created_at: Date.today) : Order.none
  end
end
```

This lets you define highly contextual and useful filters for your admin users.

---

## 4. **Custom Action Items for Better Workflow** ⚙️

Action items are those handy buttons in the admin interface. You can add custom actions like this:

```ruby
ActiveAdmin.register Post do
  action_item :publish, only: :show do
    link_to "Publish", publish_admin_post_path(post), method: :put if !post.published?
  end

  member_action :publish, method: :put do
    resource.update(published: true)
    redirect_to resource_path, notice: "Post published successfully!"
  end
end
```

Your admin panel now has a custom Publish button for unpublished posts, boosting productivity.

---

## 5. **Interactive Dashboards with Charts and Stats** 🎨

Active Admin dashboards are great for summaries, but you can make them interactive with charts. Use the *chartkick* gem:

```ruby
ActiveAdmin.register_page "Dashboard" do
  content do
    panel "User Signups" do
      line_chart User.group_by_day(:created_at).count
    end
  end
end
```

This provides an insightful and visually appealing dashboard for your app.

---

## 6. **Batch Actions for Bulk Operations** 🚀

You can add custom batch actions to handle bulk tasks efficiently:

```ruby
ActiveAdmin.register User do
  batch_action :approve do |ids|
    User.where(id: ids).update_all(approved: true)
    redirect_to collection_path, notice: "Users approved successfully!"
  end
end
```

This lets your admins perform bulk updates, saving time and effort.

---

## 7. **Extending Active Admin with Custom Views** 🎨

Want more control over how data is displayed? Use custom partials:

```ruby
index as: :block do |user|
  div class: "user-block" do
    h3 user.name
    para user.email
  end
end
```

This renders a completely customized block layout for your admin index page.

---

## 8. **Integration with Pundit for Granular Authorization** ⚖️

While Active Admin comes with built-in authorization, integrating with Pundit gives you more granular control:

```ruby
ActiveAdmin.register User do
  controller do
    include Pundit
    def scoped_collection
      policy_scope(User)
    end
  end
end
```

Admins now see only the data they’re authorized to view, enhancing security.

---

## 9. **Improving Performance with Paginated Associations** 🚗

If you’re dealing with large datasets, loading associated records can slow things down. Use `:includes` for eager loading:

```ruby
ActiveAdmin.register Order do
  includes :user, :products
end
```

This speeds up your admin panel significantly for large associations.

---

## 10. **Adding Emojis and Styles for Fun 🌟**

Make your admin panel lively by using emojis and CSS:

```ruby
ActiveAdmin.register Post do
  index do
    column(:status) { |post| post.published? ? "🔥 Published" : "❌ Draft" }
    actions
  end
end
```

A small touch like this makes your admin panel more engaging and user-friendly.

---

## Wrapping Up 🎉

Active Admin is more than just a CRUD generator; it’s a powerful tool that can be tailored to fit any admin need. By leveraging these hidden gems, you can create a more dynamic, efficient, and user-friendly admin interface. Which of these tricks are you excited to try? Share your thoughts in the comments!

Happy coding! 🚀

