# Docker Compose Configuration

> How to download, edit, and run the compose file.

Pick your setup:
- [qBittorrent-Only Stack](#-qbittorrent-only-stack)
- [Full Stack](#-full-stack)

**[🏠 Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[⬅️ Docker Desktop Installation](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Desktop-Installation)** | **[➡️ qBittorrentVPN Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/qBittorrentVPN-Configuration)**

---

## 🧲 qBittorrent-Only Stack

### Step 1 — Download the Compose File

1. Go to the repo's [docker-compose.qbittorrent-only.yml](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/docker-compose.qbittorrent-only.yml).
2. Click the **Download raw file** button (or right-click Raw → Save As).
3. Save it somewhere easy to find, for example:
   ```
   C:\Users\YourName\docker\docker-compose.yml
   ```
   > Tip: Rename it to `docker-compose.yml` in whatever folder you put it in so Docker Compose can find it automatically.

---

### Step 2 — Edit the Compose File

Open the file in a text editor. **Notepad++** or **VS Code** are recommended — plain Notepad can break YAML formatting.

#### Use Find & Replace to set your paths

Press **`Ctrl+H`** to open Find & Replace, then swap out each placeholder:

| Find (placeholder) | Replace with | Example |
|---|---|---|
| `/change/to/path/to/your/configuration/storage/for/` | Path to your fast drive (SSD/NVMe) | `/mnt/c/Users/YourName/docker/` |
| `/path/to/your/hard/drive/BIG/storage/` | Path to your large storage drive | `/mnt/d/` |
| `America/Denver` | Your timezone | `America/New_York` |

> **Why two different drives?**
> Config files (small, read/write constantly) go on your fast NVMe/SSD so apps start quickly and don't stutter.
> Media files (huge) go on your big HDD where speed matters less. If you only have one drive, you can use the same path for both.

> **Windows drive paths in WSL2:** Your C: drive is `/mnt/c/`, your D: drive is `/mnt/d/`, etc.

---

### Step 3 — Set Your VPN Credentials

Find these lines in the file:

```yaml
- VPN_USER=<<<YOUR_PROTONVPN_USERNAME>>>+pmp
- VPN_PASS=<<<YOUR_PROTONVPN_VPN_PASSWORD>>>
```

- **`VPN_USER`** — Your ProtonVPN username, with `+pmp` appended at the end.
  - Example: `johnsmith+pmp`
  - The `+pmp` enables Port Mapping Protocol (port forwarding) on ProtonVPN — required for qBittorrent to work properly.

- **`VPN_PASS`** — This is **NOT** your ProtonVPN account password.
  ProtonVPN uses a separate VPN credential. To get it:
  1. Log in at [account.proton.me](https://account.proton.me)
  2. Go to **Account → OpenVPN / IKEv2 credentials** (or search the page for "VPN password")
  3. Copy the password shown there — paste it here.

---

### Step 4 — WireGuard vs OpenVPN

The compose file is set up for **WireGuard** by default (recommended — it's faster).

If you want to use **OpenVPN** instead:
1. Comment out the WireGuard block:
   ```yaml
   # privileged: true
   # sysctls:
   #     net.ipv4.conf.all.src_valid_mark: 1
   ```
2. Uncomment the OpenVPN block:
   ```yaml
   cap_add:
       - NET_ADMIN
   devices:
       - /dev/net/tun:/dev/net/tun
   ```
3. Change `VPN_CLIENT=wireguard` to `VPN_CLIENT=openvpn`.

---

### Step 5 — Understanding Ports

You'll see lines like this in the compose file:

```yaml
ports:
    - "8113:8113"
```

The format is **`HOST_PORT:CONTAINER_PORT`**.
- The **left** number is the port on **your Windows PC** — this is what you type in your browser.
- The **right** number is the port **inside the container** — don't change this unless you also change `WEBUI_PORT`.

To open the qBittorrent UI, you'd visit: `http://localhost:8113` or `http://<YOUR_PC_IP>:8113`

---

### Step 6 — Run the Stack

1. Open **PowerShell** and navigate to the folder where you saved your compose file:
   ```powershell
   cd C:\Users\YourName\docker
   ```
2. Start the stack:
   ```powershell
   docker compose up -d
   ```
3. Watch the containers start:
   ```powershell
   docker compose logs -f
   ```
   Press `Ctrl+C` to stop watching logs (containers keep running).

---

## 🎬 Full Stack

### Step 1 — Download the Compose File

1. Go to the repo's [docker-compose.full-stack.yml](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/docker-compose.full-stack.yml).
2. Click **Download raw file**.
3. Save it as `docker-compose.yml` in a folder on your PC.

---

### Step 2 — Edit the Compose File

Press **`Ctrl+H`** and replace the same three placeholders:

| Find | Replace with |
|---|---|
| `/change/to/path/to/your/configuration/storage/for/` | Your fast drive path (SSD/NVMe) |
| `/path/to/your/hard/drive/BIG/storage/` | Your large storage drive path |
| `America/Denver` | Your timezone |

This replaces them everywhere in the file at once — all services get the right paths in one go.

---

### Step 3 — Set Your VPN Credentials

Same as the qBittorrent-only instructions above — see [Set Your VPN Credentials](#step-3--set-your-vpn-credentials).

---

### Step 4 — Set NZBGet Credentials (Full Stack Only)

Find these lines:

```yaml
- NZBGET_USER=<<<CHOOSE_A_USERNAME>>>
- NZBGET_PASS=<<<CHOOSE_A_PASSWORD>>>
```

These are credentials **you choose** — NZBGet will require this login when you open its web UI. Pick anything you want.

---

### Step 5 — WireGuard vs OpenVPN

Same as the qBittorrent-only instructions — see [WireGuard vs OpenVPN](#step-4--wireguard-vs-openvpn).

Note: In the full stack, NZBGet shares the VPN container's network (`network_mode: "service:qbittorent"`), so its UI port (`6789`) is also exposed through the qBittorrent container. You don't need to do anything extra — it's already wired up.

---

### Step 6 — Understanding Ports (Full Stack)

Each service has its own port:

| Service | Port | URL |
|---|---|---|
| qBittorrent | 8113 | `http://localhost:8113` |
| NZBGet | 6789 | `http://localhost:6789` |
| Radarr | 7878 | `http://localhost:7878` |
| Sonarr | 8989 | `http://localhost:8989` |
| Prowlarr | 9696 | `http://localhost:9696` |
| Seerr | 5055 | `http://localhost:5055` |

---

### Step 7 — Run the Full Stack

```powershell
cd C:\Users\YourName\docker
docker compose up -d
```

The first time you run it, Docker will download all the images — this may take a few minutes depending on your internet speed.

---

**[🏠 Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[⬅️ Docker Desktop Installation](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Desktop-Installation)** | **[➡️ qBittorrentVPN Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/qBittorrentVPN-Configuration)**
