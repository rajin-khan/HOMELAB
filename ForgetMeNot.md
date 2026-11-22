# 🧭 Forget Me Not

> I wrote this page in case I forget what to open when something worked yesterday and today it doesn't.

---

## 🏡 Where I Start

| I want to… | Open this |
| --- | --- |
| Find out what's going on | [Aria in the Hermes Guide](Hermes.md) |
| Get to the other apps | [Homepage](Homepage.md) |
| Watch something | [Jellyfin](Jellyfin.md) |
| Find a port or an app address | [App Addresses & Ports](Access.md) |

---

## 📋 After Starting the Pi

- [ ] Check that the USB drive is mounted. That's where the media and photos live.
- [ ] Open [Homepage](https://github.com/gethomepage/homepage) or ask your agent whether the apps are up.
- [ ] Try a file in [Jellyfin](https://github.com/jellyfin/jellyfin) before assuming a request or download went wrong.

---

## 🔍 Troubleshooting Quick Tips

| Problem | First thing I'd check |
| --- | --- |
| The library opens but looks empty | Check the USB drive and the folder selected in the [Jellyfin Setup Guide](Jellyfin.md). |
| A request was approved and I still can't play it | Check the request, download, and import in that order. [Media Automation Guide](Automation.md) |
| The iPhone can't connect away from home | Check that [Tailscale](https://github.com/tailscale/tailscale) is connected, then try the address in a browser. [Tailscale Setup Guide](Tailscale.md) |
| A photo upload isn't showing up | Check the phone's upload queue, then open the web library with the same account. [Immich Guide](Immich.md) |
| A card on [Homepage](https://github.com/gethomepage/homepage) goes nowhere | Try opening the app directly, then fix the address in [`services.yaml`](https://gethomepage.dev/configs/services/). |

Moving anything to another drive? Read [Maintenance & Backups](CronJobAutomation.md) first.

---

📖 [Back to the reading order](ReadingOrder.md)
