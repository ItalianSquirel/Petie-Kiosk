# Petie-Kiosk
A kiosk mode mah dude, get it to load on boot mah dude.
`sudo apt-get install xdotool unclutter sed`

`sudo raspi-config`

`nano /home/pi/kiosk.sh`

——————————————————————————————————————————

```bash
#!/bin/bash

xset s noblank
xset s off
xset -dpms

unclutter -idle 0.5 -root &

sed -i 's/"exited_cleanly":false/"exited_cleanly":true/' /home/pi/.config/chromium/Default/Preferences
sed -i 's/"exit_type":"Crashed"/"exit_type":"Normal"/' /home/pi/.config/chromium/Default/Preferences

/usr/bin/chromium-browser --noerrdialogs --disable-infobars --kiosk http://127.0.0.1:1880/ui/ &

while true; do
   xdotool keydown ctrl+Tab; xdotool keyup ctrl+Tab;
   sleep 10
done
```





——————————————————————————————————————————



`echo $DISPLAY`

`sudo nano /lib/systemd/system/kiosk.service`



——————————————————————————————————————————

```[Unit]
Description=Chromium Kiosk
Wants=graphical.target
After=graphical.target
```

```[Service]
Environment=DISPLAY=:0
Environment=XAUTHORITY=/home/pi/.Xauthority
Type=simple
ExecStart=/bin/bash /home/pi/kiosk.sh
Restart=on-abort
User=pi
Group=pi
```

```[Install]
WantedBy=graphical.target
```



——————————————————————————————————————————
` sudo systemctl enable kiosk.service`

`sudo systemctl start kiosk.service`



Check:

`sudo systemctl status kiosk.service`

STOP:

`sudo systemctl stop kiosk.service`

REBOOT:

`sudo reboot`

