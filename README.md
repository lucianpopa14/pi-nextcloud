
# Nextcloud DIY NAS on Raspberry Pi

## Overview
This guide documents the process of building a DIY NAS using a Raspberry Pi to host Nextcloud, replacing cloud services like Google Photos and Microsoft OneDrive.

## Hardware Requirements
- **Raspberry Pi 5** (4GB or 8GB model)
- **SATA SSDs** (for storage, configured in RAID 1 for redundancy)
- Radxa Penta SATA HAT (allows to connect 4+1 SATA drives to Raspberry Pi)
- Power supply and cooling solutions for Raspberry Pi
- Ethernet cable for stable network connection

## Software Requirements
- **Raspberry Pi OS** (64-bit recommended)
- **Nextcloud** (for self-hosted cloud storage)
- **Apache/Nginx** (web server)
- **PHP** (required for Nextcloud)
- **MySQL/MariaDB** (database for Nextcloud)
- **RAID tools** (for configuring RAID 1)

## Setup Process
### 0. Hardware setup
![IMG_9428](https://github.com/user-attachments/assets/58ea3a6a-b1e7-403b-b7f6-89760c3035ff)
![IMG_9428](https://github.com/user-attachments/assets/83461cfb-71e9-4f64-b7f9-0ad441672d0a)
![IMG_9431](https://github.com/user-attachments/assets/e14235bb-d9b4-4e5f-b45a-5b7c2c5212be)
![IMG_9432](https://github.com/user-attachments/assets/be245b9d-d543-4329-be55-20fcc0954b8f)
![IMG_9433](https://github.com/user-attachments/assets/16bc2d22-0d73-4ebf-a5fd-6e8bd9e50d07)
![IMG_9436](https://github.com/user-attachments/assets/a8372df7-312f-4178-a018-2ad29ad15d6a)

### 1. Prepare the Raspberry Pi
1. **Install Raspberry Pi OS**: Flash the OS to a microSD card using Raspberry Pi Imager.
2. **Initial Setup**: Boot the Raspberry Pi, configure network settings, and update the system:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

### 2. Configure RAID 1
1. **Connect Drives**: Attach your SATA SSDs to the Raspberry Pi using a USB-to-SATA adapter or enclosure.
2. **Install mdadm**:
   ```bash
   sudo apt install mdadm -y
   ```
3. **Create RAID Array**:
   ```bash
   sudo mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sda /dev/sdb
   ```
4. **Format and Mount**:
   ```bash
   sudo mkfs.ext4 /dev/md0
   sudo mkdir /mnt/nas
   sudo mount /dev/md0 /mnt/nas
   ```

### 3. Install Nextcloud
1. **Install Apache/Nginx and PHP**:
   ```bash
   sudo apt install apache2 mariadb-server php php-mysql php-gd php-curl php-mbstring php-intl php-imagick php-xml php-zip php-apcu php-redis -y
   ```
2. **Download Nextcloud**:
   ```bash
   wget https://download.nextcloud.com/server/releases/latest.tar.bz2
   tar -xjf latest.tar.bz2 -C /var/www/
   ```
3. **Configure Apache/Nginx**: Set up a virtual host for Nextcloud.
4. **Set Permissions**:
   ```bash
   sudo chown -R www-data:www-data /var/www/nextcloud
   ```
5. **Run Setup**: Access Nextcloud via your browser and complete the setup.

## Troubleshooting
- **RAID Issues**: Check `/proc/mdstat` for RAID status.
- **Nextcloud Errors**: Review Apache/Nginx and PHP logs.
- **Plex Problems**: Ensure the Plex service is running and ports are open.

## Conclusion
This setup provides a self-hosted, secure, and accessible NAS for family photos, videos, and media streaming. Customize as needed for your specific use case.

## License
This guide is open-source and available for personal and educational use.
