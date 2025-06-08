### Redis
Redis is used for in-memory data storage(Caching) and allows users to access the data of database over API.

Redis is offering the repo file as a rpm. Lets install it

```
dnf install redis -y
```


Usually Redis opens the port only to localhost(127.0.0.1), meaning this service can be accessed by the application that is hosted on this server only. However, we need to access this service to be accessed by another server, So we need to change the config accordingly.

Update listen address from 127.0.0.1 to 0.0.0.0 in /etc/redis/redis.conf

```
vim /etc/redis/redis.conf
```

Start & Enable Redis Service

```
systemctl enable redis
```

```
systemctl start redis
```

# Trouble shoot commands:
🌐 Check if redis service is running
```
sudo systemctl status redis

```

🌐 Check if redis is listening on port 6379
```
sudo netstat -lntp
sudo netstat -lntp | grep 6379
```

📡 Test connectivity to the redis server on port 6379
```
telnet redis.lingaiah.online 6379
```

🔍 Check if redis process is running
```
ps -ef | grep redis
```


🌍 Verify if redis server's IP address is reachable
```
ping -c 3 redis.lingaiah.online

````