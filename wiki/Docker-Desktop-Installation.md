# Docker Desktop Installation on Windows 11

> Step-by-step. No theory — just do it.

---

## Prerequisites

- Windows 11 (64-bit)
- At least 8 GB RAM
- Virtualization enabled in BIOS (most modern PCs already have this on)

---

## Step 1 — Enable WSL 2

1. Open **Start**, search for **"Turn Windows features on or off"**, and open it.
2. Check the box for **"Windows Subsystem for Linux"**.
3. Check the box for **"Virtual Machine Platform"**.
4. Click **OK** and let it install.
5. When prompted, **restart your PC**.

---

## Step 2 — Set WSL 2 as the Default Version

1. After rebooting, open **PowerShell** as Administrator.
   - Right-click the Start button → **Windows PowerShell (Admin)**
2. Run this command:
   ```powershell
   wsl --set-default-version 2
   ```
3. Then update WSL to the latest version:
   ```powershell
   wsl --update
   ```

---

## Step 3 — Download Docker Desktop

1. Go to **[https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)**
2. Click **"Download for Windows"**.
3. Run the installer (`Docker Desktop Installer.exe`).

---

## Step 4 — Install Docker Desktop

1. In the installer, make sure **"Use WSL 2 instead of Hyper-V"** is checked.
2. Click **OK** and let it install.
3. When the installer finishes, click **Close and restart**.

---

## Step 5 — First Launch

1. After reboot, Docker Desktop will start automatically (look for the whale icon in the system tray).
2. Accept the license agreement when prompted.
3. Skip the tutorial/sign-in — you don't need a Docker account to use this.

---

## Step 6 — Verify It Works

1. Open **PowerShell** (no need for admin this time).
2. Run:
   ```powershell
   docker --version
   ```
   You should see something like `Docker version 26.x.x`.
3. Run:
   ```powershell
   docker run hello-world
   ```
   You should see a "Hello from Docker!" message. If you do, you're good to go. ✅

---

## Troubleshooting

**"WSL 2 installation is incomplete"** — Run `wsl --update` in PowerShell (Admin) and restart Docker Desktop.

**Docker icon not appearing in system tray** — Open Docker Desktop from the Start menu manually.

**Virtualization error** — You may need to enable virtualization in your BIOS/UEFI. Google your specific motherboard model + "enable virtualization BIOS".

---

## Next Step

➡️ **[Docker Compose Configuration](Docker-Compose-Configuration)**
