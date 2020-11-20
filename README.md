# How to Speedrun Deploy Wordpress 5 with ec2 Linux 2 and RDS
Watch it here      
Wordpress 5.5.3 (PHP v7.4.11) Mysql 8.0  
Since Nov 20, 2020  

---  
```sh
$ sudo yum update -y  
$ sudo amazon-linux-extras enable php7.4 -y  
$ sudo yum clean metadata  
$ sudo yum install  -y php php-{pear,cgi,common,curl,mbstring,gd,mysqlnd,gettext,bcmath,json,xml,fpm,intl,zip,imap}  
$ sudo yum install  -y git  

$ php --version  
```
**Dependencies installation will take some time. After than set proper permissions on files.**  
```sh
$ sudo chown -R ec2-user:apache /var/www  
$ sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;  
$ find /var/www -type f -exec sudo chmod 0664 {} \;  

$ cd /var/www/html  
$ git clone git://github.com/WordPress/WordPress.git speedrun_wordpress  
$ cd /var/www/html/speedrun_wordpress 
```

**Choose version:**  
```sh
$ git branch -r
$ git checkout 5.6-branch
```

**Delete git repo:**  
```sh
$ rm -rf .git
```

**Set the host:**  
```sh
$ sudo vim /etc/httpd/conf/httpd.conf   
```

**Add code at the bottom of the file**  

```blade
<VirtualHost *:80>  
	ServerName wordpress.example.com  
	DocumentRoot /var/www/html/speedrun_wordpress   
	<Directory /var/www/html/speedrun_wordpress>  
		AllowOverride All  
	</Directory>  
</VirtualHost>  
```  

```sh
$ sudo systemctl start httpd  
$ sudo systemctl start php-fpm  
$ sudo systemctl enable php-fpm  
```
