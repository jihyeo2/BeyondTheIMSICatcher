# **Detailed Procedures**

### 1. PC Setup: Ubuntu 20.04 (Dual Boot), UHD Driver & srsRAN on core i7 PC

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
  
  ### 2. Configuration
  
  We can either remove the extension(.example) of an example configuration file or install base configuration files by running the following command in the build folder
  ```
  $ cd ~/srsRAN/build
  $ sudo srsRAN_install_configs.sh service 
  $ cd ./etc/srsran
  ```
  Configuration files are now installed for all users, not just to the user directory.
  
  ### Create a private LTE network with COTS UE chipped with programmable USIM
  
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
  
