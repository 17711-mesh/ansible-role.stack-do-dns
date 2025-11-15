# {17711 Mesh} Ansible Role: Stack Do DNS.

> Deploy a decentralized and privacy-focused Stack Do DNS for enhanced security within The 17711 Mesh.

## Table of Contents

- [Synopsis](#synopsis)
- [Requirements](#requirements)
- [Role Variables](#role-variables)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
- [Testing / Molecule](#testing--molecule)
- [License](#license)
- [Author Information](#author-information)

## Synopsis

Deploy a decentralized and privacy-focused Stack Do DNS for enhanced security within The 17711 Mesh.

## Requirements

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

This role depends on `mesh_17711.compose_to_quadlet`. This dependency is declared in `meta/main.yml`. To install this role and its dependencies, navigate to the role's directory and run:

```bash
ansible-galaxy install -r meta/main.yml
```

You can override the `ansible_role_compose_to_quadlet_version` by passing it as an extra variable:

```bash
ansible-galaxy install -r meta/main.yml -e "ansible_role_compose_to_quadlet_version=v0.1.1"
```

## Role Variables

Here is a list of variables that can be overridden:

| Variable | Default | Description |
|---|---|---|
| `mesh_17711_stack_do__dns_stack_enable_unbound` | `true` | Enable Unbound service. |
| `mesh_17711_stack_do__dns_stack_enable_adguard` | `true` | Enable AdGuard Home service. |
| `mesh_17711_stack_do__dns_stack_timezone` | `"Europe/Paris"` | Timezone for the services. |
| `mesh_17711_stack_do__dns_stack_quadlet_enable` | `true` | Create and enable the quadlet systemd user unit. |
| `ansible_role_compose_to_quadlet_version` | `"v0.1.0"` | Version of the `mesh_17711.compose_to_quadlet` role to install from Git. |
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

## Dependencies

This role depends on the `mesh_17711.compose_to_quadlet` role. This dependency is managed via `meta/main.yml`.

## Example Playbook

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

## Testing / Molecule

This role includes a Molecule test suite to ensure its functionality and idempotence. The tests use the `podman` driver, so Podman must be installed and running on your system.

### Prerequisites for Testing

1.  **Podman:** Ensure Podman is installed and running.
2.  **Python Virtual Environment:** It is highly recommended to run Molecule tests within a Python virtual environment.

### How to Run Tests

1.  **Navigate to the role directory:**
    ```bash
    cd ansible-role.stack-do-dns
    ```

2.  **Create and activate a Python virtual environment:**
    ```bash
    python3 -m venv venv
    source venv/bin/activate
    ```

3.  **Install testing dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Run Molecule tests:**
    *   **Create the test instance:**
        ```bash
        molecule create
        ```
    *   **Converge the role on the instance:**
        ```bash
        molecule converge
        ```
    *   **Verify the role's outcome:**
        ```bash
        molecule verify
        ```
    *   **Run the full test sequence (create, converge, verify, destroy):**
        ```bash
        molecule test
        ```
    *   **Clean up the test instance:**
        ```bash
        molecule destroy
        ```

The Molecule scenario verifies the integration between `ansible-role.stack-do-dns` and `ansible-role.compose-to-quadlet` by checking for the presence of generated quadlet unit files and ensuring the role is idempotent.

## License

>
> [MIT License](LICENCE)
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

## Author Information

This project is maintained by 🐌 [The 17711 Frame](https://17711.org).