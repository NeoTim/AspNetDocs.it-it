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
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="b4ec5-103">ricevitori ASP.NET WebHooks</span><span class="sxs-lookup"><span data-stu-id="b4ec5-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="b4ec5-104">La ricezione di WebHook dipende da chi è il mittente.</span><span class="sxs-lookup"><span data-stu-id="b4ec5-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="b4ec5-105">A volte ci sono passaggi aggiuntivi che registrano un WebHook verificando che il sottoscrittore sia realmente in ascolto.</span><span class="sxs-lookup"><span data-stu-id="b4ec5-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="b4ec5-106">Alcuni WebHook forniscono un modello push-to-pull in cui la richiesta HTTP POST contiene solo un riferimento alle informazioni sull'evento che devono quindi essere recuperate in modo indipendente.</span><span class="sxs-lookup"><span data-stu-id="b4ec5-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="b4ec5-107">Spesso il modello di sicurezza varia un po '.</span><span class="sxs-lookup"><span data-stu-id="b4ec5-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="b4ec5-108">Lo scopo di Microsoft ASP.NET WebHooks è quello di rendere più semplice e più coerente per collegare la vostra API senza spendere un sacco di tempo capire come gestire qualsiasi particolare variante di WebHooks.</span><span class="sxs-lookup"><span data-stu-id="b4ec5-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="b4ec5-109">Un ricevitore WebHook è responsabile dell'accettazione e della verifica di WebHook da un determinato mittente.</span><span class="sxs-lookup"><span data-stu-id="b4ec5-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="b4ec5-110">Un ricevitore WebHook può supportare un numero qualsiasi di WebHook, ognuno con la propria configurazione.</span><span class="sxs-lookup"><span data-stu-id="b4ec5-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="b4ec5-111">Ad esempio, il ricevitore GitHub WebHook può accettare WebHook da un numero qualsiasi di repository GitHub.For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span><span class="sxs-lookup"><span data-stu-id="b4ec5-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="b4ec5-112">URI ricevitore WebHook</span><span class="sxs-lookup"><span data-stu-id="b4ec5-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="b4ec5-113">Installando Microsoft ASP.NET WebHooksi ottiene un controller WebHook generale che accetta richieste WebHook da un numero aperto di servizi.</span><span class="sxs-lookup"><span data-stu-id="b4ec5-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="b4ec5-114">Quando arriva una richiesta, seleziona il destinatario appropriato installato per la gestione di un determinato mittente WebHook.</span><span class="sxs-lookup"><span data-stu-id="b4ec5-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="b4ec5-115">L'URI di questo controller è l'URI WebHook registrato con il servizio ed è nel formato:</span><span class="sxs-lookup"><span data-stu-id="b4ec5-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="b4ec5-116">Per motivi di sicurezza, molti ricevitori WebHook richiedono che l'URI sia un URI *https* e in alcuni casi deve contenere anche un parametro di query aggiuntivo che viene utilizzato per imporre che solo l'entità desiderata possa inviare WebHook all'URI precedente.</span><span class="sxs-lookup"><span data-stu-id="b4ec5-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="b4ec5-117">Il `<receiver>` componente è il nome del `github` `slack`ricevitore, ad esempio o .</span><span class="sxs-lookup"><span data-stu-id="b4ec5-117">The `<receiver>` component is the name of the receiver, for example `github` or `slack`.</span></span>

<span data-ttu-id="b4ec5-118">*L'ID* è un identificatore facoltativo che può essere utilizzato per identificare una particolare configurazione del ricevitore WebHook.</span><span class="sxs-lookup"><span data-stu-id="b4ec5-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="b4ec5-119">Questo può essere utilizzato per registrare N WebHooks con un particolare ricevitore.</span><span class="sxs-lookup"><span data-stu-id="b4ec5-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="b4ec5-120">Ad esempio, i tre URI seguenti possono essere utilizzati per la registrazione di tre WebHook indipendenti:</span><span class="sxs-lookup"><span data-stu-id="b4ec5-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="b4ec5-121">Installazione di un ricevitore WebHook</span><span class="sxs-lookup"><span data-stu-id="b4ec5-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="b4ec5-122">Per ricevere WebHook utilizzando Microsoft ASP.NET WebHook, è innanzitutto necessario installare il pacchetto Nuget per il provider o i provider WebHook da cui si desidera ricevere WebHook.</span><span class="sxs-lookup"><span data-stu-id="b4ec5-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="b4ec5-123">I pacchetti Nuget sono denominati [Microsoft.AspNet.WebHooks.Receivers.](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers)</span><span class="sxs-lookup"><span data-stu-id="b4ec5-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="b4ec5-124">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b4ec5-124">For example</span></span>

<span data-ttu-id="b4ec5-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) fornisce il supporto per la ricezione di WebHook da GitHub e [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) fornisce supporto per la ricezione di WebHook generati da WebHook ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b4ec5-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="b4ec5-126">Fuori dalla scatola è possibile trovare il supporto per Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, e WordPress, ma è possibile supportare qualsiasi numero di altri fornitori.</span><span class="sxs-lookup"><span data-stu-id="b4ec5-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="b4ec5-127">Configurazione di un ricevitore WebHookConfiguring a WebHook Receiver</span><span class="sxs-lookup"><span data-stu-id="b4ec5-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="b4ec5-128">I ricevitori WebHook vengono configurati tramite l'interfaccia [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) e particolari implementazioni di tale interfaccia possono essere registrate utilizzando qualsiasi modello di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="b4ec5-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="b4ec5-129">L'implementazione predefinita usa le impostazioni dell'applicazione che possono essere impostate nel file Web.config oppure, se si usano app Web di Azure, tramite il portale di [Azure.](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="b4ec5-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Impostazioni app di Azure](_static/AzureAppSettings.png)

<span data-ttu-id="b4ec5-131">Il formato per le chiavi di impostazione dell'applicazione è il seguente:The format for Application Setting keys is as follows:</span><span class="sxs-lookup"><span data-stu-id="b4ec5-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="b4ec5-132">Il valore è un elenco separato da virgole di valori che corrispondono *ai* valori di ID per i quali sono stati registrati WebHook, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b4ec5-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="b4ec5-133">Inizializzazione di un ricevitore WebHook</span><span class="sxs-lookup"><span data-stu-id="b4ec5-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="b4ec5-134">I ricevitori WebHook vengono inizializzati registrandoli, in genere nella classe statica *WebApiConfig,* ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b4ec5-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

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
