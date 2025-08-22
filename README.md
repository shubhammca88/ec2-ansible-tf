# Multi-Server EC2 Infrastructure

Terraform configuration for deploying 4 EC2 servers with different environments and security groups.

## Architecture

- **Configurable EC2 instances** (Free Tier eligible)
- **Custom instance types** per server via array
- **Individual Security Groups** with dynamic port configuration
- **Multiple Environments**: dev, staging, prod, test
- **Default VPC** with automatic AZ selection

## Server Configuration

| Server | Environment | Ports |
|--------|-------------|-------|
| master | dev | 22, 80 |
| server1 | staging | 22, 8000 |
| server2 | prod | 22, 3306 |
| server3 | test | 22, 443 |

## Variables

- `server_count`: Number of servers (default: 1, limited by vCPU quota)
- `server_names`: Server names array
- `environments`: Environment tags for each server
- `server_ports`: Port configuration per server
- `instance_types`: Custom instance type for each server
- `region`: AWS region (default: us-east-1)

## Prerequisites

1. AWS CLI configured:
   ```bash
   aws configure
   ```

2. SSH key pair:
   ```bash
   ssh-keygen -t rsa -b 2048 -f id_rsa
   ```

## Deployment

```bash
terraform init
terraform plan
terraform apply
```

### Troubleshooting

**InsufficientInstanceCapacity errors:**
1. Change instance types in `instance_types` array
2. Try different region in terraform.tfvars
3. Wait and retry later

**VcpuLimitExceeded errors:**
1. Reduce `server_count` to fit within vCPU limit
2. Request vCPU limit increase via AWS Service Quotas
3. Use smaller instance types (t2.nano = 1 vCPU)

## Outputs

- `server_ips`: All server public IP addresses

## Cleanup

```bash
terraform destroy
```

## Files

- `main.tf` - EC2 instances with count
- `network.tf` - Dynamic security groups and key pair
- `variables.tf` - Input variables
- `outputs.tf` - Server IP outputs
- `terraform.tfvars` - Variable values
- `mydeploy.sh` - Server setup script