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

###### Enable WiFi
- http://volumio.local/plugin/system_controller-network

###### Making Pi discoverable to OS X via Finder > Shared while Avahi isn't working.
- sudo apt-get install netatalk

-----

## Errors 

###### perl: warning: Setting locale failed.
- sudo apt-get install locales
- sudo dpkg-reconfigure locales
  - Enable and select: en_US.UTF-8

##### Other
- /opt/vc/bin/tvservice
  - Error: error while loading shared libraries: libvchiq_arm.so: cannot open shared object file: No such file or directory
  - $ LD_LIBRARY_PATH=/opt/vc/lib /opt/vc/bin/tvservice
