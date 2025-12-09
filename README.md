# Deploying-a-Next.js-App-on-a-VPS-pnpm-PM2-Nginx

# 🚀 Deploying a Next.js App on a VPS (pnpm + PM2 + Nginx)

This guide explains how to deploy a Next.js application on a VPS using Node.js, pnpm, PM2, and Nginx.

---

## 📌 Prerequisites
- Ubuntu VPS
- Domain name (recommended)
- Basic Linux command knowledge
- Git repository access

---

## 🛠 1. Connect to Your VPS via SSH
    ssh root@your_vps_ip

---

## 🔄 2. Update System & Install Dependencies
    sudo apt update && sudo apt upgrade -y
    sudo apt install -y git curl ufw nginx

---

## 💻 3. Install Node.js Using NVM
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
    source ~/.bashrc
    nvm install --lts

---

## 📦 4. Install pnpm
    npm install -g pnpm

---

## 🔄 5. Install PM2
    npm install -g pm2
    pm2 startup

---

## 🚀 6. Deploy Your Next.js Application

### 📥 Clone Repository
    cd /var/www
    git clone git@github.com:your_username/your_repo.git your_app_name
    cd your_app_name

### 📦 Install Dependencies
    pnpm install

### 🔨 Build the App
    pnpm build

### ▶ Start with PM2
    pm2 start pnpm --name "your_app" -- start
    pm2 save

---

## 🌐 7. Configure Nginx Reverse Proxy

### Create Nginx Config
    sudo nano /etc/nginx/sites-available/your_domain.conf

### Add the following configuration
    server {
        listen 80;
        server_name your_domain.com www.your_domain.com;

        location / {
            proxy_pass http://localhost:3000;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }

### Enable & Restart Nginx
    sudo ln -s /etc/nginx/sites-available/your_domain.conf /etc/nginx/sites-enabled/
    sudo rm /etc/nginx/sites-enabled/default
    sudo nginx -t
    sudo systemctl restart nginx

---

## 🔒 8. Enable SSL with Certbot

### Install Certbot
    sudo apt install -y certbot python3-certbot-nginx

### Add HTTPS
    sudo certbot --nginx -d your_domain.com -d www.your_domain.com

---

## 🎉 Deployment Complete

Your app is now:

✔ Running with pnpm  
✔ Managed by PM2  
✔ Served via Nginx  
✔ Secured with HTTPS  

---

## 🧰 Useful PM2 Commands

    pm2 list
    pm2 restart your_app
    pm2 stop your_app
    pm2 logs your_app
    pm2 delete your_app

---

## 📝 Notes
- Replace your_domain.com, your_repo, and your_app_name accordingly.
- Next.js default port is 3000.

---
