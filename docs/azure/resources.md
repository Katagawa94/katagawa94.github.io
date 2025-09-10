--- 
title: Resources
layout: default
parent: Azure
---

# Resources

## App Registration

App Registration is the process of registering applications with Microsoft Entra ID to enable authentication and authorization services.

### Purpose:
- **Identity Integration**: Integrate applications with Microsoft identity platform
- **Authentication**: Enable secure user sign-in and sign-up
- **Authorization**: Control access to resources and APIs
- **Token Management**: Obtain and manage access tokens for API calls

### Configuration Elements:
- **Application ID**: Unique identifier for the registered application
- **Client Secrets/Certificates**: Credentials for application authentication
- **Redirect URIs**: Valid locations for authentication responses

## Key Vaults

Azure Key Vault is a cloud service that provides secure storage and management of secrets, keys, and certificates.


### Secrets Management
- Store and manage passwords, connection strings, and API keys
- Version control for secrets with automatic rotation support
- Access logging and monitoring for compliance

### Key Management
- Create and manage cryptographic keys
- Key rotation and lifecycle management

### Certificate Management
- Store and manage SSL/TLS certificates
- Automatic certificate renewal from supported Certificate Authorities
- Integration with Azure services for automatic certificate deployment

### Integration Scenarios:
- Application configuration and secrets management
- Database connection string storage
- API key and token management
- SSL certificate automation for web applications
- Encryption key management for data protection

## SPA (Single Page Application)

Single Page Applications are web applications that load a single HTML page and dynamically update content as users interact with the application, providing a desktop-like experience.

### Configuration Requirements:
- App Registration configured for SPA platform
- Redirect URIs configured for authentication responses
- CORS settings for cross-origin requests
- Appropriate API permissions and scopes

### Implementation Steps:
1. Create App Registration in Microsoft Entra ID
2. Configure SPA platform settings and redirect URIs
4. Implement authentication flow in application
5. Handle tokens for API calls and user context
