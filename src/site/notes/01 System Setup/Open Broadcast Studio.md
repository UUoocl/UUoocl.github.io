---
{"dg-publish":true,"permalink":"/01-system-setup/open-broadcast-studio/","noteIcon":"2","created":"2025-11-16T19:20:50.907-05:00","updated":"2025-11-16T20:23:12.380-05:00"}
---

## Install OBS

Download the installer from [obsproject.com](https://obsproject.com/)

## Open OBS

Some of the UUinsome scripts use the OBS webSocket Server.  
To configure the webSocket server use the "open obs" page.

https://uuoocl.github.io/set-websocket-server/open-obs.html

>[!note]
>For macOS users download these shortcuts to automate opening OBS
>[Open OBS with Debug mode enabled](https://www.icloud.com/shortcuts/38fd3fc730a247b28712b0d3a85bb918)
>[Open OBS with Debug mode disabled](https://www.icloud.com/shortcuts/9b7279e3430a4454b654a513d5645f91)

## Configure OBS open settings

>[!Note] OBS open settings
>**Configuration name** = give the configuration a name that can be reused
>
>**OBS name** = default set for macOS to "OBS", change to the full path in windows
>
>**Profile Name** = Enter an OBS video profile
>
>**Collection Name** = Enter an OBS Scene Collection
>
>**WebSocket Server Port** = Enter an available Port number.  OBS default 4455.  The range 49152-65535 is recommended 
>WebSocket Server Password = enter a password of your choosing
>
>**Debug Mode** = check to debug browser source.  debug page http://localhost:9222/

>[!important]
>If the macOS shortcuts are imported, press the "Open OBS" button.
>
>Otherwise press the "Copy Command" button.  Open a command line terminal and paster the OBS open command

![Pasted image 20251116193840.png](/img/user/Pasted%20image%2020251116193840.png)

## Test the WebSocket Connection

Press the "Connect" button to connect to the OBS webSocket Server.
The button will turn green if the connection is successful. 

![Pasted image 20251116202043.png](/img/user/Pasted%20image%2020251116202043.png)