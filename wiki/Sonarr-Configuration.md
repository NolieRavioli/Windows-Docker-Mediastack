# Sonarr Configuration

Sonarr does the same job as Radarr, but for TV shows and episodes.

**[Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[Radarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Radarr-Configuration)** | **[Seerr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Seerr-Configuration)**

## Step 1 - Open Sonarr

Open `http://localhost:8989`.

Finish the initial setup page, then go to **Settings -> General** and copy the API key. You will need it for Prowlarr and Seerr.

## Step 2 - Set the Root Folder and Import Behavior

1. Go to **Settings -> Media Management**.
2. Click **Show Advanced**.
3. Add this root folder:

```text
/data/media/tv
```

4. Confirm these settings:

   - **Use Hardlinks instead of Copy:** on
   - **Rename Episodes:** optional, but recommended
   - **Import Extra Files:** include `srt`, `sub`, and `nfo` if you want subtitles and metadata imported

5. Save your changes.

Use `/data/media/tv`, not `/media/tv`, so Sonarr imports from the same shared `/data` tree that qBittorrent uses.

## Step 3 - Add qBittorrent as a Download Client

1. Go to **Settings -> Download Clients**.
2. Add **qBittorrent**.
3. Set:

   - **Host:** `qbittorrent`
   - **Port:** `8113`
   - **Username:** your qBittorrent username
   - **Password:** your qBittorrent password
   - **Category:** `tv`
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
   - **Category:** `tv`

3. Test and save.

## Step 5 - Add Sonarr Back Into Prowlarr

If you have not already done it in the Prowlarr page:

1. Open Prowlarr.
2. Go to **Settings -> Apps**.
3. Add **Sonarr** with:

   - **Prowlarr Server:** `http://prowlarr:9696`
   - **Sonarr Server:** `http://sonarr:8989`
   - **API Key:** the key you copied from Sonarr

4. Test and save.

## Step 6 - Test an Import

1. Add a series in Sonarr.
2. Let qBittorrent finish downloading the first episode pack or episode.
3. Check **Activity** in Sonarr.
4. When the import succeeds, the files should appear under:

```text
/data/media/tv
```

while the original torrent data remains under:

```text
/data/torrents/tv
```

If that happens, your category mapping and import path are correct.

## Common Problems

**Sonarr cannot talk to qBittorrent**

- Use `qbittorrent`, not `localhost`, for the host field.
- Make sure qBittorrent is already healthy before testing.

**Imports fail or copy instead of hardlinking**

- Keep the root folder under `/data/media/...`.
- Keep qBittorrent downloads under `/data/torrents/...`.
- Make sure the qBittorrent category is exactly `tv`.

**[Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[Radarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Radarr-Configuration)** | **[Seerr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Seerr-Configuration)**
