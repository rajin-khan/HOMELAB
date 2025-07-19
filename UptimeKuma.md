# 📊 **Uptime Kuma Setup Guide**

**"Self-hosted status monitoring for websites, servers, and APIs with alerting, dashboards, and full privacy."**

---

## 🧠 What is Uptime Kuma?

**Uptime Kuma** is a **free and open-source uptime monitoring tool** that lets you check the status of websites, applications, servers, or local services. You host it yourself, own your data, and get a beautiful UI with alerting and optional public status pages.

| Feature                  | Description                                                          |
| ------------------------ | -------------------------------------------------------------------- |
| 🌍 Multi-monitoring      | Supports HTTP(s), TCP, Ping, DNS, Docker, and more                   |
| 📈 Uptime graphs         | Clean visualizations of response time and availability               |
| 🔔 Notifications         | Alerts via Email, Discord, Telegram, Slack, Webhooks, ntfy, and more |
| 🔐 Self-hosted privacy   | No third-party tracking or cloud reliance                            |
| 🌐 Status pages          | Share real-time health reports with your users or teammates          |
| 🖥️ Lightweight & modern | Simple interface, dark mode, and API integration                     |

---

## 🛠️ Installation & Setup on macOS (No Docker)

### 1. 📦 Install Dependencies

Install **Node.js**, **npm**, and **Git** via [Homebrew](https://brew.sh):

```bash
brew install node git
```

---

### 2. ⬇️ Clone and Set Up

```bash
git clone https://github.com/louislam/uptime-kuma.git
cd uptime-kuma
npm install
npm run setup
```

This builds the UI and installs all required dependencies.

---

### 3. ▶️ Start Uptime Kuma

You can start the server manually:

```bash
node server/server.js
```

Or use `pm2` to run it persistently:

```bash
npm install -g pm2
pm2 start server/server.js --name uptime-kuma
pm2 save
pm2 startup
```

Follow the `pm2 startup` instructions to ensure Kuma runs on reboot.

---

### 4. 🌐 Access the Dashboard

Visit:

```
http://localhost:3001
```

You’ll be prompted to:

* Create an **admin account**
* Start adding monitors

---

## 🔍 Monitor Configuration (Examples)

| Monitor Name        | Type | Target                                                      | Notes                            |
| ------------------- | ---- | ----------------------------------------------------------- | -------------------------------- |
| `Jellyfin`          | HTTP | `http://localhost:8096`                                     | Monitors your local media server |
| `Sonarr`            | HTTP | `http://yourip:8989/api/v3/system/status?apikey=yourapikey` | No headers needed, API v3        |
| `Radarr`            | HTTP | `http://yourip:7878/api/v3/system/status?apikey=yourapikey` | No headers needed, API v3        |
| `Prowlarr`          | HTTP | `http://yourip:9696/api/v1/system/status?apikey=yourapikey` | No headers needed, API v1        |
| `Any Public Site`            | HTTP | `https://liketosite`                               | Public site uptime               |

> 🧠 Note: For services like Sonarr, Radarr, and Prowlarr, **you must include the full API URL with `apikey` as a query parameter**. Headers are **not required**.

---

## 🧠 Why Create a Status Page?

A **status page** lets you transparently share the health of your services with collaborators, users, or even just yourself. It adds:

* ✅ Visual confidence that everything is running
* 🔍 Debug aid when things fail
* 🌐 Public or private visibility
* 🔔 Customizable notifications for each service

---

## 🌍 Create & Customize a Status Page

1. Go to **Status Pages** tab
2. Click **"New Status Page"**
3. Customize:

   * Name & Description
   * Public or Private access
   * Layout and included monitors
4. Share the link or embed in your site/dashboard

---

## 📬 Set Up Email Alerts (Gmail SMTP Example)

### A. Create a Gmail App Password

1. Enable 2FA at [myaccount.google.com/security](https://myaccount.google.com/security)
2. Go to [App Passwords](https://myaccount.google.com/apppasswords)
3. Generate one for "Mail" → "Uptime Kuma"

### B. Configure in Kuma

* Go to **Settings → Notifications → Add SMTP**
* SMTP Server: `smtp.gmail.com`
* Port: `465`
* Username: `your_email@gmail.com`
* Password: `your 16-char app password`
* From: `your_email@gmail.com`
* To: `recipient_email@gmail.com`

Attach this notification to each monitor.

---

## 🛡️ Optional: Secure Access with Tailscale

To safely access Uptime Kuma remotely:

1. Install [Tailscale](https://tailscale.com) on your Mac and remote device
2. Login using the same account
3. Access Kuma via Tailscale IP:

```bash
http://100.x.x.x:3001
```

> 🔐 This avoids exposing Kuma to the public internet.

---

## 🔁 Routine Maintenance

| Task                    | Instruction                                                |
| ----------------------- | ---------------------------------------------------------- |
| 🔄 Restart Kuma         | `pm2 restart uptime-kuma`                                  |
| 📦 Backup data          | Backup the `/app/data` or pm2 folder if needed             |
| ⚙️ Update               | Pull latest repo and re-run `npm install && npm run setup` |
| 🚨 Add new monitor      | Click `Add New Monitor` in the dashboard                   |
| 🔔 Adjust notifications | Use per-monitor alerting settings                          |

---

## ✅ Summary

| Feature                     | Setup Status |
| --------------------------- | ------------ |
| Uptime Kuma Installed       | ✅            |
| Services Monitored          | ✅            |
| Status Page Created         | ✅            |
| Email Notifications         | ✅            |
| Remote Access via Tailscale | ✅            |

Uptime Kuma is now your personal observability dashboard — clean, reliable, and fully self-hosted.

---