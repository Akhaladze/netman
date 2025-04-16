---

## Ansible Role: `mikrotik_natpmp`

### Directory Structure
```bash
roles/
└── mikrotik_natpmp/
    ├── tasks/
    │   └── main.yml
    └── vars/
        └── main.yml
```

### `tasks/main.yml`
```yaml

- name: Enable UPNP in mikrotik
  community.routeros.command:
    commands:
      - /ip upnp set enabled=yes allow-nat-pmp=yes interfaces=bridge

- name: Add firewall NAT rule for HTTP
  community.routeros.command:
    commands:
      - /ip firewall nat add chain=dstnat protocol=tcp dst-port=80 action=dst-nat to-addresses={{ internal_server_ip }} to-ports=80

- name: Add firewall NAT rule for HTTPS
  community.routeros.command:
    commands:
      - /ip firewall nat add chain=dstnat protocol=tcp dst-port=443 action=dst-nat to-addresses={{ internal_server_ip }} to-ports=443

```bash

### `vars/main.yml`
```bash

---

# internal_server_ip: 192.168.88.100  # Replace with your internal server IP

```bash

---

### Requirements

- `community.routeros` collection must be installed:

```bash
ansible-galaxy collection install community.routeros
```

---

## Usage

Include this role in your playbook:

```yaml
- hosts: mikrotik
  gather_facts: no
  roles:
    - mikrotik_natpmp
```

---

## MikroTik CLI Equivalent

### Enable NAT-PMP and UPnP on bridge interface

```bash
/ip upnp set enabled=yes allow-nat-pmp=yes interfaces=bridge
```

### Add NAT Rule for HTTP (port 80)

```bash
/ip firewall nat add chain=dstnat protocol=tcp dst-port=80 action=dst-nat to-addresses=192.168.88.100 to-ports=80
```

### Add NAT Rule for HTTPS (port 443)

```bash
/ip firewall nat add chain=dstnat protocol=tcp dst-port=443 action=dst-nat to-addresses=192.168.88.100 to-ports=443
```

### Ensure appropriate filter rules exist (example)

```bash
/ip firewall filter add chain=forward protocol=tcp dst-port=80,443 action=accept
```
