# Docker Compose (Full Stack Configuration)

Extract the repo, copy the full-stack template to `docker-compose.yml`, replace the placeholders, create the shared folder layout, and start the stack from PowerShell.

**[🏠 Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[⬅️ Docker Desktop Installation](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Desktop-Installation)** | **[➡️ qBittorrentVPN Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/qBittorrentVPN-Configuration)**

## Drive Strategy: Single Drive Recommended

The full stack (Radarr, Sonarr, Prowlarr) uses **hardlinks** for efficient file management. Hardlinks work best when all data lives on the **same filesystem**. 

**Single Drive (Recommended):** All downloads and media on one drive (e.g., `D:/MediaStack/`) enables instant hardlinks when Radarr/Sonarr move files from `/data/torrents/` to `/data/media/`.

**Multiple Drives:** If you have separate drives for TV and Movies, hardlinks won't work—files will be copied instead, consuming extra disk space and time. If you prefer multiple drives, either:
- Consolidate to a single drive now using Windows Storage Spaces
- Start with a single drive and manually move files to other drives later as storage fills up

## Before You Start

- Use `docker compose`, not `docker-compose`.
- Use PowerShell or Command Prompt, not Portainer, for starting and stopping the stack.
- The examples below assume you extracted the repo to `C:\Users\YourName\Windows-Docker-Mediastack`.

## Common Steps

### Step 1 - Download and Extract the Repo

1. Download [Windows-Docker-Mediastack](https://github.com/NolieRavioli/Windows-Docker-Mediastack/tree/main) from GitHub with the green `<> Code` button.  
   ![github-repo-code.png](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/pics/github-repo-code.png)
2. Click **Download ZIP** and extract it where you want the stack to live, for example `C:\Users\YourName\Windows-Docker-Mediastack`.  
   ![github-zip.png](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/pics/github-zip.png)

### Step 2 - Open PowerShell in the Repo Root

```powershell
cd 'C:\Users\YourName\Windows-Docker-Mediastack'
```

### Step 3 - Copy the Full-Stack Template

```powershell
Copy-Item -Force .\docker-compose.full-stack.yml .\docker-compose.yml
```

### Step 4 - Create the Shared Folder Layout

All data goes under a single root on one drive. This `D:\MediaStack\` example shows the complete single-drive structure:

```text
D:\MediaStack\
  data\
    torrents\
      movies\
      tv\
      music\
    media\
      movies\
      tv\
      music\
    usenet\
      complete\
      incomplete\
  media\
```

Create it with PowerShell:

```powershell
$root = 'D:\MediaStack'
New-Item -ItemType Directory -Force -Path `
  "$root\data\torrents\movies", `
  "$root\data\torrents\tv", `
  "$root\data\torrents\music", `
  "$root\data\media\movies", `
  "$root\data\media\tv", `
  "$root\data\media\music", `
  "$root\data\usenet\complete", `
  "$root\data\usenet\incomplete", `
  "$root\media"
```

`$root\media` is still created because the current compose file mounts it, even though the Arr app pages intentionally use `/data/media/...` for the hardlink-friendly layout.

### Step 5 - Replace the Placeholders

Open `docker-compose.yml` in VS Code or Notepad and replace these values:

| Find | Replace with | Example |
| --- | --- | --- |
| `/path/to/your/hard/drive/BIG/storage/` | Your single drive root for all data | `D:/MediaStack/` |
| `America/Denver` | Your timezone | `America/New_York` |
| `192.168.1.0/24` | Your LAN subnet | `192.168.0.0/24` |
| `<<<YOUR_PROTONVPN_USERNAME>>>` | Your ProtonVPN VPN username | `abc123456` |
| `<<<YOUR_PROTONVPN_VPN_PASSWORD>>>` | Your ProtonVPN VPN password | `correct-horse-battery-staple` |
| `<<<CHOOSE_A_USERNAME>>>` | The NZBGet username you want to use | `nzbget` |
| `<<<CHOOSE_A_PASSWORD>>>` | The NZBGet password you want to use | `change-me-now` |

Keep the `./config/...` paths unchanged—they stay under the extracted repo folder.

With the example above, the main bind mounts become:

- `D:/MediaStack/data:/data` (all downloads, imports, and media)
- `D:/MediaStack/media:/media` (legacy mount, still used by compose file)

All Radarr and Sonarr root folder paths should use `/data/media/...` for hardlink compatibility.

### Step 6 - Download and Place the Proton WireGuard Config

Use the same WireGuard folder layout as the qBittorrent-only stack:

```powershell
New-Item -ItemType Directory -Force -Path .\config\qbittorrent\wireguard
Move-Item -Force 'C:\Users\YourName\Downloads\your-proton-config.conf' .\config\qbittorrent\wireguard\wg0.conf
```

If you want OpenVPN instead of WireGuard, make the same `VPN_CLIENT`, `cap_add`, `devices`, `privileged`, and `sysctls` changes described in the qBittorrent-only section above.

### Step 7 - Start the Stack

```powershell
cd 'C:\Users\YourName\Windows-Docker-Mediastack'
docker compose up -d
docker compose ps
```

On the first run Docker will download all images, so expect this to take a few minutes.

### Step 8 - Open the Web UIs

Use `localhost` on the Windows host. If you are connecting from another device, use your PC's IP address instead.

| Service | URL |
| --- | --- |
| qBittorrent | `http://localhost:8113` |
| NZBGet | `http://localhost:6789` |
| Radarr | `http://localhost:7878` |
| Sonarr | `http://localhost:8989` |
| Prowlarr | `http://localhost:9696` |
| Seerr | `http://localhost:5055` |

### Step 9 - Verify the Containers Started

```powershell
docker compose ps
docker compose logs qbittorrent
docker compose logs radarr
docker compose logs sonarr
```

If `qbittorrent` is not healthy yet, wait before configuring the rest of the stack.

**[🏠 Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[⬅️ Docker Desktop Installation](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Desktop-Installation)** | **[➡️ qBittorrentVPN Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/qBittorrentVPN-Configuration)**
