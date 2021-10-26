

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
