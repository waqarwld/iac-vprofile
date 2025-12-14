# Copilot Instructions for iac-vprofile

## Project Overview
This is a Terraform-based Infrastructure as Code (IaC) project for provisioning AWS resources for the vprofile application stack. It sets up a VPC and an EKS cluster using community modules.

## Architecture
- **Modules**: Uses `terraform-aws-modules/vpc/aws` for VPC creation and `terraform-aws-modules/eks/aws` for EKS cluster provisioning.
- **Key Components**:
  - VPC with private/public subnets across 3 AZs (CIDR: 172.20.0.0/16).
  - EKS cluster (v1.27) with two managed node groups using t3.small instances.
- **Data Flow**: VPC module provides subnet IDs to EKS module; Kubernetes provider connects to the cluster endpoint.
- **Why this structure**: Modular approach allows independent management of network and compute resources, following AWS best practices for EKS.

## Developer Workflows
- **Initialization**: Run `terraform init` to download providers and modules.
- **Validation**: Use `terraform fmt -check` for formatting check, `terraform validate` for syntax validation.
- **Planning**: Execute `terraform plan -out planfile` to generate a plan file.
- **Deployment**: Apply with `terraform apply -auto-approve -input=false -parallelism=1 planfile` for automated, sequential execution.
- **State Management**: Remote backend in S3 bucket `gitopsterrastate3050` (us-east-2) ensures shared state.

## Conventions and Patterns
- **File Structure**: Separate files for providers (`main.tf`), modules (`eks-cluster.tf`, `vpc.tf`), variables (`variables.tf`), outputs (`outputs.tf`), and config (`terraform.tf`).
- **Variable Naming**: Use camelCase for variable names (e.g., `clusterName`).
- **Outputs**: Expose essential cluster details like name, endpoint, region, and security group ID.
- **Provider Configuration**: AWS provider set to region variable; Kubernetes provider dynamically configured from EKS module outputs.
- **Subnet Tagging**: Private and public subnets tagged for Kubernetes ELB integration.

## Integration Points
- **AWS Services**: Relies on EC2, VPC, EKS, and S3 for backend.
- **External Dependencies**: Terraform registry modules; no custom providers.
- **Cross-Component Communication**: VPC module outputs fed into EKS module; no inter-service APIs.

## Key Files
- `terraform/main.tf`: Providers and locals.
- `terraform/eks-cluster.tf`: EKS cluster configuration.
- `terraform/vpc.tf`: VPC and subnet setup.
- `terraform/variables.tf`: Input variables with defaults.
- `terraform/outputs.tf`: Output values for cluster access.</content>
<parameter name="filePath">c:/Users/waqar/desktop/devops4sure/iac-vprofile/.github/copilot-instructions.md