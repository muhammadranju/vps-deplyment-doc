# 🚀 Complete MERN Stack Deployment Guide (A–Z)

### Ubuntu / AlmaLinux VPS

This document explains **how to deploy a full MERN Stack project** (MongoDB, Express, React/Next.js, Node.js) on a VPS **from scratch to production**.

You can reuse this guide for **any MERN project**.

---

## 🧱 TECH STACK OVERVIEW

- **Frontend:** React / Next.js (Bun or npm)
- **Backend:** Node.js + Express
- **Database:** MongoDB
- **Process Manager:** PM2
- **Web Server:** Nginx
- **OS:** Ubuntu / AlmaLinux

---

## 🚨 Follow this guide when you update your code on GitHub

[Click here to go to the project update guide](project-update.md)

## 1️⃣ SERVER PREPARATION (One-time)

### 🔹 Update system

```bash
sudo apt update && sudo apt upgrade -y      # Ubuntu
sudo dnf update -y                          # AlmaLinux
```

---

### 🔹 Install basic tools

```bash
sudo apt install -y git curl unzip
# AlmaLinux
sudo dnf install -y git curl unzip
```

---

## 2️⃣ INSTALL NODE, BUN, PM2

### 🔹 Install Node.js (LTS)

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
```

Check:

```bash
node -v
npm -v
```

---

### 🔹 Install Bun

```bash
curl -fsSL https://bun.sh/install | bash
source ~/.bashrc
bun --version
```

---

### 🔹 Install PM2

```bash
npm install -g pm2
pm2 --version
```

---

## 3️⃣ INSTALL & SETUP MONGODB

### 🔹 Install MongoDB

(Ubuntu example)

```bash
sudo apt install -y mongodb-org
```

Start & enable:

```bash
sudo systemctl start mongod
sudo systemctl enable mongod
```

Check:

```bash
mongosh
```

---

## 4️⃣ PROJECT DIRECTORY STRUCTURE

```text
/var/www/my-project
 ├── backend
 │    ├── src
 │    ├── package.json
 │    ├── .env
 │    └── server.js
 └── frontend
      ├── package.json
      ├── next.config.js
      └── .env
```

---

## 5️⃣ CLONE PROJECT FROM GITHUB

```bash
cd /var/www
git clone https://github.com/username/project.git
cd project
```

---

## 6️⃣ BACKEND DEPLOYMENT (Node.js + Express)

### 🔹 Go to backend

```bash
cd backend
```

---

### 🔹 Create `.env`

```env
PORT=5000
MONGO_URI=mongodb://localhost:27017/dbname
JWT_SECRET=your_secret
```

---

### 🔹 Install dependencies

```bash
npm install
```

---

### 🔹 Start backend with PM2

```bash
pm2 start server.js --name backend-api
```

OR (recommended):

```bash
pm2 start npm --name backend-api -- start
```

---

### 🔹 Restart after env/code update

```bash
pm2 restart backend-api --update-env
```

---

## 7️⃣ FRONTEND DEPLOYMENT (Next.js / React)

### 🔹 Go to frontend

```bash
cd ../frontend
```

---

### 🔹 Install dependencies

```bash
bun install
```

---

### 🔹 Build frontend

```bash
bun run build
```

---

### 🔹 Start frontend with PM2

```bash
pm2 start bun --name frontend-app -- start
```

---

### 🔹 Restart after env update

```bash
pm2 restart frontend-app --update-env
```

---

## 8️⃣ NGINX CONFIGURATION (Reverse Proxy)

### 🔹 Install Nginx

```bash
sudo apt install -y nginx
```

---

### 🔹 Create config

```bash
sudo nano /etc/nginx/conf.d/mern.conf
```

```nginx
server {
    server_name yourdomain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    location /api {
        proxy_pass http://localhost:5000;
    }
}
```

---

### 🔹 Restart Nginx

```bash
sudo nginx -t
sudo systemctl restart nginx
```

---

## 9️⃣ DOMAIN & SSL (HTTPS)

### 🔹 Install Certbot

```bash
sudo apt install -y certbot python3-certbot-nginx
```

### 🔹 Enable SSL

```bash
sudo certbot --nginx -d yourdomain.com
```

---

## 🔁 DAILY UPDATE WORKFLOW (MOST IMPORTANT)

```bash
git pull origin main
bun install
bun run build
pm2 restart frontend-app --update-env
pm2 restart backend-api --update-env
pm2 save
```

---

## 🧠 USEFUL PM2 COMMANDS

```bash
pm2 status
pm2 logs
pm2 stop app_name
pm2 delete app_name
pm2 save
pm2 startup
```

---

## 🔐 SECURITY CHECKLIST

- Use `.env` (never commit secrets)
- Enable firewall (UFW)
- Disable root SSH login
- Use SSL (HTTPS)

---

## ✅ FINAL CHECKLIST

- ✔ MongoDB running
- ✔ Backend online (PM2)
- ✔ Frontend online (PM2)
- ✔ Nginx proxy working
- ✔ HTTPS enabled
- ✔ PM2 startup saved

---

## 🎯 TL;DR

You now have a **production-grade MERN Stack deployment**.
This guide can be reused for **any future MERN project**.

---

🚀 Happy Shipping!
