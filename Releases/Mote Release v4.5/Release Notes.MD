

|RELEASE TITLE/VERSION|	Mote Application Release v4.5|
|---|---|
|RELEASE DATE|	13-Dec-2021|
|RELEASE STATUS|	Frozen|
|RELEASE PATH/LOCATION| ---|
|TAGS|	CEA 1.4, TSA 1.5, RAF 1.5, MOTE 4.5|

RELEASE ENVIRONMENT
-------------------
1.	The Mote is running on OS version “https://m3-shared-storage.s3.eu-west-1.amazonaws.com/mote/os/rpi/managed_mote_rpios_16a.zip”.
2.	The Mote is using “Actuated Ball Valve V2” model with FW “Linux 2.1”.
3.	The Mote is tested against Sematic IPC FW v 1.3.8150-dev.
4.	The Mote is tested against Broker container image “Managed-MQTT-Broker 1.3”.
5.	The Mote Supports Model “M3 Generic Peripheral V2” Model with firmware “M3 Generic Mote FW 1.4 MQTT/Ethernet”.
6.	The Mote Supports Model “SiSoCom Peripheral” Model with firmware “SiSoCom Mote FW 1.0 MQTT/Ethernet”.
7.	The Mote is initialized with DSA version 6.7.1.
8.	The Release is uploaded to Amazon S3 Bucket.|

APPLICABLE HARDWARE
-------------------


APPLICABLE SOFTWARE 
-------------------


OVERVIEW 
--------
This document describes changes and addition to the M3 stack with the latest SiesoCom release.
A SiESoCom application(S-app) is a scalable, multi-tier, AI-centric application deployed as various managed entities on various hosts. This release enables the M3 users to set up an S-App.

APPLICABLE USERS/PROJECTS
------------------------
All M3 registered users will be able to view and utilize the SiesoCom features.

WHAT’S NEW
----------
1.	This release enables the capture, recording, and playback of raw acoustic data through ‘managed’ and ‘unmanaged’ motes.
2.	This release brings below apps to the mix that eventually enables the set up of an end-to-end S-App:
    •	Audio Stream Filer [link to specifications]
    •	Controller & Events Archiver (CEA) [link to specifications]
    •	Event Inference Pipeline (EIP) [link to specifications]
    •	Raw Audio Filer (RAF) [link to specifications]
    •	Time Series Aggregator (TSA) [link to specifications]

WHAT’S FIXED
------------
Below list of issues were shipped with current release:
1.	The Mote is integrated with Agent v5.0.0.
2.	Fix playback file download interruption
3.	Fix playback file download deadlock which cause (mote dead state)
4.	Add publisherInfo filter for avoiding unrelated signals from reaching the mote
5.	Fix detach race condition with prevent the mote from restarting

Below list of bugs were reported and fixed during the testing of release:
### will fetch list from jira ###


ISSUES/LIMITATIONS/CHALLENGES
-----------------------------

HOW TO SET UP
-------------
1-	[Create a Setup]
(https://github.com/alya-arshad00/siesocom_/blob/018e511a9b5adeb9454b3d8997baa9d84539ee24/S-CORE/Config%20n%20Deployment/HOW%20TO%20SET%20UP/Create%20A%20Setup.MD)
2-	[Onboard and attach an S-Edge Core]
(https://github.com/alya-arshad00/siesocom_/blob/018e511a9b5adeb9454b3d8997baa9d84539ee24/S-CORE/Config%20n%20Deployment/HOW%20TO%20SET%20UP/S-EDGE%20CORE%20ONBOARDING.MD)
3-	[Onboard and attach an S-Mote] (https://github.com/alya-arshad00/siesocom_/tree/main/S-MOTE/Config%20n%20Deployment/HOW%20TO%20SET%20UP)
4-	Commission and start the Message Bus
5-	Commission and start the CEA tier
6-	Commission and start the TSA tier
7-	Commission and start the RAF tier
8-	Commission and start the AFE tier
9-	Commission and start the AED tier

APPLICABLE CONFIGURATIONS
-------------------------
