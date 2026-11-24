# Moodle 5.3 on Ubuntu 24.04 with Nginx

This repository contains an installation guide for deploying Moodle 5.3.1+ on Ubuntu Server 24.04 LTS with Nginx, PHP 8.3-FPM, and PostgreSQL 17.

## Guide

See [Moodle5.3-Ubuntu24.04-Nginx-Installation-Guide.docx](Moodle5.3-Ubuntu24.04-Nginx-Installation-Guide.docx) for the full, step-by-step installation procedure.

The guide covers:

- Ubuntu package preparation and required PHP extensions
- PostgreSQL installation and Moodle database setup
- Moodle source deployment and protected `moodledata`
- Nginx routing through Moodle's `public` web root and `r.php`
- HTTPS with Let's Encrypt
- Web-based setup, cron, service checks, backups, and production hardening

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

## Before deploying

- Check Moodle's official release requirements and confirm the selected PHP and PostgreSQL versions are supported.
- The `MOODLE_503_STABLE` branch advances as maintenance updates are published. For reproducible production deployments, record and pin a reviewed release or commit.
- Replace example domains and password placeholders. Never commit real credentials.
- Keep `moodledata` outside the web root and make sure Nginx cannot serve it.
- Install Composer before running Composer commands and follow the dependency procedure documented for the selected Moodle release.
- Review the Nginx configuration against the documentation for the exact Moodle release and verify it with `sudo nginx -t` before reloading the service.
- Back up the database, `moodledata`, and configuration, and test restores before upgrades.

## Official references

- [Moodle downloads](https://download.moodle.org/)
- [Moodle 5.3 release information](https://moodledev.io/general/releases/5.3)
- [Moodle source repository](https://github.com/moodle/moodle)
- [PostgreSQL APT repository for Ubuntu](https://www.postgresql.org/download/linux/ubuntu/)
- [Certbot instructions](https://certbot.eff.org/)
