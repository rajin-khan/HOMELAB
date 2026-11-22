# 🏡 [Homepage](https://github.com/gethomepage/homepage) Dashboard Setup Guide

> [Homepage](https://github.com/gethomepage/homepage) is my homelab's home screen. One bookmark gets me to all the apps I use.

I can also see what's playing in [Jellyfin](https://github.com/jellyfin/jellyfin) or check the USB drive from there. On the Mac I used to start it with a script. On the Pi it's a Docker container on port `3000`. If I have a question instead of an app in mind, I usually open Aria. That's covered in the [Hermes Guide](Hermes.md).

---

## 🧭 Setup Overview

1. **Run the container.** Follow the [Homepage Docker guide](https://gethomepage.dev/installation/docker/) and keep its `/app/config` directory on the system disk.
2. **Add the address you use.** Set `HOMEPAGE_ALLOWED_HOSTS` to the hostname or IP you type to open the dashboard, including the port where applicable. The [App Addresses & Ports](Access.md) page has the address formats.
3. **Start with two app links.** Use the example below and click through both before adding widgets.

---

## 📁 The Config Folder

```text
config/
├── services.yaml       App links and groups
├── settings.yaml       Title, layout and appearance
└── widgets.yaml        Info cards at the top
```

### 🧩 App Links

Add these two cards to [`services.yaml`](https://gethomepage.dev/configs/services/). Replace `pi.example.test` with the address your browser can reach.

```yaml
- Media:
    - Jellyfin:
        href: http://pi.example.test:8096
        description: Watch movies and shows
    - Jellyseerr:
        href: http://pi.example.test:5055
        description: Request a title
```

### 🎨 Layout

In [`settings.yaml`](https://gethomepage.dev/configs/settings/), use the same group name as above to lay those cards out in a row.

```yaml
title: Homelab
layout:
  Media:
    style: row
    columns: 2
```

### 📊 Info Cards

When the links are working, pick a small info card to add in [`widgets.yaml`](https://gethomepage.dev/configs/info-widgets/). The [resources widget](https://gethomepage.dev/widgets/info/resources/) is a good place to see examples for memory and disk usage.

---

## ✅ What I Check on the Pi

- **Now playing.** A quick look at what's playing in [Jellyfin](https://github.com/jellyfin/jellyfin).
- **USB drive.** A small job refreshes the drive panel about once a minute. [Homepage](https://github.com/gethomepage/homepage) still opens if the drive drops out.
- **Alerts.** I can turn on storage alerts through Aria in Telegram. See [Maintenance & Backups](CronJobAutomation.md) for where to check scheduled jobs.

The Pi has its own storage panel. If you'd like to add Docker stats to a new dashboard, start with the [restricted Docker socket proxy example](https://gethomepage.dev/configs/docker/). If you're looking for the old monitor, see [Why I Took Uptime Kuma Out](UptimeKuma.md).

---

📖 [Back to the reading order](ReadingOrder.md) · ⏭️ **Next up** [Reach the Pi with Tailscale](Tailscale.md)
