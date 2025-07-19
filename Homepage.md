# Rajin's Homepage Dashboard Setup Guide (No Docker)

## 🏠 What is Homepage?

[Homepage](https://gethomepage.dev) is a beautiful, highly customizable **self-hosted dashboard** that displays and monitors your apps, services, and infrastructure in one place. It can show:

* Widgets for system stats, weather, search
* Real-time integration with apps like **Jellyfin, Sonarr, Radarr, Prowlarr, Transmission, Uptime Kuma**
* Grouped services with icons, uptime checks, and bookmarks
* Your own branding, icons, background, layout, and API keys

It's like a mission control center for your personal server or homelab.

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
| ✅ Create config files (usually created automatically, you'll just have to modify what you need)      | `settings.yaml`, `widgets.yaml`, `services.yaml` inside `/config` |
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
│   └── images/background.jpg
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
    nohup bash -c 'cd <path-to-your-homepage-directory> && export $(cat .env | xargs) && pnpm start' > <path-to-your-homepage-directory>/homepage.log 2>&1 &
    echo "Started. Logs: homepage.log"
    ;;
  stop)
    echo "Stopping Homepage..."
    pkill -f "pnpm start"
    ;;
  logs)
    tail -f <path-to-your-homepage-directory>/homepage.log
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
headerStyle: underlined # default style

title: Rajin's Dash

description: # Your Description Goes Here

providers:
  openweathermap: # Your OpenWeatherMap API Key
  
background:
  image: /images/background.jpg
  blur: sm # sm, "", md, xl... see https://tailwindcss.com/docs/backdrop-blur
  saturate: 80 # 0-100 ... see https://tailwindcss.com/docs/backdrop-saturate
  brightness: 5 # 0-100
  opacity: 30 # 0-100

layout: # This is my custom layout, experiment and modify as you like
  Media & Downloads:
    style: row
    columns: 3
  Managers:
    style: row
    columns: 4   
  Indexer:
    style: row
    columns: 2
  Monitoring:
    style: row
    columns: 4
  Developer:
    style: row
    columns: 2
  Social:
    style: row
    columns: 2
  Entertainment:
    style: row
    columns: 2
  Tools:
    style: row
    columns: 2

fullWidth: true

useEqualHeights: true

targetBlank: true # opens links in new tab

language: en

quicklaunch:
  searchDescriptions: true
  hideInternetSearch: false
  showSearchSuggestions: false
  hideVisitURL: false
  provider: google # google, duckduckgo, bing, baidu, brave or custom

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
- Media & Downloads:
    - Jellyfin:
        icon: jellyfin.png
        href: "http://localhost:8096"
        description: Private Media Server
        widget:
          type: jellyfin
          url: "http://localhost:8096"
          key: {{HOMEPAGE_VAR_JELLYFIN_KEY}}
          enableBlocks: true
          enableNowPlaying: true
          enableUser: true
          showEpisodeNumber: true
          expandOneStreamToTwoRows: false

    - Jellyfin (Remote):
        icon: jellyfin.png
        href: "http://yourTailscaleIP:8096"
        description: Remote Access to my Private Media Server
        ping: "http://yourTailscaleIP:8096"
        widget:
          type: jellyfin
          url: "http://yourTailscaleIP:8096"
          key: {{HOMEPAGE_VAR_JELLYFIN_KEY}}
          enableBlocks: true
          enableNowPlaying: true
          enableUser: true
          showEpisodeNumber: true
          expandOneStreamToTwoRows: false

    - Transmission:
        icon: transmission.png
        href: http://localhost:9091
        description: Torrent client
        widget:
          type: transmission
          url: http://localhost:9091
          username: {{HOMEPAGE_VAR_TRANSMISSION_USER}}
          password: {{HOMEPAGE_VAR_TRANSMISSION_PASS}}

- Indexer:
    - Prowlarr:
        icon: prowlarr.png
        href: http://localhost:9696
        description: Indexer Manager
        widget:
          type: prowlarr
          url: http://localhost:9696
          key: {{HOMEPAGE_VAR_PROWLARR_KEY}}
    
    - Prowlarr (Remote):
        icon: prowlarr.png
        href: http://yourTailscaleIP:9696
        description: Remote Access to my Indexer Manager
        ping: "http://yourTailscaleIP:9696"
        widget:
          type: prowlarr
          url: http://yourTailscaleIP:9696
          key: {{HOMEPAGE_VAR_PROWLARR_KEY}}

- Managers:
    - Sonarr:
        icon: sonarr.png
        href: http://localhost:8989
        description: TV show Manager
        widget:
          type: sonarr
          url: http://localhost:8989
          key: {{HOMEPAGE_VAR_SONARR_KEY}}

    - Sonarr (Remote):
        icon: sonarr.png
        href: http://yourTailscaleIP:8989
        description: Remote Access to my TV show Manager
        ping: "http://yourTailscaleIP:8989"
        widget:
          type: sonarr
          url: http://yourTailscaleIP:8989
          key: {{HOMEPAGE_VAR_SONARR_KEY}}

    - Radarr:
        icon: radarr.png
        href: http://localhost:7878
        description: Movie Manager
        widget:
          type: radarr
          url: http://localhost:7878
          key: {{HOMEPAGE_VAR_RADARR_KEY}}
    
    - Radarr (Remote):
        icon: radarr.png
        href: http://yourTailscaleIP:7878
        description: Remote Access to my Movie Manager
        ping: "http://yourTailscaleIP:7878"
        widget:
          type: radarr
          url: http://yourTailscaleIP:7878
          key: {{HOMEPAGE_VAR_RADARR_KEY}}

- Monitoring:
    - Home Services:
        icon: si-uptimekuma-#5CDD8B
        href: http://localhost:3001
        description: Home Services Uptime Status
        widget:
          type: uptimekuma
          url: http://localhost:3001
          slug: your-slug-name # The part after the / in the status page link

    - Home Services (Remote):
        icon: si-uptimekuma-#5CDD8B
        href: http://yourTailscaleIP:3001
        description: Remote Access to Home Services Uptime Status
        widget:
          type: uptimekuma
          url: http://yourTailscaleIP:3001
          slug: your-slug-name # The part after the / in the status page link

    - Projects:
        icon: si-uptimekuma-#5CDD8B
        href: http://localhost:3001
        description: Projects Uptime Status
        widget:
          type: uptimekuma
          url: http://localhost:3001
          slug: your-slug-name # The part after the / in the status page link

    - Projects (Remote):
        icon: si-uptimekuma-#5CDD8B
        href: http://yourTailscaleIP:3001
        description: Remote Access to Projects Uptime Status
        widget:
          type: uptimekuma
          url: http://yourTailscaleIP:3001
          slug: your-slug-name # The part after the / in the status page link
```

---

## ✅ Final Tips

* Restart Homepage with `./homepage.sh stop && ./homepage.sh start`
* Access remotely via Tailscale using your Tailscale IP (e.g., `http://yourTailscaleIP:3000`)
* Add more disk paths or services as needed in `widgets.yaml` and `services.yaml`
* Replace the background image with any file in `/public/images/` (currently using `bg2.jpg`)
* Use the Quicklaunch to find and open any service from the keyboard (Just type while on Homepage)
* The `ping` parameter in remote services helps with connectivity checks
* Uptime Kuma widgets show real-time status of your services and can be customized with different status page slugs
* **Uptime Kuma Integration**: Make sure Uptime Kuma is running on port 3001 [refer to this guide](./UptimeKuma.md) and has your custom status pages, e.g, my `homeservices` status page configured

---