# pimidi


Create a bootable SD

```
Raspberry PI Imager

Raspberry OS (32 bit)
Advance Option:
  hostname: pimidi
  enable ssh
  username: pi
  password: <password>
  Wifi:
    SID: abuklea
    password: <password>
```

Insert SD into PI and power up (first boot will take a couple of mins)

ssh
```
ssh pi@pimidi.local (or find IP address from local router)
```

Apply updates (will take approx 15 mins)
```
sudo apt update
sudo apt upgrade -y
sudo reboot
```

Enable VNC
```
ssh to pi@pimidi.local (or find IP address from local router)
sudo raspi-config
  Interface Options -> VNC -> Yes -> Finish
 
Test connectivity with VNC client -> pimidi.local
```

Give the audio group real time priority
```
sudo bash -c  "(echo @audio - rtprio 80; echo @audio - memlock unlimited) > /etc/security/limits.d/audio.conf"
```

Connect USB hub via otg cable and connect USB audio dongle to usb hub and test:
```
speaker-test -c2 -twav
```

## 20221227

### Download RasPi OS and uncompress
```
wget https://downloads.raspberrypi.org/raspios_lite_armhf/images/raspios_lite_armhf-2022-09-26/2022-09-22-raspios-bullseye-armhf-lite.img.xz
gunzip 2022-09-22-raspios-bullseye-armhf-lite.img.xz
```

### Write to SD (use "diskutil list" to find dev)
```
diskutil list
sudo dd bs=1m if=2022-09-22-raspios-bullseye-armhf-lite.img of=/dev/rdisk2
```
eject and insert SSD to cause it to mount

### Enable SSH
```
touch /Volumes/boot/ssh
```

### Inject Wifi credentials
```
sudo vi /Volumes/boot/wpa_supplicant.conf
```
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
 ssid="abuklea" 
 psk="********"
}
```
eject, transfer SD to PI and power up

### Connect to PI and apply updates
```
ssh pi@192.160.20.37  # Use wifi router to determine IP - MAC: B8:27:EB:xx:xx:xx
sudo apt update
sudo apt upgrade -y
```
