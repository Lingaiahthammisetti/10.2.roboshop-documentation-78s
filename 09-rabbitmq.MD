### RabbitMQ

RabbitMQ is a messaging Queue which is used by some components of the applications.

Configure YUM Repos from the script provided by vendor.

```
curl -s https://packagecloud.io/install/repositories/rabbitmq/erlang/script.rpm.sh | bash
```

Configure YUM Repos for RabbitMQ.

```
curl -s https://packagecloud.io/install/repositories/rabbitmq/rabbitmq-server/script.rpm.sh | bash
```

Install RabbitMQ

```
dnf install rabbitmq-server -y 
```

Start RabbitMQ Service

```
systemctl enable rabbitmq-server 
```

```
systemctl start rabbitmq-server 
```

RabbitMQ comes with a default username / password as guest/guest. But this user cannot be used to connect. Hence, we need to create one user for the application.

```
rabbitmqctl add_user roboshop roboshop123
```
```
rabbitmqctl set_permissions -p / roboshop ".*" ".*" ".*"
```


# Trouble shooting commands:

Redis Server running or not?
```
systemctl status rabbitmq-server
```

Redis port opened or not?
```
netstat -lntp
```
```
netstat -lntp |  grep 5672
```

Redis  process is opened or not?
```
ps -ef | grep rabbitmq-server 
```

Verify if Redis server's IP address is reachable
```
ping -c 3 rabbitmq.lingaiah.online
````
