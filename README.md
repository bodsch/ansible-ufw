
# Ansible Role:  `ufw`

This role is an fork from [Oefenweb](https://github.com/Oefenweb/ansible-ufw)!

Installs and configure ufw on various systems.


[![GitHub Workflow Status](https://img.shields.io/github/workflow/status/bodsch/ansible-ufw/CI)][ci]
[![GitHub issues](https://img.shields.io/github/issues/bodsch/ansible-ufw)][issues]
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/bodsch/ansible-ufw)][releases]

[ci]: https://github.com/bodsch/ansible-ufw/actions
[issues]: https://github.com/bodsch/ansible-ufw/issues?q=is%3Aopen+is%3Aissue
[releases]: https://github.com/bodsch/ansible-ufw/releases

## Requirements

None

## Dependencies

None

## Variables

| variable                                      | default                  | description |
| :---                                          | : ---                    | :---        |
| `ufw_default_incoming_policy`                 | `deny`                   | Default (incoming) policy |
| `ufw_default_outgoing_policy`                 | `allow`                  | Default (outgoing) policy |
| `ufw_logging`                                 | `low`                    | Log level |
| `ufw_rules`                                   |  see `defaults/main.yml` | Rules to apply |
| `ufw_etc_default_ipv6`                        |  `false`                 | Set to yes to apply rules to support IPv6 |
| `ufw_etc_default_default_input_policy`        |  `DROP`                  | Set the default input policy to `ACCEPT`, `DROP`, or `REJECT`.<br>Please note that if you change this you will most likely want to adjust your rules |
| `ufw_etc_default_default_output_policy`       |  `ACCEPT`                | Set the default output policy to `ACCEPT`, `DROP`, or `REJECT`.<br>Please note that if you change this you will most likely want to adjust your rules |
| `ufw_etc_default_default_forward_policy`      |  `DROP`                  | Set the default forward policy to `ACCEPT`, `DROP` or `REJECT`.<br>Please note that if you change this you will most likely want to adjust your rules |
| `ufw_etc_default_default_application_policy`  |  `SKIP`                  | Set the default application policy to `ACCEPT`, `DROP`, `REJECT` or `SKIP`.<br>Please note that setting this to `ACCEPT` may be a security risk |
| `ufw_etc_default_manage_builtins`             |  `false`                 | By default, ufw only touches its own chains.<br>Set this to `true` to have ufw manage the built-in chains too.<br>**Warning: setting this to `true` will break non-ufw managed firewall rules** |
| `ufw_etc_default_ipt_sysctl`                  |  `/etc/ufw/sysctl.conf`  | IPT backend, only enable if using iptables backend |
| `ufw_etc_default_ipt_modules`                 |  `[]`                    | Extra connection tracking modules to load |

### list your ipt modules

If the kernel sources are installed, you can find a complete list in  `net/netfilter/Kconfig`.

Or you can search in the directory of the installed kernel modules::

```bash
find /lib/modules/$(uname -r) -type f -name '*.ko*' | awk -F 'netfilter/' '{ print $2}' | cut -d'.' -f1 | sort
```

As an example, for a simple router, the following modules might be important:

- `nf_conntrack_netlink`
- `nf_defrag_ipv4`
- `nf_nat`
- `nf_reject_ipv4`
- `xt_MASQUERADE`

## Example

```yaml
---
- hosts: all
  roles:
    - ufw
```

### Allow ssh
```yaml
- hosts: all
  roles:
    - ufw
  vars:
    ufw_rules:
      - rule: allow
        to_port: 22
        protocol: tcp
        comment: 'allow incoming connection on standard ssh port'
```

### Allow all traffic on eth1
```yaml
- hosts: all
  roles:
    - ufw
  vars:
    ufw_rules:
      - rule: allow
        interface: eth1
        to_port: ''
        comment: 'allow all traffic on interface eth1'
```

### Allow snmp traffic from 1.2.3.4 on eth0
```yaml
- hosts: all
  roles:
    - ufw
  vars:
    ufw_rules:
      - rule: allow
        interface: eth0
        from_ip: 1.2.3.4
        to_port: 161
        protocol: udp
```

## License

MIT

## Author Information

- Mischa ter Smitten (based on work of weareinteractive)
- Bodo Schulz

