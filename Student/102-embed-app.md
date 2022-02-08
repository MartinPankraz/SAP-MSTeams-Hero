# Lecture 102: Embed your first SAP web application in Microsoft Teams

[< Previous Challenge](./101-trust-sso.md) - **[🏠Home](../README.md)** - [Next Challenge >](./103-sap-principal-propagation.md)

## 🔭 Introduction

Single Sign On becomes a must-have once you start embedding applications. We took care of the sign on part in lecture 101.

## 📖 Description

- Get your target URL (e.g. BTP Launchpad service) from SAP.
- Add a web app to your Teams Channel and feed it your target URL.
- Enable trust settings on SAP to support embedding (e.g. iFrameDomains setting on BTP CF subaccount).
- Verify Content-Security-Policy requirements.
- (Optional) understand the implications of BTP IdP setting "available for Logon". Keeping only AAD to logon right through without choosing IdP from Teams SAP app call.

## 🏆 Success Criteria

- SAP app embedded as web app within Teams.
