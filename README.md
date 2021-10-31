# linuxmaster

### usefull keyword
[apropos](https://en.wikipedia.org/wiki/Apropos_(Unix))  
man -k keyword  
man 5 command
man 8 command
read with space bar
document /usr/share/doc  

실기요약 https://las311.tistory.com/16  


### centos virtualbox 해상도조정  
/etc/grub2.cfg 에서 linuz 검색하여 맨 끝에 vga=773 으로 변경하여 재부팅  
773: 1024x768  
775: 1280x1024  

### centos yum 이슈시  
yum install epel-release  
or  
yum clean all & yum clean metadata  


### centos 한글(일반적인 방법- /etc/locale.conf가 수정됨) [TUI에서는 한글깨진다고함](https://ipex.tistory.com/entry/CentOS7-TextMode-%EC%97%90%EC%84%9C-%ED%95%9C%EA%B8%80-%EA%B9%A8%EC%A7%90-%ED%98%84%EC%83%81?category=771640)
locale  
localectl list-locale | grep ko  
localectl set-locale LANG=ko_KR.utf8  
source /etc/locale.conf  
locale  

외부 ssh 로 접속시 정상적으로 보임  
ko_KR.euckr 까지 안되면, 터미널 뷰 자체의 문제일 수 있음  

### [root 암호 응급복구](https://it-serial.com/entry/Linux-Grub%EA%B0%9C%EB%85%90%EC%95%94%ED%98%B8-%EC%84%A4%EC%A0%95-%EC%9D%91%EA%B8%89-%EB%B3%B5%EA%B5%ACroot-%EB%B9%84%EB%B0%80%EB%B2%88%ED%98%B8-%EC%B0%BE%EA%B8%B0)
```
1. 부팅화면에서 'e' 누른다.

2. 내용 중, linux16에서
ro -> rw
rhgb quiet LANG=en_US.UTF-8 -> init /bin/sh 
로 바꾸고,
'ctrl-x' 누른다.

3. vi /etc/sysconfig/selinux 에서
SELINUX=disable
로 바꾼다.

4. passwd 로 신규암호 입력한다.

5. /sbin/reboot -f 재부팅 한다.

6.  vi /etc/sysconfig/selinux 에서
SELINUX = enforcing 
로 복구한다.
```

### ssh 방화벽설정 centos7  
rpm -qa | grep ssh  
filewall-cmd --permanent --list-all --zone=dmz  
firewall-cmd --permanent --zone=public --add-port=22/tcp  
virtualbox forwarding  
host(윈도우에서 확인) ipconfig  
guest(centos에서 확인) hostname -I (주로 10.0.2.15  )

공유기 forwarding (tcp  )

### ssh port change  
port list: cat /etc/services  
acess: ssh -vvvvvv -p 443 id@host  

ssh port add example : 443  
```
#SElinux
  semanage port -m -t ssh_port_t -p tcp 443
  semange port -l | grep ssh

#Firewall (for CentOS 7, reject is --zone=drop)
  firewall-cmd --add-port=443/tcp --permanent --zone=public
  firewall-cmd --reload
  firewall-cmd --list-all (or --list-all-zones)

#Firewall (for CentOS 6)
  iptables -I INPUT 5 -p tcp -m state --state NEW --dport 443 -j ACCEPT
  iptables-save

# etc/ssh/sshd_config (Add 443, check X11Forwarding yes)
  sed -i -e "s/\# *Port 22/Port 22\nPort 443/"/etc/ssh/sshd_config
  systemctl reload sshd
  systemctl restart sshd (or service restart sshd)

# netstat
   netstat -anlp | grep sshd
   
   
*RSA (2048 bit)
*Client generate
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub

*Server register (setting /etc/ssh/sshd_config)
~/.ssh/authorized_keys
```

### how to copy on tmux screen  
```
C-b [  
spacebar
...
enter
C-b ]
```

tmux set-option -g mode-keys vi   
or  
set-option -g mode-keys vi   
at ~/.tmux.conf  
source-file ~/.tmux.conf  
on
