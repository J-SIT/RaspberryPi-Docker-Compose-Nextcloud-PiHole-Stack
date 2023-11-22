Hallo nochmal,
ich habe mich dazu entschieden, statt die PDF-Datei zu übersetzen, einfach direkt die Zusammenfassung hier noch mal reinzuschreiben.
<br>
Ok, also als Erstes einmal, was habe ich für dieses Projekt alles verwendet:
<br>
<br>
<h1> 01_Verwendete Materialien </h1>
- SanDisk 64 GB Extreme Pro microSDXC-Karte (Achtung, nicht die microSDHC-Karte!) (i) der Raspi 4 unterstützt bei der SD-Karte nur ca. 30-60MBs Schreibgeschwindigkeit. Also bräuchte man eigentlich nicht die schnellere. Aber so können wir sicher sein, dass dies nicht der Flaschenhals ist, auch wenn die Karte ein paarmal formatiert wurde. 
<br> https://www.amazon.de/dp/B09X7BYSFG?psc=1&ref=ppx_yo2ov_dt_b_product_details
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/f5570e03-a709-4c07-b41f-b75a1fecec7e)

- Samsung 870 QVO 1 TB SSD (https://www.amazon.de/dp/B089QXQ1TV?psc=1&ref=ppx_yo2ov_dt_b_product_details) <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/aff418e5-2b6c-4363-ba9a-1dea85c240be)

- Festplattengehäuse 2,5 Zoll (https://www.amazon.de/dp/B0B92C5DSB?psc=1&ref=ppx_yo2ov_dt_b_product_details) <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/f27a6a83-f538-4eeb-b707-e2a9acb4bc6a)

- Raspberry Pi 4 Modell B - 4 GB Ram (https://www.amazon.de/dp/B07TC2BK1X?psc=1&ref=ppx_yo2ov_dt_b_product_details) <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/32d6782f-daa8-49d9-98f4-9d8134b392e2)

- Gehäuse mit Lüfter und Kühlkörper (https://www.amazon.de/dp/B07YFVC51C?psc=1&ref=ppx_yo2ov_dt_b_product_details) <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/853be2f6-6fa8-432b-afe8-95dc92e81b85)

<br> <br>
<h1> 02_Konzeptbeschreibung </h1>
Zielsetzung dieser Anleitung ist das Bereitstellen einer Nextcloud und eines Werbeblockers mittels Pi-Hole. Als Erstes wird der Raspberry Pi upgedatet und die benötigte Software installiert. Es steht dabei eine externe 1 TB SSD per USB zur Verfügung. Pi-Hole und Nginx werden auf dem Raspberry Pi gespeichert, während die Nextcloud Datenbank und die Nextcloud-Daten auf der SSD verbleiben. Damit das System angenehm flüssig läuft, wird eine schnelle microSD-Karte empfohlen. In dieser Anleitung verwenden wir eine microSDXC-Karte mit einer Lese rate von bis zu 200 MBs und einer Schreibrate von bis zu 90 MBs. Das Grundsystem wird entsprechend eingerichtet und Docker installiert. Es wird noch der grafische Remotezugriff via VNC bereitgestellt. Anschließend wird die externe SSD formatiert, die Zugriffsrechte gesetzt und die SSD automatisch beim Systemstart eingebunden. Als Letztes in der Grundkonfiguration wird das automatische Updaten eingestellt, was jeden Sonntag von 2 Uhr bis 3 Uhr läuft. Hier kommt es zu mehreren Neustarts, was die Nextcloud und Pi-Hole Verfügbarkeit in diesem Zeitraum einschränkt. <br>
Damit die Nextcloud auch von außerhalb erreichbar sein wird, wird noch eine eigene Domain benötigt. Dies empfehlen wir hier in der Anleitung über den Anbieter IPv64.net. Wenn die Domain erstellt wurde, kann Portainer installiert werden, mit wessen sich Docker grafisch einfacher bedienen lässt. Nachfolgend wird ein NginxReverseProxy installiert, mit welchem es später möglich sein, wird, die Nextcloud von extern zu erreichen. Um die SSL-Verschlüsselung kümmert sich ebenfalls Nginx. Nachdem Nginx funktioniert, wird auch Nextcloud über Portainer ausgerollt. Dann wird entsprechend der DynDNS Dienst, welcher für die Erkennung der eigenen externen IP-Adresse und dem Zuordnen zu der eigenen Domain, verantwortlich ist, eingerichtet. Danach wird Pi-Hole installiert und aus einem Backup die Konfiguration wiederhergestellt. Damit alle Docker-Container immer up to date sind, wird Watchtower installiert, welches die Container automatisch aktualisiert. Jetzt muss als Nächstes der NginxReverseProxy eingerichtet werden. Danach kann mit dem Einrichten von Nextcloud begonnen werden. Abschließend gibt es noch ein paar Hinweise, wie man die Nextcloud optimieren kann.
<br>
<br>
<h1> 03_Betriebssystem bereitstellen </h1>

3.1.	Starten Sie das Programm: Raspberry Pi Imager
 <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/08657887-f963-4a51-8488-c5a01c65d5af)

3.2.	Wählen Sie als Gerät den Raspberry Pi 4 aus.
 <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/fce35bce-bb79-48df-9219-f320d1353bb0)

3.3.	Wählen Sie als Betriebssystem Raspberry Pi OS (64 bit) with Desktop, aus.
 <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/37dc8062-07fb-4bcc-80e3-34c5f8dfb099)

3.4.	Wählen Sie bei der SD-Karte Ihre angeschlossene aus.
3.5.	Klicken Sie auf NEXT.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/6fd337ff-4da1-4587-b334-70604c432a84)

3.6.	Wählen Sie EINSTELLUNGEN BEARBEITEN aus.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/76349e40-29c0-499d-8a52-92d70ef9d44b)

<br>  3.7.	Passen Sie auf der Seite ALLGEMEIN den Benutzernamen pi und Ihr eigenes Passwort an.
<br>  Raspberry Pi Login Daten:
<br>  Benutzername:		pi
<br>  Passwort:		________________________
<br>  3.8.	Bei DIENSTE passen Sie die Einstellungen wie folgt an.
<br>  3.9.	Bei OPTIONEN passen Sie die Einstellungen wie folgt an.
<br>  3.10.	Klicken Sie anschließend auf Speichern
  
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/840b04cb-8151-4a6b-9d9e-3e648c453f23)
![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/54275aa6-fce4-4433-a422-ea6df583ac15)
![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/baaac3f8-b0ef-436a-be14-f8df57e3e921)

<br>  3.11.	Klicken Sie auf JA.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/6da92253-acae-489e-840a-46e37d5f6141)

<br>  3.12.	Bestätigen Sie die Meldung mit JA.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/80f95382-1802-4f81-9f90-b85160116a89)

<br>  3.13.	Warten Sie, bis ein Ton-Signal ertönt und auf dem Bildschirm folgende Meldung erscheint. Dann können Sie die microSD-Karte einfach herausziehen.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/70fdcc0c-1d77-4bed-8f9f-11f6eeb0996f)

<br>  3.14.	Stecken Sie die microSD-Karte in den Raspberry Pi.
<br>  3.15.	Schließen Sie die USB-Festplatte per USB an einen der blauen USB-Ports an.
<br>  3.16.	Schließen Sie den Pi per LAN an Ihren Router an. (WLAN sollte nur als Backup verwendet werden.)
<br>  3.17.	Schließen Sie das Netzteil an, um den Pi zu starten.
<br>  3.18.	Gehen Sie auf die Weboberfläche Ihres Routers und suchen Sie die IP-Adresse vom Raspberry Pi raus. Achten Sie darauf, nicht die vom WLAN zu nehmen.
<br>  3.19.	Starten Sie eine SSH-Sitzung auf Ihrem Pi. Öffnen Sie dazu auf Ihrem Computer bei Windows CMD oder bei macOS das Terminal. Geben Sie folgenden Befehl unten ein. Ersetzen Sie IP-ADRESSE durch die vorher herausgefundene IP-Adresse.
```sh
ssh pi@IP-ADRESSE
```
<br>  3.20.	Bestätigen Sie die Login-Message mit yes falls diese Angezeigt wird. (In der Regel nur beim ersten Mal Verbinden mit dem Pi.)
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/6b05f890-c256-4a50-9ac7-4fe5d2094680)

<br>  3.21.	Geben Sie das PASSWORT ein, welches Sie vorhin in Punkt 07 gesetzt haben. Sie können das Passwort nicht sehen, während Sie dieses eintippen.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/d275f0b8-7528-4e26-9842-3a1db45eb65d)

<br>  IP-Adressen des Pi:
<br>  WLAN:		___________________		MAC-Adresse:	   ___________________
<br>  LAN:		___________________		MAC-Adresse:	   ___________________
 
<br>  3.22.	Öffnen Sie die Raspberry Pi Einstellungen mit folgendem Befehl:
```sh
sudo raspi-config
```
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/2efac73f-444c-44d4-b991-03bb21d357f3)

<br>  3.23.	Navigieren Sie mit den Pfeiltasten zu Interface Options und bestätigen Sie diesen Eintrag mit Enter.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/f51ca2af-556d-45e7-8d09-4c62d96c9855)

<br>  3.24.	Navigieren Sie zu dem Punkt VNC und bestätigen Sie mit Enter.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/2be9d117-ff58-49da-b2a0-d41f95ce5c5e)

<br>  3.25.	Bestätigen Sie die Meldung mit enabled  mit <YES>.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/9ae98af7-942f-4568-9f96-457053c639e4)

<br>  3.26.	Es kommt die Bestätigungsmeldung is enabled. Schließen Sie diese mit <OK>.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/a6c80546-5fea-48df-86ec-71a57eea9a09)

<br>  3.27.	Schließen Sie die Einstellungen mit <Finish>.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/ea04bab7-67da-44a8-937b-e75f31590fca)

<br>  3.28.	Laden Sie sich das kostenlose Programm VNC herunter.
 
<br>  3.29.	Starten Sie das Programm und geben Sie oben in der Leiste die IP-Adresse des Pi ein. Bestätigen Sie mit Enter.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/8ce69248-14ae-4ddd-9d6a-6d7b876dfbe2)

<br>  3.30.	In dem nun öffnenden Fenster klicken Sie auf Fortsetzen.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/fd99a2bc-7cb5-443c-b3a8-ba4be1e0a4ae)

<br>  3.31.	Geben Sie den Benutzernamen pi und das aus Schritt 07 vergebene Passwort ein. Sie können auf Kennwort speichern, klicken.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/c3961f94-367a-45d9-85c8-293da052b07c)

<br> <br>
<h1> 04_Grundkonfiguration </h1>
<br> 4.1.	Update des Betriebssystems und aller Programme.

```sh
sudo apt-get update -y && sudo apt-get upgrade -y && sudo apt-get full-upgrade -y && sudo apt-get dist-upgrade -y && sudo reboot
```
<br> 4.2.	Aufräumen alter Programme.
```sh
sudo apt autoremove -y
```
<br> 4.3.	Installieren des Docker-Repository-Zertifikates.
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
<br> 4.4.	Erweitern des Swap(Auslagerungs)-Speichers
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
<br> 4.5.	Installieren von Docker.
```sh
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
```sh
sudo usermod -aG docker pi
```
```sh
sudo reboot
```
<br> 4.6.	Erstellen der Speicherorte für die Docker-Container Portainer, Pi-Hole und NginxProxyManager.
```sh
sudo mkdir -p /docker/02_NginxProxyManager/data /docker/02_NginxProxyManager/letsencrypt /docker/05_PiHole /docker/01_Portainer
```
<br> 4.7.	Installieren des Docker-Containers: Portainer.
```sh
docker run -d -p 8000:8000 -p 9443:9443 --name 01_Portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /docker/01_Portainer:/data portainer/portainer-ce:latest
```
<br> 4.8.	Erstellen des Speicherortes für die Nextcloud.
```sh
sudo apt install gparted -y
```
<br> 4.9.	Wechseln Sie zur grafischen Oberfläche in VNC und starten Sie das Programm GParted.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/1fea48c9-a1a1-4226-9819-34e905b3e6a1)

<br> 4.10.	Geben Sie Ihre Login-Daten ein.
<br>  ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/d13922fc-c1ae-48a7-a8d7-79cbb322ff5d)

<br> 4.11.	Wählen Sie Ihre große Festplatte aus.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/70590516-754b-4c36-ad74-90a7fbac7f13)

<br> 4.12.	Mit einem Rechtsklick auf /dev/sda1 wählen Sie das Festplatten-Format ext4 aus.
<br> Eventuell müssen Sie vorher UMount anklicken. (Wenn Format ausgegraut ist).
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/2fd1f971-f6b8-4f45-a4eb-b451f5ae3558)

<br> 4.13.	Bestätigen Sie mit dem grünen Haken und die folgende Warnung.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/26f4bff5-4ee0-4cb7-a2ce-7cf9b43c1bcc)
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/dd35c0c9-0f6a-437a-9d8e-86dd36547083)

 
<br> 4.14.	Warten Sie auf die Erfolgs-Bestätigung.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/52de5b9f-4384-4a52-803c-5905e2636689)

<br> 4.15.	Vergeben Sie ein Label für die Partition, in dem Sie wieder einen Rechtsklick auf /dev/sda1 machen. Geben Sie nextcloud ein und bestätigen Sie mit OK.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/d768a631-5baa-4de0-9012-d78fd5ef1414)
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/aba28deb-4a66-478d-be40-f69973364be2)

<br> 4.16.	Wiederholen Sie Schritt 12.
<br> 4.17.	Wechseln Sie wieder auf die Kommandozeile und geben Sie folgende Befehle ein.
```sh
sudo mkdir /nextcloud
```
<br> 4.18.	Um die externe Festplatte automatisch ins System nach jedem Neustart einzubinden, müssen Sie die sog. UUID herausfinden. Geben Sie dazu folgenden Befehl ein.
```sh
ls -al /dev/disk/by-uuid/
```
 <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/05316654-667c-4ecb-9be9-fffa444426e9)

<br> 4.19.	Kopieren Sie die blaue Nummer in der Zeile mit /sda1
<br> 4.20.	Nun müssen Sie diese ID im Start-up festlegen sowie die Swap-Erweiterung einbinden aus Schritt 4.4.
```sh
sudo nano /etc/fstab
```
 <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/d8610fa9-605e-4e73-a3be-2b38aac9b005)

<br> 4.21.	Gehen Sie mit den Pfeiltasten nach ganz unten und fügen Sie folgende Zeile ein (Rechtsklick zum Einfügen). Wechseln Sie vorher die UUID durch Ihre herausgefundene aus.
```sh
UUID=f43ce1a1-6a02-4b2b-b6ad-af6a5bc782c2 /nextcloud   ext4    defaults        0       0
/swapfile_extend       none       swap    sw        0       0
```
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/ba9ab4e8-ea32-4415-8224-6a753e32cc07)

<br> 4.22.	Zum Speichern drücken Sie Strg + O und dann Enter
<br> 4.23.	Zum Verlassen des Editors drücken Sie Strg + X.
<br> 4.24.	Zum Anwenden der Änderungen starten Sie den Pi neu.
```sh
sudo reboot
```
<br> 4.25.	Erstellen Sie Anschließend noch die Ordner für Nextcloud.
```sh
sudo mkdir /nextcloud/db /nextcloud/data
```
```sh
sudo chmod -R 0770 /nextcloud/data
```
<br> 4.26.	Damit wir die Arbeitsspeicher-Auslastung der einzelnen Container überwachen können, müssen wir in der folgenden Datei die folgenden Befehle ganz vorne anfügen
```sh
sudo nano /boot/cmdline.txt
``` 
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/b43551a7-b167-4b7b-80b7-84daeba7d554)

<br> 4.27.	Fügen Sie nun ganz vorne folgende Befehle ein: Achten Sie darauf das zwischen memory=1 und concole ein Leerzeichen ist.
```sh
cgroup_enable=memory cgroup_memory=1 
```
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/5a9e72e7-6b1b-4d33-a3f8-d09ccecad493)

<br> 4.28.	Optional (aber Empfohlen): Automatische Installation von Updates.
<br> Geben Sie folgenden Befehl ein.
```sh
sudo nano /etc/crontab
```
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/1bf96e27-1b17-4407-a0c1-e57260b260b9)

<br> 4.29.	Fügen Sie die folgenden fünf Zeilen unterhalb der bestehenden Einträge mit einem Rechtsklick ein.
```sh
0 2 * * 0 root /usr/bin/apt update -q -y >> /nextcloud/automaticupdates.log
15 2 * * 0 root /usr/bin/apt upgrade -q -y >> /nextcloud/automaticupdates.log
30 2 * * 0 root /usr/bin/apt full-upgrade -q -y >> /nextcloud/automaticupdates.log
45 2 * * 0 root /usr/bin/apt dist-upgrade -q -y >> /nextcloud/automaticupdates.log
0 3 * * 0 root /sbin/shutdown -r >> /nextcloud/automaticupdates.log
```
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/9575ea17-d2c7-4105-a47b-c0d9a4ea34e3)

<br> 4.30.	Speichern Sie mit der Tastenkombination Strg + O und verlassen Sie den Editor mit der Tastenkombination Strg + X

 <br> <br>
 <h1> 05_DynDNS Account bereitstellen </h1>
 
<br> 5.1.	Öffnen Sie die folgende Webseite und erstellen Sie sich einen kostenlosen Account.
<br> https://ipv64.net
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/29571dcc-2910-43d3-b8c5-ae3a346f9183)

<br> 5.2.	Bestätigen Sie Ihren Account mit der E-Mail, welche Ihnen zugeschickt wurde. Speichern Sie sich das Passwort ab.
<br> IPv64 Login Daten:
<br> E-Mail:		________________________
<br> Passwort:	________________________
<br> 5.3.	Melden Sie sich an und klicken Sie auf 1. Domain anlegen.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/f14897ed-155c-4df0-80d7-4f451f3f8021)

<br> 5.4.	Erstellen Sie sich unten Rechts eine Domain.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/5c4d8391-5b74-4bac-a734-d0bfe1099984)

<br> 5.5.	Klicken Sie auf Ihre neu erstelle Domain.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/a8f8d5ea-1146-401a-9222-1c4eca25943f)

<br> 5.6.	Scrollen Sie runter und klicken Sie auf das Augensymbol bei DomainUpdate URL.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/9c497fcf-8990-49af-a975-048037eb310e)

<br> 5.7.	Kopieren Sie alles, was hinter key= steht.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/92f048b9-1736-40e7-964b-eabe73a6bb24)

<br> DynDNS Daten:
<br> Domain:		_______________________________
<br> API-Schlüssel:		_______________________________
 
 <br> <br>
 <h1> 06_Installieren von NginxProxyManager </h1>

<br> 6.1.	Gehen Sie im Browser auf die Management-Seite von Portainer. Öffnen Sie dazu folgende Webseite. Ersetzen Sie IP-ADRESSE mit der vom Pi.
<br> https://IP-ADRESSE:9443

<br> 6.2.	Bestätigen Sie die Sicherheitswarnung in Ihrem Browser.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/f9e3dc2e-fbe3-4df2-9ad1-efba12ae2c37)

<br> 6.3.	Vergeben Sie mit Ihrem Passwortmanager ein kompliziertes Passwort und speichern Sie dieses ab.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/56ac233d-a149-44f9-97a2-b1c69931d7b0)

<br> Portainer Login Daten:
<br> Benutzername:		admin
<br> Passwort:		_____________________________
 
<br> 6.4.	Sollte diese Meldung kommen, starten Sie den Pi neu und wiederholen Sie Schritt 03.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/c2861b37-d043-4b00-8f53-b13c37e4de7f)

<br> 6.5.	Klicken Sie auf Get Started.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/e229f1cd-99a0-4a47-ae8d-48413d7d734d)

<br> 6.6.	Öffnen Sie die Environment Einstellungen von local.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/021e868e-6650-4f31-b148-6ef5a45655b0)

<br> 6.7.	Ersetzen Sie den Namen und bei Public IP tragen Sie die interne IP-Adresse von Ihrem Pi ein.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/475287d9-5328-4c4c-8a68-389ad15ba621)

<br> 6.8.	Klicken Sie anschließend auf Update environment.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/12d6d681-735c-4fef-b25a-da3d3d16c4ef)

<br> 6.9.	Gehen Sie zurück auf Home und klicken Sie auf den Eintrag Raspberry Pi.
<br> 6.10.	Klicken Sie auf Stacks.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/00addfc0-9117-47f3-b017-6be211a42400)

<br> Klicken Sie auf + Add stack.
<br> 6.11.	Geben Sie Folgendes als Name ein:
```
02_nginxproxymanager
```
<br> 6.12.	Im Web editor kopieren Sie folgendes Script und klicken unten Anschließend auf Deploy the stack.
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
 <h1> 06_Installieren von Nextcloud </h1>
 

<br> 6.1.	Erstellen Sie in Portainer einen neuen Stack mit dem folgenden Namen:
```
03_nextcloud
```
<br> 6.2.	Kopieren Sie folgendes Script in den Web editor. Passen Sie das Markierte mit der Domain aus Schritt 4.4 und die Passwörter an und klicken Sie anschließend auf Deploy the stack.

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
      - MYSQL_ROOT_PASSWORD=abiB4XtdLzNUCmnSnCB8K82YC6ZQ52      #   Please Change the PW. If you change the password, make sure that you do not use any special characters. Depending on the region, this may cause problems.
      - MYSQL_PASSWORD=9hr29UbBdJiee99We3YcQb3HEypXPK           #   Please Change the PW.
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud_user
      - MARIADB_AUTO_UPGRADE=1

  nextcloud_redis:
    image: arm32v7/redis
    mem_limit: 128M
    container_name: 03_Nextcloud_Redis
    hostname: nextcloud-redis
    networks:
        - default
    restart: unless-stopped
    command: redis-server --requirepass a5Q6kB8Qp8uKaj7RjcTAZByn3JLA65  #   Please Change the PW.

  nextcloud:
    image: nextcloud
    mem_limit: 2048M
    container_name: 03_Nextcloud
    restart: always
    ports:
      - 13280:80
      - 13443:443
      - 9980:9980
    links:
      - nextcloud_db
      - nextcloud_redis
    volumes:
      - /nextcloud/data:/var/www/html
    environment:
      - MYSQL_PASSWORD=9hr29UbBdJiee99We3YcQb3HEypXPK           #   Please Change the PW.
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud_user
      - MYSQL_HOST=nextcloud_db
      - REDIS_HOST=nextcloud_redis
      - REDIS_HOST_PASSWORD=a5Q6kB8Qp8uKaj7RjcTAZByn3JLA65      #   Please Change the PW.
      - OVERWRITEPROTOCOL=https
      - OVERWRITECLIURL=https://YOUR-DOMAIN.com                 #   Please Change to your Domain.
      - OVERWRITEHOST=YOUR-DOMAIN.com                           #   Please Change to your Domain.
      - NEXTCLOUD_INIT_HTACCESS=true
      - PHP_MEMORY_LIMIT=2G
      - PHP_UPLOAD_LIMIT=1G
    healthcheck:
      test: curl --fail http://10.1.0.57:13280 || exit 1        #   Please Enter your local IP.
      interval: 30s
      retries: 5
      start_period: 20s
      timeout: 10s
      
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
 <h1> 07_Installieren vom DDNS-Broker </h1>

<br> 7.1.	Erstellen Sie in Portainer einen neuen Stack mit dem folgenden Namen:
```
04_ipv64dyndns
```
<br> 7.2.	Kopieren Sie folgendes Script in den Web editor. Passen Sie DOMAIN_IPV64= mit der Domain aus Schritt 4.4 an. Passen Sie DOMAIN_KEY= mit dem API-Schlüssel aus Schritt 4.7 an und klicken Sie anschließend auf Deploy the stack.
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
 <h1> 08_Installieren von Pi-Hole </h1>
 
 <br> 8.1.	Erstellen Sie in Portainer einen neuen Stack mit dem folgenden Namen:
```
05_pihole
```
 <br> 8.2.	Kopieren Sie folgendes Script in den Web editor und klicken Sie anschließend auf Deploy the stack.
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
 <h1> 09_Installieren von Watchtower </h1>
 
 <br> 9.1.	Erstellen Sie in Portainer einen neuen Stack mit dem folgenden Namen:
```
06_watchtower
```
 <br> 9.2.	Kopieren Sie folgendes Script in den Web editor und klicken Sie anschließend auf Deploy the stack.
```yaml
version: "3"
services:
  watchtower:
    container_name: 06_WatchTower
    mem_limit: 128M
    image: containrrr/watchtower
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /etc/localtime:/etc/localtime:ro
    environment:
      WATCHTOWER_CLEANUP: "true"
      WATCHTOWER_SCHEDULE: 0 0 1 * *
      TZ: Europe/Berlin
    restart: always
```
 <br> <br>
 <h1> 10_Einrichten von NiginxProxyManager </h1>
 
<br> 10.1.	Gehen Sie auf die Containers Seite in Portainer.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/d11e193c-488e-403b-9acc-077ccfe46d0d)

<br> 10.2.	Klicken Sie auf den Port 81:81.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/8910fc50-4512-455c-8757-13f6fbcafcc2)

<br> 10.3.	Geben Sie als Login-Daten ```admin@example.com``` und als Passwort ```changeme``` ein.

<br> 10.4.	Sie werden aufgefordert, die E-Mail-Adresse zu ändern. Geben Sie hier Ihre Mailadresse an.

<br> 10.5.	Anschließend müssen Sie ein neues Passwort vergeben
<br> NginxProxyManager Login Daten:
<br> E-Mail:		________________________
<br> Passwort:	________________________
 

<br> 10.6.	Klicken Sie anschließend auf Hosts --> Proxy Hosts.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/9c766d51-396a-4311-8934-25ed3b024eb0)

<br> 10.7.	Klicken Sie auf Add Proxy Host.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/e6d4d56e-a4d7-4b0c-a9f4-dbee6401955c)

<br> 10.8.	Passen Sie die Domain mit Ihrer aus Schritt 4.4 vergebenen an. Geben Sie bei IP die interne IP-Adresse von Schritt 2.18 an.

<br> 10.9.	Passen Sie bei SSL die Einstellungen wie folgt an. Sie müssen nur Ihre eigene E-Mailadresse eingeben.

<br> 10.10.	Klicken Sie anschließend auf Save
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/e0b6c026-60b3-4a41-a9cf-ab794c30ad36)
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/43f4ff5e-6d8a-40ae-89d5-348f54f817a1)


 <br> <br>
 <h1> 11_Einrichten von Nextcloud </h1>

 <br> 11.1.	Gehen Sie auf die Containers Seite in Portainer.
 <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/2a873c04-9947-4de4-89da-4071d37fed0f)

 <br> 11.2.	Klicken Sie auf den Port 13280:80.
 <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/df76c973-ab89-4415-83a9-0c788b689bdc)

 <br> 11.3.	Es öffnet sich ein neuer Tab im Browser. Vergeben Sie hier einen Benutzernamen und ein Passwort für den Administrator der Nextcloud.
 <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/21edaa9d-e171-4c36-9feb-ca72e54d7499)

 <br> Nextcloud Administrator Login Daten:
	 <br> 				Benutzername: 	____________________________
	 <br> 				Passwort:		____________________________

 <br> 11.4.	Öffnen Sie nun die Webseite Ihrer Nextcloud, in dem Sie ihre Domain im Webbrowser eingeben und Melden Sie sich mit Ihren Nextcloud Benutzerdaten aus Schritt 11.3 an.
 <br> https://IHRE-DOMAIN
 <br> Bsp.: https://scheffler.ipv64.de
 <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/7c0e7336-9dd6-4ccd-a061-f2f3e32229a1)

 <br> Ihre Nextcloud ist jetzt erfolgreich installiert worden.
 <br> Hinweis:
 <br> Bei der Admin-Übersicht werden womöglich einige Fehler angezeigt.
 <br> -	HSTS (wird von NginxProxyManager übernommen, also egal)
 <br> -	Caldav und carddav funktionieren, dies ist ein Bugg das es hier angezeigt wird.
 <br> -	Sie können Ihren E-Mail-Account hinterlegen, damit Ihre Nextcloud auch E-Mails versenden kann.
 <br> -	Die Standard-Telefonregion wird i.d.R. nicht benötigt.
 <br> Sie können Ihre Nextcloud auf einige Sicherheitsfeatures untersuchen unter folgendem Link. Tragen Sie hier einfach die URL Ihrer Nextcloud ein.
 <br> https://scan.nextcloud.com/
 <br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/7985ec24-106d-4710-b863-5681dc8b9ee2)

 <br> <br>
 <h1> 12_Einrichten von Pi-Hole </h1>

<br> 12.1.	Gehen Sie auf die Containers Seite in Portainer.
<br>![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/cfc9faa2-f910-4ea1-873b-17196b7ef0a8)

<br> 12.2.	Klicken Sie auf den Port 8480:80.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/b7065eb7-50e4-4101-9b47-adc681f3be39)

<br> 12.3.	Es öffnet sich ein neuer Browsertab. Klicken Sie oben in die Adressleiste und geben Sie hinter :8480/admin ein.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/2977dff6-6904-4d5e-8824-7422b2450c90)

<br> 12.4.	Geben Sie als Passwort Folgendes ein:
<br> ```s74F444cqjL8pK5Wypy6sh8b39QYEo```

<br> 12.5.	Klicken Sie auf Settings --> Teleporter.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/28ece2a6-6860-4d4c-807a-98309ff0ca9d)

<br> 12.6.	Klicken Sie auf Browse… bei File input und laden Sie die Datei PiHole-Backup hoch. Klicken Sie anschließend auf Restore.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/5af299cb-27d9-4884-b528-46eb07ab0253)

<br> 12.7.	Warten Sie auf die Abschlussmeldung: Done importing und klicken Sie auf Close.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/9dc2aaa6-0d41-4e6f-832f-08137ea56fe0)

<br> 12.8.	Klicken Sie in der Seitenleiste auf Tools --> Update Gravity und klicken Sie auf Update.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/631bb718-a392-49e7-9b68-46fa50bb9c31)

 <br> <br>
 <h1> 13_Nextcloud optimieren </h1>

<br> 13.1.	Melden Sie sich auf Ihrer Nextcloud-Weboberfläche an.

<br> 13.2.	Navigieren Sie zu Apps.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/3e56460b-90d2-4b16-a9b5-813f4a41983e)

<br> 13.3.	Navigieren Sie zu App-Pakete. Laden Sie folgende Pakete herunter und aktivieren Sie diese.
<br> -	File access control
<br> -	Calendar
<br> -	Contacts
<br> -	Group folders
<br> -	Quota warning
  
<br> 13.4.	Navigieren Sie zu Vorgestellte Apps. Laden Sie folgende Pakete herunter und aktivieren Sie diese.
<br> -	Brute-force settings
<br> -	Suspicious Login
<br> -	Two-Factor TOTP Provider
  
<br> 13.5.	Navigieren Sie zu Sicherheit. Laden Sie folgende Pakete herunter und aktivieren Sie diese.
<br> -	HIBP Login check
 
<br> 13.6.	Einrichten eines E-Mail-Servers. Navigieren Sie zu Persönliche Einstellungen.
 	
<br> 13.7.	Geben Sie Ihre E-Mail-Adresse an.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/add65448-45e9-496f-8c3d-43c10b34504a)

<br> 13.8.	In unserem Beispiel verwenden wir ein Google-Konto zum Versenden der E-Mails. Da in dem Google-Konto die 2-Faktor-Authentifizierung aktiviert ist, benötigen wir ein spezielles Anwendungspasswort für die Nextcloud, damit diese in unserem <br> Namen E-Mails verschicken darf. Öffnen Sie die folgende Webseite und navigieren Sie zu Sicherheit.
<br> https://myaccount.google.com/

<br> 13.9.	Klicken Sie auf Bestätigung in zwei Schritten
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/c2f4d512-1c15-4e2b-a578-967ae25fee95)

<br> 13.10.	Scrollen Sie ganz runter und klicken Sie auf App-Passwörter.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/95fe56cb-f2d0-4c7d-bec0-4b35eebff7fa)

<br> 13.11.	Vergeben Sie einen eindeutigen Namen wie z.B. Nextcloud und klicken Sie auf Erstellen.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/bcebed1c-6939-4614-8a70-6db0b37605db)

<br> 13.12.	Jetzt wurde ein spezielles Anwendungspasswort erstellt. Dieses kopieren Sie nun.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/50566809-7889-489c-89ef-3d278c382904)

<br> E-Mail-Anwendungspasswort:	___________________________________________
<br> 13.13.	Navigieren Sie in Ihrer Nextcloud zu Administrationseinstellungen  Grundeinstellungen.

<br> 13.14.	Passen Sie die E-Mail-Server Einstellungen wie folgt an. Ersetzen Sie Ihren Benutzernamen und Passwort.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/ec08234c-2767-4e2f-8a23-408ea912b876)

<br> 13.15.	Anschließend klicken Sie zum Testen der Einstellungen auf E-Mail senden. Folgendes Banner sollte erscheinen:
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/9ff44e03-6789-4057-8ac5-3e746337c5bd)

<br>	Überprüfen Sie Ihren, Posteingang, ob Sie eine neue E-Mail bekommen haben.  
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/72a1bcae-2d66-450c-8596-4c460a5a467e)

<br> 13.16.	Sollte bei Ihnen folgender Fehler auftreten, melden Sie sich per SSH an Ihrem Pi an.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/95a69180-c429-41fa-9469-19bd1c2e219d)
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/d401c69b-f768-43ef-a8ca-d3a4b9afe1f5)

<br> 13.17.	Geben Sie folgende Befehle ein.
```sh
sudo -s
```
```sh
cd /nextcloud/data
```
```sh
rm nextcloud-init-sync.lock
```
<br> 13.18.	Klicken Sie danach auf Erneut scannen.
<br> ![image](https://github.com/J-SIT/RaspberryPi-Docker-Compose-Nextcloud-PiHole-Stack/assets/52483642/30637add-4cfc-4c02-bdbe-dff0829a75b8)



Optional aber Empfohlen:
Startet ungesunde Container wieder.

Stackname: 04_autoheal
```yaml
version: '2'
services:
  autoheal:
    restart: always
    container_name: 07_AutoHEAL
    image: willfarrell/autoheal
    environment:
      - AUTOHEAL_CONTAINER_LABEL=all
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```