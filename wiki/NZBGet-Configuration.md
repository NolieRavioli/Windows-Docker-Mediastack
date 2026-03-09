# NZBGet Configuration

NZBGet is optional in this repo. The transcript focused on qBittorrent, but if you want Usenet alongside torrents, use these settings so the paths stay organized under the same shared `/data` tree.

**[Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[qBittorrentVPN Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/qBittorrentVPN-Configuration)** | **[Prowlarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Prowlarr-Configuration)**

## Step 1 - Open NZBGet

Open `http://localhost:6789` and log in with the `NZBGET_USER` and `NZBGET_PASS` values from your compose file.

## Step 2 - Add Your Usenet Server

1. Go to **Settings -> News-Servers**.
2. Add the server details from your Usenet provider.
3. Save and reload.
4. Use the built-in test button to confirm the server works.

## Step 3 - Set the Paths

Go to **Settings -> Paths** and set:

- **MainDir:** `/data/usenet`
- **DestDir:** `/data/usenet/complete`
- **InterDir:** `/data/usenet/incomplete`

Save and reload when prompted.

## Step 4 - Let Prowlarr Manage Indexers

If you are using the full stack, do not manually add a lot of NZB indexers here. Configure them once in Prowlarr and let Prowlarr sync them outward.

## Step 5 - Connection Details for Other Apps

Because NZBGet shares the qBittorrent service network in this compose file, other containers should connect to it like this:

- Host: `qbittorrent`
- Port: `6789`

Use those values if you add NZBGet as a download client in Prowlarr, Radarr, or Sonarr.

**[Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[qBittorrentVPN Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/qBittorrentVPN-Configuration)** | **[Prowlarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Prowlarr-Configuration)**
