# ⏰ Maintenance, Scheduled Jobs & Backups

On the Mac I used cron to run scripts in the background. On the Pi, [systemd](https://github.com/systemd/systemd) timers do the little jobs that belong to the host, and [Hermes](https://github.com/NousResearch/hermes-agent) schedules the jobs that go through Aria.

---

## 🧠 The Job and the Schedule

| What runs | What tells it when to run |
| --- | --- |
| Refresh the USB drive panel in [Homepage](https://github.com/gethomepage/homepage) | A [systemd](https://github.com/systemd/systemd) timer on the Pi |
| Write the morning draft and send it through Telegram | Two jobs in the [Hermes Guide](Hermes.md), one to write it and one to send it |

I retired the old job that deleted downloads by age. If you need space, check whether [qBittorrent](https://github.com/qbittorrent/qBittorrent) is still seeding or whether [Sonarr](https://github.com/Sonarr/Sonarr) or [Radarr](https://github.com/Radarr/Radarr) is still waiting to import the file. The [Media Automation Guide](Automation.md) shows where to look.

---

## 🪵 Check That It Ran

- **Drive panel looks stale?** Check its last refresh in [Homepage](https://github.com/gethomepage/homepage). Run `systemctl list-timers --all` on the Pi to see the timers. [Homepage Guide](Homepage.md)
- **No morning message?** Check whether the draft job ran and then whether the delivery job sent anything. [Hermes scheduled jobs](Hermes.md)

---

## 💾 What to Copy Before Moving to a New Drive

1. **App settings.** Save the Compose files and a copy of each app's settings. Store credentials and tokens somewhere private.
2. **Media and photos.** Keep another copy of any files you want to keep. For photos, copy the database as well as the uploads. [Immich backup and restore guide](https://docs.immich.app/administration/backup-and-restore/)
3. **Agent settings.** Save the [Hermes](https://github.com/NousResearch/hermes-agent) data volume somewhere protected if you want to keep conversations and settings. [Hermes Docker storage setup](https://hermes-agent.nousresearch.com/docs/user-guide/docker)
4. **Test the copy.** Restore to a separate location. Try playing a video, opening a photo, or starting a chat from that setup before relying on it.

> 💡 The old backup helper has an opt-in switch called `BACKUPS_ENABLED`. I check for a copy I can actually open on another disk before counting something as backed up.

---

📖 [Back to the reading order](ReadingOrder.md) · ⏭️ **Next up** [Bookmark the quick reference](ForgetMeNot.md)
