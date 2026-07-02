# Bootstrapping trust between a TFC workspace and Azure

This directory contains example code for setting up a Terraform Cloud workspace whose runs will be automatically authenticated to Azure using Workload Identity.

The basic building blocks in `azure.tf` will configure a service principal in Azure and create federated identity credentials that are bound to a particular Terraform Cloud workspace.

The building blocks in `tfc-workspace.tf` will create that Terraform Cloud workspace and set all the configuration variables needed in order to allow runs to authenticate to Azure.

## How to use

You'll need the Terraform CLI installed, and you'll need to set the following environment variables in your local shell:

1. `TFE_TOKEN`: a Terraform Cloud user token with permission to create workspaces within your organization.

You'll also need to authenticate the Azure provider as you would normally using one of the methods mentioned in the Azure provider documentation [here](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs#authenticating-to-azure).

Copy `terraform.tfvars.example` to `terraform.tfvars` and customize the required variables. You can also set values for any other variables you'd like to customize beyond the default.

Run `terraform plan` to verify your setup, and then run `terraform apply`.

Congratulations! You now have a Terraform Cloud workspace where runs will automatically authenticate to Azure when using the AzureRM or AzureAD Terraform providers.
<!-- BEGIN_TF_DOCS -->
## Requirements

No requirements.

## Providers

| Name | Version |
| ---- | ------- |
| <a name="provider_azuread"></a> [azuread](#provider\_azuread) | n/a |
| <a name="provider_azurerm"></a> [azurerm](#provider\_azurerm) | n/a |
| <a name="provider_tfe"></a> [tfe](#provider\_tfe) | n/a |

## Modules

No modules.

## Resources

| Name | Type |
| ---- | ---- |
| [azuread_application.tfc_application](https://registry.terraform.io/providers/hashicorp/azuread/latest/docs/resources/application) | resource |
| [azuread_application_federated_identity_credential.tfc_federated_credential_apply](https://registry.terraform.io/providers/hashicorp/azuread/latest/docs/resources/application_federated_identity_credential) | resource |
| [azuread_application_federated_identity_credential.tfc_federated_credential_plan](https://registry.terraform.io/providers/hashicorp/azuread/latest/docs/resources/application_federated_identity_credential) | resource |
| [azuread_service_principal.tfc_service_principal](https://registry.terraform.io/providers/hashicorp/azuread/latest/docs/resources/service_principal) | resource |
| [azurerm_role_assignment.tfc_role_assignment](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/role_assignment) | resource |
| [tfe_variable.enable_azure_provider_auth](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.tfc_azure_client_id](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_workspace.my_workspace](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/workspace) | resource |
| [azurerm_subscription.current](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/data-sources/subscription) | data source |
| [tfe_project.tfc_project](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/data-sources/project) | data source |

## Inputs

| Name | Description | Type | Default | Required |
| ---- | ----------- | ---- | ------- | :------: |
| <a name="input_tfc_azure_audience"></a> [tfc\_azure\_audience](#input\_tfc\_azure\_audience) | The audience value to use in run identity tokens | `string` | `"api://AzureADTokenExchange"` | no |
| <a name="input_tfc_hostname"></a> [tfc\_hostname](#input\_tfc\_hostname) | The hostname of the TFC or TFE instance you'd like to use with Azure | `string` | `"app.terraform.io"` | no |
| <a name="input_tfc_organization_name"></a> [tfc\_organization\_name](#input\_tfc\_organization\_name) | The name of your Terraform Cloud organization | `string` | n/a | yes |
| <a name="input_tfc_project_name"></a> [tfc\_project\_name](#input\_tfc\_project\_name) | The project under which a workspace will be created | `string` | `"Default Project"` | no |
| <a name="input_tfc_workspace_name"></a> [tfc\_workspace\_name](#input\_tfc\_workspace\_name) | The name of the workspace that you'd like to create and connect to Azure | `string` | `"my-azure-workspace"` | no |

## Outputs

No outputs.
<!-- END_TF_DOCS -->
