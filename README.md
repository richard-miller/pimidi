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

## 20221227 - No GUI

### Raspberry Pi OS Lite
```
Raspberry PI Imager

Raspberry Pi OS Lite (32 bit)
Advance Option:
  hostname: pimidi
  enable ssh
  username: pi
  password: <password>
  Wifi:
    SID: abuklea
    password: <password>
```

Insert SD into PI and power up (first boot will take several mins)

### Apply updates

Insert SD into PI and power up (first boot will take a couple of mins)

```
ssh pi@pimidi.local (or find IP address from local router)
```

Apply updates (will take approx 15 mins)
```
sudo apt update
sudo apt upgrade -y
sudo reboot
```
### Inject ssh key
```
ssh pi@pimidi.local  'mkdir ~/.ssh; echo '`cat ~/.ssh/id_rsa.pub`' >> ~/.ssh/authorized_keys'
```

### Add pi to the audio group (maybe already a member)
```
sudo usermod -a -G audio pi
```

### Give audio real time permissions
```
sudo vi /etc/security/limits.d/audio.conf
```
```
@audio - rtprio 80
@audio - memlock unlimited
```

### Disable internal Audio
```
sudo vi /boot/config.txt
```
```
# dtparam=audio=on
```

### Enable USB audio
```
sudo vi /etc/asound.conf
```
```
pcm.!default {
  type hw card 1
}
ctl.!default {
  type hw card 1
}
```

### Reboot
```
sudo reboot
```

### Test
```
aplay -l
lsusb -t
speaker-test -c2 -twav
```

## Build FluidSynth

### Pre-requsists
sudo apt-get install git libgtk2.0-dev cmake cmake-curses-gui build-essential libasound2-dev telnet -y

### Clone it
git clone git://git.code.sf.net/p/fluidsynth/code-git

### Build and install
```
cd code-git/fluidsynth
mkdir build
cd build
cmake ..
sudo make install
```
### Get fonts
```
sudo apt-get install fluid-soundfont-gm
```

### Install NODE
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash

 



## Alt method withour Rasperry Pi Imager - failed

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

# PasteBin
UzIRjeVNyVMEzXcbKwCTBAag4_RZNf4_
353339cf7407c7e107271db43cd0918c

curl -X POST \
  -d 'api_dev_key=UzIRjeVNyVMEzXcbKwCTBAag4_RZNf4_' \
  -d 'api_user_key=353339cf7407c7e107271db43cd0918c' \
  -d 'api_paste_code=IP: '$(hostname -I | awk '{print $1}') \
  -d 'api_option=paste' \
  -d' api_paste_name=PIMIDI' \
  "https://pastebin.com/api/api_post.php"
  

