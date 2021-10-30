#apache (httpd.conf)
---------------------------------------------------------
httpd -t
httpd -l
httpd -S
httpd -k gracefull ( service httpd gracefull , apachectl graceful )

htpasswd -c /etc/password posein
htpasswd /usr/local/apache/conf/password yuloje
/usr/local/apache/htdocs/admin/.htaccess

httpd.conf
LoadModule userdir_module modules/mod_userdir.so
Include conf/extra/httpd-userdir.conf

ServerName www.ihd.or.kr:80
ServerRoot "/www"
ServerAdmin webadmin@example.com
DocumentRoot "/usr/local/apache/html"
DirectoryIndex index.html index.htm index.php
Listen 8080

UserDir www

AllowOverride AuthConfig

Alias /cgi-bin/ "/var/www/cgi-bin/"
NameVirtualHost 192.168.5.13:80

<Directory "/wwww/ihd/admin">
	Order Deny,Allow
	Deny from All
	Allow from 192.168.22.
</Directory>

ErrorDocument 403 /forbidden.html
ErrorDocument 401 /unauth.html
ErrorDocument 404 /not_found.html
=========================================================


#NIS (Network Information Service, LDAP) for user,passwd
---------------------------------------------------------
*SEVER-SIDE
service rpcbind start

service ypserv start
service yppasswd start
service ypxfrd start

nisdomainname test.co.kr 
(or set: /etc/sysconfig/network NISDOMAIN=test.co.kr)

(update: make -C /var/yp )


*CLIENT-SIDE
service ypbind start
yp-tools

ypwhich 
ypwhich -m
ypcat map_file
ypcat hosts
ypcat passwd
yptest 
yppasswd user_name

/etc/yp.conf
server nis.test.co.kr
ypserver nis.test.co.kr
domain test.co.kr
=========================================================


#SAMBA
---------------------------------------------------------
*SEVER-SIDE
/etc/rc.d/init.d/smb start
service smb restart

/etc/samba/smb.conf
[global]
	workgroup = SHARE_GROUP_NAME
	server string = explain server
	netbios name = CONNECT_NAME_FROM_WINDOW
	interfaces = lo eth0 192.168.12.2/24
	hosts allow = 127. 192.168.1.
	hosts allow = posein_pc, yuloje_pc

	log file = /var/log/samba/log.%m
	max log size = 50
	security = user
	passdb backend = tdbsam
[share]
	comment = comment message
	path = /tmp
	read only = No
	writable = yes
	valid users = posein yuloje
	public = yes
	write list = @insa

'127.' is localsystem


*CLIENT-SIDE
smbclient -L 192.168.1.1 -U root@1234
smbclient //192.168.1.1/share
smbstatus
testparm
testparm /etc/samba/smb.conf wwww 192.168.1.1
nmblookup -U 192.168.1.1 -R 'NAME'
nmblookup '*'
mount.cifs //192.168.1.1/photo /mnt
smbpasswd -a posein
smbpasswd posein
smbpasswd -x posein
smbpasswd -d posein
pdbedit -a posein
pdbedit -L -v
=========================================================


#NFS (Newtwork File System)
---------------------------------------------------------
*SERVER-SIDE
service rpcbind start
service nfs start

rpcinfo
rpcinfo -s 192.168.1.1

/etc/exports
/nfs_director1 192.168.1.1
/nfs_director2 192.168.1.0/255.255.255.0(rw,root_squash)
/nfs_director3 192.168.12.0/24(rw,no_root_squash)
/nfs_nobody *.public.com(rw,all_squash)

exportfs

showmount
showmount -e
showmount -e 192.168.1.1

nfsstat (SERVER/CLIENT all-side)


*CLIENT-SIDE
mount -t nfs 192.168.1.1:/nfs_directory1 /mnt
mount 192.168.1.1:/nfs_directory2 /mnt

/etc/fstab
192.168.1.1:/nfs_diretory3 /mnt nfs timeo=15,soft,retrans=3 0 0
=========================================================


#vsftpd (FTP)
---------------------------------------------------------
*SERVER-SIDE
service vsftpd start

/etc/vsftpd/vsftpd.conf
anonymous_enable=YES
local_enable=YESE
anon_upload_enable=NO
anon_mkdir_write_enable=NO
chroot_local_user=YES
data_connection_timeout=120
local_umask=002
listen_port=21
xferlog_enable=YES

userlist_deny Default: YES
/etc/vsftpd/user_list
=========================================================


#sendmail
---------------------------------------------------------
*SERVER-SIDE
yum install sendmail
yum install devecot
service sendmail start

/etc/mail/sendmail.cf

/etc/mail/local-host-names
posein.org
posein.co.kr

m4 /etc/mail/sendmail.mc > /etc/mail/sendmail.cf

/etc/mail/access
From:spammer@aol.com REJECT
Connect:192.168.5 DISCARD
To:posein.org RELAY

makemap hash /etc/mail/access < /etc/mail<access

/etc/aliases
webmaster: posein, yuloje, posein@naver.com
admin::include:/etc/mail/admgroup

newaliases
sendmail -bi
sendmail -I

1_SERVER-N_domain
/etc/mail/virtusertable
ceo@linux.com posein
ceo@window.com yuloje
@posein.org posein@naver.com

makemap hash /etc/mail/virusertable < /etc/mail/virtusertable

each user forwarding
vi ~/.forward
posein@naver.com
posein@gmail.com
chmod 600 .forward
=========================================================


#DNS
---------------------------------------------------------
*SERVER-SIDE
service named start

/etc/named.conf
acl "member" {192.168.1.32; 192.168.1.35; 192.168.1/24;};
options {
	directory	"/var/named";
	allow-transter {192.168.0/24;};
	forward only; (or first)
	forwarder	{203.247.32.31;};
	allow-query	{member;};
};

//root domain server, hint is root zone server
zone "." IN {
	type hint; 
	file "named.ca";
};

zone "linux.or.kr" IN {
	type master;
	file "linux.zone";
};

zone "1.168.192.in-addr.arpa" IN {
	type master;
	file "linux.rev";
};


chown root.named linux.zone
chmod 640 linux.zone

linux.zone
$TTL 1D
@       IN SOA  ns.linux.or.kr. posein.linux.or.kr. (
                                        0       ; serial
                                        1D      ; refresh
                                        1H      ; retry
                                        1W      ; expire
                                        3H )    ; minimum
        NS      ns.linux.or.kr.
        A       192.168.1.35
	MX	10
ns	A	192.168.1.35
www	A	192.168.1.35
www1	CNAME	www
www1	CNAME	www


linux.rev
$TTL 1D
@       IN SOA  ns.linux.or.kr. posein.linux.or.kr. (
                                        0       ; serial
                                        1D      ; refresh
                                        1H      ; retry
                                        1W      ; expire
                                        3H )    ; minimum
        NS      ns.linux.or.kr.
15	PTR	linux.or.kr.
15	PTR	ns.linux.or.kr.
15	PTR	www.linux.or.kr.

A:IPv4 address
AAAA:IPv6 address
NS:domain nameserver
MX:mail exchange server, number is priority
CNAME: alias

PTR : ip address to domain for reverse zone

named-checkconf /etc/named.conf
named-checkzone www /var/named/named.localhost
=========================================================


#TCP wrapper , xinetd
---------------------------------------------------------
/etc/hosts.deny <- work later
ALL : ALL

/etc/hosts.allow <- work first
ALL : localhost, .posein.org
in.telnetd : 192.168.1.35
sshd : .posein.com EXCEPT cracker.posein.com
ALL EXCEPT vstfpd : .ihd.or.kr EXCEPT bad.ihd.or.kr
in.telnetd, vsftpd : 192.168.1., .snu.kr
ALL : ALL : DENY

/etc/xinetd.conf
defaults
{
	log_type = FILE /var/log/xinetd.log
	log_on_failure = HOST (or USERID or ATTEMPT)
	log_on_success = PID HOST USERID EXIT DURATION TRAFFIC

	cps = 50 10   (50 is #req, 10 is sec )

	instances = #MAX demon of xinted 

	per_source = #MAX service from IP

	only_from = 192.169.5.13 192.168.1.0/24

	no_access = 192.168.1.35

	enable = telnet rlogin
	disable = rsync
}

service telent
{
	disable	= no (no is enable)
	flags	= REUSE
	socket_type	= stream (or dgram, raw, seqpacket)
	wait	= no (no is multi-thread)
	user	= root
	server	= /usr/sbin/in.telnetd
	log_on_failure	+= USERID
	access_times	= 01:00-07:00
	redirect	= 192.168.12.22 23
	port	= 8080
	nice	= 10
}
=========================================================


#Squid ( proxy )
---------------------------------------------------------
/etc/squid/squid.conf
http_port 3128
cache_dir ufs /var/spool/squid 100 16 256 
#100 is MB, 16 is directory, 256 is sub-directory

#ex1: approve
acl posein src 192.168.4.0/255.255.255.0

http_access allow posein
http_access deny all

#ex2: deny
acl cracker src 192.168.3.0/255.255.255.0

http_acess deny cracker
http_acess allow all

#ex3: deny from domain
acl cracker srcdomain .cracker.org

http_acess deny cracker
http_acess allow all

#ex4: approve from domain
acl example srcdomain .example.com

http_access allow .example.com
http_access deny all

#ex5: block site
acl exploit dstdomain .exploit-db.com

http_access deny exploit
http_access allow all
=========================================================


#DHCP ( Dynamic Host Configuration Protocol )
---------------------------------------------------------
/etc/dhcp/dhcpd.conf

service dhcpd start
subnet 10.5.5.0 netmask 255.255.255.224 {
  range 10.5.5.26 10.5.5.30;
  option domain-name-servers ns1.internal.example.org;
  option domain-name "internal.example.org";
  option routers 10.5.5.1;
  option broadcast-address 10.5.5.31;
  default-lease-time 600;
  max-lease-time 7200;
}

host passacaglia {
  hardware ethernet 0:0:c0:5d:bd:95;
  filename "vmunix.passacaglia";
  server-name "toccata.fugue.com";
}
=========================================================


#VNC , NTP
---------------------------------------------------------
/etc/sysconfig/vncservers
VNCSERVERS="2:root"
VNCSERVERARGS[2]="-geoetry 1024x768"

vncpasswd
service vncserver start

/etc/ntp.conf
driftfile /var/lib/ntp/drift
restrict 192.168.1.0 mask 255.255.255.0 nomodify notrap
server time.kriss.re.kr
server time.bora.net

ntpq 
ntpdate 192.168.1.80
=========================================================


#iptables ( firewalld )
---------------------------------------------------------
#show list
iptables -L
iptables-save
iptables-save -nat

#save and resore
iptables-save > firewall.sh
iptables-restore < firewall.sh

#flush
iptables -F 

#append
iptables -A INPUT -s 192.168.1.0/24 -p icmp -j ACCEPT

#delete
iptables -D INPUT 2
iptables -D INPUT -s 192.168.1.5 -p icmpt -j DROP

#block
iptable -A OUTPUT -p tcp --dport 80 -d www.posein.org -o eth0 -j DROP
iptable -A FORWARD -p tcp --dport 80 -d www.posein.org -o eth0 -j DROP

#ALL drop Except
iptable -P INPUT DROP
iptable -A INPUT -s 10.220.1.100 -j ACCEPT


#NAT
iptables -t nat -A PREROUTING -p tcp -d 1.235.51.26 --dport 443 -j DNAT --to 192.168.1.35:443
iptables -t nat -A POSTROUTING -o eth0 -j SNAT 1.235.51.26


#firewalld
firewall --list-all
firewalld --add-service=http --zone=public --permanent 
=========================================================
