---
title: Azure CLI
layout: default
parent: Tools
---

# Azure
The Azure CLI is a cross‑platform command‑line tool for creating and managing Azure resources interactively or through scripts across Windows, Linux, and macOS, with options to run in Docker or Azure Cloud Shell.  The official documentation covers installation, command references, authentication, and tips for effective usage.  Before any operation, establishing an authenticated session with Microsoft Entra ID via az login is required.

{: .note}
> The official documentation can be found [here](https://learn.microsoft.com/de-de/cli/azure/?view=azure-cli-latest).

## Installation

### Linux
The CLI can be installed with distribution package managers (apt, dnf, zypper, tdnf), via an install script, or used in Docker/Cloud Shell; after installation, check the version with az version.  For Ubuntu/Debian, follow the distribution‑specific steps to add the Microsoft package repository and then install the azure-cli package using apt as documented.  On SUSE/OpenSUSE, the package is available via zypper and can be installed directly when the repository is configured.

**Debian/ Ubuntu example:** `curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash`

### Windows
Install on Windows using WinGet, MSI, or a ZIP package; after installation, close and reopen the terminal, and confirm with az version. AzureCLI installs the latest CLI, and MSI installers support both initial install and upgrades without uninstalling. 

**WinGet example:** `winget install --exact --id Microsoft`. 

## Sign in first
Use az login to authenticate and start a CLI session; interactive login opens a browser and also provides a subscription selector to set a default subscription context.  Common methods include:
- **Interactive browser:** `az login` for a local, browser‑based sign‑in flow. (most common)
- **Device code:** `az login --use-device-code` for headless/SSH scenarios or when a browser cannot be used.
- **Specific tenant:** `az login --tenant <TENANT_ID_OR_DOMAIN>` to target a particular Microsoft Entra tenant.
- **Service principal:** `az login --service-principal` with a client secret or certificate for non‑interactive automation.
- **Managed identity:** `az login --identity` for Azure‑hosted workloads (for example, VM, App Service, Functions).

Verification and subscription context:
- Show current account and subscription: `az account show`.
- List subscriptions and pick one: `az account list` and `az account set --subscription "<SUBSCRIPTION_NAME_OR_ID>"`.

## Tool integrations
- **Terraform:** The AzureRM provider supports authenticating via the Azure CLI session; first run az login (or service principal/managed identity variants) and Terraform will reuse the active credentials.
- **Static Web Apps CLI:** Use swa login to authenticate and obtain a deployment token used by swa deploy for Azure Static Web Apps.

## Usage in COMPANY
WIP