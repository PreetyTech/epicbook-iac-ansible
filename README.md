# EpicBook — AWS Deployment with Terraform and Ansible

A production-style deployment of the EpicBook Node.js bookstore application on AWS.
Terraform provisions the infrastructure. Ansible configures and deploys the application.

## What This Deploys

- Ubuntu EC2 instance (application server)
- RDS MySQL instance (isolated database tier)
- Nginx reverse proxy (port 80 → Node.js on port 3000)
- PM2 process manager (keeps Node.js running on reboot)
- VPC, subnets, security groups, Elastic IP

## Project Structure
epicbook-iac-ansible/
├── terraform/
│   └── aws/
│       ├── main.tf           # All AWS resources
│       ├── providers.tf      # AWS provider config
│       ├── variables.tf      # Input variables
│       ├── outputs.tf        # Public IP, DB host, admin user
│       └── terraform.tfvars.example  # Template for your values
└── ansible/
    ├── ansible.cfg           # Ansible configuration
    ├── site.yml              # Orchestration playbook
    ├── inventory.ini.example # Inventory template
    ├── group_vars/
    │   └── web/
    │       ├── main.yml      # Non-sensitive variables
    │       └── vault.yml.example  # Secrets template
    └── roles/
        ├── common/           # System updates + SSH hardening
        ├── nginx/            # Nginx install + reverse proxy config
        └── epicbook/         # App clone + PM2 startup
