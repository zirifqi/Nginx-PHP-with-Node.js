# Nginx-PHP-with-Node.js

## Untuk setting domain utama menggunakan web server Nginx dan PHP
Tambahkan script pada file `/etc/nginx/sites-enabled/default` tambahkan script dibawah `location / {` :

```nginx
location / {
    index index.php index.html index.htm;
    location ~* \.php$ {
        try_files $uri =404;
        fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
        #fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
}
```

## Untuk setting subfolder pada domain menggunakan Node.js
Tambahkan script pada file `/etc/nginx/sites-enabled/default` di bawah kode `location / { .... }` :

```nginx
location /store/ {
    proxy_set_header   X-Forwarded-For $remote_addr;
    proxy_set_header   Host $http_host;
    proxy_pass         "http://127.0.0.1:8000";
    rewrite ^/store/(.*)$ /$1 break;
}
```

## Restart Nginx
```bash
service nginx restart
```