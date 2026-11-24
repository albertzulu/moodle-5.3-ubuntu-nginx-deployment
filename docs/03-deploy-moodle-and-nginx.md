# 3. Deploy Moodle and configure Nginx

## Download Moodle

The `MOODLE_503_STABLE` branch tracks the Moodle 5.3 stable series and receives maintenance updates. Check the current patch release before deployment. For controlled production releases, record and pin a reviewed release or commit.

```bash
sudo mkdir -p /var/www
cd /var/www
sudo git clone --depth 1 --branch MOODLE_503_STABLE \
  https://github.com/moodle/moodle.git moodle
cd /var/www/moodle
```

Install Composer before running the following command. Follow the dependency installation procedure documented for the exact Moodle release:
```bash
sudo apt install composer
```
```bash
sudo composer install --no-dev --classmap-authoritative
```

Keep the Moodle code owned by root and readable by the web server:

```bash
sudo chown -R root:root /var/www/moodle
sudo find /var/www/moodle -type d -exec chmod 755 {} \;
sudo find /var/www/moodle -type f -exec chmod 644 {} \;
```

## Create the Moodle data directory

The data directory must be outside the web root and must not be accessible over HTTP:

```bash
sudo mkdir -p /var/www/moodledata
sudo chown -R www-data:www-data /var/www/moodledata
sudo chmod 770 /var/www/moodledata
```

## Configure Nginx

Create `/etc/nginx/sites-available/moodle`. Replace `lms.example.com` with the domain name used for the site. This configuration uses Moodle's `public` web root and routing entry point; verify it against the documentation for the Moodle release being deployed.

```nginx
server {
    listen 80;
    server_name lms.example.com;

    root /var/www/moodle/public;
    index index.php;
    client_max_body_size 256M;

    location / {
        try_files $uri $uri/ /r.php$is_args$args;
    }

    location ~ \.php(/|$) {
        fastcgi_split_path_info ^(.+\.php)(/.*)$;
        set $path_info $fastcgi_path_info;
        try_files $fastcgi_script_name $fastcgi_script_name/ /r.php$is_args$args;
        fastcgi_pass unix:/run/php/php8.3-fpm.sock;
        include fastcgi_params;
        fastcgi_param PATH_INFO $path_info;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        fastcgi_param DOCUMENT_ROOT $realpath_root;
        fastcgi_read_timeout 300;
    }

    location ~ /\.(?!well-known).* {
        return 404;
    }

    location ~ (/vendor/|/node_modules/|composer\.json|/readme|/README|/upgrade\.txt|/UPGRADING\.md|db/install\.xml|/fixtures/|/behat/|phpunit\.xml|\.lock) {
        return 404;
    }
}
```

Enable the site, test the configuration, and restart Nginx:

```bash
sudo ln -s /etc/nginx/sites-available/moodle /etc/nginx/sites-enabled/moodle
sudo rm -f /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl restart nginx
```

## Next

Continue with [Moodle setup and server operations](04-setup-and-operations.md).
