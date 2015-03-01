---
layout: post
title: Install Apache, Mysql and PHP on Arch Linux
comments: true
description: How to install Apache, Mysql and PHP on Arch Linux
published: true
categories:
- Linux
tags:
- Linux
- ArchLinux
- PHP
- Apache
- Mysql

---

This was a really fun and quickly lab that I had one week ago and I decided to publish. So the lab it was basically install **[Apache](http://www.apache.org/){:target="_blank"}** and **[PHP](http://php.net/){:target="_blank"}**
on any linux, in my case I'm a fan of **[Arch Linux](https://www.archlinux.org/){:target="_blank"}** and of course Arch user so if you have another linux version based on Arch like **[Manjaro](https://manjaro.github.io/){:target="_blank"}** 
you can install in the same way.

In my case I had already installed the Apache and the **[Mysql](http://www.mysql.com/){:target="_blank"}** but not the PHP, so let go to the deal. Arch is use pacman as a package 
manager and is really easy to use, so first to install Apache 

{% highlight text %}
sudo pacman -S apache
{% endhighlight %}

and done, to install mysql and I choose **[MariaDB](https://mariadb.org/){:target="_blank"}** as my engine db

{% highlight text %}
sudo pacman -S mysql
{% endhighlight %}

done, and to install PHP we need to install this packages

{% highlight text %}
sudo pacman -S php
{% endhighlight %}

and

{% highlight text %}
sudo pacman -S php-apache
{% endhighlight %}

Now with the php already installed, we need to enable the php with apache, so following the arch wiki documentation, we need to add some lines
in the http.conf of the apache.

By default the http.conf of the apache is in this path **/etc/httpd/conf/httpd.conf**, so first we have a little note of the arch wiki;

**Note:** libphp5.so included with php-apache does not work with mod_mpm_event (FS#39218). You'll have to use mod_mpm_prefork instead.

To use mod_mpm_prefork, open /etc/httpd/conf/httpd.conf and replace
{% highlight text %}
LoadModule mpm_event_module modules/mod_mpm_event.so
{% endhighlight %}
with
{% highlight text %}
LoadModule mpm_prefork_module modules/mod_mpm_prefork.so
{% endhighlight %}
As an alternative, you can use mod_proxy_fcgi (see #Using php5 with php-fpm and mod_proxy_fcgi below).

To enable PHP, add these lines to /etc/httpd/conf/httpd.conf:
Place this in the LoadModule list anywhere after LoadModule dir_module modules/mod_dir.so:

{% highlight text %}
LoadModule php5_module modules/libphp5.so
{% endhighlight %}

Place this at the end of the Include list:

{% highlight text %}
Include conf/extra/php5_module.conf
{% endhighlight %}

To test whether PHP was correctly configured: create a file called phpinfo.php in your Apache DocumentRoot directory (e.g. /srv/http/ or ~/public_html) in my 
case I created a folder "phpinfo" with the following contents:

{% highlight php %}
<?php phpinfo(); ?>
{% endhighlight %}

<center>
<img alt="phplinux" src="/images/archphp.png">
</center>

References:

* <a target="_blank" href="https://wiki.archlinux.org/index.php/Apache_HTTP_Server">https://wiki.archlinux.org/index.php/Apache_HTTP_Server</a>
* <a target="_blank" href="https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-arch-linux">https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-arch-linux</a>






