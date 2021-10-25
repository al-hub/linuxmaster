

apache (httpd.conf)
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
DirectoryIndex index.html index.html index.php
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
