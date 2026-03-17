<div align="center">

# 🤖 Ultimate WhatsApp AI Bot
### Multi-Vendor • Production vFinal • Zero-Downtime Edition

[![GitHub Actions](https://img.shields.io/badge/Powered%20By-GitHub%20Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)](https://github.com/features/actions)
[![Cloudflare Tunnel](https://img.shields.io/badge/Tunnel-Cloudflare-F38020?style=for-the-badge&logo=cloudflare&logoColor=white)](https://www.cloudflare.com/)
[![MongoDB](https://img.shields.io/badge/Database-MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![Supabase](https://img.shields.io/badge/Storage-Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)](https://supabase.com/)
[![n8n](https://img.shields.io/badge/Automation-n8n-EA4B71?style=for-the-badge&logo=n8n&logoColor=white)](https://n8n.io/)
[![Telegram](https://img.shields.io/badge/Alerts-Telegram-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)](https://telegram.org/)

> Run a **24×7 Multi-Vendor WhatsApp AI Bot** on **GitHub Actions** using **Cloudflare Tunnels, MongoDB, Supabase, Telegram, and n8n** — completely **FREE**.

</div>

---

## 📌 Table of Contents

- [✨ Features](#-features)
- [⚠️ GitHub Actions — Important](#️-github-actions--important)
- [⚙️ Step 0 — Repository Settings](#️-step-0--repository-settings-critical)
- [🔐 Step 1 — GitHub Secrets](#-step-1--github-secrets)
- [🛢️ Step 2 — Cloud & Database Setup](#️-step-2--cloud--database-setup-check)
- [🌐 Step 3 — Bot Control & Webhook URLs](#-step-3--bot-control--webhook-urls)
- [📥 Receiving Messages in n8n](#-receiving-messages-in-n8n)
- [📤 Sending Messages from n8n](#-sending-messages-from-n8n)
- [▶️ How to Start the Bot](#️-how-to-start-the-bot)

---

## ✨ Features

- **True Zero-Downtime Deployments:** Less than 20 seconds offline during the 6-hour GitHub cycle handover.
- **Multi-Vendor Support:** Run multiple WhatsApp numbers (`v1`, `v2`) simultaneously.
- **Cloudflared Tunnel:** Fast, stable, and secure reverse proxy that replaces Ngrok.
- **Smart Database Sync:** Automated SQLite vacuuming and persistence via Supabase.
- **Session Safety:** MongoDB-backed Baileys sessions to avoid repeated QR rescans.
- **Smart Webhook Retries:** Prevent dropped messages during deployment handovers.

---

## ⚠️ GitHub Actions — Important

GitHub free tier limits:

| Repo Type | Minutes |
|---|---:|
| **Public Repository** | **Unlimited** ✅ |
| Private Repository | 2000 min / month ❌ |

> [!CAUTION]
> **Always make your repository PUBLIC**, otherwise the bot will eventually stop after the private-repo free minutes are exhausted.

---

## ⚙️ Step 0 — Repository Settings (CRITICAL)

Before adding secrets, allow GitHub Actions to trigger the handover loop:

1. Go to **`Settings` → `Actions` → `General`**
2. Scroll to **Workflow permissions**
3. Select **Read and write permissions**
4. Click **Save**

---

## 🔐 Step 1 — GitHub Secrets

Go to **`Settings` → `Secrets and variables` → `Actions`** and click **New repository secret**.

Add **exactly these 8 secrets**:

| Secret Name | Description / Value |
|---|---|
| `CF_TUNNEL_TOKEN` | Your Cloudflare Zero Trust Tunnel Token |
| `GH_PAT` | GitHub Personal Access Token (Classic) with `workflow` scope |
| `MONGODB_URI` | MongoDB Atlas URL, for example: `mongodb+srv://user:pass@cluster.mongodb.net/wa_bot_db` |
| `N8N_ENCRYPTION_KEY` | Any strong custom password to encrypt your n8n credentials |
| `SUPABASE_URL` | Your Supabase Project URL |
| `SUPABASE_SERVICE_ROLE` | Supabase `service_role` secret key, **do not** use the public `anon` key |
| `TELEGRAM_BOT_TOKEN` | Bot token from `@BotFather` |
| `TELEGRAM_CHAT_ID` | Your personal Telegram Chat ID from `@userinfobot` |

---

## 🛢️ Step 2 — Cloud & Database Setup Check

Make sure the following are configured correctly:

- **MongoDB:** Set **Network Access** to `0.0.0.0/0` so GitHub runners can connect.
- **Supabase:** Create a **private storage bucket** named exactly `bot-storage`.
- **Cloudflare:** Ensure your tunnel routes traffic to **`localhost:80`**.

---

## 🌐 Step 3 — Bot Control & Webhook URLs

Once the GitHub Action is running, access your system using your Cloudflare domain.

Example domain: `https://your-domain.com`

| Action | URL Endpoint |
|---|---|
| **n8n Dashboard** | `https://your-domain.com/` |
| **Incoming Webhook** | `https://your-domain.com/webhook/whatsapp` |
| **Scan QR (Vendor 1)** | `https://your-domain.com/qr?v=v1` |
| **Scan QR (Vendor 2)** | `https://your-domain.com/qr?v=v2` |
| **Reset Bot (Vendor 1)** | `https://your-domain.com/reset?v=v1` |
| **Reset Bot (Vendor 2)** | `https://your-domain.com/reset?v=v2` |
| **Health Check** | `https://your-domain.com/health` |

---

## 📥 Receiving Messages in n8n

Create a **Webhook Node** in n8n with:

- **Path:** `whatsapp`
- **Method:** `POST`

When a user sends a message, your `wa-bot` forwards it to n8n using this JSON structure:

```json
{
  "vendor": "v1",
  "full_json": {
    "key": {
      "remoteJid": "919999999999@s.whatsapp.net",
      "fromMe": false,
      "id": "ABCDEF1234567890"
    },
    "message": {
      "conversation": "Hello bot!"
    }
  }
}
This lets you identify exactly which vendor (v1 or v2) received the message.

📤 Sending Messages from n8n
To reply from n8n, create an HTTP Request Node with the following settings:

Method: POST

URL: http://wa-bot:10000/send

Request Body (JSON):

json
{
  "vendor": "v1",
  "number": "919999999999",
  "message": "Hello from Nexus Automation!"
}
[!TIP]
Make sure the vendor field matches the same vendor that received the incoming message.

▶️ How to Start the Bot
Create a file named .github/workflows/main.yml

Paste your workflow code into that file

Open the Actions tab in your repository

Select Production vFinal (Multi-Vendor + Safe Cancel) from the left menu

Click Run workflow

The system will automatically restart itself every ~5h 50m and continue running in a loop.

🧩 Notes
Keep your repository public for uninterrupted execution.

Use the Cloudflare domain for all QR, reset, webhook, and dashboard access.

Keep your Supabase and MongoDB credentials private and never hardcode them in source files.

<div align="center">
🚀 Built for reliable multi-vendor WhatsApp automation
Replace the placeholders below after creating your repository:

Stars
Forks

</div> ```
