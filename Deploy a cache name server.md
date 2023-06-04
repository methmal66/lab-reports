#linux #centos #dns #networking #practical 

Install [[Bind]] [[DNS]] server
```
$ sudo dnf install -y bind bind-utils
```

Edit `/etc/named.conf` and add the fowarders as below, inside the options. I have used google free name servers. You can use any other name server you prefer
```
forwarders { 
	8.8.8.8; 
	8.8.4.4; 
};
```

Remove the below option where the listing ports are mentioned. So the name server can be accessed from any IP. Each IPs you allow to use this server can be individually added as well.
```
listen-on port 53 { 127.0.0.1; };
```

Start the service
```
$ sudo systemctl enable --now named
$ systemctl status named
```
![](https://i.imgur.com/x8UrN7f.png)
Open the port in firewall
```
$ sudo firewall-cmd --add-service=dns --permanent
$ sudo firewall-cmd --reload
```

Then test the connection inside the localhost
```
$ sudo dig @localhost google.com
```
![](https://i.imgur.com/upqycHU.png)
Run the command again and notice the query time. Because of the cached entry, further requests for the same record are significantly faster until the entry expires.
![](https://i.imgur.com/Wf4rwdk.png)

# Use with a client

Test the connection from a remote client
```
$ dig @34.100.196.76 google.com
```
![](https://i.imgur.com/NLaVjXo.png)
# Linux

Change the name server in `/etc/resolv.conf` as bellow
```
nameserver 32.100.196.76
```

# Mac OS
![](https://i.imgur.com/j8sVjyT.png)
# Windows
![](https://i.imgur.com/MNQvffV.png)

