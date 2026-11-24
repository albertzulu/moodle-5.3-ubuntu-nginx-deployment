# 1. Architecture and prerequisites

This guide describes a production-oriented Moodle deployment on Ubuntu Server 24.04 LTS with Nginx, PHP-FPM, and PostgreSQL.

## Target architecture

| Component | Target |
| --- | --- |
| Operating system | Ubuntu Server 24.04 LTS |
| Moodle | 5.3.1+ (verify the current patch release) |
| Web server | Nginx |
| PHP | 8.3 with PHP-FPM |
| Database | PostgreSQL 17 |
| Moodle code | `/var/www/moodle` |
| Moodle web root | `/var/www/moodle/public` |
| Moodle data | `/var/www/moodledata` |

Before installing, confirm the official Moodle release requirements for the exact release you plan to deploy. Ubuntu 24.04's standard repositories may not provide the desired PostgreSQL major version.

## Update Ubuntu and install base packages

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install -y nginx git unzip curl wget software-properties-common \
  ca-certificates gnupg
```

## Next

Continue with [PHP and PostgreSQL installation](02-install-php-and-postgresql.md).
