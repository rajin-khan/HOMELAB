# Rajin's Homepage Dashboard Setup Guide (No Docker)

## 🏠 What is Homepage?

[Homepage](https://gethomepage.dev) is a beautiful, highly customizable **self-hosted dashboard** that displays and monitors your apps, services, and infrastructure in one place. It can show:

* Widgets for system stats, weather, search
* Real-time integration with apps like **Jellyfin, Sonarr, Radarr, Prowlarr, Transmission**
* Grouped services with icons, uptime checks, and bookmarks
* Your own branding, icons, background, layout, and API keys

It’s like a mission control center for your personal server or homelab.

---

## 💡 Why Use Homepage?

* ✅ Private and self-hosted (no cloud)
* 🎨 Fully customizable UI
* 🔒 Secure — access via local IP or Tailscale VPN
* ⚙️ Integrates with your actual services and shows live data
* 🚀 Light, fast, and beautiful

If, like me, you also run Jellyfin, Radarr, Sonarr, and other services on macOS (with Tailscale remote access), **Homepage provides a sleek control panel that ties everything together.**

---

## 🧭 Setup Overview

| Step                       | Description                                                       |
| -------------------------- | ----------------------------------------------------------------- |
| ✅ Install Node.js + pnpm   | Use nvm to install Node v20+, then `pnpm`                         |
| ✅ Clone Homepage repo      | `git clone https://github.com/gethomepage/homepage`               |
| ✅ Create config files (usually crated automatically, you'll just have to modify what you need)      | `settings.yaml`, `widgets.yaml`, `services.yaml` inside `/config` |
| ✅ Use `.env` for secrets   | Store API keys and login info safely                              |
| ✅ Add background and icons | Inside `/public/images/` and `/public/icons/`                     |
| ✅ Autostart with script    | `homepage.sh` runs it in background with `nohup`                  |
| ✅ Access from anywhere     | Added Tailscale IP + local IP to `HOMEPAGE_ALLOWED_HOSTS`         |

---

## 📁 Directory Structure

```bash
~/homepage/
├── .env
├── config/
│   ├── settings.yaml
│   ├── widgets.yaml
│   └── services.yaml
├── public/
│   └── images/yourbackground.jpg
├── homepage.log
├── homepage.sh (autostart script)
```

---

## ⚙️ ENV Configuration (`.env`, place in the root of your homepage directory)

```env
HOMEPAGE_ALLOWED_HOSTS=localhost:3000,yourTailscaleIP:3000,otherIPs
HOMEPAGE_VAR_SONARR_KEY=...
HOMEPAGE_VAR_RADARR_KEY=...
HOMEPAGE_VAR_PROWLARR_KEY=...
HOMEPAGE_VAR_JELLYFIN_KEY=...
HOMEPAGE_VAR_TRANSMISSION_USER=...
HOMEPAGE_VAR_TRANSMISSION_PASS=...
```

---

## 🚀 Autostart Script (`homepage.sh`)

A lightweight script to run Homepage in the background and keep logs:

```bash
#!/bin/bash

case $1 in
  start)
    echo "Starting Homepage..."
    nohup bash -c 'cd <path-to-your-homepage-directory> && export $(cat .env | xargs) && pnpm start' > <path-to-your-homepag-directory>/homepage.log 2>&1 &
    echo "Started. Logs: homepage.log"
    ;;
  stop)
    echo "Stopping Homepage..."
    pkill -f "pnpm start"
    ;;
  logs)
    tail -f <path-to-your-homepag-directory>/homepage.log
    ;;
  *)
    echo "Usage: $0 {start|stop|logs}"
    ;;
esac
```

Make it executable:

```bash
chmod +x homepage.sh
```

Then run:

```bash
./homepage.sh start
```

---

## 🎨 settings.yaml (UI + Layout)

```yaml
headerStyle: underlined # clean, boxed also available

title: Username's Dash
description: Services Dashboard # or any other description

providers:
  openweathermap: yourOpenWeatherMapAPIKey

background:
  image: /images/background.jpg
  blur: sm #xl, other blur settings as per tailwind configs
  saturate: 80 # 0-100
  brightness: 5 # 0-100
  opacity: 30 # 0-100

layout:
  Media:
    style: row
    columns: 3
  Indexers & Managers:
    style: row
    columns: 2
  Download Client:
    style: row
    columns: 1
  Developer:
    style: row
    columns: 2
  Social:
    style: row
    columns: 2
  Entertainment:
    style: row
    columns: 2

fullWidth: true
useEqualHeights: true
targetBlank: true
language: en

quicklaunch:
  searchDescriptions: true
  hideInternetSearch: false
  showSearchSuggestions: false
  hideVisitURL: false
  provider: google # you can change this, eg, duckduckgo, bing, etc

hideVersion: true
hideErrors: false
```

---

## 📊 widgets.yaml (Info Cards)

```yaml
- resources:
    backend: system
    expanded: false
    cpu: true
    memory: true
    disk:
      - / # your personal mounted drives, or '/' for system drive info

- search:
    provider: google # can have other providers like bing or duckduckgo
    target: _blank

- openweathermap:
    latitude: 23.8041 # place yours
    longitude: 90.4152 # place yours
    provider: openweathermap
    units: metric # or something else if you're crazy
```

---

## 🧩 services.yaml (Grouped Services + Widgets)

```yaml
- Media:
    - Jellyfin:
        icon: jellyfin.png
        href: "http://localhost:8096" # as Jellyfin runs on port 8096
        description: Jellyfin Media Server
        widget:
          type: jellyfin
          url: "http://localhost:8096"
          key: {{HOMEPAGE_VAR_JELLYFIN_KEY}} # sensitive api keys should always be loaded in via env variables
          enableBlocks: true
          enableNowPlaying: true
          enableUser: true
          showEpisodeNumber: true
          expandOneStreamToTwoRows: false

    - Jellyfin (Remote):
        icon: jellyfin.png
        href: "http://yourTailscaleIP:8096" # I set this up as it works for me and my remote setup, you may not need the remote services settings
        description: Jellyfin Media Server
        widget:
          type: jellyfin
          url: "http://yourTailscaleIP:8096"
          key: {{HOMEPAGE_VAR_JELLYFIN_KEY}}
          enableBlocks: true
          enableNowPlaying: true
          enableUser: true
          showEpisodeNumber: true
          expandOneStreamToTwoRows: false

- Indexers & Managers:
    - Prowlarr:
        icon: prowlarr.png
        href: http://localhost:9696
        description: Indexer manager
        widget:
          type: prowlarr
          url: http://localhost:9696
          key: {{HOMEPAGE_VAR_PROWLARR_KEY}}

    - Prowlarr (Remote):
        icon: prowlarr.png
        href: http://yourTailscaleIP:9696
        description: Indexer manager
        widget:
          type: prowlarr
          url: http://yourTailscaleIP:9696
          key: {{HOMEPAGE_VAR_PROWLARR_KEY}}

    - Sonarr:
        icon: sonarr.png
        href: http://localhost:8989
        description: TV show manager
        widget:
          type: sonarr
          url: http://localhost:8989
          key: {{HOMEPAGE_VAR_SONARR_KEY}}

    - Sonarr (Remote):
        icon: sonarr.png
        href: http://yourTailscaleIP:8989
        description: TV show manager
        widget:
          type: sonarr
          url: http://yourTailscaleIP:8989
          key: {{HOMEPAGE_VAR_SONARR_KEY}}

    - Radarr:
        icon: radarr.png
        href: http://localhost:7878
        description: Movie manager
        widget:
          type: radarr
          url: http://localhost:7878
          key: {{HOMEPAGE_VAR_RADARR_KEY}}

    - Radarr (Remote):
        icon: radarr.png
        href: http://yourTailscaleIP:7878
        description: Movie manager
        widget:
          type: radarr
          url: http://yourTailscaleIP:7878
          key: {{HOMEPAGE_VAR_RADARR_KEY}}

- Download Client:
    - Transmission:
        icon: transmission.png
        href: http://localhost:9091
        description: Torrent client
        widget:
          type: transmission
          url: http://localhost:9091
          username: {{HOMEPAGE_VAR_TRANSMISSION_USER}}
          password: {{HOMEPAGE_VAR_TRANSMISSION_PASS}}
```

---

## ✅ Final Tips

* Restart Homepage with `./homepage.sh stop && ./homepage.sh start`
* Access remotely via Tailscale using your Tailscale IP (e.g., `http://yourtailscaleIP:3000`)
* Add more disk paths or services as needed in `widgets.yaml` and `services.yaml`
* Replace the background image with any file in `/public/images/`
* Use the Quicklaunch to find and open any service from the keyboard (Just type while on Homepage)

---