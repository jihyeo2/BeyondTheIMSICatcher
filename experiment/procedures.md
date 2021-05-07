# **Detailed Procedures**

## 1. PC Setup: Ubuntu 20.04 (Dual Boot), UHD Driver & srsRAN on core i7 PC

* [Ubuntu 20.04 (Dual Boot) Installation](https://linoxide.com/install-ubuntu-18-04-dual-boot-windows-10/)
* Install UHD Driver 

  ```
  $ sudo apt-get install libuhd-dev libuhd003 uhd-host
  ```
* Commands to install srsRAN
     
  Mandatory libraries:
  ```
  $ sudo apt-get install cmake libfftw3-dev libmbedtls-dev libboost-program-options-dev libconfig++-dev libsctp-dev
  ```
  Build srsRAN:
  ```
  $ cd ~
  $ git clone https://github.com/srsran/srsRAN.git
  $ cd srsRAN
  $ rm CMakeCache.txt
  $ make clean
  $ cmake ..
  $ make
  ```
  
## 2. Configuration
  
  We can either remove the extension(.example) of an example configuration file or install base configuration files by running the following command in the build folder.
  ```
  $ cd ~/srsRAN/build
  $ sudo srsRAN_install_configs.sh service 
  $ cd ./etc/srsran
  ```
  Configuration files are now installed for all users, not just to the user directory.
  
## 3. Create a private LTE network for COTS UE with programmable USIM 

### a. Program a USIM card with *Milenage* support

#### Requirements

This subexperiment requires a programmable USIM card with *Milenage* support along with a smart card reader. I believe the official manual along with many other related posts use Sysmocom USIM that is supported by pySIM which enables you to write key information on top of it. 

However, since I am not comfortable enough to spend $60 on products + @ on international shipping, I decided to buy one from AliExpress. I found a pretty decent, cheap USIM from OYEITIMES as well as their smart card reader, I bought both from them. Thankfully, for the software, I discovered [this gorgeous post](http://www.zhixun-wireless.top/install-and-configure-srslte-enb-epc-on-ubuntu) where you can download .rar of SIM Personalize tools. Thank you, John Wu!

#### Official Document - COTS UE Application Note
Most of the information are brought from this written by srsRAN team. Truly appreciate their hard work dedicated for beginners like me!

#### How to write on USIMs

Put a programmable USIM card into a smart card reader, connect it to your computer, and fire up the SIM Personalize tools (GRSIMWrite.exe). Now, click *Read Card*, then write your own IMSI, KI, and OPC. Click *Same with LTE* and finalize it with *Write Card* button.

![SIM Personalize tools](/images/SIM_Personalize_tools.png)
This is an image I copied from [here](https://nickvsnetworking.com/roll-your-own-usims-for-private-lte-networks/)(hehe give me a break guys), so the values will not represent the values I stated below.

For the mentioned three values, I believe they can be anything. In case you wonder, I used the following

```
IMSI : 901700000020936 
KI : 4933f9c5a83e5718c52e54066dc78dcf
OPC : fc632f97bd249ce0d16ba79e6505d300
```
Neat, now take out the USIM card and put it in your test UE/smartphone. Great job if you have followed along this far :)

### b. More Configuration
  
  There are in total of three configuration files to edit: epc.conf, enb.conf, and user_db.csv. "The eNB & EPC config files will need to be edited such that the MMC & MNC values are the same across both files. The user DB file needs to be updated so that it contains the credentials associated with the USIM card being used in the UE. (SRSRAN COTS UE Application Note)"
  
  #### * epc.conf
  ```
  22 | #####################################################################
  23 | [mme]
  24 | mme_code = 0x1a
  25 | mme_group = 0x0001
  26 | tac = 0x0007
  27 | mcc = 901
  28 | mnc = 70
  29 | mme_bind_addr = 127.0.1.100
  30 | apn = srsapn
  31 | dns_addr = 8.8.8.8
  32 | encryption_algo = EEA0
  33 | integrity_algo = EIA1
  34 | paging_timer = 2
  35 |
  36 | #####################################################################
  ```
  
  I will use the default test MCC & MNC value (901 & 70 or 001/01). If this does not work, MCC will tuned to 450, my [country code](https://en.wikipedia.org/wiki/Mobile_country_code), and MNC will be set to "any value that is not currently in use by a Mobile Network Operator in my country (SRSRAN eNodeB User Manual)", for example 02. 
  
  #### * enb.conf
  ```
  18 | #####################################################################
  19 | [enb]
  20 | enb_id = 0x19B
  21 | mcc = 901
  22 | mnc = 70
  23 | mme_addr = 127.0.1.100
  24 | gtp_bind_addr = 127.0.1.1
  25 | s1c_bind_addr = 127.0.1.1
  26 | n_prb = 50
  27 | #tm = 4
  28 | #nof_ports = 2
  29 |
  30 | #####################################################################
  ```
  
  Make sure MMC & MNC are same as those in epc.conf.
  
  #### * user_db.csv
  ```
  1 | #
  2 | # .csv to store UE's information in HSS
  3 | # Kept in the following format: "Name,Auth,IMSI,Key,OP_Type,OP,AMF,SQN,QCI,IP_alloc"
  4 | #
  5 | # Name:     Human readable name to help distinguish UE's. Ignored by the HSS
  6 | # IMSI:     UE's IMSI value
  7 | # Auth:     Authentication algorithm used by the UE. Valid algorithms are XOR
  8 | #           (xor) and MILENAGE (mil)
  9 | # Key:      UE's key, where other keys are derived from. Stored in hexadecimal
  10| # OP_Type:  Operator's code type, either OP or OPc
  11| # OP/OPc:   Operator Code/Cyphered Operator Code, stored in hexadecimal
  12| # AMF:      Authentication management field, stored in hexadecimal
  13| # SQN:      UE's Sequence number for freshness of the authentication
  14| # QCI:      QoS Class Identifier for the UE's default bearer.
  15| # IP_alloc: IP allocation stratagy for the SPGW.
  16| #           With 'dynamic' the SPGW will automatically allocate IPs
  17| #           With a valid IPv4 (e.g. '172.16.0.2') the UE will have a statically assigned IP.
  18| #
  19| # Note: Lines starting by '#' are ignored and will be overwritten
  20| ue3,mil,901700000020936,4933f9c5a83e5718c52e54066dc78dcf,opc,fc632f97bd249ce0d16ba79e6505d300,9000,0000000060f8,9,dynamic
  ```
  
  Make sure IMSI, KI, and OPC match those you used to program USIM previously. For the rest of the inputs, I just sticked to the default.
  
### c. Set up an APN point on your phone  

This part kind of got me off guard. It is because, unlike the phone settings that were shown in other guides, there were no way for my LG X4 phone to add a new APN point. Until I read that my carrier only allows it only if I place a foreign USIM in it. So, I did and finally it appeared as below. 

![X4 APN setting](/images/apn_X4)

The only thing I had to change was the name of my new APN point. I wrote it as "srsapn" so that I did not have to change the EPC configuration file. Make sure if the name of the APN in your test phone is same as that in epc.conf (line 30).

### d. Connect UE to the Internet via EPC (Optional)

In *srsRAN/srsepc*, there is a pre-configured masquerading script named *srsepc_if_masq.sh* which you can use to enable IP forwarding and set up Network Address Translation to provide Internet to UEs. In order to run this script, you must be aware of the interface you are using, so just type 
```
$ route
```
on you terminal.

![route output](/images/route_output.png)

I got the output as shown above, and, as you can see, my default destination is wlan0. My command then would look like
```
$ sudo ./srsepc_if_masq.sh wlan0
```

![masquerade success](/images/masquerade_success)

Now, I got this message saying the internet between PC and EPC was successfully set up.

### e. Run srsEPC & srsENB

First, open up a terminal and run the following commands to initiate EPC.

```
$ cd ~/srsRAN
$ sudo srsepc
```

![initialize EPC](/images/epc_initialization)

Great, EPC is now set up. Next, open up another terminal and run the commands below to start ENB.
```
$ sudo srsenb
```
I received an output noting a successful build for my ENB from the ENB console

![ENB setup success](/images/enb_setup_success)

and another output that ENB is now connected to EPC from the EPC console.

![EPC+ENB](/images/epc+enb)


### f. Connect UE to the private network

Finally, the last step! Stretch your back and your fingers and give your keyboard a break. Oh, and yourself. Now, pick up your phone and connect it to the network you have been working on.



I got the following messages from each console (EPC & ENB) as a confirmation for a successful connection.



Phew...At last, we are over, but only for this experiment. We are only one fifth way through. However, be proud of yourself. Even if you are going to quit here, that's already a great work you've achived there. For others who are continuing, way to go!!


## Passive Attack w/ srsRAN & WireShark

****From here, commerical USIMs were used, not the programmable ones I used in the previous subexperiment, "3. Create a private LTE network for COTS UE with programmable USIM ". Thus, I am assuming that I have no control over the victim UE. ****

### Objectives
Obtain GUTI/TMSI of victim's UE and gain crucial knowledge about parameters (EARFCN, TAC, MNC & MCC) for my rogue eNodeB to successfully mimick an operational network.

### Method
For a passive attack, I utilized the following three: Service Mode, pdsch_ue.c in srsRAN, and wireshark. Along with them, others were also used for various purposes such as decoding ASN.

### Background
Both my target UE and test UE are signed up to a Korean network carrier, LG U+. Thus, I can retrieve any information from my test UE, but not from my target UE, until I discover its GUTI and narrow my action towards it.

#### a. Service Mode: Obtain TAC, PLMN (MMC + MNC), EARFCN & GUTI/TMSI of my test UE

In order to open up the service, on the dialing pad, I typed *123456# (this number really depends on which vendor your phone is manufactured from.)

![service mode](/images/service_mode.png)

This showed up and, as you can see, I can see the **EARFCN (frequency band my phone is using)**, Bandwidth, **PLMN(MCC + MNC)**, Cell ID, **TAC**, RRC State, **GUTI/TMSI**, and neigboring cell types and their EARFCNs of my test UE in order to determine parameters for my rogue eNodeB. (TAC and GUTI/IMSI were blurred for privacy issues.)

In case you are wondering what other parameters mean, please refer to the list below:
* RSSI = Received Signal Strength Indicator.
* RSRP = Reference Signal Received Power.
* RSRQ = Reference Signal Received Quality.
* SINR = Signal to Interference plus Noise Ratio.
* SNIR = Signal to Noise plus Interference Ratio.
* SNR = Signal to Noise Ratio.
* ASU = Arbitrary Strength Unit.
* RS SINR or RSSNR = Signal to Noise Ratio.

With the following table, you can check if you are in a stable network connection. As you can see, mine is in a pretty good state.

![Rs](/images/Rs.png)

For more information, check out [this page](https://www.signalbooster.com/blogs/news/acronyms-rsrp-rssi-rsrq-sinr#:~:text=RSSI%20%3D%20Received%20Signal%20Strength%20Indicator,to%20Interference%20plus%20Noise%20Ratio.).

For a rogue eNodeB, I will be using the same TAC and PLMN (MMC + MNC) discovered in this step. However, in order to trigger cell reselection only on the target UE, I need to calculate an optimal EARFCN and figure out GUTI/TMSI of it.

**Calculate an optimal EARFCN**

As stated in [Evolved Universal Terrestrial Radio Access (E-UTRA); User Equipment (UE) procedures in idle mode (Release 16.3.0](https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=2432) shown below,
