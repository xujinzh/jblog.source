---
title: 如何使用服务器搭建博客
date: 2019.12.22
tags:
- blog
- server
categories: technology
---

<p>参照博文《<a rel="noreferrer noopener" aria-label="（在新窗口打开）" href="https://websiteforstudents.com/install-wordpress-on-ubuntu-18-04-lts-beta-with-apache2-mariadb-and-php-7-1/" target="_blank"><em>Website for Students</em></a>》或《<a rel="noreferrer noopener" aria-label="How to Install WordPress with LAMP Stack on Ubuntu 18.04（在新窗口打开）" href="https://www.rosehosting.com/blog/how-to-install-wordpress-with-lamp-stack-on-ubuntu-18-04/" target="_blank"><em><strong>How to Install WordPress with LAMP Stack on Ubuntu 18.04</strong></em></a>》进行搭建.</p>
<p>在Ubuntu18.04上，分别按照《Website for Students》和文章《How to Install WordPress with LAMP Stack on Ubuntu 18.04》搭建网站时，发现第一篇文章能够成功进入WordPress，而第二篇文章无法配置WordPress，经分析后通过命令 </p>
<!--more-->

```bash
apt install php-mysql
```

<p>进行修复，成功进入WordPress配置页面。</p>
<p>下面根据前面两篇文章总结出一个搭建网站的方法，一共分为7步：</p>
<ul><li><a href="https://www.rosehosting.com/blog/how-to-install-wordpress-with-lamp-stack-on-ubuntu-18-04/#Step-1-Connect-to-your-Server">Step 1: Connect to your Server</a></li><li><a href="https://www.rosehosting.com/blog/how-to-install-wordpress-with-lamp-stack-on-ubuntu-18-04/#Step-2-Apache-Web-Server-Installation">Step 2: Apache Web Server Installation</a></li><li><a href="https://www.rosehosting.com/blog/how-to-install-wordpress-with-lamp-stack-on-ubuntu-18-04/#Step-3-Install-PHP">Step 3: Install PHP</a></li><li><a href="https://www.rosehosting.com/blog/how-to-install-wordpress-with-lamp-stack-on-ubuntu-18-04/#Step-4-Install-the-MySQL-Database-server">Step 4: Install the MySQL Database server</a></li><li><a href="https://www.rosehosting.com/blog/how-to-install-wordpress-with-lamp-stack-on-ubuntu-18-04/#Step-5-Create-a-Database-for-WordPress">Step 5: Create a Database for WordPress</a></li><li><a href="https://www.rosehosting.com/blog/how-to-install-wordpress-with-lamp-stack-on-ubuntu-18-04/#Step-6-Install-WordPress">Step 6: Install WordPress</a></li><li><a href="https://www.rosehosting.com/blog/how-to-install-wordpress-with-lamp-stack-on-ubuntu-18-04/#Step-6-Create-the-Virtual-Host-Files">Step 6: Create the Virtual Host Files</a></li><li><a href="https://www.rosehosting.com/blog/how-to-install-wordpress-with-lamp-stack-on-ubuntu-18-04/#Step-7-Configure-WordPress">Step 7: Configure WordPress</a></li></ul>
<p>具体地</p>
<p><strong>Step 1: Connect to your Server</strong>，更新系统</p>
```bash
apt-get update
apt-get upgrade
```

<p><strong>Step 2: Apache Web Server Installation</strong>，安装Web服务器</p>
```bash
apt-get install apache2
```

```bash
systemctl enable apache2
```

```bash
systemctl status apache2
```

<p><strong>Step 3: Install PHP</strong>，安装PHP</p>
```bash
apt install php php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip<strong> php-mysql</strong>
```

<p><strong>Step 4: Install the MySQL Database server</strong>，安装数据库</p>
```bash
apt install mysql-server
```

```bash
mysql_secure_installation
```

<p><strong>Step 5: Create a Database for WordPress</strong>，为网站创建一个数据库</p>
```bash
mysql -u root -p
```

<pre class="wp-block-preformatted">CREATE DATABASE <strong>wp</strong>;
GRANT ALL PRIVILEGES ON <strong>wp</strong>.* TO '<strong>jayzon</strong>'@'localhost' IDENTIFIED BY '<strong>StrongPassword</strong>';
FLUSH PRIVILEGES;
EXIT;</pre>
<p><strong>Step 6: Install WordPress</strong>，安装WordPress</p>
```bash
cd /var/www/html
wget -c http://wordpress.org/latest.zip
unzip latest.zip
chown -R www-data:www-data wordpress
rm latest.zip
```

```bash
cd /var/www/html/wordpress
mv wp-config-sample.php wp-config.php
```

```bash
vim wp-config.php
```

<p>插入一下代码</p>
<pre class="wp-block-preformatted">// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define('DB_NAME', '<strong>wp</strong>');

/** MySQL database username */
define('DB_USER', '<strong>jayzon</strong>');

/** MySQL database password */
define('DB_PASSWORD', '<strong>StrongPassword</strong>');

/** MySQL hostname */
define('DB_HOST', 'localhost');

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');</pre>

<p><strong>Step 6: Create the Virtual Host Files</strong>，打开80端口</p>
```bash
vim /etc/apache2/sites-available/your_domain.com.conf
```

<pre class="wp-block-preformatted">&lt;VirtualHost *:80>

ServerAdmin admin@jayzonxu.com
ServerName jayzonxu.com
ServerAlias www.jayzonxu.com
DocumentRoot /var/www/html/wordpress

&lt;Directory /var/www/html/wordpress>
     Options Indexes FollowSymLinks
     AllowOverride All
     Require all granted
&lt;/Directory>

ErrorLog ${APACHE_LOG_DIR}/jayzonxu.com_error.log 
CustomLog ${APACHE_LOG_DIR}/jayzonxu.com_access.log combined 
&lt;/VirtualHost></pre>

```bash
ln -s /etc/apache2/sites-available/jayzonxu.com.conf /etc/apache2/sites-enabled/jayzonxu.com.conf
```

<p><strong>Step 7: Configure WordPress</strong>，配置网站</p>
<p>在浏览器中输入以下网址，进行配置WordPress</p>
<pre class="wp-block-preformatted">http://www.jayzonxu.com/</pre>
更多请参考
[embed]https://www.rosehosting.com/blog/how-to-install-wordpress-with-lamp-stack-on-ubuntu-18-04/[/embed]