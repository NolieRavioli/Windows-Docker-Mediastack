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
| **Full Stack** | Everything above + NZBGet, Prowlarr, Radarr, Sonarr, Overseerr, FlareSolverr |

Use the [Wiki](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/) to follow step-by-step instructions for installation

---

## Step 1 — Install Docker Desktop

Follow the [Docker Desktop Installation wiki page](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Desktop-Installation) for step-by-step instructions on getting Docker running on Windows 11 with WSL2.

---

## 🧲 qBittorrent-Only Setup

### Download & Configure the Compose File

Download [`docker-compose.qbittorrent-only.yml`](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/docker-compose.qbittorrent-only.yml) from this repo and follow the [Docker Compose Configuration wiki page](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Compose-Configuration#-qbittorrent-only-stack) to edit your paths, timezone, and VPN credentials.

### Configure qBittorrent

Once the container is running, follow the [qBittorrentVPN Configuration wiki page](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/qBittorrentVPN-Configuration#-qbittorrent-only-stack) to:

- Change the default admin password
- Verify the VPN is active
- Set your download folders:
  - **Incomplete downloads:** `/data/incomplete`
  - **Completed downloads:** `/data/complete`
- Configure ProtonVPN port forwarding for better speeds

---

## 🎬 Full Stack Setup

### Download & Configure the Compose File

Download [`docker-compose.full-stack.yml`](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/docker-compose.full-stack.yml) and follow the [Full Stack section of the Docker Compose Configuration wiki](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Compose-Configuration#-full-stack) to edit paths, timezone, VPN credentials, and NZBGet login.

### Configure Each App

After running `docker compose up -d`, configure each app in order. Each one has a dedicated wiki page:

1. **[qBittorrentVPN Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/qBittorrentVPN-Configuration)** — Verify VPN, set download folders, configure ProtonVPN port forwarding
2. **[NZBGet Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/NZBGet-Configuration)** — Add your Usenet news server credentials
3. **[Prowlarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Prowlarr-Configuration)** — Add indexers and connect to Radarr/Sonarr
4. **[Radarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Radarr-Configuration)** — Set download client, root folder, quality profiles
5. **[Sonarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Sonarr-Configuration)** — Same as Radarr but for TV shows
6. **[Overseerr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Overseerr-Configuration)** — Connect to Plex, Radarr, and Sonarr; invite users

### Full Stack URLs (once running)

| Service | URL |
|---|---|
| qBittorrent | `http://localhost:8113` |
| NZBGet | `http://localhost:6789` |
| Prowlarr | `http://localhost:9696` |
| Radarr | `http://localhost:7878` |
| Sonarr | `http://localhost:8989` |
| Overseerr | `http://localhost:5055` |

---

## 💬 Questions?

Use the [Discussions](https://github.com/NolieRavioli/Windows-Docker-Mediastack/discussions/q-a) tab. There's a **Questions** category — ask anything there.
