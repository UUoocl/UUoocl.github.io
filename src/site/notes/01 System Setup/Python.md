---
{"dg-publish":true,"permalink":"/01-system-setup/python/","noteIcon":"2","created":"2025-11-15T14:15:03.278-05:00","updated":"2025-11-16T19:20:42.011-05:00"}
---

Python is a programming language that will need to be installed for the many of the tools in this garden to work.

## Install Python
OBS supports python version 3.11 .  It's important that Python 3.11 is installed.  Newer versions are available, but these newer versions may not work. 

>[!note]
> [Download and Install Python version 3.11](https://www.python.org/downloads/release/python-3119/)

1. Download the MacOS installer
2. Install Python 3.11 to the default location

## Install Python dependencies
Some scripts in the garden use libraries.  Enter the following command to install all the libraries  
 1. Open the Terminal app
 2. Enter the `pip` command
 ```
 python3.11 -m pip install python-osc python-rtmidi simpleobsws pynput
 ```

### Configure OBS Python Settings
OBS needs to know where Python is installed.  Use the macOS Terminal app to find the Python install path. Then enter the Python install path in OBS  

1. Open the Terminal app
2. Enter the 'which' command  
```shell
which python3.11
```
	The default install path will be returned
>[!example] example response
>`/Library/Frameworks/Python.framework/Versions/3.11/bin/python3.11`

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
![Pasted image 20251106141241.png|300](/img/user/Pasted%20image%2020251106141241.png)
