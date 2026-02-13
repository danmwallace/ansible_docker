# Ansible Role: ansible_docker

Base Docker service deployment role. This role provides a standardized pattern for deploying Docker..

## Description

This role installs Docker, Docker Compose, and related dependencies on Ubuntu, Fedora Server, and Fedora IoT

## Requirements

- Ansible 2.15+
- Target OS: Ubuntu 20.04+, Debian 11+
- Docker and Docker Compose must be installed (use `ansible_common` role first)
- `community.docker` collection

## Role Variables

### Required Variables


## Dependencies

None (but assumes Docker is installed, typically via `ansible_common`).

## Example Playbook

### Basic Usage

Run this role before one of my more specialized docker roles, that set up applications:
```yaml
---
- name: Deploy Docker service
  hosts: servers
  become: true
  roles:
    - role: ansible_docker
    - role: ansible_docker_some_application
```

### As a Dependency

Most docker service roles use this pattern in their `meta/main.yml`:

```yaml
dependencies:
  - role: ansible_docker
```

## What This Role Does

1. **Adds the GPG Key from Docker**
2. **Adds the Docker Repository**
3. **Installs Docker Compose**:

## Directory Structure

This app is not responsible for the directory structure that I use, but in case you are using this with one of my application deployment roles, you will find them here:

```
/opt/docker/
└── {service_name}/
    ├── data/              # Service data directory
    └── docker-compose.yml # Generated compose file
```

## Tags

- `ansible_docker`, `docker` 

## Common Issues

### Permission Denied

If containers can't write to data directory:
- Check directory ownership
- Verify container user IDs match
- Adjust `mode` parameter if needed

### Container Not Updating

If containers don't update:
- Verify template changes are being applied
- Check if `state: absent` is working
- Use `pull: always` in docker_compose_v2 call

## Testing

```bash
# Test deployment
ansible-playbook --check -i inventory playbooks/deploy.yml

# Verify containers are running
docker compose -f /opt/docker/service/docker-compose.yml ps

# Check logs
docker compose -f /opt/docker/service/docker-compose.yml logs
```

## License

MIT

## Author

Dan Wallace

## See Also

- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [community.docker.docker_compose_v2](https://docs.ansible.com/ansible/latest/collections/community/docker/docker_compose_v2_module.html)
