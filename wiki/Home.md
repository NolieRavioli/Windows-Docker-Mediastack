# Windows Docker Mediastack Wiki Home

Use these pages in order. The Docker/Windows setup is specific to this repo, while the app-side configuration below follows the same Arr workflow described in the transcript: shared folders, qBittorrent categories, and Radarr/Sonarr imports from the same `/data` tree.

## Setup Order

1. **[Docker Desktop Installation](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Desktop-Installation)** - Install Docker Desktop and WSL2 on Windows 11.
2. **[Docker Compose Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Compose-Configuration)** - Download the compose file, create the folder structure, and launch the stack.
3. **[qBittorrentVPN Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/qBittorrentVPN-Configuration)** - Change the default login, verify the VPN, and create the categories used by the Arr apps.

### Full Stack

4. **[NZBGet Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/NZBGet-Configuration)** - Optional Usenet client setup.
5. **[Prowlarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Prowlarr-Configuration)** - Add indexers and wire Prowlarr into the rest of the stack.
6. **[Radarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Radarr-Configuration)** - Configure movie imports, hardlinks, and qBittorrent integration.
7. **[Sonarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Sonarr-Configuration)** - Configure TV imports, hardlinks, and qBittorrent integration.
8. **[Seerr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Seerr-Configuration)** - Optional request UI for users.

## Notes

- Use `docker compose`, not the old `docker-compose` binary.
- Use the CLI, not Portainer, for starting and stopping this stack.
- If you open the web UIs from another device on your network, replace `localhost` with your Windows PC's IP address.

## Questions

Use the [Discussions -> Questions](https://github.com/NolieRavioli/Windows-Docker-Mediastack/discussions) tab for help.
