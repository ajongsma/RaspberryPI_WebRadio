# Manual addendums Volumio
UID: volumio     PW: volumio     DNS: volumio.local

```
Note:
2 stereo PCM channels, resulting in 2 audio channels
2 AC3 "data" channels, resulting in a 5.1 audio signal
2 DTS "data" channels, resulting in a 5.1 audio signal
```

-----

## Defaults

###### Generate local SSH Key (local machine)
- ssh-keygen -t rsa -C "youremail@somewhere.com"

###### Install generated SSH key  (local machine)
- cat ~/.ssh/id_rsa.pub | ssh volumio@volumio.local "mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys"

###### Connect via SSH (local machine)
- ssh volumio@volumio.local

-----

###### Install - perl: Locales.
? sudo dpkg-reconfigure locales

- sudo apt-get update
- sudo apt-get install locales
- sudo dpkg-reconfigure locales
  - Enable and select: en_US.UTF-8

###### Set Time Zone
- sudo dpkg-reconfigure tzdata

###### Install - Wireless-regdb
- sudo apt-get update
- sudo apt-get -y install wireless-regdb crda

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

###### Making Pi discoverable to OS X via Finder > Shared while Avahi isn't working.
- sudo apt-get install netatalk


-----

## Errors 

##### Other
- /opt/vc/bin/tvservice
  - Error: error while loading shared libraries: libvchiq_arm.so: cannot open shared object file: No such file or directory
  ```
  $ ldd /opt/vc/bin/tvservice

	linux-vdso.so.1 (0x7ec0a000)
	libvchiq_arm.so => not found
	libvcos.so => not found
	libpthread.so.0 => /lib/arm-linux-gnueabihf/libpthread.so.0 (0x76f02000)
	libdl.so.2 => /lib/arm-linux-gnueabihf/libdl.so.2 (0x76eef000)
	librt.so.1 => /lib/arm-linux-gnueabihf/librt.so.1 (0x76ed8000)
	libc.so.6 => /lib/arm-linux-gnueabihf/libc.so.6 (0x76d97000)
	/lib/ld-linux-armhf.so.3 (0x54b5d000)
  ```
  - $ export LD_LIBRARY_PATH=/opt/vc/lib/:LD_LIBRARY_PATH

## Extra

###### WiFi Scanner
- sudo apt-get install wavemon
  - sudo wavemon

###### vcgencmd - Error: VCHI initialization failed (https://www.raspberrypi.org/forums/viewtopic.php?f=63&t=34076)
- sudo usermod -G video volumio 

## Info
- https://yabb.jriver.com/interact/index.php?topic=97510.0
