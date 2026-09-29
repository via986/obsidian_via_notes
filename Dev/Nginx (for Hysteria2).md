In a standard NGINX setup, the main config file (`/etc/nginx/nginx.conf`) includes external configuration files using an `include` directive.

- **The Modern/RedHat Style (`conf.d`):** NGINX automatically loads everything inside this folder using `include /etc/nginx/conf.d/*.conf;`. You place your active configuration files directly here.
- **The Debian/Ubuntu Style (`sites-available` & `sites-enabled`):** This approach uses two folders. You write the file in `sites-available` and create a symbolic link (symlink) to `sites-enabled` to activate it.

`server {`
    `server_name xpira.mooo.com;`
    `root /var/www/xpira_mooo;`
    `index index.html;`

    `add_header Alt-Svc 'h3=":443"; ma=86400';`

    `listen 443 ssl; # managed by Certbot`
    `ssl_certificate /etc/letsencrypt/live/xpira.mooo.com/fullchain.pem; # managed by Certbot`
    `ssl_certificate_key /etc/letsencrypt/live/xpira.mooo.com/privkey.pem; # managed by Certbot`
    `include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot`
    `ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot`

`}`
`server {`
    `if ($host = xpira.mooo.com) {`
        `return 301 https://$host$request_uri;`
    `} # managed by Certbot`


    `listen 80;`
    `server_name xpira.mooo.com;`
    `return 404; # managed by Certbot`


`}`