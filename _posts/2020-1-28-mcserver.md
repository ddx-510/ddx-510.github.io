---
layout: post
title:  "Lets play minecraft"
date:   2020-01-28
excerpt: "How to build a MC server"
tag:
- server
- mc
- tutorial
---
Many thanks to the zhihu User: [Amserphere](https://zhuanlan.zhihu.com/p/55952581
)

# MC server
## Requirements:
- Money
- Some IQ
- A laptop
- An account : either alicloud or google cloud

## Procedures:
### Part 0:

- Find product in google cloud/ alicloud. Here I am using google cloud might be a little bit different from alicloud. Select VM engine, create a new instance and select ubuntu 18.04, I only gave it a g1-micro core, but you can add more if you want.

- open vm engine, click ssh to access to the server

### Part 1:

Gain operation right

```
sudo su root
```

Update apt

```
sudo apt update
```

Next, check if Java is already installed:

```
java -version
```

If not, follow the suggestion

```
apt install default-jre
```
RMB to click yes for the installation

Get the foooking package

```
sudo wget https://launcher.mojang.com/v1/objects/e9f105b3c5c7e85c7b445249a93362a22f62442d/server.jar
```

### Part 2:
Find path

```
pwd
```

Skip the firewall setting if u r lazy. But this will greatly reduce your server's security so think twice.

```
systemctl stop firewalld.service
```

OR else:
Add a port (25565 default)

```
firewall-cmd --zone=public --add-port=25565/tcp --permanent
```
If firewall not installed, then follow the suggested command to install firewall utils

Go to Fire wall settings:
- Add firewall rule TCP with port range 25565

### Part 3:
Start server!

```
sudo java -Xms512m -Xmx1024m -jar /home/admin/server.jar nogui
```
512 is the min rom, 1024 is the max.. /home/admin might need to change to your own home root, nogui means no graphics os

It might takes sometime, and

```
/stop
```
to stop the server

Let's change a few more settings to begin!

Agree the agreement:
```
vi eula.txt
```

Go in by enter
``
i
``
Move to the eula=false and change it to eula=true

Press esc, and key in
``
:wq
``

Press enter to save

IF you are using non-official version

```
vi server.properties
```
change
``
online-mode:true
``
to
``online-mode:false``

### Final Steps
re-enter
```
sudo java -Xms512m -Xmx1024m -jar /home/admin/server.jar nogui
```
And ur server is ready!!!

Go to mc and add ur own server address

Example:
- My server's internal ip address on vm engine page is 10.12.x.x
- We click add server, and address same as above
- Add in the port we use : 25565

And enjoy your game!
