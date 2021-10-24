# linuxmaster

### usefull keyword
[apropos](https://en.wikipedia.org/wiki/Apropos_(Unix))  
man -k keyword  

실기요약 https://las311.tistory.com/16  


### centos virtualbox 해상도조정  
/etc/grub2.cfg 에서 linuz 검색하여 맨 끝에 vga=773 으로 변경하여 재부팅  
773: 1024x768  
775: 1280x1024  

### centos yum 이슈시  
yum install epel-release  
or  
yum clean all & yum clean metadata  


### centos 한글(일반적인 방법- /etc/locale.conf가 수정됨, [TUI에서는 한글깨진다고함](https://ipex.tistory.com/entry/CentOS7-TextMode-%EC%97%90%EC%84%9C-%ED%95%9C%EA%B8%80-%EA%B9%A8%EC%A7%90-%ED%98%84%EC%83%81?category=771640)    
locale  
localectl list-locale | grep ko  
localectl set-locale LANG=ko_KR.utf8  
source /etc/locale.conf  
locale  


외부 ssh 로 접속시 정상적으로 보임  
ko_KR.euckr 까지 안되면, 터미널 뷰 자체의 문제일 수 있음  

### ssh 방화벽설정 centos7  
rpm -qa | grep ssh  
filewall-cmd --permanent --list-all --zone=dmz  
firewall-cmd --permanent --zone=public --add-port=22/tcp  
virtualbox forwarding  
host(윈도우에서 확인) ipconfig  
guest(centos에서 확인) hostname -I (주로 10.0.2.15  )

공유기 forwarding (tcp  )


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
