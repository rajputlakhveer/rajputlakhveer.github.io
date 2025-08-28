---
layout: home
title: "Setting an Automated Pipeline"
date: 2025-08-28
categories: "DevOps"
tags: [DevOps, Pipeline, CICD, Jenkins, CircleCI, GitLab]
image: 'https://github.com/user-attachments/assets/490dd352-b4b3-451f-b50b-5ccea11b324a'
---

# 🚀 **Setting an Automated Pipeline: A Step-by-Step Guide** 🛠️

In today’s fast-paced development world, automation is the key to delivering high-quality software faster and with fewer errors. An **automated pipeline** ensures that code goes through continuous integration (CI) and continuous delivery (CD) smoothly, without manual intervention. In this blog, we’ll explore essential tools, their features, and step-by-step instructions to set up an automated pipeline with **test case execution** and **Rubocop code checks**. ✅

<img width="960" height="570" alt="DevOps-pipeline (1)" src="https://github.com/user-attachments/assets/490dd352-b4b3-451f-b50b-5ccea11b324a" />

---

## 🔑 What is an Automated Pipeline?

An automated pipeline is a set of processes that take your code from development to production in an automated manner. It typically includes stages like:

* **Code build** 🏗️
* **Automated testing** 🧪
* **Static code analysis** 🔍
* **Deployment** 🚢

This reduces manual errors and keeps the development cycle consistent.

---

## 🛠️ Tools for Automated Pipelines

### 1. **Jenkins** 🤖

* **Features:**

  * Open-source automation server
  * Supports plugins for almost any tool
  * Highly customizable for CI/CD
* **Usage in pipeline:**

  * Build jobs
  * Run automated tests
  * Integrate with Rubocop for Ruby code linting

### 2. **GitHub Actions** ⚡

* **Features:**

  * Built into GitHub
  * Easy YAML-based workflow definitions
  * Great for open-source and small-to-mid projects
* **Usage in pipeline:**

  * Trigger workflows on `push` or `pull_request`
  * Run RSpec tests
  * Run Rubocop checks

### 3. **GitLab CI/CD** 🦊

* **Features:**

  * Fully integrated with GitLab repositories
  * Easy-to-write `.gitlab-ci.yml`
  * Built-in Docker support
* **Usage in pipeline:**

  * Define stages: `test`, `lint`, `deploy`
  * Run jobs in isolated environments

### 4. **CircleCI** 🔄

* **Features:**

  * Cloud-based, easy integration with GitHub/Bitbucket
  * Fast parallel job execution
  * Great dashboards
* **Usage in pipeline:**

  * Build and test Ruby apps
  * Run Rubocop as part of the lint stage

---

## ⚙️ Setting Up a Simple Pipeline (Example with GitHub Actions)

Let’s go step by step to set up a pipeline that:

1. Runs **RSpec test cases** 🧪
2. Runs **Rubocop checks** 🔍

### Step 1: Create a Workflow File

Inside your Rails app, create:

```bash
mkdir -p .github/workflows
nano .github/workflows/ci.yml
```

### Step 2: Define the Workflow

```yaml
name: CI Pipeline

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest

    services:
      postgres:
        image: postgres:13
        ports: [5432:5432]
        env:
          POSTGRES_USER: postgres
          POSTGRES_PASSWORD: password
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
    - uses: actions/checkout@v3

    - name: Set up Ruby
      uses: ruby/setup-ruby@v1
      with:
        ruby-version: 3.2
        bundler-cache: true

    - name: Install dependencies
      run: bundle install --jobs 4 --retry 3

    - name: Setup Database
      run: |
        cp config/database.yml.github config/database.yml
        bundle exec rails db:create db:schema:load

    - name: Run Tests (RSpec)
      run: bundle exec rspec

    - name: Run Rubocop
      run: bundle exec rubocop
```

---

## 🧪 Example Test Case Run

Suppose we have a simple **RSpec test**:

```ruby
# spec/models/user_spec.rb
require 'rails_helper'

describe User, type: :model do
  it "is invalid without a name" do
    user = User.new(name: nil)
    expect(user.valid?).to eq(false)
  end
end
```

When you push your code, GitHub Actions will automatically run this test. If the test fails ❌, the pipeline breaks. If it passes ✅, it continues to the Rubocop stage.

---

## 🔍 Example Rubocop Check

Rubocop ensures coding standards are followed. For example, if you wrote:

```ruby
def badMethodName
  puts "hello"
end
```

Rubocop will flag it as:

```
Naming/MethodName: Use snake_case for method names.
```

The pipeline will fail until corrected.

---

## 🎯 Bonus Tips for Perfect Pipelines

* ✅ **Use caching** to speed up builds (bundle caching in Ruby)
* ✅ **Run tests in parallel** for large projects
* ✅ **Add security scans** (e.g., Brakeman for Rails)
* ✅ **Use code coverage reports** (SimpleCov)

---

## 🚀 Wrapping Up

Automated pipelines save time, reduce errors, and enforce clean code. Whether you use **Jenkins, GitHub Actions, GitLab CI/CD, or CircleCI**, the core idea remains the same:

* **Test everything** 🧪
* **Lint everything** 🔍
* **Deploy smoothly** 🚢

By setting up a pipeline that runs **RSpec tests** and **Rubocop checks**, you ensure your Rails app stays reliable and clean. 💎
