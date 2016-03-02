# Web Radio

##### Volumio
###### SSH
ssh volumio.local -l volumio

###### Path on Volumio
/var/lib/mpd/music/WEBRADIO

-----

# Manual install Raspbian with Volumio
UID: pi
PW: raspberry

- uname -r
  - 4.1.18-v7+

###### Resize partition
- sudo raspi-config
  - expand filesystem

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

###### Upgrade Distro
- sudo apt-get -y update
- sudo apt-get -y upgrade

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

##### Install packages

###### MPD (media player deamon)
- sudo apt-get -y install alsa-utils mpd

###### MPC (command-line tool to interface MPD)
- apt-get install mpc

###### nginx, mysql and sqlite
- sudo apt-get -y install nginx sqlite3 php5-fpm php5-curl php5-sqlite php5-cli

###### samba
- sudo apt-get -y install samba samba-common-bin

###### Git
- apt-get -y install git-core

###### Install Volumio
- export GIT_SSL_NO_VERIFY=1
- sudo rm -rf /var/www
- sudo git clone https://github.com/volumio/Volumio-WebUI.git /var/www
- sudo chmod 755 /var/www/_OS_SETTINGS/etc/rc.local
- sudo chmod 755 /var/www/_OS_SETTINGS/etc/php5/mods-available/apc.ini
- sudo chmod -R 777 /var/www/command/
- sudo chmod -R 777 /var/www/db/
- sudo chmod -R 777 /var/www/inc/
- sudo chmod -R 777 /var/www/updates/
- sudo cp -arp /var/www/_OS_SETTINGS/etc/ /
- sudo cp -arp /var/www/_OS_SETTINGS/home/ /
- sudo reboot

###### Install Spotify (https://github.com/Schnouki/spop)
- wget -q -O - http://apt.mopidy.com/mopidy.gpg | sudo apt-key add -
- sudo apt-get update
- sudo apt-get install libjson-glib-dev libao-dev libdbus-glib-1-dev libnotify-dev libsoup2.4-dev libsox-dev libspotify-dev
- git clone git://github.com/Schnouki/spop.git ~/.config/spop
- cp ~/.config/spop/spopd.conf.sample ~/.config/spop/spopd.conf

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
