---
layout: home
title: "Ruby On Rails with ReactJS RTK Query Graphql"
date: 2024-11-05
categories: "Ruby On Rails"
tags: [ReactJS, Ruby On Rails, RTK Query, Graphql, Components]
image: 'https://github.com/user-attachments/assets/8c258dc1-309f-4761-bb95-f73d05416653'
---

**🚀 Supercharge Your Ruby on Rails Views with React Components! 🚀**

Ruby on Rails (RoR) is a powerful, flexible framework for building web applications. But what if you want to bring the dynamic, interactive capabilities of React into your Rails views? Well, you're in luck! Rails and React can work together beautifully, creating a powerful combo for modern, interactive, and efficient applications.

![79436261-52159b80-7fd9-11ea-994e-2a98dd43e540](https://github.com/user-attachments/assets/8c258dc1-309f-4761-bb95-f73d05416653)

In this blog, we’ll dive into how to integrate React components within your Rails views. We’ll go step-by-step, covering the integration, setting properties, and using RTK Query and GraphQL to maximize the potential of this powerful combination. Let’s get started! 🔥

---

### **1️⃣ Getting Started: Setting Up React with Rails**

First, we need to set up React in your Rails application. Here’s a simple way to get started:

1. **Add the React-Rails gem** to your Gemfile:
    ```ruby
    gem 'react-rails'
    ```
2. **Bundle install** the gem:
    ```bash
    bundle install
    ```
3. **Run the React installer** to set up the necessary files:
    ```bash
    rails generate react:install
    ```
   
Once installed, this gem will let us use React components directly in our Rails views. 🎉

---

### **2️⃣ Creating Your First React Component**

Let’s create a simple React component to display a message:

1. **Generate a React component**:
    ```bash
    rails generate react:component HelloWorld message:string
    ```
   
   This command will create a `HelloWorld` component in the `app/javascript/components` folder.

2. **Edit your component** (`HelloWorld.js`):
    ```javascript
    import React from "react";

    const HelloWorld = ({ message }) => {
      return (
        <div>
          <h1>{message}</h1>
        </div>
      );
    };

    export default HelloWorld;
    ```

3. **Render the component in a Rails view**:
    In any Rails view (e.g., `app/views/home/index.html.erb`), you can now use the `react_component` helper:
    ```erb
    <%= react_component("HelloWorld", { message: "Hello from Rails!" }) %>
    ```

   📝 **Tip**: The `react_component` helper accepts two arguments: the component name and a hash of props. This makes it easy to pass data from Rails to React! 

---

### **3️⃣ Passing Props to React Components from Rails**

Want to pass dynamic data to your React components? Here’s how:

1. In your Rails controller (e.g., `HomeController`), set a variable:
    ```ruby
    def index
      @message = "Dynamic data from Rails!"
    end
    ```

2. In the view, pass the variable as a prop:
    ```erb
    <%= react_component("HelloWorld", { message: @message }) %>
    ```

   Now, your component will display "Dynamic data from Rails!" 🎉.

---

### **4️⃣ Integrating RTK Query for Data Fetching**

RTK Query, part of Redux Toolkit, is a powerful tool for data fetching and caching. Here’s how to use it in our Rails + React setup:

1. **Install Redux Toolkit**:
    ```bash
    npm install @reduxjs/toolkit react-redux
    ```

2. **Set up a Redux store** and configure RTK Query in `store.js`:
    ```javascript
    import { configureStore } from "@reduxjs/toolkit";
    import { setupListeners } from "@reduxjs/toolkit/query";
    import { myApi } from "./services/myApi";

    export const store = configureStore({
      reducer: {
        [myApi.reducerPath]: myApi.reducer,
      },
      middleware: (getDefaultMiddleware) =>
        getDefaultMiddleware().concat(myApi.middleware),
    });

    setupListeners(store.dispatch);
    ```

3. **Create an API service** (e.g., `services/myApi.js`) with RTK Query:
    ```javascript
    import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";

    export const myApi = createApi({
      reducerPath: "myApi",
      baseQuery: fetchBaseQuery({ baseUrl: "/api/v1/" }),
      endpoints: (builder) => ({
        getItems: builder.query({
          query: () => "items",
        }),
      }),
    });

    export const { useGetItemsQuery } = myApi;
    ```

4. **Fetch data in a React component**:
    ```javascript
    import React from "react";
    import { useGetItemsQuery } from "../services/myApi";

    const ItemsList = () => {
      const { data = [], error, isLoading } = useGetItemsQuery();

      if (isLoading) return <div>Loading...</div>;
      if (error) return <div>Error loading items</div>;

      return (
        <ul>
          {data.map(item => (
            <li key={item.id}>{item.name}</li>
          ))}
        </ul>
      );
    };

    export default ItemsList;
    ```

5. **Render your component in Rails**:
    ```erb
    <%= react_component("ItemsList") %>
    ```

   You’re now fetching data in React, all within your Rails application! 🎊

---

### **5️⃣ Integrating GraphQL for Flexible APIs**

GraphQL works seamlessly with Rails and React, giving you flexibility in fetching data:

1. **Add the GraphQL gem** to your Rails app:
    ```ruby
    gem 'graphql'
    ```
2. **Install the gem**:
    ```bash
    bundle install
    rails generate graphql:install
    ```
3. **Define a GraphQL query** in `app/graphql/types/query_type.rb`:
    ```ruby
    field :items, [Types::ItemType], null: false

    def items
      Item.all
    end
    ```
   
4. **Fetch data with Apollo Client in React**:
   Install Apollo Client:
    ```bash
    npm install @apollo/client graphql
    ```

   Set up Apollo Client:
    ```javascript
    import { ApolloClient, InMemoryCache, ApolloProvider } from "@apollo/client";

    const client = new ApolloClient({
      uri: "/graphql",
      cache: new InMemoryCache(),
    });

    export default client;
    ```

   Use it in a React component:
    ```javascript
    import React from "react";
    import { gql, useQuery } from "@apollo/client";

    const GET_ITEMS = gql`
      query GetItems {
        items {
          id
          name
        }
      }
    `;

    const ItemList = () => {
      const { loading, error, data } = useQuery(GET_ITEMS);

      if (loading) return <p>Loading...</p>;
      if (error) return <p>Error :(</p>;

      return (
        <ul>
          {data.items.map(({ id, name }) => (
            <li key={id}>{name}</li>
          ))}
        </ul>
      );
    };

    export default ItemList;
    ```

---

### **6️⃣ Wrapping Up: Benefits of Integrating React with Rails**

Integrating React into Rails provides:
- **Interactivity and Real-time Data**: React’s dynamic nature lets you build highly interactive interfaces.
- **Data Fetching Power**: RTK Query and GraphQL make data fetching simple, efficient, and flexible.
- **Reusability**: Modular components make it easy to reuse code and stay DRY.

With this setup, you get the best of both worlds: Rails’ structure and React’s flexibility. 👏 Happy coding!
