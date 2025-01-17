Hugo

Start server:
```bash
hugo server -D
```

Start with IP binding
```bash
hugo server -D --bind 192.168.178.162 --appendPort=false
```
website can be then reached by: http://192.168.178.162:1313

update server
```bash
hugo --logLevel info
```

kill server
```bash
pkill hugo
```
