Bind9
=========
Bind9 DNS server

Installation
--------------

```yaml
- src: git@github.com:alexey1607/ansible-bind9.git
  scm: git
  version: master
  name: bind9
```

Role Variables
--------------

packages: BIND9 packages

zones_folder: zones folder

cache_directory: cache directory

recursion: DNS recursions yes/no 

zone_settings:
  serial: Серийный номер зоны.
  refresh: Интервал обновления (в секундах)
  retry: Интервал повторной попытки (в секундах).
  expire: Время истечения (в секундах).
  ttl: Время жизни (Time To Live) по умолчанию (в секундах).

forwarders: DNS servers

zones:
  forwards: Имя зоны
  revers: подсеть

acl: Список разрешенных сетей

Example Playbook
----------------

bind.yaml
```
---
- name: Install BIND9 сервер
  hosts: dns
  roles:
    - bind9
  vars:
    zones:
      forwards: "example.com"
      revers: "192.168.16"
    acl:
      - "192.168.16.0/24"
```
hists.ini
```
[dns]
ns-1        ansible_user=root     ansible_host=192.168.16.100   dns_role=master
ns-2        ansible_user=root     ansible_host=192.168.16.101   dns_role=slave
ns-3        ansible_user=root     ansible_host=192.168.16.102   dns_role=slave
```

License
-------

Apache-2.0

Author Information
------------------

Alexey
