#practical #lab-report #linux #centos #git #gitea

```embed
title: 'ChatGPT'
image: 'https://chat.openai.com/apple-touch-icon.png'
description: 'A conversational AI system that listens, learns, and challenges'
url: 'https://chat.openai.com/share/3b1300be-76b2-4f18-85de-465c3f5525c6'
```

# Setup MySQL 
Gitea requires a database like postgresql, mariadb or mysql. Setup the mysql server according to this guide: [[Deploy Mysql]]

>[!information]- Configs
>
>password: `e!+UvP8E<9,b`
>db user: gitea
>db: gitea

# Setup Gitea
Install git
```embed
title: 'Install Latest Git ( Git 2.x ) on CentOS 7 | ComputingForGeeks'
image: 'https://computingforgeeks.com/wp-content/uploads/2018/10/install-latest-git-centos-7.png'
description: 'Once repository is added, install Git 2.x on CentOS 7:'
url: 'https://computingforgeeks.com/install-git-2-on-centos-7/'
```

```
$ sudo dnf remove git
$ sudo yum -y install https://packages.endpointdev.com/rhel/7/os/x86_64/endpoint-repo.x86_64.rpm
$ sudo dnf install -y git 
```

>[!information]- Require a newer version of git
>Gitea requires a git version which is above 2.0 But centos7 repo include a bit older version of git. Therefor we are adding an end point to install a suitable version.

Install gitea
```
$ sudo dnf install xz
$ cd
$ curl -LO https://dl.gitea.com/gitea/1.19.3/gitea-1.19.3-linux-amd64.xz
$ xz -d gitea-1.19.3-linux-amd64.xz
$ sudo mkdir /opt/gitea
$ sudo mv gitea-1.19.3-linux-amd64 /opt/gitea/gitea
```

Set the necessary permissions
```
$ sudo useradd -m gitea
$ sudo chown -R gitea:gitea /opt/gitea
$ sudo chmod -R 770 /opt/gitea
```

Run gitea web interface 
```
$ sudo -u gitea ./opt/gitea web
```
![](https://i.imgur.com/nTKPTOI.png)
>[!information]- I tried to create a system unit, but it didn't work
>![](https://i.imgur.com/iHU3Zg2.png)

Fill the form and complete the initial configuration 
![](https://i.imgur.com/OfKR25Z.png)

Welcome to the main page
![](https://i.imgur.com/TW5klgC.png)
 
# Use with a client
Go to profile and see that we are signed in as `methmal`
![](https://i.imgur.com/zqc2cSz.png)

Create a new git repo
![](https://i.imgur.com/20drpEc.png)

![](https://i.imgur.com/S7itUwf.png)

Clone repo into local machine
```
$ git clone http://34.100.245.49:3000/methmal/test-repo.git
```

![](https://i.imgur.com/sZ3UJX8.png)
Make some changes and push it to the server. It will use password authentication by default.
![](https://i.imgur.com/qq28puV.png)
Recent changes are updated in the gitea interface
![](https://i.imgur.com/tzLUVhJ.png)
