# 🌐 App Addresses & Ports

> Use this page to look up what address to enter on the iPhone or MacBook Air.

At home I can use the Pi's local address. When I'm away, I use its address on the private network. Set that up in the [Tailscale Setup Guide](Tailscale.md) first.

---

## 📱 What I Open Where

| What I want to do | App | Address |
| --- | --- | --- |
| 🍿 Watch something | [Jellyfin](https://github.com/jellyfin/jellyfin) in a browser, or [Moonfin](https://github.com/Moonfin-Client/Moonfin-Core) on the iPhone | `http://<PI_HOST>:8096` |
| 🤖 Talk to Aria on the phone | [Hermex](https://github.com/uzairansaruzi/hermex) | `https://<TAILNET_HOST>:9443` |
| 🤖 Talk to Aria on the Mac | [Hermes Desktop](https://github.com/NousResearch/hermes-agent/tree/main/apps/desktop) | The Mac app's remote connection to the Pi |
| 📸 Browse photos or upload an album | [Immich](https://github.com/immich-app/immich) | `https://<TAILNET_HOST>:10443` |
| 🏡 Open the app shortcuts | [Homepage](https://github.com/gethomepage/homepage) | `http://<PI_HOST>:3000` |
| 🍿 Browse or request a title | [Jellyseerr](https://github.com/seerr-team/seerr) | `http://<PI_HOST>:5055` |

- Replace **`<PI_HOST>`** with the Pi's reachable local or [Tailscale](https://github.com/tailscale/tailscale) address.
- Replace **`<TAILNET_HOST>`** with the Pi's [Tailscale](https://github.com/tailscale/tailscale) DNS name.

> 💡 Enter the address of the Pi even if you're sitting at the Mac. `localhost` means the device you're on.

For sign-in and the first test on each device, see the [Jellyfin Setup Guide](Jellyfin.md), [Hermes Guide](Hermes.md), or [Immich Guide](Immich.md).

---

## 📋 Other Ports I Look Up

| App | Port on the Pi |
| --- | --- |
| [qBittorrent](https://github.com/qbittorrent/qBittorrent) web app | `8080` |
| [qBittorrent](https://github.com/qbittorrent/qBittorrent) peer traffic | `6881` TCP/UDP |
| [Sonarr](https://github.com/Sonarr/Sonarr) | `8989` |
| [Radarr](https://github.com/Radarr/Radarr) | `7878` |
| [Prowlarr](https://github.com/Prowlarr/Prowlarr) | `9696` |

The [Media Automation Guide](Automation.md) explains which of these apps connect to each other and which addresses to use inside Docker.

---

## 🔒 A Note About the Ports

I leave router port forwarding off for these web apps. The direct Docker ports in this setup also listen on the local network. On Docker versions before 28, including the Pi's version, hosts on the same network segment can sometimes reach ports published to localhost. [Docker port publishing guide](https://docs.docker.com/engine/network/port-publishing/)

---

📖 [Back to the reading order](ReadingOrder.md) · ⏭️ **Next up** [Add Immich for photos](Immich.md) · [Skip to Maintenance & Backups](CronJobAutomation.md)
