# Project-Pinfiltrate
IOT device capable of performing work reconnaissance in a plug and play usage 
# Flashing Kali to SD Card    
### Step 1
Download the kali image from kali.org. Ensure that the download is from the ARM section as shown below. Do make sure that the correct version is chosen. In this case, the first option is used.  
![image](https://user-images.githubusercontent.com/97077110/148011185-c190b42a-90aa-4d15-bba8-10e33dcc2072.png)  
### Step 2  
Install raspberry pi imager from https://www.raspberrypi.com/software/. This will allow you to flash kali into the SD card for the raspberry pi to boot. After downloading the software, proceed to run the raspberry pi imager. 
![image](https://user-images.githubusercontent.com/97077110/148011274-6f40d8f2-cf6d-4c12-92b8-b21ea1df0d95.png)  
### Step 4  
Select the kali OS downloaded online and choose to install it onto the SD card. Next click write. This process should take 15 minutes.   

### Step 5
Insert the sd card into the raspberry pi and boot it up. It should turn on like a normal computer with Kali Linux OS.  

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
# Wireless security  
An area of attack that the team decides to attack is targeting devices that use wireless connection. By using Raspberry PI and a Wifi adapter, we will be able to scan and discover potential networks and exploit these networks using the right tool. This section will cover tools that will be used to bypass wireless defenses and perform penetration testing.

# Airodump  
Now, let's change the Mac address of this interface to hide our true identity. Use macchanger to change your Mac to a random value and specify your interface. There are options to switch to another type of device; however, for this example, we will just leave it as a random Mac address using the following command: ```macchanger -r wlan0```.  

## Automation Script:  
We have used a automation script from Github to run airodump and airo crack. 
Download the file from [https://github.com/no0bie/Automate-Aircrack-ng](url) and extract the fil onto Desktop.  
The team have added an additional code into this automated script.    
```
def c():
        l =open("NODELETE.txt","w")
        x=raw_input("Your wireshark file name:")
        time.sleep(1)
        command= "aircrack-ng -w /usr/share/wordlists/rockyou.txt"+" "+x 
        l.close()
        os.system(command)

elif a == "-c":
                c()



print("-c       (6)     Attempt to crack password")
``` 
Because the original script does not have aircrack capabilities we have appented this code into the script to enhance its capabilities.  

We have also added sequence numbers to the script so that users are able to know which sequences come first.  
![image](https://user-images.githubusercontent.com/97077110/148357955-6c77bf3e-eacb-4549-9802-cdcb58103ed7.png)  
 
## Using of the Automation script:  

### Step 1  
Run the command ```$  python automation.py``` , a menu will be displayed.  
![image](https://user-images.githubusercontent.com/97077110/148013026-14b6dc58-f439-4c0d-bbac-b0a5d0d253ae.png)  
### Step 2  
Download any tools that is required, run the command ```$  python automation.py -i```.  
The monitor mode in the wireless card enables the device to listen to every packet around us. While Managed mode captures packets that have the device’s MAC address.Therefore for us to start capturing packets and discovering networks around us. We have to set our wireless device into Monitor mode.  
### Step 3 
We shall switch the interface from managed mode to monitor mode.Run the command ```$  python automation.py -si```.  
![image](https://user-images.githubusercontent.com/97077110/148358380-e7bc81f8-1c6e-448d-89a4-c5aea64ab841.png)  
### Step 4  
We will detect all the networks around us to find a potential target by running the command ```$  python automation.py -m``` 
### Step 5  
Monitor a network (Target's network) by using the command ```$  python automation.py -mc```
![image](https://user-images.githubusercontent.com/97077110/148358676-0ef954cb-d576-47f9-8c6d-7163e8515e59.png)  
### Step 6
Run the command ```automation.py -d```.  
The channel can be found in step 3 under **CH**.  
The Mac address can be found in step 3 under **BSSID**.  
Now, we will try to capture traffic from that wireless device, we have to wait until **EAPOL** appears befire we end the process.  
![image](https://user-images.githubusercontent.com/97077110/148359144-f19b2320-9c55-4fdd-a4cc-b29861ab903d.png)  
Step 7:??????????  

# Zerotier Vpn 
### Step 1
Head to [http://download.zerotier.com/debian/buster/pool/main/z/zerotier-one/](url) and locate the required file , which is zerotier-one_1.4.6_arm64.deb.  
![image](https://user-images.githubusercontent.com/97077110/148359739-81bbdd2a-da57-4bc2-9207-1ff761695c44.png)  
### Step 2
After locating the file , right click and click on *copy link address*. Next, head over to the Linux terminal , and key in ```wget http://download.zerotier.com/debian/buster/pool/main/z/zerotier-one/zerotier-one_1.4.6_arm64.deb```. The file will then be downloaded onto Linux.  
![image](https://user-images.githubusercontent.com/97077110/148359933-48ccb1f7-e5c5-4d9e-9fe0-37b8a86fc20c.png)  
### Step 3
The file can now be downloaded and de-packaged by using the command ``` sudo dpkg -i zerotier-one 1.4.6_arm64.deb```.  
![image](https://user-images.githubusercontent.com/97077110/148360115-637749e9-413e-41cd-8e44-19b35cb8f86e.png)  
### Step 4  
Zeroiter can be checked if it is successfully installed onto the Linux machine by using the command ```sudo zerotier-cli info```.  
![image](https://user-images.githubusercontent.com/97077110/148360322-f097fda5-5201-413d-ba35-678800f79c10.png)  
### Step 5  
Since zerotier is online, we can now procees to join a netowrk. However, to join a network, a Zerotier account is required. Thus, go to [https://my.zerotier.com/ ](url) and click on the create button to create a Zerotier account.  
![image](https://user-images.githubusercontent.com/97077110/148360560-8b898889-ba9d-441a-a09b-48d292d88067.png)  
### Step 6  
Click on *Create a network* on the Zerotier Central terminal and a network in Zerotier would then be created under your account.  
![image](https://user-images.githubusercontent.com/97077110/148360733-09c1932b-90fb-4bc8-af7b-e6ff47b2cb18.png)  
### Step 7  
copy the Network ID of the Zerotier network that you have created on the Linux terminal using command ```sudo zerotier-cli join networkID of your zerotier account```. In this situation, it would be *sudo zerotier-cli join d5e5fb653708e1db*.  
![image](https://user-images.githubusercontent.com/97077110/148361021-b77324f3-6590-4ec1-8dbf-b3977f5e26a6.png)  
### Step 8  
Use command ```sudo zerotier-cli peers``` to check if you have successfully joined the network. If successfully joined, people who are in the network will be shown.  
![image](https://user-images.githubusercontent.com/97077110/148361269-12c8e6ab-5981-4a51-b3a0-266951aa244b.png)  
### Step 9  
Wait for approximately 5 minutes on the Zerotier Central and refresh the page. New devices that had joined the network would be updated.  
![image](https://user-images.githubusercontent.com/97077110/148361514-01aeea67-14b6-4e87-83e6-761c589f987c.png)  
## Step 10  
Use command ```ip a``` on the Linux terminal to find out which ip address the Zerotier belongs to after you have successfully joined the network and Zerotier Central was shown appearing.  
![image](https://user-images.githubusercontent.com/97077110/148361807-a3fc424c-c973-49f7-9033-12061f5a7d9b.png)  
## Step 11  
Remote Desktop Conenction can now be established to the Raspberry Pi with the IP address of the Zerotier. In this situation, it would be *192.168.191.157*.  
![image](https://user-images.githubusercontent.com/97077110/148362548-4316e44d-dac1-4889-96d1-d1aa797d9357.png)  
Thus, Zerotier is now completely setup and you are able to remotely connect to the Raspberry Pi.  

# TheFatRat tool  

Installation of TheFatRat tool from GitHub.Run command 
$  sudo git clone https://github.com/screetsec/TheFatRat























  
