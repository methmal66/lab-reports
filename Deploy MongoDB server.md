#linux #ubuntu #mongodb #database #practical #lab-report 

Name: mongodb-region
OS: Ubuntu 20.04 Focal
CPU: 2 vCPU
Memory: 1GB
Disk: 10GB Balanced persistent disk

# Installing MongoDB
```embed
title: 'Install MongoDB Community Edition on Ubuntu'
image: 'https://www.mongodb.com/docs/assets/meta_generic.png'
description: 'Docs Home → Develop Applications → MongoDB Manual'
url: 'https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/'
```

Update system
```
apt-get update && apt-get upgrade -y
```

Import MongoDB public GPG key
```
curl -fsSL https://pgp.mongodb.com/server-6.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-6.0.gpg \
   --dearmor
```

Create the file list for Ubuntu 20.04 focal
```
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list;
apt-get update
```

Install MongoDB package
```
apt-get install -y mongodb-org
```

# Configuring MongoDB
```embed
title: 'How to connect to your remote MongoDB server'
image: 'https://miro.medium.com/v2/resize:fit:1200/0*ipFg5AhLC6XClpDG.png'
description: 'I have MongoDB running on my Ubuntu server in Amazon EC2. Since there’s no simple all-in-one tutorial out there explaining how to set up…'
url: 'https://indianceo.medium.com/how-to-connect-to-your-remote-mongodb-server-68725a8e53f'
```

Main config file is located at `/etc/mongod.conf`. Edit it and allow access from any IP address
```
# network interfaces  
net:  
	port: 27017  
	bindIp: 0.0.0.0 <- update this line
```

In the same file, edit the security options as bellow. Make sure to uncomment the security option
```
security:  
	authorization: 'enabled'
```

>[!warning]- Warning
>Do _not_ comment out the `bindIp` line without enabling authorization. Otherwise you will be opening up the whole internet to have full admin access to all mongo databases on your MongoDB server!

# Start the service
Start mongod service
```
systemctl enable --now mongod
systemctl status mongod
```
![](https://i.imgur.com/PhujopM.png)
Open ports in the firewall. The most commonly used port is 27017/tcp in the default mode. Remember to create the corresponding firewall rules in your cloud providers as well.
```
apt-get install firewalld
firewall-cmd --add-port=21017/tcp --permanent
firewall-cmd --reload
firewall-cmd --list-ports
```

Access the DB using mongo shell. User name or password is not needed to connect with the local db server.
```
mongosh
```
![](https://i.imgur.com/URteMA0.png)

There are no default users in MongoDB. Lets create a root user to create other users.
```embed
title: 'MongoDB - admin user not authorized'
image: 'https://cdn.sstatic.net/Sites/stackoverflow/Img/apple-touch-icon@2.png?v=73d79a89bded'
description: 'I am trying to add authorization to my MongoDB. I am doing all this on Linux with MongoDB 2.6.1. My mongod.conf file is in the old compatibility format (this is how it came with the installation).…'
url: 'https://stackoverflow.com/a/29472184'
```

```
use admin
db.createUser({
	user: "root",
	pwd: "root",
	roles: [{ roles: "root", db: "admin" }]
})
```

Only root user can create other users. Therefor, exit from the shell using `exit` command and login again as the root user we have just created. Insert the password when prompted
```
mongosh -u root
```

![](https://i.imgur.com/owj2Nq6.png)
Lets create another user called methmal and grant read/write access to db methmal. That user will be used to connect from a remote client.
```
use admin
db.createUser({
 user: "methmal",
 pwd: "methmal",
 roles: [{ role: "readWrite", db: "methmal" }]
})
```

Restart the service to apply the user permissions. 
```
systemctl restart mongosh
```

# Test the connection

Download and install the MongoDB Compass GUI client in the local machine.
```embed
title: 'MongoDB Compass Download (GUI)'
image: 'https://webimages.mongodb.com/_com_assets/cms/kuzt9r42or1fxvlq2-Meta_Generic.png'
description: 'MongoDB Compass, the GUI for MongoDB, is the easiest way to explore and manipulate your data. Download for free for dev environments.'
url: 'https://www.mongodb.com/try/download/compass'
```

![](https://i.imgur.com/3lv8Dg5.png)

Create a new connection to our remote mongodb server. Use authentication method as username and password. Connectioni URI should be something simillar to `mongodb://methmal:methmal@35.188.101.41:27017/?authMechanism=DEFAULT`.
![](https://i.imgur.com/dUmxfBx.png)

You are now connected our self hosted MongoDB instance. Select the db methmal.
![](https://i.imgur.com/4XygLKj.png)

>[!info]- Non existant DB
>Notice that we didn't explicitly create the DB named `methmal`. It has been created automatically when granting the permissions.

