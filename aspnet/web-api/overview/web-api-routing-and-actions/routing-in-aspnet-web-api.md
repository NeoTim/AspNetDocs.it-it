---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routing nellASP.NETAPI Web Documenti Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 85862c094cc54365267b1f21e68d235a15519cda
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676132"
---
# <a name="routing-in-aspnet-web-api"></a>Routing in ASP.NET Web API (in inglese)

da parte di [Mike Wasson](https://github.com/MikeWasson)

In questo articolo viene descritto come ASP.NETAPI Web instrada le richieste HTTP ai controller.

> [!NOTE]
> Se si ha familiarità con ASP.NET MVC, il routing dell'API Web è molto simile al routing MVC. La differenza principale è che l'API Web utilizza il verbo HTTP, non il percorso URI, per selezionare l'azione. È inoltre possibile utilizzare il routing di tipo MVC nell'API Web.You can also use MVC-style routing in Web API. In questo articolo non presuppone alcuna conoscenza di ASP.NET MVC.

## <a name="routing-tables"></a>Tabelle di instradamento

In ASP.NET API Web, un *controller* è una classe che gestisce le richieste HTTP. I metodi pubblici del controller sono chiamati metodi di *azione* o semplicemente *azioni*. Quando il framework api Web riceve una richiesta, instrada la richiesta a un'azione.

Per determinare l'azione da richiamare, il framework utilizza una tabella di *routing.* Il modello di progetto di Visual Studio per l'API Web crea una route predefinita:The Visual Studio project template for Web API creates a default route:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Questa route è definita nel file *WebApiConfig.cs,* che viene inserito nella directory *\_App Start:*

![](routing-in-aspnet-web-api/_static/image1.png)

Per ulteriori informazioni `WebApiConfig` sulla classe, vedere [Configurazione di ASP.NET API Web](../advanced/configuring-aspnet-web-api.md).

Se si ospita l'API Web self-host, `HttpSelfHostConfiguration` è necessario impostare la tabella di routing direttamente sull'oggetto. Per ulteriori informazioni, vedere [Self-Host a Web API](../older-versions/self-host-a-web-api.md).

Ogni voce della tabella di routing contiene un modello di *ciclo di lavorazione.* Il modello di route &quot;predefinito per l'API Web&quot;è api/ In questo &quot;modello, api è un segmento di percorso letterale e le variabili segnaposto sono i tipi di&quot; percorso , e .

Quando il framework API Web riceve una richiesta HTTP, tenta di confrontare l'URI con uno dei modelli di route nella tabella di routing. Se nessuna route corrisponde, il client riceve un errore 404. Ad esempio, i seguenti URI corrispondono alla route predefinita:

- /api/contatti
- /api/contatti/1
- /api/products/gizmo1

Tuttavia, l'URI seguente non corrisponde, &quot;&quot; perché manca il segmento api:

- /contatti/1

> [!NOTE]
> Il motivo per l'utilizzo di "api" nel percorso consiste nell'evitare conflitti con ASP.NET il routing MVC. In questo modo, &quot;è&quot; possibile fare in &quot;modo che&quot; /contacts eseri a un controller MVC e /api/contacts a un controller API Web. Naturalmente, se non si è come questa convenzione, è possibile modificare la tabella di route predefinita.

Una volta trovata una route corrispondente, l'API Web seleziona il controller e l'azione:

- Per trovare il controller, &quot;&quot; l'API Web aggiunge Controller al valore della variabile *di controller.*
- Per trovare l'azione, Web API esamina il verbo HTTP e quindi cerca un'azione il cui nome inizia con il nome del verbo HTTP. Ad esempio, con una richiesta GET, l'API &quot;&quot;Web cerca &quot;un'azione preceduta da Get , ad esempio GetContact&quot; o &quot;GetAllContacts&quot;. Questa convenzione si applica solo ai verbi GET, POST, PUT, DELETE, HEAD, OPTIONS e PATCH. È possibile abilitare altri verbi HTTP utilizzando gli attributi nel controller. Ne vedremo un esempio più avanti.
- Nel modello di route vengono mappate altre variabili segnaposto, ad esempio *"id",* ai parametri di azione.

Ecco un esempio. Si supponga di definire il controller seguente:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Di seguito sono riportate alcune possibili richieste HTTP, insieme all'azione che viene richiamata per ognuna:Here are some possible HTTP requests, along with the action that gets invoked for each:

| Verbo HTTP | Percorso URI | Azione | Parametro |
| --- | --- | --- | --- |
| GET | api/prodotti | Prodotti GetAll | *(nessuno)* |
| GET | api/prodotti/4 | GetProductById | 4 |
| DELETE | api/prodotti/4 | DeleteProduct (Prodotto) | 4 |
| POST | api/prodotti | *(nessuna corrispondenza)* |  |

Si noti che il segmento *"id"* dell'URI, se presente, è mappato al parametro *id* dell'azione. In questo esempio, il controller definisce due metodi GET, uno con un parametro *id* e uno senza parametri.

Si noti inoltre che la richiesta POST avrà &quot;esito negativo, perché il controller non definisce un Post... &quot; metodo.

## <a name="routing-variations"></a>Variazioni di instradamento

Nella sezione precedente è stato descritto il meccanismo di routing di base per ASP.NETAPI Web. In questa sezione vengono descritte alcune varianti.

### <a name="http-verbs"></a>Verbi HTTP

Anziché utilizzare la convenzione di denominazione per i verbi HTTP, è possibile specificare in modo esplicito il verbo HTTP per un'azione decorando il metodo di azione con uno degli attributi seguenti:

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

Nell'esempio seguente, `FindProduct` il metodo viene mappato alle richieste GET:In the following example, the method is mapped to GET requests:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Per consentire più verbi HTTP per un'azione o per consentire verbi HTTP diversi da GET, `[AcceptVerbs]` PUT, POST, DELETE, HEAD, OPTIONS e PATCH, utilizzare l'attributo, che accetta un elenco di verbi HTTP.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Instradamento per nome azione

Con il modello di routing predefinito, l'API Web utilizza il verbo HTTP per selezionare l'azione. Tuttavia, è anche possibile creare una route in cui il nome dell'azione è incluso nell'URI:However, you can also create a route where the action name is included in the URI:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

In questo modello di route, il parametro *"action"* denomina il metodo di azione nel controller. Con questo stile di routing, utilizzare gli attributi per specificare i verbi HTTP consentiti. Si supponga, ad esempio, che il controller disponga del metodo seguente:For example, suppose your controller has the following method:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

In questo caso, una richiesta GET per "api/products/details/1" verrebbe mappata al `Details` metodo. Questo stile di routing è simile a ASP.NET MVC e può essere appropriato per un'API di tipo RPC.

È possibile sostituire il nome `[ActionName]` dell'azione utilizzando l'attributo . Nell'esempio seguente vengono eseguito il &quot;mapping a api/products/thumbnail/*id*. Uno supporta GET e l'altro supporta POST:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Non azioni

Per impedire che un metodo venga richiamato `[NonAction]` come azione, utilizzare l'attributo . Ciò segnala al framework che il metodo non è un'azione, anche se altrimenti corrisponderebbe alle regole di routing.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Altre informazioni

In questo argomento è stata fornita una visualizzazione generale del routing. Per ulteriori dettagli, vedere [Routing and Action Selection](routing-and-action-selection.md), che descrive esattamente come il framework associa un URI a una route, seleziona un controller e quindi seleziona l'azione da richiamare.
