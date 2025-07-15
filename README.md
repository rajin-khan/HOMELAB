# 🎬 Rajin's Media Automation Stack

> 📁 Self-hosted, secure, and fully automated streaming system  
> ⚙️ Powered by: **Sonarr + Radarr + Prowlarr + Transmission + Jellyfin**  
> 🔐 Privacy-first via **Tailscale** VPN

---

## 📦 Stack Overview

| Tool           | Role                                           |
|----------------|------------------------------------------------|
| 🧠 Jellyfin     | Media server — organize & stream content       |
| 📺 Sonarr       | Auto-download & manage TV shows                |
| 🎬 Radarr       | Auto-download & manage movies                  |
| 🔍 Prowlarr     | Unified indexer manager for Sonarr & Radarr    |
| ⬇️ Transmission | Torrent client for actual downloads            |
| 🔒 Tailscale    | Secure remote access without open ports        |

---

## 🧭 File Structure

```

/Users/rajin/Media/
├── Downloads/
│   ├── Incomplete/   # Used by Transmission (in-progress)
│   └── Completed/    # Final download location for Sonarr/Radarr import
├── TV Shows/         # Final destination for shows (Sonarr)
├── Movies/           # Final destination for movies (Radarr)

```

---

## 🚀 How It Works

1. Add a **TV show (Sonarr)** or **movie (Radarr)**
2. **Prowlarr** searches public indexers (Nyaa, TPB, YTS, etc.)
3. Sends magnet/torrent to **Transmission**
4. Transmission downloads to `/Downloads/Incomplete`, then moves to `Completed`
5. Sonarr/Radarr detect the finished download, rename + organize it into the correct folder
6. **Jellyfin** scans the folders and displays new content

---

## 🌐 Remote Access

- **Tailscale** is used to access everything securely across devices
- No port forwarding or public exposure required
- Remote streaming via **Swiftfin** (iOS Jellyfin app) is fully supported

---

## 🔧 Notable Configuration Details

### ⚙️ Transmission Preferences

- Downloads to: `/Users/rajin/Media/Downloads/Incomplete`
- Moves completed files to: `/Users/rajin/Media/Downloads/Completed`

### 🎯 Prowlarr Indexers

All **public** and login-free:

| Indexer        | Type            | Purpose                     |
|----------------|------------------|-----------------------------|
| `nyaa.si`      | Anime             | TV/movies                   |
| `thepiratebay` | General           | TV, films                   |
| `rarbg`        | General           | High-quality content        |
| `torrentdownloads` | General      | Archives, misc              |
| `yts`          | Movies            | High-quality film releases  |
| `torrentsome`  | Korean-focused    | K-dramas, films             |
| `torrenttip`   | Korean-focused    | Alt. Korean sources         |

→ Synced with Sonarr & Radarr via Prowlarr Applications tab

---

## 📋 Default Access URLs

| App           | URL                          | Port  |
|---------------|------------------------------|-------|
| Jellyfin      | `http://localhost:8096`       | 8096  |
| Sonarr        | `http://localhost:8989`       | 8989  |
| Radarr        | `http://localhost:7878`       | 7878  |
| Prowlarr      | `http://localhost:9696`       | 9696  |
| Transmission  | `http://localhost:9091`       | 9091  |

---

## 🔐 Credentials Template

| Service      | Username    | Password     |
|--------------|-------------|--------------|
| Jellyfin     | `rajinkhan` | `__________` |
| Sonarr       | `rajinkhan` | `__________` |
| Radarr       | `rajinkhan` | `__________` |
| Prowlarr     | `rajinkhan` | `__________` |
| Transmission | `rajinkhan` | `__________` |

---

## 🛡️ Legal Use Disclaimer

This setup is configured strictly for **personal** and **legal** use.  
✅ Ideal for:
- Public domain content
- Self-created media
- Openly licensed content

❌ Avoid copyrighted material unless legally permitted.

---

## 📌 Maintenance Tips

- 🧹 Clear `/Downloads/Completed` regularly
- 🔄 Sync indexers in Prowlarr as needed
- 🔁 Back up configs from `~/Library/Application Support/...`
- 🎨 Customize Jellyfin UI via custom CSS (e.g., Finimalism theme)

---

## 📎 Optional Extensions

- 📄 [Bazarr](https://www.bazarr.media) → Auto-download subtitles
- 🧭 [Dashy](https://github.com/Lissy93/dashy) → Dashboard for all services
- ☁️ [Nextcloud](https://nextcloud.com) → Private file syncing and cloud storage
- 🛑 [Pi-hole](https://pi-hole.net) → Network-wide ad blocker

---