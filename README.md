# Web Radio

##### Volumio
###### SSH
ssh volumio.local -l volumio

###### Path on Volumio
/var/lib/mpd/music/WEBRADIO


# Other / raspbian
UID: pi
PW: raspberry

- uname -r
  - 4.1.18-v7+

###### Initial Raspberry Pi configuration
sudo raspi-config
- Internationalisation Options
  - Change Locale
    - Add: en_US.UTF-8 UTF-8
    - Change to: en_US.UTF-8
  - Change TimeZone
    - Europe
      - Amsterdam
  - Change WiFi Country
    - NL

###### Upgrade Linux kernel
- sudo rpi-update
- sync
- sudo reboot

###### Enable WiFi
- sudo iwlist wlan0 scan | grep ESSID
- sudo nano /etc/wpa_supplicant/wpa_supplicant.conf

```
country=NL
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="The_ESSID_from_earlier"
    psk="Your_wifi_password"
}
```
- sudo ifdown wlan0
- sudo ifup wlan0

###### Audio test
- alsamixer
- speaker-test -t sine

###### Add User
- sudo su -
- adduser volumio
- usermod -a -G pi,adm,dialout,cdrom,sudo,audio,video,plugdev,games,users,input,netdev,spi,i2c,gpio volumio
- sudo visudo
  - add at the bottom of the file
```
volumio ALL=(ALL) NOPASSWD: ALL
```


###### Driver(s)
/etc/modules

###### Blacklisted drivers
/etc/modprobe.d/raspi-blacklist.conf

# AddOns
###### mplayer
sudo apt-get install mplayer

###### MPD - /etc/mpd.conf
sudo apt-get update
sudo apt-get install mpd
sudo service mpd restart


# Errors Raspbian (clean image)
##### perl: warning: Setting locale failed.
- Run: sudo dpkg-reconfigure locales
- Enable: en_GB.UTF-8
- Enable: en_US.UTF-8 UTF-8
