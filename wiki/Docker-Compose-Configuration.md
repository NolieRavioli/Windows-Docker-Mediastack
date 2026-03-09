# Docker Compose Configuration

Extract the repo, copy the stack template you want to `docker-compose.yml`, replace the placeholders, create the shared folder layout, and start the stack from PowerShell.

**[🏠 Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[⬅️ Docker Desktop Installation](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Desktop-Installation)** | **[➡️ qBittorrentVPN Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/qBittorrentVPN-Configuration)**

## Before You Start

- Use `docker compose`, not `docker-compose`.
- Use PowerShell or Command Prompt, not Portainer, for starting and stopping the stack.
- The examples below assume you extracted the repo to `C:\Users\YourName\Windows-Docker-Mediastack`.

## Common Setup

### Step 1 - Download and Extract the Repo

1. Download [Windows-Docker-Mediastack](https://github.com/NolieRavioli/Windows-Docker-Mediastack/tree/main) from GitHub with the green `<> Code` button.
   ![github-repo-code.png](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/pics/github-repo-code.png)
2. Click **Download ZIP** and extract it where you want the stack to live, for example `C:\Users\YourName\Windows-Docker-Mediastack`.
   ![github-zip.png](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/pics/github-zip.png)

### Step 2 - Open PowerShell in the Repo Root

```powershell
cd 'C:\Users\YourName\Windows-Docker-Mediastack'
```

## qBittorrent-Only Stack

### Step 3 - Copy the qBittorrent-Only Template

```powershell
Copy-Item -Force .\docker-compose.qbittorrent-only.yml .\docker-compose.yml
```

### Step 4 - Replace the Placeholders

Open `docker-compose.yml` in VS Code or Notepad and replace these values:

| Find | Replace with | Example |
| --- | --- | --- |
| `/path/to/your/hard/drive/BIG/storage/` | Your download root on the large drive | `D:/MediaStack/` |
| `America/Denver` | Your timezone | `America/New_York` |
| `192.168.1.0/24` | Your LAN subnet | `192.168.0.0/24` |
| `<<<YOUR_PROTONVPN_USERNAME>>>` | Your ProtonVPN VPN username | `abc123456` |
| `<<<YOUR_PROTONVPN_VPN_PASSWORD>>>` | Your ProtonVPN VPN password | `correct-horse-battery-staple` |

Leave `+pmp` attached to `VPN_USER` if you want ProtonVPN port forwarding.

Go to [account.protonvpn.com](https://account.protonvpn.com/dashboardV2) to get your ProtonVPN `VPN_USER` and `VPN_PASS`.
![proton-vpn-creds.png](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/pics/proton-vpn-creds.png)

### Step 5 - Download and Place the Proton WireGuard Config

For the current `binhex/arch-qbittorrentvpn` image, ProtonVPN WireGuard needs a Proton `.conf` file in the qBittorrent config folder.

1. Download the WireGuard config you want from [account.protonvpn.com](https://account.protonvpn.com/dashboardV2).
   ![proton-wg-conf.png](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/pics/proton-wg-conf.png)  

2. Create the target folder under the repo root:

```powershell
New-Item -ItemType Directory -Force -Path .\config\qbittorrent\wireguard
```

3. Move and rename the downloaded file to `wg0.conf`:

```powershell
Move-Item -Force 'C:\Users\YourName\Downloads\your-proton-config.conf' .\config\qbittorrent\wireguard\wg0.conf
```

### Step 6 - Create the qBittorrent Download Folders

If you want the same category layout used later by Radarr and Sonarr, create these folders before you start the container:

```powershell
$root = 'D:\MediaStack\data\torrents'
New-Item -ItemType Directory -Force -Path `
  "$root\movies", `
  "$root\tv", `
  "$root\music"
```

### Step 7 - Optional: Switch from WireGuard to OpenVPN

The template is set up for WireGuard by default. If you want OpenVPN instead:

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

If you stay on WireGuard, leave the file as-is.

### Step 8 - Start the Stack

```powershell
cd 'C:\Users\YourName\Windows-Docker-Mediastack'
docker compose up -d
docker compose ps
```

Open qBittorrent at:

- `http://localhost:8113` from the Windows host
- `http://<YOUR_PC_IP>:8113` from another device on your network

## Full Stack

### Step 3 - Copy the Full-Stack Template

```powershell
Copy-Item -Force .\docker-compose.full-stack.yml .\docker-compose.yml
```

### Step 4 - Create the Shared Folder Layout

This repo uses one shared data tree so qBittorrent downloads and Arr imports stay under the same root.

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
| `/path/to/your/hard/drive/BIG/storage/` | Your storage root on the large drive | `D:/MediaStack/` |
| `America/Denver` | Your timezone | `America/New_York` |
| `192.168.1.0/24` | Your LAN subnet | `192.168.0.0/24` |
| `<<<YOUR_PROTONVPN_USERNAME>>>` | Your ProtonVPN VPN username | `abc123456` |
| `<<<YOUR_PROTONVPN_VPN_PASSWORD>>>` | Your ProtonVPN VPN password | `correct-horse-battery-staple` |
| `<<<CHOOSE_A_USERNAME>>>` | The NZBGet username you want to use | `nzbget` |
| `<<<CHOOSE_A_PASSWORD>>>` | The NZBGet password you want to use | `change-me-now` |

Leave the `./config/...` paths alone. They keep your container configs under the extracted repo folder.

With the example above, the main bind mounts become:

- `D:/MediaStack/data:/data`
- `D:/MediaStack/media:/media`

Later app configuration pages should still use `/data/media/...` for Radarr and Sonarr root folders.

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
