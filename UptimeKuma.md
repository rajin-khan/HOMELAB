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

You'll be prompted to:

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

## 🌐 Understanding Status Pages & Why You Need Them

### 🧠 Why Create a Status Page?

Status pages serve multiple critical purposes in your homelab or business environment:

* **🔍 Centralized Health Overview**: Get a bird's-eye view of all your services at a glance
* **📊 Historical Data**: Track uptime trends and identify patterns in service disruptions  
* **🔔 Proactive Communication**: Inform users/family about service status before they ask
* **🛠️ Troubleshooting Aid**: Quickly identify which services are affected during outages
* **📈 Performance Metrics**: Monitor response times and establish baselines
* **🤝 Team Coordination**: Share service status with teammates or household members
* **📱 Mobile Access**: Check status from anywhere without logging into the admin panel
* **🎯 Integration Ready**: Use with Homepage, Discord bots, or other dashboards

### 💡 Real-World Use Cases:

- **For Families**: "Is Plex down or is it my device?"
- **For Teams**: Share infrastructure status without giving admin access
- **For Personal Use**: Quick mobile check when away from home
- **For Integration**: Feed status data into Homepage dashboard widgets

---

## 🌍 Create & Customize Status Pages

### Step 1: Create Your First Status Page

1. Navigate to **Status Pages** tab in the sidebar
2. Click **"+ New Status Page"**
3. Configure basic settings:

   **General Settings:**
   - **Title**: `Home Services` or `My Infrastructure`
   - **Description**: `Real-time status of personal homelab services`
   - **Theme**: Light/Dark/Auto
   - **Language**: Select your preference

   **URL & Access:**
   - **Slug**: `anynameyouwant` ⚠️ **This is crucial for Homepage integration!**
   - **Show Tags**: Enable to group related services
   - **Show Powered By**: Toggle Uptime Kuma branding

### Step 2: Configure Monitors

4. **Add Monitors to Status Page:**
   - Select which monitors to display publicly
   - Group related services (e.g., "Media Services", "Download Tools")
   - Choose display order and visibility

5. **Customize Appearance:**
   - Upload custom logo/favicon
   - Set custom CSS if desired
   - Configure incident management settings

### Step 3: Access & Share

Your status page will be accessible at:
```
http://localhost:3001/status/anynameyouwant
```

Or remotely via Tailscale:
```
http://100.x.x.x:3001/status/anynameyouwant
```

---

## 🏷️ Understanding Slugs & Their Importance

### What is a Slug?

A **slug** is the URL-friendly identifier for your status page. It appears in the URL path:

```
http://your-server:3001/status/YOUR-SLUG-HERE
```

### Critical Slug Requirements:

**For Homepage Integration:**
- Use exactly: `anynameyouwant` (as shown in your Homepage config)
- Must be lowercase, no spaces
- Avoid special characters except hyphens

**Multiple Status Pages Example:**
```yaml
# You could create multiple pages for different purposes:
http://localhost:3001/status/homeservices    # For personal/family use
http://localhost:3001/status/infrastructure  # For technical services  
http://localhost:3001/status/public         # For external users
```

### Best Practices for Slugs:

- **Keep it simple**: `homeservices`, `infrastructure`, `main`
- **Be descriptive**: `dev-services`, `production-apps`
- **Stay consistent**: Once set and integrated, avoid changing slugs
- **Document it**: Note your slug in your setup documentation

---

## 📊 Status Page Management Tips

### Organizing Your Monitors

**Group by Function:**
```
🎬 Media Services
├── Jellyfin
├── Plex (if applicable)
└── Overseerr

📥 Download Management  
├── Transmission
├── qBittorrent
└── NZBGet

🔍 Content Discovery
├── Sonarr
├── Radarr  
└── Prowlarr
```

**Monitor Selection Strategy:**
- Include user-facing services (Jellyfin, Plex)
- Add critical infrastructure (DNS, VPN)
- Exclude admin-only tools unless needed
- Consider your audience (family vs technical users)

### Customization Options

**Visual Branding:**
- Upload custom favicon and logo
- Match your Homepage color scheme
- Add custom CSS for advanced styling

**Functional Features:**
- Enable incident posting for planned maintenance
- Set up maintenance mode scheduling
- Configure automatic incident updates

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

**Status Page Remote Access:**
```bash
http://100.x.x.x:3001/status/homeservices
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
| 📊 Update status pages  | Add/remove monitors from public status pages              |

---

## 🔗 Integration with Homepage Dashboard

Once your status page is created with the `anynameyouwant` slug, it will automatically work with your Homepage widgets:

```yaml
# This will now display live status from Uptime Kuma
- Home Services:
    icon: si-uptimekuma-#5CDD8B
    href: http://localhost:3001
    description: Home Services Uptime Status
    widget:
      type: uptimekuma
      url: http://localhost:3001
      slug: anynameyouwant  # ← This matches your status page slug
```

---

## ✅ Summary

| Feature                     | Setup Status |
| --------------------------- | ------------ |
| Uptime Kuma Installed       | ✅            |
| Services Monitored          | ✅            |
| Status Page Created         | ✅            |
| Slug Configured             | ✅            |
| Homepage Integration Ready  | ✅            |
| Email Notifications         | ✅            |
| Remote Access via Tailscale | ✅            |

Uptime Kuma is now your personal observability dashboard — clean, reliable, and fully self-hosted with beautiful status pages ready for sharing and integration.

---