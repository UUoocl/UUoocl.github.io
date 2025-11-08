---
{"dg-publish":true,"permalink":"/obs-scripts/keynote-controls/","noteIcon":"default","created":"2025-11-06T10:01:12.665-05:00","updated":"2025-11-07T08:43:09.056-05:00"}
---

Keynote Controls is an OBS Python script that enables...
- Navigating Keynote slides with OBS hotkeys
- Syncing OBS Scene changes to Keynote slides
- Scrolling slide presenter notes in an OBS Text Source

## Setup
### Install Python
Python is a programming language that will need to be installed for the Keynote Controls to work.

OBS supports python version 3.11 .  It's important that Python 3.11 is installed.  Newer versions are available, but these newer versions will not work. 

>[!important]
> [Download and Install Python version 3.11](https://www.python.org/downloads/release/python-3119/)

1. Download the MacOS installer
2. Install Python 3.11 to the default location
### Configure OBS Python Settings
OBS needs to know where Python is installed.  Use the macOS Terminal app to find the Python install path. Then enter the Python install path in OBS  

1. Open the Terminal app
2. Enter the 'which' command  
```shell
which python3.11
```
	The default install path will be returned
```
/Library/Frameworks/Python.framework/Versions/3.11/bin/python3.11
```
3. Use the mouse to highlight the path
4. Right click and select "Copy"

5. Open OBS
6. From the menu bar select `Tools > Scripts` 
7. In the Scripts window switch to the `Python Settings` tab 
8. Click the "Browse" button
9. Use the keyboard shortcut `cmd + shift + G`
10. In the pop-up, right click in the "Go To Folder" input field 
11. select paste
![Pasted image 20251106140904.png](/img/user/Pasted%20image%2020251106140904.png)
12. Press "Enter"
13. Click the "Open" button
The Pythons settings should now display "Loaded Python Version: 3.11"
![Pasted image 20251106141241.png](/img/user/Pasted%20image%2020251106141241.png)


## Add the Keynote Controls  Script to OBS
The "Keynote Controls" script and accompanying OBS Collection can be found on Github.

### Import the Keynote Controls OBS Collection
1. Download the [Keynote Controls Collection](https://github.com/UUoocl/Keynote_Controls/blob/main/keynote_controls.json)
2. In OBS, from the menu select `Scene Collection > Import`
3. Import the collection
4. Choose the collection `Scene Collection`

### Add the Keynote Controls Script
1. Download the [Keynote Controls Script](https://github.com/UUoocl/Keynote_Controls/blob/main/obs_python_keynote%20osascript.py)
2. In OBS, from the menu select `Tools > Scripts`
3. Click the `+ Add script` button

After installing the script, adjust the settings to the camera and slides scene. 

The "Slides" scene includes a "macOS Screen Capture" source to capture the Keynote slides. You can set the 'Display' as needed.   

![2025-11-06_14-46-18.png](/img/user/2025-11-06_14-46-18.png)


## Using "Keynote Controls"

### Navigate Slides

### Add Scene tags to slides

### Adjust teleprompter


## Configure Remote Controlller

