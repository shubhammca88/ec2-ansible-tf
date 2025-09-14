# Multi-Environment EC2 with Ansible

Terraform + Ansible setup for managing 4 EC2 servers across different environments.

## Infrastructure

| Server | Environment | Purpose |
|--------|-------------|----------|
| master | dev | Ansible control node |
| server1 | staging | Staging environment |
| server2 | prod | Production database |
| server3 | test | Testing with nginx |

## Quick Start

1. **Deploy Infrastructure**
   ```bash
   terraform init
   terraform apply
   ```

2. **Setup Ansible** (on master server)
   ```bash
   ssh -i id_rsa ubuntu@<master-ip>
   sudo apt update && sudo apt install -y ansible
   ```

3. **Run Playbooks**
   ```bash
   ansible-playbook -i inventory playbook/install_nginx.yml
   ```

## Ansible Inventory

The `inventory` file automatically groups servers by environment:
- `[dev]` - master server
- `[staging]` - server1
- `[prod]` - server2  
- `[test]` - server3

## Management

- **Master server** has Ansible installed for managing all environments
- **Inventory file** contains all server IPs and SSH configuration
- **Playbooks** in `playbook/` directory for automation tasks
- **Environment-specific** deployments using inventory groups

## Cleanup

```bash
terraform destroy
```