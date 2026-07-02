# Bootstrapping trust between a TFC workspace and AWS using Vault-Backed AWS Secrets Engine

This directory contains example code for setting up a Terraform Cloud workspace whose runs will be automatically authenticated to AWS using Workload Identity and Vault's AWS Secrets Engine.

It contains the necessary configuration for adding both the AWS Secrets Engine and JWT/OIDC authentication to your Vault instance, as well as an IAM user and roles for your Vault instance to use within your AWS account.

## How to use

You'll need the Terraform CLI installed, and you'll need to set the following environment variables in your local shell:

1. `VAULT_TOKEN`: the Vault token that you'll use to bootstrap your trust configuration in Vault. It will need the ability to enable auth backends and create roles and policies.
1. `TFE_TOKEN`: a Terraform Cloud user token with permission to create workspaces within your organization.

You'll also need to authenticate the AWS provider as you would normally using one of the methods mentioned in the AWS provider documentation [here](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#authentication-and-configuration).

Copy `terraform.tfvars.example` to `terraform.tfvars` and customize the required variables. You can also set values for any other variables you'd like to customize beyond the default.

Run `terraform plan` to verify your setup, and then run `terraform apply`.

Congratulations! You now have a Terraform Cloud workspace where runs will automatically authenticate to AWS when using the AWS Terraform provider.
<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
| ---- | ------- |
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.15.7 |

## Providers

| Name | Version |
| ---- | ------- |
| <a name="provider_aws"></a> [aws](#provider\_aws) | n/a |
| <a name="provider_tfe"></a> [tfe](#provider\_tfe) | n/a |
| <a name="provider_vault"></a> [vault](#provider\_vault) | n/a |

## Modules

No modules.

## Resources

| Name | Type |
| ---- | ---- |
| [aws_iam_access_key.secrets_engine_credentials](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_access_key) | resource |
| [aws_iam_policy.tfc_policy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_policy) | resource |
| [aws_iam_role.tfc_role](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role) | resource |
| [aws_iam_role_policy_attachment.tfc_policy_attachment](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy_attachment) | resource |
| [aws_iam_user.secrets_engine](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_user) | resource |
| [aws_iam_user_policy.vault_secrets_engine_generate_credentials](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_user_policy) | resource |
| [tfe_variable.enable_aws_provider_auth](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.enable_vault_provider_auth](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.tfc_aws_auth_type](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.tfc_aws_mount_path](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.tfc_aws_run_role_arn](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.tfc_aws_run_vault_role](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.tfc_vault_addr](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.tfc_vault_namespace](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.tfc_vault_role](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_workspace.my_workspace](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/workspace) | resource |
| [vault_aws_secret_backend.aws_secret_backend](https://registry.terraform.io/providers/hashicorp/vault/latest/docs/resources/aws_secret_backend) | resource |
| [vault_aws_secret_backend_role.aws_secret_backend_role](https://registry.terraform.io/providers/hashicorp/vault/latest/docs/resources/aws_secret_backend_role) | resource |
| [vault_jwt_auth_backend.tfc_jwt](https://registry.terraform.io/providers/hashicorp/vault/latest/docs/resources/jwt_auth_backend) | resource |
| [vault_jwt_auth_backend_role.tfc_role](https://registry.terraform.io/providers/hashicorp/vault/latest/docs/resources/jwt_auth_backend_role) | resource |
| [vault_policy.tfc_policy](https://registry.terraform.io/providers/hashicorp/vault/latest/docs/resources/policy) | resource |
| [tfe_project.tfc_project](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/data-sources/project) | data source |

## Inputs

| Name | Description | Type | Default | Required |
| ---- | ----------- | ---- | ------- | :------: |
| <a name="input_aws_secret_backend_role_name"></a> [aws\_secret\_backend\_role\_name](#input\_aws\_secret\_backend\_role\_name) | Name of AWS secret backend role you created for runs to use | `string` | n/a | yes |
| <a name="input_jwt_backend_path"></a> [jwt\_backend\_path](#input\_jwt\_backend\_path) | The path at which you'd like to mount the jwt auth backend in Vault | `string` | `"jwt"` | no |
| <a name="input_tfc_hostname"></a> [tfc\_hostname](#input\_tfc\_hostname) | The hostname of the TFC or TFE instance you'd like to use with Vault | `string` | `"app.terraform.io"` | no |
| <a name="input_tfc_organization_name"></a> [tfc\_organization\_name](#input\_tfc\_organization\_name) | The name of your Terraform Cloud organization | `string` | n/a | yes |
| <a name="input_tfc_project_name"></a> [tfc\_project\_name](#input\_tfc\_project\_name) | The project under which a workspace will be created | `string` | `"Default Project"` | no |
| <a name="input_tfc_vault_audience"></a> [tfc\_vault\_audience](#input\_tfc\_vault\_audience) | The audience value to use in run identity tokens | `string` | `"vault.workload.identity"` | no |
| <a name="input_tfc_workspace_name"></a> [tfc\_workspace\_name](#input\_tfc\_workspace\_name) | The name of the workspace that you'd like to create and connect to AWS | `string` | `"vault-backed-aws-auth"` | no |
| <a name="input_vault_namespace"></a> [vault\_namespace](#input\_vault\_namespace) | The namespace of the Vault instance you'd like to create the AWS and jwt auth backends in | `string` | `"admin"` | no |
| <a name="input_vault_url"></a> [vault\_url](#input\_vault\_url) | The URL of the Vault instance you'd like to use with Terraform Cloud | `string` | n/a | yes |

## Outputs

No outputs.
<!-- END_TF_DOCS -->
