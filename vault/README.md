# Bootstrapping trust between a TFC workspace and Vault

This directory contains example code for setting up a Terraform Cloud workspace whose runs will be automatically authenticated to Vault using Workload Identity.

The basic building blocks in `vault.tf` will enable the `jwt` auth backend in Vault and create a role that is bound to a particular Terraform Cloud workspace.

The building blocks in `tfc-workspace.tf` will create that Terraform Cloud workspace and set all the configuration variables needed in order to allow runs to authenticate to Vault.

## How to use

You'll need the Terraform CLI installed, and you'll need to set the following environment variables in your local shell:

1. `VAULT_TOKEN`: the Vault token that you'll use to bootstrap your trust configuration in Vault. It will need the ability to enable auth backends and create roles and policies.
1. `VAULT_NAMESPACE` (optional): only set this if you're not using the default Vault namespace.
1. `TFE_TOKEN`: a Terraform Cloud user token with permission to create workspaces within your organization.

Copy `terraform.tfvars.example` to `terraform.tfvars` and customize the required variables. You can also set values for any other variables you'd like to customize beyond the default.

Run `terraform plan` to verify your setup, and then run `terraform apply`.

Congratulations! You now have a Terraform Cloud workspace where runs will automatically authenticate to the given Vault instance, allowing you to use
the Vault Terraform provider to manage and retrieve secrets via Terraform.
<!-- BEGIN_TF_DOCS -->
## Requirements

No requirements.

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
| [tfe_variable.enable_vault_provider_auth](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.tfc_vault_addr](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.tfc_vault_role](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_workspace.my_workspace](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/workspace) | resource |
| [vault_jwt_auth_backend.tfc_jwt](https://registry.terraform.io/providers/hashicorp/vault/latest/docs/resources/jwt_auth_backend) | resource |
| [vault_jwt_auth_backend_role.tfc_role](https://registry.terraform.io/providers/hashicorp/vault/latest/docs/resources/jwt_auth_backend_role) | resource |
| [vault_policy.tfc_policy](https://registry.terraform.io/providers/hashicorp/vault/latest/docs/resources/policy) | resource |
| [tfe_project.tfc_project](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/data-sources/project) | data source |

## Inputs

| Name | Description | Type | Default | Required |
| ---- | ----------- | ---- | ------- | :------: |
| <a name="input_jwt_backend_path"></a> [jwt\_backend\_path](#input\_jwt\_backend\_path) | The path at which you'd like to mount the jwt auth backend in Vault | `string` | `"jwt"` | no |
| <a name="input_tfc_hostname"></a> [tfc\_hostname](#input\_tfc\_hostname) | The hostname of the TFC or TFE instance you'd like to use with Vault | `string` | `"app.terraform.io"` | no |
| <a name="input_tfc_organization_name"></a> [tfc\_organization\_name](#input\_tfc\_organization\_name) | The name of your Terraform Cloud organization | `string` | n/a | yes |
| <a name="input_tfc_project_name"></a> [tfc\_project\_name](#input\_tfc\_project\_name) | The project under which a workspace will be created | `string` | `"Default Project"` | no |
| <a name="input_tfc_vault_audience"></a> [tfc\_vault\_audience](#input\_tfc\_vault\_audience) | The audience value to use in run identity tokens | `string` | `"vault.workload.identity"` | no |
| <a name="input_tfc_workspace_name"></a> [tfc\_workspace\_name](#input\_tfc\_workspace\_name) | The name of the workspace that you'd like to create and connect to Vault | `string` | `"my-vault-workspace"` | no |
| <a name="input_vault_url"></a> [vault\_url](#input\_vault\_url) | The URL of the Vault instance you'd like to use with Terraform Cloud | `string` | n/a | yes |

## Outputs

No outputs.
<!-- END_TF_DOCS -->
