---
layout: default
title: Setup Software 
nav_order: 3
description: "How to setup a ZanzoCam from scratch."
permalink: /software-setup
---

# Software Setup

To have a working ZanzoCam, you need to configure both the Raspberry and the server that will receive the pictures.

## Server

You can configure your server for FTP, HTTP or even both.

### HTTP Mode

WARNING: TO be able to use the HTTP configuration, your server must be able to run PHP5+ scripts.

1. [Get a copy of the remote control panel](https://github.com/ZanzoCam/zanzocam-control-panel/releases).

2. Copy the entire package content under the folder where you want your pictures to be uploaded.

3. Browse to the URL where you copied the package content. You should get a JSON file containing a default ZanzoCam configuration.

4. Now add `/control-panel` at the end of the URL. You shoul now see the ZanzoCam control panel.

5. Edit the parameters to your liking and then press Save.

![](/ZanzoCam/assets/images/pannello-remoto.png)

- The Server section is critical for the system to work properly. Make sure to select HTTP in the Protocol field. Then, in the URL field, copy the URL where you've seen the JSON data (step 2.).

- Remember that this interface is just a user-friendly way to modify the JSON configuration file. You can modify the file by hand too, or with any other tool, as long as the file contains only valid JSON. If you introduce syntax errors in the file, ZanzoCam will reject the new downloaded file and will keep using the old one. Such rejection should be highlighted in the logs, so make sure to check them if you see that your new settings are not applied properly, or you observe other issues.

### FTP Mode

1. [Get a copy of the remote control panel](https://github.com/ZanzoCam/zanzocam-control-panel/releases).

2. Open the file `remote-control-panel/control-panel/index.html` with your favourite browser (should work fine on major browsers, requires JavaScript).

3. The page asks your to select a configuration file. Select `remote-control-panel/configuration/configuration.json`.

![](/ZanzoCam/assets/images/pannello-locale.png)

4. Edit the parameters to your liking and then press Save.

- The Server section is critical for the system to work properly. Make sure to select FTP in the Protocol field, and don't forget to fill the fields hostname, username and password correcty. Most system will use TSL, so make sure to check that too.

- Remember that this interface is just a user-friendly way to modify the JSON configuration file. You can modify the file by hand too, or with any other tool, as long as the file contains only valid JSON. If you introduce syntax errors in the file, ZanzoCam will reject the new downloaded file and will keep using the old one. Such rejection should be highlighted in the logs, so make sure to check them if you see that your new settings are not applied properly, or you observe other issues.

5. Once you press Save, the browser will offer you to download a file called `configuration.json`: overwrite the file you opened before (`remote-control-panel/configuration/configuration.json`).

6. Copy the content of the ``remote-control-panel` folder into the root of your FTP folder.

- If you need to store the files into a subfolder, remember to add it to the Server parameters before saving, in the Subfolder field.

## Raspberry Pi

1. [Get a ZanzoCam OS image](https://github.com/ZanSara/zanzocam/releases/latest) and unzip it. It contains a file called `zanzocam.img`.

2. Flash the image onto a microSD card.

- The SD card must be larger than 4GB.

- The flashing can be done with any software, including the [Raspberry Pi Imager](https://www.raspberrypi.org/software/). From a Linux terminal, you can also do `dd if=zanzocam.img of=/dev/sdX bs=4M status=progress oflag=sync`, replacing `/dev/sdX` with the name of the device to flash (you can find the name by typing `lsblk`: choose a device name that does NOT end with a number, like sdb, sde) and using `sudo`.

- If you are working from a Linux machine or you can mount the EXT4 partition, you might want to expand the system partition `rootfs`, which by default is slightly less than 4GB and does not expand to fill up the SD card, wasting space. Feel free to expand the partition to occupy the entire card. Use any software, like [GParted](https://gparted.org/), to perform this operation. Leave the `boot` partition untouched.

3. Insert the microSD card in the Raspberry Pi and plug it in. Wait a few minutes for the system to start and check the its networking options.

- You do not need to connect a display or any other peripherial to the Raspberry Pi at this stage.

4. Use any other device (PC, smartphone) to scan for a WiFi network called `zanzocam-setup`. Once found, connect your device to this network.

- If after 10 minutes the network didn't show up, something went wrong at flashing stage. Verify your image checksum, try flashing again or [open an issue](https://github.com/ZanzoCam/zanzocam-core/issues).

5. Open a browser and go to the following address: [http://10.0.0.5](http://10.0.0.5). The browser will ask you for a username (`zanzocam-user`) and password (`lampone`).

- You will be able to [change this username and password](hardening) later.

- This will not work if you're not connected to `zanzocam-setup`.

- We had occurrencies of the ZanzoCam hotspot interfering with existing WiFi routers. If you have hard time connecting to this network, move to an area where you can't receive any other WiFi signal strongly and try again. ZanzoCam uses channel 8 for its WiFi.

6. The page offers you a few configuration steps. First of all configure the Network information, then the Server. Keep in mind that the server section is identical to the one you've seen in the remote control panel and the content of the two must match, or the ZanzoCam will not work. Ignore the Setup webcam step for the time being.

![](/ZanzoCam/assets/images/web-ui.png)

6. Once everything is configured, click on Reboot and wait for the Raspberry Pi to come alive again. The `zanzocam-setup` network must disappear: if you notice it again, repeat the procedure above or [open an issue](https://github.com/ZanzoCam/zanzocam-core/issues).

7. In the meantime, connect your device back to the WiFi network you just configured on the ZanzoCam and scan the WiFi network for its IP address. To perform this operation there are several options:
   
- From a smartphone, use an app. There are several that perform these types of scans, for example Fing, OTHER APPS.
- From a Windows/Linux/Mac machine, you can download [Angry IP Scanner](https://angryip.org/download/) or OTHER APP and press the Scan button. 
- From the Linux terminal, use `nmap -p 22 192.168.x.0/24`, where `x` is the third value of the IP address of the ruouter of your WiFi network.

8. After about 10 minutes, the scan should display a Raspberry Pi device, or at least a new one that wasn't there before. Let's assume it's IP is `192.168.X.Y`, where `X` and `Y` are two numbers between 0 and 255.

- If you don't, then it's likely that the network `zanzocam-setup` exists again (in which case, repeat from point 5.) or that you're connected to the wrong WiFi network. If neither of these is true, [open an issue](https://github.com/ZanzoCam/zanzocam-core/issues).

9. Open a browser on any device connected to this WiFi network and go to the address http://192.168.X.Y (replace X and Y with the correct numbers)

10. At this point click on Setup Webcam. You will see a preview of the image that the ZanzoCam is going to take (without any text, logo or post-processing applied). You can refresh the picture as many times as you want and, when ready, click Take Picture.

11. You will see some text appearing on a black background. Once the last line says that the process was completed successfully, ZanzoCam is fully operational.

- At this point, if you visit your server, you will find the first picture under the `pictures` folder and the first log under `logs`. Setup log rotation on your server to prevent the logs from filling up your disk, or regularly empty the folder with a cronjob.

- If the server received nothing, or the text in the black box says that the procedure completed with errors, or something else went wrong, double-check the configuration from the server, the server data you configured on the local, green panel, and if everything is correct, [open an issue](https://github.com/ZanzoCam/zanzocam-core/issues) describing your problem in all details, including a copy of all the text in the black area.









