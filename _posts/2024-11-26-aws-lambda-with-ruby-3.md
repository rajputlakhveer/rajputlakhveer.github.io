---
layout: home
title: "AWS Lambda with Ruby 3"
date: 2024-11-26
categories: "Ruby"
tags: [AWS Lambda, AWS, Ruby, Ruby On Rails, Lambda]
image: 'https://github.com/user-attachments/assets/1442370e-d7fb-4097-b896-604bb10daf21'
---

# 🚀 Supercharge Your Serverless Apps with AWS Lambda and Ruby 3.2! 🌟  

Ruby developers, rejoice! 🎉 AWS Lambda now supports **Ruby 3.2**, making it easier than ever to build scalable, cost-effective applications. In this blog, we'll:  

1. Unpack what AWS Lambda is 🤔  
2. Walk through an example using Ruby 3.2 💻  
3. Highlight its usefulness and real-world applications 🌍  

![lambda](https://github.com/user-attachments/assets/1442370e-d7fb-4097-b896-604bb10daf21)

---

## 🌟 **What is AWS Lambda?**  

AWS Lambda is a **serverless compute service** that lets you run code without managing servers. Instead of provisioning infrastructure, you upload your code, and AWS handles everything — scaling, patching, and availability.  

Key benefits of Lambda include:  

- **Pay-as-you-go pricing** 💰  
- **Automatic scaling** for traffic spikes 📈  
- Seamless integration with AWS services 🌐  

With **Ruby 3.2** support, you can now leverage the latest Ruby features to write high-performance, serverless applications. 🎊  

---

## 🛠️ **Building a URL Shortener with AWS Lambda and Ruby 3.2**  

Let’s create a simple **URL shortener**. When a user sends a long URL, our Lambda function will generate a short URL and return it.  

### Step 1: **Set Up Your Environment**  

Ensure you have AWS CLI and AWS SAM CLI installed. These tools make it easy to test and deploy Lambda functions.  

```bash  
# Install AWS CLI  
pip install awscli --upgrade  

# Install SAM CLI  
brew install aws/tap/aws-sam-cli  
```  

### Step 2: **Initialize a Ruby 3.2 Project**  

Create a new SAM application for Ruby 3.2:  

```bash  
sam init --runtime ruby3.2 --name ruby-url-shortener  
```  

Navigate to the project directory:  

```bash  
cd ruby-url-shortener  
```  

### Step 3: **Write the Lambda Function**  

Open `hello_world/app.rb` and update the `lambda_handler` to process URLs.  

```ruby  
require 'securerandom'  
require 'json'  

# Hash to store short URLs in memory (for demonstration purposes)  
SHORT_URLS = {}  

def lambda_handler(event:, context:)  
  body = JSON.parse(event['body']) rescue {}  
  long_url = body['url']  

  if long_url  
    short_code = SecureRandom.hex(3)  
    SHORT_URLS[short_code] = long_url  

    {  
      statusCode: 200,  
      body: JSON.generate({ short_url: "https://short.ly/#{short_code}" })  
    }  
  else  
    {  
      statusCode: 400,  
      body: JSON.generate({ error: "Please provide a valid URL." })  
    }  
  end  
end  
```  

### Step 4: **Test Locally**  

Use the SAM CLI to simulate a request to your Lambda function:  

```bash  
sam local invoke "HelloWorldFunction" -e events/event.json  
```  

Create a test event (`events/event.json`):  

```json  
{  
  "body": "{\"url\": \"https://example.com/very-long-url\"}"  
}  
```  

### Step 5: **Deploy the Function to AWS**  

Deploy your Lambda function:  

```bash  
sam deploy --guided  
```  

During the deployment process, set up an API Gateway so your Lambda can receive HTTP requests. After deployment, you’ll receive an endpoint URL to test your function.  

---

## 🎯 **Why Use AWS Lambda with Ruby 3.2?**  

1. **Scalability** 📈  
   Automatically handles any load, from a single request to thousands per second.  

2. **Cost-Effectiveness** 💰  
   Pay only for the compute time you use, making it ideal for low-traffic or unpredictable workloads.  

3. **Event-Driven Architecture** 📩  
   Trigger Lambda functions using events like HTTP requests, S3 uploads, or database changes.  

4. **Modern Ruby Features** 💎  
   Take advantage of Ruby 3.2 improvements like pattern matching, better performance, and memory optimizations.  

5. **No Server Maintenance** 🛠️  
   Focus on your code while AWS takes care of the infrastructure.  

---

## 🌍 **Applications of AWS Lambda and Ruby 3.2**  

- **URL Shorteners** ✂️  
   As shown above, create short URLs for easy sharing.  

- **Image Processing** 🖼️  
   Resize or optimize images uploaded to an S3 bucket.  

- **Webhooks** 🔔  
   Handle incoming webhooks for services like Stripe or GitHub.  

- **Data Processing Pipelines** 📊  
   Process logs, analyze data, or transform files in real-time.  

- **Custom APIs** 🌐  
   Build robust RESTful or GraphQL APIs integrated with DynamoDB, S3, and other AWS services.  

---

## ✨ **Conclusion**  

AWS Lambda paired with Ruby 3.2 is a game-changer for Ruby developers. Whether you're building APIs, automating workflows, or processing data, Lambda simplifies the development process while keeping costs low.  

🔥 **Ready to take your Ruby apps to the next level?** Start exploring AWS Lambda today and build something amazing! 🚀  

---

💡 *Pro Tip*: Combine AWS Lambda with AWS services like API Gateway, S3, and DynamoDB to unlock even more possibilities! 😍  

---  

💬 **What will you build with AWS Lambda and Ruby 3.2?** Share your thoughts and projects in the comments below! 😊
