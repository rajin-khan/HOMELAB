# 🎞️ **Jellyfin Setup Guide**

**“Self-hosted streaming server for organizing and playing public domain or personal media.”**

---

## 🧠 What is Jellyfin?

**Jellyfin** is a **free and open-source media server** that allows users to self-host a personal streaming platform. It supports organizing, browsing, and playing media like movies, TV shows, and music across multiple devices, with full privacy and no cloud dependency.

| Feature             | Description                                                        |
| ------------------- | ------------------------------------------------------------------ |
| 🎬 Media playback   | Stream video and audio files through a local or remote interface   |
| 📁 Library scanning | Organizes media with metadata, cover art, and episode information  |
| 🖥️ Cross-platform  | Compatible with macOS, Linux, Windows, Android, iOS, and Smart TVs |
| 👥 Multi-user       | Supports multiple user accounts                                    |
| 🌐 Remote access    | Can be securely accessed over the internet using Tailscale         |
| 🎨 UI theming       | Custom CSS themes supported for visual customization               |

---

## 🛠️ Installation & Setup on macOS (No Docker)

### 1. 📥 Download Jellyfin

* Visit: [https://jellyfin.org/downloads](https://jellyfin.org/downloads)
* Download the **macOS version** (`.dmg`)
* Install the application and launch the Jellyfin server

Once launched, Jellyfin runs a local web interface accessible at:
👉 `http://localhost:8096`

---

### 2. 🔧 Initial Configuration

Upon first launch, the user is guided through:

* Creating an **admin account** (e.g., `rajinkhan`)
* Choosing a preferred language
* Configuring media libraries (see below)

---

### 3. 📁 Media Folder Setup

Suggested folder structure:

```bash
/Users/USERNAME/Media/
├── Movies/         # For movie files
├── TV Shows/       # For episodic content
└── Downloads/      # Temporary downloads before being moved
```

> 📌 Only public domain or personally owned media should be placed here. Users are strongly advised to avoid copyrighted material.

In Jellyfin:

* Navigate to **Dashboard → Libraries → + Add Media Library**
* Add entries for:

  * **Movies** → `/Users/USERNAME/Media/Movies`
  * **TV Shows** → `/Users/USERNAME/Media/TV Shows`

Jellyfin will then scan these folders and organize content with metadata and artwork from public sources.

---

### 4. 🎨 Apply the *Finimalism* Theme (Custom CSS Method)

To achieve a clean, minimal user interface:

1. Obtain the **Finimalism** theme as a raw CSS file from its GitHub repository
   (e.g., [https://github.com/dkanada/jellyfin-theme-finimalism](https://github.com/dkanada/jellyfin-theme-finimalism))

2. Copy the raw CSS URL

3. Go to **Dashboard → Display → Custom CSS**

4. Paste the entire Finimalism CSS or import it with:

   ```css
   @import url("https://raw.githubusercontent.com/dkanada/jellyfin-theme-finimalism/main/finimalism.css");
   ```

5. Save and refresh the interface

✅ The UI now features a slick, minimal look — especially well-suited for modern devices.

---

### 5. 📱 Remote Access with Tailscale + Swiftfin (iOS)

To securely access Jellyfin from mobile devices and laptops:

#### A. Set up Tailscale:

* Install [Tailscale](https://tailscale.com) on both the Jellyfin host and client devices
* Log in with the same account to join the private mesh network

#### B. Use Swiftfin (Jellyfin iOS client):

* Install **Swiftfin** from the App Store
* In the app, enter the **Tailscale IP** of the Jellyfin host (e.g., `http://100.x.x.x:8096`)
* Login with Jellyfin credentials

> ⚠️ Do not expose Jellyfin directly to the internet. Use Tailscale for secure and encrypted remote access.

---

## ✅ Day-to-Day Usage

| Task                  | Instructions                                                                 |
| --------------------- | ---------------------------------------------------------------------------- |
| 🎬 Watch media        | Open Jellyfin in browser or use Swiftfin app                                 |
| 🔄 Update library     | Jellyfin auto-scans folders; can also trigger manual scan from the Dashboard |
| 🎨 Apply/modify theme | Edit under **Dashboard → Display → Custom CSS**                              |
| 🌐 Connect remotely   | Ensure Tailscale is active on both server and client                         |

---

## 🔐 Example Admin Credentials (Template)

| Field    | Value        |
| -------- | ------------ |
| Username | `rajinkhan`  |
| Password | `__________` |

---

## 🧠 Important Notes

* Jellyfin **does not** host or distribute any content — it only plays local files already present on the machine.
* Ideal for **public domain**, self-created, or personal media collections.
* Subtitle files should be named exactly like the video file (e.g., `film.mp4` → `film.en.srt`)
* Multiple user accounts and profiles can be configured under **Dashboard → Users**

---

## 🔁 Routine Maintenance

* 🧹 Clear out `/Downloads` if files are left after import
* 🔄 Manually trigger library scans if files are added outside Sonarr/Radarr
* 📦 Backup settings from:
  `~/Library/Application Support/Jellyfin/Server`

---

## ⚖️ Legal Reminder

Using Jellyfin to organize or view **public domain or self-created media** is entirely legal.
Downloading or streaming **copyrighted content without permission** may violate laws depending on jurisdiction.
This guide assumes all media is legal to access and use.

---