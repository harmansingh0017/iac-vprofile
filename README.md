# Automated Infrastructure Deployment using Terraform and GitHub Actions

This repository contains Infrastructure as Code (IaC) for deploying the Vprofile application using Terraform and GitHub Actions.

## Workflow

The `terraform` job will run on pushes to `main` and `stage` branches as well as pull requests targeting those branches.

It executes the following steps:

- Checkout code
- Setup Terraform
- Initialize Terraform 
- Format check
- Validate
- Plan
- Apply (only on `main` branch push)

The terraform code will be applied to the AWS account using the provided credentials.

It will create/update resources in the `us-east-1` region.

The remote backend uses an S3 bucket specified in `BUCKET_TF_STATE` secret. 

## Resources

The Terraform code will create the following resources:

- VPC
- Subnets (Public and Private)
- Internet Gateway
- Route Tables
- EKS Cluster
- Node Groups
- Security Groups

After deploying the EKS cluster, it will also deploy the NGINX ingress controller using `kubectl`.

## Secrets

The following secrets need to be configured:

- `AWS_ACCESS_KEY_ID` 
- `AWS_SECRET_ACCESS_KEY`
- `BUCKET_TF_STATE`
- `EKS_CLUSTER` - Name of the EKS cluster

## Conclusion

This allows safely managing IaC for EKS using Terraform applied through CI/CD. Changes will be verified by Terraform plan before getting applied.
