---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Informazioni sul ASP.NET processo di esecuzione di MVC . Documenti Microsoft
author: rick-anderson
description: Informazioni su come il framework mvc ASP.NET elabora una richiesta del browser passo-passo.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 48afbbe7349b80e0ed0b9bab987ae3ccda493aca
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541026"
---
# <a name="understanding-the-aspnet-mvc-execution-process"></a>Informazioni sul processo di esecuzione di ASP.NET MVC

da parte [di Microsoft](https://github.com/microsoft)

> Informazioni su come il framework mvc ASP.NET elabora una richiesta del browser passo-passo.

Le richieste a un'applicazione Web basata su MVC ASP.NET passano innanzitutto l'oggetto **UrlRoutingModule,** che è un modulo HTTP. Questo modulo analizza la richiesta ed esegue la selezione delle route. L'oggetto **UrlRoutingModule** seleziona il primo oggetto route che corrisponde alla richiesta corrente. Un oggetto route è una classe che implementa **RouteBase**e in genere è un'istanza della classe **Route.** Se nessuna route corrisponde, l'oggetto **UrlRoutingModule** non esegue alcuna operazione e consente alla richiesta di eseguire il rollback alla normale elaborazione delle ASP.NET o delle richieste IIS.

Dall'oggetto **Route** selezionato, l'oggetto **UrlRoutingModule** ottiene l'oggetto **IRouteHandler** associato all'oggetto **Route.** In genere, in un'applicazione MVC, si tratterà di un'istanza di **MvcRouteHandler**. L'istanza **IRouteHandler** crea un oggetto **IHttpHandler** e gli passa l'oggetto **IHttpContext.** Per impostazione predefinita, l'istanza **di IHttpHandler** per MVC è l'oggetto **MvcHandler.** **L'oggetto MvcHandler** seleziona quindi il controller che gestirà la richiesta.

> [!NOTE]
> Quando un'applicazione MVC ASP.NET viene eseguita in IIS 7.0, per i progetti MVC non è necessaria alcuna estensione di file. In IIS 6.0 il gestore richiede tuttavia che si esegua il mapping dell'estensione di file mvc alla DLL ISAPI ASP.NET.

Il modulo e il gestore sono i punti di ingresso al framework MVC ASP.NET. e consentono di eseguire le seguenti azioni:

- Selezionare il controller appropriato in un'applicazione Web MVC.
- Ottenere un'istanza del controller specifica.
- Chiamare il metodo **Execute** del controller.

Di seguito sono elencate le fasi di esecuzione per un progetto Web MVC:

- Ricezione della prima richiesta per l'applicazione 

    - Nel file Global.asax, gli oggetti **Route** vengono aggiunti all'oggetto **RouteTable.**
- Esecuzione del routing 

    - Il modulo **UrlRoutingModule** utilizza il primo oggetto **Route** corrispondente nella raccolta **RouteTable** per creare l'oggetto **RouteData,** che viene quindi utilizzato per creare un oggetto **RequestContext** (**IHttpContext**).
- Creazione del gestore di richieste MVC 

    - L'oggetto **MvcRouteHandler** crea un'istanza della classe **MvcHandler** e le passa l'istanza **RequestContext.**
- Creazione del controller 

    - Il **MvcHandler** oggetto utilizza il **RequestContext** istanza per identificare il **IControllerFactory** oggetto (in genere un'istanza di **DefaultControllerFactory** classe) per creare l'istanza del controller con.
- Esegui controller: l'istanza **MvcHandler** chiama il metodo **Execute** del controller s . |
- Richiamo dell'azione 

    - La maggior parte dei controller eredita dalla classe di base **Controller.** Per i controller che lo fanno, il **ControllerActionInvoker** oggetto associato al controller determina quale metodo di azione della classe controller chiamare e quindi chiama tale metodo.
- Esecuzione del risultato 

    - Un metodo di azione tipico potrebbe ricevere l'input dell'utente, preparare i dati di risposta appropriati e quindi eseguire il risultato restituendo un tipo di risultato. I tipi di risultato predefiniti che possono essere eseguiti includono i seguenti: **ViewResult** (che esegue il rendering di una visualizzazione ed è il tipo di risultato utilizzato più di frequente), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**e **EmptyResult**.
