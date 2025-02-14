---
title: "Deutsche Bahn,Docker and Wifi in an ICE and VPN problems"
description: "As a developer with installed Docker you could have a problem connecting to the WiFi in an ICE, the so called WiFi on ICE, because they use the same IP Range."
date: 2019-09-06
---

The Deutsche Bahn is providing free wifi for some years now.
It works for most of the people pretty well, but if you are a developer you could have some issues. I was suffering a lot with this, this is why I'm posting it, to help some of you

As you can see, the WiFi is running on the `IP 172.18.xxx`
![Alt text](/img/wp_wifi_on_ice.png "screenshot of networkmanager, where it's connected to Wifi on ICE")

If you are running *docker* it can happen, that you have in your `ifconfig` a bridge already on this IP:

```shell
ifconig

br-bcc4b1218247: flags=4099<UP,BROADCAST,MULTICAST> mtu 1500
inet 172.18.0.1 netmask 255.255.0.0 broadcast 172.30.255.255
ether 02:42:55:fb:67:74 txqueuelen 0 (Ethernet)
RX packets 0 bytes 0 (0.0 B)
RX errors 0 dropped 0 overruns 0 frame 0
TX packets 0 bytes 0 (0.0 B)
TX errors 0 dropped 0 overruns 0 carrier 0 collisions 0
```

you can delete this bridge by using the following commands:
```shell
sudo ip link set br-bcc4b1218247 down
sudo brctl delbr br-bcc4b1218247
```

This should work, that you can connect then to the WiFi on Ice

UPDATE: 12.09.2023
### Loginpage is not opening
Normally the login page should update automatically, but sometimes (on Linux) it isn't. 
Go to [https://login.wifionice.de](https://login.wifionice.de) for opening it in your browser. 

UPDATE:
you can also delete all docker networks easily with:
```shell
docker network prune
```

I had the problem, that I could not reach some websitse in our company network,when I'm using a VPN.
The ping to our internal subdomain was working, but I could not reach it in my browser:
```shell
ping subdomain.intern.ourdomain.net

PING something.elb.amazonaws.com (10.x.x.x) 56(84) bytes of data.
64 bytes from 10.x.x.x (10.x.x.x): icmp_seq=1 ttl=254 time=108 ms
64 bytes from 10.x.x.x (10.x.x.x): icmp_seq=2 ttl=254 time=192 ms
64 bytes from 10.x.x.x (10.x.x.x): icmp_seq=3 ttl=254 time=697 ms
64 bytes from 10.x.x.x (10.x.x.x): icmp_seq=4 ttl=254 time=344 ms
64 bytes from 10.x.x.x (10.x.x.x): icmp_seq=5 ttl=254 time=52.2 ms
64 bytes from 10.x.x.x (10.x.x.x): icmp_seq=6 ttl=254 time=54.3 ms
64 bytes from 10.x.x.x (10.x.x.x): icmp_seq=7 ttl=254 time=267 ms
64 bytes from 10.x.x.x (10.x.x.x): icmp_seq=8 ttl=254 time=181 ms
64 bytes from 10.x.x.x (10.x.x.x): icmp_seq=9 ttl=254 time=141 ms
--- something.elb.amazonaws.com ping statistics ---
9 packets transmitted, 9 received, 0% packet loss, time 13011ms
````
The `ifconfig` for the vpn tunnel and the wifi were showing the following:
```bash
ifconfig: 

tun0: flags=4305<UP,POINTOPOINT,RUNNING,NOARP,MULTICAST> mtu 1500
....
wlp4s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST> mtu 1440
.....
```

My colleague, who is traveling every day by the ICE of Deutsche Bahn told me to set down the MTU, then everything was working:
```bash
sudo ip link set wlp4s0 mtu 500
sudo ip link set tun0 mtu 500
```

Updated 24.09.2019:
It seems that, if you have the `network-manager` running, it can happen, that you have to rerun this process, every time you try to log into the wifi @ ice

