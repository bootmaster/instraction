# Prosper install
1. Nginx configuration
    server {
        listen  80;
        server_name  www.domain domain_ip;
        add_header X-Frame-Options "SAMEORIGIN";
        return 301 $scheme://domain.com$request_uri;
      }
      
      server {
              listen 80;
              server_name domain.com;
              root /var/domains/domain/www/domain.com;
              index index.php index.html index.htm;
              access_log /var/domains/domain/data/logs/access.log;
              error_log /var/domains/domain/data/logs/error.log;
      #       auth_b
      #       auth_basic_user_file /var/domains/domain/data/.htpasswd;
      
      location / {
              try_files $uri $uri/ =404;
      }
      error_page 404 /404.php;
      error_page 500 502 503 504 /50x.html;
      location = /50x.html {
              root /var/domains/domain/www/domain.com;
      }
      location ~* ^.+\.(jpg|jpeg|gif|png|svg|htm|js|css|mp3|ogg|mpe?g|avi|zip|gz|bz2?|rar)$ {
              root /var/domains/domain/www/domain.com;
              expires 7d;
              error_page 404 = @fallback;
      }

      location ~ \.php$ {
              try_files $uri =404;
              fastcgi_split_path_info ^(.+\.php)(/.+)$;
              fastcgi_pass unix:/var/run/php5-fpm.sock;
              fastcgi_index index.php;
              include fastcgi_params;
              fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
              }
  }

2. Enable domain.com
    ln -s /etc/nginx/sites-available/domain.com /etc/nginx/sites-enabled/

3. Restart nginx
    service nginx restart
4. Add DB and user in MySql
  - if ddin't have root access
      dpkg-reconfigure mysql-server-5.5
  
  CREATE DATABASE proper;
  CREATE USER 'proper'@'localhost' IDENTIFIED BY 'password';
  GRANT ALL PRIVILEGES ON proper. * TO 'proper'@'localhost';
  FLUSH PRIVILEGES;

5. Upload prospre files into main directory of our website.
6. Unzip archive.
7. Rename 202-config-sample.php to 202-config.php and add to it our DB parameters.
8. Don't foget to add PHP Memcache.
    apt-get install php5 php5-memcache
