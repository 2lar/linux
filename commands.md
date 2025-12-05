# 🐧 Ubuntu HomeLab / WSL Command Cheatsheet

## 🧹 Disk Hygiene & Cleanup (Crucial for WSL)
*Tools to reclaim space when your "virtual disk" gets too full.*

| Command | Purpose |
| :--- | :--- |
| `ncdu` | **Interactive Disk Usage Tool.** The best way to visualize what folders are eating your space. Use arrow keys to navigate and `d` to delete. |
| `du -h --max-depth=1 \| sort -rh` | **Quick Scan.** Lists the largest folders in your current directory. |
| `npx npkill` | **Project Cleaner.** Interactively finds and deletes heavy `node_modules` folders from old coding projects. |
| `npm cache clean --force` | Clears the local **NPM cache** (often 5GB+). |
| `sudo journalctl --vacuum-time=2d` | **Log Vacuum.** Deletes system logs older than 2 days. |
| `sudo apt clean` | Removes **cached package files** (.deb) from `/var/cache/apt/archives`. |

---

## 🧠 AI & LLM Management (Ollama)
*Managing local models like Llama 3, Qwen, or DeepSeek.*

| Command | Purpose |
| :--- | :--- |
| `ollama list` | **Inventory.** Lists all downloaded models and their sizes (GB). |
| `ollama ps` | **Active Check.** Shows which model is currently loaded in RAM/VRAM. |
| `ollama rm modelname` | **Delete.** Removes a specific model (e.g., `ollama rm qwen2.5-coder:14b`). |
| `ollama pull modelname` | **Download.** Downloads a model without running it immediately. |
| `ollama stop modelname` | **Stop.** Unloads a running model to free up RAM. |

---

## 🐳 Docker Management

| Command | Purpose |
| :--- | :--- |
| `docker system df` | **Disk Usage.** Shows exactly how much space images, containers, and volumes are using. |
| `docker system prune` | **Standard Cleanup.** Removes stopped containers and dangling images (safe). |
| `docker system prune -a --volumes` | **Nuclear Cleanup.** ⚠️ Deletes **ALL** images not currently running and **ALL** volumes. Use only when resetting. |
| `docker stats` | **Live Monitor.** Like "Task Manager" for your containers (CPU/RAM usage). |
| `docker-compose up -d` | **Start Stack.** Starts your services in the background (Detached). |
| `docker-compose pull` | **Update.** Downloads the latest version of the images in your stack. |

---

## 🪟 Windows-Side Commands (PowerShell Admin)
*Run these in Windows PowerShell to shrink the WSL virtual hard drive file.*

| Command | Purpose |
| :--- | :--- |
| `wsl --shutdown` | **Stop Linux.** Kills all running WSL distributions immediately. |
| `wsl --list --verbose` | **Status Check.** Confirms if Ubuntu is `Running` or `Stopped`. |
| `diskpart` | **Disk Partition Tool.** Opens the utility needed to compact the VHDX file. |

**Disk Shrink Sequence (in `diskpart`):**
1. `select vdisk file="C:\Users\YOUR_USER\AppData\Local\...\ext4.vhdx"`
2. `compact vdisk`
3. `exit`

---

## 🔐 Networking & File Sharing

| Command | Purpose |
| :--- | :--- |
| `tailscale ip -4` | **Get IP.** Shows your server's private Tailscale IP (access from anywhere). |
| `tailscale status` | **Network Check.** Shows other devices connected to your mesh. |
| `sudo service smbd restart` | **Restart SMB.** Applies changes made to `/etc/samba/smb.conf`. |
| `htop` | **Task Manager.** Visual process viewer (Ctrl+C to exit). |

---

## 🕵️ Tailscale Exit Node & Security
*Commands to manage the "Personal VPN" feature.*

| Command | Purpose |
| :--- | :--- |
| `sudo tailscale up --advertise-exit-node` | **Enable.** Tells Tailscale "I want to be an exit node." (Requires Admin Console approval). |
| `sudo tailscale up --advertise-exit-node=false` | **Disable.** Stops acting as an exit node immediately. |
| `tailscale status` | **Check Peers.** Shows who is connected to you. Look for "active" devices. |
| `tailscale netcheck` | **Health Check.** Verifies your connection to the encrypted relay servers. |
| `sudo tailscale logout` | **Kill Switch.** Completely disconnects the server from the Tailscale network. |

### 🚨 How to COMPLETELY Revert Exit Node Settings
If you want to "factory reset" the network settings we changed:

1. **Turn off the feature:**
   `sudo tailscale up --advertise-exit-node=false --reset`

2. **Delete the IP Forwarding rule:**
   `sudo rm /etc/sysctl.d/99-tailscale.conf`

3. **Reload System Networking:**
   `sudo sysctl -p`
   *(Or just `sudo reboot` to be sure).*
