Conntrackd
==========
Ansible Role that install and configure [conntrackd](http://conntrack-tools.netfilter.org/conntrackd.html "conntrack-tools: Netfilter's connection tracking userspace tools").  

This role install the conntrackd packages and transfer the configuration file (_conntrackd.conf_) to the server. The configuration template is set to use UDP for comunication beetween the two firewalls. It could be easily adapted to use Multicast if necessary.  

The two firewalls must have a dedicated network interface to be used by conntrackd's comunication.  

Requirements
------------
Debian 8 or 9  

Role Variables
--------------
The IPv4 address assined to the dedicated network interface used to comunicate with the other host:
```yaml
conntrackd_local_sync_ipv4: "192.168.100.3"
```

The IPv4 address of the neighbor host through the dedicated network interface:
```yaml
conntrackd_remote_sync_ipv4: "192.168.100.2"
```

The name of the dedicated network interface:
```yaml
conntrackd_sync_int: "eth1"
```

The IPv4 addresses that must be ignored by conntrackd, those are the IPv4 addresses of the host itself.
```yaml
conntrackd_ignore_addresses:
  - "192.168.20.10"
  - "192.168.30.1"
  - "192.168.30.2"
  - "192.168.30.3"
  - '{{ conntrackd_local_sync_ipv4 }}'
```

Dependencies
------------

None.

Example Playbook
----------------
```yaml
---
- name: Install and configure conntrackd
  hosts: firewall-01
  become: true
  gather_facts: true

  roles:
    - role: ansible-role-conntrackd
      conntrackd_local_sync_ipv4: "192.168.100.2"
      conntrackd_remote_sync_ipv4: "192.168.100.3"
      conntrackd_sync_int: "eth1"
      conntrackd_ignore_addresses:
        - "192.168.20.10"
        - "192.168.30.1"
        - "192.168.30.2"
        - "192.168.30.3"
        - '{{ conntrackd_local_sync_ipv4 }}'
```

License
-------

MIT

Author Information
------------------

Fabricio Boreli  
fabricioboreli at openmailbox dot org
