# SERVER CONFIGURATIONS UBUNTU
If you do not remove the default DNS stub resolver on Ubuntu you will have a conflict when starting Pihole docker-compose.yml file\
dont forget if you use ubuntu disable and remove ufw\

### -------------------------------------------
### UPDATE YOUR SYSTEM
- **root@comp %** ```sudo apt update -y```
- **root@comp %** ```sudo apt -y install nano curl net-tools docker.io docker-compose```

### copy the yaml files to there locations

/etc/netplan/netplan1.yml

/opt/docker/PiHole/docker-compose.yml

then configure the dns stub resolver and check internet

- **root@comp %** ```sudo docker-compose up -d```
- **root@comp %** ```sudo netplan apply```

### -------------------------------------------
### REMOVE DEFAULT LINUX DNS STUB RESOLVER

Disable systemd-resolved listener on port 53 dns will use this port
- **root@comp %** ```sudo systemctl stop systemd-resolved.service```
- **root@comp %** ```sudo systemctl disable systemd-resolved.service```

The next command should fail ping: google.com: Temporary failure in name resolution
- **root@comp %** ```ping www.google.com```

Now modify the resolv file and change the 127.0.0.53 to point to googles dns 8.8.8.8 or 4.4.4.4
- **root@comp %** ```sudo nano /etc/resolv.conf```

Now ping will work
- **root@comp %** ```ping www.google.com```


### -------------------------------------------
### Setup static ipaddr for ubuntu server

Modify this file to suit your network device -> ens18 ==> eth0

- **root@comp %** ```sudo nano /etc/netplan/netplan1.yaml```

<sub>make sure about your tab in the yaml file as it is very strict and could cause break</sub>
```
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
     dhcp4: no
     addresses: [192.168.1.233/24]
     gateway4: 192.168.1.1
     nameservers:
       addresses: [8.8.8.8,8.8.4.4]
```

- **root@comp %** ```sudo netplan apply```
- **root@comp %** ```sudo apt install net-tools```
- **root@comp %** ```sudo ifconfig grep | inet```

### -------------------------------------------

#Admin Interface :: http://<your_ipaddr>/admin


