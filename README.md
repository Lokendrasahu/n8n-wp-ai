# 🤖 Ultimate WhatsApp AI Bot  
**Production v90**

Run a **24×7 WhatsApp AI Bot** on **GitHub Actions** using **Ngrok, MongoDB, Supabase, Telegram and n8n** — completely **FREE**.

---

# 🚀 What You Get
- WhatsApp Auto Reply Bot  
- AI Chat (g4f)  
- MongoDB Database  
- Supabase Backup & Storage  
- Telegram Notifications  
- n8n Workflow Automation  
- Runs forever on GitHub Actions  

---

# ⚠️ GitHub Actions – VERY IMPORTANT

GitHub free limits:

| Repo Type | Minutes |
|----------|--------|
| **Public Repository** | **Unlimited** ✅ |
| Private Repository | 2000 min / month ❌ |

👉 **Always make your repository PUBLIC**, otherwise the bot will stop.

---

# 🔐 Step 0 — GitHub Secrets Page

Open:
```
Repository → Settings → Secrets and variables → Actions
```
Click **New repository secret** for every key below.

---

# 🛠 Step 1 — Ngrok (Public URL)

1. Login: https://dashboard.ngrok.com  
2. Open **Setup & Installation**
3. Copy **Authtoken**

| Secret | Value |
|-------|-------|
| `NGROK_TOKEN` | Your Ngrok token |

4. Scroll to **Deploy your app online**
5. Copy domain (example: `bot.ngrok-free.dev`)

| Secret | Value |
|-------|-------|
| `NGROK_DOMAIN` | Domain (NO https://) |

---

# 🛢 Step 2 — MongoDB Atlas

## 1️⃣ Database User
MongoDB → **Database Access**

Set:
```
Built-in Role: atlasAdmin
```

---

## 2️⃣ Network Access (IMPORTANT)
MongoDB → **Network Access → Add IP Address**

Add:
```
0.0.0.0/0
```
This allows GitHub + Ngrok to connect.

---

## 3️⃣ Connection String

Original:
```
mongodb+srv://user:<password>@cluster0.abc.mongodb.net/?appName=Cluster0
```

Edit to:
```
mongodb+srv://user:password@cluster0.abc.mongodb.net/wa_bot_db?retryWrites=true&w=majority
```

Save in GitHub:

| Secret | Value |
|-------|-------|
| `MONGODB_URI` | Final edited URL |

---

# 🛠 Step 3 — Supabase

## Create Storage Bucket
Supabase → **Storage → New Bucket**

```
Name: bot-storage
Public: OFF
```

## API Keys (Settings → API)

| Secret | Value |
|-------|-------|
| `SUPABASE_URL` | Project URL |
| `SUPABASE_SERVICE_ROLE` | service_role key (NOT anon) |

---

# 🛠 Step 4 — Telegram Alerts

1. Create bot → **@BotFather**
2. Get your ID → **@userinfobot**

| Secret | Value |
|-------|-------|
| `TELEGRAM_BOT_TOKEN` | Bot token |
| `TELEGRAM_CHAT_ID` | Your Telegram ID |

---

# 🛠 Step 5 — GitHub Token

GitHub → **Settings → Developer Settings → Tokens (classic)**  
Enable: `workflow`

| Secret | Value |
|-------|-------|
| `GH_PAT` | GitHub token |
| `N8N_ENCRYPTION_KEY` | Any password |

---

# 🌐 Bot Control URLs

After bot starts:

| URL | Use |
|------|-----|
| `https://YOUR-NGROK-DOMAIN/qr` | Scan WhatsApp QR |
| `https://YOUR-NGROK-DOMAIN/reset` | Reset WhatsApp if stuck |

---

# 📤 Send WhatsApp (n8n)

```
POST http://wa-bot:10000/send
```

```json
{
  "number": "919999999999",
  "message": "Hello from n8n"
}
```

---

# 🤖 AI Chat (g4f)

```
POST http://ai-server:5000/chat
```

```json
{
  "message": "Write a short story"
}
```

---

# ▶️ Run the Bot

1. Create:
```
.github/workflows/main.yml
```
2. Paste **Production v90 workflow**
3. Commit  
4. Go to:
```
Actions → Run Workflow
```

---

# ✅ DONE

Your **WhatsApp AI Bot** is now running **24×7 on GitHub Actions**.
