---
uid: webhooks/receiving/handlers
title: ASP.NET gestori WebHooks . Documenti Microsoft
author: rick-anderson
description: Come gestire le richieste in ASP.NET WebHook.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: ff12dd8df167eca17ecbd9f03a71807d44af2b30
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675221"
---
# <a name="aspnet-webhooks-handlers"></a>ASP.NET gestori WebHooks

Una volta che le richieste WebHooks sono state convalidate da un ricevitore WebHook, sono pronte per essere elaborate dal codice utente. È qui che entrano in gioco *i gestori.* I gestori derivano dall'interfaccia [IWebHookHandler,](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) ma in genere utilizzano la classe [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) anziché derivare direttamente dall'interfaccia.

Una richiesta WebHook può essere elaborata da uno o più gestori. I gestori vengono chiamati in ordine in base alla rispettiva proprietà *Order* che va dal più basso al più alto, dove Order è un numero intero semplice (suggerito di essere compreso tra 1 e 100):

![Diagramma delle proprietà dell'ordine del gestore WebHook](_static/Handlers.png)

Un gestore può facoltativamente impostare la proprietà *Response* su WebHookHandlerContext che porterà l'interruzione dell'elaborazione e la risposta da inviare come risposta HTTP a WebHook. Nel caso precedente, il gestore C non verrà chiamato perché ha un ordine superiore rispetto a B e B imposta la risposta.

L'impostazione della risposta è in genere rilevante solo per i WebHook in cui la risposta può riportare le informazioni all'API di origine. Questo è ad esempio il caso di Slack WebHooks in cui viene eseguito il postback della risposta al canale da cui proviene WebHook. I gestori possono impostare la proprietà Receiver se desiderano solo ricevere WebHook da quel particolare ricevitore. Se non impostano il ricevitore sono chiamati per tutti loro.

Un altro uso comune di una risposta consiste nell'utilizzare una risposta *410 Gone* per indicare che il WebHook non è più attivo e non devono essere inviate ulteriori richieste.

Per impostazione predefinita, un gestore verrà chiamato da tutti i ricevitori WebHook.By default a handler will be called by all WebHook receivers. Tuttavia, se la proprietà *Receiver* è impostata sul nome di un gestore, tale gestore riceverà solo le richieste WebHook da tale ricevitore.

## <a name="processing-a-webhook"></a>Elaborazione di un WebHookProcessing a WebHook

Quando viene chiamato un gestore, ottiene un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) contenente informazioni sulla richiesta WebHook. I dati, in genere il corpo della richiesta HTTP, sono disponibili dalla proprietà *Data.The* data, typically the HTTP request body, is available from the Data property.

Il tipo di dati è in genere dati JSON o HTML del modulo, ma è possibile eseguire il cast a un tipo più specifico, se lo si desidera. Ad esempio, è possibile eseguire il cast dei WebHook personalizzati generati da ASP.NET WebHook al tipo [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) nel modo seguente:

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a>Elaborazione in coda

La maggior parte dei mittenti WebHook invierà nuovamente un WebHook se una risposta non viene generata entro un certo numero di secondi. Ciò significa che il gestore deve completare l'elaborazione entro tale intervallo di tempo per non poter essere chiamato di nuovo.

Se l'elaborazione richiede più tempo o viene gestita meglio separatamente, è possibile usare [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) per inviare la richiesta WebHook a una coda, ad esempio [Coda di archiviazione di Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Di seguito viene fornita una struttura di un'implementazione di [WebHookQueueHandler:](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
