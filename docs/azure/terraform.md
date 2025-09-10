--- 
title: Terraform
layout: default
parent: Azure
---

# Terraform

Terraform is an open-source Infrastructure as Code (IaC) tool that allows you to define and provision Azure infrastructure using declarative configuration files written in HashiCorp Configuration Language (HCL) or other sources.

![Terraform flow](/assets/images/terraform-platform.png)


## Benefits for Azure:
- **Declarative Syntax**: Define desired state rather than imperative steps
- **Plan and Apply**: Preview changes before applying them
- **State Management**: Track and manage infrastructure state
- **Version Control**: Store infrastructure code in source control
- **Reusability**: Create reusable modules and templates

## Cloud Development Kit Terraform (CDKTF)

CDKTF extends Terraform by allowing you to define infrastructure using familiar programming languages instead of HCL.

### Supported Languages:
- TypeScript (preferred)
- Python
- Java
- C#
- Go

### Key Advantages:
- **Programming Language Familiarity**: Use existing development skills
- **IDE Support**: Full IDE support with autocompletion and error detection
- **Testing**: Unit test infrastructure code using standard testing frameworks
- **Modularity**: Create reusable infrastructure components using OOP principles
- **Existing Toolchain**: Leverage existing CI/CD and development workflows

## Basic Workflow:
1. Create an infrastructure project code using the template project.
2. CDKTF synthesizes code into Terraform JSON configuration when using `npx cdktf diff` or `npx cdktf apply`. 

### Code example

The following code generates a terraform config to create a new resource group.

```typescript
(imports)

const tenantId = "TENANT_ID";
const subscriptionId = "SUBSCRIPTION_ID";

const projectName = "PROJECT_NAME"
const location = "LOCATION"
const environment = 'ENVIRONMENT'
const resourceGroupName =  'RESOURCE_GROUP'
const azureClientSecretName = 'CLIENT_SECRET'

function makeAzurermBackend(scope: Construct, id: string): AzurermBackend {
  const backend = new AzurermBackend(scope, {
    tenantId,
    subscriptionId,
    storageAccountName: "ACCOUNT_NAME",
    containerName: "CONTAINER_NAME",
    key: `${projectName}-${id}`,
  });

  return backend
}

function makeAzurermProvider(scope: Construct): AzurermProvider {
  const provider = new AzurermProvider(scope, "azurermProvider", {
    subscriptionId,
    features: [{}],
  });

  return provider
}

function makeDataAzurermResourceGroup(scope: Construct) : DataAzurermResourceGroup{
      const resourceGroup = new DataAzurermResourceGroup(scope, "resourceGroup", {
      name: resourceGroupName,
    });
    return resourceGroup
}

class BootstrapStack extends TerraformStack {
  constructor(scope: Construct, id: string) {
    super(scope, id);

    makeAzurermBackend(this, id);
    makeAzurermProvider(this);

    const resourceGroup = new ResourceGroup(this, "resourceGroup", {
      name: resourceGroupName,
      location: location,
    });
  }
}

const app = new App();
new BootstrapStack(app, "bootstrap")
app.synth();


```