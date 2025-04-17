---
tags:
    - Proxmox
    - OPNsense
    - VXLAN
    - Networking
---

# 🔀 Setup a VXLAN between OPNsense and Proxmox

When running a hybrid homelab or datacenter environment, it's often necessary to **bridge Layer 2 networks across Layer 3 boundaries**—especially between **firewall appliances like OPNsense** and **hypervisors like Proxmox**. VXLAN (Virtual eXtensible LAN) is a powerful solution that enables this by encapsulating Ethernet frames in UDP packets.

In this guide, you'll learn how to **create a stable VXLAN tunnel** between OPNsense and Proxmox, allowing seamless connectivity between VMs and networks across different physical locations or VLANs.

!!! warning

    VXLAN traffic is not encrypted. Always make sure that the traffic between the different systems is properly protected, for example by using a VPN



---

## ✅ Prerequisites

Before starting:

- You have a stable connection between the Proxmox Server and the OPNsense firewall
- You have access to:
    - the Proxmox Server (SSH and WebUI)
    - the OPNsense WebUI

---

## 🪜 Step-by-Step Guide