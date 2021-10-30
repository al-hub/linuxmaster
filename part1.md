# user
---------------------------------------------------------
groupadd linux
mkdir /home-mc/posein
useradd posein -d /home-mc/posein -g linux

useradd -D
useradd -D -b /home-mc -s /bin/zsh -e 2021-12-31
cat /etc/default/useradd #home directory
cat /etc/login.defs #UID/GID, encrypt information etc
cat /etc/skel       #addtional directory skeletone

usermod -L posein #Lock
usermod -l new_name old_name
usermod -g linux posein
usermod -s /bin/zsh posein
usermod -d /home-mc/posein -m posein

passwd -S posein
passwd -l posein #lock
passwd -u posein #unlock
passwd -d posein #remove passwd only , login ok
passwd -e posein #end
passwd -n 10 -x 100 -w 5 -i 5 posein

chage -m 10 -M 100 -W 5 -I 3 posein #min,MAX,warnning,inactvie
chage -E 2021-12-31 posein

/etc/passwd
username:password:UID:GID:fullname:home-directory:shell

/etc/shawdow
username:password:last:may:must:warn:expire:disable:reserved

grpconv #make /etc/gshadow
grpck #group check
groupadd/groupdel
groupmod -n new_group old_group
gpasswd -A posein terran

newgrp #temporary group change , exit

who / whoami / w
uname -r #kernel release information 

last #login/reboot information 
last posein
last reboot
last -f /var/log/wtmp #wtmp binary

lastb #login fail information
lastb -f /var/log/btmp #btmp binary

lastlog #last login information for each user
lastlog -u posein
lastlog -t 3 # inner 3day
lastlog -b 3 # before 3day
========================================================

