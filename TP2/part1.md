```
[neird4@vbox ~]$ file /bin/ls     
/bin/ls: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=1afdd52081d4b8b631f2986e26e69e0b275e159c, for GNU/Linux 3.2.0, stripped
[neird4@vbox ~]$ file /usr/sbin/ip
/usr/sbin/ip: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=77a2f5899f0529f27d87bb29c6b84c535739e1c7, for GNU/Linux 3.2.0, stripped
```

### B. readelf

readelf permet d'obtenir des informations sur un fichier ELF : un exécutable Linux.
De la même façon qu'un fichier texte possède des numéros de ligne quand on l'affiche, si on affiche le contenu d'un programme, chaque ligne est numérotée.
Chaque ligne du programme a donc une adresse, qui est notée en hexadécimal.
readelf permet notamment de voir de quelle adresse à quelle adresse se trouve  tell ou telle section.

#### 🌞 Utiliser readelf sur le programme ls

afficher le header du programme

il contient toutes les métadonnées principales du programme
c'est l'option readelf -h

```bash
[neird4@vbox ~]$ readelf -h /bin/ls                                                                                                                            
ELF Header:
  Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00 
  Class:                             ELF64
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              DYN (Shared object file)
  Machine:                           Advanced Micro Devices X86-64
  Version:                           0x1
  Entry point address:               0x6b10
  Start of program headers:          64 (bytes into file)
  Start of section headers:          139032 (bytes into file)
  Flags:                             0x0
  Size of this header:               64 (bytes)
  Size of program headers:           56 (bytes)
  Number of program headers:         13
  Size of section headers:           64 (bytes)
  Number of section headers:         30
  Section header string table index: 29
```

afficher la liste des sections du programme

c'est l'option readelf -S

```bash
[neird4@vbox ~]$ readelf -S /bin/ls             
There are 30 section headers, starting at offset 0x21f18:

Section Headers:
  [Nr] Name              Type             Address           Offset
       Size              EntSize          Flags  Link  Info  Align
  [ 0]                   NULL             0000000000000000  00000000
       0000000000000000  0000000000000000           0     0     0
  [ 1] .interp           PROGBITS         0000000000000318  00000318
       000000000000001c  0000000000000000   A       0     0     1
  [ 2] .note.gnu.pr[...] NOTE             0000000000000338  00000338
       0000000000000030  0000000000000000   A       0     0     8
  [ 3] .note.gnu.bu[...] NOTE             0000000000000368  00000368
       0000000000000024  0000000000000000   A       0     0     4
  [ 4] .note.ABI-tag     NOTE             000000000000038c  0000038c
       0000000000000020  0000000000000000   A       0     0     4
  [ 5] .gnu.hash         GNU_HASH         00000000000003b0  000003b0
       0000000000000040  0000000000000000   A       6     0     8
  [ 6] .dynsym           DYNSYM           00000000000003f0  000003f0
       0000000000000bb8  0000000000000018   A       7     1     8
  [ 7] .dynstr           STRTAB           0000000000000fa8  00000fa8
       00000000000005c5  0000000000000000   A       0     0     1
  [ 8] .gnu.version      VERSYM           000000000000156e  0000156e
       00000000000000fa  0000000000000002   A       6     0     2
  [ 9] .gnu.version_r    VERNEED          0000000000001668  00001668
       00000000000000c0  0000000000000000   A       7     2     8
  [10] .rela.dyn         RELA             0000000000001728  00001728
       0000000000001410  0000000000000018   A       6     0     8
  [11] .rela.plt         RELA             0000000000002b38  00002b38
       00000000000009d8  0000000000000018  AI       6    24     8
  [12] .init             PROGBITS         0000000000004000  00004000
       000000000000001b  0000000000000000  AX       0     0     4
  [13] .plt              PROGBITS         0000000000004020  00004020
       00000000000006a0  0000000000000010  AX       0     0     16
  [14] .plt.sec          PROGBITS         00000000000046c0  000046c0
       0000000000000690  0000000000000010  AX       0     0     16
  [15] .text             PROGBITS         0000000000004d50  00004d50
       0000000000012532  0000000000000000  AX       0     0     16
  [16] .fini             PROGBITS         0000000000017284  00017284
       000000000000000d  0000000000000000  AX       0     0     4
  [17] .rodata           PROGBITS         0000000000018000  00018000
       0000000000004dec  0000000000000000   A       0     0     32
  [18] .eh_frame_hdr     PROGBITS         000000000001cdec  0001cdec
       000000000000056c  0000000000000000   A       0     0     4
  [19] .eh_frame         PROGBITS         000000000001d358  0001d358
       0000000000002128  0000000000000000   A       0     0     8
  [20] .init_array       INIT_ARRAY       0000000000020f70  0001ff70
       0000000000000008  0000000000000008  WA       0     0     8
  [21] .fini_array       FINI_ARRAY       0000000000020f78  0001ff78
       0000000000000008  0000000000000008  WA       0     0     8
  [22] .data.rel.ro      PROGBITS         0000000000020f80  0001ff80
       0000000000000a98  0000000000000000  WA       0     0     32
  [23] .dynamic          DYNAMIC          0000000000021a18  00020a18
       0000000000000210  0000000000000010  WA       7     0     8
  [24] .got              PROGBITS         0000000000021c28  00020c28
       00000000000003c8  0000000000000008  WA       0     0     8
  [25] .data             PROGBITS         0000000000022000  00021000
       0000000000000278  0000000000000000  WA       0     0     32
  [26] .bss              NOBITS           0000000000022280  00021278
       00000000000012c0  0000000000000000  WA       0     0     32
  [27] .gnu_debuglink    PROGBITS         0000000000000000  00021278
       0000000000000020  0000000000000000           0     0     4
  [28] .gnu_debugdata    PROGBITS         0000000000000000  00021298
       0000000000000b58  0000000000000000           0     0     1
  [29] .shstrtab         STRTAB           0000000000000000  00021df0
       0000000000000128  0000000000000000           0     0     1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings), I (info),
  L (link order), O (extra OS processing required), G (group), T (TLS),
  C (compressed), x (unknown), o (OS specific), E (exclude),
  l (large), p (processor specific)
```

déterminer à quel adresse commence le code du programme

pour rappel, le code est dans la section .text

vous devriez voir cette adresse dans la sortie de readelf -S

```bash
[neird4@vbox ~]$ readelf -S /bin/ls | grep .text
  [15] .text             PROGBITS         0000000000004d50  00004d50
```

### C. ldd

ldd est un outil qui permet de manipuler le dynamic linker de Linux. Le dynamic linker c'est un programme qui s'occupe de trouver les librairies nécessaires quand un autre programme se lance.
On peut utiliser ldd notamment pour visualiser de quelle librairie a besoin un programme donné.

#### 🌞 Utiliser ldd sur le programme ls

afficher la liste des librairies que va utiliser ls pendant son fonctionnement
déterminer, parmi ces librairies, laquelle est la Glibc

```bash
[neird4@vbox ~]$ ldd /bin/ls
	linux-vdso.so.1 (0x00007ffcd0181000)
	libselinux.so.1 => /lib64/libselinux.so.1 (0x00007fcd35de6000)
	libcap.so.2 => /lib64/libcap.so.2 (0x00007fcd35ddc000)
	libc.so.6 => /lib64/libc.so.6 (0x00007fcd35a00000)
	libpcre2-8.so.0 => /lib64/libpcre2-8.so.0 (0x00007fcd35d40000)
	/lib64/ld-linux-x86-64.so.2 (0x00007fcd35e3d000)
```


### 2. Syscalls basics

### A. Syscall list

Vous pourrez trouver une liste des syscalls Linux sur un système x86_64 iciiii.

#### 🌞 Donner le nom ET l'identifiant unique d'un syscall qui permet à un processus de...

Lire un fichier stocké sur disque :
- read
- 0

Écrire dans un fichier stocké sur disque :
- write
- 1

Lancer un nouveau processus :
- fork
- 57

### B. objdump

objdump permet de désassembler un programme, c'est à dire d'afficher le code contenu par un exécutable, sous forme de langage assembleur compréhensible par les humains (un peu, beaucoup plus qu'une purée d'octets en tout cas !)
#### 🌞 Utiliser objdump sur la commande ls

afficher le contenu de la section .text

je vous laisse trouver la commande sur l'internet :D


mettez en évidence quelques lignes qui contiennent l'instruction call
il devrait y en avoir plusieurs
chaque call est un appel à une fonction, potentiellement importée via une librairie

```bash
[neird4@vbox ~]$ objdump -d -j .text /bin/ls | grep call | head
    4d51:	e8 da f9 ff ff       	callq  4730 <abort@plt>
    4d56:	e8 d5 f9 ff ff       	callq  4730 <abort@plt>
    4d5b:	e8 d0 f9 ff ff       	callq  4730 <abort@plt>
    4d60:	e8 cb f9 ff ff       	callq  4730 <abort@plt>
    4d65:	e8 c6 f9 ff ff       	callq  4730 <abort@plt>
    4d6a:	e8 c1 f9 ff ff       	callq  4730 <abort@plt>
    4d6f:	e8 bc f9 ff ff       	callq  4730 <abort@plt>
    4d74:	e8 b7 f9 ff ff       	callq  4730 <abort@plt>
    4d79:	e8 b2 f9 ff ff       	callq  4730 <abort@plt>
    4dbc:	e8 6f fb ff ff       	callq  4930 <strrchr@plt>
```

mettez en évidence quelques lignes qui contiennent l'instruction syscall

il y en a aucune normalement : ls ne contient pas directement de syscalls
car il importe la Glibc, qui contient des syscalls, et les appelle avec call

#### 🌞 Utiliser objdump sur la librairie Glibc

vous avez repéré son chemin exact au point d'avant avec ldd
mettez en évidence quelques lignes qui contiennent l'instruction syscall

il devrait y en avoir pas mal
chaque ligne qui contient l'instruction syscall est la dernière d'un bloc de code qui est le syscall lui-même

```bash
[neird4@vbox ~]$ objdump -d /lib64/libc.so.6 | grep syscall | head      
   295f4:	0f 05                	syscall 
   3e737:	0f 05                	syscall 
   3e801:	0f 05                	syscall 
   3e969:	0f 05                	syscall 
   3e99e:	0f 05                	syscall 
   3e9ea:	0f 05                	syscall 
   3ea18:	0f 05                	syscall 
   3ef49:	0f 05                	syscall 
   3f3b6:	0f 05                	syscall 
   3f418:	0f 05                	syscall
```

trouvez l'instrution syscall qui exécute le syscall close()

Pour exécuter un syscall, le programme met dans le registre eax l'identifiant du syscall (avec l'instruction mov) puis exécute l'instruction syscall. Vous cherchez donc une instruction syscall précédé d'un mov qui met l'identifiant de close() dans eax.

```bash

