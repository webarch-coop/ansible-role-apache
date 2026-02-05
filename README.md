# Webarchitect Apache Ansible Role

[![pipeline status](https://git.coop/webarch/apache/badges/master/pipeline.svg)](https://git.coop/webarch/apache/-/commits/master)

This repo contains an Ansible role for installing and configuring Apache on Debian servers.

If you use this role please use a tagged release, see [the release notes](https://git.coop/webarch/apache/-/releases).

By default this role uses [mod_md](https://github.com/icing/mod_md/) for HTTPS certifiates, in addition the cert and key matching the `inventory_hostname` are be copied for use by other applications, specifically Exim, see the `apache_md_cert_copy` list of dictionaries.

## Usage

Run this role using the `apache_install` tag for the intial install, once `apache2` has been installed the role can be run in check mode.

## Role variables

See the [defaults/main.yml](defaults/main.yml) file for the default variables, the [vars/main.yml](vars/main.yml) file for the preset variables and the [meta/argument_specs.yml](meta/argument_specs.yml) file for the variable specification.

## Repository

The primary URL of this repo is [`https://git.coop/webarch/apache`](https://git.coop/webarch/apache) however it is also [mirrored to GitHub](https://github.com/webarch-coop/ansible-role-apache) and [available via Ansible Galaxy](https://galaxy.ansible.com/chriscroome/apache).

If you use this role please use a tagged release, see [the release notes](https://git.coop/webarch/apache/-/releases).

## Copyright

Copyright 2018-2026 Chris Croome, &lt;[chris@webarchitects.co.uk](mailto:chris@webarchitects.co.uk)&gt;.

This role is released under [the same terms as Ansible itself](https://github.com/ansible/ansible/blob/devel/COPYING), the [GNU GPLv3](LICENSE).
