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
[neird4@node1 ~]$ sudo pvs
  PV         VG      Fmt  Attr PSize  PFree
  /dev/sda2  rl_vbox lvm2 a--  17.00g    0

[neird4@node1 ~]$ sudo vgs
  VG      #PV #LV #SN Attr   VSize  VFree
  rl_vbox   1   4   0 wz--n- 17.00g    0 

[neird4@node1 ~]$ sudo lvs
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

### 2. HELP my partition is full
#### 🌞 Remplissez votre partition /home

```bash
[neird4@vbox ~]$ dd if=/dev/zero of=/home/neird4/bigfile bs=4M count=2500    
dd: error writing '/home/neird4/bigfile': No space left on device
933+0 records in
932+0 records out
3912097792 bytes (3.9 GB, 3.6 GiB) copied, 2.28416 s, 1.7 GB/s
```

#### 🌞 Constater que la partition est pleine

```bash
[neird4@node1 ~]$ df -h /dev/rl_vbox/home 
Filesystem                Size  Used Avail Use% Mounted on
/dev/mapper/rl_vbox-home  3.9G  3.7G     0 100% /home
```

#### 🌞 Agrandir la partition

avec des commandes LVM il faut agrandir le logical volume
récupérez tout l'espace libre restant
ensuite il faudra indiquer au système de fichier ext4 que la partition a été agrandie
prouvez avec un df -h que vous avez récupéré de l'espace en plus

#### 🌞 Remplissez votre partition /home


3. Prepare another partition
Pour la suite du TP, on va préparer une dernière partition. Il devrait vous rester 20G de libre avec le disque de 40 que vous venez d'ajouter.
Cette partition contiendra des fichiers HTML pour des sites web (fictifs).
🌞 Créez une nouvelle partition

le LV doit s'appeler web

elle doit faire 20G et être formatée en ext4
il faut la monter sur /var/www


🌞 Proposez au moins une option de montage

au moment où on monte la partition (avec fstab ou la commande mount), on peut choisir des options de montage
proposez au moins une option de montage qui augmente le niveau de sécurité lors de l'utilisation de la partition
je rappelle que la partition ne contiendra que des fichiers HTML
