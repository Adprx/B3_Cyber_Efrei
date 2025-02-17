### 2. Proofs

🌞 Prouvez que le schéma de partitionnement a bien été appliqué

genre les partitions qu'on a défini à l'install
une commande qui affiche toutes les partitions en cours d'utilisation
ainsi que l'espace disponible sur chacune des partitions

```
[neird4@vbox ~]$ lsblk                                                    
NAME             MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda                8:0    0   20G  0 disk 
├─sda1             8:1    0    1G  0 part /boot
└─sda2             8:2    0   17G  0 part 
  ├─rl_vbox-root 253:0    0    8G  0 lvm  /
  ├─rl_vbox-swap 253:1    0    1G  0 lvm  [SWAP]
  ├─rl_vbox-var  253:2    0    4G  0 lvm  /var
  └─rl_vbox-home 253:3    0    4G  0 lvm  /home
```

➜ Par défaut, sous Rocky Linux :

il existe un groupe appelé wheel déjà créé à l'installation
le groupe wheel est déjà dans la conf sudo pour autoriser ses membres à utiliser les droits de root avec la commande sudo


🌞 Mettre en évidence la ligne de configuration sudo qui concerne le groupe wheel

avec un cat TRUC | grep TRUC je veux voir que la bonne ligne

```bash
[neird4@vbox ~]$ cat /etc/sudoers | grep wheel                            
cat: /etc/sudoers: Permission non accordée
[neird4@vbox ~]$ sudo !!
sudo cat /etc/sudoers | grep wheel
[sudo] Mot de passe de neird4 : 
Désolé, essayez de nouveau.
[sudo] Mot de passe de neird4 : 
## Allows people in group wheel to run all commands
%wheel	ALL=(ALL)	ALL
# %wheel	ALL=(ALL)	NOPASSWD: ALL
```

🌞 Prouvez que votre utilisateur est bien dans le groupe wheel

```bash
[neird4@vbox ~]$ groups
neird4 wheel
```

🌞 Prouvez que la langue configurée pour l'OS est bien l'anglais
je veux une ligne de commande qui affiche la langue actuelle de l'OS
que vos messages d'erreur soient en anglais ça me suffit pas ;D

```bash
[neird4@vbox ~]$ sudo cat /etc/locale.conf  
LANG="en_US.UTF-8"
[neird4@vbox ~]$ echo $LANG
en_US.utf8
```

🌞 Prouvez que le firewall est déjà actif

```bash
[neird4@vbox ~]$ sudo systemctl status firewalld                                                                                                               
WARNING: terminal is not fully functional
Press RETURN to continue 
● firewalld.service - firewalld - dynamic firewall daemon
     Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; preset: enabled)
     Active: active (running) since Mon 2025-02-17 14:59:05 CET; 1h 9min ago
       Docs: man:firewalld(1)
   Main PID: 692 (firewalld)
      Tasks: 2 (limit: 11087)
     Memory: 44.6M
        CPU: 348ms
     CGroup: /system.slice/firewalld.service
             └─692 /usr/bin/python3 -s /usr/sbin/firewalld --nofork --nopid

Feb 17 14:59:05 localhost systemd[1]: Starting firewalld - dynamic firewall daemon...
Feb 17 14:59:05 localhost systemd[1]: Started firewalld - dynamic firewall daemon.
```

le service de firewalling s'appelle firewalld sous Rocky (on le manipule avec la commande firewall-cmd)
