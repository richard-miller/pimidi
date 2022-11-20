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
