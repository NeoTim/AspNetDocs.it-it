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
# <a name="aspnet-webhooks-overview"></a>Panoramica di ASP.NET WebHooks

WebHooks è un modello HTTP leggero che fornisce un semplice modello di pub/sub per il cablaggio insieme di API Web e servizi SaaS. Quando si verifica un evento in un servizio, viene inviata una notifica sotto forma di richiesta HTTP POST ai sottoscrittori registrati. La richiesta POST contiene informazioni sull'evento che consentono al destinatario di agire di conseguenza.

A causa della loro semplicità, i WebHook sono già esposti da un gran numero di servizi tra cui [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)e molti altri. Ad esempio, un WebHook può indicare che un file è stato modificato in [Dropbox](http://dropbox.com/)o che è stato eseguito il commit di una modifica del codice in GitHub oppure che è stato avviato un pagamento in [PayPal](http://www.paypal.com/)oppure che è stata creata una carta in [Trello](http://www.trello.com/). Le possibilità sono infinite!

Microsoft ASP.NET WebHooks semplifica l'invio e la ricezione di WebHook nell'ambito dell'applicazione ASP.NET:

* Sul lato ricevente, fornisce un modello comune per la ricezione e l'elaborazione di WebHook da qualsiasi numero di provider WebHook. Esce dalla scatola con il supporto per [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) e [.](https://www.zendesk.com/)

* Sul lato di invio fornisce il supporto per la gestione e l'archiviazione delle sottoscrizioni, nonché per l'invio di notifiche di eventi al set di sottoscrittori corretto. Ciò consente di definire il proprio set di eventi che i sottoscrittori possono sottoscrivere e notificare loro quando si verificano le cose.

Le due parti possono essere utilizzate insieme o tra di parte a seconda dello scenario. Se hai solo bisogno di ricevere WebHook da altri servizi, puoi utilizzare solo la parte del ricevitore; se si desidera esporre solo WebHook per altri utenti da utilizzare, è possibile farlo.

Il codice è destinato a ASP.NET API Web 2 e ASP.NET MVC 5 ed è disponibile come [OSS in GitHub](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Panoramica di WebHooks

WebHooks è un modello che significa che varia come viene utilizzato da un servizio all'altro, ma l'idea di base è la stessa. È possibile considerare WebHooks come un semplice modello pub/sub in cui un utente può sottoscrivere eventi che si verificano altrove. Le notifiche degli eventi vengono propagate come richieste HTTP POST contenenti informazioni sull'evento stesso.

In genere la richiesta HTTP POST contiene un oggetto JSON o dati del modulo HTML determinati dal mittente Di WebHook, incluse informazioni sull'evento che causa il trigger di WebHook. Ad esempio, un corpo della richiesta WEBHook POST da GitHub si presenta come risultato di un nuovo problema aperto in un determinato repository:For example, a WebHook POST request body from [GitHub](https://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:

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

Per garantire che il WebHook provenga effettivamente dal mittente previsto, la richiesta POST viene protetta in qualche modo e quindi verificata dal destinatario. Ad esempio, [GitHub WebHooks](https://developer.github.com/webhooks/) include un'intestazione HTTP *X-Hub-Signature* con un hash del corpo della richiesta che viene controllato dall'implementazione del ricevitore in modo da non doversi preoccupare.

Il flusso WebHook in genere è simile al seguente:The WebHook flow generally goes something like this:

* Il mittente WebHook espone gli eventi che un client può sottoscrivere. Gli eventi descrivono le modifiche osservabili al sistema, ad esempio che è stato inserito un nuovo elemento di dati, che un processo è stato completato o qualcos'altro.

* Il ricevitore WebHook sottoscrive registrando un WebHook costituito da quattro elementi:

     1. URI per il quale deve essere inviata la notifica dell'evento sotto forma di richiesta HTTP POST.

     2. Un set di filtri che descrivono gli eventi specifici per i quali il WebHook deve essere generato;

     3. Chiave segreta utilizzata per firmare la richiesta HTTP POST.

     4. Dati aggiuntivi da includere nella richiesta HTTP POST. Può trattarsi, ad esempio, di proprietà o di proprietà di intestazione HTTP HTTP aggiuntive incluse nel corpo della richiesta HTTP POST.

* Una volta che si verifica un evento, vengono trovate le registrazioni WebHook corrispondenti e vengono inviate le richieste HTTP POST. In genere, la generazione delle richieste HTTP POST viene ritentata più volte se per qualche motivo il destinatario non risponde o la richiesta HTTP POST genera una risposta di errore.

## <a name="webhooks-processing-pipeline"></a>Pipeline di elaborazione WebHooks

La pipeline di elaborazione di Microsoft ASP.NET WebHooks per WebHook in ingresso è simile alla seguente:

![ASP.NET Pipeline di elaborazione WebHook](_static/WebHookReceivers.png)

I due concetti chiave qui sono *ricevitori* e *gestori*:

* *I ricevitori* sono responsabili della gestione del particolare tipo di WebHook da un determinato mittente e dell'applicazione dei controlli di sicurezza per garantire che la richiesta WebHook provenga effettivamente dal mittente previsto.

* *I gestori* sono in genere in cui il codice utente esegue l'elaborazione del WebHook specifico.

Nei nodi seguenti questi concetti sono descritti in modo più dettagliato.
