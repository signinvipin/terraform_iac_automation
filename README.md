# terraform-iac-automation

Infrastructure as code with Terraform: automated Azure resource provisioning, run through an Azure DevOps pipeline. This is a lab/learning project, currently being restructured into modules.

## Status
- ✅ v1: single-file (non-modular) configuration in `azuretf/simpletf/`
- 🚧 Next: split into a modular structure (providers, variables, reusable modules for networking, Key Vault, storage and VMs)

## What it provisions
- Resource group in Australia East, with Azure Policy requiring tags (managed-by, deployment-id, environment)
- User-assigned managed identity, storage account encrypted with a customer-managed key (Terraform state backend)
- Key Vault with keys and secrets, accessed through RBAC and a private DNS zone
- Virtual networks (public/private subnets), NSGs, peering, NAT gateways, route tables, Bastion
- Linux and Windows VMs with disk encryption

## Structure

```text
azuretf/
├── simpletf/
│   ├── main.tf               # providers, resources, variables, outputs (v1, single file)
│   ├── provider.tf           # placeholder for the modular refactor
│   ├── variables.tf          # placeholder for the modular refactor
│   ├── azure-pipelines.yml   # Azure DevOps pipeline (init, plan, apply)
│   ├── runterraform.sh       # runs Terraform commands with logging
│   └── pushlogs.sh           # pushes run logs to the terraform-logs branch
├── azure_cli_guide.txt       # Azure CLI notes
└── terraform_guide.txt       # setup and troubleshooting notes
```

## Pipeline
- Runs on a self-hosted agent (on an EC2 instance)
- Reads Service Principal details from Azure Key Vault
- Terraform init → plan → apply, with logs saved per run

## Usage

```bash
cd azuretf/simpletf
export ARM_SUBSCRIPTION_ID="<your-subscription-id>"
terraform init
terraform plan
terraform apply
```

## Notes
- Subscription IDs and secrets in the notes are masked
- Never commit `*.tfstate`, `*.tfvars` or secrets (see `.gitignore`)
