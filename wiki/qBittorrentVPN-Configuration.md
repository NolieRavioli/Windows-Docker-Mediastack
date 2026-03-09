# qBittorrentVPN Configuration

These steps apply to both the qBittorrent-only stack and the full stack. The full stack depends on the category setup here, so do not skip the Downloads and Categories sections.

**[Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[Docker Compose Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Compose-Configuration)** | **[NZBGet Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/NZBGet-Configuration)**

## Step 1 - Open qBittorrent

Open:

- `http://localhost:8113` from the Windows host
- `http://<YOUR_PC_IP>:8113` from another device on your network

Default login for this image:

- Username: `admin`
- Password: `adminadmin`

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
- **Default Save Path:** `/data/torrents`
- Enable **Use Subcategories**
- Enable **Use Category Paths in Manual Mode** if your qBittorrent build shows it

Click **Save**.

Do not point qBittorrent at `/data/media`. Downloads should land in `/data/torrents/...` and then be imported by Radarr or Sonarr.

## Step 5 - Create Categories

In the left sidebar, right-click **Categories** and add these:

| Category | Save Path |
| --- | --- |
| `movies` | `movies` |
| `tv` | `tv` |
| `music` | `music` |

The category names must exactly match what you put into Radarr and Sonarr later.

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

**WireGuard permission errors**

- Leave `privileged: true` enabled when using WireGuard.

**Categories are missing later**

- Create them before you configure Radarr and Sonarr.

**[Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[Docker Compose Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Compose-Configuration)** | **[NZBGet Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/NZBGet-Configuration)**
