Bind9
=========

Ansible role for installing and configuring BIND9 DNS server with support for master-slave replication.

## Features

- Support for Debian/Ubuntu platforms
- Master-slave DNS server configuration
- Automatic configuration validation before applying changes
- Custom DNS records (A, CNAME, MX, TXT, SRV)
- Forward and reverse zone management
- ACL-based access control

## Requirements

- Ansible >= 2.1
- Target systems: Debian/Ubuntu

Installation
--------------

```yaml
- src: git@github.com:alexey1607/ansible-bind9.git
  scm: git
  version: develop
  name: bind9
```

Role Variables
--------------

### Main Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `bind_service_name` | `bind9` | BIND service name |
| `bind_config_path` | `/etc/bind` | Configuration directory path |
| `zones_folder` | `{{ bind_config_path }}/zones` | Directory for zone files |
| `cache_directory` | `/var/cache/bind` | BIND cache directory |
| `recursion` | `yes` | Allow DNS recursion |

### Zone Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `zones.forwards` | `alexhost.vg` | Forward zone name |
| `zones.revers` | `192.168.16` | Reverse zone subnet |

### Zone Settings

| Variable | Default | Description |
|----------|---------|-------------|
| `zone_settings.serial` | `10` | Zone serial number (use `{{ ansible_date_time.epoch }}` for auto-increment) |
| `zone_settings.refresh` | `604800` | Refresh interval (seconds) |
| `zone_settings.retry` | `86400` | Retry interval (seconds) |
| `zone_settings.expire` | `2419200` | Expiration time (seconds) |
| `zone_settings.ttl` | `604800` | Default TTL (seconds) |

### DNS Forwarders

```yaml
forwarders:
  - "1.1.1.1"
  - "8.8.8.8"
```

### Access Control

```yaml
acl:
  - "192.168.16.0/24"
  - "10.0.0.0/8"
```

### Custom DNS Records

```yaml
dns_records:
  a_records:
    - name: "server1"
      ip: "192.168.16.100"
    - name: "server2"
      ip: "192.168.16.101"
  
  mx_records:
    - priority: 10
      host: "mail.example.com."
  
  txt_records:
    - name: "@"
      value: "v=spf1 mx ~all"
    - name: "_dmarc"
      value: "v=DMARC1; p=none; rua=mailto:dmarc@example.com"
  
  srv_records:
    - name: "_ldap._tcp"
      priority: 0
      weight: 100
      port: 389
      target: "ldap.example.com."

cname:
  - name: "dns-1"
    target: "ns-1"
  - name: "www"
    target: "server1"
```

Example Playbook
----------------

### Basic Setup with Master-Slave

**playbook.yaml**
```yaml
---
- name: Configure BIND9 DNS servers
  hosts: dns
  roles:
    - bind9
  vars:
    zones:
      forwards: "example.com"
      revers: "192.168.16"
    acl:
      - "192.168.16.0/24"
    forwarders:
      - "1.1.1.1"
      - "8.8.8.8"
    dns_records:
      a_records:
        - name: "web"
          ip: "192.168.16.10"
        - name: "mail"
          ip: "192.168.16.20"
      mx_records:
        - priority: 10
          host: "mail.example.com."
      cname:
        - name: "www"
          target: "web"
```

**inventory.ini**
```ini
[dns]
ns-1    ansible_user=root    ansible_host=192.168.16.100    dns_role=master
ns-2    ansible_user=root    ansible_host=192.168.16.101    dns_role=slave
ns-3    ansible_user=root    ansible_host=192.168.16.102    dns_role=slave
```

### Running the Playbook

```bash
# Install all
ansible-playbook -i inventory.ini playbook.yaml

# Only install packages
ansible-playbook -i inventory.ini playbook.yaml --tags install

# Only configure
ansible-playbook -i inventory.ini playbook.yaml --tags configure

# Validate configuration
ansible-playbook -i inventory.ini playbook.yaml --tags validate
```

## Host Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `dns_role` | Yes | Either `master` or `slave` |
| `ansible_host` | Yes | IP address of the DNS server |

License
-------

Apache-2.0

Author Information
------------------

Alexey (alexey1607)

## Contributing

Issues and pull requests are welcome on GitHub.

