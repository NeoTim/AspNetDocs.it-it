---
uid: webhooks/receiving/receivers
title: ricevitori ASP.NET WebHooks Documenti Microsoft
author: rick-anderson
description: ricevitori ASP.NET WebHooks
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.openlocfilehash: 60f46141b59fc3888a6480d8201160420469d1a7
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675158"
---
# <a name="aspnet-webhooks-receivers"></a>ricevitori ASP.NET WebHooks

La ricezione di WebHook dipende da chi è il mittente. A volte ci sono passaggi aggiuntivi che registrano un WebHook verificando che il sottoscrittore sia realmente in ascolto. Alcuni WebHook forniscono un modello push-to-pull in cui la richiesta HTTP POST contiene solo un riferimento alle informazioni sull'evento che devono quindi essere recuperate in modo indipendente. Spesso il modello di sicurezza varia un po '.

Lo scopo di Microsoft ASP.NET WebHooks è quello di rendere più semplice e più coerente per collegare la vostra API senza spendere un sacco di tempo capire come gestire qualsiasi particolare variante di WebHooks.

Un ricevitore WebHook è responsabile dell'accettazione e della verifica di WebHook da un determinato mittente. Un ricevitore WebHook può supportare un numero qualsiasi di WebHook, ognuno con la propria configurazione. Ad esempio, il ricevitore GitHub WebHook può accettare WebHook da un numero qualsiasi di repository GitHub.For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.

## <a name="webhook-receiver-uris"></a>URI ricevitore WebHook

Installando Microsoft ASP.NET WebHooksi ottiene un controller WebHook generale che accetta richieste WebHook da un numero aperto di servizi. Quando arriva una richiesta, seleziona il destinatario appropriato installato per la gestione di un determinato mittente WebHook.

L'URI di questo controller è l'URI WebHook registrato con il servizio ed è nel formato:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Per motivi di sicurezza, molti ricevitori WebHook richiedono che l'URI sia un URI *https* e in alcuni casi deve contenere anche un parametro di query aggiuntivo che viene utilizzato per imporre che solo l'entità desiderata possa inviare WebHook all'URI precedente.

Il `<receiver>` componente è il nome del `github` `slack`ricevitore, ad esempio o .

*L'ID* è un identificatore facoltativo che può essere utilizzato per identificare una particolare configurazione del ricevitore WebHook. Questo può essere utilizzato per registrare N WebHooks con un particolare ricevitore. Ad esempio, i tre URI seguenti possono essere utilizzati per la registrazione di tre WebHook indipendenti:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Installazione di un ricevitore WebHook

Per ricevere WebHook utilizzando Microsoft ASP.NET WebHook, è innanzitutto necessario installare il pacchetto Nuget per il provider o i provider WebHook da cui si desidera ricevere WebHook. I pacchetti Nuget sono denominati [Microsoft.AspNet.WebHooks.Receivers.](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) Ad esempio:

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) fornisce il supporto per la ricezione di WebHook da GitHub e [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) fornisce supporto per la ricezione di WebHook generati da WebHook ASP.NET.

Fuori dalla scatola è possibile trovare il supporto per Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, e WordPress, ma è possibile supportare qualsiasi numero di altri fornitori.

## <a name="configuring-a-webhook-receiver"></a>Configurazione di un ricevitore WebHookConfiguring a WebHook Receiver

I ricevitori WebHook vengono configurati tramite l'interfaccia [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) e particolari implementazioni di tale interfaccia possono essere registrate utilizzando qualsiasi modello di inserimento delle dipendenze. L'implementazione predefinita usa le impostazioni dell'applicazione che possono essere impostate nel file Web.config oppure, se si usano app Web di Azure, tramite il portale di [Azure.](https://portal.azure.com/)

![Impostazioni app di Azure](_static/AzureAppSettings.png)

Il formato per le chiavi di impostazione dell'applicazione è il seguente:The format for Application Setting keys is as follows:

```
MS_WebHookReceiverSecret_<receiver>
```

Il valore è un elenco separato da virgole di valori che corrispondono *ai* valori di ID per i quali sono stati registrati WebHook, ad esempio:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Inizializzazione di un ricevitore WebHook

I ricevitori WebHook vengono inizializzati registrandoli, in genere nella classe statica *WebApiConfig,* ad esempio:

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
