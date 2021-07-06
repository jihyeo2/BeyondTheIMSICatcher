# **Detailed Procedures**

## 1. PC Setup: Ubuntu 20.04 (Dual Boot), UHD Driver & srsRAN on core i7 PC

* [Ubuntu 20.04 (Dual Boot) Installation](https://linoxide.com/install-ubuntu-18-04-dual-boot-windows-10/)
* Install UHD Driver 

  ```
  $ sudo apt-get install libuhd-dev libuhd3.15.0 uhd-host
  ```
  
* Connect USRP B210 to your computer and check if UHD Driver reads it successfully
  
  ```
  $ uhd_usrp_probe
  [INFO] [UHD] linux; GNU C++ version 9.3.0; Boost_107100; UHD_3.15.0.0-release
  [INFO] [B200] Detected Device: B210
  [INFO] [B200] Operating over USB 3.
  [INFO] [B200] Initialize CODEC control...
  [INFO] [B200] Initialize Radio control...
  [INFO] [B200] Performing register loopback test... 
  [INFO] [B200] Register loopback test passed
  [INFO] [B200] Performing register loopback test... 
  [INFO] [B200] Register loopback test passed
  [INFO] [B200] Setting master clock rate selection to 'automatic'.
  [INFO] [B200] Asking for clock rate 16.000000 MHz... 
  [INFO] [B200] Actually got clock rate 16.000000 MHz.
    _____________________________________________________
   /
  |       Device: B-Series Device
  |     _____________________________________________________
  |    /
  |   |       Mboard: B210
  |   |   serial: 3218CC8
  |   |   name: MyB210
  |   |   product: 2
  |   |   revision: 4
  |   |   FW Version: 8.0
  |   |   FPGA Version: 16.0
  |   |   
  |   |   Time sources:  none, internal, external, gpsdo
  |   |   Clock sources: internal, external, gpsdo
  |   |   Sensors: ref_locked
  |   |     _____________________________________________________
  |   |    /
  |   |   |       RX DSP: 0
  |   |   |   
  |   |   |   Freq range: -8.000 to 8.000 MHz
  |   |     _____________________________________________________
  |   |    /
  |   |   |       RX DSP: 1
  |   |   |   
  |   |   |   Freq range: -8.000 to 8.000 MHz
  |   |     _____________________________________________________
  |   |    /
  |   |   |       RX Dboard: A
  |   |   |     _____________________________________________________
  |   |   |    /
  |   |   |   |       RX Frontend: A
  |   |   |   |   Name: FE-RX2
  |   |   |   |   Antennas: TX/RX, RX2
  |   |   |   |   Sensors: temp, rssi, lo_locked
  |   |   |   |   Freq range: 50.000 to 6000.000 MHz
  |   |   |   |   Gain range PGA: 0.0 to 76.0 step 1.0 dB
  |   |   |   |   Bandwidth range: 200000.0 to 56000000.0 step 0.0 Hz
  |   |   |   |   Connection Type: IQ
  |   |   |   |   Uses LO offset: No
  |   |   |     _____________________________________________________
  |   |   |    /
  |   |   |   |       RX Frontend: B
  |   |   |   |   Name: FE-RX1
  |   |   |   |   Antennas: TX/RX, RX2
  |   |   |   |   Sensors: temp, rssi, lo_locked
  |   |   |   |   Freq range: 50.000 to 6000.000 MHz
  |   |   |   |   Gain range PGA: 0.0 to 76.0 step 1.0 dB
  |   |   |   |   Bandwidth range: 200000.0 to 56000000.0 step 0.0 Hz
  |   |   |   |   Connection Type: IQ
  |   |   |   |   Uses LO offset: No
  |   |   |     _____________________________________________________
  |   |   |    /
  |   |   |   |       RX Codec: A
  |   |   |   |   Name: B210 RX dual ADC
  |   |   |   |   Gain Elements: None
  |   |     _____________________________________________________
  |   |    /
  |   |   |       TX DSP: 0
  |   |   |   
  |   |   |   Freq range: -8.000 to 8.000 MHz
  |   |     _____________________________________________________
  |   |    /
  |   |   |       TX DSP: 1
  |   |   |   
  |   |   |   Freq range: -8.000 to 8.000 MHz
  |   |     _____________________________________________________
  |   |    /
  |   |   |       TX Dboard: A
  |   |   |     _____________________________________________________
  |   |   |    /
  |   |   |   |       TX Frontend: A
  |   |   |   |   Name: FE-TX2
  |   |   |   |   Antennas: TX/RX
  |   |   |   |   Sensors: temp, lo_locked
  |   |   |   |   Freq range: 50.000 to 6000.000 MHz
  |   |   |   |   Gain range PGA: 0.0 to 89.8 step 0.2 dB
  |   |   |   |   Bandwidth range: 200000.0 to 56000000.0 step 0.0 Hz
  |   |   |   |   Connection Type: IQ
  |   |   |   |   Uses LO offset: No
  |   |   |     _____________________________________________________
  |   |   |    /
  |   |   |   |       TX Frontend: B
  |   |   |   |   Name: FE-TX1
  |   |   |   |   Antennas: TX/RX
  |   |   |   |   Sensors: temp, lo_locked
  |   |   |   |   Freq range: 50.000 to 6000.000 MHz
  |   |   |   |   Gain range PGA: 0.0 to 89.8 step 0.2 dB
  |   |   |   |   Bandwidth range: 200000.0 to 56000000.0 step 0.0 Hz
  |   |   |   |   Connection Type: IQ
  |   |   |   |   Uses LO offset: No
  |   |   |     _____________________________________________________
  |   |   |    /
  |   |   |   |       TX Codec: A
  |   |   |   |   Name: B210 TX dual DAC
  |   |   |   |   Gain Elements: None


  $ uhd_find_devices
  [INFO] [UHD] linux; GNU C++ version 9.3.0; Boost_107100; UHD_3.15.0.0-release
  --------------------------------------------------
  -- UHD Device 0
  --------------------------------------------------
  Device Address:
      serial: 3218CC8
      name: MyB210
      product: B210
      type: b200
  ```
  
* Commands to install srsRAN (**USRP should stay connected to PC! If not, the UE and eNB applications will not be build by default.**) 
     
  Get mandatory libraries:
  ```
  $ sudo apt-get install cmake libfftw3-dev libmbedtls-dev libboost-program-options-dev libconfig++-dev libsctp-dev
  ```
  Download and build srsRAN:
  ```
  $ sudo su
  $ cd ~
  $ git clone https://github.com/srsran/srsRAN.git
  $ cd srsRAN
  $ mkdir build
  $ cd build
  $ cmake ../
  $ make
  $ make test
  ```
  
  After ```$ make``` command, if you run into an error like this...
  
  ```
  cc1plus: error: bad value (‘tigerlake’) for ‘-march=’ switch

  cc1plus: note: valid arguments to ‘-march=’ switch are: nocona core2 nehalem corei7 westmere sandybridge corei7-avx ivybridge core-avx-i haswell core-avx2    broadwell skylake skylake-avx512 cannonlake icelake-client icelake-server cascadelake bonnell atom silvermont slm goldmont goldmont-plus tremont knl knm x86-64 eden-x2 nano nano-1000 nano-2000 nano-3000 nano-x2 eden-x4 nano-x4 k8 k8-sse3 opteron opteron-sse3 athlon64 athlon64-sse3 athlon-fx amdfam10 barcelona bdver1 bdver2 bdver3 bdver4 znver1 znver2 btver1 btver2 native

  cc1plus: error: bad value (‘tigerlake’) for ‘-mtune=’ switch

  cc1plus: note: valid arguments to ‘-mtune=’ switch are: nocona core2 nehalem corei7 westmere sandybridge corei7-avx ivybridge core-avx-i haswell core-avx2 broadwell skylake skylake-avx512 cannonlake icelake-client icelake-server cascadelake bonnell atom silvermont slm goldmont goldmont-plus tremont knl knm intel x86-64 eden-x2 nano nano-1000 nano-2000 nano-3000 nano-x2 eden-x4 nano-x4 k8 k8-sse3 opteron opteron-sse3 athlon64 athlon64-sse3 athlon-fx amdfam10 barcelona bdver1 bdver2 bdver3 bdver4 znver1 znver2 btver1 btver2 generic native

  make[2]: *** [makefileCommon/compile.core.mk:240: /home/josue/Documents/of_v0.11.0_linux64gcc6_release/libs/openFrameworksCompiled/lib/linux64/obj/Release/libs/openFrameworks/events/ofEvents.o] Error 1

  make[1]: *** [makefileCommon/compile.core.mk:204: Release] Error 2

  make[1]: Leaving directory '/home/josue/Documents/of_v0.11.0_linux64gcc6_release/libs/openFrameworksCompiled/project'
  make: *** [/home/josue/Documents/of_v0.11.0_linux64gcc6_release/libs/openFrameworksCompiled/project/makefileCommon/compile.project.mk:125: Release] Error 2
  ```
  
  If so, run the commands below to update gcc & g++ to a higher version (10) as a default:
  
  ```
  $ sudo apt install gcc-10 g++-10
  $ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-10 100 --slave /usr/bin/g++ g++ /usr/bin/g++-10 --slave /usr/bin/gcov gcov /usr/bin/gcov-10
  $ sudo update-alternatives --config gcc
  ```
  *** credit goes to [this stackoverflow post](https://stackoverflow.com/questions/64493692/cc1plus-error-bad-value-tigerlake-for-march-switch-compilation-error). 
  
  Install srsRAN:
  
  ```
  $ make install
  $ ldconfig
  $ srsran_install_configs.sh user
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

![add apn](/images/add_apn.png)
![apn settings](/images/apn_settings.png)

The only thing I had to change was the name and APN of my new APN point. I wrote both as "srsapn" so that I did not have to change the EPC configuration file. Make sure if the name and APN of the APN in your test phone is same as that in epc.conf (line 30).

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



Phew...At last, we are over, but only for this experiment. We are only one fifth way through. However, be proud of yourself. Even if you are going to quit here, that's already a great work you've achieved there. For others who are continuing, way to go!!


## Passive Attack w/ pdsch_ue.c in srsRAN

****From here, commerical USIMs were used, not the programmable ones I used in the previous subexperiment, "3. Create a private LTE network for COTS UE with programmable USIM ". Thus, I am assuming that I have no control over the victim UE. ****

### Objectives
Obtain GUTI/TMSI of victim's UE and gain crucial knowledge about parameters (EARFCN, TAC, MNC & MCC) for my rogue eNodeB to successfully mimick an operational network.

### Method
For a passive attack, I utilized the Service Mode and pdsch_ue.c in srsRAN. Along with them, other tools were also used for various purposes such as decoding ASN.

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

For a rogue eNodeB, I will be using the same TAC and PLMN (MMC + MNC) discovered in this step. However, in order to trigger cell reselection only on the target UE, I need to calculate an optimal EARFCN and figure out GUTI/TMSI of it by sniffing SIB messages.

#### b. Sniff SIB messages

- Calculate an optimal EARFCN

-------------------------------------------------------------------------- Theoretical Approach -----------------------------------------------------------------------------

As stated in [Evolved Universal Terrestrial Radio Access (E-UTRA); User Equipment (UE) procedures in idle mode (Release 16.3.0)](https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=2432) shown below,

![cell reselection criteria](/images/cell_reselection_criteria.png)

given that a minimum power threshold ```Srxlev > ThreshX-HighP``` is met, a cell of higher priority frequency will trigger a cell reselection from UEs. Thus, to trigger it, I first need to figure out the minimum RX power and frequencies on a higher priority than that of a serving cell.

Before explaining further, here are some terminologies you would wonder from the image above. Referenced from [Huawei Forum thread](https://forum.huawei.com/enterprise/en/q2-how-the-multi-carrier-cell-re-selection-happened-in-lte/thread/540339-100305), [Handbook_LTE_Cell_Reselection](http://sharetechnote.com/html/Handbook_LTE_Cell_Reselection.html) and [How LTE Stuff Works?](http://howltestuffworks.blogspot.com/2019/10/5g-nr-sib5.html).

| Term | Explanation | where it can be found | default value | actual value |
--- | --- | --- | --- | ---
ThreshX-High/ ThreshX-HighP | The threshold of a target cell **Srxlev** to perfrom reselection from a low priority to a high priority cell. (i.e, a large ThreshXHigh value makes reselection harder) | SIB 5 | - | field value * 2 (dB)
threshServingHighQ | The threshold of a serving cell **Squal** to perfrom reselection from a low priority to a high priority cell. (i.e, a large ThreshXHigh value makes reselection harder) | SIB 3 | - | field value * 2 (dB)
q-RxLevMin | minimum RX level in dBm mandatory in a cell that is operating as EUTRA-FA | SIB 1 & 3 | -128 dBm ( IE value in SIB 1 is -64 ) | field value * 2 (dBm)

* Srxlev: Cell selection RX level value (in dB) measured by UE; default value = Qrxlevmeas-Qrxlevmin (-128dBm).
* Squal: Cell selection quality value (in dB); default value = Qqualmeas - qQualMin. 
* Qrxlevemeas: RSCP level measured by UE.
* Qqualmeas: EcNo level measured by UE.
* qQualMin: a value specified in SIB.
* EUTRAN: LTE architecture
* UTRAN: UMTS architecture

First, I was able to figure out how to calculate a minimum RX power for my eNodeB from this [Huawei Forum thread](https://forum.huawei.com/enterprise/en/q2-how-the-multi-carrier-cell-re-selection-happened-in-lte/thread/540339-100305).

![huawei thread](/images/huawei_thread.png)

As shown above, unlike the official document from 3GPP, I need to add q-RxLevMin with ThreshX-High to get RSRP. (Update Required: Soon to be revealed which is right after conducting an actual experiment) 

Second, the High priority frequencies (0: Lowest - 7: Highest) were obtainable from SIB Type 5 messages (INTER). 

--------------------------------------------------------------------------- Practical Approach ------------------------------------------------------------------------------

As a practice, I started off by running pdsch_ue.c with the following command to get MIB & SIB message Type 1 from PDSCH on frequency 806MHz. My main guide for this step was Appendix E. Decoding Paging Messages from [*Location Disclosure in LTE Networks by using IMSI Catcher*](/papers/Location Disclosure in LTE Networks by using IMSI Catcher.pdf) by Christian Sørseth.

```
$ ~/srsRAN/lib/examples/pdsch_ue –r fffe –f 806000000
```

However, this will not output the messages. Therefore, I needed to add the function *srslte_vec_fprint_byte()* after line 828 to print the hexstring to stdout. pdsch_ue.c should look like so:

```
828 |             if (n > 0) {
829 |               srslte_vec_fprint_byte(stdout, data, n/8); OR srslte_vec_fprint_byte(stdout, data[0], n/8); (Update required)
830 |               /* Send data if socket active */
```
In *srslte_vec_fprint_byte()*, the argument, *n*, is an integer indicating the data packet (if the value is greater than 1, a data packet is found.), and the other argument, *data*, is a poitner containing the paging packet.

Now, I can see the hex string as below:

![pdsch_ue result](/images/pdsch_ue_result.png)

Since I did not have enough brain cells to interpret what this means, I copy-pasted the output to a [ASN decoder](http://www.marben-products.com/asn.1/services/decoder-asn1-lte.html).

I finally got MIB & SIB Type 1 messages in a readable XML format as below.

![mibsib1](/images/mibsib1.png)

That was a decent warmup. Now, the real deal: SIB 3 & 5.

To achieve this, I modified the code in pdsch_ue.c to make it decode further SIB messages other than MIB and SIB Type 1.

This part actually took me quite a while to figure out how it works, and, basically, I had to 1) calculate which SFN () and subframe each SIB message gets broadcasted from SIB Type 1 message 2) modify the code to decode PDSCH at the exact time I previously calculated. For more conceptual information, check out [Handbook_LTE_SFN](https://www.sharetechnote.com/html/Handbook_LTE_SFN.html#:~:text=In%20LTE%2C%20the%20arm%20ticking,number%20is%20called%20subframe%20number.&text=When%20SFN%20number%20hits%20the,it%20goes%20back%20to%200.)and [BasicProcedure_LTE_SIB_Scheduling](https://www.sharetechnote.com/html/BasicProcedure_LTE_SIB_Scheduling.html). 

![SIB1](/images/SIB1.png]

From the captured SIB Type 1 message above, I was able to detect periodicities of each SIB message, which I organized into a table below.

| Periodicity | rf8 | rf16 | rf32 | rf64 |
--- | --- | --- | --- | ---
sibType# | ?? | ?? | ?? | ??

Also, on the bottom, I saw what the si-WindowLength value (??ms) is.

With those two, I had to modify line 764 of srsRAN/lib/examples/pdsch_ue.c.
```
763 |      /* We are looking for SIB1 Blocks, search only in appropiate places */
764 |       if ((sf_idx == 5 && (sfn % 2) == 0) || mch_table[sf_idx] == 1) {
765 |         decode_pdsch = true;
766 |        } else {
767 |          decode_pdsch = false;
768 |        }
```
Based on the algorithm described in the second link, along with the help of [this forum post from srslte-users](http://www.softwareradiosystems.com/pipermail/srslte-users/2021-April/004198.html), the result looks like the following.
```
763 |      /* We are looking for SIB1 Blocks, search only in appropiate places */
764 |       si_periodicity[4] = {16, 32, 64, 64}; 
765 |       int n, x, a, sfn_e;
766 |       n = ?;
767 |       x = (n - 1) * winlen;
768 |       a = x % 10;
769 |       sfn_e = floor(x/10);
770 |       if ((sf_idx == a) && ((sfn % si_periodicity[n]) == sfn_e)) {
771 |         decode_pdsch = true;
772 |        } else {
773 |          decode_pdsch = false;
774 |        }
```
As a result, I received the output as shown below in an order of SIB3, SIB5, SIB6, and SIB7.
![SIB3](/images/SIB3.png]
![SIB5](/images/SIB5.png]
![SIB6](/images/SIB6.png]
![SIB7](/images/SIB7.png]

After copy-pasting it to a [ASN decoder](http://www.marben-products.com/asn.1/services/decoder-asn1-lte.html), I could see it in a read-friendly XML format.

![SIB3 XML](/images/SIB3_XML.png]
![SIB5 XML](/images/SIB5_XML.png]
![SIB6 XML](/images/SIB6_XML.png]
![SIB7 XML](/images/SIB7_XML.png]

Awesome! Now, 
1) Find a EARFCN that has a higher priority than that of UE's current serving cell.
   dl_earfcn (downlinnk EARFCN) = ??
2) Calculate the minimum power (dB) needed from q-RxLevMin and threshX-High in SIB Type 5 message.

3) Reflect the changes in srsRAN/srsenb/enb.conf and srsRAN/srsenb/rr.conf.

srsRAN/srsenb/enb.conf
```
63 | [rf]
64 | dl_earfcn = ??
65 | tx_gain = ??
```
&
srsRAN/srsenb/rr.conf
```
56 |    // rf_port = 0;
57 |    cell_id = 0x01;
58 |    tac = 0x0007;
59 |    pci = 1;
60 |    // root_seq_idx = 204;
61 |    dl_earfcn = ??;
```

All things covered, now it is time to run my rogue eNodeB!

```
$ sudo srsenb
```

After a few second, we see that the victim UE is successfully connected to my private network both from the output of srsenb and Service Mode screen of UE.

![srsenb caught UE](/images/srsenb_caught_UE.png)
![Service Mode after connection](/images/service_mode_after_conn.png]

## Semi-Passive Attack: IMSI Catcher

## Active Attack (1): Location Leak
## Active Attack (2): Denial-of-Service (DoS)
## Active Attack (3): Eavesdrop VoLTE calls

## Similar Guides
[LimeSDR + SoapySDR + srsLTE](https://gist.github.com/JamesHagerman/fafec6ee2ee076fe7cda4cf4dd74edd0)
[IMSI Catcher repo](# **Detailed Procedures**

## 1. PC Setup: Ubuntu 20.04 (Dual Boot), UHD Driver & srsRAN on core i7 PC

* [Ubuntu 20.04 (Dual Boot) Installation](https://linoxide.com/install-ubuntu-18-04-dual-boot-windows-10/)
* Install UHD Driver 

  ```
  $ sudo apt-get install libuhd-dev libuhd3.15.0 uhd-host
  ```
  
* Connect USRP B210 to your computer and check if UHD Driver reads it successfully
  
  ```
  $ uhd_usrp_probe
  [INFO] [UHD] linux; GNU C++ version 9.3.0; Boost_107100; UHD_3.15.0.0-release
  [INFO] [B200] Detected Device: B210
  [INFO] [B200] Operating over USB 3.
  [INFO] [B200] Initialize CODEC control...
  [INFO] [B200] Initialize Radio control...
  [INFO] [B200] Performing register loopback test... 
  [INFO] [B200] Register loopback test passed
  [INFO] [B200] Performing register loopback test... 
  [INFO] [B200] Register loopback test passed
  [INFO] [B200] Setting master clock rate selection to 'automatic'.
  [INFO] [B200] Asking for clock rate 16.000000 MHz... 
  [INFO] [B200] Actually got clock rate 16.000000 MHz.
    _____________________________________________________
   /
  |       Device: B-Series Device
  |     _____________________________________________________
  |    /
  |   |       Mboard: B210
  |   |   serial: 3218CC8
  |   |   name: MyB210
  |   |   product: 2
  |   |   revision: 4
  |   |   FW Version: 8.0
  |   |   FPGA Version: 16.0
  |   |   
  |   |   Time sources:  none, internal, external, gpsdo
  |   |   Clock sources: internal, external, gpsdo
  |   |   Sensors: ref_locked
  |   |     _____________________________________________________
  |   |    /
  |   |   |       RX DSP: 0
  |   |   |   
  |   |   |   Freq range: -8.000 to 8.000 MHz
  |   |     _____________________________________________________
  |   |    /
  |   |   |       RX DSP: 1
  |   |   |   
  |   |   |   Freq range: -8.000 to 8.000 MHz
  |   |     _____________________________________________________
  |   |    /
  |   |   |       RX Dboard: A
  |   |   |     _____________________________________________________
  |   |   |    /
  |   |   |   |       RX Frontend: A
  |   |   |   |   Name: FE-RX2
  |   |   |   |   Antennas: TX/RX, RX2
  |   |   |   |   Sensors: temp, rssi, lo_locked
  |   |   |   |   Freq range: 50.000 to 6000.000 MHz
  |   |   |   |   Gain range PGA: 0.0 to 76.0 step 1.0 dB
  |   |   |   |   Bandwidth range: 200000.0 to 56000000.0 step 0.0 Hz
  |   |   |   |   Connection Type: IQ
  |   |   |   |   Uses LO offset: No
  |   |   |     _____________________________________________________
  |   |   |    /
  |   |   |   |       RX Frontend: B
  |   |   |   |   Name: FE-RX1
  |   |   |   |   Antennas: TX/RX, RX2
  |   |   |   |   Sensors: temp, rssi, lo_locked
  |   |   |   |   Freq range: 50.000 to 6000.000 MHz
  |   |   |   |   Gain range PGA: 0.0 to 76.0 step 1.0 dB
  |   |   |   |   Bandwidth range: 200000.0 to 56000000.0 step 0.0 Hz
  |   |   |   |   Connection Type: IQ
  |   |   |   |   Uses LO offset: No
  |   |   |     _____________________________________________________
  |   |   |    /
  |   |   |   |       RX Codec: A
  |   |   |   |   Name: B210 RX dual ADC
  |   |   |   |   Gain Elements: None
  |   |     _____________________________________________________
  |   |    /
  |   |   |       TX DSP: 0
  |   |   |   
  |   |   |   Freq range: -8.000 to 8.000 MHz
  |   |     _____________________________________________________
  |   |    /
  |   |   |       TX DSP: 1
  |   |   |   
  |   |   |   Freq range: -8.000 to 8.000 MHz
  |   |     _____________________________________________________
  |   |    /
  |   |   |       TX Dboard: A
  |   |   |     _____________________________________________________
  |   |   |    /
  |   |   |   |       TX Frontend: A
  |   |   |   |   Name: FE-TX2
  |   |   |   |   Antennas: TX/RX
  |   |   |   |   Sensors: temp, lo_locked
  |   |   |   |   Freq range: 50.000 to 6000.000 MHz
  |   |   |   |   Gain range PGA: 0.0 to 89.8 step 0.2 dB
  |   |   |   |   Bandwidth range: 200000.0 to 56000000.0 step 0.0 Hz
  |   |   |   |   Connection Type: IQ
  |   |   |   |   Uses LO offset: No
  |   |   |     _____________________________________________________
  |   |   |    /
  |   |   |   |       TX Frontend: B
  |   |   |   |   Name: FE-TX1
  |   |   |   |   Antennas: TX/RX
  |   |   |   |   Sensors: temp, lo_locked
  |   |   |   |   Freq range: 50.000 to 6000.000 MHz
  |   |   |   |   Gain range PGA: 0.0 to 89.8 step 0.2 dB
  |   |   |   |   Bandwidth range: 200000.0 to 56000000.0 step 0.0 Hz
  |   |   |   |   Connection Type: IQ
  |   |   |   |   Uses LO offset: No
  |   |   |     _____________________________________________________
  |   |   |    /
  |   |   |   |       TX Codec: A
  |   |   |   |   Name: B210 TX dual DAC
  |   |   |   |   Gain Elements: None


  $ uhd_find_devices
  [INFO] [UHD] linux; GNU C++ version 9.3.0; Boost_107100; UHD_3.15.0.0-release
  --------------------------------------------------
  -- UHD Device 0
  --------------------------------------------------
  Device Address:
      serial: 3218CC8
      name: MyB210
      product: B210
      type: b200
  ```
  
* Commands to install srsRAN (**USRP should stay connected to PC! If not, the UE and eNB applications will not be build by default.**) 
     
  Mandatory libraries:
  ```
  $ sudo apt-get install cmake libfftw3-dev libmbedtls-dev libboost-program-options-dev libconfig++-dev libsctp-dev
  ```
  Build srsRAN:
  ```
  $ cd ~
  $ git clone https://github.com/srsran/srsRAN.git
  $ cd srsRAN
  $ mkdir build
  $ cd build
  $ cmake ../
  $ make
  $ make test
  ```
  
  In doing so, if you run into an error like this...
  
  ```
  cc1plus: error: bad value (‘tigerlake’) for ‘-march=’ switch

  cc1plus: note: valid arguments to ‘-march=’ switch are: nocona core2 nehalem corei7 westmere sandybridge corei7-avx ivybridge core-avx-i haswell core-avx2    broadwell skylake skylake-avx512 cannonlake icelake-client icelake-server cascadelake bonnell atom silvermont slm goldmont goldmont-plus tremont knl knm x86-64 eden-x2 nano nano-1000 nano-2000 nano-3000 nano-x2 eden-x4 nano-x4 k8 k8-sse3 opteron opteron-sse3 athlon64 athlon64-sse3 athlon-fx amdfam10 barcelona bdver1 bdver2 bdver3 bdver4 znver1 znver2 btver1 btver2 native

  cc1plus: error: bad value (‘tigerlake’) for ‘-mtune=’ switch

  cc1plus: note: valid arguments to ‘-mtune=’ switch are: nocona core2 nehalem corei7 westmere sandybridge corei7-avx ivybridge core-avx-i haswell core-avx2 broadwell skylake skylake-avx512 cannonlake icelake-client icelake-server cascadelake bonnell atom silvermont slm goldmont goldmont-plus tremont knl knm intel x86-64 eden-x2 nano nano-1000 nano-2000 nano-3000 nano-x2 eden-x4 nano-x4 k8 k8-sse3 opteron opteron-sse3 athlon64 athlon64-sse3 athlon-fx amdfam10 barcelona bdver1 bdver2 bdver3 bdver4 znver1 znver2 btver1 btver2 generic native

  make[2]: *** [makefileCommon/compile.core.mk:240: /home/josue/Documents/of_v0.11.0_linux64gcc6_release/libs/openFrameworksCompiled/lib/linux64/obj/Release/libs/openFrameworks/events/ofEvents.o] Error 1

  make[1]: *** [makefileCommon/compile.core.mk:204: Release] Error 2

  make[1]: Leaving directory '/home/josue/Documents/of_v0.11.0_linux64gcc6_release/libs/openFrameworksCompiled/project'
  make: *** [/home/josue/Documents/of_v0.11.0_linux64gcc6_release/libs/openFrameworksCompiled/project/makefileCommon/compile.project.mk:125: Release] Error 2
  ```
  
  If so, run the commands below to update gcc & g++ to a higher version (10) as a default:
  
  ```
  $ sudo apt install gcc-10 g++-10
  $ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-10 100 --slave /usr/bin/g++ g++ /usr/bin/g++-10 --slave /usr/bin/gcov gcov /usr/bin/gcov-10
  $ sudo update-alternatives --config gcc
  ```
  *** credit goes to [this stackoverflow post](https://stackoverflow.com/questions/64493692/cc1plus-error-bad-value-tigerlake-for-march-switch-compilation-error). 
  
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

![add apn](/images/add_apn.png)
![apn settings](/images/apn_settings.png)

The only thing I had to change was the name and APN of my new APN point. I wrote both as "srsapn" so that I did not have to change the EPC configuration file. Make sure if the name and APN of the APN in your test phone is same as that in epc.conf (line 30).

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



Phew...At last, we are over, but only for this experiment. We are only one fifth way through. However, be proud of yourself. Even if you are going to quit here, that's already a great work you've achieved there. For others who are continuing, way to go!!


## Passive Attack w/ pdsch_ue.c in srsRAN

****From here, commerical USIMs were used, not the programmable ones I used in the previous subexperiment, "3. Create a private LTE network for COTS UE with programmable USIM ". Thus, I am assuming that I have no control over the victim UE. ****

### Objectives
Obtain GUTI/TMSI of victim's UE and gain crucial knowledge about parameters (EARFCN, TAC, MNC & MCC) for my rogue eNodeB to successfully mimick an operational network.

### Method
For a passive attack, I utilized the Service Mode and pdsch_ue.c in srsRAN. Along with them, other tools were also used for various purposes such as decoding ASN.

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

For a rogue eNodeB, I will be using the same TAC and PLMN (MMC + MNC) discovered in this step. However, in order to trigger cell reselection only on the target UE, I need to calculate an optimal EARFCN and figure out GUTI/TMSI of it by sniffing SIB messages.

#### b. Sniff SIB messages

- Calculate an optimal EARFCN

-------------------------------------------------------------------------- Theoretical Approach -----------------------------------------------------------------------------

As stated in [Evolved Universal Terrestrial Radio Access (E-UTRA); User Equipment (UE) procedures in idle mode (Release 16.3.0)](https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=2432) shown below,

![cell reselection criteria](/images/cell_reselection_criteria.png)

given that a minimum power threshold ```Srxlev > ThreshX-HighP``` is met, a cell of higher priority frequency will trigger a cell reselection from UEs. Thus, to trigger it, I first need to figure out the minimum RX power and frequencies on a higher priority than that of a serving cell.

Before explaining further, here are some terminologies you would wonder from the image above. Referenced from [Huawei Forum thread](https://forum.huawei.com/enterprise/en/q2-how-the-multi-carrier-cell-re-selection-happened-in-lte/thread/540339-100305), [Handbook_LTE_Cell_Reselection](http://sharetechnote.com/html/Handbook_LTE_Cell_Reselection.html) and [How LTE Stuff Works?](http://howltestuffworks.blogspot.com/2019/10/5g-nr-sib5.html).

| Term | Explanation | where it can be found | default value | actual value |
--- | --- | --- | --- | ---
ThreshX-High/ ThreshX-HighP | The threshold of a target cell **Srxlev** to perfrom reselection from a low priority to a high priority cell. (i.e, a large ThreshXHigh value makes reselection harder) | SIB 5 | - | field value * 2 (dB)
threshServingHighQ | The threshold of a serving cell **Squal** to perfrom reselection from a low priority to a high priority cell. (i.e, a large ThreshXHigh value makes reselection harder) | SIB 3 | - | field value * 2 (dB)
q-RxLevMin | minimum RX level in dBm mandatory in a cell that is operating as EUTRA-FA | SIB 1 & 3 | -128 dBm ( IE value in SIB 1 is -64 ) | field value * 2 (dBm)

* Srxlev: Cell selection RX level value (in dB) measured by UE; default value = Qrxlevmeas-Qrxlevmin (-128dBm).
* Squal: Cell selection quality value (in dB); default value = Qqualmeas - qQualMin. 
* Qrxlevemeas: RSCP level measured by UE.
* Qqualmeas: EcNo level measured by UE.
* qQualMin: a value specified in SIB.
* EUTRAN: LTE architecture
* UTRAN: UMTS architecture

First, I was able to figure out how to calculate a minimum RX power for my eNodeB from this [Huawei Forum thread](https://forum.huawei.com/enterprise/en/q2-how-the-multi-carrier-cell-re-selection-happened-in-lte/thread/540339-100305).

![huawei thread](/images/huawei_thread.png)

As shown above, unlike the official document from 3GPP, I need to add q-RxLevMin with ThreshX-High to get RSRP. (Update Required: Soon to be revealed which is right after conducting an actual experiment) 

Second, the High priority frequencies (0: Lowest - 7: Highest) were obtainable from SIB Type 5 messages (INTER). 

--------------------------------------------------------------------------- Practical Approach ------------------------------------------------------------------------------

As a practice, I started off by running pdsch_ue.c with the following command to get MIB & SIB message Type 1 from PDSCH on frequency 806MHz. My main guide for this step was Appendix E. Decoding Paging Messages from [*Location Disclosure in LTE Networks by using IMSI Catcher*](/papers/Location Disclosure in LTE Networks by using IMSI Catcher.pdf) by Christian Sørseth.

```
$ ~/srsRAN/lib/examples/pdsch_ue –r fffe –f 806000000
```

However, this will not output the messages. Therefore, I needed to add the function *srslte_vec_fprint_byte()* after line 828 to print the hexstring to stdout. pdsch_ue.c should look like so:

```
828 |             if (n > 0) {
829 |               srslte_vec_fprint_byte(stdout, data, n/8); OR srslte_vec_fprint_byte(stdout, data[0], n/8); (Update required)
830 |               /* Send data if socket active */
```
In *srslte_vec_fprint_byte()*, the argument, *n*, is an integer indicating the data packet (if the value is greater than 1, a data packet is found.), and the other argument, *data*, is a poitner containing the paging packet.

Now, I can see the hex string as below:

![pdsch_ue result](/images/pdsch_ue_result.png)

Since I did not have enough brain cells to interpret what this means, I copy-pasted the output to a [ASN decoder](http://www.marben-products.com/asn.1/services/decoder-asn1-lte.html).

I finally got MIB & SIB Type 1 messages in a readable XML format as below.

![mibsib1](/images/mibsib1.png)

That was a decent warmup. Now, the real deal: SIB 3 & 5.

To achieve this, I modified the code in pdsch_ue.c to make it decode further SIB messages other than MIB and SIB Type 1.

This part actually took me quite a while to figure out how it works, and, basically, I had to 1) calculate which SFN () and subframe each SIB message gets broadcasted from SIB Type 1 message 2) modify the code to decode PDSCH at the exact time I previously calculated. For more conceptual information, check out [Handbook_LTE_SFN](https://www.sharetechnote.com/html/Handbook_LTE_SFN.html#:~:text=In%20LTE%2C%20the%20arm%20ticking,number%20is%20called%20subframe%20number.&text=When%20SFN%20number%20hits%20the,it%20goes%20back%20to%200.)and [BasicProcedure_LTE_SIB_Scheduling](https://www.sharetechnote.com/html/BasicProcedure_LTE_SIB_Scheduling.html). 

![SIB1](/images/SIB1.png]

From the captured SIB Type 1 message above, I was able to detect periodicities of each SIB message, which I organized into a table below.

| Periodicity | rf8 | rf16 | rf32 | rf64 |
--- | --- | --- | --- | ---
sibType# | ?? | ?? | ?? | ??

Also, on the bottom, I saw what the si-WindowLength value (??ms) is.

With those two, I had to modify line 764 of srsRAN/lib/examples/pdsch_ue.c.
```
763 |      /* We are looking for SIB1 Blocks, search only in appropiate places */
764 |       if ((sf_idx == 5 && (sfn % 2) == 0) || mch_table[sf_idx] == 1) {
765 |         decode_pdsch = true;
766 |        } else {
767 |          decode_pdsch = false;
768 |        }
```
Based on the algorithm described in the second link, along with the help of [this forum post from srslte-users](http://www.softwareradiosystems.com/pipermail/srslte-users/2021-April/004198.html), the result looks like the following.
```
763 |      /* We are looking for SIB1 Blocks, search only in appropiate places */
764 |       si_periodicity[4] = {16, 32, 64, 64}; 
765 |       int n, x, a, sfn_e;
766 |       n = ?;
767 |       x = (n - 1) * winlen;
768 |       a = x % 10;
769 |       sfn_e = floor(x/10);
770 |       if ((sf_idx == a) && ((sfn % si_periodicity[n]) == sfn_e)) {
771 |         decode_pdsch = true;
772 |        } else {
773 |          decode_pdsch = false;
774 |        }
```
As a result, I received the output as shown below in an order of SIB3, SIB5, SIB6, and SIB7.
![SIB3](/images/SIB3.png]
![SIB5](/images/SIB5.png]
![SIB6](/images/SIB6.png]
![SIB7](/images/SIB7.png]

After copy-pasting it to a [ASN decoder](http://www.marben-products.com/asn.1/services/decoder-asn1-lte.html), I could see it in a read-friendly XML format.

![SIB3 XML](/images/SIB3_XML.png]
![SIB5 XML](/images/SIB5_XML.png]
![SIB6 XML](/images/SIB6_XML.png]
![SIB7 XML](/images/SIB7_XML.png]

Awesome! Now, 
1) Find a EARFCN that has a higher priority than that of UE's current serving cell.
   dl_earfcn (downlinnk EARFCN) = ??
2) Calculate the minimum power (dB) needed from q-RxLevMin and threshX-High in SIB Type 5 message.

3) Reflect the changes in srsRAN/srsenb/enb.conf and srsRAN/srsenb/rr.conf.

srsRAN/srsenb/enb.conf
```
63 | [rf]
64 | dl_earfcn = ??
65 | tx_gain = ??
```
&
srsRAN/srsenb/rr.conf
```
56 |    // rf_port = 0;
57 |    cell_id = 0x01;
58 |    tac = 0x0007;
59 |    pci = 1;
60 |    // root_seq_idx = 204;
61 |    dl_earfcn = ??;
```

All things covered, now it is time to run my rogue eNodeB!

```
$ sudo srsenb
```

After a few second, we see that the victim UE is successfully connected to my private network both from the output of srsenb and Service Mode screen of UE.

![srsenb caught UE](/images/srsenb_caught_UE.png)
![Service Mode after connection](/images/service_mode_after_conn.png]

## Semi-Passive Attack: IMSI Catcher

## Active Attack (1): Location Leak
## Active Attack (2): Denial-of-Service (DoS)
## Active Attack (3): Eavesdrop VoLTE calls

## Similar Guides
[LimeSDR + SoapySDR + srsLTE](https://gist.github.com/JamesHagerman/fafec6ee2ee076fe7cda4cf4dd74edd0)
[IMSI catcher repo](https://github.com/Wooniety/srsLTE-Sniffer)






