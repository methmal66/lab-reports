#database #postgresql #linux #devops  #centos

# Installation
Install [[PostgreSQL]] server
```
$ sudo dnf install -y postgresql-server
```

Initialize the database
```
$ sudo postgresql-setup initdb
```

Run the service
```
$ sudo systemctl enable --now postgresql
$ systemctl status postgresql
```
![[Pasted image 20230510083338.png]]
>[!important]- Default data directory
>`/var/lib/pgsql/data`

Open the port in firewall. Remember to configure your cloud provider's firewall as well
```
$ sudo firewall-cmd --add-port=5432/tcp --permanent
$ sudo firewall-cmd --reload
$ sudo firewall-cmd --list-ports
```
![[Pasted image 20230510085329.png]]
Using a remote client to check if the port is open using [[nmap]]
![[Pasted image 20230510085743.png]]
# Post installation
To enable password authentication on remote connections, change the `ident` to `md5` and ipv4 host to listen from any address as the final config file is shown below
```
$ sudo vi /var/lib/psql/data/pg_hba.conf
```
![](https://i.imgur.com/XpzEeOt.png)
Then restart the server
```
$ sudo systemctl restart postgresql
```

Access the psql shell
```
$ sudo -u postgres psql
```

![](https://i.imgur.com/y1jbzUn.png)
>[!warning]- Dont reset the password for postgres
>By default, a superuser called `postgres` is created. It is for the administration purposes doing directly from the localhost only. Therefor, that account is locked by default. So do not reset it's password using `passwd`. Instead we are accessing the `psql` as postgres user without unlocking that account

To connect remotely, lets create a sperate user and db, then grant access to it
```
postgres=# CREATE USER methmal WITH PASSWORD 'methmal'
postgres=# CREATE DATABASE methmal;
postgres=# GRANT ALL PRIVILEGES ON DATABASE methmal TO methmal;
```

Test if that user working properly
```
$ psql -U methmal -d methmal
```
![](https://i.imgur.com/KhDhLot.png)
Edit the `postgresql.conf` file and enable listen on all addresses. Add or uncomment the below line
```
$ sudo vi /var/lib/pgsql/data/postgresql.conf
```

```
listen_addresses = '*'
```
# Connect to clients

## Connect with cli client
Install postgresql to get the client (non of the client only tools i tested didn't work)
```
$ brew install postgresql
```

Connect to remote host
```
$ psql -h 35.244.22.48 -U methmal -d methmal
```
![](https://i.imgur.com/iX7q9YQ.png)
## Connect with gui client

Download [[pgAdmin]]4 desktop version
```embed
title: 'Download'
image: 'https://www.pgadmin.org/static/COMPILED/assets/img/favicon.ico'
description: 'pgAdmin - PostgreSQL Tools for Windows, Mac, Linux and the Web'
url: 'https://www.pgadmin.org/download/'
```

Open application
![](https://i.imgur.com/txwUbl5.png)
Setup a new connection to our server
![](https://i.imgur.com/DWUNZj8.png)
![](https://i.imgur.com/GqZL49W.png)
