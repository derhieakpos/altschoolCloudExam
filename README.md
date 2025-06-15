# Dynamic Startup Landing Page

This project showcases a deployment of a dynamic landing page using Nginx.

## 🛠️ Tech Stack
- Ubuntu 24.04.2 LTS (AWS EC2)
- Nginx (Reverse Proxy)
- TailwindCSS
- Node.js and Express

## 🚀 Setup Steps
1. Provision EC2 Ubuntu server
. Log into AWS Console
Visit: https://console.aws.amazon.com
Sign in with your AWS credentials.

. Launch a New Instance
Click Launch Instance.

Fill in the following:
a. Name and Tags
Name: my-ec2-instance (or any custom name)

b. Application and OS Image
Choose Ubuntu AMI (HVM), SSD Volume Type

c. Instance Type
Choose t2.micro (Free Tier eligible)

d. Key Pair (login)
Select an existing key pair or create a new one to access the instance via SSH.

e. Network Settings
Allow SSH (port 22) and HTTP (port 80) and HTTPS (port 443).

f. Storage
Default (8 GB) is fine unless you have specific requirements.

. Launch the Instance
Review your settings and click Launch Instance.
Wait for it to enter the running state.

. Connect to Your EC2 Instance
Go to Instances in EC2 dashboard.

Select your instance.
Click Connect > choose SSH.
Use the provided command in your terminal:
ssh -i "exam.pem" ubuntu@ec2-54-171-110-51.eu-west-1.compute.amazonaws.com
To install webserver
update the library

2. Install Node.js & Nginx
##
        sudo apt install nginx -y
        curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
        sudo apt install -y nodejs

3. Deploy `index.html` and `server.js`

##
        npm init -y
        npm install express
        node server.js

4. Configure Nginx reverse proxy
##
        sudo nano /etc/nginx/sites-available/myapp


5. Save the Nginx and the server.js configuration files

##
        server {
            listen 80;
            server_name localhost;
            
            location / {
                proxy_pass http://localhost:3000;
                proxy_http_version 1.1;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                }
        }

##
        const express = require('express');
        const path = require('path');
        const app = express();
        const PORT = 3000;

        // Serve static files from the "public" folder
        app.use(express.static(path.join(__dirname, 'public')));

        app.listen(PORT, () => {
            console.log(`Server running on http://localhost:${PORT}`);
            });


6. Enable the config:

##
        sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/
        sudo nginx -t
        sudo systemctl restart nginx

## 🌍 Live Demo
Public IP: http://54.171.110.51:3000/

## 📸 Screenshot
![Image](<Screenshot 2025-06-15 041654.png>)