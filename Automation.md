# 📺🎬 **Sonarr, Radarr, Prowlarr, and Transmission Setup Guide**

**“Automated pipeline for acquiring, organizing, and serving public domain or personal media with Jellyfin.”**

---

## 🧠 OVERVIEW: What Each Tool Does

| Tool             | Purpose                                                                    |
| ---------------- | -------------------------------------------------------------------------- |
| **Sonarr**       | Automates the download and organization of **TV shows**                    |
| **Radarr**       | Automates the download and organization of **movies**                      |
| **Transmission** | Torrent client used to download content (triggered by Sonarr/Radarr)       |
| **Prowlarr**     | Unified indexer manager that connects public torrent sources to the system |

---

## 🧭 PIPELINE WORKFLOW OVERVIEW

1. **User adds a show/movie** in Sonarr or Radarr
2. **Prowlarr searches torrent indexers** for matching releases
3. A **torrent or magnet link is passed to Transmission**
4. **Transmission downloads** to a temporary folder
5. **Sonarr/Radarr monitor** that folder, detect completed downloads
6. Files are **renamed and moved** to the appropriate **TV Shows** or **Movies** folders
7. **Jellyfin automatically updates**, showing the new content

---

## 🛠️ TOOL-BY-TOOL SETUP (macOS, No Docker)

---

### ⚙️ 1. **Transmission Setup**

#### 📥 Installation

* Download from: [https://transmissionbt.com](https://transmissionbt.com)
* Install and open the app
* Access Transmission Web:
  👉 `http://localhost:9091/transmission/web`

#### ⚙️ Configuration via Preferences (Critical)

1. Open the **Transmission app** (not just the web UI)
2. Go to **Transmission → Preferences → Transfers**
3. Set:

   * ✅ **Download to:** `/Users/USERNAME/Media/Downloads/Incomplete`
   * ✅ **Move completed downloads to:** `/Users/USERNAME/Media/Downloads/Completed`

This ensures:

* Downloads occur in an **incomplete folder**
* Once completed, they are moved to a **completed folder**
* Sonarr and Radarr **watch only the completed folder**, so they only import finished files

Example directory layout:

```bash
/Users/USERNAME/Media/
├── Downloads/
│   ├── Incomplete/      # Where Transmission stores partial downloads
│   └── Completed/       # Where completed files are moved for processing
├── TV Shows/            # Sonarr final destination
├── Movies/              # Radarr final destination
```

---

### ⚙️ 2. **Sonarr & Radarr Setup**

#### 📥 Installation

* **Sonarr**: [https://sonarr.tv/#downloads](https://sonarr.tv/#downloads) → `http://localhost:8989`
* **Radarr**: [https://radarr.video/#downloads](https://radarr.video/#downloads) → `http://localhost:7878`

#### 📁 Media Folders

* Sonarr root: `/Users/USERNAME/Media/TV Shows`
* Radarr root: `/Users/USERNAME/Media/Movies`

#### 🔗 Add Transmission as Download Client

In both Sonarr and Radarr:

1. Go to **Settings → Download Clients → + Add**
2. Choose:

   * Type: `Transmission`
   * Host: `localhost`
   * Port: `9091`
   * Username/Password: leave blank unless set
3. Set **Download folder** to:

   * `/Users/USERNAME/Media/Downloads/Completed`
4. Test and Save

Sonarr and Radarr will now:

* Send torrents to Transmission
* Monitor only the **Completed** folder
* Ignore unfinished files

---

### ⚙️ 3. **Prowlarr Setup**

#### 📥 Installation

* Download: [https://prowlarr.com/#downloads](https://prowlarr.com/#downloads)
* Run and access: `http://localhost:9696`

---

### 🌐 Indexers in Prowlarr

#### ✅ What It Does

Prowlarr connects torrent **indexers** to Sonarr/Radarr using a single control panel.

| Indexer            | Purpose                     |
| ------------------ | --------------------------- |
| `nyaa.si`          | Anime content (TV & movies) |
| `thepiratebay`     | General-purpose             |
| `rarbg`            | General + high-quality      |
| `torrentdownloads` | Archive/general releases    |
| `yts`              | High-quality movies         |
| `torrentsome`      | Korean-focused media        |
| `torrenttip`       | Additional Korean sources   |

Only **public** indexers are used — no login or API key required.

---

#### 🔗 Sync Prowlarr with Sonarr & Radarr

1. Prowlarr → **Settings → Applications → + Add**
2. Add:

   * Sonarr: `localhost:8989` + API Key from Sonarr
   * Radarr: `localhost:7878` + API Key from Radarr
3. Click **"Sync Indexers"** for each app

✅ All added indexers are now mirrored in Sonarr & Radarr with correct categories:

* Anime → Sonarr (Anime category)
* Movies → Radarr
* General → Both apps

---

## 🔄 FILE FLOW SUMMARY

```text
1. User adds a TV show/movie (Sonarr/Radarr)
2. Prowlarr finds torrent from public indexers
3. Transmission downloads to Incomplete folder
4. Once complete, file moves to Completed
5. Sonarr/Radarr rename and move to Media/TV Shows or Media/Movies
6. Jellyfin auto-scans folders and shows content
```

---

## 🧪 Testing the System

To verify everything:

* Add a **public domain TV show or movie**
* Wait for download to complete
* Check:

  * File appears in Completed folder
  * File is renamed/moved to correct media folder
  * Appears in Jellyfin at `localhost:8096`

---

## 🔐 Example Credentials (Template)

| App          | Username    | Password     |
| ------------ | ----------- | ------------ |
| Transmission | `rajinkhan` | `__________` |
| Sonarr       | `rajinkhan` | `__________` |
| Radarr       | `rajinkhan` | `__________` |
| Prowlarr     | `rajinkhan` | `__________` |

---

## 🔁 Routine Maintenance

* 🧹 Clean up old files from `/Downloads/Completed` if no longer needed
* 🔄 Refresh indexers in Prowlarr occasionally
* 🛠️ Back up configs from:

  * `~/Library/Application Support/Transmission`
  * `~/Library/Application Support/Sonarr`
  * `~/Library/Application Support/Radarr`
  * `~/Library/Application Support/Prowlarr`

---

## 🛡️ Legal & Ethical Use

This setup is intended for:

* Public domain content (e.g., [archive.org](https://archive.org))
* Personal, self-created, or legally purchased media
* Testing automation pipelines with openly licensed files

**Avoid copyrighted content** or any material that violates laws.

---