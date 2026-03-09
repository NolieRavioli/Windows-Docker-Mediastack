# qBittorrentVPN Configuration

> Step-by-step configuration of qBittorrent after the containers are running.

Pick your setup:
- [qBittorrent-Only Stack](#-qbittorrent-only-stack)
- [Full Stack](#-full-stack-additional-steps)

**[🏠 Home](../)**  **[⬅️ Docker Compose Configuration](Docker-Compose-Configuration)**  **[➡️ NZBGet Configuration](NZBGet-Configuration)**

---

## 🧲 qBittorrent-Only Stack

### Step 1 — Open qBittorrent Web UI

1. Open a browser and go to: `http://localhost:8113`
2. Default login:
   - **Username:** `admin`
   - **Password:** `adminadmin`
   > Change this immediately after logging in (see Step 2).

---

### Step 2 — Change the Admin Password

1. In the qBittorrent Web UI, click the **gear icon** (Tools → Options).
2. Go to the **Web UI** tab.
3. Under **Authentication**, change the username and password to something secure.
4. Click **Save**.

---

### Step 3 — Verify the VPN is Working

1. In the Web UI, go to **Tools → Options → Advanced**.
2. Look at the **Network Interface** — it should show something like `wg0` (WireGuard) or `tun0` (OpenVPN), not your regular network adapter.
3. For a quick check, open a new browser tab and go to [https://ipleak.net](https://ipleak.net) — the IP address shown should **not** match your real home IP.
   > Make sure you're visiting the real ipleak.net and not a look-alike phishing site. The URL should start with `https://`.

> If the VPN isn't connecting, check `docker compose logs qtorrent` in PowerShell for error messages.

---

### Step 4 — Configure Download Folders

1. In the Web UI, go to **Tools → Options → Downloads**.
2. Set **Default Save Path** to `/data/complete`
3. Check **"Keep incomplete torrents in:"** and set it to `/data/incomplete`

> **Why?** The `/data` folder is mapped to your big storage drive. Keeping incomplete downloads in a separate folder prevents the Arr apps from trying to import a half-downloaded file.

4. Click **Save**.

---

### Step 5 — Configure ProtonVPN Port Forwarding (WireGuard)

ProtonVPN supports port forwarding on WireGuard servers (P2P servers). This gives you an open port for better torrent speeds.

1. After the container has been running for a minute or two, check the logs:
   ```powershell
   docker compose logs qtorrent | findstr "port"
   ```
2. Look for a line like: `Forwarded port: 12345`
3. In the qBittorrent Web UI, go to **Tools → Options → Connection**.
4. Set **Port used for incoming connections** to the forwarded port number from the logs.
5. Uncheck **"Use UPnP / NAT-PMP port forwarding"** (the VPN handles this).
6. Click **Save**.

---

### Step 6 — Set Up Categories (Optional but Recommended)

Categories tell qBittorrent (and the Arr apps later) where to save specific types of downloads.

1. Right-click in the left sidebar under **Categories**, click **Add category**.
2. Add:
   - Name: `movies` — Save Path: `/data/complete/movies`
   - Name: `tv` — Save Path: `/data/complete/tv`

---

## 🎬 Full Stack (Additional Steps)

> Complete all the steps above first, then continue here.

### Step 7 — Verify NZBGet is Accessible

1. Open `http://localhost:6789`
2. Log in with the `NZBGET_USER` and `NZBGET_PASS` you set in the compose file.
3. If it doesn't load, check the logs:
   ```powershell
   docker compose logs nzbget
   ```
   > Remember: NZBGet shares the VPN network with qBittorrent. If qBittorrent's VPN isn't up yet, NZBGet won't start either (it waits for the healthcheck to pass).

---

### Step 8 — Configure NZBGet Download Paths

1. In the NZBGet Web UI, go to **Settings → Paths**.
2. Set **MainDir** to `/data/nzbget`
3. Set **DestDir** to `/data/complete`
4. Set **InterDir** to `/data/incomplete`
5. Click **Save** and **Reload** when prompted.

---

## Troubleshooting

**Container keeps restarting** — Run `docker compose logs qtorrent` and look for VPN errors. Most common causes:
- Wrong `VPN_USER` or `VPN_PASS`
- Using your account password instead of the ProtonVPN VPN-specific password
- Forgot to add `+pmp` to the username

**"Permission denied" errors on volumes** — Check that your folder paths in the compose file actually exist. Docker will not create them automatically.

**WireGuard: "RTNETLINK answers: Operation not permitted"** — Make sure `privileged: true` is set and not commented out.

---

**[🏠 Home](../)**  **[⬅️ Docker Compose Configuration](Docker-Compose-Configuration)**  **[➡️ NZBGet Configuration](NZBGet-Configuration)**
