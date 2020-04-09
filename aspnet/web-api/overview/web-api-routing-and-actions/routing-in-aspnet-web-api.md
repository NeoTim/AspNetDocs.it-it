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
# <a name="routing-in-aspnet-web-api"></a><span data-ttu-id="5def4-102">Routing in ASP.NET Web API (in inglese)</span><span class="sxs-lookup"><span data-stu-id="5def4-102">Routing in ASP.NET Web API</span></span>

<span data-ttu-id="5def4-103">da parte di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5def4-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="5def4-104">In questo articolo viene descritto come ASP.NETAPI Web instrada le richieste HTTP ai controller.</span><span class="sxs-lookup"><span data-stu-id="5def4-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="5def4-105">Se si ha familiarità con ASP.NET MVC, il routing dell'API Web è molto simile al routing MVC.</span><span class="sxs-lookup"><span data-stu-id="5def4-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="5def4-106">La differenza principale è che l'API Web utilizza il verbo HTTP, non il percorso URI, per selezionare l'azione.</span><span class="sxs-lookup"><span data-stu-id="5def4-106">The main difference is that Web API uses the HTTP verb, not the URI path, to select the action.</span></span> <span data-ttu-id="5def4-107">È inoltre possibile utilizzare il routing di tipo MVC nell'API Web.You can also use MVC-style routing in Web API.</span><span class="sxs-lookup"><span data-stu-id="5def4-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="5def4-108">In questo articolo non presuppone alcuna conoscenza di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5def4-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>

## <a name="routing-tables"></a><span data-ttu-id="5def4-109">Tabelle di instradamento</span><span class="sxs-lookup"><span data-stu-id="5def4-109">Routing Tables</span></span>

<span data-ttu-id="5def4-110">In ASP.NET API Web, un *controller* è una classe che gestisce le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="5def4-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="5def4-111">I metodi pubblici del controller sono chiamati metodi di *azione* o semplicemente *azioni*.</span><span class="sxs-lookup"><span data-stu-id="5def4-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="5def4-112">Quando il framework api Web riceve una richiesta, instrada la richiesta a un'azione.</span><span class="sxs-lookup"><span data-stu-id="5def4-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="5def4-113">Per determinare l'azione da richiamare, il framework utilizza una tabella di *routing.*</span><span class="sxs-lookup"><span data-stu-id="5def4-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="5def4-114">Il modello di progetto di Visual Studio per l'API Web crea una route predefinita:The Visual Studio project template for Web API creates a default route:</span><span class="sxs-lookup"><span data-stu-id="5def4-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="5def4-115">Questa route è definita nel file *WebApiConfig.cs,* che viene inserito nella directory *\_App Start:*</span><span class="sxs-lookup"><span data-stu-id="5def4-115">This route is defined in the *WebApiConfig.cs* file, which is placed in the *App\_Start* directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="5def4-116">Per ulteriori informazioni `WebApiConfig` sulla classe, vedere [Configurazione di ASP.NET API Web](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="5def4-116">For more information about the `WebApiConfig` class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="5def4-117">Se si ospita l'API Web self-host, `HttpSelfHostConfiguration` è necessario impostare la tabella di routing direttamente sull'oggetto.</span><span class="sxs-lookup"><span data-stu-id="5def4-117">If you self-host Web API, you must set the routing table directly on the `HttpSelfHostConfiguration` object.</span></span> <span data-ttu-id="5def4-118">Per ulteriori informazioni, vedere [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="5def4-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="5def4-119">Ogni voce della tabella di routing contiene un modello di *ciclo di lavorazione.*</span><span class="sxs-lookup"><span data-stu-id="5def4-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="5def4-120">Il modello di route &quot;predefinito per l'API Web&quot;è api/</span><span class="sxs-lookup"><span data-stu-id="5def4-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="5def4-121">In questo &quot;modello, api è un segmento di percorso letterale e le variabili segnaposto sono i tipi di&quot; percorso , e .</span><span class="sxs-lookup"><span data-stu-id="5def4-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="5def4-122">Quando il framework API Web riceve una richiesta HTTP, tenta di confrontare l'URI con uno dei modelli di route nella tabella di routing.</span><span class="sxs-lookup"><span data-stu-id="5def4-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="5def4-123">Se nessuna route corrisponde, il client riceve un errore 404.</span><span class="sxs-lookup"><span data-stu-id="5def4-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="5def4-124">Ad esempio, i seguenti URI corrispondono alla route predefinita:</span><span class="sxs-lookup"><span data-stu-id="5def4-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="5def4-125">/api/contatti</span><span class="sxs-lookup"><span data-stu-id="5def4-125">/api/contacts</span></span>
- <span data-ttu-id="5def4-126">/api/contatti/1</span><span class="sxs-lookup"><span data-stu-id="5def4-126">/api/contacts/1</span></span>
- <span data-ttu-id="5def4-127">/api/products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="5def4-127">/api/products/gizmo1</span></span>

<span data-ttu-id="5def4-128">Tuttavia, l'URI seguente non corrisponde, &quot;&quot; perché manca il segmento api:</span><span class="sxs-lookup"><span data-stu-id="5def4-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="5def4-129">/contatti/1</span><span class="sxs-lookup"><span data-stu-id="5def4-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="5def4-130">Il motivo per l'utilizzo di "api" nel percorso consiste nell'evitare conflitti con ASP.NET il routing MVC.</span><span class="sxs-lookup"><span data-stu-id="5def4-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="5def4-131">In questo modo, &quot;è&quot; possibile fare in &quot;modo che&quot; /contacts eseri a un controller MVC e /api/contacts a un controller API Web.</span><span class="sxs-lookup"><span data-stu-id="5def4-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="5def4-132">Naturalmente, se non si è come questa convenzione, è possibile modificare la tabella di route predefinita.</span><span class="sxs-lookup"><span data-stu-id="5def4-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="5def4-133">Una volta trovata una route corrispondente, l'API Web seleziona il controller e l'azione:</span><span class="sxs-lookup"><span data-stu-id="5def4-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="5def4-134">Per trovare il controller, &quot;&quot; l'API Web aggiunge Controller al valore della variabile *di controller.*</span><span class="sxs-lookup"><span data-stu-id="5def4-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="5def4-135">Per trovare l'azione, Web API esamina il verbo HTTP e quindi cerca un'azione il cui nome inizia con il nome del verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="5def4-135">To find the action, Web API looks at the HTTP verb, and then looks for an action whose name begins with that HTTP verb name.</span></span> <span data-ttu-id="5def4-136">Ad esempio, con una richiesta GET, l'API &quot;&quot;Web cerca &quot;un'azione preceduta da Get , ad esempio GetContact&quot; o &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="5def4-136">For example, with a GET request, Web API looks for an action prefixed with &quot;Get&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="5def4-137">Questa convenzione si applica solo ai verbi GET, POST, PUT, DELETE, HEAD, OPTIONS e PATCH.</span><span class="sxs-lookup"><span data-stu-id="5def4-137">This convention applies only to GET, POST, PUT, DELETE, HEAD, OPTIONS, and PATCH verbs.</span></span> <span data-ttu-id="5def4-138">È possibile abilitare altri verbi HTTP utilizzando gli attributi nel controller.</span><span class="sxs-lookup"><span data-stu-id="5def4-138">You can enable other HTTP verbs by using attributes on your controller.</span></span> <span data-ttu-id="5def4-139">Ne vedremo un esempio più avanti.</span><span class="sxs-lookup"><span data-stu-id="5def4-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="5def4-140">Nel modello di route vengono mappate altre variabili segnaposto, ad esempio *"id",* ai parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="5def4-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="5def4-141">Ecco un esempio.</span><span class="sxs-lookup"><span data-stu-id="5def4-141">Let's look at an example.</span></span> <span data-ttu-id="5def4-142">Si supponga di definire il controller seguente:</span><span class="sxs-lookup"><span data-stu-id="5def4-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="5def4-143">Di seguito sono riportate alcune possibili richieste HTTP, insieme all'azione che viene richiamata per ognuna:Here are some possible HTTP requests, along with the action that gets invoked for each:</span><span class="sxs-lookup"><span data-stu-id="5def4-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="5def4-144">Verbo HTTP</span><span class="sxs-lookup"><span data-stu-id="5def4-144">HTTP Verb</span></span> | <span data-ttu-id="5def4-145">Percorso URI</span><span class="sxs-lookup"><span data-stu-id="5def4-145">URI Path</span></span> | <span data-ttu-id="5def4-146">Azione</span><span class="sxs-lookup"><span data-stu-id="5def4-146">Action</span></span> | <span data-ttu-id="5def4-147">Parametro</span><span class="sxs-lookup"><span data-stu-id="5def4-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5def4-148">GET</span><span class="sxs-lookup"><span data-stu-id="5def4-148">GET</span></span> | <span data-ttu-id="5def4-149">api/prodotti</span><span class="sxs-lookup"><span data-stu-id="5def4-149">api/products</span></span> | <span data-ttu-id="5def4-150">Prodotti GetAll</span><span class="sxs-lookup"><span data-stu-id="5def4-150">GetAllProducts</span></span> | <span data-ttu-id="5def4-151">*(nessuno)*</span><span class="sxs-lookup"><span data-stu-id="5def4-151">*(none)*</span></span> |
| <span data-ttu-id="5def4-152">GET</span><span class="sxs-lookup"><span data-stu-id="5def4-152">GET</span></span> | <span data-ttu-id="5def4-153">api/prodotti/4</span><span class="sxs-lookup"><span data-stu-id="5def4-153">api/products/4</span></span> | <span data-ttu-id="5def4-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="5def4-154">GetProductById</span></span> | <span data-ttu-id="5def4-155">4</span><span class="sxs-lookup"><span data-stu-id="5def4-155">4</span></span> |
| <span data-ttu-id="5def4-156">DELETE</span><span class="sxs-lookup"><span data-stu-id="5def4-156">DELETE</span></span> | <span data-ttu-id="5def4-157">api/prodotti/4</span><span class="sxs-lookup"><span data-stu-id="5def4-157">api/products/4</span></span> | <span data-ttu-id="5def4-158">DeleteProduct (Prodotto)</span><span class="sxs-lookup"><span data-stu-id="5def4-158">DeleteProduct</span></span> | <span data-ttu-id="5def4-159">4</span><span class="sxs-lookup"><span data-stu-id="5def4-159">4</span></span> |
| <span data-ttu-id="5def4-160">POST</span><span class="sxs-lookup"><span data-stu-id="5def4-160">POST</span></span> | <span data-ttu-id="5def4-161">api/prodotti</span><span class="sxs-lookup"><span data-stu-id="5def4-161">api/products</span></span> | <span data-ttu-id="5def4-162">*(nessuna corrispondenza)*</span><span class="sxs-lookup"><span data-stu-id="5def4-162">*(no match)*</span></span> |  |

<span data-ttu-id="5def4-163">Si noti che il segmento *"id"* dell'URI, se presente, è mappato al parametro *id* dell'azione.</span><span class="sxs-lookup"><span data-stu-id="5def4-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="5def4-164">In questo esempio, il controller definisce due metodi GET, uno con un parametro *id* e uno senza parametri.</span><span class="sxs-lookup"><span data-stu-id="5def4-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="5def4-165">Si noti inoltre che la richiesta POST avrà &quot;esito negativo, perché il controller non definisce un Post... &quot; metodo.</span><span class="sxs-lookup"><span data-stu-id="5def4-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="5def4-166">Variazioni di instradamento</span><span class="sxs-lookup"><span data-stu-id="5def4-166">Routing Variations</span></span>

<span data-ttu-id="5def4-167">Nella sezione precedente è stato descritto il meccanismo di routing di base per ASP.NETAPI Web.</span><span class="sxs-lookup"><span data-stu-id="5def4-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="5def4-168">In questa sezione vengono descritte alcune varianti.</span><span class="sxs-lookup"><span data-stu-id="5def4-168">This section describes some variations.</span></span>

### <a name="http-verbs"></a><span data-ttu-id="5def4-169">Verbi HTTP</span><span class="sxs-lookup"><span data-stu-id="5def4-169">HTTP verbs</span></span>

<span data-ttu-id="5def4-170">Anziché utilizzare la convenzione di denominazione per i verbi HTTP, è possibile specificare in modo esplicito il verbo HTTP per un'azione decorando il metodo di azione con uno degli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="5def4-170">Instead of using the naming convention for HTTP verbs, you can explicitly specify the HTTP verb for an action by decorating the action method with one of the following attributes:</span></span>

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

<span data-ttu-id="5def4-171">Nell'esempio seguente, `FindProduct` il metodo viene mappato alle richieste GET:In the following example, the method is mapped to GET requests:</span><span class="sxs-lookup"><span data-stu-id="5def4-171">In the following example, the `FindProduct` method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="5def4-172">Per consentire più verbi HTTP per un'azione o per consentire verbi HTTP diversi da GET, `[AcceptVerbs]` PUT, POST, DELETE, HEAD, OPTIONS e PATCH, utilizzare l'attributo, che accetta un elenco di verbi HTTP.</span><span class="sxs-lookup"><span data-stu-id="5def4-172">To allow multiple HTTP verbs for an action, or to allow HTTP verbs other than GET, PUT, POST, DELETE, HEAD, OPTIONS, and PATCH, use the `[AcceptVerbs]` attribute, which takes a list of HTTP verbs.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="5def4-173">Instradamento per nome azione</span><span class="sxs-lookup"><span data-stu-id="5def4-173">Routing by Action Name</span></span>

<span data-ttu-id="5def4-174">Con il modello di routing predefinito, l'API Web utilizza il verbo HTTP per selezionare l'azione.</span><span class="sxs-lookup"><span data-stu-id="5def4-174">With the default routing template, Web API uses the HTTP verb to select the action.</span></span> <span data-ttu-id="5def4-175">Tuttavia, è anche possibile creare una route in cui il nome dell'azione è incluso nell'URI:However, you can also create a route where the action name is included in the URI:</span><span class="sxs-lookup"><span data-stu-id="5def4-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="5def4-176">In questo modello di route, il parametro *"action"* denomina il metodo di azione nel controller.</span><span class="sxs-lookup"><span data-stu-id="5def4-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="5def4-177">Con questo stile di routing, utilizzare gli attributi per specificare i verbi HTTP consentiti.</span><span class="sxs-lookup"><span data-stu-id="5def4-177">With this style of routing, use attributes to specify the allowed HTTP verbs.</span></span> <span data-ttu-id="5def4-178">Si supponga, ad esempio, che il controller disponga del metodo seguente:For example, suppose your controller has the following method:</span><span class="sxs-lookup"><span data-stu-id="5def4-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="5def4-179">In questo caso, una richiesta GET per "api/products/details/1" verrebbe mappata al `Details` metodo.</span><span class="sxs-lookup"><span data-stu-id="5def4-179">In this case, a GET request for "api/products/details/1" would map to the `Details` method.</span></span> <span data-ttu-id="5def4-180">Questo stile di routing è simile a ASP.NET MVC e può essere appropriato per un'API di tipo RPC.</span><span class="sxs-lookup"><span data-stu-id="5def4-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="5def4-181">È possibile sostituire il nome `[ActionName]` dell'azione utilizzando l'attributo .</span><span class="sxs-lookup"><span data-stu-id="5def4-181">You can override the action name by using the `[ActionName]` attribute.</span></span> <span data-ttu-id="5def4-182">Nell'esempio seguente vengono eseguito il &quot;mapping a api/products/thumbnail/*id*. Uno supporta GET e l'altro supporta POST:</span><span class="sxs-lookup"><span data-stu-id="5def4-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="5def4-183">Non azioni</span><span class="sxs-lookup"><span data-stu-id="5def4-183">Non-Actions</span></span>

<span data-ttu-id="5def4-184">Per impedire che un metodo venga richiamato `[NonAction]` come azione, utilizzare l'attributo .</span><span class="sxs-lookup"><span data-stu-id="5def4-184">To prevent a method from getting invoked as an action, use the `[NonAction]` attribute.</span></span> <span data-ttu-id="5def4-185">Ciò segnala al framework che il metodo non è un'azione, anche se altrimenti corrisponderebbe alle regole di routing.</span><span class="sxs-lookup"><span data-stu-id="5def4-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="5def4-186">Altre informazioni</span><span class="sxs-lookup"><span data-stu-id="5def4-186">Further Reading</span></span>

<span data-ttu-id="5def4-187">In questo argomento è stata fornita una visualizzazione generale del routing.</span><span class="sxs-lookup"><span data-stu-id="5def4-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="5def4-188">Per ulteriori dettagli, vedere [Routing and Action Selection](routing-and-action-selection.md), che descrive esattamente come il framework associa un URI a una route, seleziona un controller e quindi seleziona l'azione da richiamare.</span><span class="sxs-lookup"><span data-stu-id="5def4-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
