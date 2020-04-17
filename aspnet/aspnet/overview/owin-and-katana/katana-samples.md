---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Esempi di Katana Documenti Microsoft
author: rick-anderson
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 15cc1084b16db2619f2295ee21dec4f49eb2e354
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540441"
---
# <a name="katana-samples"></a>Esempi di Katana

da parte [di Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Esempi di Katana

**Codice** | [sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes) di esempio ASP.NET Routes  
In alcune applicazioni è consigliabile collegare i componenti OWIN nel Asp.Net tabella di route affiancati a componenti non OWIN. In questo esempio viene illustrato come utilizzare i metodi di estensione RouteCollection MapOwinPath e MapOwinRoute forniti da Microsoft.Owin.Host.SystemWeb.

**Codice** | [sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines) di esempio delle pipeline di diramazione  
Le pipeline di elaborazione delle richieste OWIN non devono essere lineari, ma possono essere ramificate per elaborare le richieste in modi diversi. In questo esempio viene illustrato come costruire una pipeline di diramazione in base ai percorsi di richiesta o ad altri dati di richiesta, ad esempio le intestazioni. Questi componenti sono disponibili nel pacchetto Microsoft.Owin.Mapping nuget.

**Custom Server Sample** | [Codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) di esempio server personalizzatoCustom Server Sample Source Code   
Viene illustrato come utilizzare un server OWIN personalizzato quando si esegue l'hosting autonomo OWIN.

**Embedded Sample** | [Codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded) del campione incorporato  
Alcuni server OWIN possono essere eseguiti&quot;all'interno&quot;del proprio processo ( self-hosted ). In questo esempio viene illustrato come avviare un'applicazione OWIN utilizzando gli strumenti forniti dal pacchetto Microsoft.Owin.Hosting nuget.

**HelloWorld Sample** | [Codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld) dell'esempio HelloWorld  
OWIN è un'astrazione dell'API del server HTTP che consente la portabilità delle applicazioni tra vari server. In questo esempio viene illustrato come scrivere un'applicazione Hello World utilizzando alcuni **semplici wrapper** intorno all'astrazione OWIN non elaborata ed eseguirla in un server Web come ASP.NET.

**Hello World Raw OWIN Sample** | [Codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin) del esempio OWIN non elaborato Hello World  
In questo esempio viene illustrato come scrivere un'applicazione Hello World utilizzando l'astrazione OWIN **non elaborata** ed eseguirla in un server Web come Asp.Net.

**SignalR Sample** | [Codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR) del campione SignalR  
Mostra come auto-ospitare SignalR utilizzando OWIN / Katana. Per ulteriori informazioni su SignalR self-hosting, vedere [Esercitazione: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Static Files Sample** | [Codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) di esempio di file statici   
Viene illustrato come supportare le richieste HTTP per i file statici utilizzando OWIN / Katana.

**Web API** | [Codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) API Web   
In questo esempio viene illustrato come ospitare OWIN in IIS e aggiungere API Web alla pipeline OWIN.

**Web Socket Sample** | [Codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) dell'esempio di socket Web   
Viene illustrato come supportare Web Sockets in OWIN utilizzando la classe [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) .
