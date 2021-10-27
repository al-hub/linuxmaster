

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
=========================================================


#NIS (Network Information Service, LDAP) for user,passwd
---------------------------------------------------------
*SEVER-SIDE
service rpcbind start

ypserv
yppasswd
ypxfrd

nisdomainname test.co.kr 
(or set: /etc/sysconfig/network NISDOMAIN=test.co.kr)

(update: make -C /var/yp )


*CLIENT-SIDE
ypbind
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
	valid users = posein
	public = yes
	write list = @insa

*CLIENT-SIDE
smbclient -L 192.168.1.1 -U root@1234
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
