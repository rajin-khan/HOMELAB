# 🤖 [Hermes Agent](https://github.com/NousResearch/hermes-agent) on the Pi

> [Hermes](https://github.com/NousResearch/hermes-agent) gives me an assistant for the Pi that I can message from my phone. I can ask it to check what's running or help set up an app.

I call mine **Aria**. If something feels off on the Pi, that's where I start.

I'd rather ask Aria why a show hasn't appeared in [Jellyfin](https://github.com/jellyfin/jellyfin) than open four tabs to trace a request. For this setup, I like [Hermes](https://github.com/NousResearch/hermes-agent) way better than [OpenClaw](https://github.com/openclaw/openclaw). I can check the server, follow a request, and get the morning message from the same agent.

---

## 🛠️ Start With [Hermes](https://github.com/NousResearch/hermes-agent) and Give It This Repo

If I were doing it from scratch, I'd get [Hermes](https://github.com/NousResearch/hermes-agent) running first and let it do most of the setup. Give it the guide, tell it where the Pi and drive are, and let it work through the apps one by one.

1. **Get one chat working.** Start with the Pi setup in the [Reading Order](ReadingOrder.md), then run [Hermes](https://github.com/NousResearch/hermes-agent) with a data volume on the system disk. Sign in to a model provider and send a test message. [Hermes Docker setup](https://hermes-agent.nousresearch.com/docs/user-guide/docker)
2. **Give it a way to work on the Pi.** I run the agent in Docker. If the agent needs to set things up on the host, use a scoped SSH connection to the Pi and enter credentials privately. [Hermes terminal settings](https://hermes-agent.nousresearch.com/docs/user-guide/configuration#terminal-backend-configuration)
3. **Paste the prompt below.** Point [Hermes](https://github.com/NousResearch/hermes-agent) at the [HOMELAB repo](https://github.com/rajin-khan/HOMELAB). If you're using a local copy of the guide, give it that path instead.

```text
Use https://github.com/rajin-khan/HOMELAB to set up this Pi.
Start with the setup checklist. Check what is already running and use the
drive and folders I choose. Get the media server working first, then add
requests, downloads, the dashboard, and the extras I pick.

Check each app as you go and give me the addresses to open on the iPhone
and Mac. Keep credentials private and ask before formatting a drive,
replacing existing data, or making a service public.
```

[Hermes](https://github.com/NousResearch/hermes-agent) can write the Compose files, start the apps it has access to, and check that they respond. I handle the sign-ins and play the first video myself so I know it works on the device I plan to use.

---

## 🧠 What I Use for the Model

I use **three OpenAI subscriptions for Codex** and keep their sign-ins in one credential pool in [Hermes](https://github.com/NousResearch/hermes-agent). Aria is the agent I talk to, and Codex is the provider it uses. Each sign-in still has its own subscription limits. [Hermes credential pools](https://hermes-agent.nousresearch.com/docs/user-guide/features/credential-pools)

---

## 💬 Things I Ask Aria

| What I'm checking | What I'd say |
| --- | --- |
| 🟢 Whether the Pi is ready | “Is the media server running? How much space is left on the USB drive?” |
| 🍿 Whether a title is ready | “Is this already in the library? Check the request if you can't find it.” |
| 🔍 Why a request is taking a while | “Is it still waiting for a download, or did the import fail?” |
| 📺 What a new request would do | “Find the title and season, then show me what you'd request before you submit it.” |

On the Pi, `aria-media` helps with status, library searches, and requests. For a new request, I confirm the exact title and the seasons for TV before it submits anything. It also checks the USB drive. The [Media Automation Guide](Automation.md) shows what happens after that.

---

## 📱 How I Connect

- **iPhone.** I use [Hermex](https://github.com/uzairansaruzi/hermex). Connect [Tailscale](https://github.com/tailscale/tailscale), enter the Pi's [Hermes](https://github.com/NousResearch/hermes-agent) dashboard address, and sign in. Use version 2 or later to connect to the built-in dashboard. [Hermex setup guide](https://hermexapp.com/setup)
- **M1 MacBook Air.** I use [Hermes Desktop](https://github.com/NousResearch/hermes-agent/tree/main/apps/desktop) to connect to the same agent on the Pi.
- **Telegram.** This is where I receive scheduled messages and storage alerts. If you're adding a bot, limit who can message it. [Hermes Telegram setup](https://hermes-agent.nousresearch.com/docs/user-guide/messaging/telegram)

Use the [Tailscale Setup Guide](Tailscale.md) to connect the phone or Mac and the [address table](Access.md) for the URLs. The agent's Docker volume keeps settings and conversations on the Pi so I can pick up where I left off from either device.

---

## ⏰ What Runs in the Background

| Dhaka time | Job |
| --- | --- |
| 4:15 AM | Run a small housekeeping job |
| 9:00 AM | Write a morning draft |
| From 9:05 AM | Look for the finished draft and send it to Telegram |

Writing and sending are separate jobs. If the draft takes longer, the sender can wait for it. It keeps track of the messages it already sent so a retry can pick up where it left off.

> 💡 For your first scheduled job, try a sample draft and send it to a chat you control. [Hermes scheduled tasks](https://hermes-agent.nousresearch.com/docs/user-guide/features/cron)

---

📖 [Back to the reading order](ReadingOrder.md) · ⏭️ **Next up** [Get a video playing in Jellyfin](Jellyfin.md)
