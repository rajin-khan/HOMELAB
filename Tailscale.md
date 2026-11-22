# 🔒 Tailscale Setup Guide

> [Tailscale](https://github.com/tailscale/tailscale) gives me my own private VPN back to the Pi. My phone and MacBook Air can reach the apps at home even when I'm somewhere else.

## ✨ Why I Think It's So Cool

I can leave home with just the iPhone, turn on [Tailscale](https://github.com/tailscale/tailscale), and ask Aria what's happening on the Pi. Then I can watch the same [Jellyfin](https://github.com/jellyfin/jellyfin) library or open the photos in [Immich](https://github.com/immich-app/immich), even on mobile data. The files stay on the drive at home, and the Pi keeps them available even when the MacBook Air is closed.

It's what ties the setup together for me. Put the Pi, iPhone, and MacBook Air on the same private network and they can all reach the Pi. Each app keeps its own address and sign-in, if it needs one, but one connection gets me to all of them. I don't have to forward a different port on the home router for every app.

| What I want to do away from home | What I open |
| --- | --- |
| 🤖 Ask Aria where a request got stuck | [Hermex](https://github.com/uzairansaruzi/hermex) on the iPhone or [Hermes Desktop](https://github.com/NousResearch/hermes-agent/tree/main/apps/desktop) on the Mac |
| 🎬 Watch a video | [Jellyfin](https://github.com/jellyfin/jellyfin) in a browser or [Moonfin](https://github.com/Moonfin-Client/Moonfin-Core) on the iPhone |
| 📸 Look at a photo or upload one | [Immich](https://github.com/immich-app/immich) |
| 🏡 Find the other apps | [Homepage](https://github.com/gethomepage/homepage) |

The [App Addresses & Ports](Access.md) page has the address for each one.

---

## 🛠️ Put the Devices on the Same Network

1. **On the Pi**, install the client using the [Linux setup guide](https://tailscale.com/docs/install/linux). Run `sudo tailscale up` and follow the sign-in link it gives you.
2. **On the iPhone and Mac**, install the client and join the same network as the Pi. The app calls this network a *tailnet*. [iPhone setup](https://tailscale.com/docs/install/ios) · [Mac setup](https://tailscale.com/docs/install/mac)
3. **Find the Pi in the device list.** Copy its address or name. That replaces `<PI_HOST>` in the [app address table](Access.md) when you're away from home.

### ✅ Try One App First

Open `http://<PI_HOST>:8096` in the phone's browser, sign in to [Jellyfin](https://github.com/jellyfin/jellyfin), and play a video. Once it works, try it on mobile data with Wi-Fi off and the private connection still on. That's the moment it clicked for me.

If the page won't open, check that both devices are connected, that the Pi's address is right, and that the app opens at home. The [Jellyfin Setup Guide](Jellyfin.md) covers the first playback test, and the [Tailscale quickstart](https://tailscale.com/docs/how-to/quickstart) covers joining the network.

---

## 🔐 Give an App a Private HTTPS Link

I use [Tailscale Serve](https://tailscale.com/docs/features/tailscale-serve) for the [Hermes](https://github.com/NousResearch/hermes-agent) dashboard and [Immich](https://github.com/immich-app/immich). Serve gives an app a private HTTPS address and passes the request along to the app already running on the Pi. I keep [Funnel](https://tailscale.com/docs/features/tailscale-funnel) off because it makes a route available on the public internet.

For example, if your [Hermes](https://github.com/NousResearch/hermes-agent) dashboard is already answering on `localhost:9119`, here is how to give it HTTPS port `9443` on the private network. Check the current routes first and choose an unused port for a new route.

```sh
tailscale serve status
sudo tailscale serve --bg --https=9443 http://localhost:9119
tailscale serve status
```

If prompted, follow the instructions to enable HTTPS on your network. Open the HTTPS address from the status output on the iPhone or Mac and sign in to the app. On this Pi, the photo library uses HTTPS port `10443`. The [App Addresses & Ports](Access.md) page has both addresses, and the [Serve command guide](https://tailscale.com/docs/reference/tailscale-cli/serve) explains how to adapt the example.

---

📖 [Back to the reading order](ReadingOrder.md) · ⏭️ **Next up** [Look up the app addresses and ports](Access.md)
