# {17711 Mesh} Ansible Role: Stack Do DNS.

> Deploy a decentralized and privacy-focused Stack Do DNS for enhanced security within The 17711 Mesh.

## Table of Contents

- [Synopsis](#synopsis)
- [Changelog](#changelog)
- [License](#license)
- [Get started](#get-started)
- [Prerequisites](#prerequisites)
- [Example with inventory](#example-with-inventory)

## Synopsis

Deploy a decentralized and privacy-focused Stack Do DNS for enhanced security within The 17711 Mesh.

## Get started

### Prerequisites

This role requires Ansible to be installed on the control node. Here are some ways to install it:

**Using pip (recommended):**

```bash
python3 -m pip install --user ansible
```

**Using a package manager (e.g., apt on Debian/Ubuntu):**

```bash
sudo apt update
sudo apt install ansible
```

For more information, see the [Ansible installation guide](https://docs.ansible.com/ansible/latest/installation_guide/index.html).

### Installation

This role can be installed directly from the Git repository using `ansible-galaxy`:

```bash
ansible-galaxy install git+https://github.com/17711-mesh/ansible-role.stack-do-dns.git
```

### Example Playbook

Here is an example of how to use this role in a playbook:

```yaml
- hosts: dns_servers
  roles:
    - role: ansible-role.stack-do-dns
      vars:
        mesh_17711_stack_do__dns_stack_enable_adguard: false # Disable AdGuard Home if only Unbound is desired
        mesh_17711_stack_do__dns_adguard_dns_bind_ipv4:
          - "192.168.1.50" # Custom IPv4 address for AdGuard Home
        mesh_17711_stack_do__dns_adguard_dns_bind_ipv6: [] # Disable IPv6 binding for AdGuard Home
        mesh_17711_stack_do__dns_stack_ops_user: "dnsuser" # Run services under a different user
```

### Example with inventory

Here is an example of how to use this role with an inventory file:

**Inventory file (`inventory.yml`):**

```yaml
all:
  hosts:
    dns_server_1:
      ansible_host: 192.168.1.50
      ansible_user: root
```

**Playbook file (`playbook.yml`):**

```yaml
- hosts: all
  roles:
    - role: ansible-role.stack-do-dns
```

**Command:**

```bash
ansible-playbook -i inventory.yml playbook.yml
```

### Role Variables

Here is a list of variables that can be overridden:

| Variable | Default | Description |
|---|---|---|
| `mesh_17711_stack_do__dns_stack_enable_unbound` | `true` | Enable Unbound service. |
| `mesh_17711_stack_do__dns_stack_enable_adguard` | `true` | Enable AdGuard Home service. |
| `mesh_17711_stack_do__dns_stack_timezone` | `"Europe/Paris"` | Timezone for the services. |
| `mesh_17711_stack_do__dns_stack_podman_compose_dir` | `"/data/17711/do/dns"` | Directory for podman-compose files. |
| `mesh_17711_stack_do__dns_unbound_config_dir` | `"/data/17711/do/dns/config"` | Directory for Unbound configuration. |
| `mesh_17711_stack_do__dns_adguard_work_dir` | `"/data/17711/do/adguard/work"` | AdGuard Home work directory. |
| `mesh_17711_stack_do__dns_adguard_conf_dir` | `"/data/17711/do/adguard/conf"` | AdGuard Home configuration directory. |
| `mesh_17711_stack_do__dns_unbound_image` | `"docker.io/crazymax/unbound:1.24.0"` | Unbound container image. |
| `mesh_17711_stack_do__dns_adguard_image` | `"docker.io/adguard/adguardhome:v0.107.69"` | AdGuard Home container image. |
| `mesh_17711_stack_do__dns_stack_ops_user` | `"ops"` | User that owns and runs the DNS stack. |
| `mesh_17711_stack_do__dns_stack_compose_dir` | `"/home/{{ mesh_17711_stack_do__dns_stack_ops_user }}/.config/17711"` | Compose file directory for the user. |
| `mesh_17711_stack_do__dns_stack_compose_file` | `"{{ mesh_17711_stack_do__dns_stack_compose_dir }}/stack-do-dns.yml"` | Path to the compose file. |
| `mesh_17711_stack_do__dns_stack_quadlet_dir` | `"/home/{{ mesh_17711_stack_do__dns_stack_ops_user }}/.config/containers/systemd"` | Quadlet directory for user services. |
| `mesh_17711_stack_do__dns_stack_quadlet_unit_name` | `"stack-do-dns"` | Name of the quadlet systemd user unit. |
| `mesh_17711_stack_do__dns_stack_quadlet_enable` | `true` | Create and enable the quadlet systemd user unit. |
| `mesh_17711_stack_do__dns_enable_ipv6` | `true` | Enable IPv6 support. |
| `mesh_17711_stack_do__dns_bridge_lan_ipv4_cidr` | `"192.168.1.0/24"` | LAN IPv4 CIDR. |
| `mesh_17711_stack_do__dns_wg_ipv4_cidr` | `"10.177.11.0/24"` | WireGuard IPv4 CIDR. |
| `mesh_17711_stack_do__dns_private_ipv4_ranges` | `[ "10.0.0.0/8", "172.16.0.0/12", "192.168.0.0/16", "{{ mesh_17711_stack_do__dns_wg_ipv4_cidr }}" ]` | Private IPv4 ranges. |
| `mesh_17711_stack_do__dns_private_ipv6_ranges` | `[ "fd00::/8", "fe80::/10" ]` | Private IPv6 ranges. |
| `mesh_17711_stack_do__dns_allowed_ipv4_cidrs` | `[ "{{ mesh_17711_stack_do__dns_bridge_lan_ipv4_cidr }}", "{{ mesh_17711_stack_do__dns_wg_ipv4_cidr }}" ]` | Allowed IPv4 CIDRs for DNS queries. |
| `mesh_17711_stack_do__dns_allowed_ipv6_cidrs` | `"{{ mesh_17711_stack_do__dns_private_ipv6_ranges }}"` | Allowed IPv6 CIDRs for DNS queries. |
| `mesh_17711_stack_do__dns_admin_hostname_internal` | `"thierry1.mesh.17711"` | Internal hostname for the admin interface. |
| `mesh_17711_stack_do__dns_admin_port` | `3000` | Admin interface port. |
| `mesh_17711_stack_do__dns_adguard_dns_bind_ipv4` | `[ "192.168.1.10" ]` | AdGuard Home DNS listen IPv4 addresses. |
| `mesh_17711_stack_do__dns_adguard_dns_bind_ipv6` | `[ "2001:861:4748:81c0:dea6:32ff:fedf:bb90" ]` | AdGuard Home DNS listen IPv6 addresses. |
| `mesh_17711_stack_do__dns_use_forwarders` | `false` | Use upstream DNS forwarders instead of recursive mode. |
| `mesh_17711_stack_do__dns_forwarders` | `[ { addr: "1.1.1.1", tls_host: "cloudflare-dns.com", port: 853 }, { addr: "1.0.0.1", tls_host: "cloudflare-dns.com", port: 853 } ]` | List of DNS-over-TLS forwarders. |


## Changelog

See [CHANGELOG](CHANGELOG.md) for a detailed history of changes.

## License

>
> MIT License
>
> Copyright (c) 2025, 🐌 [The 17711 Frame](https://17711.org)
> 
> Permission is hereby granted, free of charge, to any person obtaining a copy
> of this software and associated documentation files (the "Software"), to deal
> in the Software without restriction, including without limitation the rights
> to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
> copies of the Software, and to permit persons to whom the Software is
> furnished to do so, subject to the following conditions:
> 
> The above copyright notice and this permission notice shall be included in all
> copies or substantial portions of the Software.
> 
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
> IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
> FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
> AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
> LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
> OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
> SOFTWARE.
> 
