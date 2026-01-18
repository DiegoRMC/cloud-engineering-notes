# Home Server Setup: Initial Provisioning

This project documents the provisioning of a bare-metal home lab to simulate a production server environment. 
Its primary goal is to master headless Linux administration and containerization (Docker) as a practical foundation for Cloud/DevOps workflows.

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
After the first reboot, I performed the "Headless Test" and set up the initial environment:

1. **Remote Connection:** Disconnected monitor and keyboard.
2. **SSH Access:** Connected from main workstation: `ssh diego@192.168.1.XX`
3. **Essential Tools:** Updated repositories and installed the "Survival Kit" for monitoring and administration:
   ```bash
   sudo apt update
   sudo apt install htop git curl neofetch -y
## 4. Docker Engine Installation (Native Repository)
Instead of using the automated "convenience script", I manually configured the official Docker repository to ensure security, package integrity, and future updates via `apt`.

- **Prerequisites:** Verified `curl` and `ca-certificates` were installed.
- **Security (GPG):** Downloaded the official Docker GPG key to `/etc/apt/keyrings` to verify package signatures.
- **Repository:** Added the stable branch for Ubuntu `noble` (24.04) to `sources.list.d`.
- **Installation:** Installed the latest stable version of `docker-ce`, `docker-ce-cli`, and `containerd.io`.
- **User Permissions:** Added user `diego` to the `docker` group to allow running containers without `sudo` (Principle of Least Privilege).
   ```bash
   sudo usermod -aG docker $USER
- **Verification:** Validated the installation by running the test container:
  ```bash
  docker run hello-world
