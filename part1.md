# user  
---------------------------------------------------------
```
groupadd linux
mkdir /home-mc/posein
useradd posein -d /home-mc/posein -g linux

useradd -D
useradd -D -b /home-mc -s /bin/zsh -e 2021-12-31
/etc/default/useradd 	#home, shell /bin/dash directory.

/etc/login.defs 	#UID/GID, encrypt information. 
/etc/skel       	#Diretory containing default files.

usermod -L posein #Lock
usermod -l new_name old_name (-m)
usermod -g linux posein
usermod -s /bin/zsh posein
usermod -d /home-mc/posein -m posein

passwd -S posein
passwd -l posein 	#lock
passwd -u posein 	#unlock
passwd -d posein 	#remove passwd only , login ok
passwd -e posein 	#must change next-time password
passwd -n 10 -x 100 -w 5 -i 5 posein

chage -m 10 -M 100 -W 5 -I 3 posein #min,MAX,warnning,inactvie
chage -E 2021-12-31 posein

/etc/passwd
username:password:UID:GID:fullname:home-directory:shell

/etc/shadow
username:password:last:may:must:warn:expire:disable:reserved

visudo #/etc/sudoers
posein ALL=ALL

grpconv 		#make /etc/gshadow
grpck 			#group check
groupadd/groupdel
groupmod -n new_group old_group
gpasswd -A posein terran

newgrp 			#temporary group change , exit

who / whoami / w
uname -r 		#kernel release information 

last #login/reboot information 
last posein
last reboot
last -f /var/log/wtmp 	#wtmp binary

lastb 			#login fail information
lastb -f /var/log/btmp 	#btmp binary

lastlog #last login information for each user
lastlog -u posein
lastlog -t 3 		#inner 3day
lastlog -b 3 		#before 3day
```
========================================================


# owner permission  
---------------------------------------------------------
```
Set-UID 4 100 #temporary apply sudo,  rwx -> rws or rwS
Set-GID 2 010 #temporary apply group, rwx -> rwx or rwS
Sticky  1 001 #only make (no delete)  rwx -> rwt or rwT

chmod 1777 tmp
chomd o+t tmp or chmod o-t tmp
chmod 3070 /project

umask 022
defualt directory 777 - 022 = 755 drwxr-xr-x
default file      666 - 022 = 644 -rw-r--r--
```
========================================================


# file system
---------------------------------------------------------
```
ext2(2GB), ext3(16TB), ext4(1EB), xfs(8EB), vfat(win FAT32), nfs, cifs
mount -t ext4 -o ro /dev/sdb1 /mmn
mount -t cifs -o useranme=admin,pasword='1234' //192.168.1.35/data /net_drive

mount           	#sysfs on /sys type sysfs (fw,...
cat /etc/mtab   	#sysfs /sys sysfs rw ...


fdisk -l  #including USB, cat /proc/partitions
fdisk /dev/sdb1

mkfs -t xfs /dev/sdb1
mke2fs -j /dev/sdb1
mke2fs -t ext4 /dev/sdc1
mke2fs -j -b 4096 -R stride=32 /dev/md0 #raid md0,  block size 4096, 32byte/stripe

fsck /dev/sdb1
e2fsck -y /dev/sda3 	#forced yes

df -hT
du -h --max-depth=1
du -sh ~posein

dd if=a.txt conv=ucase of=b.txt #upper case
dd if=/dev/sda of=/media/disk1.imgs bs=1M couont=620
dd if=/dev/sda of=/media/disk2.imgs bs=1M couont=620 skip=621
dd if=/dev/zero of=/dev/sda7 #initial sda7

stat /etc/passwd
stat -f /etc/passwd

vi /etc/fstab -> UUID /mnt xfs uquota 0 0

blkid #block device UUID LABEL check

*SWAP
mkswap /swap-file 10240 #as memory
mkswap -c /dev/sdb2 	#mk swap patition after checking bad block
swapon swap-file 	#enable 
swapon -a        	#enable all
swapoff -a       	#disable all

free 			#/proc/meminfo

*MAKE-SWAP FILE
dd if=/dev/zero of=/swap-file bs=1ks count=1024000 #1GB file
mkswap /swap-file
swapon /swap-file
vi /etc/fstab -> /swap-file swap swap defaults 0 0

*MAKE-SWAP PARTITION
fdisk /dev/sdb #'t' swapcode 82 -> /dev/sdb2
mkswap -c /dev/sdb2
swapon /dev/sdb2
vi /etc/fstab -> /dev/sdb2 swap swap defaults 0 0

*Quota
edquota posein			#assgin disk
edquota -t 			#grace period
edquota -p posein yuloje 	#posein -> yuloje
quota yuloje			#see disk info

setquota -u yuloje 10000 11000 0 0 /home #soft 10MB, hard 11MB, 
setquota -t 86400 28800 /home #limit 1day, I-node 8 hour

vi /etc/fstab -> filed4: usrquota 
mount -o remount /home
quotacheck /home 	#make file
quotacheck -mf /home
edquota posein   	#group: edquota -g terran
quotaon /home
repquota /home   	#group: repquota -g /home 

*LINK
ln src.file hard-link.file
ln -s src.file soft-link.file
```
========================================================


# processor manage  
---------------------------------------------------------
```
ps -ef
ps aux

pstree

top -d 2 -p PID

jobs #backgroup process
fg
bg

nice --10 bash 		#add from bash
renice 10 PID  		#direct change

nohub tar cvf source.tar /opt/src &

pgrep httpd
pgrep -u posein,yuloje

pkill -9 -u yuloje

ls -l /proc/PID/exe
/proc/cpuinfo
/proc/meminfo
/proc/mdstat 				#raid
/proc/verstion				#uname -r
/proc/partitions

crontab -e -u posein		
0/10 * * * * /etch/10mintue.sh		#* * * * * #min hour day month week
```
========================================================


# inatall (configure -> make -> make install)  
---------------------------------------------------------
```
tar cvf posein.tar posein/
tar Jxvf php.tar.xz -C /usr/local/src

zip posein posein.tar
unzip posein.zip

gcc -o my_cal main.c sum.c

rpm -Uhv my.rpm			#install
rpm -e eog			#erase
rpm -e httpd --nodeps		#remove depdency

rpm -qa				#query all
rpm -qi sendmail		#info
rpm -ql	sendmail		#list
rpm -qc sendmail		#config
rpm -qR sendmail		#dependency list
rpm -qf /usr/sbin/sendmail 	#where from
ldd -v /usr/sbin/sendmail	#check shared libarary

rpm -qip sendmail.rpm
rpm -qlp sendmail.rpm

yum install telnet-server
yum remove telnet-server

libarary
/etc/ld.so.conf
ldconfig -p 
```
========================================================


# Module  
---------------------------------------------------------
```
/lib/modules/3...../kernel/dirvers/acpi
lsmod
insmod cdrom.ko
rmmod cdrom

modprobe cdrom			#depdency ok
modinfo cdrom.ko

depmod				#modules.dep update
/lib/modules/3...../modules.dep

*kernel compile
tar Jxvf linux.xz
cd linux
make clean			#*.o
make distclean			#all remove

make mrproper			#initial: *.o, *.conf, *.bak
make menuconfig
make bzImage			#kernel image
make modules
make modules_install
make install
```
========================================================


# addtional device  
---------------------------------------------------------
```
*add disk
fdisk -l
fdisk /dev/sdb
reboot
mkfs -t ex4 /dev/sdb1
mkdir /backup
mount -t ext4 /dev/sdb1 /backup
vi /etc/fstab
/dev/sdb1 /backup ext4 defaults 1 1

*print
lpr -# 2 -P ML-2070 part1.txt
lp -n 2 -P ML-2070 part1.txt
```
========================================================


# rsyslog  
---------------------------------------------------------
```
service rsyslog restart
/etc/rsyslog.conf
facility.priority action	#facility: cron, auth, authpriv, kern, mail
				#priority: none, debug, info, notice, warn, err, crit, alert, emerg, panic
				#action: file, @host(UDP), @@host(TCP), :omusrmsg:

*.=crit;kern.none	/var/log/critical
*.emerg			:omusrmsg:*
authpriv.*		:omusrmsg:root,posein
authpriv.*		/dev/tty2
mail.*;mail.!=info	/var/log/maillog
uucp,news.crit		/var/log/news

logrotate -f /etc/logrotate.conf

/var/log/httpd/access.log{
	rotate 5
	mail posein@gmail.com
	size 100k
	missingok
	dateext
	postrotate
		/usr/bin/killall -HUP httpd
	endscript
}

/var/log/secure		#auth log
/var/log/maillog	#sendmail, dovecot
/var/log/dmesg		#boot log
```
========================================================


# security  
---------------------------------------------------------
```
grub password
grub
md5crypt
vi /boot/grub/grub.conf
password --md5

*sysctl		#kernel control (/proc/sys_
echo 0 > /proc/sys/net/ipv4/icmp_echo_ignore_all
ping -c 2 localhost

sysctl -a		#print all parameter
sysctl -p		#/etc/sysctl.conf
sysctl -n net.ipv4.icmp_echo_ignore_all
sysctl -w net.ipv4.icmp_echo_ignore_all=0

*ssh
*SERVER-SIDE
service sshd start
/etc/ssh/sshd_config

*CLIENT-SIDE
ssh -p 22 root@192.168.1.35
ssh-keygen 		#id_rsa, id_rsa.pub -> copy server .ssh/authorized_keys

lsattr /etc/passwd
chattr +a /var/log/messages	#append only
chattr +i /etc/services		#ignore: not change
chattr -i +a /etc/services

acl
getfacl	/etc/passwd		#file access control list
setfacl	-m u:posein:rw test.txt

nmap -O -p 1-1024 localhost
```
========================================================


# backup  
---------------------------------------------------------
```
*tar
tar cvfp home.tar /home
tar xvf home.tar

tar -g list -cvfp home.tar /home
tar xvf home.tar -C

tar -N '13 Oct 2021' -cvf home.tar /home

tar cvfz /home | split -b 10m - home.tar.gz
cat home.tar.gz | tar zxvf -

*cpio
find /home | cpio -ocv > home.cpio
cpio -icvd < home.cpio

ls *.conf | cpio -ocv > conf.cpio
cpio -icvt < conf.cpio

*dump/restore
dump -0u -f backup.dump /dev/sda7
restore -rf backup.dump

*dd
dd if=/dev/sda1 of=/dev/sdb1 bs=1k

*rsync
rsync -av /home /home5			#/home -> /home5
rsync -avz 192.168.1.35:/home /backup
rsync -avz -e ssh root@192.168.1.35:/home /backup
rsync -av /home 192.168.1.35:/backup
```
========================================================
