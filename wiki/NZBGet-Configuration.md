# NZBGet Configuration

> Step-by-step first-time setup for NZBGet (Usenet downloader).

---

## Step 1 — Open NZBGet

1. Open a browser and go to: `http://localhost:6789`
2. Log in with the username and password you set in the compose file (`NZBGET_USER` / `NZBGET_PASS`).

---

## Step 2 — Add a Usenet News Server

You need a **Usenet provider** (paid service). Common ones: Newshosting, Eweka, UsenetExpress, etc.

1. In the NZBGet Web UI, go to **Settings → News-Servers**.
2. Click **Add server**.
3. Fill in the details from your Usenet provider:
   - **Active:** Yes
   - **Host:** (your provider's server address, e.g. `news.newshosting.com`)
   - **Port:** 563 (SSL) or 119 (non-SSL)
   - **Username:** your Usenet account username
   - **Password:** your Usenet account password
   - **Connections:** 20–50 (check your plan limits)
   - **Encryption:** Yes (if using port 563)
4. Click **Save** and **Reload**.

---

## Step 3 — Verify the Server Connection

1. Go to **Settings → News-Servers**.
2. Click **Test** next to your server.
3. You should see a green checkmark. If not, double-check your host/port/credentials.

---

## Step 4 — Configure Paths

1. Go to **Settings → Paths**.
2. Confirm these are set (you may have done this in the qBittorrentVPN wiki):
   - **MainDir:** `/data/nzbget`
   - **DestDir:** `/data/complete`
   - **InterDir:** `/data/incomplete`
3. Click **Save** and **Reload**.

---

## Step 5 — Add an Indexer (NZB Source)

Indexers are websites that list Usenet content. You can configure them here or let Prowlarr manage them (recommended).

> If you're using the Full Stack, skip manual indexer setup here — **Prowlarr will handle this automatically** once it's configured.

---

## Step 6 — Note the API Key

Radarr, Sonarr, and Prowlarr will need to talk to NZBGet. They use a simple username/password — no API key needed for NZBGet specifically, but keep your credentials handy.

---

## Next Step

➡️ **[Prowlarr Configuration](Prowlarr-Configuration)**
