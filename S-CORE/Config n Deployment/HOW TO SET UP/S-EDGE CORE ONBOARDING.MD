

SIESOCOM EDGE CORE --- SETUP & ONBOARDING GUIDE
------------------



1 Context
---------


**On-boarding a new SiESoCom Edge-Core (S-Edge-Core) device typically consists of following 3 steps:**

●  Installing S-Edge-Core Distribution on Edge-Core hardware

●  Commissioning the S-Edge-Core resource on M3  infra servers

●  Initializing the S-Edge-Core using Device Setup Application (DSA)


***The SiESoCom project is using Siemens IPC "SIMATIC Microbox 427E" for Edge-Core hardware, so this guide refers to this particualar hardware.***



2 Step 1: Installing S-Edge-Core distribution




2.1 Preparing a USB stick installer




2.1.1     Software Requirement


•	The paths to latest images for S-Edge-Core distribution are available available [here](https://github.com/alya-arshad00/siesocom_/tree/main/Releases)

●	_A USB Flashing utility : e.g Etcher is used to burn the image on the usb mass storage device for installation._


2.1.2     Hardware Requirement

●	Siemens SIMATIC Microbox PC (e.g. 427E with AVX instruction set support)
●  Display (supporting DisplayPort - DP)
●  Keyboard
●  USB Flash Disk (16 GB or more)


2.1.3     Preparing the Installation Disk

Download and decompress the image from the latest release path. After download and unzip, you should see:

 


Using USB Flashing utility, burn this installer image on a USB flash disk, using a tool like "Etcher'.

 

Notice: it's recommended to re-format the USB flash disk prior to flashing it, in particular if you 'recycle' a previously flashed disk for Edge-Core (that  windows  PC  will  not  recognize  as  valid).  The  freeware  'SD  card formatter' does this job smoothly:

**2.1.4     Connecting Peripherals to SIMATIC Microbox PC**

●  Connect the power, display and keyboard to the SIMATIC Microbox PC
●	Connect the network cable. There are 3 ethernet ports on 427E labeled as X1 P1, X2 P1 and X3 P1. Ensure to connect the network cable on
Port X1 P1. Other two ports are not supported.


| Connection      | Port Label | Usage Description |
| ----------- | ----------- |----------- |
| USB      | X60, X61, X62, X63       | Any posrt can be used.    |
| Display   | X71, X70        |Any port can be used.     |
| Ethernet   | X1, P1        |Only use this for LAN connectivity  |
| Ethernet   | X2 P1, X3 P1       |Unused - Do not use these.     |


**2.2 Extracting IPC serial number and LAN1 MAC**

The secure commissioning of the S-Edge-Core requires the knowlegde of the IPC serial number and the MAC address of its LAN1 interface. This section explains how to get these info:


The serial number in available on the identification sticker of the SIMATIC
microbox

The MAC address can be obtained from the SIMATIC BIOS application as follow:
•    Connect a keyboard to the SIMATIC microbox
•    Power up the SIMATIC IPC while hitting repetitively the ESC key
•    When Bios menu appears, select "SCU" option, then "Advanced" tab
•    Read LAN 1 MAC

Example:
In the following example the S/N is L8950006 and LAN 1 MAC is
20:87:56:F2:20:9A

 


  

 

Keep these 2 informations available for the commissioning step of the process.



**2.3 Setting the IPC Boot Order & Flashing the IPC**

●	Insert the newly flashed USB Flash Disk in any USB Port of SIMATIC Microbox PC and power on the device.
●  As soon as BIOS Notice appears, press Esc OR F12 on the Keyboard to
enter into BIOS Menu.
●	On the BIOS Menu press “Boot Manager”. You should see 2 boot devices as shown below. Select “EFI USB Device” and press ENTER to boot from USB disk.

 

●	The installation will start. During the installation the Maintenance L3 LED on 427E is steady RED. You may also notice information on your display related to installation but you can safely ignore that.
●  Once the installation completes successfully, Maintenance L3 LED on 427E will blink as heartbeat in ORANGE. You can now Power off the device.
_Notes:
This image writing operation typically take 5 mn, depending on the class of the USB device. 

_The 'prod' version of M3 edge core image does not output anything on the device console for security reasons. The operator shall monitor the LEDs during installation._

●	Remove the USB Flash Installer Disk. (This USB stick can be reused to flash another IPC)
●  Power on the IPC. By default the Siemens SIMATIC Microbox PC will
boot from an internal SATA disk but you can ensure this in the BIOS Settings.
●  After this first boot, the M3  Edge Core device comes up in Blank
state: The LED L1 is ON state in ORANGE color.



At this point, your S-Edge-Core is fully installed, in 'blank' state, awaiting a secure injection of its infra credentials (see section 4 below)




3 Step 2: S-Edge-Core Commissioning

This section deals with the commissioning of a S-Edge-Core resource
(avatar) with the M3  SiESoCom portal.

We assume here that the operator is logged in on a valid SiESoCom account on the M3  portal:

Portal URL: https://pspg.m3-solutions.io
SiESoCom integration account username: pspg.integration@gmail.com SiESoCom integration account password: ***** (per previous communication)


We also assume the operator has gotten the IPC serial number and LAN1 MAC
address of the IPC to commission, per section 2.2 above

•	login to the SiESoCom account of interest using the following details: Portal URL: https://pspg.m3-solutions.io
SiESoCom integration account username: pspg.integration@gmail.com
SiESoCom integration account password: ***** (per previous communication)

•   Navigate to "Edge Cores/Manage" menu item and click on the 'Commission
New' button: 

 


•	Then, the operator shall fill the creation form, per the following example:


 

Notes:

•	The serial number and LAN1 MAC address shall be an exact match to the serial number  and MAC address obtained previously. (section
2.2 above)
•	The 'Oem' and 'Type' shall be set to "Siemens" and "simatic- ipc:cspedge" resp.
•    The 'Base Firmware' shall match the version actually flashed on
the IPC  in the previous step (section 2.3 aboveError! Reference s ource not found.) 

•	After clicking on "Commission", the S-Edge-Core resource is created into the system with the state 'Never Connected', per the following snapshot:



 



At this point, your S-Edge-Core resource (avatar) is ready and the real- world device (IPC) initialization can be carried out





4 Step 3: S-Edge-Core initializion




This section deals with the initialization (onboarding) of the S-Edge-Core hardware. The purpose of this step is to securely inject into the S-Edge- Core the operational credentials it requires to connect to M3  infra.

This injection is carried out via the M3  Device Setup Application (DSA) after an authorized logging of the operator either over a serial interface or over a TCP/IP connection, provided the S-Edge-Core and the DSA host are on the same LAN.

The DSA application installer is available for Windows and Linux operating systems. The windows version can be downloaded from here: https://m3- shared-storage.s3.eu-west-1.amazonaws.com/apps/device-setup- client/releases/6.7.1/m3-dsa%20Setup%206.7.1.exe

The following instructions illustrate the S-Edge-Core initialization using the TCP protocol. This is the preferred method. This method however
requires the knowledge of the local IP address allocated to the S-Edge_Core
by the LAN DHCP server. This IP address can be discovered by checking the addresses allocated by the local router to the S-Edge-Core. The user is encouraged to make this IP fixed by setting a DHCP rule is the local router. 



M3 infra & servers



LAN router with DHCP server


M3  credentials injection











 


IPC 427E
 
Host PC running
DSA 

Logged in user


Fig: S-Edge-Core initialization


To initialize a S-Edge-Core the operator shall:

•	Make sure it has DSA installed on a host PC connected to the internet via the same LAN than the one the S-Edge-Core is connected to (fig above)
•	Make sure the S-Edge-Core to initialize is connected and IP'ed to the same LAN than the DSA app host (fig above)
•	Launch DSA app and confirm it is set to use the correct M3  domain: In our case ,it shall be "m3-solutions.io" 

 




•   Logging to the SiESoCom account of interest:

 

•	Select the SiESoCom account, click on Edge Core, input the S-Edge-Core serial number (from a drop down list), select HTTP  and input the S- Edge-Core local IP address. 

 

•	After clicking on "Connect", DSA will proxy a S-Edge-Core/infra mutual authentication and relay the operational credentials generated by the M3 infra for S-Edge-Core. The progress is displayed on a transient window:

  



•	Once the initialization process is finished, the S-Edge-Core state transitions to 'online' and can be managed from the SiESoCom portal application. The DSA app can be disconnected from the Edge-Core.

 

 


At this point, your S-Edge-Core can be managed. 

5 Appendix A. LED Status Chart

Below is the LED status mapping on the SIMATIC IPC before , during and after the installation of the firmware image are mapped in the form of table matrix.


LED L1            LED L2               LED L3              State

OFF               OFF                  RED - ON            Firmware
Installation
 
OFF	OFF                  ORANGE - Heartbeat
 
Installation
Finished 

GREEN - Timer     OFF                  OFF                 System
Initializing

GREEN - Timer     RED - ON             OFF                 Network error during system initialization
 
GREEN - Timer     RED - Heartbeat (30 seconds)
 
Time synchronization failed. 

ORANGE - On       OFF                  OFF                 Device in Blank
State
 
GREEN - Heartbeat
 
OFF	OFF                 Establishing connection with M3
Platform 

GREEN - On                                                 Connection with M3
Platform established
 
ORANGE - Heartbeat
 
Application
Deploying 
 
GREEN - Heartbeat
 
Application
Starting 

GREEN - On                                                 Application
Started
 
RED - Heartbeat (30 seconds)
 
Application Error 

RED - ON                                Fatal Error



Onboard & Attach a S-Edge-Core to a Setup
--------------------------------------------

•	Follow the steps described in the "S-Edge-Core onboarding" guide to on-board a new S-Edge-Core. Once the new Edge Core has been onboarded, it shall appear on the Edge-Core list with the status "on- line". For the rest of the guide, we are assuming that we have onboarded Edge-Core S/N "L890006".

![Picture7](https://user-images.githubusercontent.com/98868747/152132010-1201c26b-65b9-4257-91ca-c3e564633b93.jpg)


•	Attach the Edge Core to the Setup by clicking on (i) the Edge-Core on the list, then (ii) on the "Setups" tab of the L8950006 Edge Core and (iii) on the "Join Setup" button:

![Picture6](https://user-images.githubusercontent.com/98868747/152132000-a5a8a053-e7f8-442f-827a-ee217aae397a.png)

•	A drop down window will display all the available Setups on the account. Select the setup you want the edge core to join and click on "Join". In this example, we assume we join the Setup "SiESoCom_2"

![Picture5](https://user-images.githubusercontent.com/98868747/152131988-f7662d39-f164-4e6d-9304-3a5b5ba06ae0.png)

![Picture4](https://user-images.githubusercontent.com/98868747/152131970-358ab698-7bdb-4dc0-808c-2e8736417fb9.png)

 // The new S-Edge-Core is now a member of the SiESoCom_2 setup
