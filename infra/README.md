# Infrastructure Setup Guide for StockTradePro

This guide outlines the infrastructure-as-code (IaC) setup used to deploy the StockTradePro application on AWS using Terraform. It is intended for DevOps engineers and infrastructure maintainers.

---

## Folder Purpose

The `infra/` directory contains all Terraform configurations, backend state management setup, and environment-specific provisioning instructions. It enables reproducible and scalable infrastructure management on AWS.

---

## Prerequisites

Before using the infrastructure code, ensure the following:

- [ ] AWS CLI is installed and configured (`aws configure`)
- [ ] Terraform v1.4+ is installed
- [ ] An AWS IAM user with necessary permissions (EC2, S3, IAM, ECS, etc.)
- [ ] Terraform backend (e.g., S3 bucket + DynamoDB) is set up for remote state (if applicable)
- [ ] SSH key pair (for EC2 instances, if applicable)

---

## Structure Overview

```

infra/
├── main.tf               # Root module
├── variables.tf          # Input variables
├── outputs.tf            # Output values
├── providers.tf          # AWS provider config
├── modules/              # Reusable module definitions
│   ├── vpc/
│   ├── ec2/
│   ├── s3/
│   ├── rds/
│   └── ecs/
├── env/
│   ├── dev/
│   └── prod/
└── README.md             # This file

````

---

## Setup Instructions

1. **Clone the Repository**

   ```bash
   git clone https://github.com/your-org/stock-trade-pro.git
   cd stock-trade-pro/infra
````

2. **Initialize Terraform**

   ```bash
   terraform init
   ```

3. **Select or Create Workspace**

   ```bash
   terraform workspace new dev   # Or use 'prod' if deploying to production
   terraform workspace select dev
   ```

4. **Review and Customize Variables**

   Update `env/dev/terraform.tfvars` as needed for environment-specific values (e.g., region, instance type, app name, ports, etc.).

5. **Plan the Infrastructure**

   ```bash
   terraform plan -var-file="env/dev/terraform.tfvars"
   ```

6. **Apply Changes**

   ```bash
   terraform apply -var-file="env/dev/terraform.tfvars"
   ```

7. **Verify Resources**

   After successful deployment, validate that the AWS resources (VPC, EC2, ECS, RDS, etc.) are created as expected in the AWS Console.

---

## Maintenance & Operations

* To destroy the infrastructure:

  ```bash
  terraform destroy -var-file="env/dev/terraform.tfvars"
  ```

* To upgrade modules or provider plugins:

  ```bash
  terraform init -upgrade
  ```

* Rotate secrets and update backend configurations as per your organization's security policies.

---

## Environment Separation

Use workspaces and `env/` folders to manage multiple environments (e.g., `dev`, `stage`, `prod`). Ensure that each environment has isolated state files and variable overrides.

---

## ✅ Best Practices

* Enable version control and locking for remote backend state.
* Use Terraform Cloud or GitHub Actions for CI/CD automation.
* Apply tagging for all AWS resources.
* Store secrets in AWS Secrets Manager (never in code or plain text).
* Enable monitoring via CloudWatch and log shipping for observability.

---

## 🤝 Contributors

This infrastructure setup is maintained by the DevOps team at **StockTradePro**. For questions or contributions, contact the infrastructure lead or open an issue in the main repository.