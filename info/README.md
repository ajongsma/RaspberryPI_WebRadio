# Manual install Raspbian with AirPlay
| UserID | Password | DNS |
| --- | --- |
| pi | raspberry | raspberrypi.local |

> UID: pi     PW: raspberry     DNS: raspberrypi.local

-----

## Defaults

###### Generate local SSH Key (local machine)
- ssh-keygen -t rsa -C "youremail@somewhere.com"

###### Install generated SSH key  (local machine)
- cat ~/.ssh/id_rsa.pub | ssh pi@raspberrypi.local "mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys"

###### Connect via SSH (local machine)
- ssh pi@raspberrypi.local

###### Initial Raspberry Pi configuration
- sudo raspi-config
  - expand filesystem
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

###### Install RPI Update
- sudo apt-get install -y rpi-update

###### Install ChkConfig
- sudo apt-get install -y chkconfig

###### Install Git
- sudo apt-get -y install git-core

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
    key_mgmt=WPA-PSK
}
```
- sudo ifdown wlan0
- sudo ifup wlan0

####### Set alsamixer volume to 85%
- amixer sset "PCM" 95%

```
Speaker test
- alsamixer
- speaker-test -t sine
```

## Shairport-Sync (https://github.com/mikebrady/shairport-sync)

###### Install Shairport-Sync
- cd ~
- sudo apt-get -y install autoconf automake libtool libdaemon-dev libasound2-dev libpopt-dev libconfig-dev avahi-daemon libavahi-client-dev libssl-dev libsoxr-dev libshairport2 
- git clone https://github.com/mikebrady/shairport-sync.git
- cd shairport-sync
- autoreconf -i -f
- ./configure --with-alsa --with-avahi --with-ssl=openssl --with-metadata --with-soxr --with-systemd
- make
- ./shairport-sync --version
```
2.8.1-openssl-Avahi-ALSA-soxr-metadata
```
- getent group shairport-sync || sudo groupadd -r shairport-sync
- getent passwd shairport-sync || sudo useradd -r -M -g shairport-sync -s /usr/bin/nologin -G audio shairport-sync
- sudo make install
- sudo systemctl enable shairport-sync
```
Creates: /lib/systemd/system/shairport-sync.service
```
- sudo systemctl start shairport-sync
- systemctl status shairport-sync
- sudo reboot

```
Other notes:
- shairport-sync -V
- shairport-sync -vv

- Avahi provides the Zeroconf service needed to make the AirPlay service visible on the network.
- avahi-browse -ar

- aplay -l

- File: .asoundrc
  - pcm.!default {
  -         type hw
  -         card 0
  - }
  - 
  - pcm.bluetooth {
  -         type bluetooth
  -         device 00:xx:xx:xx:xx:xx // my actual mac address is here, but you know
  -         profile "auto"
  - }
  - 
  - ctl.!default {
  -         type hw
  -         card 0
  - }
- mplayer -ao alsa:device=bluetooth audiofile.mp3

- http://komputermaschine.blogspot.nl/2015/03/raspberry-als-shairport-empfanger-2015.html
- http://qiita.com/yuyakato/items/600f499cf0610bfc1a16

- start-stop-daemon --start --quiet --pidfile $PIDFILE --exec $DAEMON -- -d -a "Pi" -- -d hw:1 -t hardware -c "PCM" || return 2

```

-----

# Experimental

-----

###### MPD (media player deamon)
- sudo apt-get -y install alsa-utils mpd mpc

###### nginx, mysql and sqlite
- sudo apt-get -y install nginx sqlite3 php5-fpm php5-curl php5-sqlite php5-cli

###### samba
- sudo apt-get -y install samba samba-common-bin

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

##### Turn off Console Blanking
- sudo nano /etc/kbd/config
```
BLANK_TIME=0
```

###### Update firmware
- sudo rpi-update
- sync
- sudo reboot

-----

###### Install rPlay
- sudo apt-get -y install libjpeg8
- sudo apt-get -y install libao-dev avahi-utils libavahi-compat-libdnssd-dev libva-dev youtube-dl
- wget -O rplay-1.0.1-armhf.deb http://www.vmlite.com/rplay/rplay-1.0.1-armhf.deb
- sudo dpkg -i rplay-1.0.1-armhf.deb
- sudo reboot

- http://raspberrypi.local:7100/admin
- uid: admin / pw: admin
- lic: S1377T8072I7798N4133R
```
/etc/rplay.conf
-----------------
airplay=1
fullscreen=0
onscreen_code=0
admin_password=admin
license_key=S1377T8072I7798N4133R
```

-----

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
