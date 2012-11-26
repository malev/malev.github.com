Instalar wordpress en nginx

sudo yum -y install mysql-server php-fpm php-mysql php-pecl-apc

h8fF40IJ840f4

rubik_db
rubik_user
F40Jf490jws

/usr/bin/mysql_secure_installation

para configurar correctamente y seguramente el mysql

$ mysql -u adminusername -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 5340 to server version: 3.23.54
 
Type 'help;' or '\h' for help. Type '\c' to clear the buffer.
 
mysql> CREATE DATABASE rubik_db;
Query OK, 1 row affected (0.00 sec)
 
mysql> GRANT ALL PRIVILEGES ON rubik_db.* TO "rubik_user"@"locahost" IDENTIFIED BY "F40Jf490jws";
Query OK, 0 rows affected (0.00 sec)
  
mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.01 sec)

mysql> EXIT
Bye
$ 


# wget -O /tmp/root-passenger-21270/nginx.tar.gz http://www.nginx.org/download/nginx-1.0.10.tar.gz
--2012-04-09 00:07:03--  http://www.nginx.org/download/nginx-1.0.10.tar.gz
Resolving www.nginx.org... 206.251.255.63
Connecting to www.nginx.org|206.251.255.63|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 686011 (670K) [application/octet-stream]
Saving to: “/tmp/root-passenger-21270/nginx.tar.gz”

100%[=====================================================================================================================================================================================>] 686,011      590K/s   in 1.1s    

2012-04-09 00:07:05 (590 KB/s) - “/tmp/root-passenger-21270/nginx.tar.gz” saved [686011/686011]

Extracting Nginx source tarb

server {
  listen 80;

  server_name code.malev.com.ar;
  root /home/malev/sites/code.malev.com.ar/;

  location ~ \.php$ {
    root /home/malev/sites/code.malev.com.ar/;
    passenger_enabled off;
    index index.html index.php;
    try_files $uri $uri/ /index.php?q=$uri;
  }

  location ~ \.php$ {
    root /home/malev/sites/code.malev.com.ar/;
    passenger_enabled off;
    fastcgi_pass 127.0.0.1:49232;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include /opt/nginx/conf/fastcgi_params;
  }
}

server {
    listen 80;
    root /u/apps/magicapp/current/public;

    location / {
        passenger_enabled on;
    }

    location /blog {
        passenger_enabled off;
        index index.html index.php;
        try_files $uri $uri/ /blog/index.php?q=$uri;
    }

    location ~ \.php$ {
        passenger_enabled off;
        fastcgi_pass 127.0.0.1:49232;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include /opt/nginx/conf/fastcgi_params;
    }
}
