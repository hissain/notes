### Sys commands

Free RAM: ```free -h```

Sys info: ```uname -a```


### Bining ports
```
jupyter notebook --no-browser --port=8080 # in remote
ssh -L 8080:localhost:8080 user@remote_host # local_port:remote_host:remote_port
```

### Allow port in Firewall
```
sudo ufw status
sudo ufw allow 8181
```
