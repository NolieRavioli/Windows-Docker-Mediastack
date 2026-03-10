# Seerr Configuration

Seerr is the request UI that sits on top of Radarr and Sonarr. Users request content there, then the Arr apps do the actual searching and importing.

**[🏠 Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[⬅️ Sonarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Sonarr-Configuration)**

## Step 1 - Open Seerr

Open `http://localhost:5055` and start the setup wizard.

## Step 2 - Sign In

If you use Plex, sign in with Plex and select your server.

If you do not use Plex yet, create the local admin account first and finish Plex integration later.

## Step 3 - Connect Radarr

Add a Radarr server with:

- **Hostname or IP:** `radarr`
- **Port:** `7878`
- **API Key:** copy it from Radarr -> **Settings -> General**
- **Default Root Folder:** `/data/media/movies`

Test it before saving.

## Step 4 - Connect Sonarr

Add a Sonarr server with:

- **Hostname or IP:** `sonarr`
- **Port:** `8989`
- **API Key:** copy it from Sonarr -> **Settings -> General**
- **Default Root Folder:** `/data/media/tv`

Test it before saving.

## Step 5 - Finish Setup

Complete the wizard, then submit a test request and confirm it appears in Radarr or Sonarr.

## Note

Use the same root folders here that you used in Radarr and Sonarr. In this repo's updated wiki flow, that means `/data/media/...`, not `/media/...`.

**[🏠 Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[⬅️ Sonarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Sonarr-Configuration)**
