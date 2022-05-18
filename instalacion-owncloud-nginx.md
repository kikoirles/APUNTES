---
title: "INSTALACION OWNCLOUD NGINX"
tags: ""
---

# How to install Owncloud Nginx on Ubuntu Linux

As mentioned above, we’re going to be using Nginx web server to run ownCloud. ownCloud requires a web server to function, and Nginx is the most popular open source web servers available today.

To install Nginx on Ubuntu, run the commands below:

```shell
sudo apt update
sudo apt install nginx
```

After installing Nginx, the commands below can be used to stop, start and enable Nginx services to always start up everytime your server starts up.

```shell
sudo systemctl stop nginx.service
sudo systemctl start nginx.service
sudo systemctl enable nginx.service
```

To test whether Nginx is installed and functioning, open your web browser and browse to the server’s IP address or hostname.

http://localhost
Nginx web server default test page

If you see the above page in your browser, then Nginx is working as expected.
How to install MariaDB on Ubuntu Linux

A database server is required for ownCloud to function. ownCloud stores its content in a database, and MariaDB is probably the best database server available to run ownCloud.

MariaDB is fast, secure and the default server for almost all Linux servers. To install MariaDB, run the commands below:

```shell
sudo apt install mariadb-server
sudo apt install mariadb-client
```

After installing MariaDB, the commands below can be used to stop, start and enable MariaDB services to always start up when the server boots.

```shell
sudo systemctl stop mariadb.service
sudo systemctl start mariadb.service
sudo systemctl enable mariadb.service
```

Next, run the commands below to secure the database server with a root password if you were not prompted to do so during the installation.

```shell
sudo mysql_secure_installation
```

When prompted, use the guide below to answer:

```shell
If you've just installed MariaDB, and haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none): PRESS ENTER

Switch to unix_socket authentication [Y/n] n

Change the root password? [Y/n] n

Remove anonymous users? [Y/n] y

Disallow root login remotely? [Y/n] y

Remove test database and access to it? [Y/n] y

Reload privilege tables now? [Y/n] y

All done!
```

To verify and validate that MariaDB is installed and working, login to the database console using the commands below:

```shell
sudo mysql -u root -p
```

You should automatically be logged in to the database server since we initiated the login request as root. Only the root can login without password, and only from the server console.
mariadb welcome

If you see a similar screen as shown above, then the server was successfully installed.
How to install PHP on Ubuntu Linux

Also, PHP is required to run ownCloud. PHP packages are added to Ubuntu repositories. The versions the repositories might not be the latest. If you need to install the latest versions, you’ll need to add a third party PPA repository.

To a third party repository with the latest versions of PHP, run the commands below.


```shell
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:ondrej/php
```

At the time of this writing, the latest PHP version 8.0.


```shell
sudo apt update
```

Next, run the commands below to install PHP 8.0 and related modules.

```shell
sudo apt install php7.4-fpm php7.4-imagick php7.4-common php7.4-mysql php7.4-gmp php7.4-imap php7.4-json php7.4-pgsql php7.4-ssh2 php7.4-sqlite3 php7.4-ldap php7.4-curl php7.4-intl php7.4-mbstring php7.4-xmlrpc php7.4-gd php7.4-xml php7.4-cli php7.4-zip
```

Next, you’ll want to change some PHP configuration settings that work great with ownCloud. Run the commands below to open PHP default configuration file.


```shell
sudo nano /etc/php/7.4/fpm/php.ini
```

Then change the line settings to be something line the lines below. Save your changes and exit.


```shell
file_uploads = On
allow_url_fopen = On
short_open_tag = On
memory_limit = 256M
upload_max_filesize = 100M
max_execution_time = 360
date.timezone = America/Chicago
```

How to create ownCloud database on Ubuntu

At this point, we’re ready to create ownCloud database. As mentioned above, ownCloud uses databases to store its content.

To create a database for ownCloud , run the commands below:

```shell
sudo mysql -u root -p
```

Then create a database called owncloud

```shell
CREATE DATABASE owncloud;
```
Next, create a database user called ownclouduser and set password

```shell
CREATE USER 'ownclouduser'@'localhost' IDENTIFIED BY 'barcelona22';
```

Then grant the user full access to the database.

```shell
GRANT ALL ON owncloud.* TO 'ownclouduser'@'localhost' WITH GRANT OPTION;
```

Finally, save your changes and exit.

```shell
FLUSH PRIVILEGES;
EXIT;
```

How to download ownCloud on Ubuntu

We’re ready to download ownCloud and begin configuring it. First, run the commands below to download the latest version of ownCloud from its repository.

Next, extract the downloaded content into Nginx root directory. This will create folder called owncloud.

```shell
wget https://download.owncloud.org/community/owncloud-complete-20210721.zip -P /tmp
sudo unzip /tmp/owncloud-complete-20210721.zip -d /var/www
```

Then run command below to allow www-data user to own the new owncloud directory.

```shell
sudo chown -R www-data:www-data /var/www/owncloud/
sudo chmod -R 755 /var/www/owncloud/
```

How to configure Nginx for ownCloud

We have downloaded ownCloud content into a new folder we called ownCloud. Now, let’s configure Nginx to create a new server block to use with our ownCloud website. You can create as many server blocks with Nginx.

To do that, run the commands below to create a new configuration file called owncloud.conf in the /etc/nginx/sites-available/ directory to host our ownCloud server block.

```shell
sudo nano /etc/nginx/sites-available/owncloud.conf
```

In the file, copy and paste the content below into the file and save.

```shell
upstream php-handler { 
    server unix:/var/run/php/php7.4-fpm.sock;
}
server {
    listen 80;
    listen [::]:80;
    root /var/www;
    index  index.php index.html index.htm;
    server_name  example.com;


  location ^~ /owncloud {

        client_max_body_size 512M;
        fastcgi_buffers 8 4K;
        fastcgi_ignore_headers X-Accel-Buffering;

        gzip off;

        error_page 403 /owncloud/core/templates/403.php;
        error_page 404 /owncloud/core/templates/404.php;

        location /owncloud {
            rewrite ^ /owncloud/index.php$uri;
        }

        location ~ ^/owncloud/(?:build|tests|config|lib|3rdparty|templates|changelog|data)/ {
            return 404;
        }
        location ~ ^/owncloud/(?:\.|autotest|occ|issue|indie|db_|console|core/skeleton/) {
            return 404;
        }
        location ~ ^/owncloud/core/signature\.json {
            return 404;
        }

        location ~ ^/owncloud/(?:index|remote|public|cron|core/ajax/update|status|ocs/v[12]|updater/.+|oc[sm]-provider/.+|core/templates/40[34])\.php(?:$|/) {
            fastcgi_split_path_info ^(.+\.php)(/.*)$;
            include fastcgi_params;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            fastcgi_param SCRIPT_NAME $fastcgi_script_name;
            fastcgi_param PATH_INFO $fastcgi_path_info;
            fastcgi_param modHeadersAvailable true;
            fastcgi_read_timeout 180;
            fastcgi_pass php-handler;
            fastcgi_intercept_errors on;
            fastcgi_request_buffering off;
        }

        location ~ ^/owncloud/(?:updater|oc[sm]-provider)(?:$|/) {
            try_files $uri $uri/ =404;
            index index.php;
        }

        # Adding the cache control header for js and css files
        # Make sure it is BELOW the PHP block
        location ~ /owncloud/.*\.(?:css|js) {
            try_files $uri /owncloud/index.php$uri$is_args$args;
            add_header Cache-Control "max-age=15778463" always;
            add_header X-Content-Type-Options "nosniff" always;
            add_header X-Frame-Options "SAMEORIGIN" always;
            add_header X-XSS-Protection "1; mode=block" always;
            add_header X-Robots-Tag "none" always;
            add_header X-Download-Options "noopen" always;
            add_header X-Permitted-Cross-Domain-Policies "none" always;
            access_log off;
        }

        location ~ /owncloud/.*\.(?:svg|gif|png|html|ttf|woff|ico|jpg|jpeg|map|json) {
            try_files $uri /owncloud/index.php$uri$is_args$args;
            add_header Cache-Control "public, max-age=7200" always;
            access_log off;
        }
    }
}

```

Save the file and exit.

After saving the file above, run the commands below to enable the new file that contains our ownCloud server block, as well as other important Nginx modules.

Restart Nginx after that.

```shell
sudo ln -s /etc/nginx/sites-available/owncloud.conf /etc/nginx/sites-enabled/
```

Reload Nginx when done the the configuration above.

```shell
sudo systemctl restart nginx.service
```

Now that ownCloud is downloaded, and the necessary services are configured open your browser and start the ownCloud installation by visiting your server’s domain name or IP address followed by /owncloud :

```shell
http://localhost/owncloud
```

However, we want to make sure our server is protected with Let’s Encrypt free SSL certificates. So, continue below to learn how to generate Let’s Encrypt SSL certificate for websites.

### Encrypt OWNCLOUD 
https://websiteforstudents.com/how-to-install-owncloud-on-ubuntu-linux-with-nginx/
