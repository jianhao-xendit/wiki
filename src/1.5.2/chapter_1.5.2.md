#   Chapter 1.5.2 - Zeek

>This chapter explains how to install the Zeek on your `Kali Linux Machine`

_"Zeek is not an active security device, like a firewall or intrusion prevention system. Rather, Zeek sits on a “sensor,” a hardware, software, virtual, or cloud platform that quietly and unobtrusively observes network traffic. Zeek interprets what it sees and creates compact, high-fidelity transaction logs, file content, and fully customized output, suitable for manual review on disk or in a more analyst-friendly tool like a security and information event management (SIEM) system."_

![Screenshot command](./assets/zeek.png)

***Source: https://zeek.org/get-zeek/***

Installing Zeek
====
***source: https://github.com/crimsoncore/zeek.git***

Login to your `kali linux machine` with SSH and login with your kali username and password 

**Kali Username : thadmin**  
**Kali Password : PROVIDED_PASSWORD**

```code
ssh thadmin@az-kali-lsazure.westeurope.cloudapp.azure.com
``` 

Then we're going to clone the git repository that runs Zeek in a docker container

```code
cd /opt
git clone https://github.com/crimsoncore/zeek.git
```

.
├── config
│   ├── networks.cfg
│   ├── node.cfg
│   └── zeekctl.cfg
├── dc.zeekcrimson.yml
├── logs
│   └── README.md
├── pcap
│   └── README.md
├── README.md
└── site
    └── local.zeek

```code
cd /opt/zeek
docker-compose -f dc.zeekcrimson.yml up -d
```

root@elk:/opt/zeek/logs# tree
.
├── conn.log
├── dns.log
├── files.log
├── ntp.log
├── packet_filter.log
├── README.md
├── ssl.log
├── weird.log
└── x509.log

Edit /usr/local/zeek/etc/node.cfg to set the interface to monitor; usually interface eth0

```code
[zeek]
type=standalone
host=localhost
interface=eth0  
```

Edit /usr/local/zeek/networks.cfg to add the IP addresses and short descriptions of your different routed networks. For example:  

```code
10.0.0.0/8 Private IP space 172.16.0.0/12 Private IP space 192.168.0.0/16 Private IP space  
```

Edit /usr/local/zeek/etc/zeekctl.cfg and set the

CompressLogs = 0
LogRotationInterval = 1