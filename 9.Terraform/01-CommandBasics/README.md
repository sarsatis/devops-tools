# 1.Course GitHub Repository Links

## GitHub Repository Links

[Course Main Repository: hashicorp-certified-terraform-associate-on-azure](https://github.com/stacksimplify/hashicorp-certified-terraform-associate-on-azure)

[Terraform Cloud Demo: terraform-cloud-azure-demo1](https://github.com/stacksimplify/terraform-cloud-azure-demo1)

[Terraform Public Module Registry: terraform-azurerm-staticwebsitepublic](https://github.com/stacksimplify/terraform-azurerm-staticwebsitepublic)

[Terraform Private Module Registry: terraform-azurerm-staticwebsiteprivate](https://github.com/stacksimplify/terraform-azurerm-staticwebsiteprivate)

[Terraform Sentinel Policies: terraform-sentinel-policies-azure](https://github.com/stacksimplify/terraform-sentinel-policies-azure)

[Course Presentation: Presentation Slides PPT](https://github.com/stacksimplify/hashicorp-certified-terraform-associate-on-azure/tree/main/course-presentation)

# 2.Infrastructure as Code Basics

## Step-01: Understand Problems with Traditional way of Managing Infrastructure
- Time it takes for building multiple environments
- Issues we face with different environments
- Scale-Up and Scale-Down On-Demand

## Step-02: Discuss how IaC with Terraform Solves them
- Visibility
- Stability
- Scalability
- Security
- Audit

## Step-03: MACOS: IDE for Terraform - VS Code Editor
- [Microsoft Visual Studio Code Editor](https://code.visualstudio.com/download)
- [Hashicorp Terraform Plugin for VS Code](https://marketplace.visualstudio.com/items?itemName=HashiCorp.terraform)
- Configure [Course Github Repository](https://github.com/stacksimplify/hashicorp-certified-terraform-associate-on-azure) using VS Code Editor


### MAC OS
```
# Install on MAC OS
brew install hashicorp/tap/terraform

# Verify Terraform Version
terraform version

# To Upgrade on MAC OS
brew upgrade hashicorp/tap/terraform

# Verify Terraform Version
terraform version

# Verify Installation
terraform help
terraform help plan

# Enable Tab Completion
terraform -install-autocomplete
```

### Install Azure CLI
```
# AZ CLI Current Version (if installed)
az --version

# Install Azure CLI (if not installed)
brew update && brew install azure-cli

# Upgrade az cli version
az --version
brew upgrade azure-cli 
[or]
az upgrade
az --version

# Azure CLI Login
az login

# List Subscriptions
az account list

# Set Specific Subscription (if we have multiple subscriptions)
az account set --subscription="SUBSCRIPTION_ID"
```






