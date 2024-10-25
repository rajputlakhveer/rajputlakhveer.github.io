---
layout: home
title: "RTK Query with Ruby On Rails"
date: 2024-10-25
categories: "Ruby On Rails"
tags: [Ruby On Rails, Ruby, React, RTK Query, Toolkit, Redux]
image: 'https://github.com/user-attachments/assets/4dfb5a46-43d4-4b57-86d5-9e16f3793199'
---

### 🚀 Mastering RTK Query with Ruby on Rails: The Ultimate Guide 🚀

Are you ready to boost your Ruby on Rails app by making it more interactive and dynamic? With **RTK Query** (Redux Toolkit Query), integrating a fast, flexible, and efficient data-fetching layer becomes a breeze. Let’s dive into *what* RTK Query is, *why* it's worth using, and *how* you can implement it in your Rails app with ease. 💡

---

### 📌 What is RTK Query?

**RTK Query** is a powerful data-fetching and caching tool built on top of **Redux Toolkit**. It’s specially designed to simplify managing server state in front-end applications, particularly with **React**. Think of RTK Query as a built-in solution to handle **API requests** and automatically manage caching, background updates, and synchronization between client and server.

![s_118F96EA7D62F679A47C2AD439C93DC9203009C47C6538B354532E5ADE3FA23F_1676627146090_redux-graphs](https://github.com/user-attachments/assets/4dfb5a46-43d4-4b57-86d5-9e16f3793199)

### 🧩 Why Use RTK Query with Rails?

Combining RTK Query with Rails helps you create an app that is:
- **Efficient:** Automatically caches responses, reducing redundant API calls and improving app speed 🚀.
- **Synchronized:** Keeps client and server data in sync without complex code 🧘.
- **Simple:** Helps you manage loading, error, and refetching states with minimal configuration ✨.
- **Scalable:** Handles complex data requirements in larger apps with ease 🏗️.

---

### 📖 Setting Up RTK Query with Ruby on Rails

Here’s a step-by-step guide on how to integrate RTK Query with your Rails app. We’ll be working with a **React-Redux front-end** and a **Rails back-end API**. 

#### Prerequisites 🛠️

1. **Rails API**: Make sure your Rails backend has an API setup, e.g., `rails new myapp --api`.
2. **React-Redux App**: We’ll be using Redux and RTK Query in a React project.

---

### 🚦 Step 1: Build the Rails API

Let’s assume a basic Rails setup where we have a `Posts` resource that we want to interact with from our React app.

```ruby
# config/routes.rb
Rails.application.routes.draw do
  namespace :api do
    namespace :v1 do
      resources :posts, only: [:index, :show, :create, :update, :destroy]
    end
  end
end
```

```ruby
# app/controllers/api/v1/posts_controller.rb
module Api::V1
  class PostsController < ApplicationController
    def index
      posts = Post.all
      render json: posts
    end

    def show
      post = Post.find(params[:id])
      render json: post
    end

    # Add more actions as needed
  end
end
```

After setting up the Rails API, let’s test it to ensure it works by starting the server (`rails s`) and hitting the `/api/v1/posts` endpoint.

---

### 🚦 Step 2: Install RTK and RTK Query in React

First, install **Redux Toolkit** and **RTK Query** in your React app.

```bash
npm install @reduxjs/toolkit react-redux
```

---

### 🚦 Step 3: Configure Redux Store with RTK Query

Now, let’s set up a Redux store that uses RTK Query to handle data from the Rails API.

```javascript
// src/app/store.js
import { configureStore } from '@reduxjs/toolkit';
import { postsApi } from '../services/postsApi';

export const store = configureStore({
  reducer: {
    [postsApi.reducerPath]: postsApi.reducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(postsApi.middleware),
});
```

---

### 🚦 Step 4: Create RTK Query API Slice

Let’s create an **API slice** to define endpoints for fetching posts.

```javascript
// src/services/postsApi.js
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';

export const postsApi = createApi({
  reducerPath: 'postsApi',
  baseQuery: fetchBaseQuery({ baseUrl: 'http://localhost:3000/api/v1' }),
  endpoints: (builder) => ({
    getPosts: builder.query({
      query: () => '/posts',
    }),
    getPostById: builder.query({
      query: (id) => `/posts/${id}`,
    }),
  }),
});

export const { useGetPostsQuery, useGetPostByIdQuery } = postsApi;
```

Here, we define two endpoints:
1. `getPosts`: Fetches all posts from the Rails API.
2. `getPostById`: Fetches a specific post by ID.

---

### 🚦 Step 5: Fetch Data in React Components

Now we can use these endpoints directly in our components with RTK Query’s auto-generated hooks. 🎣

```javascript
// src/components/PostsList.js
import React from 'react';
import { useGetPostsQuery } from '../services/postsApi';

const PostsList = () => {
  const { data: posts, error, isLoading } = useGetPostsQuery();

  if (isLoading) return <p>Loading posts... 🔄</p>;
  if (error) return <p>Error loading posts! ❌</p>;

  return (
    <div>
      <h2>Posts List 📝</h2>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
};

export default PostsList;
```

With the `useGetPostsQuery` hook, RTK Query handles loading and error states for us automatically. We just need to define how we want to display the data in the component.

---

### 🚦 Step 6: Handling Mutations

For creating, updating, or deleting data, RTK Query provides *mutations*. Here’s an example of how to create a new post.

```javascript
// src/services/postsApi.js
// Add this in postsApi endpoints
addPost: builder.mutation({
  query: (newPost) => ({
    url: '/posts',
    method: 'POST',
    body: newPost,
  }),
}),
```

Now use it in a component:

```javascript
// src/components/AddPost.js
import React, { useState } from 'react';
import { useAddPostMutation } from '../services/postsApi';

const AddPost = () => {
  const [title, setTitle] = useState('');
  const [addPost, { isLoading }] = useAddPostMutation();

  const handleAddPost = async () => {
    await addPost({ title }).unwrap();
    setTitle('');
  };

  return (
    <div>
      <h2>Add a New Post 🆕</h2>
      <input
        type="text"
        value={title}
        onChange={(e) => setTitle(e.target.value)}
        placeholder="Enter post title"
      />
      <button onClick={handleAddPost} disabled={isLoading}>
        {isLoading ? 'Adding...' : 'Add Post'}
      </button>
    </div>
  );
};

export default AddPost;
```

---

### ✨ Wrapping Up

With RTK Query, we can:
- Simplify API integration without manually handling loading, success, or error states 🧹.
- Automatically cache data and refetch it when necessary ⚡.
- Scale API handling in your Rails and React projects like a pro 💪.

This setup not only optimizes performance but also keeps your app’s code clean and maintainable. So go ahead and integrate RTK Query with Rails and witness the ease and efficiency it brings! 🥳
