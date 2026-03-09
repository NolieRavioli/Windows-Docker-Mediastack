# Windows Docker Mediastack

A guide to running a self-hosted media automation stack on Windows 11 using Docker Desktop and Docker Compose.

---

## 🐳 What is Docker?

Think of your computer as a building. Normally, every program you install lives directly in the building and shares the same walls, plumbing, and electricity. If one program breaks something, it can affect everything else.

**Docker** lets you install programs inside isolated units called **containers** — like shipping containers. Each container has everything it needs to run: its own mini operating system, its own files, its own settings. They don't interfere with each other, and they don't clutter up your Windows system. When you're done with a container, you just delete it.

---

## ⚡ Why Docker Compose?

This stack uses several apps that all need to talk to each other: a VPN-protected torrent downloader, a Usenet downloader, indexer managers, movie/TV automation tools, and a request interface. Setting all of these up manually — installing them, configuring them to find each other, making them all start on boot — would take days.

**Docker Compose** lets you describe your entire stack in a single text file. One command brings everything online. Another command tears it all down. It makes deploying a complex multi-app system incredibly simple compared to doing it the traditional way.

---

## 🚀 Choose Your Setup

This repo has two options:

| Setup | What's Included |
|---|---|
| **qBittorrent-Only** | qBittorrent behind a ProtonVPN WireGuard tunnel |
| **Full Stack** | Everything above + NZBGet, Prowlarr, Radarr, Sonarr, Seerr, FlareSolverr |

---

## What Each Container Does

| Container | Included In | What It Does |
|---|---|---|
| **qBittorrentVPN** | qBittorrent-Only, Full Stack | Torrent download client running behind a ProtonVPN/WireGuard tunnel so torrent traffic stays inside the VPN container. |
| **NZBGet** | Full Stack | Usenet download client for grabbing releases from your news server. In this stack it shares qBittorrent's VPN network. |
| **Prowlarr** | Full Stack | Indexer manager that connects your torrent and Usenet indexers to Radarr and Sonarr from one place. |
| **FlareSolverr** | Full Stack | Helper service used by Prowlarr for indexers protected by Cloudflare or similar anti-bot checks. |
| **Radarr** | Full Stack | Movie automation app that watches for wanted movies, sends downloads to your client, and imports completed files. |
| **Sonarr** | Full Stack | TV automation app that does the same job as Radarr, but for series and episodes. |
| **Seerr** | Full Stack | Request interface for users to ask for movies and shows, then forward approved requests to Radarr and Sonarr. |

---

### **Use the **[🏠 Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** to follow step-by-step instructions for installation**

### **Use the [Discussions](https://github.com/NolieRavioli/Windows-Docker-Mediastack/discussions/q-a) tab. There's a **Questions** category — ask anything there.**

---

### Install Docker Desktop

Follow the [Docker Desktop Installation wiki page](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Desktop-Installation) for step-by-step instructions on getting Docker running on Windows 11 with WSL2.

---

## 🧲 qBittorrent-Only Setup

### Download & Configure the Compose File

Download [`docker-compose.qbittorrent-only.yml`](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/docker-compose.qbittorrent-only.yml) from this repo and follow the [Docker Compose Configuration wiki page](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Compose-Configuration#qbittorrent-only-stack) to edit your paths, timezone, and VPN credentials.

### Configure qBittorrent

Once the container is running, follow the [qBittorrentVPN Configuration wiki page](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/qBittorrentVPN-Configuration) to:

- Change the default admin password
- Verify the VPN is active
- Set your download folders under the shared `/data/torrents` tree
- Create the qBittorrent categories used later by Radarr and Sonarr
- Configure ProtonVPN port forwarding for better speeds

---

## 🎬 Full Stack Setup

### Download & Configure the Compose File

Download [`docker-compose.full-stack.yml`](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/docker-compose.full-stack.yml) and follow the [Full Stack section of the Docker Compose Configuration wiki](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Compose-Configuration#full-stack) to edit paths, timezone, VPN credentials, and NZBGet login.

### Configure Each App

After running `docker compose up -d`, configure each app in order. Each one has a dedicated wiki page:

1. **[qBittorrentVPN Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/qBittorrentVPN-Configuration)** — Verify VPN, set download folders, configure ProtonVPN port forwarding
2. **[NZBGet Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/NZBGet-Configuration)** — Add your Usenet news server credentials
3. **[Prowlarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Prowlarr-Configuration)** — Add indexers and connect to Radarr/Sonarr
4. **[Radarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Radarr-Configuration)** — Set download client, root folder, quality profiles
5. **[Sonarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Sonarr-Configuration)** — Same as Radarr but for TV shows
6. **[Seerr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Overseerr-Configuration)** — Connect to Plex, Radarr, and Sonarr; invite users

### Full Stack URLs (once running)

| Service | URL |
|---|---|
| qBittorrent | `http://localhost:8113` |
| NZBGet | `http://localhost:6789` |
| Prowlarr | `http://localhost:9696` |
| Radarr | `http://localhost:7878` |
| Sonarr | `http://localhost:8989` |
| Seerr | `http://localhost:5055` |
