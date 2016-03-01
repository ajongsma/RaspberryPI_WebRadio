# Web Radio

##### Volumio
###### SSH
ssh volumio.local -l volumio

###### Path on Volumio
/var/lib/mpd/music/WEBRADIO


# Other
###### Upgrade Linux kernel
sudo rpi-update
sync
sudo reboot

###### Driver(s)
/etc/modules

###### Blacklisted drivers
/etc/modprobe.d/raspi-blacklist.conf

###### AddOns
sudo apt-get install mplayer
