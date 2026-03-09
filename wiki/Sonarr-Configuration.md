# Sonarr Configuration

> Sonarr monitors and automatically downloads TV shows.

---

## Step 1 — Open Sonarr

1. Go to: `http://localhost:8989`
2. Complete the first-launch setup wizard.
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
- Category: `tv`
- Click **Test** → **Save**

**For NZBGet:**
- Select **NZBGet**
- Host: `localhost`
- Port: `6789`
- Username: your NZBGet username
- Password: your NZBGet password
- Category: `tv`
- Click **Test** → **Save**

---

## Step 3 — Add Root Folder

1. Go to **Settings → Media Management**.
2. Scroll down to **Root Folders**, click **Add Root Folder**.
3. Set the path to `/media/tv`
4. Click **OK**.

---

## Step 4 — Configure Quality Profiles

> Pre-made quality and custom format profiles will be available to import — check the [repo releases](../../releases) for configuration files.

For now:
1. Go to **Settings → Profiles**.
2. Review the default profiles and choose one.
3. You can leave these as defaults until the import files are available.

---

## Step 5 — Enable Rename (Optional but Recommended)

1. Go to **Settings → Media Management**.
2. Enable **Rename Episodes**.
3. Use the default naming format or customize it.
4. Click **Save**.

---

## Step 6 — Copy the API Key

1. Go to **Settings → General**.
2. Copy the **API Key** — paste it into Prowlarr (if you haven't already) and Overseerr.

---

## Step 7 — Add Your First Show (Test)

1. Click **+ Add Series**.
2. Search for any TV show.
3. Set the quality profile, root folder, and whether to monitor future episodes.
4. Click **Add Series**.

---

## Next Step

➡️ **[Overseerr Configuration](Overseerr-Configuration)**
