# Webarchitects Apache Ansible Role

[![pipeline status](https://git.coop/webarch/apache/badges/master/pipeline.svg)](https://git.coop/webarch/apache/-/commits/master)

This repo contains an Ansible role for installing and configuring Apache on Debian servers.

If you use this role please use a tagged release, see [the release notes](https://git.coop/webarch/apache/-/releases).

On Debian Buster [Apache](https://packages.debian.org/buster-backports/apache2) is installed from [backports](https://backports.debian.org/) as this allows for the use of TLSv1.3, support for [mod_md](https://httpd.apache.org/docs/trunk/mod/mod_md.html) on Buster still needs to be added see [this issue](https://git.coop/webarch/apache/issues/5).

## Role variables

See the [defaults/main.yml](defaults/main.yml) file for the default variables, the [vars/main.yml](vars/main.yml) file for the preset variables and the [meta/argument_specs.yml](meta/argument_specs.yml) file for the variable specification.

## Repository

The primary URL of this repo is [`https://git.coop/webarch/apache`](https://git.coop/webarch/apache) however it is also [mirrored to GitHub](https://github.com/webarch-coop/ansible-role-apache) and [available via Ansible Galaxy](https://galaxy.ansible.com/chriscroome/apache).

If you use this role please use a tagged release, see [the release notes](https://git.coop/webarch/apache/-/releases).

## Copyright

Copyright 2018-2024 Chris Croome, &lt;[chris@webarchitects.co.uk](mailto:chris@webarchitects.co.uk)&gt;.

This role is released under [the same terms as Ansible itself](https://github.com/ansible/ansible/blob/devel/COPYING), the [GNU GPLv3](LICENSE).
