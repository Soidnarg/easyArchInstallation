# easyArchInstallation
Guida personale per l'installazione di arch-linux.
E' a scopo personale, ma visto che può essere utile anche a voi ve la metto pubblica.
Questa vuole essere una serie di comandi da lanciare senza guardare guide.
Consultate la documentazione ufficiale per avere i dettagli, è spaventosamente piena di informazioni e copre tutti i casi d'uso.
https://wiki.archlinux.org/title/Installation_guide

Specifiche utilizzate
Piattaforma: Oracle VM Virtualbox: https://www.virtualbox.org/
Architettura: i386
Processore: Ryzen 5 3600 (4 thread su 12 utilizzati) 
RAM: 8GB
Spazio di archiviazione: 50GB (
  BOOT: ~350MB
  SWAP: ~12GB
  ROOT: ~20GB
  //Il resto verrà usato per una partizione dati
)


# Comandi ArchLinux

**tastiera in italiano**
loadkeys i386/qwerty/it.map.gz

**attiva internet (connessione via cavo)** 
dhcpcd

**visualizzare dischi presenti**
fdisk -l

**partizionare disco disponibile (esempio /dev/sda1)**
fdisk /dev/sda1

**create new MBR table (eliminerà tutti i dati presenti sul disco)**
o
#ALTERNATIVA#
**create new GPT table (eliminerà tutti i dati presenti sul disco)**
g


**creare nuova partizione boot (only MBR)**
n

**selezionare partizione come primaria (only MBR) (l'indirizzo che volete x2)**
p

**lasciare il partition number di default 1 (only MBR)**

**start address in byte (only MBR)**
2048

**last address in byte (only MBR) (l'indirizzo che volete x2)**
700000

**creare nuova partizione swap**
n

**selezionare partizione come primaria (l'indirizzo che volete x2)**
p

**lasciare il partition number di default 2**

**start address in byte**
700416

**last address in byte (l'indirizzo che volete x2)**
2800416

**creare nuova partizione root**
n

**selezionare partizione come primaria (l'indirizzo che volete x2)**
p

**lasciare il partition number di default 3**

**start address in byte**
2801664

**last address in byte (l'indirizzo che volete x2)**
50800416

**cambia partizione 1 EFI system patition (o EFI (FAT-12/16/32)**
t
1
ef

**cambia partizione 2 in Linux swap (o Linux swap / So)**
t
2
82

**cambia partizione 3 in Linux x86-64 root (o Linux)**
t
2
83

**rendere partizione boot bootable**
a
1

**formattare partizione di root**
mkfs.ext4 /dev/sda3

**formattare partizione di swap**
mkswap /dev/sda2

**formattare partizione di boot**
mkfs.fat -F 32 /dev/sda1

**montare il file system**
mount /dev/sda3 /mnt

**montare il boot**
mount --mkdir /dev/sda1 /mnt/boot

**swap su partizione di swap**
swapon /dev/sda2

**update mirrorlist (/etc/pacman.d/mirrorlist)**
reflector

**installare pacchetti essenziali**
pacstrap -K /mnt base linux linux-firmware

**generare fstab**
genfstab -U /mnt >> /mnt/etc/fstab

**cambiare root di riferimento**
arch-chroot /mnt

**selezionare timezone**
ln -sf /usr/share/zoneinfo/Europe/Rome /etc/localtime

**installare GRUB o altro bootloader a scelta**
pacman -S grub

**UEFI only** grub-install -target=x86_64-efi -efi-directory=/boot/efi -bootloader-id=GRUB
**MBR only** grub-install -target=i386-pc /dev/sda

**crea file di configurazione grub**
grub-mkconfig -o /boot/grub/grub.cfg


**generare /etc/adjtime**
hwclock --systohc

**installare un editor (esempio nano)**
pacman -S nano

**modificare file /etc/locale.conf e decomentare en_US.UTF-8 UTF-8 ed altre lingue desiderate**
nano /etc/locale.gen

**generare lingue locali**
locale-gen

**generare file /etc/locale.conf ed inserire le lingue**
nano /etc/locale.conf

LANG=en_US.UTF-8
LANG=it_IT.UTF-8


**tastiera in italiano di default**
nano /etc/vconsole.conf

KEYMAP=it-map


**creare hostname**
nano /etc/hostname

virtualos


**cambiare password root**
passwd

