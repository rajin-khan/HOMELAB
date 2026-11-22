# 📖 Homelab Reading Order & Setup Checklist

> This is the order I'd use if I were setting everything up again. Get one chat working with [Hermes](https://github.com/NousResearch/hermes-agent), get one video playing in [Jellyfin](https://github.com/jellyfin/jellyfin), and add the bits you want from there.

Skim the [README](README.md) to see what runs on the Pi and where the files live. Then come back here and check things off as you go ✅

---

## 🧭 Recommended Order

### 1. 🤖 [Hermes Guide](Hermes.md) · Get the Assistant Running

- [ ] Install 64-bit Linux and [Docker Compose](https://github.com/docker/compose) on the Pi. Keep app settings on the system disk and make sure the USB drive is mounted before you put media folders on it. [Docker on Debian](https://docs.docker.com/engine/install/debian/)
- [ ] Get one chat working. Use the [copy-paste setup prompt](Hermes.md) to give [Hermes](https://github.com/NousResearch/hermes-agent) the repo and let it work through the apps below.

> 💡 Setting it up by hand? Start with the [Jellyfin Setup Guide](Jellyfin.md) and come back to the [Hermes Guide](Hermes.md) when you want to add the assistant.

---

### 2. 🎞️ [Jellyfin Setup Guide](Jellyfin.md) · Play One Video

- [ ] Add a movie or TV folder from the USB drive and scan it in [Jellyfin](https://github.com/jellyfin/jellyfin).
- [ ] Try a video you already have permission to use. Once it plays in a browser, you know the library works before bringing in the download apps.

---

### 3. 📺 [Media Automation Guide](Automation.md) · Follow a Request

- [ ] Set up [qBittorrent](https://github.com/qbittorrent/qBittorrent) for downloads, [Sonarr](https://github.com/Sonarr/Sonarr) for TV, [Radarr](https://github.com/Radarr/Radarr) for movies, and [Prowlarr](https://github.com/Prowlarr/Prowlarr) for the sources they search.
- [ ] Connect [Jellyseerr](https://github.com/seerr-team/seerr) and follow a test request all the way through. Find it in the right app, watch the file land in the library, then play it in [Jellyfin](https://github.com/jellyfin/jellyfin).

---

### 4. 🏡 [Homepage Guide](Homepage.md) · Put the Apps in One Place

- [ ] Start with links to [Jellyfin](https://github.com/jellyfin/jellyfin) and [Jellyseerr](https://github.com/seerr-team/seerr). Open each card and check that it goes where you expect.
- [ ] Add the layout and widgets once the links work. Two working cards are a good start.

---

### 5. 🔒 [Tailscale Setup Guide](Tailscale.md) · Reach the Pi Away From Home

- [ ] Join the Pi, iPhone, and MacBook Air to the same [Tailscale](https://github.com/tailscale/tailscale) network.
- [ ] Try playing a video from the phone on mobile data before setting up more apps.

---

### 6. 🌐 [App Addresses & Ports](Access.md) · Open Your Apps

- [ ] Find what to enter on the iPhone or MacBook Air. If you set up private HTTPS for an app, try signing in at that address too.

---

### 7. 📸 [Immich Guide](Immich.md) · Add Photos if You Want Them

- [ ] Try uploading one photo from the iPhone and open it in the web library on the MacBook Air.

Just here for the media stack? [Skip to Maintenance & Backups](CronJobAutomation.md).

---

### 8. ⏰ [Maintenance & Backups](CronJobAutomation.md) · Keep a Spare Copy

- [ ] See what runs on a schedule and where to check if it worked.
- [ ] Back up the app settings and files you want to keep. Try restoring a copy while the setup is still small.

---

### 9. 🧭 [Forget Me Not](ForgetMeNot.md) · Bookmark It

Keep it handy for the days when a request gets stuck, a link stops working, or [Jellyfin](https://github.com/jellyfin/jellyfin) can't see the drive.

---

## 👋 Coming From the Old Mac Guide?

The old setup ran [Uptime Kuma](https://github.com/louislam/uptime-kuma) for monitoring. I took it out because the emails got annoying. See [Why I Took Uptime Kuma Out](UptimeKuma.md) for what I use now.
