---
title: CDKTF CLI
layout: default
parent: Tools
---

# Cloud Development Kit for Terraform (CDKTF)
Cloud Development Kit for Terraform (CDKTF) allows you to use familiar programming languages to define and provision infrastructure. Supported languages are TypeScript, Python, Java, C#, and Go. CDKTF compiles your code into Terraform JSON, then uses the Terraform CLI to plan and apply changes. This lets you leverage Terraform’s ecosystem while writing infrastructure as code in a general-purpose language.

The official documentation can be found [here](https://developer.hashicorp.com/terraform/cdktf).

## Prerequisites
- Node.js 18+ and npm or pnpm installed.
- Terraform 1.5+ available on your PATH.
- An active Azure CLI session in the current shell.
    - Read the Sign in chapter in the [Azure CLI Page](./azure-cli.md) first.


## Installation

### Global
Install the CLI globally:
- **npm:** `npm i -g cdktf-cli`
- **pnpm:** `pnpm add -g cdktf-cli`

Use it:
- `cdktf --help`,
- `cdktf diff`, 
- `cdktf deploy`, 
    - `cdktf apply` is a working alias in most cases
- `cdktf destroy`

### As devDependency
Install the CLI locally so your project pins a known version.

```json
{
  (...),
  "devDependencies": {
    "@types/node": "^24.3.0",
    "cdktf-cli": "^0.21.0",
    "tsx": "^4.20.5",
    "typescript": "^5.9.2"
  }
}
```

Install dependencies with one of:
- **npm:** `npm install`
- **pnpm:** `pnpm install`

Use it by adding scripts:

```json
{
  (...),
  "scripts": {
    "cdktf": "cdktf",
    "get": "cdktf get",s
    "synth": "cdktf synth",
    "diff": "cdktf diff",
    "deploy": "cdktf deploy",
    "destroy": "cdktf destroy",
    "output": "cdktf output"
  },
}
```

- `npx cdktf <command>`
- `pnpm exec cdktf <command>`

- or via scripts: `pnpm run deploy`

## Project Setup

### Using the github-template 

COMPANY provides a template for a terraform project.

TODO: WIP

### Initialize a new project (TypeScript) 

- Run `cdktf init --template=typescript --local` to create a new CDKTF project.

What this does:
- Creates a basic TypeScript project with CDKTF scaffolding.
- Generates a tsconfig.json, main.ts entrypoint, and cdktf.json project config.

### Configure providers and modules
Edit `cdktf.json` to declare providers and modules, then generate bindings.

Example cdktf.json:
```json
{
  "language": "typescript",
  "app": "tsx main.ts",
  "terraformProviders": [
    "hashicorp/azurerm@~>3.100",
    "hashicorp/random@~>3.6"
  ],
  "terraformModules": [],
  "codeMakerOutput": "imports",
  "projectId": "YOUR-UNIQUE-ID"
}
```

- Then run `pnpm run get` or `npx cdktf get`
This generates provider bindings in the `imports` directory (or `.gen` for older templates), which you import in your code.

### Minimal Azure example (TypeScript)
main.ts:
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

Commands:
- `pnpm run synth` to generate cdk.tf.json
- `pnpm run diff` to see changes
- `pnpm run deploy` to apply
- `pnpm run output` to print Terraform outputs

## Common Commands

### Help and info
- `cdktf --help` shows all commands.
- `cdktf version` prints CLI version.

### Initialization
- `cdktf init --template=typescript --local` scaffolds a project.
- `cdktf get` generates provider/module bindings from cdktf.json.

### Synthesis and planning
- `cdktf synth` compiles code into Terraform JSON (cdk.tf.json).
- `cdktf diff` runs a Terraform plan to show changes.

### Apply and destroy
- `cdktf deploy` applies changes interactively.
- `cdktf deploy --auto-approve` applies without prompts (CI-friendly).
- `cdktf destroy` destroys resources interactively.
- `cdktf destroy --auto-approve` destroys without prompts.

### Stacks and selection
- `cdktf list` lists defined stacks.
- `cdktf deploy <stack>` targets a specific stack by ID used in code (e.g., `azure` above).
- `cdktf destroy <stack>` targets a specific stack.

### Converting HCL to CDKTF
- Run `cdktf convert --language=typescript --provider=azurerm --path ./hcl` generates starter code from HCL. Review and refactor the result.

## Working with multiple environments

### Stacks

**Deploy selectively:**
- `cdktf deploy dev`
- `cdktf deploy prod`

### Context and configuration
Pass configuration at deploy time:
- `cdktf deploy dev -c location=westeurope -c namePrefix=demo`

Read context in code:
```ts
const location = this.node.tryGetContext("location") ?? "westeurope";
const namePrefix = this.node.tryGetContext("namePrefix") ?? "app";
```

## CI/CD tips
- Use non-interactive flags:
  - `cdktf diff` to validate the plan.
  - `cdktf deploy --auto-approve` to apply.
  - `cdktf destroy --auto-approve` for teardown jobs.
- Cache the `imports` and `.terraform` directories to speed up builds.
- Always run `cdktf get` after checking out the repo and before `synth/diff/deploy`.
