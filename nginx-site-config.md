````
    server {
        listen 80;
        listen	[::]:80;
        listen 443 ssl http2;
        listen [::]:443 ssl http2;
        
        server_name www.example.com example.com;
        charset UTF-8;
        access_log  logs/www.example.com.access.log;
        error_log  logs/www.example.com.error.log;
        root   html/www.example.com;
        index  index.html index.htm index.php;

        location ~* \.(ico|css|js|gif|jpe?g|png)(\?[0-9]+)?$ {
                expires max;
                log_not_found off;
        }

        location / {
            autoindex on;
            try_files $uri $uri/ /index.php;
#            try_files $uri $uri/ /index.php?/$request_uri;
#            try_files $uri $uri/ /index.php?$query_string;
#             rewrite ^/testrw/(.*)$ /test.php?id=$1 last;
        }

#        error_page  404	/404.html;
#        error_page   500 502 503 504  /50x.html;
#        location = /50x.html {
#            root   html/www.example.com/;
#        }

        location ~ \.php$ {
            fastcgi_pass localhost:9123;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include fastcgi_params;
        }

#        location ~ /\.ht {
#            deny  all;
#        }

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;
        ssl_ciphers ECDHE+RSAGCM:ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:!aNULL!eNull:!EXPORT:!DES:!3DES:!MD5:!DSS;
        ssl_certificate certs/server.crt;
        ssl_certificate_key certs/server.key;
    }
````
