---
layout: home
title: "Uploading Large Files in Ruby on Rails"
date: 2024-12-10
categories: "Ruby On Rails"
tags: [Ruby On Rails, Large File Upload, AWS]
image: 'https://github.com/user-attachments/assets/4b01fa87-c2bc-427c-a59f-a9892cd08bbd'
---

# 🚀 Uploading Large Files in Ruby on Rails: A Complete Guide

Managing large file uploads can be challenging in web development, especially when working with Ruby on Rails. But don’t worry – in this blog, we’ll explore various options to handle large file uploads efficiently, with examples to make it crystal clear. Let’s dive in! 🌊

![send_large_files](https://github.com/user-attachments/assets/4b01fa87-c2bc-427c-a59f-a9892cd08bbd)

---

## 🌟 Why Large File Uploads Are Challenging

Large file uploads often require:
- Efficient memory usage.
- Handling timeouts.
- Secure file handling.
- Compatibility with cloud storage.

Thankfully, Ruby on Rails provides several options to handle these challenges. Let’s explore them one by one! 🕵️‍♂‍

---

## 🛠 Option 1: **Direct Uploads to Cloud Storage**

Direct uploads offload the burden from your Rails server by uploading files straight to cloud storage like AWS S3, GCS, or Azure Blob Storage.

### **Implementation with Active Storage**
1. Add Active Storage to your app:
   ```bash
   rails active_storage:install
   rails db:migrate
   ```

2. Configure cloud storage in `config/storage.yml`:
   ```yaml
   amazon:
     service: S3
     access_key_id: <%= ENV["AWS_ACCESS_KEY_ID"] %>
     secret_access_key: <%= ENV["AWS_SECRET_ACCESS_KEY"] %>
     region: <%= ENV["AWS_REGION"] %>
     bucket: <%= ENV["AWS_BUCKET"] %>
   ```

3. Enable direct uploads in the form:
   ```ruby
   <%= form_with(model: @file, url: files_path, method: :post, multipart: true) do |form| %>
     <%= form.file_field :attachment, direct_upload: true %>
     <%= form.submit "Upload" %>
   <% end %>
   ```

### **Benefits**
- 🚀 **Speed**: Files don’t pass through your server.
- 🌐 **Scalability**: Works seamlessly with large files.
- 🔐 **Security**: Uses signed URLs for secure uploads.

---

## ⚙️ Option 2: **Chunked File Uploads**

Chunked uploads split large files into smaller parts, uploading them sequentially or in parallel. This reduces memory usage and minimizes upload failures.

### **Implementation with JavaScript**
1. Use a library like [FineUploader](https://fineuploader.com/) or [Dropzone.js](https://www.dropzone.dev/).

2. Create an endpoint in Rails:
   ```ruby
   class UploadsController < ApplicationController
     def create
       uploaded_file = params[:file]
       File.open(Rails.root.join('uploads', uploaded_file.original_filename), 'wb') do |file|
         file.write(uploaded_file.read)
       end
       render json: { message: "File uploaded successfully!" }, status: :ok
     end
   end
   ```

3. Configure the frontend to handle chunks:
   ```javascript
   const uploader = new FineUploader({
     request: {
       endpoint: "/uploads",
     },
     chunking: {
       enabled: true,
       partSize: 2 * 1024 * 1024, // 2 MB
     },
   });
   ```

### **Benefits**
- 🛡 **Resilience**: Can resume uploads if interrupted.
- 🧵 **Memory Efficiency**: Small chunks reduce memory load.

---

## 🌐 Option 3: **Background Processing**

Use background jobs for processing large uploads asynchronously. This ensures the server remains responsive during uploads.

### **Implementation with Sidekiq**
1. Add Sidekiq to your app:
   ```bash
   bundle add sidekiq
   ```

2. Create a background job:
   ```ruby
   class FileUploadJob < ApplicationJob
     queue_as :default

     def perform(file)
       # Process the file (e.g., move to storage, resize images)
     end
   end
   ```

3. Enqueue the job after file upload:
   ```ruby
   def create
     file = params[:file]
     FileUploadJob.perform_later(file.path)
     render json: { message: "File will be processed soon!" }, status: :accepted
   end
   ```

### **Benefits**
- 🏗 **Scalability**: Handles heavy processing without blocking the main thread.
- 🕒 **Deferred Processing**: Keeps the user interface responsive.

---

## 📦 Option 4: **Using External Gems**

Some gems make handling large uploads a breeze. Let’s explore a popular one:

### **Shrine**
[Shrine](https://shrinerb.com/) is a flexible and lightweight file upload solution.

1. Add Shrine to your Gemfile:
   ```ruby
   gem 'shrine'
   ```

2. Set up the uploader:
   ```ruby
   class FileUploader < Shrine
     plugin :presign_endpoint
   end
   ```

3. Enable presigned uploads:
   ```ruby
   class UploadsController < ApplicationController
     def presign
       presign_data = FileUploader.presign("cache")
       render json: presign_data
     end
   end
   ```

### **Benefits**
- 🧩 **Modular**: Highly customizable.
- 📊 **Detailed Support**: Supports multiple storage backends.

---

## 💡 Pro Tips for Large File Uploads

1. **Optimize File Size**: Encourage users to compress files before upload.
2. **Use CDN**: Deliver files through a CDN for faster access.
3. **Implement Progress Indicators**: Keep users informed with upload progress bars.

---

## 🎉 Conclusion

Uploading large files in Ruby on Rails can be made efficient with the right approach. Whether you choose direct uploads, chunking, background processing, or an external gem like Shrine, Rails has you covered. 🚀

Got questions or suggestions? Drop a comment below! 💬 Happy coding! 👨‍💻👩‍💻

