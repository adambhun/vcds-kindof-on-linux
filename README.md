# vcds-kindof-on-linux
This repo shows you how to run the VCDS software on Linux (in Windows-in-Docker). Tested on Manjaro Linux.

# Prerequisites
- Docker
- git
- everything listed for the [dockur/windows](https://github.com/dockur/windows/) project
- Recommended: Docker compose
- Optional: Some RDP client for Linx, like [Rdesktop](https://wiki.archlinux.org/title/rdesktop)

# Usage
1. Connect your VAG-COM USB device to your computer.
2. Run the `lsusb` command.
3. You should get multiple lines of output, each one looking like this:
  `Bus 003 Device 005: ID 1a2c:4c5e China Resource Semico Co., Ltd USB Keyboard`
  - `003` is the ID of the port of your machine (PORT in docker-compose.yml)
  - `005` is the ID of the USBdevice (DEVICE in docker-compose.yml)
  - `1a2c` is the vendor ID (VENDOR_ID in docker-compose.yml)
  - `4c5e` is the product ID (PRODUCT_ID in docker-compose.yml)
  Replace the values encased in "<>" in the docker-compose.yml file with their respective values.
4. Copy the VCDS installer and any other files into `./storage/shared`
5. Run the `docker compose up` command.
6. Wait for the container to start.
7. Use the GUI of Windows in your container. You have two options:
7.A Web browser interface: visit localhost:8006 in a browser.
7.B RDP:
  Run these commands:
  ```
  CONTAINER_IP="$(docker inspect <container_name_or_id> | jq -r '.[].NetworkSettings.Networks[].IPAddress')"
  rdesktop -u docker $CONTAINER_IP
  ```
8. Login to windows with the username 'docker' and no password.
9. Open File Explorer and click on the Network section, you will see a computer called host.lan, double-click it and it will show a folder called Data. You may need to change some network discovery related stuff in Windows to actually see it.
Inside this folder you can access any files that are placed in /storage/shared (see above) on the host.
10. Install VCDS, the required drivers and do stuff.


Notes:
- The device ID may be a different number for each time you plug in a USB device.
- Using `/dev/bus/usb` # ! using this is not an option


## Tips
### Resource wasting
If you don't want to go through the whole installation and configuration process, but don't want the container to waste resources in the background, use the `docker compose stop` and `docker compose restart` commands.

### Unplugging and reconnecting your VAG-COM device
If you do this, the device ID may change.
In that case, you have to update the device ID in your docker-compose.yml file as described in the "Usage" section and run the `docker compose restart` command.

# Acknowledgements
This project is heavily based on [dockur/windows](https://github.com/dockur/windows/). I would like to thank the original authors for their groundbreaking work and for inspiring this derivative project.

# Disclaimer
The product names, logos, brands, and other trademarks referred to within this project are the property of their respective trademark holders.
This project is not affiliated, sponsored, or endorsed by Ross-Tech, the Microsoft Corporation, or any other entities.
