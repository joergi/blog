---
title: "Starting a server for mobile HTML testing"
description: "if you want to test a website on your mobile, you must start a python server"
date: 2025-12-12
lastmod: 2025-12-19
---

# Problem: how to test on your mobile phone
When I'm building a HTML website with responsive design, I want to test the website on my mobile phone. 
So I need to start an easy webserver.

# Starting the server
As python is installed on my machine, I can start it easily with python:

```bash
$ python3 -m http.server 8000
# Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

Now find out the ip from your own computer where you develop:
```bash
ifconfig
# wlp....:
# inet 192.168.178.162  netmask 255.255.255.0  broadcast 192.168.178.255
```
So you can see the IP is on 192.168.178.162
So inside the same network (wifi) the website is now available via http://192.168.178.162:8000
