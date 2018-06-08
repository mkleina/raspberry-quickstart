# raspberry-quickstart
Useful Raspberry Pi 3 commands, tips and tricks

## Add things to boot (easiest way):
```shell
sudo nano /etc/rc.local
```

## Reinstall ALSA (be careful):
```shell
sudo apt-get purge libasound2 libasound2-data alsa-base alsa-utils
sudo apt-get install libasound2 libasound2-data alsa-base alsa-utils
```

## Remove ALSA warnings
```shell
sudo nano /usr/share/alsa/alsa.conf
```

Comment out several devices:
```
# redirect to load-on-demand extended pcm definitions
pcm.cards cards.pcm

pcm.default cards.pcm.default
pcm.sysdefault cards.pcm.default
#pcm.front cards.pcm.front
#pcm.rear cards.pcm.rear
#pcm.center_lfe cards.pcm.center_lfe
#pcm.side cards.pcm.side
#pcm.surround21 cards.pcm.surround21
#pcm.surround40 cards.pcm.surround40
#pcm.surround41 cards.pcm.surround41
#pcm.surround50 cards.pcm.surround50
#pcm.surround51 cards.pcm.surround51
#pcm.surround71 cards.pcm.surround71
#pcm.iec958 cards.pcm.iec958
#pcm.spdif iec958
#pcm.hdmi cards.pcm.hdmi
pcm.dmix cards.pcm.dmix
pcm.dsnoop cards.pcm.dsnoop
#pcm.modem cards.pcm.modem
#pcm.phoneline cards.pcm.phoneline
```


## ALSA configuration: default audio jack + USB mic setting
```shell
sudo nano /etc/asound.conf
```
```
pcm.usb
{
    type hw
    card RECORD_DEVICE_NAME
}

pcm.internal
{
    type hw
    card PLAYBACK_DEVICE_NAME
}

pcm.!default
{
    type asym
    playback.pcm
    {
        type plug
        slave.pcm "internal"
    }
    capture.pcm
    {
        type plug
        slave.pcm "usb"
    }
}

ctl.!default
{
    type asym
    playback.pcm
    {
        type plug
        slave.pcm "internal"
    }
    capture.pcm
    {
        type plug
        slave.pcm "usb"
    }
}
```


## Install libssl 1.0 for Domoticz
```shell
wget http://ftp.nl.debian.org/debian/pool/main/o/openssl/libssl1.0.0_1.0.2l-1~bpo8+1_armhf.deb
sudo dpkg -i libssl1.0.0_1.0.2l-1~bpo8+1_armhf.deb
```

## Install Domoticz
```shell
curl -L install.domoticz.com | sudo bash
```

## Enable VNC / SSH
```shell
sudo raspi-config
```
Go to `Interfacing options`

## Configure WiFi
```shell
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=PL

network={
	ssid="SSID"
	psk="password"
}
```

## Configure static IP
```shell
sudo nano /etc/dhcpcd.conf
```
```
interface wlan0
  static ip_address=192.168.0.6/24
  static routers=192.168.0.1
  static domain_name_servers=62.179.1.61 62.179.1.63
```
