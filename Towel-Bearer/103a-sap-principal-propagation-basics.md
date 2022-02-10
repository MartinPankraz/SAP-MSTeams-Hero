# Lecture 103a: Configure SAP Principal Propagation with AAD and SAP OAuth server

[< Previous Challenge](./102-embed-app.md) - **[🏠Home](../README.md)** - [Next Challenge >](./103b-sap-principal-propagation-apim.md)

## 🔭 Introduction

Great, user-based SSO for your web app is working now. But a complete setup also requires service to service authentication to request/change data on SAP through APIs usually. Executing those requests within the context of the AAD authenticated user brings compliance challenges, because the logged in client needs to leave an auditable trace on the SAP backend that can be linked to an SAP named-user. The solution to this is often referred to as "SAP Principal Propagation". Basically it describes the mechanism of mapping a user from its Azure AD identity to your SAP backend user id.

## 📖 Description

Familiarize yourself with this blog post "[Part III: Teams SSO, Process Integration & Core Data Services](https://blogs.sap.com/2021/02/24/principal-propagation-in-a-multi-cloud-solution-between-microsoft-azure-and-sap-cloud-platform-scp-part-iii-teams-sso-process-integration-core-data-services/)" to learn more about the context.

### SAP SAML config

- Configure Local Provider using SAP transaction SAML2. Pay attention to the Provider Name setting. Azure AD requires the name format to be URI compliant. A simple option would be to prefix it with "spn:" (service provider name). See the [Azure Docs](https://docs.microsoft.com/azure/active-directory/develop/single-sign-on-saml-protocol#audience) for more details.
- Export your Metadata for import into AAD in the next step.

### Azure AD SAML config

- Create an Enterprise Application on Azure AD for SAP NetWeaver.
- Upload the metadata from SAP to the SAML config of the enterprise app. Verify Entity ID with provider name from SAML2 transaction and metadata. Be aware that the "**spn:**" prefix will not show on Azure AD.
- Download Federation Metadata XML for import in SAP OAuth server.

### SAP OAuth2 config

- Configure OAuth2 Identity Provider (SAML2 -> Trusted Providers -> OAuth 2.0. Identity Providers -> Add) using downloaded metadata from Azure AD. Maintain E-mail or Unspecified as supported NameID format depending on your desired identification source on AAD. We will rely on email (upn) in our lecture.
- Configure OAuth2 client using transaction SOAUTH2.
- Keep defaults and maintain **Resource Owner Authentication -> Trusted OAuth 2.0 IdP** with your new trusted IdP.
- Maintain required scope for your target OData service (e.g. ZEPM_REF_APPS_PROD_MAN_SRV_0001 for basic EPM demo model).

### Testing

Build upone the [Postman collection](https://github.com/MartinPankraz/AzureSAPODataReader/blob/master/Templates/AAD_APIM_SAP_Principal_Propagation.postman_collection.json) to test your setup (Skip APIM related requests for now).

## 🏆 Success Criteria

- Be able to call your SAP OData api using an Azure AD authenticated client
- Prove SAP user mapping by maintaining emails (or object ids) on the SAP backend transaction SU01
- (Optionally) Run SAP transaction SEC_DIAG_TOOL to trace OAuth client requests and identify user mapping step on the trace
