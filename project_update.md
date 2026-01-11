# 🚀 Next.js + Bun + PM2 Deployment Guide (Reusable)

This guide is for **production deployment** of a **Next.js app using Bun and PM2** on a VPS (Ubuntu / AlmaLinux).
Use this **every time you update code or environment variables**.

---

## 📁 Prerequisites (One-time)

- Bun installed
- PM2 installed globally
- Project already cloned from GitHub
- `.env` file exists

---

## 🔁 STANDARD UPDATE WORKFLOW (Use Every Time)

Use this workflow whenever you:

- update code
- update `.env`
- pull new changes from GitHub

### ✅ Step 1: Pull latest code

```bash
git pull origin main
```

---

### ✅ Step 2: Install / update dependencies

```bash
bun install
```

---

### ✅ Step 3: Build Next.js app (production)

```bash
bun run build
```

> This creates the `.next` folder

---

### ✅ Step 4: Start app (FIRST TIME ONLY)

⚠️ **Use this only if the app is NOT already running in PM2**

```bash
pm2 start bun --name "app_name" -- start
```

Example:

```bash
pm2 start bun --name "nextjs-app" -- start
```

---

### ✅ Step 5: Restart app after `.env` or code update

```bash
pm2 restart app_name --update-env
```

Example:

```bash
pm2 restart nextjs-app --update-env
```

---

### ✅ Step 6: Zero-downtime reload (optional)

Use this **instead of restart** if the site is live

```bash
pm2 reload app_name --update-env
```

---

### ✅ Step 7: Save PM2 state (recommended)

```bash
pm2 save
```

> Ensures app restarts automatically after server reboot

---

## 🔍 Useful PM2 Commands

### Check running apps

```bash
pm2 status
```

### View logs

```bash
pm2 logs app_name
```

### Stop app

```bash
pm2 stop app_name
```

### Delete app from PM2

```bash
pm2 delete app_name
```

---

## 🧠 Important Notes

- `.env` changes **never work without restart/reload**
- Always use `--update-env`
- `bun run build` is mandatory for production
- Do **NOT** use `next dev` on VPS

---

## 🧾 TL;DR (Quick Copy)

```bash
git pull origin main
bun install
bun run build
pm2 restart app_name --update-env
```

---

## ✅ Recommended First-time Setup

```bash
pm2 startup
pm2 save
```

Run the command shown by `pm2 startup` once.

---

✔️ This guide is **production-safe** and reusable for all future deployments.
