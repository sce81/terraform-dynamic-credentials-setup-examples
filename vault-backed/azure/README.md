# Bootstrapping trust between a TFC workspace and Azure using Vault-Backed Azure Secrets Engine

This directory contains example code for setting up a Terraform Cloud workspace whose runs will be automatically authenticated to Azure using Workload Identity and Vault's Azure Secrets Engine.

It contains the necessary configuration for adding both the Azure Secrets Engine and JWT/OIDC authentication to your Vault instance. It assumes you already have an existing Azure service principle and resource group. If you don't, you can create everything you need by following [this section](https://developer.hashicorp.com/vault/tutorials/secrets-management/azure-secrets#create-an-azure-service-principal-and-resource-group) of the tutorial for setting up an Azure Secrets Engine in Vault.

## How to use

You'll need the Terraform CLI installed, and you'll need to set the following environment variables in your local shell:

1. `VAULT_TOKEN`: the Vault token that you'll use to bootstrap your trust configuration in Vault. It will need the ability to enable auth backends and create roles and policies.
1. `TFE_TOKEN`: a Terraform Cloud user token with permission to create workspaces within your organization.

You'll also need to authenticate the Azure provider as you would normally using one of the methods mentioned in the Azure provider documentation [here](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs#authenticating-to-azure).

Copy `terraform.tfvars.example` to `terraform.tfvars` and customize the required variables. You can also set values for any other variables you'd like to customize beyond the default. You can acquire the majority of these variables by following [this section](https://developer.hashicorp.com/vault/tutorials/secrets-management/azure-secrets#create-an-azure-service-principal-and-resource-group) of the tutorial for setting up an Azure Secrets Engine in Vault.

Run `terraform plan` to verify your setup, and then run `terraform apply`.

Congratulations! You now have a Terraform Cloud workspace where runs will automatically authenticate to Azure when using the AzureRM or AzureAD Terraform providers.
<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
| ---- | ------- |
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.15.7 |

## Providers

| Name | Version |
| ---- | ------- |
| <a name="provider_tfe"></a> [tfe](#provider\_tfe) | n/a |
| <a name="provider_vault"></a> [vault](#provider\_vault) | n/a |

## Modules

No modules.

## Resources

| Name | Type |
| ---- | ---- |
| [tfe_variable.enable_azure_provider_auth](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.enable_vault_provider_auth](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.tfc_azure_mount_path](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.tfc_azure_vault_namespace](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.tfc_azure_vault_role](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.tfc_vault_addr](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.tfc_vault_role](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_workspace.my_workspace](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/workspace) | resource |
| [vault_azure_secret_backend.azure_secret_backend](https://registry.terraform.io/providers/hashicorp/vault/latest/docs/resources/azure_secret_backend) | resource |
| [vault_azure_secret_backend_role.azure_secret_backend_role](https://registry.terraform.io/providers/hashicorp/vault/latest/docs/resources/azure_secret_backend_role) | resource |
| [vault_jwt_auth_backend.tfc_jwt](https://registry.terraform.io/providers/hashicorp/vault/latest/docs/resources/jwt_auth_backend) | resource |
| [vault_jwt_auth_backend_role.tfc_role](https://registry.terraform.io/providers/hashicorp/vault/latest/docs/resources/jwt_auth_backend_role) | resource |
| [vault_policy.tfc_policy](https://registry.terraform.io/providers/hashicorp/vault/latest/docs/resources/policy) | resource |
| [tfe_project.tfc_project](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/data-sources/project) | data source |

## Inputs

| Name | Description | Type | Default | Required |
| ---- | ----------- | ---- | ------- | :------: |
| <a name="input_azure_client_id"></a> [azure\_client\_id](#input\_azure\_client\_id) | Azure client ID (WARNING - Will be written to this configuration's state and plan files in plaintext) | `string` | n/a | yes |
| <a name="input_azure_client_secret"></a> [azure\_client\_secret](#input\_azure\_client\_secret) | Azure client secret (WARNING - Will be written to this configuration's state and plan files in plaintext) | `string` | n/a | yes |
| <a name="input_azure_resource_group_name"></a> [azure\_resource\_group\_name](#input\_azure\_resource\_group\_name) | Name of the Azure resource group you wish to provision resources for | `string` | n/a | yes |
| <a name="input_azure_secret_backend_role_name"></a> [azure\_secret\_backend\_role\_name](#input\_azure\_secret\_backend\_role\_name) | Name of Azure secret backend role you created for runs to use | `string` | n/a | yes |
| <a name="input_azure_subscription_id"></a> [azure\_subscription\_id](#input\_azure\_subscription\_id) | Azure subscription ID | `string` | n/a | yes |
| <a name="input_azure_tenant_id"></a> [azure\_tenant\_id](#input\_azure\_tenant\_id) | Azure tenant ID | `string` | n/a | yes |
| <a name="input_jwt_backend_path"></a> [jwt\_backend\_path](#input\_jwt\_backend\_path) | The path at which you'd like to mount the jwt auth backend in Vault | `string` | `"jwt"` | no |
| <a name="input_tfc_hostname"></a> [tfc\_hostname](#input\_tfc\_hostname) | The hostname of the TFC or TFE instance you'd like to use with Vault | `string` | `"app.terraform.io"` | no |
| <a name="input_tfc_organization_name"></a> [tfc\_organization\_name](#input\_tfc\_organization\_name) | The name of your Terraform Cloud organization | `string` | n/a | yes |
| <a name="input_tfc_project_name"></a> [tfc\_project\_name](#input\_tfc\_project\_name) | The project under which a workspace will be created | `string` | `"Default Project"` | no |
| <a name="input_tfc_vault_audience"></a> [tfc\_vault\_audience](#input\_tfc\_vault\_audience) | The audience value to use in run identity tokens | `string` | `"vault.workload.identity"` | no |
| <a name="input_tfc_workspace_name"></a> [tfc\_workspace\_name](#input\_tfc\_workspace\_name) | The name of the workspace that you'd like to create and connect to Azure | `string` | `"vault-backed-azure-auth"` | no |
| <a name="input_vault_namespace"></a> [vault\_namespace](#input\_vault\_namespace) | The namespace of the Vault instance you'd like to create the Azure and jwt auth backends in | `string` | `"admin"` | no |
| <a name="input_vault_url"></a> [vault\_url](#input\_vault\_url) | The URL of the Vault instance you'd like to use with Terraform Cloud | `string` | n/a | yes |

## Outputs

No outputs.
<!-- END_TF_DOCS -->
