# Radarr Configuration

Radarr watches for movies, sends grabs to your download client, and imports completed files into your library.

**[Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[Prowlarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Prowlarr-Configuration)** | **[Sonarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Sonarr-Configuration)**

## Step 1 - Open Radarr

Open `http://localhost:7878`.

Finish the initial setup page, then go to **Settings -> General** and copy the API key. You will need it for Prowlarr and Seerr.

## Step 2 - Set the Root Folder and Import Behavior

1. Go to **Settings -> Media Management**.
2. Click **Show Advanced**.
3. Add this root folder:

```text
/data/media/movies
```

4. Confirm these settings:

   - **Use Hardlinks instead of Copy:** on
   - **Rename Movies:** optional, but recommended
   - **Import Extra Files:** include `srt`, `sub`, and `nfo` if you want subtitles and metadata imported

5. Save your changes.

Use `/data/media/movies`, not `/media/movies`, so Radarr imports from the same shared `/data` tree that qBittorrent uses.

## Step 3 - Add qBittorrent as a Download Client

1. Go to **Settings -> Download Clients**.
2. Add **qBittorrent**.
3. Set:

   - **Host:** `qbittorrent`
   - **Port:** `8113`
   - **Username:** your qBittorrent username
   - **Password:** your qBittorrent password
   - **Category:** `movies`
   - **Use SSL:** off

4. Test and save.

The category must match the qBittorrent category you created earlier.

## Step 4 - Add NZBGet If You Use Usenet

If you are using NZBGet as well:

1. Add **NZBGet** under **Settings -> Download Clients**.
2. Set:

   - **Host:** `qbittorrent`
   - **Port:** `6789`
   - **Username:** your NZBGet username
   - **Password:** your NZBGet password
   - **Category:** `movies`

3. Test and save.

## Step 5 - Add Radarr Back Into Prowlarr

If you have not already done it in the Prowlarr page:

1. Open Prowlarr.
2. Go to **Settings -> Apps**.
3. Add **Radarr** with:

   - **Prowlarr Server:** `http://prowlarr:9696`
   - **Radarr Server:** `http://radarr:7878`
   - **API Key:** the key you copied from Radarr

4. Test and save.

## Step 6 - Test an Import

1. Add a movie in Radarr.
2. Let qBittorrent finish downloading it.
3. Check **Activity** in Radarr.
4. When the import succeeds, the movie should appear under:

```text
/data/media/movies
```

while the original torrent data remains under:

```text
/data/torrents/movies
```

If that happens, your category mapping and import path are correct.

## Common Problems

**Radarr cannot talk to qBittorrent**

- Use `qbittorrent`, not `localhost`, for the host field.
- Make sure qBittorrent is already healthy before testing.

**Imports fail or copy instead of hardlinking**

- Keep the root folder under `/data/media/...`.
- Keep qBittorrent downloads under `/data/torrents/...`.
- Make sure the qBittorrent category is exactly `movies`.

**[Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[Prowlarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Prowlarr-Configuration)** | **[Sonarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Sonarr-Configuration)**
