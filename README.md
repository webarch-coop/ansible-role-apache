# Ansible Apache Role

Currently this repository contains an Ansible role for installing Apache on Debian Stretch. 

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
ansible-galaxy install -r requirements.yml --force -p roles 
```

The other repo should also contain a `.yml` file that contains something like this:

```yml
---
- name: Install Apache
  become: yes

  hosts: apache_servers

  vars:

  roles:
    - apache
```

And a `hosts.yml` file that contains a `matomo_servers` list as follows: 

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
ansible-playbook matomo.yml -i hosts.yml
``` 
