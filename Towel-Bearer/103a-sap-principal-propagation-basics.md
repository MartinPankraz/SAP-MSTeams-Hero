# Lecture 103a: Configure SAP Principal Propagation with AAD and SAP OAuth server

[< Previous Challenge](./102-embed-app.md) - **[🏠Home](../README.md)** - [Next Challenge >](./103b-sap-principal-propagation-apim.md)

## 🔭 Introduction

Great, user-based SSO for your web app is working now. But a complete setup also requires service to service authentication to request/change data on SAP through APIs usually. Executing those requests within the context of the AAD authenticated user brings compliance challenges, because the logged in client needs to leave an auditable trace on the SAP backend that can be linked to an SAP named-user. The solution to this is often referred to as "SAP Principal Propagation". Basically it describes the mechanism of mapping a user from its Azure AD identity to your SAP backend user id.

> if you lack a system we can recommend the [SAP Cloud Appliance Library](https://cal.sap.com/). You can [signup](https://blogs.sap.com/2020/01/17/creating-a-p-user-in-sap-cloud-platform-to-practise-sap-hana./) for a P-User from the community in case you don't have access via your company.

[![third session link to YouTube](../img/103.png)](https://www.youtube.com/watch?v=JGvJJnMSEHM&list=PLvqyDwoCkBXZ85LoFrNWv9Mj88TiDAc4g&index=4)

## 📖 Description

Familiarize yourself with this blog post "[Part III: Teams SSO, Process Integration & Core Data Services](https://blogs.sap.com/2021/02/24/principal-propagation-in-a-multi-cloud-solution-between-microsoft-azure-and-sap-cloud-platform-scp-part-iii-teams-sso-process-integration-core-data-services/)" to learn more about the context.

😱 **Don't PANIC**, Martin's series can be intimidating in detail and length but you know where your towel is.

### SAP SAML config ⚙

- Configure Local Provider using SAP transaction SAML2. Pay attention to the Provider Name setting. Azure AD requires the name format to be URI compliant. A simple option would be to prefix it with "spn:" (service provider name). See the [Azure Docs](https://docs.microsoft.com/azure/active-directory/develop/single-sign-on-saml-protocol#audience) for more details.
- Export your Metadata for import into AAD in the next step.

> hint: In case transaction SAML2 doesn't show -> call transaction SICF_SESSIONS and verify saml2 and myssocntl nodes on SICF.

### Azure AD SAML config ⚙

- Create an Enterprise Application on Azure AD for SAP NetWeaver.
- Upload the metadata from SAP to the SAML config of the enterprise app. Verify Entity ID with provider name from SAML2 transaction and metadata. Be aware that the "**spn:**" prefix will not show on Azure AD.
- Alter values for Reply URL `https://domain:port/sap/bc/sec/oauth2/token`
- Alter values for SignOn URL to any URI compliant entry: `https://dummy.org`. Note the endpoint won't be called, because we leverage OAuth2SAMLBearer flow. We simply need a valid value, since this is a mandatory field.
- Download Federation Metadata XML for import in SAP OAuth server.

#### Register a client app on AAD to call SAP APIs

- Create an Application Registration and maintain Reply URL (Manage -> Authentication):`https://localhost:44326/signin-oidc` and check `Access Tokens` and `ID tokens` to enable our tests from Postman. This will be good enough for initial testing.
- Maintain a web profile (Manager -> Expose an API -> Create Application ID URI) and add a scope
- Maintain `openid` next to the default User.Read GraphAPI permission.
- Copy the app client id and add as "authorized client app" on the app registration for SAP NetWeaver that you created before (Manage -> Expose an API -> Add Client Application).

### SAP OAuth2 config ⚙

- Configure OAuth2 Identity Provider (SAML2 -> Trusted Providers -> OAuth 2.0. Identity Providers -> Add) using downloaded metadata from Azure AD. Maintain E-mail or Unspecified as supported NameID format depending on your desired identification source on AAD. We will rely on email (upn) in our lecture.
- Configure OAuth2 client using transaction SOAUTH2 (create a system user using transaction SU01). The transaction opens a webdynpro on following path `/sap/bc/webdynpro/sap/oauth2_config`.
- Keep defaults and maintain **Resource Owner Authentication -> Trusted OAuth 2.0 IdP** with your new trusted IdP.
- Maintain required scope for your target OData service (e.g. ZEPM_REF_APPS_PROD_MAN_SRV_0001 for basic EPM demo model). Activate OAuth support on /IWFND/MAINT_SERVICE if required.

### Testing 🧪

Build upon the [Postman collection](../Templates/Hitchhiker-103a.postman_collection.json) to test your setup. You can copy into your own Postman client via URL from [here](https://raw.githubusercontent.com/MartinPankraz/SAP-MSTeams-Hero/main/Templates/Hitchhiker-103a.postman_collection.json).

For detailed troubleshooting use SAP transaction `/nsec_diag_tool`.

There are dangerous things out there in the Galaxy, but as you can see principal propagation is mostly harmless🦄 (except pointy thing at the top of the shiny horse).

## 🏆 Success Criteria 🏆

- Be able to call your SAP OData api using an Azure AD authenticated client
- Prove SAP user mapping by maintaining emails (or object ids) on the SAP backend transaction SU01
- (Optionally) Run SAP transaction SEC_DIAG_TOOL to trace OAuth client requests and identify user mapping step on the trace

## 📖 Further Reading

[Azure Application Gateway Web Application Firewall (WAF) v2 Setup for Internet facing SAP Fiori Apps](https://blogs.sap.com/2020/12/03/sap-on-azure-application-gateway-web-application-firewall-waf-v2-setup-for-internet-facing-sap-fiori-apps/)

[Single Sign On Configuration using SAML and Azure Active Directory for Public and Internal URLs](https://blogs.sap.com/2020/12/10/sap-on-azure-single-sign-on-configuration-using-saml-and-azure-active-directory-for-public-and-internal-urls/)
