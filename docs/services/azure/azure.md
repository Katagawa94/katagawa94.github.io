--- 
title: Azure
layout: default
parent: Services
---

# Azure

Microsoft Azure is a comprehensive cloud computing platform that provides a wide range of services including computing, analytics, storage, and networking. Azure enables organizations to build, deploy, and manage applications through Microsoft's global network of data centers.

## Key Benefits:
- **Scalability**: Resources can be scaled up or down based on demand
- **Global Reach**: Data centers in multiple regions worldwide
- **Security**: Enterprise-grade security and compliance certifications

## Tenants

An Azure tenant represents an organization in Azure Active Directory (now Microsoft Entra ID). It serves as a dedicated instance of Microsoft Entra ID that an organization receives when signing up for Microsoft cloud services.

### Key Characteristics:
- **Identity Boundary**: Defines the security and administrative boundary for an organization
- **Unique Identifier**: Each tenant has a unique tenant ID (GUID)
- **Domain Association**: Can be associated with custom domains or use the default onmicrosoft.com domain
- **User Management**: Contains all users, groups, and applications for the organization
- **Single Sign-On**: Enables SSO across all Microsoft cloud services

### Use Cases:
- Managing organizational identities and access
- Controlling access to Azure resources
- Implementing security policies across the organization

## Subscriptions

Azure subscriptions are logical containers that link Azure resources to billing accounts and provide access management boundaries. Every Azure resource must belong to a subscription.

### Key Features:
- **Billing Unit**: All resources in a subscription are billed together
- **Access Control**: RBAC (Role-Based Access Control) can be applied at subscription level
- **Resource Limits**: Each subscription has default limits and quotas for resources
- **Policy Application**: Azure policies can be enforced at subscription level

## Resources

Azure resources are the individual services and components that you create and manage within Azure subscriptions. Resources are organized into resource groups for management purposes.

### Common Resource Types:

- Web Applications
- Key Vaults
- App Registrations

### Resource Management:
- **Resource Groups**: Logical containers for related resources
- **RBAC**: Control who can access and modify resources

