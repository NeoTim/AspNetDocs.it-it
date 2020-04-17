---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: Informazioni sui filtri delle azioni Documenti Microsoft
author: rick-anderson
description: L'obiettivo di questa esercitazione è spiegare i filtri azione. Un filtro azione è un attributo che è possibile applicare a un'azione del controller, o a un intero controller...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: 75ba7b1dce280a45cd092de97c464eade5f49838
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542183"
---
# <a name="understanding-action-filters-c"></a>Informazioni sui filtri per azioni (C#)

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> L'obiettivo di questa esercitazione è spiegare i filtri azione. Un filtro azione è un attributo che è possibile applicare a un'azione del controller, o a un intero controller, che modifica il modo in cui viene eseguita l'azione.

## <a name="understanding-action-filters"></a>Informazioni sui filtri azione

L'obiettivo di questa esercitazione è spiegare i filtri azione. Un filtro azione è un attributo che è possibile applicare a un'azione del controller, o a un intero controller, che modifica il modo in cui viene eseguita l'azione. Il framework di MVC ASP.NET include diversi filtri azione:

- OutputCache: questo filtro azione memorizza nella cache l'output di un'azione del controller per un periodo di tempo specificato.
- HandleError: questo filtro azione gestisce gli errori generati quando viene eseguita un'azione del controller.
- Autorizza: questo filtro azioni consente di limitare l'accesso a un determinato utente o ruolo.

È inoltre possibile creare filtri azione personalizzati. Ad esempio, è possibile creare un filtro azione personalizzato per implementare un sistema di autenticazione personalizzato. In alternativa, è possibile creare un filtro azione che modifica i dati di visualizzazione restituiti da un'azione del controller.

In questa esercitazione imparerai a creare un filtro d'azione da zero. Viene creato un filtro azione log che registra diverse fasi dell'elaborazione di un'azione nella finestra Output di Visual Studio.We create a Log action filter that logs different stages of the processing of an action to the Visual Studio Output window.

### <a name="using-an-action-filter"></a>Utilizzo di un filtro azioni

Un filtro azione è un attributo. È possibile applicare la maggior parte dei filtri azione a una singola azione del controller o a un intero controller.

Ad esempio, il controller di dati nel `Index()` listato 1 espone un'azione denominata che restituisce l'ora corrente. Questa azione è `OutputCache` decorata con il filtro azione. Questo filtro fa sì che il valore restituito dall'azione venga memorizzato nella cache per 10 secondi.

**Lista torto 1 –`Controllers\DataController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

Se si richiama `Index()` ripetutamente l'azione immettendo l'URL /Data/Index nella barra degli indirizzi del browser e premendo il pulsante Aggiorna più volte, verrà visualizzato lo stesso tempo per 10 secondi. L'output `Index()` dell'azione viene memorizzato nella cache per 10 secondi (vedere Figura 1).

[![Tempo memorizzato nella cache](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)

**Figura 01**: Tempo memorizzato nella cache ([Fare clic per visualizzare l'immagine a dimensioni intere](understanding-action-filters-cs/_static/image3.png))

Nel listato 1, al `OutputCache` `Index()` metodo viene applicato un singolo filtro azione, ovvero il filtro azione. Se necessario, è possibile applicare più filtri azione alla stessa azione. Ad esempio, è possibile applicare `OutputCache` `HandleError` entrambi i filtri di azione e alla stessa azione.

Nel listato `OutputCache` 1, il filtro `Index()` azione viene applicato all'azione. È anche possibile applicare `DataController` questo attributo alla classe stessa. In tal caso, il risultato restituito da qualsiasi azione esposta dal controller verrà memorizzato nella cache per 10 secondi.

### <a name="the-different-types-of-filters"></a>I diversi tipi di filtri

Il framework di ASP.NET MVC supporta quattro diversi tipi di filtri:

1. Filtri di autorizzazione: implementa l'attributo. `IAuthorizationFilter`
2. Filtri azione: `IActionFilter` implementa l'attributo.
3. Filtri dei risultati `IResultFilter` : Implementa l'attributo.
4. Filtri eccezioni: `IExceptionFilter` implementa l'attributo .

I filtri vengono eseguiti nell'ordine sopra elencato. Ad esempio, i filtri di autorizzazione vengono sempre eseguiti prima dei filtri azione e i filtri eccezioni vengono sempre eseguiti dopo ogni altro tipo di filtro.

I filtri di autorizzazione vengono utilizzati per implementare l'autenticazione e l'autorizzazione per le azioni del controller. Ad esempio, il filtro Autorizza è un esempio di filtro Autorizzazione.

I filtri azione contengono la logica che viene eseguita prima e dopo l'esecuzione di un'azione del controller. È possibile utilizzare un filtro azioni, ad esempio, per modificare i dati di visualizzazione restituiti da un'azione del controller.

I filtri dei risultati contengono la logica che viene eseguita prima e dopo l'esecuzione di un risultato della visualizzazione. Ad esempio, è possibile modificare il risultato di una vista subito prima che venga eseguito il rendering della vista nel browser.

I filtri eccezioni sono l'ultimo tipo di filtro da eseguire. È possibile usare un filtro eccezioni per gestire gli errori generati dalle azioni del controller o dai risultati dell'azione del controller. È inoltre possibile utilizzare i filtri eccezioni per registrare gli errori.

Ogni tipo diverso di filtro viene eseguito in un ordine particolare. Se si desidera controllare l'ordine di esecuzione dei filtri dello stesso tipo, è possibile impostare la proprietà Order di un filtro.

La classe base per tutti `System.Web.Mvc.FilterAttribute` i filtri azione è la classe. Se si desidera implementare un particolare tipo di filtro, è necessario creare una classe che eredita `IAuthorizationFilter` `IActionFilter`dalla `IResultFilter`classe `IExceptionFilter` Filter di base e implementa una o più interfacce , , o .

### <a name="the-base-actionfilterattribute-class"></a>La classe Base ActionFilterAttribute

Per semplificare l'implementazione di un filtro azione personalizzato, il `ActionFilterAttribute` framework MVC ASP.NET include una classe base. Questa classe implementa `IActionFilter` `IResultFilter` le interfacce e `Filter` e dalla classe .

La terminologia qui non è del tutto coerente. Tecnicamente, una classe che eredita dalla classe ActionFilterAttribute è sia un filtro azione che un filtro dei risultati. Tuttavia, in senso sciolto, il filtro azione parola viene utilizzato per fare riferimento a qualsiasi tipo di filtro nel ASP.NET framework MVC.

La `ActionFilterAttribute` classe base dispone dei seguenti metodi di cui è possibile eseguire l'override:

- OnActionExecuting : questo metodo viene chiamato prima dell'esecuzione di un'azione del controller.
- OnActionExecuted : questo metodo viene chiamato dopo l'esecuzione di un'azione del controller.
- OnResultExecuting: questo metodo viene chiamato prima dell'esecuzione di un risultato dell'azione del controller.
- OnResultExecuted : questo metodo viene chiamato dopo l'esecuzione di un risultato dell'azione del controller.

Nella sezione successiva, vedremo come è possibile implementare ciascuno di questi diversi metodi.

### <a name="creating-a-log-action-filter"></a>Creazione di un filtro azioni logCreating a Log Action Filter

Per illustrare come è possibile compilare un filtro azione personalizzato, verrà creato un filtro azione personalizzato che registra le fasi dell'elaborazione di un'azione del controller nella finestra Output di Visual Studio.In to illustrate how you can build a custom action filter, we'll create a custom action filter that logs the stages of processing a controller action to the Visual Studio Output window. Il `LogActionFilter` nostro è contenuto nella lista 2.

**Listato 2 –`ActionFilters\LogActionFilter.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

Nel listato `OnActionExecuting()`2, `OnResultExecuting()`i `OnResultExecuted()` metodi , `Log()` `OnActionExecuted()`, e chiamano tutti il metodo . Il nome del metodo e i dati `Log()` della route corrente vengono passati al metodo. Il `Log()` metodo scrive un messaggio nella finestra Output di Visual Studio (vedere Figura 2).

[![Scrittura nella finestra Output di Visual StudioWriting to the Visual Studio Output window](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)

**Figura 02**: Scrittura nella finestra Output di Visual Studio ([Fare clic per visualizzare l'immagine](understanding-action-filters-cs/_static/image6.png)a dimensioni intere )

Il controller Home nel listato 3 illustra come applicare il filtro azione Log a un'intera classe controller. Ogni volta che viene richiamata una delle azioni `Index()` esposte `About()` dal controller Home, ovvero il metodo o il metodo, le fasi dell'elaborazione dell'azione vengono registrate nella finestra Output di Visual Studio.

**Lista 3 –`Controllers\HomeController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a>Riepilogo

In questa esercitazione sono stati introdotti a ASP.NETi filtri di azione MVC. Sono stati acquisiti i quattro diversi tipi di filtri: filtri di autorizzazione, filtri azione, filtri risultati e filtri eccezioni. Hai anche imparato `ActionFilterAttribute` a conoscere la classe base.

Infine, si è appreso come implementare un filtro azione semplice. È stato creato un filtro azione di log che registra le fasi dell'elaborazione di un'azione del controller nella finestra Output di Visual Studio.We created a Log action filter that logs the stages of processing a controller action to the Visual Studio Output window.

> [!div class="step-by-step"]
> [Successivo](asp-net-mvc-routing-overview-cs.md)
> [precedente](improving-performance-with-output-caching-cs.md)
