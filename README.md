# Node.js CI/CD with GitHub Actions

This project demonstrates how to create a simple Node.js application with Express, serve a static web page, and set up a GitHub Actions workflow for CI/CD.

## Features
- Caching with actions/cache
- Unit testing with Jest
- Linting with ESLint
- Deployment-ready (Heroku placeholder)

---

## Setup Instructions

### 1. Clone and install
```bash
git clone https://github.com/YOUR_USERNAME/node-app-ci-cd.git
cd node-app-ci-cd
npm install
```

### 2. Run locally
```bash
npm start
```

### 3. Run tests
```bash
npm test
```

### 4. Lint your code
```bash
npm run lint
```

---

## GitHub Actions Workflow
- Workflow file: `.github/workflows/node.js.yml`
- Auto runs on `push` or `pull_request` to `main`

---

## Deployment
You can deploy to:
- **Heroku** (see workflow comments for setup)
- **AWS** or **Vercel** (manual steps required)

## Deploying to AWS:

✅ Prerequisites
AWS account

EC2 key pair (.pem file)

Node.js app (your zipped project)

Security group with ports 22 (SSH) and 3000 (Node.js app) open

1. Launch an EC2 Instance
Go to AWS EC2 Console → Launch instance

Choose Amazon Linux 2 or Ubuntu

Select an instance type (e.g., t2.micro)

Choose or create a key pair

Configure security group:

Allow SSH (22) from your IP

Allow Custom TCP (3000) from your IP or Anywhere (for testing)

Launch the instance

2. Connect to the Instance via SSH

```
chmod 400 your-key.pem
ssh -i your-key.pem ec2-user@your-ec2-public-ip     # for Amazon Linux
# OR
ssh -i your-key.pem ubuntu@your-ec2-public-ip       # for Ubuntu

```

3. Install Node.js and Git

```
sudo apt update -y
sudo apt install -y nodejs npm git unzip

```

4. Upload Your App
Option A: Upload ZIP via SCP
From your local terminal:

`scp -i your-key.pem node-app-ci-cd-updated.zip ec2-user@your-ec2-public-ip:~`

Option B: Clone from GitHub

```
git clone https://github.com/YOUR_USERNAME/node-app-ci-cd.git
cd node-app-ci-cd

```

5. Setup and Run App

```
unzip node-app-ci-cd-updated.zip
cd node-app-ci-cd
npm install
npm start

```

6. Access the App
Go to:

`http://<EC2-PUBLIC-IP>:3000`

You should see:

“Welcome to My Node.js App!”

✅ Optional: Keep App Running with pm2
Install pm2 globally:

```
sudo npm install -g pm2
pm2 start server.js
pm2 startup
pm2 save

```

✅ Optional: Use NGINX as Reverse Proxy
To serve the app via http://<your-ec2-ip> (port 80):

1. Install NGINX:

`sudo apt install nginx`  # or sudo yum install nginx

2. Configure:

`sudo nano /etc/nginx/sites-available/default  # or /etc/nginx/nginx.conf`

Replace or add: 

```
server {
    listen 80;
    server_name _;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}

```

3. Restart NGINX:

`sudo systemctl restart nginx`

Done.

### You’ve manually deployed your Node.js app to AWS EC2.


yyy