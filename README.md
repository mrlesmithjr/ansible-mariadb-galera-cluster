# ansible-mariadb-galera-cluster

An [Ansible](https://www.ansible.com) role to deploy and manage a [MariaDB Galera Cluster](https://mariadb.com/kb/en/what-is-mariadb-galera-cluster/) for high-availability MySQL.

## Ansible Galaxy

```bash
ansible-galaxy install mrlesmithjr.mariadb_galera_cluster
```

## Features

- Automatic cluster bootstrap and node joining
- TLS encryption for MySQL client, WSREP, and SST connections
- Multiple SST methods: `rsync`, `mariabackup`
- InnoDB tuning with automatic memory calculations
- Slow query logging and email notifications for cluster events
- Database and user management via variables

## Supported Platforms

| Platform | Versions |
|----------|----------|
| Ubuntu | 20.04, 22.04, 24.04 |
| Debian | 11, 12, 13 |
| Rocky Linux / RHEL | 8, 9, 10 |
| Fedora | 39+ |

## Requirements

### Ansible Collections

```bash
ansible-galaxy collection install community.mysql
```

### Minimum Ansible Version

Ansible 9.1+

## Quick Start

### 1. Inventory

```ini
[galera-cluster-nodes]
node1 ansible_host=192.168.1.10
node2 ansible_host=192.168.1.11
node3 ansible_host=192.168.1.12

[galera-cluster-nodes:vars]
galera_cluster_bind_interface=eth0
```

### 2. Playbook

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

### 3. Run

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
| `galera_enable_mariadb_repo` | `true` | Use official MariaDB repositories |

### Cluster Settings

| Variable | Default | Description |
|----------|---------|-------------|
| `galera_cluster_name` | `"vagrant-test"` | Galera cluster name |
| `galera_cluster_nodes_group` | `"galera-cluster-nodes"` | Ansible group containing cluster nodes |
| `galera_cluster_bind_interface` | `"eth0"` | Network interface for cluster communication |
| `galera_sst_method` | `"rsync"` | State snapshot transfer method |

### TLS Configuration

```yaml
galera_wsrep_tls_enabled: true
galera_sst_tls_enabled: true
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

See [defaults/main.yml](defaults/main.yml) for all available variables.

## Cluster Operations

### Reconfigure an existing cluster

```yaml
galera_reconfigure_galera: true
```

### Upgrade MariaDB version

```yaml
mariadb_upgrade: true
```

## Testing

```bash
pip install molecule molecule-docker
molecule test
```

## Support This Project

If your organization runs this role in production, consider
[sponsoring its maintenance](https://github.com/sponsors/mrlesmithjr).
Enterprise tiers with priority support and 48-hour issue triage are available.

## License

MIT

## Author

Larry Smith Jr. — [everythingshouldbevirtual.com](http://everythingshouldbevirtual.com) · [mrlesmithjr@gmail.com](mailto:mrlesmithjr@gmail.com)
