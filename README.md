# Webarchitect Apache Ansible Role

[![pipeline status](https://git.coop/webarch/apache/badges/master/pipeline.svg)](https://git.coop/webarch/apache/-/commits/master)

This repo contains an Ansible role for installing and configuring Apache on Debian servers.

If you use this role please use a tagged release, see [the release notes](https://git.coop/webarch/apache/-/releases).

By default this role uses the latest Apache from the [Sury apache2 deb repo](https://packages.sury.org/apache2/README.txt) and [mod_md](https://httpd.apache.org/docs/2.4/mod/mod_md.html) for HTTPS certifiates, optionally the cert and key matching the `inventory_hostname` are be copied for use by other applications, specifically Exim, see the `apache_md_cert_copy` list of dictionaries.

## Usage

Run this role using the `apache_install` tag for the intial install, once `apache2` has been installed the role can be run in check mode.

## Role variables

See the [defaults/main.yml](defaults/main.yml) file for the default variables, the [vars/main.yml](vars/main.yml) file for the preset variables and the [meta/argument_specs.yml](meta/argument_specs.yml) file for the variable specification.

## Automatically reloading Apache for mod_md cert renewals

In theory Systemd can be used to restart Apache when a `/etc/apache2/md/staging/*/pubcert.pem` file exists, for example a `/etc/systemd/system/apache-md.path` containing

```ini
[Unit]
Description=Reload Apache when new Let's Encrypt certificates are available

[Path]
PathExistsGlob=/etc/apache2/md/staging/*/pubcert.pem
TriggerLimitIntervalSec=60

[Install]
WantedBy=multi-user.target
```

And a corresponding `/etc/systemd/system/apache-md.service` file containing:

```ini
[Unit]
Description=Reload Apache when new Let's Encrypt certificates are available
StartLimitIntervalSec=60

[Service]
Type=oneshot
ExecStart=/usr/bin/systemctl restart apache2
```

These can be enabled and started using:

```bash
systemctl daemon-reload
systemctl enable --now apache-md.path
```

However the above don't result in Apache being restarted after certs are created in a sub-directory of the `/etc/apache2/md/staging` directory, unless the service is restarted `systemctl restart apache-md.path`, then it works...

So a [templates/apache_md_cert_reload.sh.j2](templates/apache_md_cert_reload.sh.j2) Bash script is run via cron for this.

## Repository

The primary URL of this repo is [`https://git.coop/webarch/apache`](https://git.coop/webarch/apache) however it is also [mirrored to GitHub](https://github.com/webarch-coop/ansible-role-apache) and [available via Ansible Galaxy](https://galaxy.ansible.com/chriscroome/apache).

If you use this role please use a tagged release, see [the release notes](https://git.coop/webarch/apache/-/releases).

## Copyright

Copyright 2018-2026 Chris Croome, &lt;[chris@webarchitects.co.uk](mailto:chris@webarchitects.co.uk)&gt;.

This role is released under [the same terms as Ansible itself](https://github.com/ansible/ansible/blob/devel/COPYING), the [GNU GPLv3](LICENSE).
