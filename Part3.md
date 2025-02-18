## Part III : Storage is still disks in 2025

### 1. LVM
LVM (pour Logical Volume Manager) est l'outil de référence aujourd'hui sous Linux pour créer et gérer les partitions des disques.

Il a beaucoup beaucoup trop de features de fou, il se contente pas de couper des disques !

#### 🌞 Afficher l'état actuel de LVM

afficher la liste des PV (Volume Volumes)
ce sont les disque durs et partitions physiques que LVM gère
afficher la liste des VG (Volume Groups)
on regroupe les PV en des groupes appelés VG
afficher la liste des LV (Logical Volumes)
les VG sont découpés en LV
un LV est une partition utilisable

```bash
[neird4@vbox ~]$ sudo pvs
[sudo] password for neird4: 
  PV         VG      Fmt  Attr PSize  PFree
  /dev/sda2  rl_vbox lvm2 a--  17.00g    0 
[neird4@vbox ~]$ sudo pvg
sudo: pvg: command not found

[neird4@vbox ~]$ sudo pvs
  PV         VG      Fmt  Attr PSize  PFree
  /dev/sda2  rl_vbox lvm2 a--  17.00g    0

[neird4@vbox ~]$ sudo vgs
  VG      #PV #LV #SN Attr   VSize  VFree
  rl_vbox   1   4   0 wz--n- 17.00g    0 

[neird4@vbox ~]$ sudo lvs
  LV   VG      Attr       LSize Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  home rl_vbox -wi-ao---- 4.00g                                                    
  root rl_vbox -wi-ao---- 8.00g                                                    
  swap rl_vbox -wi-ao---- 1.00g                                                    
  var  rl_vbox -wi-ao---- 4.00g        
```

#### 🌞 Déterminer le type de système de fichiers

```bash
[neird4@node1 ~]$ sudo blkid /dev/rl_vbox/root  
/dev/rl_vbox/root: UUID="85a61ef1-eed4-4e37-bad6-ab3be1727f37" TYPE="ext4"
[neird4@node1 ~]$ sudo blkid /dev/rl_vbox/home 
/dev/rl_vbox/home: UUID="db20a60f-e4d6-4a20-a96d-ca570fa79289" TYPE="ext4"
```
