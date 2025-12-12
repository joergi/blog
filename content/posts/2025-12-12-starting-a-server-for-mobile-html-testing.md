---
title: "Starting a server for mobile HTML testing"
description: "if you want to test a website on your mobile, you must start a python server"
date: 2025-12-12
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

The website is now available via [http://0.0.0.0:8000/](http://0.0.0.0:8000/)
