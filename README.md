# SERVER CONFIGURATIONS UBUNTU
If you do not remove the default DNS stub resolver on Ubuntu you will have a conflict when starting Pihole docker-compose.yml file

### -------------------------------------------
### UPDATE YOUR SYSTEM
- **root@comp %** ```sudo apt update -y```
- **root@comp %** ```sudo apt -y install nano curl net-tools docker.io docker-compose```

### copy the yaml files to there locations

/etc/netplan/netplan1.yml

/var/containers/pihole/docker-compose.yml

then configure the dns stub resolver and check internet

- **root@comp %** ```sudo docker-compose up -d```
- **root@comp %** ```sudo docker-compose run pihole pihole -a -p```

- **root@comp %** ```sudo netplan apply```
- **root@comp %** ```sudo ifconfig grep | inet```


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
### CHECK DNS OF UBUNTU SERVER

- **root@comp %** ```sudo apt install dnsutils```

test if ok with answer --> status: NOERROR
- **root@comp %** ```dig sigok.verteiltesysteme.net @192.168.0.222 -p 5335 +dnssec```

test if fail no answer --> status: SERVFAIL
- **root@comp %** ```dig sigfail.verteiltesysteme.net @192.168.0.222 -p 5335 +dnssec```


### -------------------------------------------
### Setup static ipaddr for ubuntu server

Modify this file to suit your network device -> ens18 ==> eth0

- **root@comp %** ```sudo nano /etc/netplan/netplan1.yaml```

```
network:
   version: 2
   ethernets:
     ens18:
       dhcp4: False
       addresses:
          - 192.168.0.245/16
       gateway4: 192.168.0.1
       nameservers:
          addresses: [8.8.8.8,4.4.4.4]
```

- **root@comp %** ```sudo netplan apply```
- **root@comp %** ```sudo apt install net-tools```
- **root@comp %** ```sudo ifconfig grep | inet```

### -------------------------------------------




