# Other Info

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

###### Upgrade Distro
- sudo apt-get -y update
- sudo apt-get -y upgrade

###### Update the firmware
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



##### Turn off Console Blanking
- sudo nano /etc/kbd/config
```
BLANK_TIME=0
```

-----

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

###### Shairport-Sync (https://github.com/mikebrady/shairport-sync)
- sudo apt-get -y install autoconf automake libtool libdaemon-dev libasound2-dev libpopt-dev libconfig-dev
- sudo apt-get -y install avahi-daemon libavahi-client-dev
- sudo apt-get -y install libssl-dev
- sudo apt-get -y install libpolarssl-dev
- sudo apt-get install -y libsoxr-dev
- git clone https://github.com/mikebrady/shairport-sync.git ~/shairport-sync
- cd ~/shairport-sync
- autoreconf -i -f
- ./configure --with-alsa --with-avahi --with-ssl=openssl --with-metadata --with-systemd
- make
- getent group shairport-sync &>/dev/null || sudo groupadd -r shairport-sync >/dev/null
- getent passwd shairport-sync &> /dev/null || sudo useradd -r -M -g shairport-sync -s /usr/bin/nologin -G audio shairport-sync >/dev/null
- sudo systemctl enable shairport-sync

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

###### Disable root to use SSH
- sudo nano /etc/ssh/sshd_config
```
PermitRootLogin no
```
- sudo service ssh reload

###### Driver(s)
- /etc/modules

###### Blacklisted drivers
- /etc/modprobe.d/raspi-blacklist.conf

# AddOns
###### mplayer
- sudo apt-get install mplayer

###### MPD - /etc/mpd.conf
- sudo apt-get update
- sudo apt-get install mpd
- sudo service mpd restart

###### BlueTooth
- sudo apt-get install pi-bluetooth
- bluetoothctl -a
- scan on
- pair <keyboard MAC>

###### Blank Console
- sudo nano /boot/cmdline.txt
```
consoleblank=0
```

# Errors Raspbian (clean image)
- sudo apt-get update
- sudo apt-get upgrade
- sudo apt-get install binutils
- sudo apt-get dist-upgrade
- sudo shutdown -r now
- sudo apt-get autoremove
- sudo rpi-update

- -? sudo apt-get install raspberrypi-ui-mods
- dpkg-reconfigure localepurge
