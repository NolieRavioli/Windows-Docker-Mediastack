# Prowlarr Configuration

Prowlarr manages your indexers and pushes them into Radarr and Sonarr so you only have to maintain them in one place.

**[🏠 Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[⬅️ NZBGet Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/NZBGet-Configuration)** | **[➡️ Radarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Radarr-Configuration)**

## Step 1 - Open Prowlarr

Open `http://localhost:9696`.

On first launch, create your admin username and password.

## Step 2 - Add qBittorrent as a Download Client

1. Go to **Settings -> Download Clients**.
2. Click **Add** and choose **qBittorrent**.
3. Set:

   - **Host:** `qbittorrent`
   - **Port:** `8113`
   - **Username:** your qBittorrent username
   - **Password:** your qBittorrent password
   - **Use SSL:** off

4. Click **Test** and make sure you get a green check.
5. Click **Save**.

You can leave the category blank here unless you want a separate manual-grab category.

## Step 3 - Add NZBGet If You Use Usenet

If you also use NZBGet:

1. Go to **Settings -> Download Clients**.
2. Add **NZBGet**.
3. Set:

   - **Host:** `qbittorrent`
   - **Port:** `6789`
   - **Username:** your NZBGet username
   - **Password:** your NZBGet password
   - **Use SSL:** off unless you changed it yourself

4. Test and save.

## Step 4 - Add Indexers

1. Go to **Indexers -> Add Indexer**.
2. Add the trackers or NZB indexers you actually use.
3. Test each one before saving it.

If you want a harmless test source first, use a legal/public-domain source such as Internet Archive if it is available in your Prowlarr build.

## Step 5 - FlareSolverr for Protected Indexers

Only do this if one of your indexers requires it.

1. Go to **Settings -> Indexers**.
2. Find the FlareSolverr section.
3. Set the URL to `http://flaresolverr:8191`.
4. Save.

## Step 6 - Connect Prowlarr to Radarr and Sonarr

Do this after you have copied the API keys from Radarr and Sonarr.

For Radarr:

1. Go to **Settings -> Apps**.
2. Add **Radarr**.
3. Set:

   - **Prowlarr Server:** `http://prowlarr:9696`
   - **Radarr Server:** `http://radarr:7878`
   - **API Key:** copy it from Radarr -> **Settings -> General**

4. Test and save.

For Sonarr:

1. Add **Sonarr**.
2. Set:

   - **Prowlarr Server:** `http://prowlarr:9696`
   - **Sonarr Server:** `http://sonarr:8989`
   - **API Key:** copy it from Sonarr -> **Settings -> General**

3. Test and save.

## Step 7 - Verify Sync

After Radarr and Sonarr are connected:

1. Check **Radarr -> Settings -> Indexers**.
2. Check **Sonarr -> Settings -> Indexers**.
3. Your Prowlarr indexers should appear there automatically.

**[🏠 Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[⬅️ NZBGet Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/NZBGet-Configuration)** | **[➡️ Radarr Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Radarr-Configuration)**
