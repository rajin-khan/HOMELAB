# 📺 Media Automation Setup Guide

> These apps take a movie or show request, download the file, and put it in your library.

The part I like is picking a title once and opening my [Jellyfin](https://github.com/jellyfin/jellyfin) library when it's ready. I usually start with Aria if I have a title in mind. [Jellyseerr](https://github.com/seerr-team/seerr) is still handy when I want to browse and make the request myself. The [Hermes Guide](Hermes.md) has examples of how I ask.

---

## 🧠 What Each Tool Does

| Tool | Job |
| --- | --- |
| 🍿 **[Jellyseerr](https://github.com/seerr-team/seerr)** | Accepts a request and sends it to the right app |
| 📺 **[Sonarr](https://github.com/Sonarr/Sonarr)** | Finds and organizes TV episodes |
| 🎬 **[Radarr](https://github.com/Radarr/Radarr)** | Finds and organizes movies |
| 🔍 **[Prowlarr](https://github.com/Prowlarr/Prowlarr)** | One list of places your movie and TV apps look for files |
| ⬇️ **[qBittorrent](https://github.com/qbittorrent/qBittorrent)** | Downloads files and can share them with other people |
| 🎞️ **[Jellyfin](https://github.com/jellyfin/jellyfin)** | Scans the library and plays the finished files |

The Mac used [Transmission](https://github.com/transmission/transmission). The Pi uses [qBittorrent](https://github.com/qbittorrent/qBittorrent) and still runs [Jellyseerr](https://github.com/seerr-team/seerr). For a fresh setup, you can use its successor, [Seerr](https://github.com/seerr-team/seerr). [Seerr installation guide](https://docs.seerr.dev/getting-started/)

---

## 🧭 Where the Files Go

| App | Downloads inside the app | Library inside the app |
| --- | --- | --- |
| [qBittorrent](https://github.com/qbittorrent/qBittorrent) | `/downloads` | |
| [Sonarr](https://github.com/Sonarr/Sonarr) | `/downloads` | `/data/Aria-Inbox/TV` |
| [Radarr](https://github.com/Radarr/Radarr) | `/downloads` | `/data/Aria-Inbox/Movies` |

On the Pi, `/downloads` points to `/srv/media/Aria-Downloads`. [Sonarr](https://github.com/Sonarr/Sonarr) and [Radarr](https://github.com/Radarr/Radarr) also see the USB drive as `/data`. [Jellyfin](https://github.com/jellyfin/jellyfin) gets its own read-only view of the [movie and TV folders](Jellyfin.md).

> 💡 Keep downloads and finished library files in separate folders. On the exFAT drive, leave room for both copies while a download is still seeding.

---

## 🛠️ Tool-by-Tool Setup

### 1. ⬇️ Set Up [qBittorrent](https://github.com/qbittorrent/qBittorrent)

- Use the [qBittorrent Compose example](https://docs.linuxserver.io/images/docker-qbittorrent/) to start the container. The web app uses port `8080` here.
- Set the save path inside the app to `/downloads`.
- Give movies and shows different categories. For a fresh setup, `movies` and `tv` work as example names.

### 2. 📺 Connect [Sonarr](https://github.com/Sonarr/Sonarr) & [Radarr](https://github.com/Radarr/Radarr)

- Use the [Sonarr container guide](https://docs.linuxserver.io/images/docker-sonarr/) and [Radarr container guide](https://docs.linuxserver.io/images/docker-radarr/) to add both apps. Put them on the same Compose network as [qBittorrent](https://github.com/qbittorrent/qBittorrent).
- In each app, set the library's **root folder** to the path from the table above.
- Under **Settings → Download Clients**, add [qBittorrent](https://github.com/qbittorrent/qBittorrent). Enter its Compose service name, web port, and sign-in details. Pick the category you created for that app, then test the connection before saving. [Sonarr settings guide](https://wiki.servarr.com/sonarr/settings)

### 3. 🔍 Connect [Prowlarr](https://github.com/Prowlarr/Prowlarr)

- Start it from the [Prowlarr container guide](https://docs.linuxserver.io/images/docker-prowlarr/) on the same network.
- In **Settings → Applications**, add [Sonarr](https://github.com/Sonarr/Sonarr) and [Radarr](https://github.com/Radarr/Radarr). If you named the services `sonarr` and `radarr`, the internal addresses are `http://sonarr:8989` and `http://radarr:7878`. Use the names from your own Compose setup.
- Add the API key for each connection in the app settings and sync the sources you're allowed to use. Keep the keys off the public repo.

### 4. 🍿 Add the Request App

Connect [Jellyseerr](https://github.com/seerr-team/seerr) or [Seerr](https://github.com/seerr-team/seerr) to [Jellyfin](https://github.com/jellyfin/jellyfin), [Sonarr](https://github.com/Sonarr/Sonarr), and [Radarr](https://github.com/Radarr/Radarr) during setup. Pick the library and quality profile for each app before approving a request. The [Seerr setup guide](https://docs.seerr.dev/getting-started/) has the install steps for a new instance.

---

## 🧪 Try the Whole Thing With One File

Pick a title you can find in the request app and that you have permission to download and share. To test just the library and playback, use a video you already have and follow the [Jellyfin Setup Guide](Jellyfin.md).

- [ ] The request shows up in [Sonarr](https://github.com/Sonarr/Sonarr) or [Radarr](https://github.com/Radarr/Radarr).
- [ ] [qBittorrent](https://github.com/qbittorrent/qBittorrent) finishes the download.
- [ ] [Sonarr](https://github.com/Sonarr/Sonarr) or [Radarr](https://github.com/Radarr/Radarr) imports it into the right library folder.
- [ ] [Jellyfin](https://github.com/jellyfin/jellyfin) scans it and the video plays.

If it stops halfway, open the [quick reference](ForgetMeNot.md) or ask your agent which app is waiting.

---

📖 [Back to the reading order](ReadingOrder.md) · ⏭️ **Next up** [Put the apps on the Homepage dashboard](Homepage.md)
