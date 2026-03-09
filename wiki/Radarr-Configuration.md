# Radarr Configuration

> Radarr monitors and automatically downloads movies.

**[🏠 Wiki Home](/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[⬅️ Prowlarr Configuration](/NolieRavioli/Windows-Docker-Mediastack/wiki/Prowlarr-Configuration)** | **[➡️ Sonarr Configuration](/NolieRavioli/Windows-Docker-Mediastack/wiki/Sonarr-Configuration)**

---

## Step 1 — Open Radarr

1. Go to: `http://localhost:7878`
2. On first launch, you'll walk through a short setup wizard. Complete it.
3. Go to **Settings → General** and note your **API Key** — you'll need it for Prowlarr and Overseerr.

---

## Step 2 — Add a Download Client

1. Go to **Settings → Download Clients**.
2. Click **+** to add a new client.

**For qBittorrent:**
- Select **qBittorrent**
- Host: `localhost`
- Port: `8113`
- Username: your qBittorrent username
- Password: your qBittorrent password
- Category: `movies`
- Click **Test** → **Save**

**For NZBGet:**
- Select **NZBGet**
- Host: `localhost`
- Port: `6789`
- Username: your NZBGet username
- Password: your NZBGet password
- Category: `movies`
- Click **Test** → **Save**

---

## Step 3 — Add Root Folder

1. Go to **Settings → Media Management**.
2. Scroll down to **Root Folders**, click **Add Root Folder**.
3. Set the path to `/media/movies`
4. Click **OK**.

---

## Step 4 — Configure Quality Profiles

> Pre-made quality and custom format profiles will be available to import — check the [repo releases](/NolieRavioli/Windows-Docker-Mediastack/releases) for configuration files.

For now:
1. Go to **Settings → Profiles**.
2. Review the default profiles (HD-1080p, Ultra-HD, etc.) and choose which one suits you.
3. You can leave these as defaults until the import files are available.

---

## Step 5 — Enable Rename (Optional but Recommended)

1. Go to **Settings → Media Management**.
2. Enable **Rename Movies**.
3. Use the default naming format or customize it.
4. Click **Save**.

---

## Step 6 — Copy the API Key

1. Go to **Settings → General**.
2. Copy the **API Key** — you'll paste it into Prowlarr (if you haven't already) and Overseerr.

---

## Step 7 — Add Your First Movie (Test)

1. Click **+ Add Movie** (or Movies → Add New).
2. Search for any movie.
3. Set the quality profile and root folder.
4. Click **Add Movie**.
5. If Prowlarr is configured, Radarr will search for it automatically.

---

**[🏠 Wiki Home](/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[⬅️ Prowlarr Configuration](/NolieRavioli/Windows-Docker-Mediastack/wiki/Prowlarr-Configuration)** | **[➡️ Sonarr Configuration](/NolieRavioli/Windows-Docker-Mediastack/wiki/Sonarr-Configuration)**
