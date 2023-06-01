#devops #server #apache  #centos #http #https #ssl

# Run a http server
Start the [[Apache]] web server
```
$ sudo dnf install httpd
$ sudo systemctl start httpd
$ sudo systemctl status httpd
```

![](https://i.imgur.com/eTH9MCg.png)

Create a virtual host configuration file to server our website. Add the below content to the config file
```
sudo vi /etc/httpd/conf.d/react-app.conf
```

```
<VirtualHost *:80>
    ServerName web.methmal.lk
    DocumentRoot /var/www/html/react-app
    ErrorLog /var/log/httpd/react-app-error.log
    CustomLog /var/log/httpd/react-app-access.log combined
</VirtualHost>
```

Copy the build directory
```
$ sudo cp -r ~/Documents/react-app/build /etc/www/html/react-app
```

Reload apache to virtual host config file to take affect. Apache server is hosting our app in `localhost:80`
```
$ sudo systemctl reload httpd
```

![](https://i.imgur.com/StATpHO.png)

# Enable SSL

[[Obtain a SSL certificate]] first. Then edit the virtual host file and add the following lines
```
$ sudo vi /etc/httpd/conf.d/react-app.conf
```

```
<VirtualHost *:443>
    ServerName web.methmal.lk
    DocumentRoot /var/www/html/react-app
    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/web.methmal.lk/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/web.methmal.lk/privkey.pem
    ErrorLog /var/log/httpd/react-app-error.log
    CustomLog /var/log/httpd/react-app-access.log combined
</VirtualHost>
```

Enable `ssl_mod` in apache and finally restart the server
```
$ sudo dnf install -y ssl_mod
$ sudo ln -s /etc/httpd/modules/mod_ssl.so /etc/httpd/modules/mod_ssl.load
$ sudo systemctl enable --now httpd
```

Add a DNS record
![](https://i.imgur.com/w7hCYxL.png)

Now your site must be running on the given domain name, in https
![](https://i.imgur.com/lvJ4SNl.png)
