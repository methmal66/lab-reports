#linux #devops #rhel #redhat #gcp #ssh #practical #rdp

Create an instance in GCP with the following specifications
```
Memory : 32GB
CPU : 8vCPU
Storage : 200GB (SSD Presistent)
Image : RHEL9
Region : Mumbai
Instance name : rhel-desktop
Hostname : rhel-desktop
Sudo user : methmal
Firewall rules :
	* Allow HTTP
	* Allow HTTPS
	* Allow RDP
```

# Setup SSH access
Login to the remote instance via the cloud shell and create a sudo user as methmal. Edit the sudoers file as below to grant all privillage to the sudo group
```Remote
$ su root
$ groupadd sudo
$ useradd methmal
$ passwd methmal
$ usermod methmal -aG sudo 
$ chmod u+x /etc/sudoers
$ vi /etc/sudoers
```

```
%sudo   ALL=(ALL)       ALL
```

Then edit the ssh config file to enable password authentication and allow users. Uncomment or add the below two lines
```Remote
$ vi /etc/ssh/sshd_config
```

```
PasswordAuthentication yes
AllowUsers methmal
```

Create a new ssh key in the local machine
```Local
$ ssh-keygen -f ~/.ssh/rhel-desktop -P ""
```

Transfer the public key to the remote instance
```Local
$ ssh-copy-id -i ~/.ssh/rhel-desktop.pub methmal@35.244.22.48
```

>[!warning]- One public key per user
>Suppose you are accesing the server from a cloud shell, it still read you by your os. So you cant add another key to ssh with a third party client. But, you can force the new key to replace the old key. In that case, you lose access from the already existing cloud shell. Following is the command
``` Local
$ ssh-copy-id -i ~/.ssh/rhel-desktop.pub -f methmal@35.244.22.48
```

Then ssh into the remote instance. This will not prompt for the methmal's password
```Local
$ ssh -i ~/.ssh/centos-desktop methmal@35.244.22.48
```
![[Pasted image 20230507194642.png]]
Add the default ssh config for our cloud instance
```Local
$ sudo vi ~/.ssh/config
```

```
Host rhel-desktop
HostName 34.125.220.126
User methmal
IdentityFile ~/.ssh/rhel-desktop
```

Then you can ssh into the server by just using the alias we provided
```Local
$ ssh rhel-desktop
```
![[Pasted image 20230507194532.png]]
Register system under redhat
```Remote
$ sudo dnf install -y subscription-manager
$ sudo subscription-manager register --username "Methmal66" --password "jephy9-Dykmut-dujgud"
$ sudo subscription-manager identity
```
![[Pasted image 20230507194424.png]]

# Install desktop environment


Install dnf as a replacement of yum
```Remote
$ sudo yum install -y dnf
```

Install KDD as a desktop environment
```Remote
$ sudo dnf update -y
$ sudo yum groupinstall -y "Server with GUI"
```

Boot system into the gui mode by default
```Remote
$ sudo systemctl set-default graphical.target
```

Reboot the system, then it will boot into gui mode
```Remote
$ reboot
```

# Setup xrdp access
Install xrdp
```Remote
$ sudo dnf install xrdp
```

Start xrdp server
```Remote
$ sudo systemctl enable --now xrdp
$ systemctl status xrdp
```
![[Pasted image 20230507185131.png]]
Install Microsoft Remote Desktop in local system. Then connect to the remote system as below.
![[Pasted image 20230507185615.png]]
Rememer to adjust the display resolution as you prefered
![[Pasted image 20230507190914.png]]
