# nukiPyBridge

This python library let's you talk with Nuki lock (https://nuki.io/en/)

## Get started
1. install a BLE-compatible USB dongle (or use the built-in bluetooth stack if available)
2. execute the script: install-bluetooth-deps.sh, make sure it fully finishes.
3. Modify bluetooth service: nano /lib/systemd/system/bluetooth.service
4. add --experimental to ExecStart: ExecStart=/usr/local/libexec/bluetooth/bluetoothd --experimental  
5. Use supervise service to start nuki-bridge: sudo supervisorctl start nuki
6. ready to start using the library in python!

## Get started - Docker (only for ARM like raspberry pi)
1. Install docker and docker-compose
2. Use the docker-compose.yml in this repo to start the container: docker-compose up -d
3. Check the logs: docker-compose logs -f nuki 
4. Hit the url to check if its working: http://localhost:10000. You should see: {}

## Example usage
### Authenticate
Before you will be able to send commands to the Nuki lock using the library, you must first authenticate (once!) yourself with a self-generated public/private keypair (using NaCl), you can use the flask endpoint:

http://localhost:10000/connect/{MAC_ADDRESS}/{NAME}

For example: http://localhost:10000/connect/56:D2:72:54:1A:91/Reardoor

**REMARK 1** The credentials are stored in the file: nuki.cfg, created on a first connect

**REMARK 2** Authenticating is only possible if the lock is in 'pairing mode'. You can set it to this mode by pressing the button on the lock for 5 seconds until the complete LED ring starts to shine.

**REMARK 3** You can find out your Nuki's MAC address by using 'hcitool lescan' for example.

**REMARK 4** The device needs to be initialized once (i.e. using the Nuki app on your cell phone) before it can be controlled with this library.

### Commands for Nuki
Once you are authenticated (and the nuki.cfg file is created on your system), you can use the library to send command to your Nuki lock:

http://localhost:10000/Reardoor/unlock
http://localhost:10000/Reardoor/lock

For more endpoints, check server.py
