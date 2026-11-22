# 🎞️ [Jellyfin](https://github.com/jellyfin/jellyfin) Setup Guide

> [Jellyfin](https://github.com/jellyfin/jellyfin) is like having my own private Netflix for the movies and shows I keep on the Pi.

Mine includes the folders I brought over from the Mac. The app scans them for movies and shows and fills in things like artwork and episode information. I can watch from the MacBook Air in a browser or pull up the same library on the iPhone.

---

## 🛠️ Installation & Setup on the Pi

### 1. 📦 Run the Container

I use the [LinuxServer image](https://github.com/linuxserver/docker-jellyfin). Start with its [Jellyfin Compose example](https://docs.linuxserver.io/images/docker-jellyfin/). Keep the `/config` folder on the system disk and mount your movie and TV folders read-only, using `:ro` on those media entries.

Check that the USB drive is mounted. If this is a fresh setup, run these from the folder with the Compose file, using the service name from the example.

```sh
docker compose config --quiet
docker compose up -d jellyfin
```

The [Reading Order](ReadingOrder.md) covers the Pi and drive setup if you're starting from scratch.

### 2. 📁 Add Your Libraries

Open [Jellyfin](https://github.com/jellyfin/jellyfin) on port `8096`, create an account, and add a **Movies** or **Shows** library. Use the folder names [Jellyfin](https://github.com/jellyfin/jellyfin) sees inside the container. Here's how those look on the Pi.

| Folder on the USB drive | Folder inside [Jellyfin](https://github.com/jellyfin/jellyfin) |
| --- | --- |
| `/srv/media/Aria-Inbox/Movies` | `/media/Aria-Inbox-Movies` |
| `/srv/media/Aria-Inbox/TV` | `/media/Aria-Inbox-TV` |
| Movies brought over from the Mac | `/media/Movies` |
| TV brought over from the Mac | `/media/TV_Shows` |

### 3. 🍿 Play Something

Start with one file, like a video you made. Scan the folder and try to play the file in [Jellyfin](https://github.com/jellyfin/jellyfin). If it doesn't show up, check the library path and whether the container can read it before changing any other settings.

---

## 📱 Watching From the iPhone & MacBook Air

- **iPhone.** I mostly use [Moonfin](https://github.com/Moonfin-Client/Moonfin-Core) here. Add the Pi's media server address to the app and sign in with the same account you use in the browser. [Connect Moonfin to Jellyfin](https://github.com/Moonfin-Client/Moonfin-Core/wiki/Getting-Started)
- **M1 MacBook Air.** I open [Jellyfin](https://github.com/jellyfin/jellyfin) itself in a browser, usually from [Homepage](Homepage.md).
- **Away from home.** Connect your device using the [Tailscale Setup Guide](Tailscale.md), then look up the Pi's address in [App Addresses & Ports](Access.md).

---

## 🎨 Make the Web App Look the Way You Like

I use a bit of custom CSS in [Jellyfin](https://github.com/jellyfin/jellyfin)'s web interface. If you want to try it, make one change in the custom CSS settings and refresh the browser to see how it looks. [Jellyfin's custom CSS guide](https://jellyfin.org/docs/general/clients/css-customization/)

---

## ✅ Day-to-Day Tips

| What you're doing | Try this |
| --- | --- |
| Adding a file by hand | Run a library scan if it hasn't appeared yet |
| Adding subtitles | Put the file next to the video with the same base name, like `Film.mkv` and `Film.en.srt` |
| Working out why a title hasn't appeared | Ask your agent where the request got stuck, or work through the [quick checks](ForgetMeNot.md) |

[Jellyfin's file naming guide](https://jellyfin.org/docs/general/server/media/movies/) has more examples. See the [Hermes Guide](Hermes.md) for what I ask Aria when I'm checking the library.

---

📖 [Back to the reading order](ReadingOrder.md) · ⏭️ **Next up** [Set up requests and downloads](Automation.md)
