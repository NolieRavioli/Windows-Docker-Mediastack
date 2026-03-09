# Docker Desktop Installation on Windows 11

> Step-by-step. No theory — just do it.

**[🏠 Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[➡️ Docker Compose Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Compose-Configuration)**

---

## Prerequisites

- Windows 11 (64-bit)
- At least 8 GB RAM
- Virtualization enabled in BIOS (most modern PCs already have this on)

---

## Step 1 — Enable WSL 2

1. Open **Start**, search for **"Turn Windows features on or off"**, and open it.  
   ![wsl-start-menu.png](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/pics/wsl-start-menu.png)  

2. Check the box for **"Virtual Machine Platform"**.  
   ![wsl-virt-mach-platf.png](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/pics/wsl-virt-mach-platf.png)  
   
3. Check the box for **"Windows Subsystem for Linux"**.  
   ![wsl-win-subsys.png](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/pics/wsl-win-subsys.png)  
   
4. Click **OK** and let it install.  

5. When prompted, **restart your PC**.  

---

## Step 2 — Set WSL 2 as the Default Version

1. After rebooting, open **PowerShell** as Administrator.  
   - Right-click the Start button → **Windows PowerShell (Admin)**  
      ![powershell-right-start-menu.png](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/pics/powershell-right-start-menu.png)  

2. Run this command:
   ```powershell
   wsl --set-default-version 2
   ```  

3. Then update WSL to the latest version:
   ```powershell
   wsl --update
   ```

### Expected output:
```powershell
PS C:\Windows\system32> wsl --set-default-version 2
For information on key differences with WSL 2 please visit https://aka.ms/wsl2
The operation completed successfully.
PS C:\Windows\system32> wsl --update
Checking for updates.
The most recent version of Windows Subsystem for Linux is already installed.
```

---

## Step 3 — Download Docker Desktop

1. Go to **[https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)**
2. Click **"Download for Windows"**.
3. Run the installer (`Docker Desktop Installer.exe`).

---

## Step 4 — Install Docker Desktop

1. In the installer, make sure **"Use WSL 2 instead of Hyper-V"** is checked.  
   ![docker-desk-configuration.png](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/pics/docker-desk-configuration.png)  

2. Click **OK** and let it install.  

3. When the installer finishes, click **Close and restart**.

---

## Step 5 — First Launch

1. After reboot, Docker Desktop will start automatically (look for the whale icon in the system tray). ![docker-desk-ico.png](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/pics/docker-desk-ico.png)  

2. Accept the license agreement when prompted.  

3. Skip the tutorial/sign-in — you don't need a Docker account to use this.  
   ![docker-desk-login-skip.png](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/pics/docker-desk-login-skip.png)
4. Find and enable the integrated terminal in Docker Desktop.  
   ![docker-desk-terminal-button.png](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/pics/docker-desk-terminal-button.png)
   ![docker-desk-terminal-enable.png](https://github.com/NolieRavioli/Windows-Docker-Mediastack/blob/main/pics/docker-desk-terminal-enable.png)

---

## Step 6 — Verify It Works

1. Run:
   ```powershell
   docker --version
   ```
   You should see something like `Docker version 26.x.x`.
2. Run:
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

**[🏠 Wiki Home](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/)** | **[➡️ Docker Compose Configuration](https://github.com/NolieRavioli/Windows-Docker-Mediastack/wiki/Docker-Compose-Configuration)**
