# Windows Docker Mediastack Wiki Home

Use these pages in order. The Docker/Windows setup is specific to this repo, while the app-side configuration below follows the same Arr workflow described in the transcript: shared folders, qBittorrent categories, and Radarr/Sonarr imports from the same `/data` tree.

## 📋 Setup Order

1. **[Docker Desktop Installation](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Desktop-Installation)** - Install Docker Desktop and WSL2 on Windows 11.

2. **Choose your stack** and follow the corresponding Docker Compose configuration:
   - **[Docker Compose (qBittorrent-Only Configuration)](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Compose-qBittorrent-Only-Configuration)** - Extract the repo, copy the qBittorrent-only stack template to `docker-compose.yml`, create the folder structure, and launch. Works well with **multiple drives** (e.g., `T:/` for TV, `G:/` for Movies).
   - **[Docker Compose (Full Stack Configuration)](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Compose-Full-Configuration)** - Extract the repo, copy the full-stack template to `docker-compose.yml`, create the folder structure, and launch. **Recommended to use a single drive** for easier hardlinking and Arr workflow management.

3. **[qBittorrentVPN Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/qBittorrentVPN-Configuration)** - Change the default login, verify the VPN, and create the categories used by the Arr apps.

### Full Stack Only

4. **[NZBGet Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/NZBGet-Configuration)** - Optional Usenet client setup.
5. **[Prowlarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Prowlarr-Configuration)** - Add indexers and wire Prowlarr into the rest of the stack.
6. **[Radarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Radarr-Configuration)** - Configure movie imports, hardlinks, and qBittorrent integration.
7. **[Sonarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Sonarr-Configuration)** - Configure TV imports, hardlinks, and qBittorrent integration.
8. **[Seerr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Seerr-Configuration)** - Optional request UI for users.

## 💾 Drive Configuration

### qBittorrent-Only Stack

Works with **single or multiple drives**. You can keep downloads on one drive and media on another without issues.

Example with multiple drives:
- `T:/` for TV downloads and library
- `G:/` for Movie downloads and library

### Full Stack (Radarr + Sonarr + Prowlarr)

**Recommended: Use a single drive.** The Arr suite uses hardlinks for efficient file management, which works best when all data (`/data`) is on the same filesystem.

If you start with multiple drives and later want to consolidate:
1. Use Windows Storage Spaces to create a single virtual drive
2. Or manually migrate files to a single drive as storage fills up

Example with single drive:
- `D:/MediaStack/` for all downloads and media (torrents, usenet, movies, TV, music)

## Notes

- Use `docker compose`, not the old `docker-compose` binary.
- If you open the web UIs from another device on your network, replace `localhost` with your Windows PC's IP address.

## 💬 Questions?

Use the [Discussions -> Questions](https://github.com/NolieRavioli/Windows-Docker-Mediastack/discussions) tab for help.
