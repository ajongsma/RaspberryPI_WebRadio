# Node - Still Errors

- wget http://node-arm.herokuapp.com/node_latest_armhf.deb 
- sudo dpkg -i node_latest_armhf.deb
- node -v

- sudo git clone --recursive https://github.com/volumio/Volumio2.git /volumio2
- sudo chown -R pi:pi /volumio2
- sudo mkdir /data
- sudo chown -R pi:pi /data
- cd /volumio2
- npm install
- node index.js
