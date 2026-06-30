# Proxmox VE Setup

**Version:** Proxmox VE 8.x  
**Hardware:** Intel i5-8600K, ASUS Strix Z370-H Gaming, 32GB DDR4, 2x 256GB NVMe

---

## BIOS Configuration

Before installing Proxmox, the following BIOS settings were configured:

- **Virtualization (VT-x):** Enabled
- **VT-d (IOMMU):** Enabled — required for GPU passthrough later
- **Above 4G Decoding:** Enabled — required for GPU passthrough; without this the VM fails to properly map the GPU's memory
- **Resizable BAR:** Disabled — the GTX 1080 (Pascal architecture) does not support it

> BIOS settings may vary by motherboard. On ASUS Strix Z370-H Gaming these are found under **Advanced → CPU Configuration** and **Advanced → System Agent Configuration**.

---

## Creating a Bootable USB

Downloaded Proxmox VE 8.x ISO from [proxmox.com](https://www.proxmox.com/en/downloads) and flashed it to a USB drive using [Rufus](https://rufus.ie).

---

## Installation

1. Boot from USB
2. Select **Install Proxmox VE (Graphical)**
3. Accept EULA
4. **Target disk:** 
5. **Locale:** `
6. Set root password and email
7. **Network configuration:**
   - Hostname: 
   - IP Address: 
   - Gateway: 
   - DNS: 
8. Complete installation and reboot

---

## Post-Installation

## Kernel Version Note

Proxmox originally shipped with kernel 6.8.12-9-pve, later auto-updated to 6.8.12-29-pve. This kernel version is incompatible with NVIDIA's last Pascal-supporting driver (525.x) — see GPU passthrough documentation for the full downgrade procedure to kernel 6.5.13-1-pve.

### Access the Web UI

Navigate to `https://<proxmox-ip>:8006` in a browser and log in as `root`.

### Switch to No-Subscription Repository (optional)

Proxmox by default uses the enterprise repository which requires a subscription. For homelab use, switch to the free no-subscription repo:

```bash
# Disable enterprise repo
echo "# disabled" > /etc/apt/sources.list.d/pve-enterprise.list

# Add no-subscription repo
echo "deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription" \
  > /etc/apt/sources.list.d/pve-no-subscription.list

# Update
apt update && apt full-upgrade -y
```

### Storage Configuration

The system uses 2x 256GB NVMe drives:
- NVMe 1: Proxmox host OS and ISO storage
- NVMe 2: VM and container disk storage

---

## Notes

`<!-- TODO: any quirks or issues encountered during installation -->`
