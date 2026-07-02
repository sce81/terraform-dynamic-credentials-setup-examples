# Bootstrapping trust between a TFC workspace and GCP

This directory contains example code for setting up a Terraform Cloud workspace whose runs will be automatically authenticated to GCP using Workload Identity.

The basic building blocks in `gcp.tf` will configure a workload identity pool and provider and create a service account that is bound to a particular Terraform Cloud workspace.

The building blocks in `tfc-workspace.tf` will create that Terraform Cloud workspace and set all the configuration variables needed in order to allow runs to authenticate to GCP.

## How to use

You'll need the Terraform CLI installed, and you'll need to set the following environment variables in your local shell:

1. `TFE_TOKEN`: a Terraform Cloud user token with permission to create workspaces within your organization.

You'll also need to authenticate the GCP provider as you would normally using one of the methods mentioned in the GCP provider documentation [here](https://registry.terraform.io/providers/hashicorp/google/latest/docs/guides/getting_started#adding-credentials).

Copy `terraform.tfvars.example` to `terraform.tfvars` and customize the required variables. You can also set values for any other variables you'd like to customize beyond the default.

Run `terraform plan` to verify your setup, and then run `terraform apply`.

Congratulations! You now have a Terraform Cloud workspace where runs will automatically authenticate to GCP instance when using the GCP provider.
<!-- BEGIN_TF_DOCS -->
## Requirements

No requirements.

## Providers

| Name | Version |
| ---- | ------- |
| <a name="provider_google"></a> [google](#provider\_google) | n/a |
| <a name="provider_google-beta"></a> [google-beta](#provider\_google-beta) | n/a |
| <a name="provider_tfe"></a> [tfe](#provider\_tfe) | n/a |

## Modules

No modules.

## Resources

| Name | Type |
| ---- | ---- |
| [google-beta_google_iam_workload_identity_pool.tfc_pool](https://registry.terraform.io/providers/hashicorp/google-beta/latest/docs/resources/google_iam_workload_identity_pool) | resource |
| [google-beta_google_iam_workload_identity_pool_provider.tfc_provider](https://registry.terraform.io/providers/hashicorp/google-beta/latest/docs/resources/google_iam_workload_identity_pool_provider) | resource |
| [google_project_iam_member.tfc_project_member](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_iam_member) | resource |
| [google_project_service.services](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_service) | resource |
| [google_service_account.tfc_service_account](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/service_account) | resource |
| [google_service_account_iam_member.tfc_service_account_member](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/service_account_iam_member) | resource |
| [tfe_variable.enable_gcp_provider_auth](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.tfc_gcp_service_account_email](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable.tfc_gcp_workload_provider_name](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_workspace.my_workspace](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/workspace) | resource |
| [google_project.project](https://registry.terraform.io/providers/hashicorp/google/latest/docs/data-sources/project) | data source |
| [tfe_project.tfc_project](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/data-sources/project) | data source |

## Inputs

| Name | Description | Type | Default | Required |
| ---- | ----------- | ---- | ------- | :------: |
| <a name="input_gcp_project_id"></a> [gcp\_project\_id](#input\_gcp\_project\_id) | The ID for your GCP project | `string` | n/a | yes |
| <a name="input_gcp_service_list"></a> [gcp\_service\_list](#input\_gcp\_service\_list) | APIs required for the project | `list(string)` | <pre>[<br/>  "iam.googleapis.com",<br/>  "cloudresourcemanager.googleapis.com",<br/>  "sts.googleapis.com",<br/>  "iamcredentials.googleapis.com"<br/>]</pre> | no |
| <a name="input_tfc_gcp_audience"></a> [tfc\_gcp\_audience](#input\_tfc\_gcp\_audience) | The audience value to use in run identity tokens if the default audience value is not desired. | `string` | `""` | no |
| <a name="input_tfc_hostname"></a> [tfc\_hostname](#input\_tfc\_hostname) | The hostname of the TFC or TFE instance you'd like to use with GCP | `string` | `"app.terraform.io"` | no |
| <a name="input_tfc_organization_name"></a> [tfc\_organization\_name](#input\_tfc\_organization\_name) | The name of your Terraform Cloud organization | `string` | n/a | yes |
| <a name="input_tfc_project_name"></a> [tfc\_project\_name](#input\_tfc\_project\_name) | The project under which a workspace will be created | `string` | `"Default Project"` | no |
| <a name="input_tfc_workspace_name"></a> [tfc\_workspace\_name](#input\_tfc\_workspace\_name) | The name of the workspace that you'd like to create and connect to GCP | `string` | `"my-gcp-workspace"` | no |

## Outputs

No outputs.
<!-- END_TF_DOCS -->
