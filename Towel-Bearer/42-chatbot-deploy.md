# Lecture 42: Build upon the solution for everything - Deploy your first Chatbot to Teams and query/change data in SAP

[< Previous Challenge](./103b-sap-principal-propagation-apim.md) - **[🏠Home](../README.md)**

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

- Familiarize yourself with this [repo](https://github.com/ROBROICH/Teams-Chatbot-SAP-NW-Principal-Propagation) and Bot [Devblog](https://devblogs.microsoft.com/microsoft365dev/building-great-bots-for-microsoft-teams-with-azure-bot-framework-composer/).
- Setup your [Bot Framework Composer development environment](https://docs.microsoft.com/composer/install-composer?tabs=windows) including [Bot Framework Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases/tag/v4.14.1) (Releases -> Assets) and [ngrok](https://ngrok.com/download).
- Create a new Azure Bot
- Add Web platform to app registration on Azure AD of your bot and maintain Redirect Uris `https://token.botframework.com/.auth/web/redirect`.
- Configure your bot to perform [OAuth login](https://docs.microsoft.com/s/composer/how-to-use-oauth?tabs=v2x) (Access External Resources -> OAuth Login) as you did for [103b](./103b-sap-principal-propagation-apim.md). Store your token using bot scope `dialog.myoauth`.
- Maintain OAuth Connection Settings for AAD v2 on your Azure Bot. Hint: Scopes likely similar to `api://<appid>/SAP.ReadWrite`
- Configure your bot with a [task](https://docs.microsoft.com/composer/how-to-send-http-request?tabs=v2x) that targets your OData API on Azure APIM (Access External Resources -> Send an HTTP request).
- Design an [Adaptive Card](https://adaptivecards.io/designer/) or use a [hero card](https://docs.microsoft.com/composer/how-to-send-cards?tabs=v2x#card-types) to present the result to the end-user of the bot. Hint: Use `Send a Response` task with Attachment.

Snippet for hero card and response structure for demo SAP OData service *epm_ref_apps_prod_man_srv*.

```javascript
[HeroCard
  title = ${dialog.api_response.content.d.Name}
  subtitle = ${dialog.api_response.content.d.Price} ${dialog.api_response.content.d.CurrencyCode}
  text = ${dialog.api_response.content.d.Description}
  image = https://apimngtowelsap.azure-api.net${dialog.api_response.content.d.ImageUrl}
]
```

## 🏆 Success Criteria

- Be able to retrieve SAP data via chatbot, that is Microsoft Teams enabled (e.g. using simulator) and present result within an adaptive card.
- (Optionally) Add interaction to the adaptive card.
- (Optionally) Deploy bot within Teams.

## So Long, and Thanks for All the Fish! 🐠

## 📖 Further Reading

- [Add user authentication to Azure Bot](https://docs.microsoft.com/s/composer/how-to-use-oauth?tabs=v2x)
- [Make an http request from Bot Composer](https://docs.microsoft.com/composer/how-to-send-http-request?tabs=v2x)
- [Martin Raepple's series on SAP Principal Propagation](https://blogs.sap.com/2021/04/13/principal-propagation-in-a-multi-cloud-solution-between-microsoft-azure-and-sap-business-technology-platform-btp-part-iv-sso-with-a-power-virtual-agent-chatbot-and-on-premises-data-gateway/)
