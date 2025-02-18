### Part II : Networking
#### 1. Basic networking conf
#### A. Static IP

🌞 Attribuer l'adresse IP 10.1.1.11/24 à la VM

ça veut dire que votre PC a pour adresse IP 10.1.1.X/24 (il est dans le même réseau)
je vous file les instructions pour la définition de l'IP dans la VM, avec Rocky Linux on peut faire comme ça pour la définition d'une IP statique :

```bash

[neird4@vbox network-scripts]$ sudo cat ifcfg-enp0s8
DEVICE=enp0s8
NAME=lan

BOOTPROTO=static
ONBOOT=yes

IPADDR=10.1.1.11
NETMASK=255.255.255.0
```
```bash
[neird4@vbox ~]$ ip -c a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:47:27:9b brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 78143sec preferred_lft 78143sec
    inet6 fd00::a00:27ff:fe47:279b/64 scope global dynamic noprefixroute 
       valid_lft 86320sec preferred_lft 14320sec
    inet6 fe80::a00:27ff:fe47:279b/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:9d:0d:30 brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.11/24 brd 10.1.1.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe9d:d30/64 scope link 
       valid_lft forever preferred_lft forever
```
B. Hostname
🌞 Attribuer le nom node1.tp1.b3 à la VM

ça se fait avec une commande hostnamectl en 2025 svp

```bash
[neird4@vbox network-scripts]$ hostname
node1.tp1.b3
[neird4@vbox network-scripts]$ hostnamectl 
   Static hostname: (unset)                           
Transient hostname: node1.tp1.b3
         Icon name: computer-vm
           Chassis: vm 🖴
        Machine ID: e92ae3030ece4289ba5fd4f8334949e0
           Boot ID: d5fce095f5b34fed9696cd6d415d8a32
    Virtualization: oracle
  Operating System: Rocky Linux 9.5 (Blue Onyx)       
       CPE OS Name: cpe:/o:rocky:rocky:9::baseos
            Kernel: Linux 5.14.0-503.23.2.el9_5.x86_64
      Architecture: x86-64
   Hardware Vendor: innotek GmbH
    Hardware Model: VirtualBox
  Firmware Version: VirtualBox
```

2. Listening ports

🌞 Déterminer la liste des programmes qui écoutent sur un port TCP
🌞 Déterminer la liste des programmes qui écoutent sur un port UDP

```bash
[neird4@node1 network-scripts]$ sudo ss -tulnp
[sudo] password for neird4: 
Netid        State         Recv-Q        Send-Q               Local Address:Port               Peer Address:Port       Process                                  
udp          UNCONN        0             0                        127.0.0.1:323                     0.0.0.0:*           users:(("chronyd",pid=695,fd=5))        
udp          UNCONN        0             0                            [::1]:323                        [::]:*           users:(("chronyd",pid=695,fd=6))        
tcp          LISTEN        0             128                        0.0.0.0:22                      0.0.0.0:*           users:(("sshd",pid=729,fd=3))           
tcp          LISTEN        0             128                           [::]:22                         [::]:*           users:(("sshd",pid=729,fd=4))
``` 

3. Firewalling
➜ Vous pouvez afficher l'état actuel de firewalld, le firewall de Rocky Linux, avec :

🌞 Pour chacun des ports précédemment repérés...

montrez qu'il existe une règle firewall qui autorise le trafic entrant sur ce port
ou pas ?

```bash
[neird4@node1 ~]$ sudo firewall-cmd --list-all                                   
[sudo] password for neird4: 
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources: 
  services: cockpit dhcpv6-client ssh
  ports: 
  protocols: 
  forward: yes
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules:
```

```bash
[neird4@node1 ~]$ sudo firewall-cmd --list-services
cockpit dhcpv6-client ssh
```

```bash
[neird4@node1 ~]$ sudo cat /etc/services | grep dhcpv6-clien
dhcpv6-client   546/tcp
dhcpv6-client   546/udp
[neird4@node1 ~]$ sudo cat /etc/services | grep ssh         
ssh             22/tcp                          # The Secure Shell (SSH) Protocol
ssh             22/udp                          # The Secure Shell (SSH) Protocol
x11-ssh-offset  6010/tcp                        # SSH X11 forwarding offset
ssh             22/sctp                 # SSH
[neird4@node1 ~]$ sudo cat /etc/services | grep cockpit
[neird4@node1 ~]$ 
```

🌞 Fermez tous les ports inutilement ouverts dans le firewall

principe du moindre privilège encore et encore !
pas besoin qu'un port soit ouvert si aucun service n'écoute dessus

```bash
[neird4@node1 ~]$ sudo firewall-cmd --remove-service=cockpit --permanent         
success
[neird4@node1 ~]$ sudo firewall-cmd --remove-service=ssh --permanent    
success
[neird4@node1 ~]$ sudo firewall-cmd --remove-service=dhcpv6-client --permanent
success
[neird4@node1 ~]$ sudo firewall-cmd --reload                                  
success
[neird4@node1 ~]$ sudo firewall-cmd --list-all                                
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources: 
  services: 
  ports: 
  protocols: 
  forward: yes
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
```

🌞 Pour toutes les applications qui sont en écoute sur TOUTES les adresses IP

dans Linux, ce sont les applications qui écoutent sur la pseudo-adresse IP 0.0.0.0 : ça signifie que toutes les adresses IP de la machine sont concernées
modifier la configuration de l'application pour n'écouter que une seule IP : celle qui est nécessaire

```bash
[neird4@node1 ~]$ sudo firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="10.1.1.1" port port="22"  
protocol="tcp" accept' --permanent  
[sudo] password for neird4:    
success  
[neird4@node1 ~]$ sudo firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="10.1.1.1" port port="323  
" protocol="udp" accept' --permanent  
success
```
