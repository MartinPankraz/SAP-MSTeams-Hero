# Lecture 102: Embed your first SAP web application in Microsoft Teams

[< Previous Challenge](./101-trust-sso.md) - **[🏠Home](../README.md)** - [Next Challenge >](./103a-sap-principal-propagation-basics.md)

## 🔭 Introduction

Single Sign On becomes a must-have once you start embedding applications. We took care of the sign on part in lecture 101.

[![Second session link to YouTube](img/102.png)](https://www.youtube.com/watch?v=a8w7b8WhrHM&list=PLvqyDwoCkBXZ85LoFrNWv9Mj88TiDAc4g&index=3)

## 📖 Description

Continue with the [blog post](https://blogs.sap.com/2022/01/26/integrate-sap-cloud-portal-and-launchpad-service-into-microsoft-teams-including-sso/).

- Get your target URL (e.g. BTP Launchpad service) from SAP.
- Add a web app to your Teams Channel and feed it your target URL.
- Enable trust settings on SAP to support embedding (e.g. iFrameDomains setting on BTP CF subaccount). Check provided [Postman collection](../Templates/BTP-Security-API.postman_collection.json) for CF example.
- Verify [Content-Security-Policy](https://developer.mozilla.org/docs/Web/HTTP/CSP) requirements.
- (Optional) understand the implications of BTP IdP setting "available for Logon". Keeping only AAD to logon right through without choosing IdP from Teams SAP app call (hint: Revisit the blog post).

## 🏆 Success Criteria

SAP app embedded as web app within Teams.
