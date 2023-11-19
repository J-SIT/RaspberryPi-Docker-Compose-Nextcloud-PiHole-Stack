Hello again,
instead of translating the PDF file, I have decided to simply write the summary here again.
Ok, so first of all, what did I use for this project?
<br>
<br>
<h1> 01_Materials used </h1>
- SanDisk 64GB Extreme Pro microSDXC card (note, not the microSDHC card!) (i) the Raspi 4 only supports about 30-60MBs write speed with the SD card. So you don't really need the faster one. But this way we can be sure that this is not the bottleneck, even if the card has been formatted a few times. (https://www.amazon.de/dp/B09X7BYSFG?psc=1&ref=ppx_yo2ov_dt_b_product_details) <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/cbf42f4f-1af2-40e5-aaf3-1885dc172e02)

- Samsung 870 QVO 1 TB SSD (https://www.amazon.de/dp/B089QXQ1TV?psc=1&ref=ppx_yo2ov_dt_b_product_details) <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/4c763a67-440e-4c62-874f-9926a55f5912)

- Hard disk enclosure 2.5 inch (https://www.amazon.de/dp/B0B92C5DSB?psc=1&ref=ppx_yo2ov_dt_b_product_details) <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/68fb0f88-3a01-44c8-a575-79357c74fa09)

- Raspberry Pi 4 Modell B - 4 GB Memory (https://www.amazon.de/dp/B07TC2BK1X?psc=1&ref=ppx_yo2ov_dt_b_product_details) <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/fb11ee99-7c58-42e6-9e1c-dfe8b48fedad)

- Housing with fan and heat sink (https://www.amazon.de/dp/B07YFVC51C?psc=1&ref=ppx_yo2ov_dt_b_product_details) <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/e6596839-33ea-426c-8bfb-de6dbc637929)

<br> <br>
<h1> 02_Concept description </h1>
The aim of these instructions is to provide a Nextcloud and an ad blocker using Pi-Hole. The first step is to update the Raspberry Pi and install the required software. An external 1 TB SSD is available via USB. Pi-Hole and Nginx are stored on the Raspberry Pi, while the Nextcloud database and Nextcloud data remain on the SSD. A fast microSD card is recommended to ensure that the system runs smoothly. In this guide, we use a microSDXC card with a read rate of up to 200 MBs and a write rate of up to 90 MBs. The basic system is set up accordingly and Docker is installed. Graphical remote access via VNC is also provided. The external SSD is then formatted, the access rights are set and the SSD is automatically integrated at system startup. The last step in the basic configuration is to set up automatic updating, which runs every Sunday from 2 am to 3 am. This results in several restarts, which limits Nextcloud and Pi-Hole availability during this period. <br>
In order for Nextcloud to be accessible from outside, you will need your own domain. We recommend this here in the instructions via the provider IPv64.net. Once the domain has been created, Portainer can be installed, which makes Docker easier to use graphically. An NginxReverseProxy is then installed, with which it will later be possible to reach the Nextcloud externally. Nginx also takes care of SSL encryption. Once Nginx is working, Nextcloud is also rolled out via Portainer. The DynDNS service, which is responsible for recognizing your own external IP address and mapping it to your own domain, is then set up accordingly. Pi-Hole is then installed and the configuration is restored from a backup. To ensure that all Docker containers are always up to date, Watchtower is installed, which automatically updates the containers. The next step is to set up the NginxReverseProxy. You can then start setting up Nextcloud. Finally, there are a few tips on how to optimize Nextcloud.
<br>
<br>
<h1> 03_Provide operating system </h1>

3.1.	Start the program: Raspberry Pi Imager
 <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/08657887-f963-4a51-8488-c5a01c65d5af)

3.2.	Select the Raspberry Pi 4 as the device.
 <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/fce35bce-bb79-48df-9219-f320d1353bb0)

3.3.	Select Raspberry Pi OS (64 bit) with Desktop as the operating system.
 <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/37dc8062-07fb-4bcc-80e3-34c5f8dfb099)

3.4.	Select the SD card you have connected.
3.5.	Click on NEXT.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/6fd337ff-4da1-4587-b334-70604c432a84)

3.6.	Select EDIT SETTINGS.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/76349e40-29c0-499d-8a52-92d70ef9d44b)

<br>  3.7.	Customize the user name pi and your own password on the GENERAL page.
<br>  Raspberry Pi login data:
<br>  Username:		pi
<br>  Password:		________________________
<br>  3.8.	For SERVICES, adjust the settings as follows.
<br>  3.9.	Under OPTIONS, adjust the settings as follows.
<br>  3.10.	Then click on Save
  
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/840b04cb-8151-4a6b-9d9e-3e648c453f23)
![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/54275aa6-fce4-4433-a422-ea6df583ac15)
![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/baaac3f8-b0ef-436a-be14-f8df57e3e921)

<br>  3.11.	Click on YES.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/6da92253-acae-489e-840a-46e37d5f6141)

<br>  3.12.	Confirm the message with YES.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/80f95382-1802-4f81-9f90-b85160116a89)

<br>  3.13.	Wait until you hear a sound signal and the following message appears on the screen. You can then simply pull out the microSD card.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/70fdcc0c-1d77-4bed-8f9f-11f6eeb0996f)

<br> 3.14. Insert the microSD card into the Raspberry Pi.

<br> 3.15. Connect the USB hard disk via USB to one of the blue USB ports.

<br> 3.16. Connect the Pi to your router via LAN. (WLAN sollte nur als Backup verwendet werden.)

<br> 3.17. Connect the power supply to start the Pi.

<br> 3.18. Go to the web interface of your router and search for the IP address of the Raspberry Pi. Achten Sie darauf, nicht die vom WLAN zu nehmen.

<br> 3.19. Start an SSH session on your Pi. Öffnen Sie dazu auf Ihrem Computer bei Windows CMD oder bei macOS das Terminal. Geben Sie folgenden Befehl unten ein. Ersetzen Sie IP-ADRESSE durch die vorher herausgefundene IP-Adresse.
```sh
ssh pi@IP-ADRESSE
```
<br>  3.20. Confirm the login message with yes if it is displayed. (Usually only the first time you connect to the Pi.)
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/6b05f890-c256-4a50-9ac7-4fe5d2094680)

<br>  3.21. Enter the PASSWORD that you set earlier in step 07. You cannot see the password while you are typing it in.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/d275f0b8-7528-4e26-9842-3a1db45eb65d)

<br>  IP addresses of the Pi:
<br>  WLAN:		___________________		MAC-Adress:	   ___________________
<br>  LAN:		___________________		MAC-Adress:	   ___________________
 
<br>  3.22.	Open the Raspberry Pi settings with the following command:
```sh
sudo raspi-config
```
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/2efac73f-444c-44d4-b991-03bb21d357f3)

<br>  3.23.	Use the arrow keys to navigate to Interface Options and confirm this entry with Enter.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/f51ca2af-556d-45e7-8d09-4c62d96c9855)

<br>  3.24. Navigate to the VNC item and confirm with Enter.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/2be9d117-ff58-49da-b2a0-d41f95ce5c5e)

<br>  3.25. Confirm the message with enabled with <YES>.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/9ae98af7-942f-4568-9f96-457053c639e4)

<br>  3.26. The confirmation message is enabled appears. Close this with <OK>.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/a6c80546-5fea-48df-86ec-71a57eea9a09)

<br>  3.27. Close the settings with <Finish>.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/ea04bab7-67da-44a8-937b-e75f31590fca)

<br>  3.28. Download the free VNC program.
 
<br>  3.29. Start the program and enter the IP address of the Pi in the bar at the top. Confirm with Enter.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/8ce69248-14ae-4ddd-9d6a-6d7b876dfbe2)

<br>  3.30. In the window that now opens, click on Continue.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/fd99a2bc-7cb5-443c-b3a8-ba4be1e0a4ae)

<br>  3.31. Enter the user name pi and the password assigned in step 07. You can click on Save password.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/c3961f94-367a-45d9-85c8-293da052b07c)

<br> <br>
<h1> 04_Basic configuration </h1>
<br> 4.1.	Update of the operating system and all programs.

```sh
sudo apt-get update -y && sudo apt-get upgrade -y && sudo apt-get full-upgrade -y && sudo apt-get dist-upgrade -y && sudo reboot
```
<br> 4.2.	Clean up old programs.
```sh
sudo apt autoremove -y
```
<br> 4.3.	Install the Docker repository certificate.
```sh
sudo apt-get install ca-certificates curl gnupg
```
```sh
sudo install -m 0755 -d /etc/apt/keyrings
```
```sh
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
```sh
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
```sh
>> echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```sh
sudo apt-get update -y
```
<br> 4.4.	Extending the swap memory
```sh
sudo fallocate -l 8G /swapfile_extend
```
```sh
sudo chmod 600 /swapfile_extend
```
```sh
sudo mkswap /swapfile_extend
```
```sh
sudo swapon /swapfile_extend
```
<br> 4.5.	Installing Docker.
```sh
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
```sh
sudo usermod -aG docker pi
```
```sh
sudo reboot
```
<br> 4.6.	Create the storage locations for the Docker containers Portainer, Pi-Hole and NginxProxyManager.
```sh
sudo mkdir -p /docker/02_NginxProxyManager/data /docker/02_NginxProxyManager/letsencrypt /docker/05_PiHole /docker/01_Portainer
```
<br> 4.7.	Installing the Docker container: Portainer.
```sh
docker run -d -p 8000:8000 -p 9443:9443 --name 01_Portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /docker/01_Portainer:/data portainer/portainer-ce:latest
```
<br> 4.8.	Create the storage location for the Nextcloud.
```sh
sudo apt install gparted -y
```
<br> 4.9.	Switch to the graphical user interface in VNC and start the GParted program.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/1fea48c9-a1a1-4226-9819-34e905b3e6a1)

<br> 4.10.	Enter your login data.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/d13922fc-c1ae-48a7-a8d7-79cbb322ff5d)

<br> 4.11.	Select your large hard disk.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/70590516-754b-4c36-ad74-90a7fbac7f13)

<br> 4.12.	Right-click on /dev/sda1 to select the ext4 hard disk format.
<br> You may have to click on UMount first. (If Format is grayed out).
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/2fd1f971-f6b8-4f45-a4eb-b451f5ae3558)

<br> 4.13.	Confirm with the green tick and the following warning.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/26f4bff5-4ee0-4cb7-a2ce-7cf9b43c1bcc)
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/dd35c0c9-0f6a-437a-9d8e-86dd36547083)

 
<br> 4.14.	Wait for confirmation of success.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/52de5b9f-4384-4a52-803c-5905e2636689)

<br> 4.15.	Assign a label for the partition by right-clicking on /dev/sda1 again. Enter nextcloud and confirm with OK.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/d768a631-5baa-4de0-9012-d78fd5ef1414)
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/aba28deb-4a66-478d-be40-f69973364be2)

<br> 4.16.	Repeat step 12.
<br> 4.17.	Switch back to the command line and enter the following commands.
```sh
sudo mkdir /nextcloud
```
<br> 4.18.	To automatically integrate the external hard disk into the system after each restart, you must find out the so-called UUID. To do this, enter the following command.
```sh
ls -al /dev/disk/by-uuid/
```
 <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/05316654-667c-4ecb-9be9-fffa444426e9)

<br> 4.19.	Copy the blue number in the line with /sda1
<br> 4.20.	You must now define this ID in the start-up and integrate the swap extension from step 4.4.
```sh
sudo nano /etc/fstab
```
 <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/d8610fa9-605e-4e73-a3be-2b38aac9b005)

<br> 4.21.	Use the arrow keys to go to the bottom and insert the following line (right-click to insert). First replace the UUID with the one you have found.
```sh
UUID=f43ce1a1-6a02-4b2b-b6ad-af6a5bc782c2 /nextcloud   ext4    defaults        0       0
/swapfile_extend       none       swap    sw        0       0
```
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/ba9ab4e8-ea32-4415-8224-6a753e32cc07)

<br> 4.22.	To save, press Ctrl + O and then Enter
<br> 4.23.	To exit the editor, press Ctrl + X.
<br> 4.24.	To apply the changes, restart the Pi.
```sh
sudo reboot
```
<br> 4.25.	Then create the folders for Nextcloud.
```sh
sudo mkdir /nextcloud/db /nextcloud/data
```
```sh
sudo chmod -R 0770 /nextcloud/data
```
<br> 4.26.	So that we can monitor the memory utilization of the individual containers, we must add the following commands to the very front of the following file
```sh
sudo nano /boot/cmdline.txt
``` 
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/b43551a7-b167-4b7b-80b7-84daeba7d554)

<br> 4.27.	Now insert the following commands at the very beginning: Make sure that there is a space between memory=1 and console.
```sh
cgroup_enable=memory cgroup_memory=1 
```
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/5a9e72e7-6b1b-4d33-a3f8-d09ccecad493)

<br> 4.28.	Optional (but recommended): Automatic installation of updates.
<br> Enter the following command.
```sh
sudo nano /etc/crontab
```
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/1bf96e27-1b17-4407-a0c1-e57260b260b9)

<br> 4.29.	Insert the following five lines below the existing entries by right-clicking.
```sh
0 2 * * 0 root /usr/bin/apt update -q -y >> /nextcloud/automaticupdates.log
15 2 * * 0 root /usr/bin/apt upgrade -q -y >> /nextcloud/automaticupdates.log
30 2 * * 0 root /usr/bin/apt full-upgrade -q -y >> /nextcloud/automaticupdates.log
45 2 * * 0 root /usr/bin/apt dist-upgrade -q -y >> /nextcloud/automaticupdates.log
0 3 * * 0 root /sbin/shutdown -r >> /nextcloud/automaticupdates.log
```
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/9575ea17-d2c7-4105-a47b-c0d9a4ea34e3)

<br> 4.30.	Save with the key combination Ctrl + O and exit the editor with the key combination Ctrl + X

 <br> <br>
 <h1> 05_Provide DynDNS account </h1>
 
<br> 5.1.	Open the following website and create a free account.
<br> https://ipv64.net
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/29571dcc-2910-43d3-b8c5-ae3a346f9183)

<br> 5.2.	Confirm your account with the e-mail that was sent to you. Save the password.
<br> IPv64 Login Data:
<br> E-Mail:		________________________
<br> Password:	________________________
<br> 5.3.	Log in and click on 1. create domain.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/f14897ed-155c-4df0-80d7-4f451f3f8021)

<br> 5.4.	Create a domain at the bottom right.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/5c4d8391-5b74-4bac-a734-d0bfe1099984)

<br> 5.5.	Click on your newly created domain.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/a8f8d5ea-1146-401a-9222-1c4eca25943f)

<br> 5.6.	Scroll down and click on the eye symbol next to DomainUpdate URL.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/9c497fcf-8990-49af-a975-048037eb310e)

<br> 5.7.	Copy everything after key=.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/92f048b9-1736-40e7-964b-eabe73a6bb24)

<br> DynDNS Data:
<br> Domain:		_______________________________
<br> API-Key:		_______________________________
 
 <br> <br>
 <h1> 06_Installing NginxProxyManager </h1>

<br> 6.1.	Go to the Portainer management page in your browser. Open the following website. Replace IP-ADRESS with the one from the Pi.
<br> https://IP-ADRESSE:9443

<br> 6.2.	Confirm the security warning in your browser.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/f9e3dc2e-fbe3-4df2-9ad1-efba12ae2c37)

<br> 6.3.	Assign a complicated password with your password manager and save it.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/56ac233d-a149-44f9-97a2-b1c69931d7b0)

<br> Portainer Login Data:
<br> Username:		admin
<br> Password:		_____________________________
 
<br> 6.4.	If this message appears, restart the Pi and repeat step 03.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/c2861b37-d043-4b00-8f53-b13c37e4de7f)

<br> 6.5.	Click on Get Started.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/e229f1cd-99a0-4a47-ae8d-48413d7d734d)

<br> 6.6.	Open the Environment settings of local.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/021e868e-6650-4f31-b148-6ef5a45655b0)

<br> 6.7.	Replace the name and for Public IP enter the internal IP address of your Pi.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/475287d9-5328-4c4c-8a68-389ad15ba621)

<br> 6.8.	Then click on Update environment.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/12d6d681-735c-4fef-b25a-da3d3d16c4ef)

<br> 6.9.	Go back to Home and click on the Raspberry Pi entry.
<br> 6.10.	Click on Stacks.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/00addfc0-9117-47f3-b017-6be211a42400)

<br> Click on + Add stack.
<br> 6.11. Enter the following as the name:
```
02_nginxproxymanager
```
<br> 6.12.	In the Web editor, copy the following script and then click Deploy the stack at the bottom.
<br> <br>
```yaml
version: '3'
services:
  npm:
    image: 'jc21/nginx-proxy-manager:latest'
    mem_limit: 128M
    container_name: 02_NginxProxyManager
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    environment:
      DB_SQLLITE_FILE: "/docker/02_NginxProxyManager/data/npm.sqlite"
    volumes:
      - /docker/02_NginxProxyManager/data:/data
      - /docker/02_NginxProxyManager/letsencrypt:/etc/letsencrypt

```
 <br> <br>
 <h1> 06_Installing Nextcloud </h1>
 

<br> 6.1.	Create a new stack in Portainer with the following name:
```
03_nextcloud
```
<br> 6.2.	Copy the following script into the web editor. Customize the marked with the domain from step 4.4 and the passwords and then click on Deploy the stack.
```yaml
version: '3'
services:
  nextcloud_db:
    image: yobasystems/alpine-mariadb:latest
    mem_limit: 128M
    container_name: 03_Nextcloud_DB
    restart: always
    command: --transaction-isolation=READ-COMMITTED --log-bin=binlog --binlog-format=ROW
    volumes:
      - /nextcloud/db:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=abiB4XtdLzNUCmnSnCB8K82YC6ZQ52               # Change me (for compatibility reasons, please do not use special characters.)
      - MYSQL_PASSWORD=9hr29UbBdJiee99We3YcQb3HEypXPK                    # Change me (for compatibility reasons, please do not use special characters.)
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud_user
      - MARIADB_AUTO_UPGRADE=1

  nextcloud_redis:
    image: arm32v7/redis
    mem_limit: 128M
    container_name: 03_Nextcloud_Redis
    hostname: nextcloud-redis
    environment:
        - memory=512M
    networks:
        - default
    restart: unless-stopped
    command: redis-server --requirepass a5Q6kB8Qp8uKaj7RjcTAZByn3JLA65   # Change me (for compatibility reasons, please do not use special characters.)

  nextcloud:
    image: nextcloud
    mem_limit: 2048M
    container_name: 03_Nextcloud
    restart: always
    ports:
      - 13280:80
      - 13443:443
    links:
      - nextcloud_db
      - nextcloud_redis
    volumes:
      - /nextcloud/data:/var/www/html
    environment:
      - MYSQL_PASSWORD=9hr29UbBdJiee99We3YcQb3HEypXPK                    # Change me (for compatibility reasons, please do not use special characters.)
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud_user
      - MYSQL_HOST=nextcloud_db
      - REDIS_HOST=nextcloud_redis
      - REDIS_HOST_PASSWORD=a5Q6kB8Qp8uKaj7RjcTAZByn3JLA65               # Change me (for compatibility reasons, please do not use special characters.)
      - OVERWRITEPROTOCOL=https
      - OVERWRITECLIURL=https://example.com                              # Change to your Domain
      - OVERWRITEHOST=example.com                                        # Change to your Domain
      - NEXTCLOUD_INIT_HTACCESS=true
      - PHP_MEMORY_LIMIT=2G
      - PHP_UPLOAD_LIMIT=1G
      
  cron_job:
    image: nextcloud
    mem_limit: 256M
    container_name: 03_Nextcloud_Cronjob
    restart: always
    volumes:
      - /nextcloud/data:/var/www/html
    entrypoint: /cron.sh
    environment:
      - PHP_MEMORY_LIMIT=512M
    depends_on:
      - nextcloud_db
      - nextcloud_redis
```
 <br> <br>
 <h1> 07_Installing the DDNS broker </h1>

<br> 7.1.	Create a new stack in Portainer with the following name:
```
04_ipv64dyndns
```
<br> 7.2.	Copy the following script into the web editor. Customize DOMAIN_IPV64= with the domain from step 4.4. Customize DOMAIN_KEY= with the API key from step 4.7 and then click on Deploy the stack.
```yaml
version: "3.9"
services:
  ddns-ipv64:
    image: alcapone1933/ddns-ipv64:latest
    mem_limit: 64M
    container_name: 04_DDNS-IPv64
    restart: always
    environment:
      - "TZ=Europe/Berlin"
      - "CRON_TIME=*/15 * * * *"
      - "CRON_TIME_DIG=*/30 * * * *"
      - "DOMAIN_IPV64=example.com"                                       # Change to your Domain
      - "DOMAIN_KEY=fV5L6j62cE8T4x2W5uQiRm26fjtdE8m"
```
 <br> <br>
 <h1> 08_Installing Pi-Hole </h1>
 
 <br> 8.1.	Create a new stack in Portainer with the following name:
```
05_pihole
```
 <br> 8.2.	Copy the following script into the Web editor and then click on Deploy the stack.
```yaml
version: "3"
services:
  pihole:
    container_name: 05_Pi-Hole
    mem_limit: 512M
    image: pihole/pihole:latest
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "8480:80/tcp"
    hostname: raspberrypi
    environment:
      TZ: 'Europe/Berlin'
      WEBPASSWORD: 'idf2egiB557jX5C2p63iFuiHJAF58Nk5'                      # Change me (for compatibility reasons, please do not use special characters.)
      DNSSEC: 'true'
      FTL_CMD: 'no-daemon -- --dns-forward-max 600'
    volumes:
      - '/docker/05_PiHole/etc-pihole:/etc/pihole'
      - '/docker/05_PiHole/etc-dnsmasq.d:/etc/dnsmasq.d'
    restart: unless-stopped
```
 <br> <br>
 <h1> 09_Install Watchtower </h1>
 
 <br> 9.1.	Create a new stack in Portainer with the following name:
```
06_watchtower
```
 <br> 9.2.	Copy the following script into the Web editor and then click on Deploy the stack.
```yaml
version: "3"
services:
  watchtower:
    container_name: 06_WatchTower
    mem_limit: 128M
    image: containrrr/watchtower
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    restart: always
```
 <br> <br>
 <h1> 10_Setting up NiginxProxyManager </h1>
 
<br> 10.1.	Go to the Containers page in Portainer.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/d11e193c-488e-403b-9acc-077ccfe46d0d)

<br> 10.2.	Click on the port 81:81.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/8910fc50-4512-455c-8757-13f6fbcafcc2)

<br> 10.3.	Enter ```admin@example.com``` as the login data and ```changeme``` as the password.

<br> 10.4.	You will be asked to change your e-mail address. Enter your e-mail address here.

<br> 10.5.	You must then assign a new password
<br> NginxProxyManager Login Data:
<br> E-Mail:		________________________
<br> Password:	________________________
 

<br> 10.6.	Then click on Hosts --> Proxy Hosts.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/9c766d51-396a-4311-8934-25ed3b024eb0)

<br> 10.7.	Click on Add Proxy Host.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/e6d4d56e-a4d7-4b0c-a9f4-dbee6401955c)

<br> 10.8.	Customize the domain with the one you assigned in step 4.4. For IP, enter the internal IP address from step 2.18.

<br> 10.9.	Adjust the SSL settings as follows. You only need to enter your own e-mail address.

<br> 10.10.	Then click on Save
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/e0b6c026-60b3-4a41-a9cf-ab794c30ad36)
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/43f4ff5e-6d8a-40ae-89d5-348f54f817a1)


<br> <br>
<h1> 11_Setting up Nextcloud </h1>

<br> 11.1.	Go to the Containers page in Portainer.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/2a873c04-9947-4de4-89da-4071d37fed0f)

<br> 11.2.	Click on the port 13280:80.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/df76c973-ab89-4415-83a9-0c788b689bdc)

<br> 11.3.	A new tab opens in the browser. Enter a user name and password for the Nextcloud administrator here.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/21edaa9d-e171-4c36-9feb-ca72e54d7499)

<br> Nextcloud administrator login data:
<br> 				Username: 	____________________________
<br> 				Password:	____________________________

<br> 11.4.	Now open the website of your Nextcloud by entering your domain in the web browser and log in with your Nextcloud user data from step 11.3.
<br> https://YOUR-DOMAIN
<br> Bsp.: https://scheffler.ipv64.de
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/7c0e7336-9dd6-4ccd-a061-f2f3e32229a1)

 <br> Your Nextcloud has now been successfully installed.
 <br> Note:
 <br> Some errors may be displayed in the admin overview.
 <br> -	HSTS (is taken over by NginxProxyManager, so it doesn't matter)
 <br> -	Caldav and carddav work, this is a bugg that it is displayed here.
 <br> -	You can store your e-mail account so that your Nextcloud can also send e-mails.
 <br> -	The default telephone region is generally not required.
 <br> You can check your Nextcloud for some security features under the following link. Simply enter the URL of your Nextcloud here.
 <br> https://scan.nextcloud.com/
 <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/7985ec24-106d-4710-b863-5681dc8b9ee2)

 <br> <br>
 <h1> 12_Setting up Pi-Hole </h1>

<br> 12.1.	Go to the Containers page in Portainer.
<br>![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/cfc9faa2-f910-4ea1-873b-17196b7ef0a8)

<br> 12.2.	Click on the port 8480:80.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/b7065eb7-50e4-4101-9b47-adc681f3be39)

<br> 12.3.	A new browser tab opens. Click in the address bar at the top and enter :8480/admin.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/2977dff6-6904-4d5e-8824-7422b2450c90)

<br> 12.4.	Enter the following password:
<br> ```s74F444cqjL8pK5Wypy6sh8b39QYEo```

<br> 12.5.	Click on Settings --> Teleporter.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/28ece2a6-6860-4d4c-807a-98309ff0ca9d)

<br> 12.6.	Click on Browse... at File input and upload the PiHole backup file. Then click on Restore.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/5af299cb-27d9-4884-b528-46eb07ab0253)

<br> 12.7.	Wait for the completion message: Done importing and click on Close.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/9dc2aaa6-0d41-4e6f-832f-08137ea56fe0)

<br> 12.8.	Click on Tools --> Update Gravity in the sidebar and click on Update.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/631bb718-a392-49e7-9b68-46fa50bb9c31)

 <br> <br>
 <h1> 13_Optimize nextcloud </h1>

<br> 13.1.	Log in to your Nextcloud web interface.

<br> 13.2.	Navigate to Apps.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/3e56460b-90d2-4b16-a9b5-813f4a41983e)

<br> 13.3.	Navigieren Sie zu App-Pakete. Laden Sie folgende Pakete herunter und aktivieren Sie diese.
<br> -	File access control
<br> -	Calendar
<br> -	Contacts
<br> -	Group folders
<br> -	Quota warning
  
<br> 13.4.	Navigate to Featured apps. Download and activate the following packages.
<br> -	Brute-force settings
<br> -	Suspicious Login
<br> -	Two-Factor TOTP Provider
  
<br> 13.5.	Navigate to Security. Download the following packages and activate them.
<br> -	HIBP Login check
 
<br> 13.6.	Set up an e-mail server. Navigate to Personal settings.
 	
<br> 13.7.	Enter your e-mail address.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/add65448-45e9-496f-8c3d-43c10b34504a)

<br> 13.8.	In our example, we use a Google account to send emails. As 2-factor authentication is activated in the Google account, we need a special application password for Nextcloud so that it can send emails on our behalf. Open the following website and navigate to Security.
<br> https://myaccount.google.com/

<br> 13.9.	Click on Confirmation in two steps
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/c2f4d512-1c15-4e2b-a578-967ae25fee95)

<br> 13.10.	Scroll all the way down and click on App passwords.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/95fe56cb-f2d0-4c7d-bec0-4b35eebff7fa)

<br> 13.11.	Assign a unique name such as Nextcloud and click Create.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/bcebed1c-6939-4614-8a70-6db0b37605db)

<br> 13.12.	A special application password has now been created. You can now copy this.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/50566809-7889-489c-89ef-3d278c382904)

<br> E-mail application password:	___________________________________________
<br> 13.13.	In your Nextcloud, navigate to Administration settings  Basic settings.

<br> 13.14.	Customize the e-mail server settings as follows. Replace your user name and password.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/ec08234c-2767-4e2f-8a23-408ea912b876)

<br> 13.15.	Then click on Send e-mail to test the settings. The following banner should appear:
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/9ff44e03-6789-4057-8ac5-3e746337c5bd)

<br>	Check your inbox to see if you have received a new e-mail.  
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/72a1bcae-2d66-450c-8596-4c460a5a467e)

<br> 13.16.	If you encounter the following error, log in to your Pi via SSH.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/95a69180-c429-41fa-9469-19bd1c2e219d)
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/d401c69b-f768-43ef-a8ca-d3a4b9afe1f5)

<br> 13.17.	Enter the following commands.
```sh
sudo -s
```
```sh
cd /nextcloud/data
```
```sh
rm nextcloud-init-sync.lock
```
<br> 13.18.	Then click on Rescan.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/30637add-4cfc-4c02-bdbe-dff0829a75b8)
