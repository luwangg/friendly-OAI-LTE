## Running 1 eNB and 1 UE without S1 Interface

## H.W. Setup:

<p align="center">
  <img src="https://github.com/astro7x/friendly-OAI-LTE/blob/master/img/SISO.png?raw=true"/>
</p>

### Building
#### The following Building Commands should be run on Both Host Machine

```bash
cd ~/openairinterface5g/
source oaienv
cd cmake_targets
./build_oai -w USRP --eNB --UE --noS1 -x
```

### Running eNB

1st: Setup eNB IP
```bash
cd ~/openairinterface5g/
source oaienv
source ./cmake_targets/tools/init_nas_nos1 eNB
```
2nd: Check eNB IP

```bash
ifconfig 
```

```
oai0      Link encap:AMPR NET/ROM  HWaddr C2-C6-7B-07-22-08 
          inet addr:10.0.1.1  Bcast:10.0.1.255  Mask:255.255.255.0
          UP BROADCAST RUNNING NOARP MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:100 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

```

```bash
cd cmake_targets
sudo -E ./lte_noS1_build_oai/build/lte-softmodem-nos1 -d -O $OPENAIR_TARGETS/PROJECTS/GENERIC-LTE-EPC/CONF/enb.band7.tm1.usrpb210.conf 2>&1 | tee ENB.log
```



### Running UE

1st: Setup UE IP
```bash
cd ~/openairinterface5g/
source ./targets/bin/init_nas_nos1 UE
```

2nd: Check UE IP

```bash
ifconfig 
```

```
oai0      Link encap:AMPR NET/ROM  HWaddr 71-7C-CA-24-25-0B
          inet addr:10.0.1.9  Bcast:10.0.1.255  Mask:255.255.255.0
          UP BROADCAST RUNNING NOARP MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:100 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
```

```bash
cd cmake_targets
sudo -E ./lte_noS1_build_oai/build/lte-softmodem-nos1  -U -C2660000000 -r25 --ue-scan-carrier --ue-txgain 90 --ue-rxgain 115 -d >&1 | tee UE.log
```




TO DO:
>  Will Be tested to be on 2 virtual machines based on a physical host

src:
> https://gitlab.eurecom.fr/oai/openairinterface5g/wikis/HowToConnectOAIENBWithOAIUEWithoutS1Interface
