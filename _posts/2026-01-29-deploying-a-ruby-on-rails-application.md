---
layout: home
title: "Deploying a Ruby on Rails Application"
date: 2026-01-29
categories: "Software Engineer"
tags: [Ruby On Rails, Deployment, DevOps, Software Engineer, Application, Production, Programming]
image: 'https://github.com/user-attachments/assets/ac89b3af-2fb5-45a9-801d-c903a4a820dd'
---

# 🚀 Deploying a Ruby on Rails Application Like a Pro (Step-by-Step Guide) 🌍🔥

*From Localhost to Live Server with Domains, Routing, Production Setup & Optimization*

Deploying a Ruby on Rails application is one of the most powerful milestones for any developer.
It’s the moment your project goes from:

💻 *“Works on my machine”* → 🌍 *Available to the whole world*

<img width="1024" height="1536" alt="ChatGPT Image Jan 29, 2026, 10_02_13 PM" src="https://github.com/user-attachments/assets/ac89b3af-2fb5-45a9-801d-c903a4a820dd" />

In this guide, you’ll learn:

✅ Every deployment step
✅ Production vs Development separation
✅ Domain + Routing basics
✅ Best optimization techniques
✅ Real examples + pro-level practices

Let’s begin! 🚀

---

# 🏗️ 1. What Does Deployment Mean in Rails?

Deployment means:

* Moving your Rails app from your laptop
* To a live server (AWS, DigitalOcean, Render, etc.)
* Configuring it for production users

A deployed Rails app includes:

🌐 Web Server (Nginx)
⚙️ App Server (Puma)
🗄️ Database (PostgreSQL/MySQL)
🔐 Environment Variables
📦 Assets + Optimization

---

# 🧑‍💻 2. Prepare Your Rails App for Production

Before deploying, your Rails app must be production-ready.

---

## ✅ Use PostgreSQL (Recommended)

Rails apps in production almost always use PostgreSQL.

Update `Gemfile`:

```ruby
gem "pg"
```

Run:

```bash
bundle install
```

Update database config:

```yaml
production:
  adapter: postgresql
  encoding: unicode
  database: myapp_production
```

---

## ✅ Set Rails Environment Correctly

Rails has environments:

* development (local)
* test
* production (live)

Rails automatically uses:

```bash
RAILS_ENV=production
```

in deployment.

---

# 🌍 3. Choose a Deployment Server

Popular options:

| Platform                 | Best For                 |
| ------------------------ | ------------------------ |
| AWS EC2 ☁️               | Full control, scalable   |
| Render 🚀                | Easiest Rails deployment |
| DigitalOcean 🌊          | Affordable VPS           |
| Heroku (limited free) 🟣 | Beginner-friendly        |

We’ll explain deployment using **AWS EC2 + Nginx + Puma** (most professional setup).

---

# ☁️ 4. Setup Server (AWS EC2)

---

## ✅ Launch an EC2 Instance

Steps:

1. Go to AWS Console
2. Launch Instance
3. Select Ubuntu 22.04
4. Enable port:

* 22 (SSH)
* 80 (HTTP)
* 443 (HTTPS)

Download `.pem` key.

---

## 🔐 Connect to Server via SSH

```bash
ssh -i mykey.pem ubuntu@your-server-ip
```

Now you are inside your cloud server 🎉

---

# ⚙️ 5. Install Required Dependencies

Update server packages:

```bash
sudo apt update && sudo apt upgrade -y
```

Install essentials:

```bash
sudo apt install git curl build-essential -y
```

---

## ✅ Install Ruby

Use `rbenv`:

```bash
sudo apt install rbenv -y
rbenv install 3.2.2
rbenv global 3.2.2
```

Check:

```bash
ruby -v
```

---

## ✅ Install Rails

```bash
gem install rails
rails -v
```

---

## ✅ Install Node.js + Yarn (Assets)

Rails needs JS runtime:

```bash
sudo apt install nodejs yarn -y
```

---

# 🗄️ 6. Setup Database (PostgreSQL)

Install PostgreSQL:

```bash
sudo apt install postgresql postgresql-contrib -y
```

Create DB user:

```bash
sudo -u postgres createuser myappuser -s
sudo -u postgres psql
```

Set password:

```sql
ALTER USER myappuser WITH PASSWORD 'password';
```

Exit:

```bash
\q
```

---

# 📦 7. Upload Rails Application Code

Clone from GitHub:

```bash
git clone https://github.com/yourusername/myapp.git
cd myapp
```

Install gems:

```bash
bundle install
```

---

# 🔐 8. Configure Environment Variables (Secrets)

Never hardcode secrets like:

* API keys
* DB passwords
* Rails master key

Use:

```bash
EDITOR=nano rails credentials:edit
```

Or export variables:

```bash
export RAILS_MASTER_KEY=yourkey
export DATABASE_URL=postgres://...
```

---

# ⚙️ 9. Run Production Setup Commands

---

## ✅ Precompile Assets

Rails compiles CSS/JS for production:

```bash
RAILS_ENV=production rails assets:precompile
```

---

## ✅ Migrate Database

```bash
RAILS_ENV=production rails db:migrate
```

---

## ✅ Seed Data (Optional)

```bash
RAILS_ENV=production rails db:seed
```

---

# 🚀 10. Setup Puma App Server

Rails uses Puma in production.

Start Puma:

```bash
bundle exec puma -e production
```

But in real deployments, Puma runs as a service.

Create:

```bash
sudo nano /etc/systemd/system/puma.service
```

Example:

```ini
[Unit]
Description=Puma Rails Server
After=network.target

[Service]
User=ubuntu
WorkingDirectory=/home/ubuntu/myapp
ExecStart=/home/ubuntu/.rbenv/shims/bundle exec puma -e production
Restart=always

[Install]
WantedBy=multi-user.target
```

Enable:

```bash
sudo systemctl start puma
sudo systemctl enable puma
```

---

# 🌐 11. Setup Nginx Reverse Proxy

Install Nginx:

```bash
sudo apt install nginx -y
```

Configure:

```bash
sudo nano /etc/nginx/sites-available/myapp
```

Example config:

```nginx
server {
    listen 80;
    server_name example.com www.example.com;

    root /home/ubuntu/myapp/public;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
    }
}
```

Enable site:

```bash
sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled
sudo systemctl restart nginx
```

Now your Rails app is live 🚀

---

# 🌍 12. Domain + Routing Explained

---

## ✅ How Domain Works

When a user visits:

```
www.example.com
```

DNS maps domain → server IP.

---

### Steps:

1. Buy domain from GoDaddy/Namecheap
2. Add DNS Record:

| Type  | Name | Value       |
| ----- | ---- | ----------- |
| A     | @    | Server IP   |
| CNAME | www  | example.com |

Within minutes, domain points to your Rails server.

---

## ✅ Routing in Rails

Rails routing decides:

URL → Controller Action

Example:

```ruby
get "/about", to: "pages#about"
```

User visits:

```
example.com/about
```

Rails runs:

```ruby
PagesController#about
```

---

# ⚡ 13. Optimization Techniques for Production

Deploying is not enough. Optimize it! 🚀

---

## ✅ Enable Caching

In `production.rb`:

```ruby
config.action_controller.perform_caching = true
```

---

## ✅ Use Background Jobs

Use Sidekiq for heavy tasks:

```ruby
gem "sidekiq"
```

Example:

```ruby
EmailJob.perform_later(user.id)
```

---

## ✅ Use CDN for Assets

Serve images & JS faster via Cloudflare/AWS CloudFront.

---

## ✅ Database Indexing

Add indexes:

```ruby
add_index :users, :email
```

Speeds up queries massively ⚡

---

## ✅ Use SSL (HTTPS)

Install Certbot:

```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx
```

Now your app is secure 🔒

---

# 🧪 14. Separating Development vs Production Properly

Rails automatically separates environments:

| Feature    | Development | Production     |
| ---------- | ----------- | -------------- |
| Debug logs | ✅ Yes       | ❌ No           |
| Caching    | ❌ Off       | ✅ On           |
| Assets     | Live reload | Precompiled    |
| Errors     | Full trace  | Friendly pages |

Use:

```bash
rails s
```

for dev

Use:

```bash
RAILS_ENV=production rails s
```

for prod

---

# 🎯 Final Deployment Checklist ✅

✔ App runs locally
✔ PostgreSQL configured
✔ Secrets stored safely
✔ Assets precompiled
✔ Puma service running
✔ Nginx routing works
✔ Domain connected
✔ HTTPS enabled
✔ Production optimized

---

# 🌟 Conclusion: Rails Deployment Mastery

Deploying Rails is a superpower 💎
Once you master it, you can launch:

🚀 SaaS Products
🌍 Startups
📦 APIs
🛒 E-commerce apps
📱 Mobile backends

Rails is built for production success!
