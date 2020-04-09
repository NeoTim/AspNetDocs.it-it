---
uid: webhooks/diagnostics/logging
title: ASP.NET di registrazione di WebHooks Documenti Microsoft
author: rick-anderson
description: Come eseguire la registrazione in ASP.NET WebHook.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.openlocfilehash: 350732acbd3a73bddb8f8b20dcd50c225d89be82
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675349"
---
# <a name="aspnet-webhooks-logging"></a>ASP.NET di registrazione di WebHook

Microsoft ASP.NET WebHooks utilizza la registrazione per segnalare problemi e problemi. Per impostazione predefinita i log vengono scritti utilizzando [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) in cui possono essere mandati utilizzando [i listener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) di traccia come qualsiasi altro flusso di log.

Quando si distribuisce l'applicazione Web come app Web di Azure, i log vengono raccolti automaticamente e possono essere gestiti insieme a qualsiasi altra registrazione [System.Diagnostics.Trace.When](https://msdn.microsoft.com/library/system.diagnostics.trace) deploying your Web Application as an Azure Web App, the logs are automatically picked up and can be managed together with any other System.Diagnostics.Trace logging. Per informazioni dettagliate, vedere [Abilitare la registrazione diagnostica per le app Web nel servizio app](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/) di AzureFor details, please see Enable diagnostics logging for web apps in Azure App Service

Inoltre, i log possono essere ottenuti direttamente dall'interno di Visual Studio come descritto in Risolvere i problemi di [un'app Web nel servizio app di Azure usando Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Reindirizzamento dei registri

Anziché scrivere log in [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), è possibile fornire un'implementazione di registrazione alternativa in grado di accedere direttamente a un gestore di log, ad esempio [Log4Net](http://logging.apache.org/log4net/) e [NLog](http://nlog-project.org/). È sufficiente fornire un'implementazione di [ILogger](https://github.com/aspnet/AspNetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) e registrarlo con un motore di inserimento delle dipendenze di propria scelta e verrà prelevato da Microsoft ASP.NET WebHooks. Per informazioni [dettagliate,](https://www.asp.net/web-api/overview/advanced/dependency-injection) vedere Inserimento delle dipendenze in ASP.NET API Web 2.Please see Dependency Injection in ASP.NET Web API 2 for details.
