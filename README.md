# Web Radio

##### Volumio
###### SSH
ssh volumio.local -l volumio

###### Path on Volumio
/var/lib/mpd/music/WEBRADIO


# Other / raspbian
UID: pi
PW: raspberry

###### Initial Raspberry Pi configuration
sudo raspi-config

###### Upgrade Linux kernel
sudo rpi-update
sync
sudo reboot

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
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
	LANGUAGE = (unset),
	LC_ALL = "en_US.UTF-8",
	LC_CTYPE = "UTF-8",
	LANG = "en_GB.UTF-8"
    are supported and installed on your system.
perl: warning: Falling back to a fallback locale ("en_GB.UTF-8").
locale: Cannot set LC_CTYPE to default locale: No such file or directory
locale: Cannot set LC_MESSAGES to default locale: No such file or directory
locale: Cannot set LC_ALL to default locale: No such file or directory
