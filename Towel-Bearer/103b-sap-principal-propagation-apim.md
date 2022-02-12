# Lecture 103b: Streamline SAP Principal Propagation with API Management

[< Previous Challenge](./103a-sap-principal-propagation-basics.md) - **[🏠Home](../README.md)** - [Next Challenge >](./42-chatbot-deploy.md)

## 🔭 Introduction

So far so good. You have established the famous "SAP Principal Propagation". It doesn't stop there though. Often there is a desire to deal with the complexity of this authentication mechanism in one place and solve for all clients centrally. We will be employing Azure APIM for that purpose.

## 📖 Description

Familiarize yourself with this blog post "[AzureSAPODataReader](https://github.com/MartinPankraz/AzureSAPODataReader)" and [GitHub repos](https://blogs.sap.com/2021/08/12/.net-speaks-odata-too-how-to-implement-azure-app-service-with-sap-odata-gateway/) to learn more about the context.

- Ensure access to your APIM exposed API endpoints. Are you running externally only? Hybrid? Internet facing?
- [Convert](https://aka.ms/ODataOpenAPI) your OData $metadata to OpenAPIv3 and import into Azure APIM
- Add [SAP Principal Propagation policy](https://github.com/Azure/api-management-policy-snippets/blob/master/examples/Request%20OAuth2%20access%20token%20from%20SAP%20using%20AAD%20JWT%20token.xml) to your OData api
- (Optionally) Build upone the [Postman collection](https://github.com/MartinPankraz/AzureSAPODataReader/blob/master/Templates/AAD_APIM_SAP_Principal_Propagation.postman_collection.json) to test your setup

## 🏆 Success Criteria

- Be able to call your SAP OData api through APIM using an Azure AD authenticated client
- Offloaded SAP Principal Propagation into APIM.
