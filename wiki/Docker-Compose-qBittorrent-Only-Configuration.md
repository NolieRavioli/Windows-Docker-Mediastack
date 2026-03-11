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

---

**[🏠 Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[⬅️ Docker Desktop Installation](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Desktop-Installation)** | **[➡️ qBittorrentVPN Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/qBittorrentVPN-Configuration)**
