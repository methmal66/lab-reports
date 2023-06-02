#ssh #devops #ssh #centos

# Configure the remote instace
Create an account to log in using [[SSH]]
```Remote
$ su root
$ useradd -m methmal
$ groupadd sudo
$ usermod -aG sudo methmal
```

>[!tip]- SSH in cloud instances
>In most cases, SSH service is already running in cloud instances. We only need to edit the config files 

Login to the VM using cloud console. Then enable password authentication for the above created user as metioned below
```Remote
$ su root
$ chmod u+w /etc/ssh/sshd_config
$ vi /etc/ssh/sshd_config
```

```
PasswordAuthentication yes
AllowUsers methmal
```

Now go the local machine and add a host name pointing to the remote instance
```Local
$ sudo vi /etc/hosts
```

```
34.100.129.247 web.methmal.lk
```

Create a new ssh key
```Local
$ ssh-keygen -f ~/.ssh/web.methmal.lk -P ""
```

Copy the public key into the remote instace
```
$ ssh-copy-id -i ~/.ssh/web.methmal.lk.pub methmal@web.methmal.lk
```

You should now be able to ssh into our remote instace with out the password
```Local
$ ssh -i ~/.ssh/web.methmal.lk methmal@web.methmal.lk
```

>[!important]- You must do the all configs of the local machine loged as the same user, methmal

Check to make sure that only the ssh key we need has been added
```Remote
$ sudo cat ~/.ssh/authorized_keys
```

![](https://i.imgur.com/gOmRH8m.png)

Now, make a new entry in `~/.ssh/config` file as below
```
Host web-server
HostName 34.100.129.247
User methmal
IdentityFile ~/.ssh/web.methmal.lk
```

Connect to ssh server
```
$ ssh web-server
```
