---
layout: home
title: "Graphql in Ruby On Rails"
date: 2024-09-03
categories: "Ruby on Rails"
tags: [Ruby, Graphql, Ruby on Rails, Development]
image: '/assets/images/graphql.png'
---

### 🚀 Getting Started with GraphQL in Ruby on Rails

GraphQL is a powerful query language for your API, allowing clients to request exactly what they need, and nothing more. In this blog, we'll explore how to implement GraphQL in a Ruby on Rails application, making it simple and easy to understand with examples and emojis!

<img src="/assets/images/graphql.png" alt="drawing" style="width:80%;"/>

### 🧐 What is GraphQL?

GraphQL, developed by Facebook, is a query language for APIs and a runtime for executing those queries. Unlike REST, where you might need multiple endpoints to get related data, GraphQL allows you to fetch the exact data you need in a single request.

**Key Benefits:**
- **Single Endpoint:** All requests go to a single `/graphql` endpoint.
- **Client-Specified Queries:** Clients define the structure of the response.
- **Efficient Data Fetching:** Avoid over-fetching or under-fetching data.

### 🔧 Setting Up GraphQL in Ruby on Rails

Let’s dive into how to integrate GraphQL into a Ruby on Rails application.

#### 1. **Add GraphQL Gem**

First, add the `graphql` gem to your Gemfile and run `bundle install`:

```ruby
gem 'graphql'
```

#### 2. **Generate GraphQL Setup**

Run the following generator to set up GraphQL:

```bash
rails generate graphql:install
```

This will create a bunch of files, including:
- `app/graphql/types/query_type.rb`: Defines the queries.
- `app/graphql/myapp_schema.rb`: Defines the schema for your app.

### 📝 Defining a Simple Query

Let’s say we have a `Post` model with `title` and `content`. We want to create a GraphQL query to fetch all posts.

#### 1. **Create a Post Type**

In `app/graphql/types/`, create a new file `post_type.rb`:

```ruby
module Types
  class PostType < Types::BaseObject
    field :id, ID, null: false
    field :title, String, null: false
    field :content, String, null: true
  end
end
```

This defines the `PostType` object that will be returned by our query.

#### 2. **Add a Query to Fetch Posts**

In `app/graphql/types/query_type.rb`, define the query:

```ruby
module Types
  class QueryType < Types::BaseObject
    field :posts, [PostType], null: false

    def posts
      Post.all
    end
  end
end
```

This query fetches all posts from the database and returns them as an array of `PostType` objects.

### 🎯 Triggering the GraphQL Endpoint from Frontend

Let’s see how you can trigger this endpoint from the frontend using a simple fetch API call.

```javascript
fetch('/graphql', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Accept': 'application/json',
  },
  body: JSON.stringify({
    query: `
      {
        posts {
          id
          title
          content
        }
      }
    `
  })
})
.then(response => response.json())
.then(data => console.log(data));
```

### 🛠️ Executing the Query

When the frontend triggers the `/graphql` endpoint with the query above, it requests exactly the fields it needs: `id`, `title`, and `content`. The backend executes this query and returns the requested data in a JSON format:

```json
{
  "data": {
    "posts": [
      {
        "id": "1",
        "title": "GraphQL is Awesome",
        "content": "Here is why GraphQL is awesome..."
      },
      {
        "id": "2",
        "title": "Another Post",
        "content": "More content here..."
      }
    ]
  }
}
```

### 🤓 Conclusion

GraphQL provides a more flexible and efficient way to interact with your API, especially when compared to traditional REST APIs. By specifying exactly what data you need, you can reduce the amount of data transferred and improve the performance of your application.

With this setup, you're ready to start using GraphQL in your Ruby on Rails application. Happy coding! 🎉
