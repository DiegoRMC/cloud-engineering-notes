# Home Server Setup: Initial Provisioning

This project documents the provisioning of a bare-metal home lab to simulate a production server environment.
It's primary goal is to master headless Linux administration and containerization (Docker) as a practical foundation for Cloud/DevOps workflows.

**Date:** 2026-01-16
**Hardware:** Lenovo ThinkCentre M700 Tiny
**CPU:** Intel Core i3-6100T
**RAM:** 8GB DDR4
**Storage:** 128GB SSD (SATA)
**OS:** Ubuntu Server 24.04 LTS

## 1. Hardware Configuration (BIOS)
Before installation, the following BIOS adjustments were necessary to allow booting from USB and ensure compatibility:
- **Secure Boot:** Disabled (Required for non-Windows OS).
- **Boot Priority:** Set USB (Ventoy) as primary boot device.

## 2. OS Installation Process
Used **Ventoy** to boot the Ubuntu Server ISO.

- **Disk Partitioning:** Used "Entire Disk" option (LVM group enabled).
- **Network:** Configured via DHCP during install.
  - *Initial IP identified:* `192.168.1.XX` (Noted for SSH access).
- **User Setup:** Created administrative user `diego`.
- **SSH Server:** Selected `[x] Install OpenSSH server` during the wizard to enable headless management immediately after first boot.

## 3. Post-Installation & Headless Handoff
After the first reboot, I performed the "Headless Test":
1. Disconnected monitor and keyboard.
2. Connected via SSH from main workstation: ssh diego@192.168.1.XX
