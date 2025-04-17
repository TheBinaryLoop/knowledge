---
tags:
    - Proxmox
    - Hetzner
---
# 🖥️ Install Proxmox on a Hetzner Dedicated Server (The Right Way)

When using Hetzner dedicated servers, the default install options via their “Installimage” are limited—particularly when it comes to using **ZFS as the root filesystem**. Here’s how to do it **properly** using a **Proxmox ISO and KVM console**.

---

## ✅ Prerequisites

Before starting:

- You’ve **ordered a dedicated server** from [Hetzner Robot](https://robot.your-server.de) or the [Server Auction](https://www.hetzner.com/sb)
- You have access to:
    - The **Hetzner Robot panel**
    - The ability to **request KVM access**
    - The **latest Proxmox ISO URL** (e.g., from [Proxmox downloads](https://www.proxmox.com/en/downloads))

---

## 🪜 Step-by-Step Guide

### 1. Access the Robot Panel

Once your server is active:

- Log in to [Hetzner Robot](https://robot.your-server.de)
- Navigate to your dedicated server
- Take note of:
    - IP address
    - Server model
    - Any RAID config (you may need to disable HW RAID later)

---

### 2. Request KVM Console + USB ISO Flash

In the Robot UI:

1. Go to **“Rescue” → “Request KVM Console”**
2. In the **Message** box, paste the **URL of the latest Proxmox ISO**, for example:

``` text
https://enterprise.proxmox.com/iso/proxmox-ve_8.4-1.iso
```

3. Submit the request.

**What happens next:**

- Hetzner support **flashes a USB stick** with your ISO.
- They **attach the USB** to your server.
- They enable a **KVM console** (via Java applet or HTML5 noVNC).

You’ll receive an email with access credentials.

---

### 3. Boot Into KVM & Install Proxmox

- Open the KVM console
- On boot, press the appropriate key (usually `F11` or `F12`) to select **boot device**
- Choose the **USB stick** flashed by Hetzner
- Start the **Proxmox installer**

---

### 4. Install Proxmox with ZFS

When prompted during install:

- Select **ZFS (RAID0 or Mirror)** as root filesystem
- Configure storage layout based on your drives
- Set hostname, IP, and password as needed

!!! tip

    If Hetzner preconfigured a RAID controller (e.g., H700), you might need to switch to AHCI mode in BIOS or delete existing RAID volumes for ZFS to detect disks properly.

---

### 5. Final Touches After Reboot

Once installed:

- Remove the USB install media via Robot or KVM
- Reboot into your fresh Proxmox install
- Update the system:

```bash
apt update && apt full-upgrade -y
```

- Add Proxmox subscription-free repo (optional):

```bash
sed -i "s|^deb https://enterprise.proxmox.com|#deb https://enterprise.proxmox.com|" /etc/apt/sources.list.d/pve-enterprise.list
echo "deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription" > /etc/apt/sources.list.d/pve-no-subscription.list
apt update
```

---

## 🛠️ Tips & Troubleshooting

| Issue | Fix |
|------|-----|
| Can’t see USB in boot menu | Ensure Hetzner has attached it and it’s prioritized in BIOS |
| RAID controller hides disks | Switch to AHCI in BIOS or remove RAID volumes |
| ZFS install fails | Check memory—ZFS requires 8GB+ for stability |
| Proxmox web UI not loading | Check IP config or firewall via console |

---

## 🔗 Related Links

- [Proxmox ISO Downloads](https://www.proxmox.com/en/downloads)
- [Hetzner Robot Panel](https://robot.your-server.de)
- [Proxmox Docs: Installation](https://pve.proxmox.com/wiki/Installation)

---

## 🙋 FAQ

**Q: Why not use Hetzner’s installimage?**  
A: It doesn’t support ZFS root by default and lacks full customization.

**Q: Can I use software RAID with ZFS?**  
A: Yes, and it’s preferred. Proxmox ZFS RAID is designed for this purpose.