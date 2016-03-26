# Manual addendums Volumio
UID: volumio     PW: volumio     DNS: volumio.local

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

-----
## Errors - perl: warning: Setting locale failed.

###### Install Locales
- sudo apt-get install locales
