---
layout: home
title: "ROR-React Applications"
date: 2024-09-17
categories: "Ruby On Rails"
tags: [Ruby On Rails, ReactJs, Application, Javascript]
---

# Building Applications with Ruby on Rails (RoR) and ReactJS: Approaches and Examples 🚀

Ruby on Rails (RoR) and ReactJS are a powerful combination for building modern web applications. RoR excels at handling backend logic and database interactions, while ReactJS shines on the frontend by offering dynamic user interfaces. This combo allows developers to build scalable, performant, and interactive applications.

In this blog, I'll walk you through different approaches to integrate RoR with ReactJS, explaining each step carefully. Let's dive into it! 💻

## 1. **Monolithic Approach: Adding ReactJS to an Existing RoR App** 🏗️

In the monolithic approach, we embed ReactJS within a traditional Rails application. This is the simplest way to get React into your RoR project, ideal for smaller or less complex apps.

### Steps:
1. **Create a Rails App:**
   Start by creating a new Rails project if you don’t already have one:
   ```bash
   rails new my_app
   ```
   
2. **Add Webpacker Gem:**
   Webpacker is a modern asset pipeline for Rails and supports React out of the box.
   Add the `webpacker` gem to your Gemfile:
   ```ruby
   gem 'webpacker'
   ```
   Then run:
   ```bash
   bundle install
   rails webpacker:install
   rails webpacker:install:react
   ```

3. **Create React Components:**
   Once Webpacker is set up, create your first React component:
   ```bash
   rails generate react:component MyComponent
   ```

4. **Render React in Views:**
   In your Rails views, you can now call the React component like so:
   ```erb
   <%= react_component("MyComponent", { prop1: "value1" }) %>
   ```

5. **Controller Logic:**
   RoR handles your backend and sends data to your React components via props, or through API calls.

### Example: A Simple React Component in Rails 🛠️
Suppose we want to create a basic React component that displays a user’s name:

```jsx
// app/javascript/components/UserGreeting.js
import React from 'react';

const UserGreeting = ({ name }) => {
  return <h1>Hello, {name}! 👋</h1>;
};

export default UserGreeting;
```

In your Rails view, render it like this:

```erb
<%= react_component("UserGreeting", { name: "Lakhveer" }) %>
```

#### Pros:
- 🟢 Easy to implement in existing Rails apps.
- 🟢 Best for small or medium-sized applications.

#### Cons:
- 🔴 Limited separation of concerns, which might become harder to manage as the app grows.

---

## 2. **API-Only Rails with a React Frontend (Separate Frontend-Backend Architecture) 🖥️🔗**

In this approach, you separate Rails as the API backend and ReactJS as the frontend. They communicate through HTTP requests (typically REST or GraphQL). This is a more scalable solution, great for larger apps.

### Steps:

1. **Create API-Only Rails App:**
   Start by creating an API-only Rails app:
   ```bash
   rails new my_api --api
   ```

2. **Set Up Controllers:**
   In the Rails backend, you’ll create controllers that serve JSON data. Here's an example of a simple `UsersController`:
   ```ruby
   class UsersController < ApplicationController
     def index
       users = User.all
       render json: users
     end
   end
   ```

3. **Create a React Frontend:**
   You can either use `create-react-app` to bootstrap a React application or set it up manually:
   ```bash
   npx create-react-app my_frontend
   ```

4. **Fetch Data from Rails:**
   In the React app, fetch data from your Rails API:
   ```jsx
   import React, { useState, useEffect } from 'react';

   const UsersList = () => {
     const [users, setUsers] = useState([]);

     useEffect(() => {
       fetch('http://localhost:3000/users')
         .then(response => response.json())
         .then(data => setUsers(data));
     }, []);

     return (
       <ul>
         {users.map(user => (
           <li key={user.id}>{user.name}</li>
         ))}
       </ul>
     );
   };

   export default UsersList;
   ```

5. **Run Rails and React:**
   Start both your Rails API and React frontend servers, making sure to set up proper CORS handling.

### Example: API-Only Rails with ReactJS 🎨
Imagine you have a Rails API that serves a list of users. Your React frontend will display this list dynamically.

Rails API response (JSON):
```json
[
  { "id": 1, "name": "Alice" },
  { "id": 2, "name": "Bob" }
]
```

React frontend:
```jsx
import React, { useEffect, useState } from 'react';

const UsersList = () => {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("http://localhost:3000/users")
      .then(response => response.json())
      .then(data => setUsers(data));
  }, []);

  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
};

export default UsersList;
```

#### Pros:
- 🟢 Clear separation between backend and frontend, making the app easier to scale.
- 🟢 You can deploy the backend and frontend separately.
- 🟢 React developers can work independently from Rails developers.

#### Cons:
- 🔴 More complex setup.
- 🔴 You have to manage two separate deployments.

---

## 3. **Full Stack with GraphQL: Rails + ReactJS with GraphQL Query Language 📊**

This approach uses GraphQL to enhance communication between Rails and React. GraphQL allows more flexible and efficient data fetching compared to traditional REST.

### Steps:

1. **Set Up GraphQL in Rails:**
   First, add the `graphql` gem to your Rails app and install it:
   ```ruby
   gem 'graphql'
   ```

   Then, generate the GraphQL schema:
   ```bash
   rails generate graphql:install
   ```

2. **Create a GraphQL Query:**
   Define your GraphQL queries in the Rails backend:
   ```ruby
   class Types::QueryType < Types::BaseObject
     field :users, [Types::UserType], null: false

     def users
       User.all
     end
   end
   ```

3. **Set Up Apollo Client in React:**
   Install Apollo Client in your React app for GraphQL data fetching:
   ```bash
   npm install @apollo/client graphql
   ```

   Then, use Apollo Client to fetch data in React:
   ```jsx
   import React from 'react';
   import { useQuery, gql } from '@apollo/client';

   const GET_USERS = gql`
     query {
       users {
         id
         name
       }
     }
   `;

   const UsersList = () => {
     const { loading, error, data } = useQuery(GET_USERS);

     if (loading) return <p>Loading...</p>;
     if (error) return <p>Error :(</p>;

     return (
       <ul>
         {data.users.map(user => (
           <li key={user.id}>{user.name}</li>
         ))}
       </ul>
     );
   };

   export default UsersList;
   ```

### Example: Full Stack with GraphQL 🌐
The above setup allows your React frontend to make more efficient queries to the Rails API using GraphQL. Instead of making multiple API requests, you can fetch all the needed data in one query.

#### Pros:
- 🟢 More efficient data fetching.
- 🟢 Reduces over-fetching and under-fetching.
- 🟢 Very flexible for front-end developers.

#### Cons:
- 🔴 Steeper learning curve compared to REST.
- 🔴 Requires more setup in both backend and frontend.

---

## Conclusion 🎯

The combination of Ruby on Rails and ReactJS offers great flexibility in building web applications. Whether you're integrating React into a monolithic Rails app, separating the front and back ends, or using GraphQL for optimized data handling, there are approaches to suit every need.

- If you're working on a small or medium-sized project, **Monolithic Rails + React** might be the best option.
- For larger, scalable apps, **API-Only Rails with a React Frontend** is ideal.
- For a more modern approach with optimized data fetching, **Rails + React with GraphQL** is the way to go.

Choose the approach that works best for your project and start building awesome applications today! 🚀

Happy coding! ✨
