---
layout: home
title: "Deploying Rails on AWS EC2"
date: 2025-11-19
categories: "Software Development"
tags: [Ruby On Rails, Software Development, Software Developer, Programming, Deployment, EC2, AWS]
image: 'https://github.com/user-attachments/assets/e1f30799-be3f-448f-8b2b-f1d0b69ed5b2'
---

# 🚀 **Deploying Rails on AWS EC2: The *No-Mistake* Step-by-Step Guide**

### The Ultimate Beginner-to-Pro Deployment Handbook 💡🔧

Deploying a Ruby on Rails app to AWS EC2 can be confusing — servers, dependencies, secrets, networking, ports… one wrong step and everything crashes.
This guide removes all guesswork and gives you a **bulletproof, production-grade deployment flow**.
Let’s do it — mistake-free! 💯✨

<img width="1024" height="1536" alt="ChatGPT Image Nov 19, 2025, 05_33_50 PM" src="https://github.com/user-attachments/assets/e1f30799-be3f-448f-8b2b-f1d0b69ed5b2" />

---

# 🧠 **Concepts You MUST Know Before Deploying**

## 🔸 **1. AWS EC2 (Elastic Compute Cloud)**

It’s your **virtual Linux machine** in the cloud. Think of it as a laptop running 24x7, where your Rails app lives.

## 🔸 **2. SSH (Secure Shell)**

A secure way to connect to your EC2 instance using your terminal.

## 🔸 **3. Nginx**

A powerful **web server** that receives browser requests → forwards to Rails.

## 🔸 **4. Passenger / Puma**

The **Rails application server**.

* **Passenger** integrates well with Nginx (classic choice).
* **Puma** is fast and modern (default Rails choice).

## 🔸 **5. RDS (Optional)**

Managed database service for PostgreSQL/MySQL.

## 🔸 **6. Deployment Directory Structure**

Example:

```
/var/www/myapp
├── current     # Active version
├── shared      # Logs, uploads, secrets
└── releases    # Old deployments
```

---

# 📦 **Step 1: Launch Your EC2 Instance**

### 🔧 Recommended Setup

* **AMI:** Ubuntu 22.04 LTS
* **Instance type:** t2.micro (free-tier)
* **Storage:** 20GB
* **Security groups:**

  * SSH → port 22
  * HTTP → port 80
  * HTTPS → port 443

### 📝 Generate a key pair

Download `myapp.pem` — **don’t lose it!**
You’ll use it to SSH into the server.

---

# 🔗 **Step 2: SSH Into the Server**

```bash
chmod 400 myapp.pem
ssh -i myapp.pem ubuntu@your-public-ip
```

If connected, you're inside your cloud machine 🙌

---

# 🛠️ **Step 3: Install Dependencies**

## 📌 Update packages

```bash
sudo apt update && sudo apt upgrade -y
```

## 📌 Install Git, cURL, Nginx

```bash
sudo apt install -y git curl gnupg nginx build-essential
```

---

# 🔥 **Step 4: Install Ruby Using rbenv**

```bash
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
rbenv init
```

Install Ruby-build:

```bash
git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
sudo apt-get install -y libssl-dev libreadline-dev zlib1g-dev
```

Install Ruby:

```bash
rbenv install 3.3.0
rbenv global 3.3.0
```

Install Bundler:

```bash
gem install bundler
```

---

# 🗄️ **Step 5: Install and Configure PostgreSQL (or MySQL)**

### Install PostgreSQL

```bash
sudo apt install postgresql postgresql-contrib -y
```

Create user & database:

```bash
sudo -u postgres createuser --interactive
sudo -u postgres createdb myapp_production
```

Set password:

```bash
sudo -u postgres psql
ALTER USER username WITH PASSWORD 'your-password';
```

---

# 📁 **Step 6: Clone Your Rails App**

```bash
cd /var/www
sudo mkdir myapp
sudo chown ubuntu:ubuntu myapp
cd myapp
git clone https://github.com/your/repo.git .
```

Install gems:

```bash
bundle install
```

Set up secrets:

```bash
EDITOR="nano" bin/rails credentials:edit
```

---

# 🗃️ **Step 7: Precompile Assets & Migrate DB**

```bash
RAILS_ENV=production bin/rails assets:precompile
RAILS_ENV=production bin/rails db:migrate
```

---

# 🔥 **Step 8: Install & Configure Passenger + Nginx (or Puma)**

## Install Passenger

```bash
sudo apt install -y dirmngr gnupg
sudo sh -c 'curl -sS https://oss-binaries.phusionpassenger.com/apt/passenger/$(lsb_release -sc) \
 | sudo tee /etc/apt/sources.list.d/passenger.list'
sudo apt update
sudo apt install -y libnginx-mod-http-passenger
```

Check installation:

```bash
passenger-config validate-install
```

---

# 🌐 **Step 9: Configure Nginx for Rails**

Edit configuration:

```bash
sudo nano /etc/nginx/sites-available/myapp
```

Paste this:

```
server {
    listen 80;
    server_name your-server-ip;

    root /var/www/myapp/public;

    passenger_enabled on;
    passenger_ruby /home/ubuntu/.rbenv/shims/ruby;

    client_max_body_size 20m;
}
```

Enable site:

```bash
sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```

If you see your app in the browser → 🎉 success!

---

# ☁️ **Step 10: Setup Firewall (UFW)**

```bash
sudo ufw allow OpenSSH
sudo ufw allow 'Nginx Full'
sudo ufw enable
```

---

# 🎉 **Step 11: Deploy Updates Without Errors (The Safe Way!)**

Pull updates:

```bash
git pull origin main
bundle install
RAILS_ENV=production rails db:migrate
rails assets:precompile
sudo systemctl restart nginx
```

---

# 📊 **High-Level Deployment Architecture Diagram**

```
User → Nginx → Passenger/Puma → Rails App → PostgreSQL (local or RDS)
```

---

# ⚡ **Tips to Improve Your Deployment**

### ⭐ 1. Use ENV variables instead of credentials

Store secrets with:

* AWS SSM Parameter Store
* dotenv gem
* Rails encrypted credentials

### ⭐ 2. Move DB to AWS RDS

Better scaling, backups, monitoring.

### ⭐ 3. Enable HTTPS with Let’s Encrypt

Use Certbot:

```bash
sudo certbot --nginx
```

### ⭐ 4. Enable Auto-Restart for Rails

With Passenger, it restarts automatically after deployments.

### ⭐ 5. Create Deployment Script

Automate with Bash or GitHub Actions.

### ⭐ 6. Use CloudWatch for Monitoring

Track RAM, CPU, traffic.

### ⭐ 7. Use Load Balancer + Auto Scaling

For big applications.

---

# 🏁 **Final Words**

Deploying a Rails app on AWS EC2 is not hard — **it’s just a checklist**.
Follow the exact steps above and you’ll have a **production-grade, scalable, secure infrastructure** with zero mistakes. 🚀🔥
