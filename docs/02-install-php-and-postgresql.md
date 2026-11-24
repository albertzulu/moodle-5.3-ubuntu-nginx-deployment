# 2. Install PHP and PostgreSQL

## Install PHP 8.3 and extensions

```bash
sudo apt install -y php8.3-fpm php8.3-cli php8.3-common php8.3-curl \
  php8.3-gd php8.3-intl php8.3-mbstring php8.3-xml php8.3-zip \
  php8.3-soap php8.3-bcmath php8.3-opcache php8.3-pgsql php8.3-ldap

php -v
sudo systemctl enable --now php8.3-fpm
```

### Configure PHP

Edit `/etc/php/8.3/fpm/php.ini` and apply values suitable for your workload:
```bash
sudo nano /etc/php/8.3/fpm/php.ini
```
```
memory_limit = 512M
upload_max_filesize = 256M
post_max_size = 256M
max_execution_time = 300
max_input_time = 300
max_input_vars = 5000
date.timezone = Africa/Lusaka
```

Apply equivalent settings to `/etc/php/8.3/cli/php.ini`, then restart PHP-FPM:
```bash
sudo nano /etc/php/8.3/cli/php.ini
```
```bash
sudo systemctl restart php8.3-fpm
```

## Install PostgreSQL 17

Ubuntu 24.04's standard repository may provide an older PostgreSQL major version. Add the official PostgreSQL APT repository:

```bash
sudo install -d /usr/share/postgresql-common/pgdg
sudo curl -o /usr/share/postgresql-common/pgdg/apt.postgresql.org.asc \
  https://www.postgresql.org/media/keys/ACCC4CF8.asc
echo "deb [signed-by=/usr/share/postgresql-common/pgdg/apt.postgresql.org.asc] https://apt.postgresql.org/pub/repos/apt noble-pgdg main" \
  | sudo tee /etc/apt/sources.list.d/pgdg.list
sudo apt update
sudo apt install -y postgresql-17 postgresql-client-17
sudo systemctl enable --now postgresql
psql --version
```

## Create the Moodle database

Open the PostgreSQL prompt:

```bash
sudo -u postgres psql
```

At the prompt, create a database user and database. Replace the placeholder with a unique, strong password:

```sql
CREATE USER moodle WITH PASSWORD 'REPLACE_WITH_A_STRONG_PASSWORD';
CREATE DATABASE moodle OWNER moodle ENCODING 'UTF8' TEMPLATE template0;
\q
```

Store the database password securely. Do not commit it to this repository.

## Next

Continue with [Moodle deployment and Nginx configuration](03-deploy-moodle-and-nginx.md).
