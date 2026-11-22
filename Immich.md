# 📸 [Immich](https://github.com/immich-app/immich) Photo Library

> [Immich](https://github.com/immich-app/immich) is like having my own private Google Photos, with the photos and videos stored on my own drive.

I can upload from the iPhone and open the same library on the MacBook Air. It runs on the Pi in a separate [Docker Compose](https://github.com/docker/compose) project from the media apps. The database lives on the system disk, and the photos and videos go to `/srv/media/Immich` on the USB drive.

---

## 🛠️ Set Up the Server

1. Start with the [Immich Docker Compose guide](https://docs.immich.app/install/docker-compose/).
2. Choose the folder on the USB drive for uploads and keep the database on the system disk. Check that the drive is mounted before starting the app.
3. Open the web library and create an account. For access away from home, I use the Pi's private HTTPS address on port `10443`. See [App Addresses & Ports](Access.md) for the address format and the [Tailscale Setup Guide](Tailscale.md) for the connection.

I use [Immich](https://github.com/immich-app/immich)'s storage templates for the files and keep machine learning and video transcoding off on this Pi.

---

## 📱 Try an Upload From the iPhone

1. Connect [Tailscale](https://github.com/tailscale/tailscale), open [Immich](https://github.com/immich-app/immich), and sign in at the same address you used in the browser.
2. Pick an album and try uploading one photo before you queue the rest.
3. Open the web library on the MacBook Air and check that the photo arrived.

> 💡 If a batch is slow, check the upload queue with the app open. iOS decides when the app gets time to upload in the background. [Immich mobile backup guide](https://docs.immich.app/features/mobile-backup/)

---

## 💾 Space & Backups

I aim to keep the library within **100 GB** and log a warning at **90 GB**. Uploads can still go through after the warning.

Keep a copy of **both** the database and the photos on separate storage. Try restoring a few photos into a separate instance before relying on the backup. [Immich backup and restore guide](https://docs.immich.app/administration/backup-and-restore/)

---

📖 [Back to the reading order](ReadingOrder.md) · ⏭️ **Next up** [Set up maintenance and backups](CronJobAutomation.md)
