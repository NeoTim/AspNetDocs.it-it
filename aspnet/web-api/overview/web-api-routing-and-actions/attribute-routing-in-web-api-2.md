---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Routing degli attributi nellASP.NETAPI Web 2 Documenti Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: bb68fe8e6769619029a3fa039d6f0d6f3303afbe
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675391"
---
# <a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="09013-102">Routing degli attributi nellASP.NETAPI Web 2</span><span class="sxs-lookup"><span data-stu-id="09013-102">Attribute Routing in ASP.NET Web API 2</span></span>

<span data-ttu-id="09013-103">da parte di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="09013-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="09013-104">*Il routing* è il modo in cui l'API Web associa un URI a un'azione.</span><span class="sxs-lookup"><span data-stu-id="09013-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="09013-105">Web API 2 supporta un nuovo tipo di routing, denominato *routing degli attributi*.</span><span class="sxs-lookup"><span data-stu-id="09013-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="09013-106">Come implica il nome, il routing degli attributi utilizza gli attributi per definire le route.</span><span class="sxs-lookup"><span data-stu-id="09013-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="09013-107">Il routing degli attributi offre un maggiore controllo sugli URI nell'API Web.</span><span class="sxs-lookup"><span data-stu-id="09013-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="09013-108">Ad esempio, è possibile creare facilmente URI che descrivono gerarchie di risorse.</span><span class="sxs-lookup"><span data-stu-id="09013-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="09013-109">Lo stile precedente di routing, denominato routing basato su convenzioni, è ancora completamente supportato.</span><span class="sxs-lookup"><span data-stu-id="09013-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="09013-110">Infatti, è possibile combinare entrambe le tecniche nello stesso progetto.</span><span class="sxs-lookup"><span data-stu-id="09013-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="09013-111">In questo argomento viene illustrato come abilitare il routing degli attributi e vengono descritte le varie opzioni per il routing degli attributi.</span><span class="sxs-lookup"><span data-stu-id="09013-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="09013-112">Per un'esercitazione end-to-end che usa il routing degli attributi, vedere [Creare un'API REST con routing degli attributi in Web API 2.](create-a-rest-api-with-attribute-routing.md)</span><span class="sxs-lookup"><span data-stu-id="09013-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="09013-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="09013-113">Prerequisites</span></span>

<span data-ttu-id="09013-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Edizione Community, Professional o Enterprise</span><span class="sxs-lookup"><span data-stu-id="09013-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional, or Enterprise edition</span></span>

<span data-ttu-id="09013-115">In alternativa, usare NuGet Package Manager per installare i pacchetti necessari.</span><span class="sxs-lookup"><span data-stu-id="09013-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="09013-116">Dal menu **Strumenti** di Visual Studio, selezionare **NuGet Package Manager**, quindi selezionare Console gestione **pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="09013-116">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="09013-117">Immettere il comando seguente nella finestra della console di Gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="09013-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="09013-118">Perché il routing degli attributi?</span><span class="sxs-lookup"><span data-stu-id="09013-118">Why Attribute Routing?</span></span>

<span data-ttu-id="09013-119">La prima versione di Web API utilizzava il routing *basato su convenzioni.*</span><span class="sxs-lookup"><span data-stu-id="09013-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="09013-120">In questo tipo di routing, si definiscono uno o più modelli di route, che sono fondamentalmente stringhe con parametri.</span><span class="sxs-lookup"><span data-stu-id="09013-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="09013-121">Quando il framework riceve una richiesta, corrisponde all'URI rispetto al modello di route.</span><span class="sxs-lookup"><span data-stu-id="09013-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="09013-122">Per ulteriori informazioni sul routing basato su convenzioni, vedere [Routing in ASP.NETAPI Web](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="09013-122">For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="09013-123">Uno dei vantaggi del routing basato su convenzioni è che i modelli vengono definiti in un'unica posizione e le regole di routing vengono applicate in modo coerente tra tutti i controller.</span><span class="sxs-lookup"><span data-stu-id="09013-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="09013-124">Sfortunatamente, il routing basato su convenzioni rende difficile il supporto di determinati modelli URI comuni nelle API RESTful.</span><span class="sxs-lookup"><span data-stu-id="09013-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="09013-125">Ad esempio, le risorse spesso contengono risorse figlio: i clienti hanno ordini, i film hanno attori, i libri hanno autori e così via.</span><span class="sxs-lookup"><span data-stu-id="09013-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="09013-126">È naturale creare URI che riflettono queste relazioni:It's natural to create URIs that reflect these relations:</span><span class="sxs-lookup"><span data-stu-id="09013-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="09013-127">Questo tipo di URI è difficile da creare utilizzando il routing basato su convenzioni.</span><span class="sxs-lookup"><span data-stu-id="09013-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="09013-128">Anche se può essere fatto, i risultati non scalabili bene se si dispone di molti controller o tipi di risorse.</span><span class="sxs-lookup"><span data-stu-id="09013-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="09013-129">Con il routing degli attributi, è semplice definire una route per questo URI.</span><span class="sxs-lookup"><span data-stu-id="09013-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="09013-130">È sufficiente aggiungere un attributo all'azione del controller:You simply add an attribute to the controller action:</span><span class="sxs-lookup"><span data-stu-id="09013-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="09013-131">Ecco alcuni altri modelli che il routing degli attributi semplifica.</span><span class="sxs-lookup"><span data-stu-id="09013-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="09013-132">**Controllo delle versioni API**</span><span class="sxs-lookup"><span data-stu-id="09013-132">**API versioning**</span></span>

<span data-ttu-id="09013-133">In questo esempio, "/api/v1/products" verrebbe instradato a un controller diverso da "/api/v2/products".</span><span class="sxs-lookup"><span data-stu-id="09013-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`
`/api/v2/products`

<span data-ttu-id="09013-134">**Segmenti URI di overload**</span><span class="sxs-lookup"><span data-stu-id="09013-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="09013-135">In questo esempio, "1" è un numero di ordine, ma "in sospeso" esegue il mapping a una raccolta.</span><span class="sxs-lookup"><span data-stu-id="09013-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`
`/orders/pending`

<span data-ttu-id="09013-136">**Più tipi di parametri**</span><span class="sxs-lookup"><span data-stu-id="09013-136">**Multiple parameter types**</span></span>

<span data-ttu-id="09013-137">In questo esempio, "1" è un numero d'ordine, ma "2013/06/16" specifica una data.</span><span class="sxs-lookup"><span data-stu-id="09013-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="09013-138">Abilitazione del routing degli attributi</span><span class="sxs-lookup"><span data-stu-id="09013-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="09013-139">Per abilitare il routing degli attributi, chiamare **MapHttpAttributeRoutes** durante la configurazione.</span><span class="sxs-lookup"><span data-stu-id="09013-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="09013-140">Questo metodo di estensione è definito nella classe **System.Web.Http.HttpConfigurationExtensions.**</span><span class="sxs-lookup"><span data-stu-id="09013-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="09013-141">Il routing degli attributi può essere combinato con il routing [basato su convenzioni.](routing-in-aspnet-web-api.md)</span><span class="sxs-lookup"><span data-stu-id="09013-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="09013-142">Per definire route basate su convenzioni, chiamare il **MapHttpRoute** metodo.</span><span class="sxs-lookup"><span data-stu-id="09013-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="09013-143">Per ulteriori informazioni sulla configurazione dell'API Web, vedere [Configurazione di ASP.NET API Web 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="09013-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="09013-144">Nota: migrazione da API Web 1</span><span class="sxs-lookup"><span data-stu-id="09013-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="09013-145">Prima di Web API 2, i modelli di progetto API Web generano codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="09013-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="09013-146">Se il routing degli attributi è abilitato, questo codice genererà un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="09013-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="09013-147">Se si aggiorna un progetto API Web esistente per utilizzare il routing degli attributi, assicurarsi di aggiornare questo codice di configurazione al seguente:</span><span class="sxs-lookup"><span data-stu-id="09013-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="09013-148">Per ulteriori informazioni, vedere [Configurazione dell'API Web con ASP.NET hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="09013-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>

<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="09013-149">Aggiunta di attributi di percorso</span><span class="sxs-lookup"><span data-stu-id="09013-149">Adding Route Attributes</span></span>

<span data-ttu-id="09013-150">Di seguito è riportato un esempio di route definita utilizzando un attributo:Here is an example of a route defined using an attribute:</span><span class="sxs-lookup"><span data-stu-id="09013-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="09013-151">La &quot;stringa customers/'customerId'/orders&quot; è il modello URI per la route.</span><span class="sxs-lookup"><span data-stu-id="09013-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="09013-152">Api Web tenta di associare l'URI della richiesta al modello.</span><span class="sxs-lookup"><span data-stu-id="09013-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="09013-153">In questo esempio, "customers" e "orders" sono segmenti letterali e "'customerId'" è un parametro di variabile.</span><span class="sxs-lookup"><span data-stu-id="09013-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="09013-154">I seguenti URI corrisponderebbero a questo modello:</span><span class="sxs-lookup"><span data-stu-id="09013-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="09013-155">È possibile limitare la corrispondenza utilizzando [i vincoli](#constraints)descritti più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="09013-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="09013-156">Si noti che &quot;&quot; il parametro "customerId" nel modello di route corrisponde al nome del parametro *customerId* nel metodo.</span><span class="sxs-lookup"><span data-stu-id="09013-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="09013-157">Quando l'API Web richiama l'azione del controller, tenta di associare i parametri di route.</span><span class="sxs-lookup"><span data-stu-id="09013-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="09013-158">Ad esempio, se `http://example.com/customers/1/orders`l'URI è , l'API Web tenta di associare il valore "1" al parametro *customerId* nell'azione.</span><span class="sxs-lookup"><span data-stu-id="09013-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="09013-159">Un modello URI può avere diversi parametri:A URI template can have several parameters:</span><span class="sxs-lookup"><span data-stu-id="09013-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="09013-160">Tutti i metodi del controller che non dispongono di un attributo di route utilizzano il routing basato su convenzione.</span><span class="sxs-lookup"><span data-stu-id="09013-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="09013-161">In questo modo, è possibile combinare entrambi i tipi di ciclo nello stesso progetto.</span><span class="sxs-lookup"><span data-stu-id="09013-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="09013-162">Metodi HTTP</span><span class="sxs-lookup"><span data-stu-id="09013-162">HTTP Methods</span></span>

<span data-ttu-id="09013-163">API Web seleziona anche le azioni in base al metodo HTTP della richiesta (GET, POST e così via).</span><span class="sxs-lookup"><span data-stu-id="09013-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="09013-164">Per impostazione predefinita, l'API Web cerca una corrispondenza senza distinzione tra maiuscole e minuscole con l'inizio del nome del metodo del controller.</span><span class="sxs-lookup"><span data-stu-id="09013-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="09013-165">Ad esempio, un `PutCustomers` metodo del controller denominato corrisponde a una richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="09013-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="09013-166">È possibile eseguire l'override di questa convenzione decorando il metodo con i seguenti attributi:</span><span class="sxs-lookup"><span data-stu-id="09013-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="09013-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="09013-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="09013-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="09013-168">**[HttpGet]**</span></span>
- <span data-ttu-id="09013-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="09013-169">**[HttpHead]**</span></span>
- <span data-ttu-id="09013-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="09013-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="09013-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="09013-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="09013-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="09013-172">**[HttpPost]**</span></span>
- <span data-ttu-id="09013-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="09013-173">**[HttpPut]**</span></span>

<span data-ttu-id="09013-174">Nell'esempio seguente viene eseguito il mapping del metodo CreateBook alle richieste HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="09013-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="09013-175">Per tutti gli altri metodi HTTP, inclusi i metodi non standard, utilizzare l'attributo **AcceptVerbs,** che accetta un elenco di metodi HTTP.</span><span class="sxs-lookup"><span data-stu-id="09013-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="09013-176">Prefissi di route</span><span class="sxs-lookup"><span data-stu-id="09013-176">Route Prefixes</span></span>

<span data-ttu-id="09013-177">Spesso, le route in un controller iniziano tutte con lo stesso prefisso.</span><span class="sxs-lookup"><span data-stu-id="09013-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="09013-178">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="09013-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="09013-179">È possibile impostare un prefisso comune per un intero controller usando l'attributo **[RoutePrefix]:You** can set a common prefix for an entire controller by using the [RoutePrefix] attribute:</span><span class="sxs-lookup"><span data-stu-id="09013-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="09013-180">Utilizzare una tilde (-) sull'attributo del metodo per eseguire l'override del prefisso della route:</span><span class="sxs-lookup"><span data-stu-id="09013-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="09013-181">Il prefisso della route può includere parametri:The route prefix can include parameters:</span><span class="sxs-lookup"><span data-stu-id="09013-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="09013-182">Vincoli di route</span><span class="sxs-lookup"><span data-stu-id="09013-182">Route Constraints</span></span>

<span data-ttu-id="09013-183">I vincoli di route consentono di limitare la corrispondenza dei parametri nel modello di percorso.</span><span class="sxs-lookup"><span data-stu-id="09013-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="09013-184">La sintassi &quot;generale è&quot;"parameter:constraint" .</span><span class="sxs-lookup"><span data-stu-id="09013-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="09013-185">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="09013-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="09013-186">In questo caso, la prima &quot;route&quot; verrà selezionata solo se il segmento id dell'URI è un numero intero.</span><span class="sxs-lookup"><span data-stu-id="09013-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="09013-187">In caso contrario, verrà scelto il secondo percorso.</span><span class="sxs-lookup"><span data-stu-id="09013-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="09013-188">Nella tabella seguente sono elencati i vincoli supportati.</span><span class="sxs-lookup"><span data-stu-id="09013-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="09013-189">Vincolo</span><span class="sxs-lookup"><span data-stu-id="09013-189">Constraint</span></span> | <span data-ttu-id="09013-190">Descrizione</span><span class="sxs-lookup"><span data-stu-id="09013-190">Description</span></span> | <span data-ttu-id="09013-191">Esempio</span><span class="sxs-lookup"><span data-stu-id="09013-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="09013-192">alpha</span><span class="sxs-lookup"><span data-stu-id="09013-192">alpha</span></span> | <span data-ttu-id="09013-193">Corrisponde ai caratteri dell'alfabeto latino maiuscolo o minuscolo (a-z, A-z)</span><span class="sxs-lookup"><span data-stu-id="09013-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="09013-194">x:alfa</span><span class="sxs-lookup"><span data-stu-id="09013-194">{x:alpha}</span></span> |
| <span data-ttu-id="09013-195">bool</span><span class="sxs-lookup"><span data-stu-id="09013-195">bool</span></span> | <span data-ttu-id="09013-196">Corrisponde a un valore booleano.</span><span class="sxs-lookup"><span data-stu-id="09013-196">Matches a Boolean value.</span></span> | <span data-ttu-id="09013-197">X:bool</span><span class="sxs-lookup"><span data-stu-id="09013-197">{x:bool}</span></span> |
| <span data-ttu-id="09013-198">Datetime</span><span class="sxs-lookup"><span data-stu-id="09013-198">datetime</span></span> | <span data-ttu-id="09013-199">Corrisponde a un valore **DateTime.**</span><span class="sxs-lookup"><span data-stu-id="09013-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="09013-200">x:datetime</span><span class="sxs-lookup"><span data-stu-id="09013-200">{x:datetime}</span></span> |
| <span data-ttu-id="09013-201">decimal</span><span class="sxs-lookup"><span data-stu-id="09013-201">decimal</span></span> | <span data-ttu-id="09013-202">Corrisponde a un valore decimale.</span><span class="sxs-lookup"><span data-stu-id="09013-202">Matches a decimal value.</span></span> | <span data-ttu-id="09013-203">x:decimale</span><span class="sxs-lookup"><span data-stu-id="09013-203">{x:decimal}</span></span> |
| <span data-ttu-id="09013-204">double</span><span class="sxs-lookup"><span data-stu-id="09013-204">double</span></span> | <span data-ttu-id="09013-205">Corrisponde a un valore a virgola mobile a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="09013-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="09013-206">x:double</span><span class="sxs-lookup"><span data-stu-id="09013-206">{x:double}</span></span> |
| <span data-ttu-id="09013-207">float</span><span class="sxs-lookup"><span data-stu-id="09013-207">float</span></span> | <span data-ttu-id="09013-208">Corrisponde a un valore a virgola mobile a 32 bit.</span><span class="sxs-lookup"><span data-stu-id="09013-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="09013-209">x:float</span><span class="sxs-lookup"><span data-stu-id="09013-209">{x:float}</span></span> |
| <span data-ttu-id="09013-210">guid</span><span class="sxs-lookup"><span data-stu-id="09013-210">guid</span></span> | <span data-ttu-id="09013-211">Corrisponde a un valore GUID.</span><span class="sxs-lookup"><span data-stu-id="09013-211">Matches a GUID value.</span></span> | <span data-ttu-id="09013-212">x:guid</span><span class="sxs-lookup"><span data-stu-id="09013-212">{x:guid}</span></span> |
| <span data-ttu-id="09013-213">INT</span><span class="sxs-lookup"><span data-stu-id="09013-213">int</span></span> | <span data-ttu-id="09013-214">Corrisponde a un valore integer a 32 bit.</span><span class="sxs-lookup"><span data-stu-id="09013-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="09013-215">X:int</span><span class="sxs-lookup"><span data-stu-id="09013-215">{x:int}</span></span> |
| <span data-ttu-id="09013-216">length</span><span class="sxs-lookup"><span data-stu-id="09013-216">length</span></span> | <span data-ttu-id="09013-217">Corrisponde a una stringa con la lunghezza specificata o all'interno di un intervallo di lunghezze specificato.</span><span class="sxs-lookup"><span data-stu-id="09013-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="09013-218">x:lunghezza(6) x:length(1,20)</span><span class="sxs-lookup"><span data-stu-id="09013-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="09013-219">long</span><span class="sxs-lookup"><span data-stu-id="09013-219">long</span></span> | <span data-ttu-id="09013-220">Corrisponde a un valore integer a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="09013-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="09013-221">x:long</span><span class="sxs-lookup"><span data-stu-id="09013-221">{x:long}</span></span> |
| <span data-ttu-id="09013-222">max</span><span class="sxs-lookup"><span data-stu-id="09013-222">max</span></span> | <span data-ttu-id="09013-223">Corrisponde a un numero intero con un valore massimo.</span><span class="sxs-lookup"><span data-stu-id="09013-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="09013-224">x:max(10)</span><span class="sxs-lookup"><span data-stu-id="09013-224">{x:max(10)}</span></span> |
| <span data-ttu-id="09013-225">Maxlength</span><span class="sxs-lookup"><span data-stu-id="09013-225">maxlength</span></span> | <span data-ttu-id="09013-226">Corrisponde a una stringa con una lunghezza massima.</span><span class="sxs-lookup"><span data-stu-id="09013-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="09013-227">x:maxlength(10)</span><span class="sxs-lookup"><span data-stu-id="09013-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="09013-228">Min</span><span class="sxs-lookup"><span data-stu-id="09013-228">min</span></span> | <span data-ttu-id="09013-229">Corrisponde a un numero intero con un valore minimo.</span><span class="sxs-lookup"><span data-stu-id="09013-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="09013-230">x:min(10)</span><span class="sxs-lookup"><span data-stu-id="09013-230">{x:min(10)}</span></span> |
| <span data-ttu-id="09013-231">Minlength</span><span class="sxs-lookup"><span data-stu-id="09013-231">minlength</span></span> | <span data-ttu-id="09013-232">Corrisponde a una stringa con una lunghezza minima.</span><span class="sxs-lookup"><span data-stu-id="09013-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="09013-233">x:minlength(10)</span><span class="sxs-lookup"><span data-stu-id="09013-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="09013-234">range</span><span class="sxs-lookup"><span data-stu-id="09013-234">range</span></span> | <span data-ttu-id="09013-235">Corrisponde a un numero intero compreso in un intervallo di valori.</span><span class="sxs-lookup"><span data-stu-id="09013-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="09013-236">x:intervallo(10,50)</span><span class="sxs-lookup"><span data-stu-id="09013-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="09013-237">regex</span><span class="sxs-lookup"><span data-stu-id="09013-237">regex</span></span> | <span data-ttu-id="09013-238">Corrisponde a un'espressione regolare.</span><span class="sxs-lookup"><span data-stu-id="09013-238">Matches a regular expression.</span></span> | <span data-ttu-id="09013-239">{3}x:regex('d'd'd'd'){4}{3}</span><span class="sxs-lookup"><span data-stu-id="09013-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="09013-240">Si noti che alcuni &quot;vincoli, ad esempio min&quot;, accettano argomenti tra parentesi.</span><span class="sxs-lookup"><span data-stu-id="09013-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="09013-241">È possibile applicare più vincoli a un parametro, separati da due punti.</span><span class="sxs-lookup"><span data-stu-id="09013-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="09013-242">Vincoli di route personalizzati</span><span class="sxs-lookup"><span data-stu-id="09013-242">Custom Route Constraints</span></span>

<span data-ttu-id="09013-243">È possibile creare vincoli di route personalizzati implementando l'interfaccia **IHttpRouteConstraint.You** can create custom route constraints by implementing the IHttpRouteConstraint interface.</span><span class="sxs-lookup"><span data-stu-id="09013-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="09013-244">Ad esempio, il vincolo seguente limita un parametro a un valore intero diverso da zero.</span><span class="sxs-lookup"><span data-stu-id="09013-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="09013-245">Nel codice seguente viene illustrato come registrare il vincolo:The following code shows how to register the constraint:</span><span class="sxs-lookup"><span data-stu-id="09013-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="09013-246">Ora è possibile applicare il vincolo nei percorsi:</span><span class="sxs-lookup"><span data-stu-id="09013-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="09013-247">È inoltre possibile sostituire l'intera classe **DefaultInlineConstraintResolver** implementando l'interfaccia **IInlineConstraintResolver.**</span><span class="sxs-lookup"><span data-stu-id="09013-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="09013-248">In questo modo verranno sostituiti tutti i vincoli incorporati, a meno che l'implementazione di **IInlineConstraintResolver** li aggiunge in modo specifico.</span><span class="sxs-lookup"><span data-stu-id="09013-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="09013-249">Parametri URI facoltativi e valori predefinitiOptional URI Parameters and Default Values</span><span class="sxs-lookup"><span data-stu-id="09013-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="09013-250">È possibile rendere facoltativo un parametro URI aggiungendo un punto interrogativo al parametro route.</span><span class="sxs-lookup"><span data-stu-id="09013-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="09013-251">Se un parametro di route è facoltativo, è necessario definire un valore predefinito per il parametro del metodo.</span><span class="sxs-lookup"><span data-stu-id="09013-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="09013-252">In questo `/api/books/locale/1033` esempio `/api/books/locale` e restituire la stessa risorsa.</span><span class="sxs-lookup"><span data-stu-id="09013-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="09013-253">In alternativa, è possibile specificare un valore di default all'interno del modello di ciclo di lavorazione, come indicato di seguito:Alternatively, you can specify a default value inside the route template, as follows:</span><span class="sxs-lookup"><span data-stu-id="09013-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="09013-254">Questo è quasi lo stesso dell'esempio precedente, ma c'è una leggera differenza di comportamento quando viene applicato il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="09013-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="09013-255">Nel primo esempio di codice ( , lcid:int?, ), il valore predefinito di 1033 viene assegnato direttamente al parametro del metodo , pertanto il parametro avrà questo valore esatto.</span><span class="sxs-lookup"><span data-stu-id="09013-255">In the first example ("{lcid:int?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="09013-256">Nel secondo esempio ( , lcid:int , 1033 "), il valore predefinito di "1033" passa attraverso il processo di associazione del modello.</span><span class="sxs-lookup"><span data-stu-id="09013-256">In the second example ("{lcid:int=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="09013-257">Il model-binder predefinito convertirà "1033" nel valore numerico 1033.</span><span class="sxs-lookup"><span data-stu-id="09013-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="09013-258">Tuttavia, è possibile collegare un raccoglitore di modelli personalizzato, che potrebbe eseguire un'operazione diversa.</span><span class="sxs-lookup"><span data-stu-id="09013-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="09013-259">Nella maggior parte dei casi, a meno che non si disponga di raccoglitori di modelli personalizzati nella pipeline, i due formati saranno equivalenti.</span><span class="sxs-lookup"><span data-stu-id="09013-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="09013-260">Nomi delle route</span><span class="sxs-lookup"><span data-stu-id="09013-260">Route Names</span></span>

<span data-ttu-id="09013-261">Nell'API Web, ogni route ha un nome.</span><span class="sxs-lookup"><span data-stu-id="09013-261">In Web API, every route has a name.</span></span> <span data-ttu-id="09013-262">I nomi delle route sono utili per generare collegamenti, in modo che sia possibile includere un collegamento in una risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="09013-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="09013-263">Per specificare il nome della route, impostare la proprietà **Name** sull'attributo.</span><span class="sxs-lookup"><span data-stu-id="09013-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="09013-264">Nell'esempio seguente viene illustrato come impostare il nome della route e come utilizzare il nome della route durante la generazione di un collegamento.</span><span class="sxs-lookup"><span data-stu-id="09013-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="09013-265">Ordine ciclo di lavorazione</span><span class="sxs-lookup"><span data-stu-id="09013-265">Route Order</span></span>

<span data-ttu-id="09013-266">Quando il framework tenta di trovare una corrispondenza con un URI con una route, valuta le route in un ordine specifico.</span><span class="sxs-lookup"><span data-stu-id="09013-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="09013-267">Per specificare l'ordine, impostare la proprietà **Order** sull'attributo del ciclo di lavorazione.</span><span class="sxs-lookup"><span data-stu-id="09013-267">To specify the order, set the **Order** property on the route attribute.</span></span> <span data-ttu-id="09013-268">I valori inferiori vengono valutati per primi.</span><span class="sxs-lookup"><span data-stu-id="09013-268">Lower values are evaluated first.</span></span> <span data-ttu-id="09013-269">Il valore di ordine predefinito è zero.</span><span class="sxs-lookup"><span data-stu-id="09013-269">The default order value is zero.</span></span>

<span data-ttu-id="09013-270">Ecco come viene determinato l'ordine totale:</span><span class="sxs-lookup"><span data-stu-id="09013-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="09013-271">Confrontare la proprietà **Order** dell'attributo della route.</span><span class="sxs-lookup"><span data-stu-id="09013-271">Compare the **Order** property of the route attribute.</span></span>
2. <span data-ttu-id="09013-272">Esaminare ogni segmento URI nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="09013-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="09013-273">Per ogni segmento, ordinare come segue:</span><span class="sxs-lookup"><span data-stu-id="09013-273">For each segment, order as follows:</span></span>

    1. <span data-ttu-id="09013-274">Segmenti letterali.</span><span class="sxs-lookup"><span data-stu-id="09013-274">Literal segments.</span></span>
    2. <span data-ttu-id="09013-275">Parametri del ciclo di lavorazione con vincoli.</span><span class="sxs-lookup"><span data-stu-id="09013-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="09013-276">Parametri di percorso senza vincoli.</span><span class="sxs-lookup"><span data-stu-id="09013-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="09013-277">Segmenti di parametro con caratteri jolly con vincoli.</span><span class="sxs-lookup"><span data-stu-id="09013-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="09013-278">Segmenti di parametro con caratteri jolly senza vincoli.</span><span class="sxs-lookup"><span data-stu-id="09013-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="09013-279">Nel caso di un collegamento, le route vengono ordinate in base a un confronto di stringhe ordinale senza distinzione tra maiuscole e minuscole ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) del modello di route.</span><span class="sxs-lookup"><span data-stu-id="09013-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="09013-280">Ecco un esempio.</span><span class="sxs-lookup"><span data-stu-id="09013-280">Here is an example.</span></span> <span data-ttu-id="09013-281">Si supponga di definire il controller seguente:</span><span class="sxs-lookup"><span data-stu-id="09013-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="09013-282">Questi percorsi sono ordinati come segue.</span><span class="sxs-lookup"><span data-stu-id="09013-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="09013-283">ordini/dettagli</span><span class="sxs-lookup"><span data-stu-id="09013-283">orders/details</span></span>
2. <span data-ttu-id="09013-284">ordini/ id</span><span class="sxs-lookup"><span data-stu-id="09013-284">orders/{id}</span></span>
3. <span data-ttu-id="09013-285">ordini/ nomecliente</span><span class="sxs-lookup"><span data-stu-id="09013-285">orders/{customerName}</span></span>
4. <span data-ttu-id="09013-286">ordini/data\*di</span><span class="sxs-lookup"><span data-stu-id="09013-286">orders/{\*date}</span></span>
5. <span data-ttu-id="09013-287">ordini/in sospeso</span><span class="sxs-lookup"><span data-stu-id="09013-287">orders/pending</span></span>

<span data-ttu-id="09013-288">Si noti che "dettagli" è un segmento letterale e viene visualizzato prima di "id" ma "in sospeso" viene visualizzato per ultimo perché il **Order** proprietà è 1.</span><span class="sxs-lookup"><span data-stu-id="09013-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **Order** property is 1.</span></span> <span data-ttu-id="09013-289">In questo esempio si presuppone che non siano presenti clienti denominati "dettagli" o "in sospeso".</span><span class="sxs-lookup"><span data-stu-id="09013-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="09013-290">In generale, cercare di evitare percorsi ambigui.</span><span class="sxs-lookup"><span data-stu-id="09013-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="09013-291">In questo esempio, un `GetByCustomer` modello di route migliore per è "customers/'customerName'" )</span><span class="sxs-lookup"><span data-stu-id="09013-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
