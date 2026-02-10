# ansible-mariadb-galera-cluster

An [Ansible](https://www.ansible.com) role to deploy and manage a [MariaDB Galera Cluster](https://mariadb.com/kb/en/mariadb/what-is-mariadb-galera-cluster/) for high availability MySQL.

## Features

- Automatic cluster bootstrap and node joining
- Support for MariaDB 10.x versions
- TLS encryption for MySQL, WSREP, and SST connections
- Multiple SST methods (rsync, mariabackup)
- InnoDB tuning with automatic memory calculations
- Slow query logging
- Email notifications for cluster events
- Database and user management

## Supported Platforms

| Platform | Versions |
|----------|----------|
| Ubuntu | 16.04 (Xenial), 18.04 (Bionic), 20.04 (Focal), 22.04 (Jammy) |
| Debian | 11 (Bullseye) |
| EL/CentOS/Rocky | 8, 9 |
| Fedora | 39 |

## Requirements

### Collections

```bash
ansible-galaxy collection install community.mysql
```

### Minimum Ansible Version

- Ansible 9.1+

## Quick Start

### 1. Create inventory

```ini
[galera-cluster-nodes]
node1 ansible_host=192.168.1.10
node2 ansible_host=192.168.1.11
node3 ansible_host=192.168.1.12

[galera-cluster-nodes:vars]
galera_cluster_bind_interface=eth0
```

### 2. Create playbook

```yaml
---
- hosts: galera-cluster-nodes
  become: true
  vars:
    mariadb_version: "10.11"
    galera_cluster_name: "my-galera-cluster"
    mariadb_mysql_root_password: "secure_password_here"
  roles:
    - role: mrlesmithjr.mariadb_galera_cluster
```

### 3. Run playbook

```bash
ansible-playbook -i inventory playbook.yml
```

## Key Variables

### MariaDB Settings

| Variable | Default | Description |
|----------|---------|-------------|
| `mariadb_version` | `"10.11"` | MariaDB version to install |
| `mariadb_mysql_root_password` | `"root"` | MySQL root password |
| `mariadb_mysql_port` | `3306` | MySQL listen port |
| `galera_enable_mariadb_repo` | `true` | Use official MariaDB repos |

### Cluster Settings

| Variable | Default | Description |
|----------|---------|-------------|
| `galera_cluster_name` | `"vagrant-test"` | Name of the Galera cluster |
| `galera_cluster_nodes_group` | `"galera-cluster-nodes"` | Ansible group containing cluster nodes |
| `galera_cluster_bind_interface` | `"eth0"` | Network interface for cluster communication |
| `galera_sst_method` | `"rsync"` | State snapshot transfer method |

### TLS Configuration

```yaml
mariadb_tls_files:
  ca_cert:
    name: "ca.pem"
    content: "{{ lookup('file', 'certs/ca.pem') }}"
  server_key:
    name: "server-key.pem"
    content: "{{ lookup('file', 'certs/server-key.pem') }}"
  server_cert:
    name: "server-cert.pem"
    content: "{{ lookup('file', 'certs/server-cert.pem') }}"

galera_wsrep_tls_enabled: true
galera_sst_tls_enabled: true
```

### Database and User Management

```yaml
mariadb_databases:
  - name: myapp
  - name: analytics
    init_script: files/init_analytics.sql

mariadb_mysql_users:
  - name: myapp_user
    password: "secret"
    hosts:
      - "%"
      - "localhost"
    priv: "myapp.*:ALL"
```

## Role Variables

See [defaults/main.yml](defaults/main.yml) for all available variables.

## Cluster Operations

### Reconfigure Cluster

To reconfigure an existing cluster (triggers shutdown and bootstrap):

```yaml
galera_reconfigure_galera: true
```

### Upgrading MariaDB

To allow MariaDB version upgrades:

```yaml
mariadb_upgrade: true
```

## Dependencies

None

## Example Playbook

See [playbook.yml](./playbook.yml) for a complete example.

## Testing

This role includes Molecule tests:

```bash
pip install molecule molecule-docker
cd ansible-mariadb-galera-cluster
molecule test
```

## License

MIT

## Author Information

Larry Smith Jr.

- [@mrlesmithjr](https://twitter.com/mrlesmithjr)
- [mrlesmithjr@gmail.com](mailto:mrlesmithjr@gmail.com)
- [http://everythingshouldbevirtual.com](http://everythingshouldbevirtual.com)

<a href="https://www.buymeacoffee.com/mrlesmithjr" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>
