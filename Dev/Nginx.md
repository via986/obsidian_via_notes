In a standard NGINX setup, the main config file (`/etc/nginx/nginx.conf`) includes external configuration files using an `include` directive.

- **The Modern/RedHat Style (`conf.d`):** NGINX automatically loads everything inside this folder using `include /etc/nginx/conf.d/*.conf;`. You place your active configuration files directly here.
- **The Debian/Ubuntu Style (`sites-available` & `sites-enabled`):** This approach uses two folders. You write the file in `sites-available` and create a symbolic link (symlink) to `sites-enabled` to activate it.