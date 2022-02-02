SiESoCom mote setup & on-boarding guide
---------------------------------------

***1. Context***


A SiESoCom mote (S-mote) is a standard Raspberry PI 3B+ device to which we connect a USB microphone/acoustic sensor and (optionally) a temperature/humidity sensor.

S-motes use ethernet to connect to M3/Orion system. We made the design decision to make S-motes 'simple' i.e. their main system function is to be a managed data source while the processing of the data is  mostly ensured by containerized applications deployed on local edge cores.

_Note: As the SiESoCom specification evolve, the S-mote capability will also evolve, e.g. additional sensors can be enabled, more audio channels etc._


On-boarding a new SiESoCom mote typically consists of following 3 steps

●	Setup the mote hardware in particular connecting the sensors, flashing the OS and installing the SiESoCom mote application environment and application

●  Commissioning the SiESoCom mote resource on M3  infra servers

●  Initializing the SiESoCom mote using Device Setup Application (DSA)


The SiESoCom project is using "Raspberry pi 3B+" for mote hardware, so this guide refers to this particualar hardware. 

2.
**Step 1: Setting up the S-Mote hardware**




2.1 S-mote hardware inventory & assembly



The following components are required to build a S-mote:

•    Raspberry Pi 3 Model B+:
https://www.raspberrypi.org/products/raspberry-pi-3-model-b-plus/
•    32 GB SD card (16 MB can also work, but 32 is recommended)
•	(Optional) Temperature and humidity digital sensor (I2C interface): Sparkfun Spark Si702  https://www.sparkfun.com/products/13763.
•    Vibration sensor: SiESoCom accelerometer  PCB 608A11/010AC + USB A2D
converter Digital ICP Signal Conditioner, model 485B39


![f1](https://user-images.githubusercontent.com/98868747/152139195-67eb0014-4f9d-498b-9236-41a6dce69bb3.jpg)


 
Fig 1: S-mote on RPI3 B+

The connectivity is simple to wire:

•    The USB signal conditioner can be connected to any USB port of the RPI
device.
•	The optional T/H sensor shall be connected to the I2C bus of the RPI device: pin 8 and 9 of the master connector (fig 2 below). It also needs to be powered. You can use pin 1 and 9 for VCC and ground resp.
•    S-mote are networked over ethernet. 
![f2](https://user-images.githubusercontent.com/98868747/152139227-043aec5e-efca-43d6-91c0-b6e403446c72.jpg)

 
Fig 2: RPI master connector mapping



2.2 S-mote operating system install



S-motes are off-the shelf raspberry PI  3B+ hardware running the standard RPI operating system 'RPIOS'. Follow the instructions below to install RPIOS on the S-mote. These Instructions shall be run once when using the board for the first time.

•	Download OS zip file from https://downloads.raspberrypi.org/raspios_full_armhf_latest
•   Uncompress the downloaded zip to extract the  .img file.
•	Insert your SD card in SD card reader of your pc and flash the extracted .img file into your SD card, using a tool like 'Etcher'

 
![f3](https://user-images.githubusercontent.com/98868747/152139345-ecfb03a7-80d2-493d-a21f-3e47090d76ab.jpg)

Notice: it's recommended to re-format the SD-card prior to flashing it, in particular if you 'recycle' a previously flashed SD card for RPI (that windows PC will not recognize as valid). The freeware 'SD card formatter' does this job smoothly: 

![f4](https://user-images.githubusercontent.com/98868747/152139364-4ba820dd-b86e-4459-aa6e-2d8b4efe4d19.jpg)

 

•	For Linux host system you can use the “disks” utility to check the mount point and unmount all partitions (e.g /dev/mmcblk0 ) and then issue the following command:
$ sudo dd if=[path to .img] of=[path to mount point] bs=4M
•	Insert the SD card into raspberry pi, then connect ethernet cable, hdmi display, USB mouse and keyboard to your raspberry PI.
•	Power up the board and monitor the hdmi display: Raspberry PI OS will boot and will prompt you for some basic information like keyboard, location, changing password (to ensure problem free working please change the password) . When asked for Wi-Fi information please skip the remaining steps.  Note that this flashing step takes some time as the installer typically downloads and deploys many updates.


Important: Make sure the keyboard map is properly committed when inputing the new password (it's not always the case) and also before you disconnect the PI at the end of the process. If keyboard map is not committed, explicitly set it  via the RPI main menu (the raspberry icon on the top left of the display)-
>preferences->keyboard and mouse.
•	Open a Linux console for the next steps (application framework and application installation): On your HDMI screen you shall see on the upper right corner a black icon (first one).  Please click on it and proceed to the next step.


At this point, your RPI device has an OS and the SiESoCom application environment and application can be installed




2.3 Application environment install

This section deals with the installation of the M3  application environment on the RPIOS. 

Issue the following command on a linux console:
$ sudo wget -v -O prepare_rpi_script https://m3-shared-storage.s3-eu-west-
1.amazonaws.com/managed-mote/RaspberryPi/Scripts/prepare_rpi_script && sudo chmod a+x prepare_rpi_script && sudo ./prepare_rpi_script

You shall eventually see the following output and the board will be rebooted:

***********************************************************************
****************  Serial Number : CSP-B827EB689C39 *******************
****************  Host Name     : csp-b827eb689c39    *******************
********** Raspberry Pi3+ is ready for Managed Mote Application ***********
***********************************************************************

The Serial number (CSP-B827EB689C39) is used to create the device resource on the portal (Make a note of this Serial Number).

The Host name (csp-b827ebdd5bf4) is used to SSH to the board using Putty or WinSCP  by writing the Host Name instead of the Board IP and use port 22 (Make a note of this Host Name).

2.4 S-Mote application install

This section deals with the installation of the latest S-mote application on the RPIOS/M3  application environment.

Issue the following command on the console.
$ sudo wget -v -O setup_rpi_script https://m3-shared-storage.s3-eu-west-
1.amazonaws.com/managed-mote/RaspberryPi/Scripts/setup_rpi_script && sudo chmod a+x setup_rpi_script && sudo ./setup_rpi_script

When prompted to enter the mote management type, input "managed":

![f5](https://user-images.githubusercontent.com/98868747/152142884-ac17a9fe-15a7-493a-aa1b-d06517357ca8.jpg)



When prompted to enter the sensor source, input "external" and confirm the configuration with "yes":
 

![f6](https://user-images.githubusercontent.com/98868747/152142924-eb4c71f6-7abc-4dc5-b0c3-fad92139f52c.jpg)

At this point, your S-mote software is fully installed ,in 'blank' state, awaiting a secure injection of its application credentials (see section 4) Notes:

•	As the integration of the SiESocom solution progresses, the S-mote application will be updated (new features, bug fixes,..) . To deploy a new version of the S-mote application, the operator will just need
to call the 'setup_rpi_script', after having detached the S-mote from the M3  setup via the portal
$ sudo ./setup_rpi_script 


•    To download a specific version of the S-mote application, call the
'setup_rpi_script' with the version number as an argument e.g.
$ sudo ./setup_rpi_script <version>

•    To display logs in SSH session (debug situation), use this command:
$ tail -f
/usr/local/mgc/managed_mote_application/Logging/log.txt




3 Step 2: S-mote commissioning

This section deals with the commissioning of the S-mote resource (avatar) with the M3  SiESoCom portal. We assume here that the operator is logged in on a valid SiESoCom account on the M3  portal:

Portal URL: https://pspg.m3-solutions.io
SiESoCom integration account username: pspg.integration@gmail.com
SiESoCom integration account password: ***** (per previous communication)



3.1 S-Mote creation

To create (commission) a new S-Mote resource on a SiESoCom application account, the operator shall:

•   Navigate to "Device/Manage" menu item and click on the 'Add New' button:


 ![f7](https://user-images.githubusercontent.com/98868747/152143108-eb186aba-4fc5-48b7-a56d-d121f9cb4991.jpg)


•	Then, the operator shall fill the creation form, per the following example: 
![f8](https://user-images.githubusercontent.com/98868747/152143119-03213721-3b7b-48cd-b49a-82f71957d084.jpg)

 
Notes:

•	The serial number shall be an exact match to the serial number provided by the 'prepare_rpi_script' during the S-mote application environment install step (section 2.3 above). Make sure to not include any blank or tab character in the name (use copy/paste
with care)
•	The 'Manufacturer', 'Type', 'Model' and 'Firmware Version' shall be set to "CSP", "M3 mote",  "SiSoCom Peripheral" and "SiSoCom Mote FW 1.0 MQTT/Ethernet" resp.




•	After clicking on "Create", the S-Mote resource is created into the system with the state 'Commissioned', per the following snapshot: 

 


3.2 S-Mote attaching

To attach a commissioned S-Mote resource to a gateway and setup, the operator shall:

•	Navigate to "Device/Manage" menu item and select its commissioned device from the list:

 

•   Once the device details are displayed, click on the "Operation" tab then
"Attach" button: 

 

•	The attach options are then proposed. The operator shall select both a gateway and a setup:

  

Note: The gateway and setup of choice depends on the application topology, which is not discussed here.

After clicking on "Attach", the S-Mote resource state is updated to
'Attached' , per the following snapshot

 



At this point, your S-mote resource (avatar) is ready and the real-world device (RPI board) initialization can be carried out



4 Step 3: S-mote initialization

This section deals with the initialization (onboarding) of the S-mote hardware. The purpose of this step is to securely inject into the S-mote the operational credentials it requires to connect to an M3  gateway and report into an M3  setup.

This injection is carried out via the M3  Device Setup Application (DSA) after an authorized logging of the operator either over a serial interface or over a TCP/IP connection, provided the S-mote and the DSA host are on the same LAN.

The DSA application installer is available for Windows and Linux operating systems. The windows version can be downloaded from here: https://m3- shared-storage.s3.eu-west-1.amazonaws.com/apps/device-setup- client/releases/6.7.1/m3-dsa%20Setup%206.7.1.exe

The following instructions illustrate the S-mote initialization using the TCP protocol. This is the preferred method. This method however requires the knowledge of the local IP address allocated to the S-mote by the LAN DHCP server. This IP address can be discovered by checking the addresses allocated by the local router to the S-mote device. The user is encouraged to make this IP fixed by setting a DHCP rule is the local router. 



M3 infra & servers



LAN router with DHCP server

M3  credentials injection










 

S-Mote
 
Host PC running
DSA 

Logged in user
Fig: S-mote initialization

To initialize a S-Mote the operator shall:

•	Make sure it has DSA installed on a host PC connected to the internet via the same LAN than the one the S-mote is connected to (fig above)
•    Make sure the S-mote to initialize is connected and IP'ed to the same
LAN than the DSA app host (fig above)
•	Launch DSA app and confirm it is set to use the correct M3  domain: In our case ,it shall be "m3-solutions.io"

























•   Logging to the SiESoCom account of interest: 

 

•	Select the SiESoCom account, click on DEVICE, input the S-mote  serial number (from a drop down list), select TCP  and input the S-mote local IP address.

  

•	After clicking on "Connect", DSA will proxy a device/infra mutual authentication and relay the operational credentials generated by the M3  infra for S-mote. The progress is displayed on a transient window:



























•	Once the initialization process is finished, the S-mote device state transitions to 'online' and can be managed from the SiESoCom portal application. The DSA app can be disconnected from the mote

  


 

At this point, your S-mote can be managed.

Note: A DSA session to an already initialized mote is always possible, typically to update the IP address of the gateway to which the mote is attached. It is however strongly recommended to prevent gateway from getting different IP addresses by setting proper rules in the DHCP server.
 




ATTACH S-MOTE
-------------------------


•	Follow the steps described in "S-Mote setup and onboarding" guide to on-board a new S-Mote. Once the new S-Mote has been onboarded, it shall appear on the Devices list with the status "on-line". For the rest of the guide, we are assuming that we have onboarded S-mote "S- mote#1" (S/N CSP-B827EB2DB0F1)
 
***Important Notice: When 'attaching' the S-mote in section 3.2 of the S-Mote setup and onboarding guide, make sure to attach it to the 'good' Edge-Core 
    ("L890006" in this example) and Setup ("SiESoCom_2" in this example)*** 

![Picture8](https://user-images.githubusercontent.com/98868747/152133094-8850261e-76aa-4652-9572-e67ad702c5ad.jpg)

_The new S-Mote is now a member of the SiESoCom_2 setup_
