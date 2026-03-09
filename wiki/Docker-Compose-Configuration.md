# Docker Compose Configuration

Download the compose file, replace the placeholders, create the shared folder layout, and start the stack with the CLI.

**[Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[Docker Desktop Installation](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Desktop-Installation)** | **[qBittorrentVPN Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/qBittorrentVPN-Configuration)**

## Before You Start

- Use `docker compose`, not `docker-compose`.
- Use PowerShell or Windows Terminal, not Portainer, for starting and stopping the stack.
- If you are building the full stack, create the folder tree first so qBittorrent, Radarr, and Sonarr all work from the same shared `/data` mount.

## qBittorrent-Only Stack

### Step 1 - Download the Compose File

1. Open [docker-compose.qbittorrent-only.yml](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/docker-compose.qbittorrent-only.yml).
2. Click **Raw** and save it as `docker-compose.yml` in a folder such as:

```text
C:\Users\YourName\docker\qbittorrent-only\
```

### Step 2 - Replace the Placeholders

Open the file in VS Code or Notepad++ and replace these values:

| Find | Replace with | Example |
| --- | --- | --- |
| `/change/to/path/to/your/configuration/storage/for/` | Your config root on the fast drive | `/mnt/c/Users/YourName/docker/` |
| `/path/to/your/hard/drive/BIG/storage/` | Your media/download root on the large drive | `/mnt/d/MediaStack/` |
| `America/Denver` | Your timezone | `America/New_York` |
| `192.168.1.0/24` | Your LAN subnet | `192.168.0.0/24` |

Windows drive letters become WSL-style paths inside Docker Desktop:

- `C:\` -> `/mnt/c/`
- `D:\` -> `/mnt/d/`

### Step 3 - Create the qBittorrent Download Folders

If you want the same category layout used later by Radarr and Sonarr, create these folders on the large drive before you start the container:

```powershell
$root = "D:\MediaStack\data\torrents"
New-Item -ItemType Directory -Force -Path `
  "$root\movies", `
  "$root\tv", `
  "$root\music"
```

### Step 4 - Set Your ProtonVPN Credentials

Find these lines:

```yaml
- VPN_USER=<<<YOUR_PROTONVPN_USERNAME>>>+pmp
- VPN_PASS=<<<YOUR_PROTONVPN_VPN_PASSWORD>>>
```

- `VPN_USER` is your ProtonVPN username with `+pmp` appended.
- `VPN_PASS` is your ProtonVPN VPN password, not your account password.

### Step 5 - WireGuard vs OpenVPN

The compose file is set up for WireGuard by default.

If you want OpenVPN instead:

1. Comment out:

```yaml
privileged: true
sysctls:
    net.ipv4.conf.all.src_valid_mark: 1
```

2. Uncomment:

```yaml
cap_add:
    - NET_ADMIN
devices:
    - /dev/net/tun:/dev/net/tun
```

3. Change `VPN_CLIENT=wireguard` to `VPN_CLIENT=openvpn`.

### Step 6 - Start the Stack

```powershell
cd C:\Users\YourName\docker\qbittorrent-only
docker compose up -d
docker compose ps
```

qBittorrent will be available at `http://localhost:8113`.

## Full Stack

### Step 1 - Download the Compose File

1. Open [docker-compose.full-stack.yml](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/docker-compose.full-stack.yml).
2. Click **Raw** and save it as `docker-compose.yml` in a folder such as:

```text
C:\Users\YourName\docker\full-stack\
```

### Step 2 - Create the Shared Folder Layout

The transcript uses one shared data tree so qBittorrent downloads and Arr imports stay under the same root. On Windows, create that tree first.

Example host layout:

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

You can create it with PowerShell:

```powershell
$root = "D:\MediaStack"
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

`$root\media` is still created because the current compose file mounts it, even though the Arr steps below intentionally use `/data/media/...` for the hardlink-friendly layout.

### Step 3 - Replace the Placeholders

Open the file in VS Code or Notepad++ and replace these values:

| Find | Replace with | Example |
| --- | --- | --- |
| `/change/to/path/to/your/configuration/storage/for/` | Your config root on the fast drive | `/mnt/c/Users/YourName/docker/` |
| `/path/to/your/hard/drive/BIG/storage/` | Your storage root on the large drive | `/mnt/d/MediaStack/` |
| `America/Denver` | Your timezone | `America/New_York` |
| `192.168.1.0/24` | Your LAN subnet | `192.168.0.0/24` |

With the example above, these bind mounts end up like this:

- `/mnt/d/MediaStack/data:/data`
- `/mnt/d/MediaStack/media:/media`

### Step 4 - Set Your ProtonVPN Credentials

Find these lines:

```yaml
- VPN_USER=<<<YOUR_PROTONVPN_USERNAME>>>+pmp
- VPN_PASS=<<<YOUR_PROTONVPN_VPN_PASSWORD>>>
```

Set them the same way as the qBittorrent-only stack.

### Step 5 - Set NZBGet Credentials

Find these lines:

```yaml
- NZBGET_USER=<<<CHOOSE_A_USERNAME>>>
- NZBGET_PASS=<<<CHOOSE_A_PASSWORD>>>
```

These are credentials you choose yourself.

### Step 6 - Start the Stack

```powershell
cd C:\Users\YourName\docker\full-stack
docker compose up -d
docker compose ps
```

On the first run Docker will download all images, so expect this to take a few minutes.

### Step 7 - Open the Web UIs

Use `localhost` on the Windows host. If you are connecting from another device, use your PC's IP address instead.

| Service | URL |
| --- | --- |
| qBittorrent | `http://localhost:8113` |
| NZBGet | `http://localhost:6789` |
| Radarr | `http://localhost:7878` |
| Sonarr | `http://localhost:8989` |
| Prowlarr | `http://localhost:9696` |
| Seerr | `http://localhost:5055` |

### Step 8 - Verify the Containers Started

Useful commands:

```powershell
docker compose ps
docker compose logs qbittorrent
docker compose logs radarr
docker compose logs sonarr
```

If `qbittorrent` is not healthy yet, wait before configuring the rest of the stack.

**[Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[Docker Desktop Installation](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Desktop-Installation)** | **[qBittorrentVPN Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/qBittorrentVPN-Configuration)**
