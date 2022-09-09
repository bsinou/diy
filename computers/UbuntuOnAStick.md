# Get Ubuntu on a USB-Stick

## How to

- Retrieve latest Desktop image from the [official Ubuntu website](https://releases.ubuntu.com/)

```ssh
sudo add-apt-repository universe
sudo add-apt-repository ppa:mkusb/ppa
sudo apt update
sudo apt install --install-recommends mkusb mkusb-nox usb-pack-efi
cd ~/dev/vms/isos/
sudo mkusb ubuntu-22.04.1-desktop-amd64.iso 

```

## Links

- rather using mkusb: https://ostechnix.com/how-to-create-persistent-live-usb-on-ubuntu/
 and https://help.ubuntu.com/community/mkusb
- If you want to use `usb-creator-gtk`: https://linuxconfig.org/create-bootable-ubuntu-22-04-usb-startup-disk