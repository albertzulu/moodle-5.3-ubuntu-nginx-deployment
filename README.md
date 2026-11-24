# Moodle 5.3 on Ubuntu 24.04 with Nginx

A step-by-step deployment guide for Moodle 5.3.1+ on Ubuntu Server 24.04 LTS using Nginx, PHP 8.3-FPM, and PostgreSQL 17.

## Installation guide

1. [Architecture and prerequisites](docs/01-architecture-and-prerequisites.md)
2. [Install PHP and PostgreSQL](docs/02-install-php-and-postgresql.md)
3. [Deploy Moodle and configure Nginx](docs/03-deploy-moodle-and-nginx.md)
4. [Finish setup and operate the server](docs/04-setup-and-operations.md)

## Target architecture

| Component | Version or path |
| --- | --- |
| Operating system | Ubuntu Server 24.04 LTS |
| Moodle | 5.3.1+; verify current patch release and requirements |
| Web server | Nginx |
| PHP | 8.3 with PHP-FPM |
| Database | PostgreSQL 17 |
| Moodle code | `/var/www/moodle` |
| Moodle web root | `/var/www/moodle/public` |
| Moodle data | `/var/www/moodledata` |

## Important

- Confirm the selected Moodle release's supported PHP and database versions before deployment.
- The `MOODLE_503_STABLE` branch advances over time. For reproducible production deployments, record and pin a reviewed release or commit.
- Replace example domains and password placeholders. Never commit real credentials.
- Keep `moodledata` outside the web root and ensure Nginx cannot serve it.
- Install Composer before running Composer commands and follow the dependency procedure for the selected Moodle release.
- Review Nginx configuration against the selected Moodle release's documentation. Run `sudo nginx -t` before reloading Nginx.
- Back up the database, `moodledata`, and configuration, and test restores before upgrades.

## Official references

- [Moodle downloads](https://download.moodle.org/)
- [Moodle 5.3 release information](https://moodledev.io/general/releases/5.3)
- [Moodle source repository](https://github.com/moodle/moodle)
- [PostgreSQL APT repository for Ubuntu](https://www.postgresql.org/download/linux/ubuntu/)
- [Certbot instructions](https://certbot.eff.org/)
