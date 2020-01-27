# Ansible Apache Role

This repository contains an Ansible role for installing Apache on Debian Stretch and Buster (Ubuntu support should be added at some point). 

On Buster [Apache](https://packages.debian.org/buster-backports/apache2) is installed from [backports](https://backports.debian.org/) as this allows for the use of TLSv1.3, support for [mod_md](https://httpd.apache.org/docs/trunk/mod/mod_md.html) on Buster still needs to be added see [this issue](https://git.coop/webarch/apache/issues/5).

To use this role you need to use Ansible Galaxy to install it into another repository by adding a `requirements.yml` file in that repo that contains:

```yml
---
- name: apache
  src: https://git.coop/webarch/apache.git
  version: master
  scm: git
```

To pull this repo in run:

```bash
ansible-galaxy install -r requirements.yml --force -p galaxy/roles 
```

The other repo should also contain a (for example) `apache.yml` file that contains something like this (for `mpm_event` / `php-fpm`):

```yml
---
- name: Install Apache
  become: yes

  hosts: apache_buster_servers

  vars:
    apache_mods_enabled:
      - dir
      - env
      - headers
      - http2
      - include
      - mime
      - proxy
      - proxy_fcgi
      - rewrite
      - ssl
      - status
      - mpm_event
    apache_mods_disabled:
      - suexec
      - mpm_itk
      - mpm_prefork
      - php7.3
    apache_conf_enabled:
      - php7.3-fpm
      - webarch
    apache_conf_disabled:
      - serve-cgi-bin
      - phpmyadmin
    apache_sites_enabled:
      - localhost
    apache_sites_disabled:
      - 000-default
    apache_user: www-data
    apache_group: www-data
    # The following vars are all optional
    apache_ulimit: 65536
    # These vars depend on the ratelimit conf
    apache_apache_rate_limit: 64
    apache_rate_initial_burst: 256

  roles:
    - apache
```

Or something like this for `mod_php` and `mpm-itk`:

```yml
---
- name: Install Apache
  become: yes

  hosts: apache_servers

  vars:
    apache_mods_enabled:
      - dir
      - env
      - headers
      - include
      - mime
      - proxy
      - rewrite
      - ssl
      - mpm-itk
      - mpm_prefork
      - php7.3
    apache_mods_disabled:
      - http2
      - mpm_event
      - suexec
      - proxy_fcgi
    apache_conf_enabled:
      - webarch
    apache_conf_disabled:
      - serve-cgi-bin
      - php7.3-fpm
      - phpmyadmin
    apache_sites_enabled:
      - localhost
    apache_sites_disabled:
      - 000-default
    apache_user: www-data
    apache_group: www-data
    # The following vars are all optional
    apache_ulimit: 65536
    # These vars depend on the ratelimit conf
    apache_apache_rate_limit: 64
    apache_rate_initial_burst: 256
    # These vars depend on mpm-itk / mpm_prefork modules
    apache_mpm_max_request_workers: 128
    apache_mpm_max_connections_per_child: 10000

  roles:
    - apache
```

And a `hosts.yml` file that contains a `apache_servers` list as follows: 

```yml
---
all:
  children:
    apache_servers:
      hosts:
        host1.example.org:
        host2.example.org:
```

Then it can be run as follows:

```bash
ansible-playbook apache.yml -i hosts.yml
```
