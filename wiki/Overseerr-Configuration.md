# Seerr Configuration

> Seerr is the user-facing request interface — think of it like your own personal Netflix request app. Users can browse and request movies/shows, and Radarr/Sonarr handle the rest automatically.

**[🏠 Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[⬅️ Sonarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Sonarr-Configuration)**

---

## Step 1 — Open Seerr

1. Go to: `http://localhost:5055`
2. You'll land on the setup wizard — click **Get Started**.

---

## Step 2 — Sign In with Plex (or Set Up Local Login)

Seerr is designed to work with **Plex** as the media server. If you have Plex:
1. Click **Sign In with Plex**.
2. Authorize Seerr to access your Plex account.
3. Select your Plex server from the list.
4. Click **Save Changes**.

If you don't have Plex yet, you can skip this for now and set up a local admin account. You can always add Plex later.

---

## Step 3 — Connect Radarr

1. In the setup wizard, click **Add Radarr Server**.
2. Fill in:
   - **Server Name:** Radarr (or anything you like)
   - **Hostname or IP:** `radarr`  *(the container name — Docker handles the DNS)*
   - **Port:** `7878`
   - **API Key:** *(paste the API key from Radarr → Settings → General)*
   - **Default Quality Profile:** choose your preferred profile
   - **Default Root Folder:** `/media/movies`
3. Click **Test** — you should see a green checkmark.
4. Check **Default Server** and **Enable Scan**.
5. Click **Add Server**.

---

## Step 4 — Connect Sonarr

1. Click **Add Sonarr Server**.
2. Fill in:
   - **Server Name:** Sonarr
   - **Hostname or IP:** `sonarr`
   - **Port:** `8989`
   - **API Key:** *(paste the API key from Sonarr → Settings → General)*
   - **Default Quality Profile:** choose your preferred profile
   - **Default Root Folder:** `/media/tv`
   - **Default Language Profile:** English (or your preference)
3. Click **Test** → **Add Server**.

---

## Step 5 — Finish Setup

1. Click **Finish Setup**.
2. Seerr will scan your Plex library (if connected) and sync with Radarr/Sonarr.

---

## Step 6 — Invite Users (Optional)

1. Go to **Settings → Users**.
2. Click **Import Plex Users** (if using Plex) to let your friends/family log in with their Plex accounts and request content.
3. Set permissions as needed (e.g., auto-approve requests or require your approval).

---

## Step 7 — Test a Request

1. On the Seerr home page, search for any movie or TV show.
2. Click **Request**.
3. Check Radarr or Sonarr — the item should appear there automatically and begin searching.

---

## You're Done! 🎉

Your full media stack is now up and running. Here's a summary of all your UIs:

| Service | URL |
|---|---|
| qBittorrent | `http://localhost:8113` |
| NZBGet | `http://localhost:6789` |
| Prowlarr | `http://localhost:9696` |
| Radarr | `http://localhost:7878` |
| Sonarr | `http://localhost:8989` |
| Seerr | `http://localhost:5055` |

---

## Questions?

Head to the [Discussions → Questions](https://github.com/NolieRavioli/Windows-Docker-Mediastack/discussions) tab.

---

**[🏠 Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[⬅️ Sonarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Sonarr-Configuration)**
