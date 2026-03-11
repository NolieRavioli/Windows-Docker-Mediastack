NEVER remove any emoji like 📋 💬 🏠 ⬅️ ➡️ ETC... from this repo.

Windows Docker Desktop uses Windows-style path (I still like to use `/` for windows: like `C:/Users/YourName/`)

`./wiki/` contains md documents for step-by-step installation and configuration of the docker media stack on windows.
  - `./wiki/Docker-Desktop-Installation.md`     STATUS: COMPLETE - DO NOT CHANGE
  - `./wiki/Docker-Compose-Configuration.md`    STATUS: COMPLETE - DO NOT CHANGE
  - `./wiki/qBittorrentVPN-Configuration.md`    STATUS: UNTESTED
  - `./wiki/NZBGet-Configuration.md`            STATUS: UNTESTED
  - `./wiki/Prowlarr-Configuration.md`          STATUS: UNTESTED
  - `./wiki/Radarr-Configuration.md`            STATUS: UNTESTED
  - `./wiki/Sonarr-Configuration.md`            STATUS: UNTESTED
  - `./wiki/Seerr-Configuration.md`             STATUS: UNTESTED

# Deployment notes

A windows user should never WRITE wg0.conf or things will break. the `.yml` files do not break when you edit them on windows.

use `./config/*` for the application files

currently using `T:/` and `G:/` for TV and Movies respectively.
    - The qbittorrent only stack should NOT make the user switch to a single data drive. Movies go in G and TV goes in T.
    - The Full stack should talk about the downside of multiple data drives and convice the reader to switch to a single data drive. Either through Storage Spaces Feature or only using a single drive for 'automatic' media, then manually mv the files over to the other drive when storage is filling up.

