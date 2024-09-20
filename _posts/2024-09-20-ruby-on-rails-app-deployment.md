---
layout: home
title: "Ruby On Rails App Deployment"
date: 2024-09-20
categories: "Ruby on Rails"
tags: [Ruby, AWS, Ruby on Rails, EC2, Deployment]
image: ''
---

### Guide to Deploy a Ruby on Rails App Using AWS EC2

Deploying a Ruby on Rails (RoR) application using AWS EC2 can seem complex, but with the right guidance, it becomes manageable. This blog will take you step by step through the process, helping you get your RoR app up and running on AWS EC2. 💻🚀

### Step 1: **Set Up an AWS EC2 Instance**
First, create an EC2 instance that will act as the server to host your application.

1. **Log in to AWS**: Go to the [AWS Management Console](https://aws.amazon.com/console/) and log in.
2. **Launch an Instance**:
   - Navigate to **EC2** under the "Services" tab.
   - Click **Launch Instance**.
3. **Select AMI**: Choose **Ubuntu Server** (recommended for Rails deployments).
4. **Choose Instance Type**: Select `t2.micro` for a small app (free tier) or a larger instance for higher resource needs.
5. **Configure Instance**: 
   - Keep the default settings but ensure to configure security groups to allow HTTP, HTTPS, and SSH access.
6. **Add Storage**: The default 8GB is fine for small applications.
7. **Launch**: Click **Review and Launch**. You will need to create a key pair to securely access the instance.

🚨 **Pro Tip**: Make sure to download and keep your private key (`.pem` file) safe, as you’ll need it to SSH into the server.

### Step 2: **Connect to Your EC2 Instance**
Once your EC2 instance is up and running, you'll need to connect to it.

1. **Open Terminal** (or any SSH client).
2. **SSH into the Instance**:
   ```bash
   ssh -i path_to_your_pem_file.pem ubuntu@your_instance_ip
   ```
   Replace `path_to_your_pem_file.pem` with the path to your downloaded key and `your_instance_ip` with your EC2 instance’s public IP.

You’re now inside your server! 🖥️

### Step 3: **Install Ruby, Rails, and Dependencies**
With your EC2 instance connected, it's time to install Ruby and Rails.

1. **Update the system**:
   ```bash
   sudo apt-get update
   sudo apt-get upgrade
   ```
2. **Install RVM (Ruby Version Manager)**:
   ```bash
   sudo apt-get install curl gpg
   \curl -sSL https://get.rvm.io | bash -s stable
   ```
   After installation, load RVM:
   ```bash
   source ~/.rvm/scripts/rvm
   ```
3. **Install Ruby**:
   ```bash
   rvm install 3.1.2
   rvm use 3.1.2 --default
   ```
4. **Install Rails**:
   ```bash
   gem install rails
   ```
5. **Install Node.js and Yarn** (required by Rails for compiling JavaScript):
   ```bash
   sudo apt-get install -y nodejs
   sudo apt-get install -y yarn
   ```

### Step 4: **Install and Configure PostgreSQL**
For this guide, we will use PostgreSQL as the database.

1. **Install PostgreSQL**:
   ```bash
   sudo apt-get install postgresql postgresql-contrib libpq-dev
   ```
2. **Create a User and Database**:
   Switch to the PostgreSQL user:
   ```bash
   sudo -u postgres psql
   ```
   Create a user and set a password:
   ```sql
   CREATE USER myuser WITH PASSWORD 'mypassword';
   ALTER ROLE myuser CREATEDB;
   ```
   Then exit the prompt:
   ```sql
   \q
   ```

### Step 5: **Clone Your Rails Application**
Next, pull your Ruby on Rails application from your repository.

1. **Install Git**:
   ```bash
   sudo apt-get install git
   ```
2. **Clone Your App**:
   ```bash
   git clone https://github.com/username/your-rails-app.git
   cd your-rails-app
   ```

### Step 6: **Set Up Environment Variables**
For production apps, use a `.env` file or an environment manager like `dotenv`.

1. **Install dotenv**:
   ```bash
   gem install dotenv-rails
   ```
2. **Create a `.env` file** with your database credentials and secrets:
   ```bash
   touch .env
   ```

### Step 7: **Configure Rails for Production**
Edit your `config/database.yml` to use PostgreSQL with the credentials you created earlier.

```yaml
production:
  adapter: postgresql
  encoding: unicode
  database: your_database_name
  pool: 5
  username: your_database_user
  password: <%= ENV['DATABASE_PASSWORD'] %>
  host: localhost
```

Run the database migrations:
```bash
rails db:migrate RAILS_ENV=production
```

### Step 8: **Precompile Assets**
Before deploying, precompile the Rails assets:
```bash
rails assets:precompile RAILS_ENV=production
```

### Step 9: **Install a Web Server (Nginx + Puma)**
1. **Install Nginx**:
   ```bash
   sudo apt-get install nginx
   ```
2. **Install Puma**:
   Add Puma to your Gemfile:
   ```ruby
   gem 'puma'
   ```
   Then run:
   ```bash
   bundle install
   ```

3. **Configure Nginx**:
   Create an Nginx config file for your Rails app in `/etc/nginx/sites-available/your_app`:
   ```bash
   sudo nano /etc/nginx/sites-available/your_app
   ```
   Example configuration:
   ```nginx
   server {
     listen 80;
     server_name your_domain_or_ip;

     root /path_to_your_app/public;

     location / {
       proxy_pass http://localhost:3000;
       proxy_set_header Host $host;
       proxy_set_header X-Real-IP $remote_addr;
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
     }
   }
   ```
   Enable the site and restart Nginx:
   ```bash
   sudo ln -s /etc/nginx/sites-available/your_app /etc/nginx/sites-enabled/
   sudo systemctl restart nginx
   ```

### Step 10: **Start the Rails Application**
Finally, start the Rails application in production mode using Puma:
```bash
RAILS_ENV=production bundle exec puma -C config/puma.rb
```

### Step 11: **Access Your Application**
Open your browser and go to your EC2 instance's public IP to see your deployed Rails app!

🚀 **Congrats!** You've successfully deployed your Ruby on Rails app on AWS EC2. 🎉

---

Let me know if you need more tips or have questions in the comments! 😊
