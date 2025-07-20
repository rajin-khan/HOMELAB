# 📖 Homelab Reading Order & Setup Checklist

> This guide follows the order of what I consider **most important and fun** to get the most out of your homelab.

Each step builds toward a fully automated, private, and usable media experience.

---

## Recommended Order

> You can check these boxes as you go ✅

### 1. **[`README.md`](./README.md)** — *Overview*

* Learn how the full stack fits together.
* Understand what each tool does and how they interact.

---

### 2.  **[`Jellyfin.md`](./Jellyfin.md)** — *Media Server First*

* Install Jellyfin and set up your media libraries.
* 🎯 **Now is a good time to organize any existing media** into:

  ```
  /Users/rajin/Media/
  ├── Movies/
  ├── TV Shows/
  └── Downloads/
  ```
* Customize the UI, add remote access via Tailscale.

---

### 3. **[`Automation.md`](./Automation.md)** — *Automate Media Acquisition*

* Set up Sonarr (TV) and Radarr (Movies).
* Configure Prowlarr with public indexers.
* Link Transmission as the torrent client.
* Ensure the full pipeline: Add → Download → Rename → Jellyfin.

---

### 4. **[`Homepage.md`](./Homepage.md)** — *Visual Dashboard*

* Install Homepage from source.
* Create widgets for all services.
* Add icons, background, and layout for that slick control center feel.

---

### 5. **[`UptimeKuma.md`](./UptimeKuma.md)** — *Service Monitoring*

* Set up Uptime Kuma to monitor services and display public/private status pages.
* Optionally configure email or Telegram alerts for downtime.

---

### 6. **[`ForgetMeNot.md`](./ForgetMeNot.md)** — *Cheat Sheet & Recovery*

* Keep this for quick reference, startup instructions, common issues, and routine maintenance.

---

## 🧠 Notes

* You can rearrange the setup if needed, but this order helps you:

  1. **See results early (Jellyfin)**
  2. **Automate next (Sonarr/Radarr)**
  3. **Control visually (Homepage)**
  4. **Stay aware (Uptime Kuma)**
  5. **Never forget (ForgetMeNot)**