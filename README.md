# Project-Pinfiltrate
IOT device capable of performing network reconnaissance in a plug and play usage 
# Flashing Kali to SD Card    
To begin, download the kali image from kali.org. Ensure that the download is from the ARM section as shown below. Do make sure that the correct version is chosen. In this case, the first option is used.  
![image](https://user-images.githubusercontent.com/97077110/148011185-c190b42a-90aa-4d15-bba8-10e33dcc2072.png)  
Next, Install raspberry pi imager from https://www.raspberrypi.com/software/. This will allow you to flash kali into the SD card for the raspberry pi to boot. After downloading the software, proceed to run the raspberry pi imager. 
![image](https://user-images.githubusercontent.com/97077110/148011274-6f40d8f2-cf6d-4c12-92b8-b21ea1df0d95.png)  
Select the kali OS downloaded online and choose to install it onto the SD card. Next click write. This process should take 15 minutes.   

Next, insert the sd card into the raspberry pi and boot it up. It should turn on like a normal computer with Kali Linux OS.  

Headless Setup:
To create a headless setup, we must configure the kali to autologin and auto connect to wifi. To do this,  I looked for the config file that was incharge of network connections (/etc/network/interfaces). 
Next, I edited the file to ensure kali auto connects to the network every time by adding the following.
```
auto wlan0
allow-hotplug wlan0
iface wlan0 inet manual
wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf
iface default inet dhcp
```
Afterwards, kali began to auto reconnect to internet every time we log in.  

Finally, to complete the headless setup, I had to configure auto-login. To do so, I editted the following config files ```/etc/lightdm & /etc/pam.d/```.  
```
# /etc/lightdm
autologin-user=root

autologin-user-timeout=0

# /etc/pam.d/

auth required pam_succeed_if.so user != root quiet_success
```
After a reboot, kali will now 
	1. Auto Login without user input
	2. Auto reconnect to a previously connected network wirelessly (Wi-Fi)

# Airodump  
We have used a automation script from Github to run airodump and airo crack  
Step 1: Run the command ```python automation.py```  
A menu will be displayed.  
![image](https://user-images.githubusercontent.com/97077110/148013026-14b6dc58-f439-4c0d-bbac-b0a5d0d253ae.png)
Step 2: Download any tools that is required 
Run the command python automation.py -i






  
