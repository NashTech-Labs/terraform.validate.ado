# ADO.Pipelines.Templates.frameWork.terraform.actions

## Pipeline Requirements

The Terraform Module Package pipeline requires the following parameters to be defined:
Paramaters:


| Name  | Displayname | type | Default | Values | Opional/Required | Comments |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| ProjectName | Name of the Project | string | 'Testing' | | Required | This enables to use different display name for the pipeline |
| initbackend | Remote Backend to be used for Terraform | string | 'tfworkspace' |'tfworkspace' / 'azure' | Required | This defines which templates to be taken in consideration |
| workspace  | Terraform Cloud workspace | String |  | | Required | This defines the workspace to be used |
| workingDirectory | Location of Terraform packages | string |  | | Required | This defines the Location of Terraform packages to be used |
| enablePlan  | Boolean value to define to use Terraform plan or not | boolean | true | true / false | Required | This is a Boolean value to define whether to use Terraform plan or not |
| terraformaccesstoken | Acces Token to define Terraform Cloud | string | | | Optional | This defines the Acces Token to be used for Terraform Cloud |
| tfc_organization_name | Name of Terraform Organization | string | | | Optional | This defines the Terraform Organization to be used |
| tfplancommandOptions | Extra command to be used with Terraform plan | string | '-out=tfplan -lock=false' || Optional | This defines the extra command to be used with Terraform plan command |
| applycommandOptions | Extra command to be used with Terraform apply | string | '-auto-approve'|| Required | This defines the extra command to be used with Terraform apply command |
| destroycommandOptions | Extra command to be used with Terraform destroy | string | '-auto-approve'|| Required | This defines the extra command to be used with Terraform destroy command |
| provider | Name of the Provider | string | 'azurerm' |  | Optional | This defines the provider to be used |
| backendServiceArm  | Azure Service connection to authorize backend access | string |  | | Optional | This defines Azure Service connection to authorize backend access |
| backendAzureRmResourceGroupName | Azure backend resource-group | string |  | | Optional | This defines the Azure backend resource-group |
| backendAzureRmStorageAccountName | Azure location shortname of the backend storage account | string |  |  | Optional | This defines the Azure location shortname of the backend storage account |
| backendAzureRmContainerName | Azure blob container to store the state file | string |  |  | Optional | This defines the Azure blob container to store the state file |
| backendAzureRmKey | Azure blob file name | string |  |  | Optional | This defines the Azure blob file name |
| environmentServiceNameAzureRM  | Azure Service Connection | String |  |  | Optional | This defines the Azure environment Service Connection |
| recursive | Name of module | Boolean | true | | Optional | It determines whether the formatting operation should be performed recursively on subdirectories |
| checkMode  | Terraform registry access token | Boolean | false | | Optional | It determines whether the formatting should be in check mode, without making any changes to the configuration files |
| ApplyResource | Boolean value to define to use Terraform apply or not | boolean | true | true / false | Required | This is a Boolean value to define whether to use Terraform apply or not |
| DestroyResource | Boolean value to define to use Terraform destroy or not | boolean | true | true / false | Required | This is a Boolean value to define whether to use Terraform destroy or not |

   
  These parameters provide configuration options for the Terraform Module Package pipeline, such as the Terraform Module Package version, build configuration name and enable/disable flags for different stages.

## Use Cases

### 1. Terraform Cloud as Terraform Backend

  ```yaml
  # azure-pipeline.yml
  resources:
    repositories:
      - repository: TerraformTemplate
        type: github
        name: duck-creek/ADO.Pipelines.Templates
        ref: <respective branch name>
        endpoint: 'Duck Creek'

  steps:
  - template: frameWork/terraform/actions/main.yml@TerraformTemplate
    parameters:
      terraformVersion: '1.4.1'
      workingDirectory: 'terraformmodule'
      initbackend: 'tfworkspace'
      ProjectName: 'Example'
      workspace: 'tfworkspace-name'
      tfc_organization_name: 'DCTEnterprise'
      terraformaccesstoken: $(accesstoken)
      enablePlan: false
      ApplyResource: true
      DestroyResource: true
      applycommandOptions: "-auto-approve"
  ```
In the above pipeline, the Terraform Actions templates are sourced from the duck-creek/ADO.Pipelines.Templates repository on GitHub. The parameters are provided to configure the Terraform pipeline according to the desired build configuration and stages. The value of enablePlan is *false* here but, when set to *true* you need to add *tfplancommandOptions* parameter value. This setup is used when the backend is configured for terraform cloud workspace.
Please make sure to adjust the repository name, branch name, and parameter values according to your project's requirements.

### 2. Azure Blob storage as Terraform Backend

  ```yaml
  # azure-pipeline.yml
  resources:
    repositories:
      - repository: TerraformTemplate
        type: github
        name: duck-creek/ADO.Pipelines.Templates
        ref: <respective branch name>
        endpoint: 'Duck Creek'

  steps:
  - template: frameWork/terraform/actions/main.yml@TerraformTemplate
    parameters:
      terraformVersion: '1.4.1'
      workingDirectory: 'terraformmodule'
      initbackend: 'azure'
      ProjectName: 'Example'
      workspace: 'tfworkspace-name'
      enablePlan: false
      ApplyResource: true
      DestroyResource: true
      applycommandOptions: "-auto-approve"
  ```
In the above pipeline, the Terraform Actions templates are sourced from the duck-creek/ADO.Pipelines.Templates repository on GitHub. The parameters are provided to configure the Terraform pipeline according to the desired build configuration and stages. The value of enablePlan is *false* here but, when set to *true* you need to add *tfplancommandOptions* parameter value. This setup is used when the backend is configured for terraform cloud workspace
Please make sure to adjust the repository name, branch name, and parameter values according to your project's requirements.

