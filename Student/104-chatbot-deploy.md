# Lecture 104: Deploy your first Chatbot to Teams and query/change data in SAP

[< Previous Challenge](./103-sap-principal-propagation.md) - **[🏠Home](../README.md)**

## 🔭 Introduction

After you exposed your SAP backend APIs through Azure APIM, you are all set to connect client apps. A chatbot 🤖 is arguably the most interesting one. There are three chatbot implementation options for Chatbots in Microsoft Teams.

|| [Bot Framework SDK](https://docs.microsoft.com/azure/bot-service/bot-service-quickstart-create-bot?view=azure-bot-service-4.0&tabs=csharp%2Cvs) | [Bot Framework Composer](https://docs.microsoft.com/composer/introduction?tabs=v2x) | [Power Virtual Agent (PVA)](https://docs.microsoft.com/power-virtual-agents/teams/fundamentals-what-is-power-virtual-agents-teams) |
|----------|-------------|------|---|
| coding style |  full-code | low-code | low-code |
| programming language | C#, Java, NodeJS and Python | built-in designer with PowerFX scripting language | built-in designer with PowerFX scripting language |
| complexity | high, but fully flexible | medium, more freedom than PVA but less than SDK | low |
| required dev environment setup | IDE of choice, build-environment (e.g. NPM), simulator, etc. | shipped with Composer studio | built into Microsoft Teams |

We will discuss Bot Framework Composer as part of the lecture.

## 📖 Description

- Familiarize yourself with this [repo](https://github.com/ROBROICH/Teams-Chatbot-SAP-NW-Principal-Propagation).
- Setup your [Bot Framework Composer development environment](https://docs.microsoft.com/composer/install-composer?tabs=windows)
- Configure your bot with a task that targets your OData API.
- Design an [Adaptive Card](https://adaptivecards.io/designer/) to present the result to user of the bot.

## 🏆 Success Criteria

- Be able to retrieve SAP data via chatbot, that is Microsoft Teams enabled (e.g. using simulator).
- (Optionally) Deployed bot within Teams
