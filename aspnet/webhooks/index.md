---
uid: webhooks/index
title: Cenni preliminari su ASP.NET WebHooks - Documenti Microsoft
author: rick-anderson
description: Introduzione a ASP.NET WebHook.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: aa5fa190386ec803a6801de2d815c948677fe1f5
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675435"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="3c643-103">Panoramica di ASP.NET WebHooks</span><span class="sxs-lookup"><span data-stu-id="3c643-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="3c643-104">WebHooks è un modello HTTP leggero che fornisce un semplice modello di pub/sub per il cablaggio insieme di API Web e servizi SaaS.</span><span class="sxs-lookup"><span data-stu-id="3c643-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="3c643-105">Quando si verifica un evento in un servizio, viene inviata una notifica sotto forma di richiesta HTTP POST ai sottoscrittori registrati.</span><span class="sxs-lookup"><span data-stu-id="3c643-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="3c643-106">La richiesta POST contiene informazioni sull'evento che consentono al destinatario di agire di conseguenza.</span><span class="sxs-lookup"><span data-stu-id="3c643-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="3c643-107">A causa della loro semplicità, i WebHook sono già esposti da un gran numero di servizi tra cui [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)e molti altri.</span><span class="sxs-lookup"><span data-stu-id="3c643-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="3c643-108">Ad esempio, un WebHook può indicare che un file è stato modificato in [Dropbox](http://dropbox.com/)o che è stato eseguito il commit di una modifica del codice in GitHub oppure che è stato avviato un pagamento in [PayPal](http://www.paypal.com/)oppure che è stata creata una carta in [Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="3c643-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="3c643-109">Le possibilità sono infinite!</span><span class="sxs-lookup"><span data-stu-id="3c643-109">The possibilities are endless!</span></span>

<span data-ttu-id="3c643-110">Microsoft ASP.NET WebHooks semplifica l'invio e la ricezione di WebHook nell'ambito dell'applicazione ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="3c643-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="3c643-111">Sul lato ricevente, fornisce un modello comune per la ricezione e l'elaborazione di WebHook da qualsiasi numero di provider WebHook.</span><span class="sxs-lookup"><span data-stu-id="3c643-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="3c643-112">Esce dalla scatola con il supporto per [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) e [.](https://www.zendesk.com/)</span><span class="sxs-lookup"><span data-stu-id="3c643-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="3c643-113">Sul lato di invio fornisce il supporto per la gestione e l'archiviazione delle sottoscrizioni, nonché per l'invio di notifiche di eventi al set di sottoscrittori corretto.</span><span class="sxs-lookup"><span data-stu-id="3c643-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="3c643-114">Ciò consente di definire il proprio set di eventi che i sottoscrittori possono sottoscrivere e notificare loro quando si verificano le cose.</span><span class="sxs-lookup"><span data-stu-id="3c643-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="3c643-115">Le due parti possono essere utilizzate insieme o tra di parte a seconda dello scenario.</span><span class="sxs-lookup"><span data-stu-id="3c643-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="3c643-116">Se hai solo bisogno di ricevere WebHook da altri servizi, puoi utilizzare solo la parte del ricevitore; se si desidera esporre solo WebHook per altri utenti da utilizzare, è possibile farlo.</span><span class="sxs-lookup"><span data-stu-id="3c643-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="3c643-117">Il codice è destinato a ASP.NET API Web 2 e ASP.NET MVC 5 ed è disponibile come [OSS in GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="3c643-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="3c643-118">Panoramica di WebHooks</span><span class="sxs-lookup"><span data-stu-id="3c643-118">WebHooks Overview</span></span>

<span data-ttu-id="3c643-119">WebHooks è un modello che significa che varia come viene utilizzato da un servizio all'altro, ma l'idea di base è la stessa.</span><span class="sxs-lookup"><span data-stu-id="3c643-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="3c643-120">È possibile considerare WebHooks come un semplice modello pub/sub in cui un utente può sottoscrivere eventi che si verificano altrove.</span><span class="sxs-lookup"><span data-stu-id="3c643-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="3c643-121">Le notifiche degli eventi vengono propagate come richieste HTTP POST contenenti informazioni sull'evento stesso.</span><span class="sxs-lookup"><span data-stu-id="3c643-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="3c643-122">In genere la richiesta HTTP POST contiene un oggetto JSON o dati del modulo HTML determinati dal mittente Di WebHook, incluse informazioni sull'evento che causa il trigger di WebHook.</span><span class="sxs-lookup"><span data-stu-id="3c643-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="3c643-123">Ad esempio, un corpo della richiesta WEBHook POST da GitHub si presenta come risultato di un nuovo problema aperto in un determinato repository:For example, a WebHook POST request body from [GitHub](https://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span><span class="sxs-lookup"><span data-stu-id="3c643-123">For example, a WebHook POST request body from [GitHub](https://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

<span data-ttu-id="3c643-124">Per garantire che il WebHook provenga effettivamente dal mittente previsto, la richiesta POST viene protetta in qualche modo e quindi verificata dal destinatario.</span><span class="sxs-lookup"><span data-stu-id="3c643-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="3c643-125">Ad esempio, [GitHub WebHooks](https://developer.github.com/webhooks/) include un'intestazione HTTP *X-Hub-Signature* con un hash del corpo della richiesta che viene controllato dall'implementazione del ricevitore in modo da non doversi preoccupare.</span><span class="sxs-lookup"><span data-stu-id="3c643-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="3c643-126">Il flusso WebHook in genere è simile al seguente:The WebHook flow generally goes something like this:</span><span class="sxs-lookup"><span data-stu-id="3c643-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="3c643-127">Il mittente WebHook espone gli eventi che un client può sottoscrivere.</span><span class="sxs-lookup"><span data-stu-id="3c643-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="3c643-128">Gli eventi descrivono le modifiche osservabili al sistema, ad esempio che è stato inserito un nuovo elemento di dati, che un processo è stato completato o qualcos'altro.</span><span class="sxs-lookup"><span data-stu-id="3c643-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="3c643-129">Il ricevitore WebHook sottoscrive registrando un WebHook costituito da quattro elementi:</span><span class="sxs-lookup"><span data-stu-id="3c643-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="3c643-130">URI per il quale deve essere inviata la notifica dell'evento sotto forma di richiesta HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="3c643-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="3c643-131">Un set di filtri che descrivono gli eventi specifici per i quali il WebHook deve essere generato;</span><span class="sxs-lookup"><span data-stu-id="3c643-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="3c643-132">Chiave segreta utilizzata per firmare la richiesta HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="3c643-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="3c643-133">Dati aggiuntivi da includere nella richiesta HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="3c643-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="3c643-134">Può trattarsi, ad esempio, di proprietà o di proprietà di intestazione HTTP HTTP aggiuntive incluse nel corpo della richiesta HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="3c643-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="3c643-135">Una volta che si verifica un evento, vengono trovate le registrazioni WebHook corrispondenti e vengono inviate le richieste HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="3c643-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="3c643-136">In genere, la generazione delle richieste HTTP POST viene ritentata più volte se per qualche motivo il destinatario non risponde o la richiesta HTTP POST genera una risposta di errore.</span><span class="sxs-lookup"><span data-stu-id="3c643-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="3c643-137">Pipeline di elaborazione WebHooks</span><span class="sxs-lookup"><span data-stu-id="3c643-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="3c643-138">La pipeline di elaborazione di Microsoft ASP.NET WebHooks per WebHook in ingresso è simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="3c643-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![ASP.NET Pipeline di elaborazione WebHook](_static/WebHookReceivers.png)

<span data-ttu-id="3c643-140">I due concetti chiave qui sono *ricevitori* e *gestori*:</span><span class="sxs-lookup"><span data-stu-id="3c643-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="3c643-141">*I ricevitori* sono responsabili della gestione del particolare tipo di WebHook da un determinato mittente e dell'applicazione dei controlli di sicurezza per garantire che la richiesta WebHook provenga effettivamente dal mittente previsto.</span><span class="sxs-lookup"><span data-stu-id="3c643-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="3c643-142">*I gestori* sono in genere in cui il codice utente esegue l'elaborazione del WebHook specifico.</span><span class="sxs-lookup"><span data-stu-id="3c643-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="3c643-143">Nei nodi seguenti questi concetti sono descritti in modo più dettagliato.</span><span class="sxs-lookup"><span data-stu-id="3c643-143">In the following nodes these concepts are described in more details.</span></span>
