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

Setup SSH access into that as described below
[[SSH into a cloud VPS]]

Register system under redhat
```Remote
$ sudo dnf install -y subscription-manager
$ sudo subscription-manager register --username "Methmal66" --password "jephy9-Dykmut-dujgud"
$ sudo subscription-manager identity
```
![](https://i.imgur.com/VywIWSf.png)
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
![](https://i.imgur.com/ewvXfjG.png)

Install Microsoft Remote Desktop in local system. Then connect to the remote system as below.
![](https://i.imgur.com/1jEYGyv.png)
Rememer to adjust the display resolution as you prefered
![](https://i.imgur.com/rALXyfy.png)

