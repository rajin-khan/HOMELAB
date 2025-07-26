# 🧭 **Rajin's Homelab Guide**

**This is a guide in case I ever forget everything. You can use it as a cheatsheet to remember things as well.**

---

## 🧠 OVERVIEW: What Each Service Does

| Service          | What it Does                                                                                                 |
| ---------------- | ------------------------------------------------------------------------------------------------------------ |
| **Jellyfin**     | Plays and organizes my media (movies, shows, anime, etc.)                                                    |
| **Sonarr**       | Auto-downloads and renames **TV shows**                                                                      |
| **Radarr**       | Auto-downloads and renames **movies**                                                                        |
| **Transmission** | Downloads torrents from Sonarr/Radarr                                                                        |
| **Tailscale**    | Creates a private network between all my devices, lets me securely access this server remotely from anywhere |
| **Prowlarr**     | Acts as the unified **indexer manager**, connecting multiple torrent sources to Sonarr & Radarr              |
| **Homepage**     | A custom dashboard that shows the status and shortcuts to all my services                                    |
| **Uptime Kuma**  | Monitors all services for uptime/downtime, sends alerts, and provides status pages for sharing               |
| **Cron**         | Automates scheduled tasks like cleanup scripts and backups                                                   |

---

## 🛠️ SERVICES & HOW TO ACCESS

> 📌 All services are local web apps. Use your browser and go to:

| Service             | URL                                                        | Default Port | My Username | My Password  |
| ------------------- | ---------------------------------------------------------- | ------------ | ----------- | ------------ |
| **Jellyfin**        | `http://localhost:8096`                                    | `8096`       | `rajinkhan` | `__________` |
| **Sonarr**          | `http://localhost:8989`                                    | `8989`       | `rajinkhan` | `__________` |
| **Radarr**          | `http://localhost:7878`                                    | `7878`       | `rajinkhan` | `__________` |
| **Transmission**    | `http://localhost:9091`                                    | `9091`       | `rajinkhan` | `__________` |
| **Tailscale Admin** | [https://login.tailscale.com](https://login.tailscale.com) | N/A          | `rajinkhan` | `__________` |
| **Prowlarr**        | `http://localhost:9696`                                    | `9696`       | `rajinkhan` | `__________` |
| **Homepage**        | `http://localhost:3000`                                    | `3000`       | _none_      | _none_       |
| **Uptime Kuma**     | `http://localhost:3001`                                    | `3001`       | `rajinkhan` | `__________` |

You can also replace `localhost` with your machine's local IP (e.g., `192.168.xxx.xxx`) if you're on the same LAN.

---

## 🧱 FILE STRUCTURE (Custom Setup)

```bash
/Users/rajin/Media/
├── Movies/         # Where Radarr stores organized movie files | Where Jellyfin retrieves Movies
├── TV Shows/       # Where Sonarr stores organized TV files | Where Jellyfin retrieves Shows
└── Downloads/      # Where Transmission saves new downloads temporarily, moves them to the Jellyfin Media folders later
```

---

## 📋 STARTUP CHECKLIST

When booting or recovering the homelab:

1. **Start Transmission** (torrent client)

   * Search "Transmission" in Spotlight
   * Or: `open -a Transmission`
   * Visit [http://localhost:9091/transmission/web](http://localhost:9091/transmission/web)

2. **Start Sonarr**

   * `open -a Sonarr`
   * Visit [http://localhost:8989](http://localhost:8989)

3. **Start Radarr**

   * `open -a Radarr`
   * Visit [http://localhost:7878](http://localhost:7878)

4. **Start Jellyfin**

   * `open -a Jellyfin`
   * Visit [http://localhost:8096](http://localhost:8096)

5. **Start Prowlarr**

   * `open -a Prowlarr`
   * Visit [http://localhost:9696](http://localhost:9696)

6. **Start Uptime Kuma**

   * Run: `pm2 start uptime-kuma` (if using pm2)
   * Or: `cd uptime-kuma && node server/server.js`
   * Visit [http://localhost:3001](http://localhost:3001)

7. **Start Tailscale (optional)**

   * Run: `tailscale up`
   * Check status: `tailscale status`
   * Visit remote server: `http://<tailscale-ip>:8096` from your phone/laptop
   * Or, just turn on the tailscale app and connect
   * Then, copy the tailscale IP of the macbook-pro, add the relevant port number, and login to that server/service

8. **Start Homepage Dashboard (optional)**

   * Run: `./homepage.sh start`
   * Logs: `./homepage.sh logs`
   * Access: [http://localhost:3000](http://localhost:3000)

9. **Verify Cron Jobs (optional)**

   * Run: `crontab -l`
   * This confirms that automated tasks (like cleanup) are scheduled to run.

---

## 🔄 WHAT HAPPENS WHEN I ADD A SHOW OR MOVIE?

### 📺 Sonarr Workflow (TV Shows)

1. I go to `http://localhost:8989`
2. Click **Add New Series**
3. Choose a show, select `/Users/rajin/Media/TV Shows` as folder
4. It auto-finds episodes via indexers from **Prowlarr**
5. Transmission downloads it
6. Sonarr moves/renames the file to `TV Shows/ShowName/...`
7. Jellyfin sees it automatically

### 🎬 Radarr Workflow (Movies)

1. I go to `http://localhost:7878`
2. Click **Add New Movie**
3. Choose movie, folder = `/Users/rajin/Media/Movies`
4. Radarr sends download to Transmission
5. Once complete, it moves file and notifies Jellyfin, which sees it automatically, just like Sonarr

---

### 🌐 Prowlarr Workflow (Indexer Management)

1. I go to `http://localhost:9696`
2. I add indexers (e.g., Nyaa, TPB, RARBG, TorrentDownloads, Torrentsome, TorrentTip, YTS)
3. Prowlarr automatically syncs these with Sonarr and Radarr
4. When I search for shows or movies in Sonarr/Radarr, results are pulled through Prowlarr
5. It acts as a unified backend for **indexer search, RSS, and management**

---

## 🔍 TROUBLESHOOTING QUICK TIPS

| Problem                           | What to Do                                                                                 |
| --------------------------------- | ------------------------------------------------------------------------------------------ |
| Jellyfin shows nothing            | Check that files are in `/Movies` or `/TV Shows`, and Jellyfin libraries are set to those  |
| Transmission not downloading      | Make sure it's running, check port settings, or test torrents manually                     |
| Sonarr/Radarr not finding content | Check indexers in **Prowlarr**, make sure they're enabled and properly synced              |
| Remote access fails               | Run `tailscale up`, check `tailscale status`, run Tailscale and check correct Tailscale IP |
| Uptime Kuma not monitoring       | Check monitors are configured with correct API URLs (include apikey parameter)             |
| Services show offline in Kuma    | Verify service URLs and API keys, restart Uptime Kuma with `pm2 restart uptime-kuma`      |
| Subtitle missing                  | Install **Bazarr** and point it to the same folders (work on this later)                        |
| Automated tasks not running     | Check cron logs (`~/cron_homelab.log`), verify schedule (`crontab -l`), and script permissions (`chmod +x`) |

---

## 🔐 PLACEHOLDER CREDENTIALS

| App          | Username            | Password     |
| ------------ | ------------------- | ------------ |
| Jellyfin     | `rajinkhan`         | `__________` |
| Sonarr       | `rajinkhan`         | `__________` |
| Radarr       | `rajinkhan`         | `__________` |
| Transmission | `rajinkhan`         | `__________` |
| Prowlarr     | `rajinkhan`         | `__________` |
| Uptime Kuma  | `rajinkhan`         | `__________` |
| Tailscale    | `github auth login` | `__________` |

---

## 🔁 REGULAR MAINTENANCE

* 🧹 **Verify automated cleanup** is working by checking `~/cron_homelab.log`.
* 🔄 Update apps via Homebrew or from official sites
* 🧠 Refresh indexers in **Prowlarr**
* 📊 Check **Uptime Kuma** for any service alerts or downtime
* 🛡️ Run occasional backups of `/Media` to an external drive
* 🧐 Monitor Homepage logs in homepage.log

---

## ✨ OPTIONAL TOOLS TO ADD LATER

* **Bazarr** — auto subtitles
* **Pi-hole** — network ad blocker
* **Nextcloud** — private file cloud

---