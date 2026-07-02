# Ansible Lab

Ansible playbooks and roles lab — configuration management, Jinja2 templates, and infrastructure automation across multiple server types.

## What this demonstrates

- **Role-based structure** — modular roles for webserver, database, monitoring, and common setup
- **Jinja2 templating** — dynamic config generation for NGINX, PostgreSQL, and Prometheus
- **Handlers** — service restarts triggered only on config changes
- **Docker-based lab** — local multi-node environment using Docker Compose (no cloud required)

## Architecture

```
Ansible Control → SSH → 3 target nodes (via Docker Compose)
├── web (port 2201)        → NGINX reverse proxy
├── db (port 2202)         → PostgreSQL
└── monitoring (port 2203) → Prometheus + Grafana config
```

## Roles

| Role | What it does |
|------|-------------|
| `common` | Base packages (curl, wget, htop), deploy user |
| `webserver` | Install NGINX, template config, enable service |
| `database` | Install PostgreSQL, template config, credentials file |
| `monitoring` | Prometheus config via Jinja2 templates |

## Usage

```bash
# Start the lab environment
docker compose up -d

# Run the playbook
ansible-playbook -i inventory.ini site.yml

# Run a specific play
ansible-playbook -i inventory.ini site.yml --tags webserver
```

## Structure

```
├── site.yml                    # Main playbook
├── playbook.yml                # Alternative entrypoint
├── inventory.ini               # Host definitions (local Docker)
├── docker-compose.yml          # 3 Ubuntu containers with SSH
├── group_vars/all/vault.yml    # Encrypted variables
└── roles/
    ├── common/tasks/main.yml
    ├── webserver/
    │   ├── tasks/main.yml
    │   ├── templates/nginx.conf.j2
    │   └── handlers/main.yml
    ├── database/
    │   ├── tasks/main.yml
    │   ├── templates/pg_config.j2
    │   ├── defaults/main.yml
    │   └── handlers/main.yml
    └── monitoring/
        ├── tasks/main.yml
        ├── templates/prometheus.yml.j2
        └── defaults/main.yml
```

## Tags

`ansible` `configuration-management` `jinja2` `automation` `iac`
