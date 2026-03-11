# qBittorrentVPN Configuration

These steps apply to both the qBittorrent-only stack and the full stack. The full stack depends on the category setup here, so do not skip the Downloads and Categories sections.

**[🏠 Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[⬅️ Docker Compose qBittorrent-Only Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Compose-qBittorrent-Only-Configuration)** **OR** **[⬅️ Docker Compose Full Stack Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Compose-Full-Configuration)** | **[➡️ NZBGet Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/NZBGet-Configuration)**

## Drive Configuration Reference

### Single Drive (Full Stack Recommended)

All paths under one root:

```text
D:\MediaStack\
  data\
    torrents\      ← Save path in qBittorrent
      movies\      ← Category: movies
      tv\          ← Category: tv
      music\       ← Category: music
    media\         ← Final home for Radarr/Sonarr imports
      movies\
      tv\
      music\
```

Configure docker-compose with: `/path/to/your/hard/drive/BIG/storage/ = D:/MediaStack/`

### Multiple Drives (qBittorrent-Only Friendly)

Separate drives for different media:

```text
T:\              ← TV drive
  Downloads\     ← Category: tv save path
  Library\       ← Final TV library

G:\              ← Movie drive
  Downloads\     ← Category: movies save path
  Library\       ← Final Movie library

D:\
  Downloads\     ← Category: music save path
  Library\       ← Final Music library
```

Mount each drive separately in docker-compose.

---
 | **[➡️ NZBGet Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/NZBGet-Configuration)**

## Step 1 - Open qBittorrent

Open:

- `http://localhost:8113` from the Windows host
- `http://<YOUR_PC_IP>:8113` from another device on your network

Default login for this image:

- Username: `admin`
- Password: randomly generated and written to `/config/supervisord.log`

If you are using the example host layout from this repo, check the password in:

```text
C:\Users\YourName\docker\...\config\qbittorrent\supervisord.log
```

If the container never becomes healthy and the logs say no WireGuard config was found in `/config/wireguard/`, stop here and add the Proton WireGuard `.conf` file first. The current `binhex/arch-qbittorrentvpn` image needs that file for Proton WireGuard.

## Step 2 - Change the Web UI Login

1. Go to **Tools -> Options -> Web UI**.
2. Change the username and password to something you will keep.
3. Click **Save**.

## Step 3 - Verify the VPN and Port Forwarding

1. Go to **Tools -> Options -> Advanced**.
2. Confirm the network interface is the VPN interface, not your normal adapter.
3. In PowerShell, check the logs:

```powershell
docker compose logs qbittorrent
```

If you are using ProtonVPN port forwarding, look for the forwarded port in the logs and set it under **Tools -> Options -> Connection**. Turn off UPnP/NAT-PMP inside qBittorrent after you set the forwarded port.

## Step 4 - Configure Downloads

Go to **Tools -> Options -> Downloads** and change these values:

- **Default Torrent Management Mode:** `Automatic`
- **Default Save Path:** Choose based on your drive setup:
  - **Single Drive:** `/data/torrents` (same path for all categories)
  - **Multiple Drives:** `/downloads` (or mount each drive and point here)
- Enable **Use Subcategories**
- Enable **Use Category Paths in Manual Mode** if your qBittorrent build shows it

Click **Save**.

**Important:** Do not point qBittorrent at `/data/media` or your media drive library folder. Downloads should land in a separate `/data/torrents/` (single drive) or `/downloads/` (multiple drives) and then be imported by Radarr/Sonarr from there. This protects your library from incomplete or corrupted files.

## Step 5 - Create Categories

In the left sidebar, right-click **Categories** and add these. The save paths depend on your drive setup:

### Single Drive Setup

| Category | Save Path |
| --- | --- |
| `movies` | `movies` |
| `tv` | `tv` |
| `music` | `music` |

These relative paths place downloads in `/data/torrents/movies/`, `/data/torrents/tv/`, etc.

### Multiple Drive Setup

If using separate drives (e.g., `T:/`, `G:/`, `D:/`), configure categories to save directly to your per-drive folders. Example for TV on `T:/` drive:

| Category | Save Path |
| --- | --- |
| `tv` | `Downloads` (mounts to `T:/Downloads/`) |
| `movies` | `Downloads` (mounts to `G:/Downloads/`) |
| `music` | `Downloads` (mounts to `D:/Downloads/`) |

Adjust the category names to match exactly what you'll use in Radarr and Sonarr later. The category names must be consistent across all Arr apps.

## Step 6 - Full Stack Check

If you are running the full stack:

1. Confirm qBittorrent stays up and healthy.
2. Confirm NZBGet is reachable at `http://localhost:6789`.
3. Then move on to Prowlarr, Radarr, and Sonarr.

## Troubleshooting

**Web UI does not load**

- Check `docker compose ps` and make sure `qbittorrent` is running.
- Confirm `LAN_NETWORK` in the compose file matches your home subnet.

**Container keeps restarting**

- Run `docker compose logs qbittorrent`.
- Wrong ProtonVPN credentials and missing `+pmp` on `VPN_USER` are the most common causes.
- If the logs say no config exists in `/config/wireguard/`, create a host folder named `wireguard` under your qBittorrent config path and place your Proton `.conf` file there as `wg0.conf`.

**WireGuard permission errors**

- Leave `privileged: true` enabled when using WireGuard.

**Categories are missing later**

- Create them before you configure Radarr and Sonarr.

**[🏠 Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[⬅️ Docker Compose qBittorrent-Only Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Compose-qBittorrent-Only-Configuration)** **OR** **[⬅️ Docker Compose Full Stack Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Compose-Full-Configuration)** | **[➡️ NZBGet Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/NZBGet-Configuration)**
