### Dispatch
Dispatch is the service which dispatches the product after purchase. It is written in GoLang, So wanted to install GoLang.

Install GoLang

```
dnf install golang -y
```

Configure the application.

Add application User

```
useradd roboshop
```

Lets setup an app directory.

```
mkdir /app 
```

Download the application code to created app directory.

```
curl -L -o /tmp/dispatch.zip https://roboshop-builds.s3.amazonaws.com/dispatch.zip
cd /app 
unzip /tmp/dispatch.zip
```

Every application is developed by development team will have some common softwares that they use as libraries. This application also have the same way of defined dependencies in the application configuration.

Lets download the dependencies & build the software.

```
cd /app 
go mod init dispatch
go get 
go build
```

We need to setup a new service in systemd so systemctl can manage this service

Setup SystemD Dispatch Service

```
vim /etc/systemd/system/dispatch.service
```

```
[Unit]
Description = Dispatch Service
[Service]
User=roboshop
Environment=AMQP_HOST=rabbitmq.lingaiah.online
Environment=AMQP_USER=roboshop
Environment=AMQP_PASS=roboshop123
ExecStart=/app/dispatch
SyslogIdentifier=dispatch

[Install]
WantedBy=multi-user.target
```

Load the service.

```
systemctl daemon-reload
```

Start the service.

```
systemctl enable dispatch 
systemctl start dispatch
```

# Trouble shoot commands:
🌐 Check if dispatch  service is running
```
sudo systemctl status dispatch
```
🔍 Check if dispatch process is running
```
ps -ef | grep dispatch
```

🌍 Verify if dispatch server's IP address is reachable
```
ping -c 3 dispatch.lingaiah.online

````


We already discussed in Linux basics section that applications should run as nonroot user.

Add application User
```
useradd --system --home /app --shell /sbin/nologin --comment "roboshop system user" roboshop
```
Explanation:
That command creates a system user named roboshop. Here’s what each part does:

useradd \
  --system                 # Create a system account (usually UID < 1000)
  --home /app               # Set /app as the home directory
  --shell /sbin/nologin     # Disable interactive shell login
  --comment "roboshop system user"  # Description (GECOS field)
  roboshop
