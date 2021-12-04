# linuxmaster

### do you know the meaining of linux command?  
umask 022    
chmod 1660  
grub  

### structure of directory
```
/bin 기본명령어
/etc 설정
/usr 응용프로그램
/proc 프로세스
/var/log 동적파일로그
/lib 응응프로그램(ldd , .... module.dep)
/dev 장치 드라이브 (가상디렉터리 )
```

### usefull keyword for me
[apropos](https://en.wikipedia.org/wiki/Apropos_(Unix))  
man -k keyword  
man 5 command  
man 8 command  
read with space bar  
document /usr/share/doc    

```
ssh -p 22 id@host
netstat -alp
vmstat

ldd /bin/cat
rpm -qf
    -qi
    
nohup &
smbclient -L //host/share

mkswap 하드디스크를 메모리처럼

crontab
*/10 * * * * 

/proc/
/var/log/

fdisk -l
```
### 암기  
네트웍표기법  
sendmail    192.168.1  
xinetd, DNS 192.168.1.0/25  
else        192.168.1.0/255.255.255.0  



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
VMWARE/VirtualPC forwarding 확인필요!

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


### printer setting [CUPS](https://zosystem.tistory.com/196)
```
vi /etc/cups/cupsd.conf
<Location />
  Order allow,deny
  Allow 192.168.1.0/24
</Location>

systemctl status cups

firewall-cmd --permanent --add-service=ipp
firewall-reload

yum install splix (samsung printer)

lpinfo --make-and-model samsung -m | grep my_priner

lpr -P NAME -H 192.168.1.69:631 document.txt
```

### refence site 
[개념잡기](https://wiseworld.tistory.com/category/%EA%B3%B5%EB%B6%80%EB%B0%A9/%EB%A6%AC%EB%88%85%EC%8A%A4)  
[실기요약](https://las311.tistory.com/16)  
[기출풀기](https://q.fran.kr/%ec%8b%9c%ed%97%98/%eb%a6%ac%eb%88%85%ec%8a%a4%eb%a7%88%ec%8a%a4%ed%84%b0%201%ea%b8%89)  
[기출해설](https://m.blog.naver.com/is_king?categoryNo=37&proxyReferer=)  
[LPIC](https://lpickorea.org/)  


### trend(22)
```
트렌트코리아(22) - 최지혜
(빅데이터 분석 : 코난테크놀로지-펄스k 활용 : 겹치는게 적음)

post 코로나
문화 충격 이론 (2년 지나면 익숙해 짐 - 변화에 적응하는 시간)
-. 결론: 이분법적 사고를 하지 말 것
          내가 알고 있는 것을 너는 모른것이 트렌드다.     
        내 취향이 아닌 contents 추천 받는것도 즐거워 한다.
        (내 취향 우물안의 개구리 , 실증이 남 , 우연의 발견 고민)

-. 10 keyword (거시적인것은 여기서 다루지 않음)
나노사회:
다수가 즐길수 있는 contents가 적어짐
(오징어게임은 넥플릭스 ott 서비스를 즐기고 있는 사람들)
(같은 contents라도 다른 시간대의 내용을 즐김)
(세대간의 공감력 회복 중요 - 드디어 이해하게 되었다.)

머니러쉬
/로 확장 해 나감
/그림, 음악투자 -> mz 눈이 높아졌다. -> 프리미엄시장 성장
/ generation -> 겸직을 하고 싶어하는 z 세대 (공론의 타이밍)

득템력
소비자들끼리 경쟁하도록 만든다.
(수량을 제한하는 limit 마케팅 = hunger 마케팅)
(나이키 10개 운동화)
(돈, 시간, 수고가 있어야 함)
(브랜드의 기획력을 오랜시간 동안 쌓아와야 함 - 아티스트와 노력)

러스틱라이프(5도2촌 - 공유office workation-일과 휴가병행)
시골스러움이 hip 해지고 있음 (코로나의 영향 -> 힐링?)
시골학교 유학 , 농막유행(나의펜션, 1년기다려야함)

헬시플레저(건강을 즐겁게)
어다행다: 어짜피다이어트할것행복하게다이어트하자
밀가루가안들어간 케익, 칼로리가낮은 아이스크림
오쏘몰(비타민)-효과24간내, 미리미리(탄력크림,탈모예방)

엑스틴이즈백
x세대자식z
x세대:playingcoach 
자본성장의어른모습필요

바른생활루틴이
유튭 mz - 운동완료 대댓글 
selfbinding (물을 마시는 습관)
-> 계속되는 자유도를 좋아하는 것은 아니다!! 증명

실재감테크
live commers, 가상인퓰런스(완벽한미인x, 성격라이프스탈일)
untact , meta 

likecommers(좋아요 제조,마케팅,유통대행)
최소 제작수량이 낮아졌다.
나를 follower해주는 사람들에게 판매할 수 있다.
스마트스토어 만드는 것은 어렵지 않고, 홍보를 하는게 어렵다.
follower 가있으면 홍보+판매할 수 있다.
아이디어 기획력만 있다면 제조 할 수 있다.

내러티브 자본 (Narrative)
진정성을 가진 다른 점  (테슬라)
```
