# Prowlarr Configuration

> Prowlarr is your indexer manager — it connects to torrent and Usenet indexer sites and pushes them to Radarr/Sonarr automatically.

**[🏠 Wiki Home](/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[⬅️ NZBGet Configuration](/NolieRavioli/Windows-Docker-Mediastack/wiki/NZBGet-Configuration)** | **[➡️ Radarr Configuration](/NolieRavioli/Windows-Docker-Mediastack/wiki/Radarr-Configuration)**

---

## Step 1 — Open Prowlarr

1. Go to: `http://localhost:9696`
2. On first launch, you'll be asked to create an **admin username and password**. Do that now.

---

## Step 2 — Add Indexers

Indexers are the sites Prowlarr searches for content.

1. In Prowlarr, go to **Indexers → Add Indexer**.
2. Search for your preferred indexer (you can search by name inside Prowlarr — it has a large built-in list of public and private trackers). Common choices include public trackers like The Pirate Bay, 1337x, or any private tracker you have access to.
3. Fill in any required credentials (username, password, or API key from that site).
4. Click **Test** — you should see a green checkmark.
5. Click **Save**.
6. Repeat for any additional indexers.

---

## Step 3 — Add Applications (Connect to Radarr/Sonarr)

This tells Prowlarr to automatically sync indexers into Radarr and Sonarr.

1. Go to **Settings → Apps**.
2. Click **Add Application** → select **Radarr**.
3. Fill in:
   - **Prowlarr Server:** `http://prowlarr:9696`
   - **Radarr Server:** `http://radarr:7878`
   - **API Key:** *(copy from Radarr — see below)*
4. Click **Test** → **Save**.
5. Repeat for **Sonarr** (server: `http://sonarr:8989`).

> **How to get the Radarr/Sonarr API key:**
> Open Radarr → **Settings → General** → copy the API Key shown there.
> Do the same for Sonarr.

---

## Step 4 — Add Download Clients (Optional — Radarr/Sonarr can do this too)

You can configure download clients directly in Prowlarr for manual searches:

1. Go to **Settings → Download Clients**.
2. Click **Add** → select **qBittorrent** or **NZBGet**.
3. Fill in connection details:
   - **Host:** `localhost` (or the container name)
   - **Port:** `8113` (qBittorrent) or `6789` (NZBGet)
   - **Username/Password:** your credentials
4. Click **Test** → **Save**.

---

## Step 5 — Add FlareSolverr (For Cloudflare-Protected Indexers)

If any of your indexers are blocked by Cloudflare, FlareSolverr handles that automatically.

1. Go to **Settings → Indexers**.
2. Find **FlareSolverr** and set:
   - **URL:** `http://flaresolverr:8191`
3. Click **Save**.

> FlareSolverr is already running as part of your stack — you just need to point Prowlarr at it.

---

## Step 6 — Verify Sync

1. Go to **Indexers** — you should see your added indexers listed.
2. Go to **Radarr** and check **Settings → Indexers** — your indexers should now appear there automatically.

---

**[🏠 Wiki Home](/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[⬅️ NZBGet Configuration](/NolieRavioli/Windows-Docker-Mediastack/wiki/NZBGet-Configuration)** | **[➡️ Radarr Configuration](/NolieRavioli/Windows-Docker-Mediastack/wiki/Radarr-Configuration)**
