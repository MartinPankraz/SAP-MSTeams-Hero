# Lecture 103: Deal with SAP Principal Propagation for API consumption

[< Previous Challenge](./102-embed-app.md) - **[🏠Home](../README.md)** - [Next Challenge >](./104-chatbot-deploy.md)

## 🔭 Introduction

Great, user-based SSO for your web app is working now. But a complete setup also requires service to service authentication to request/change data on SAP through APIs usually. Executing those requests within the context of the AAD authenticated user brings compliance challenges, because the logged in client needs to leave an auditable trace on the SAP backend that can be linked to an SAP named-user. The solution to this is often referred to as "SAP Principal Propagation". Basically it describes the mechanism of mapping a user from its Azure AD identity to your SAP backend user id.

In addition to that there is a desire to deal with the complexity of this authentication mechanism in one place and solve for all clients centrally. We will be employing Azure APIM for that purpose.

## 📖 Description

Familiarize yourself with these two blog posts and GitHub repos to learn more about the context.

- [Part III: Teams SSO, Process Integration & Core Data Services](https://blogs.sap.com/2021/02/24/principal-propagation-in-a-multi-cloud-solution-between-microsoft-azure-and-sap-cloud-platform-scp-part-iii-teams-sso-process-integration-core-data-services/)
- [.NET speaks OData too – how to implement Azure App Service with SAP Gateway](https://blogs.sap.com/2021/08/12/.net-speaks-odata-too-how-to-implement-azure-app-service-with-sap-odata-gateway/)
- [AzureSAPODataReader](https://github.com/MartinPankraz/AzureSAPODataReader)

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

### Azure APIM

- Ensure access to your APIM exposed API endpoints. Are you running externally only? Hybrid? Internet facing?
- [Convert](https://aka.ms/ODataOpenAPI) your OData $metadata to OpenAPIv3 and import into Azure APIM
- Add [SAP Principal Propagation policy](https://github.com/Azure/api-management-policy-snippets/blob/master/examples/Request%20OAuth2%20access%20token%20from%20SAP%20using%20AAD%20JWT%20token.xml) to your OData api
- (Optionally) Use the Postman collection to test your setup

## 🏆 Success Criteria

- Be able to call your SAP OData api using an Azure AD authenticated client
- Prove SAP user mapping by maintaining emails (or object ids) on the SAP backend transaction SU01
- (Optionally) Run SAP transaction SEC_DIAG_TOOL to trace OAuth client requests and identify user mapping step on the trace
