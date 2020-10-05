---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Supporto delle opzioni di query OData in API Web ASP.NET 2-ASP.NET 4. x
author: MikeWasson
description: Panoramica con esempi di codice mostra le opzioni di query OData di supporto in API Web ASP.NET 2 per ASP.NET 4. x.
ms.author: riande
ms.date: 02/04/2013
ms.custom: seoapril2019
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 96820fab7ac89885058962f44ded86cb0184ee97
ms.sourcegitcommit: 4ed0b65ae32d9f35e42ee6296b877747e063df4d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/05/2020
ms.locfileid: "86188625"
---
# <a name="supporting-odata-query-options-in-aspnet-web-api-2"></a><span data-ttu-id="91b3d-103">Supporto delle opzioni di query OData in API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="91b3d-103">Supporting OData Query Options in ASP.NET Web API 2</span></span>

<span data-ttu-id="91b3d-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="91b3d-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="91b3d-105">Questa panoramica con esempi di codice illustra le opzioni di query OData di supporto in API Web ASP.NET 2 per ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="91b3d-105">This overview with code examples demonstrates the supporting OData Query Options in ASP.NET Web API 2 for ASP.NET 4.x.</span></span> 

<span data-ttu-id="91b3d-106">OData definisce i parametri che possono essere usati per modificare una query OData.</span><span class="sxs-lookup"><span data-stu-id="91b3d-106">OData defines parameters that can be used to modify an OData query.</span></span> <span data-ttu-id="91b3d-107">Il client invia questi parametri nella stringa di query dell'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="91b3d-107">The client sends these parameters in the query string of the request URI.</span></span> <span data-ttu-id="91b3d-108">Per ordinare i risultati, ad esempio, un client usa il parametro $orderby:</span><span class="sxs-lookup"><span data-stu-id="91b3d-108">For example, to sort the results, a client uses the $orderby parameter:</span></span>

`http://localhost/Products?$orderby=Name`

<span data-ttu-id="91b3d-109">La specifica OData chiama queste *Opzioni di query*dei parametri.</span><span class="sxs-lookup"><span data-stu-id="91b3d-109">The OData specification calls these parameters *query options*.</span></span> <span data-ttu-id="91b3d-110">È possibile abilitare le opzioni di query OData per qualsiasi controller API Web nel progetto &#8212; non è necessario che il controller sia un endpoint OData.</span><span class="sxs-lookup"><span data-stu-id="91b3d-110">You can enable OData query options for any Web API controller in your project &#8212; the controller does not need to be an OData endpoint.</span></span> <span data-ttu-id="91b3d-111">Si tratta di un modo pratico per aggiungere funzionalità come il filtro e l'ordinamento a qualsiasi applicazione API Web.</span><span class="sxs-lookup"><span data-stu-id="91b3d-111">This gives you a convenient way to add features such as filtering and sorting to any Web API application.</span></span>

<span data-ttu-id="91b3d-112">Prima di abilitare le opzioni di query, vedere l'argomento [Guida alla sicurezza di OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="91b3d-112">Before enabling query options, please read the topic [OData Security Guidance](odata-security-guidance.md).</span></span>

- [<span data-ttu-id="91b3d-113">Abilitazione di opzioni di query OData</span><span class="sxs-lookup"><span data-stu-id="91b3d-113">Enabling OData Query Options</span></span>](#enable)
- [<span data-ttu-id="91b3d-114">Query di esempio</span><span class="sxs-lookup"><span data-stu-id="91b3d-114">Example Queries</span></span>](#examples)
- [<span data-ttu-id="91b3d-115">Paging basato su server</span><span class="sxs-lookup"><span data-stu-id="91b3d-115">Server-Driven Paging</span></span>](#server-paging)
- [<span data-ttu-id="91b3d-116">Limitazione delle opzioni di query</span><span class="sxs-lookup"><span data-stu-id="91b3d-116">Limiting the Query Options</span></span>](#limiting_query_options)
- [<span data-ttu-id="91b3d-117">Richiamo diretto delle opzioni query</span><span class="sxs-lookup"><span data-stu-id="91b3d-117">Invoking Query Options Directly</span></span>](#ODataQueryOptions)
- [<span data-ttu-id="91b3d-118">Convalida query</span><span class="sxs-lookup"><span data-stu-id="91b3d-118">Query Validation</span></span>](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a><span data-ttu-id="91b3d-119">Abilitazione di opzioni di query OData</span><span class="sxs-lookup"><span data-stu-id="91b3d-119">Enabling OData Query Options</span></span>

<span data-ttu-id="91b3d-120">API Web supporta le seguenti opzioni di query OData:</span><span class="sxs-lookup"><span data-stu-id="91b3d-120">Web API supports the following OData query options:</span></span>

| <span data-ttu-id="91b3d-121">Opzione</span><span class="sxs-lookup"><span data-stu-id="91b3d-121">Option</span></span> | <span data-ttu-id="91b3d-122">Descrizione</span><span class="sxs-lookup"><span data-stu-id="91b3d-122">Description</span></span> |
| --- | --- |
| <span data-ttu-id="91b3d-123">$expand</span><span class="sxs-lookup"><span data-stu-id="91b3d-123">$expand</span></span> | <span data-ttu-id="91b3d-124">Espande le entità correlate inline.</span><span class="sxs-lookup"><span data-stu-id="91b3d-124">Expands related entities inline.</span></span> |
| <span data-ttu-id="91b3d-125">$filter</span><span class="sxs-lookup"><span data-stu-id="91b3d-125">$filter</span></span> | <span data-ttu-id="91b3d-126">Filtra i risultati in base a una condizione booleana.</span><span class="sxs-lookup"><span data-stu-id="91b3d-126">Filters the results, based on a Boolean condition.</span></span> |
| <span data-ttu-id="91b3d-127">$inlinecount</span><span class="sxs-lookup"><span data-stu-id="91b3d-127">$inlinecount</span></span> | <span data-ttu-id="91b3d-128">Indica al server di includere il conteggio totale delle entità corrispondenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="91b3d-128">Tells the server to include the total count of matching entities in the response.</span></span> <span data-ttu-id="91b3d-129">(Utile per il paging lato server).</span><span class="sxs-lookup"><span data-stu-id="91b3d-129">(Useful for server-side paging.)</span></span> |
| <span data-ttu-id="91b3d-130">$orderby</span><span class="sxs-lookup"><span data-stu-id="91b3d-130">$orderby</span></span> | <span data-ttu-id="91b3d-131">Ordina i risultati.</span><span class="sxs-lookup"><span data-stu-id="91b3d-131">Sorts the results.</span></span> |
| <span data-ttu-id="91b3d-132">$select</span><span class="sxs-lookup"><span data-stu-id="91b3d-132">$select</span></span> | <span data-ttu-id="91b3d-133">Consente di selezionare le proprietà da includere nella risposta.</span><span class="sxs-lookup"><span data-stu-id="91b3d-133">Selects which properties to include in the response.</span></span> |
| <span data-ttu-id="91b3d-134">$skip</span><span class="sxs-lookup"><span data-stu-id="91b3d-134">$skip</span></span> | <span data-ttu-id="91b3d-135">Ignora i primi n risultati.</span><span class="sxs-lookup"><span data-stu-id="91b3d-135">Skips the first n results.</span></span> |
| <span data-ttu-id="91b3d-136">$top</span><span class="sxs-lookup"><span data-stu-id="91b3d-136">$top</span></span> | <span data-ttu-id="91b3d-137">Restituisce solo i primi n risultati.</span><span class="sxs-lookup"><span data-stu-id="91b3d-137">Returns only the first n the results.</span></span> |

<span data-ttu-id="91b3d-138">Per usare le opzioni di query OData, è necessario abilitarle in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="91b3d-138">To use OData query options, you must enable them explicitly.</span></span> <span data-ttu-id="91b3d-139">È possibile abilitarli globalmente per l'intera applicazione o abilitarli per controller specifici o azioni specifiche.</span><span class="sxs-lookup"><span data-stu-id="91b3d-139">You can enable them globally for the entire application, or enable them for specific controllers or specific actions.</span></span>

<span data-ttu-id="91b3d-140">Per abilitare le opzioni di query OData globalmente, chiamare **EnableQuerySupport** sulla classe **HttpConfiguration** all'avvio:</span><span class="sxs-lookup"><span data-stu-id="91b3d-140">To enable OData query options globally, call **EnableQuerySupport** on the **HttpConfiguration** class at startup:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

<span data-ttu-id="91b3d-141">Il metodo **EnableQuerySupport** Abilita le opzioni di query a livello globale per qualsiasi azione del controller che restituisce un tipo **IQueryable** .</span><span class="sxs-lookup"><span data-stu-id="91b3d-141">The **EnableQuerySupport** method enables query options globally for any controller action that returns an **IQueryable** type.</span></span> <span data-ttu-id="91b3d-142">Se non si desidera che le opzioni di query siano abilitate per l'intera applicazione, è possibile abilitarle per azioni specifiche del controller aggiungendo l'attributo **[Queryable]** al metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="91b3d-142">If you don't want query options enabled for the entire application, you can enable them for specific controller actions by adding the **[Queryable]** attribute to the action method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a><span data-ttu-id="91b3d-143">Query di esempio</span><span class="sxs-lookup"><span data-stu-id="91b3d-143">Example Queries</span></span>

<span data-ttu-id="91b3d-144">Questa sezione illustra i tipi di query possibili usando le opzioni di query OData.</span><span class="sxs-lookup"><span data-stu-id="91b3d-144">This section shows the types of queries that are possible using the OData query options.</span></span> <span data-ttu-id="91b3d-145">Per dettagli specifici sulle opzioni di query, vedere la documentazione di OData in [www.OData.org](http://www.odata.org/).</span><span class="sxs-lookup"><span data-stu-id="91b3d-145">For specific details about the query options, refer to the OData documentation at [www.odata.org](http://www.odata.org/).</span></span>

<span data-ttu-id="91b3d-146">Per informazioni su $expand e $select, vedere [utilizzo di $Select, $Expand e $value in API Web ASP.NET OData](using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="91b3d-146">For information about $expand and $select, see [Using $select, $expand, and $value in ASP.NET Web API OData](using-select-expand-and-value.md).</span></span>

<span data-ttu-id="91b3d-147">**Paging basato su client**</span><span class="sxs-lookup"><span data-stu-id="91b3d-147">**Client-Driven Paging**</span></span>

<span data-ttu-id="91b3d-148">Per i set di entità di grandi dimensioni, il client potrebbe voler limitare il numero di risultati.</span><span class="sxs-lookup"><span data-stu-id="91b3d-148">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="91b3d-149">Ad esempio, un client potrebbe visualizzare 10 voci alla volta, con collegamenti "Avanti" per ottenere la pagina di risultati successiva.</span><span class="sxs-lookup"><span data-stu-id="91b3d-149">For example, a client might show 10 entries at a time, with "next" links to get the next page of results.</span></span> <span data-ttu-id="91b3d-150">A tale scopo, il client utilizza le opzioni $top e $skip.</span><span class="sxs-lookup"><span data-stu-id="91b3d-150">To do this, the client uses the $top and $skip options.</span></span>

`http://localhost/Products?$top=10&$skip=20`

<span data-ttu-id="91b3d-151">L'opzione $top indica il numero massimo di voci da restituire e l'opzione $skip indica il numero di voci da ignorare.</span><span class="sxs-lookup"><span data-stu-id="91b3d-151">The $top option gives the maximum number of entries to return, and the $skip option gives the number of entries to skip.</span></span> <span data-ttu-id="91b3d-152">Nell'esempio precedente vengono recuperate le voci da 21 a 30.</span><span class="sxs-lookup"><span data-stu-id="91b3d-152">The previous example fetches entries 21 through 30.</span></span>

<span data-ttu-id="91b3d-153">**Filtro**</span><span class="sxs-lookup"><span data-stu-id="91b3d-153">**Filtering**</span></span>

<span data-ttu-id="91b3d-154">L'opzione $filter consente a un client di filtrare i risultati applicando un'espressione booleana.</span><span class="sxs-lookup"><span data-stu-id="91b3d-154">The $filter option lets a client filter the results by applying a Boolean expression.</span></span> <span data-ttu-id="91b3d-155">Le espressioni di filtro sono piuttosto potenti. sono inclusi operatori logici e aritmetici, funzioni stringa e funzioni di data.</span><span class="sxs-lookup"><span data-stu-id="91b3d-155">The filter expressions are quite powerful; they include logical and arithmetic operators, string functions, and date functions.</span></span>

| <span data-ttu-id="91b3d-156">Restituisce tutti i prodotti con categoria uguale a "Toys".</span><span class="sxs-lookup"><span data-stu-id="91b3d-156">Return all products with category equal to "Toys".</span></span> | <span data-ttu-id="91b3d-157">`http://localhost/Products?$filter=Category` EQ ' Toys '</span><span class="sxs-lookup"><span data-stu-id="91b3d-157">`http://localhost/Products?$filter=Category` eq 'Toys'</span></span> |
| --- | --- |
| <span data-ttu-id="91b3d-158">Restituisce tutti i prodotti con prezzo inferiore a 10.</span><span class="sxs-lookup"><span data-stu-id="91b3d-158">Return all products with price less than 10.</span></span> | <span data-ttu-id="91b3d-159">`http://localhost/Products?$filter=Price` lt 10</span><span class="sxs-lookup"><span data-stu-id="91b3d-159">`http://localhost/Products?$filter=Price` lt 10</span></span> |
| <span data-ttu-id="91b3d-160">Operatori logici: restituisce tutti i prodotti in cui Price >= 5 e Price <= 15.</span><span class="sxs-lookup"><span data-stu-id="91b3d-160">Logical operators: Return all products where price >= 5 and price <= 15.</span></span> | <span data-ttu-id="91b3d-161">`http://localhost/Products?$filter=Price` GE 5 e Price le 15</span><span class="sxs-lookup"><span data-stu-id="91b3d-161">`http://localhost/Products?$filter=Price` ge 5 and Price le 15</span></span> |
| <span data-ttu-id="91b3d-162">Funzioni stringa: restituisce tutti i prodotti con "ZZ" nel nome.</span><span class="sxs-lookup"><span data-stu-id="91b3d-162">String functions: Return all products with "zz" in the name.</span></span> | `http://localhost/Products?$filter=substringof('zz',Name)` |
| <span data-ttu-id="91b3d-163">Funzioni di data: restituiscono tutti i prodotti con rilasciato dopo 2005.</span><span class="sxs-lookup"><span data-stu-id="91b3d-163">Date functions: Return all products with ReleaseDate after 2005.</span></span> | <span data-ttu-id="91b3d-164">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span><span class="sxs-lookup"><span data-stu-id="91b3d-164">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span></span> |

<span data-ttu-id="91b3d-165">**Ordinamento**</span><span class="sxs-lookup"><span data-stu-id="91b3d-165">**Sorting**</span></span>

<span data-ttu-id="91b3d-166">Per ordinare i risultati, usare il filtro $orderby.</span><span class="sxs-lookup"><span data-stu-id="91b3d-166">To sort the results, use the $orderby filter.</span></span>

| <span data-ttu-id="91b3d-167">Ordina per prezzo.</span><span class="sxs-lookup"><span data-stu-id="91b3d-167">Sort by price.</span></span> | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| <span data-ttu-id="91b3d-168">Ordina per prezzo in ordine decrescente (dal più alto al più basso).</span><span class="sxs-lookup"><span data-stu-id="91b3d-168">Sort by price in descending order (highest to lowest).</span></span> | `http://localhost/Products?$orderby=Price desc` |
| <span data-ttu-id="91b3d-169">Ordina per categoria, quindi Ordina per prezzo in ordine decrescente all'interno delle categorie.</span><span class="sxs-lookup"><span data-stu-id="91b3d-169">Sort by category, then sort by price in descending order within categories.</span></span> | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a><span data-ttu-id="91b3d-170">Paging basato su server</span><span class="sxs-lookup"><span data-stu-id="91b3d-170">Server-Driven Paging</span></span>

<span data-ttu-id="91b3d-171">Se il database contiene milioni di record, non è opportuno inviarli tutti in un unico payload.</span><span class="sxs-lookup"><span data-stu-id="91b3d-171">If your database contains millions of records, you don't want to send them all in one payload.</span></span> <span data-ttu-id="91b3d-172">Per evitare questo problema, il server può limitare il numero di voci che invia in una singola risposta.</span><span class="sxs-lookup"><span data-stu-id="91b3d-172">To prevent this, the server can limit the number of entries that it sends in a single response.</span></span> <span data-ttu-id="91b3d-173">Per abilitare il paging del server, impostare la proprietà **pageSize** nell'attributo **Queryable** .</span><span class="sxs-lookup"><span data-stu-id="91b3d-173">To enable server paging, set the **PageSize** property in the **Queryable** attribute.</span></span> <span data-ttu-id="91b3d-174">Il valore è il numero massimo di voci da restituire.</span><span class="sxs-lookup"><span data-stu-id="91b3d-174">The value is the maximum number of entries to return.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

<span data-ttu-id="91b3d-175">Se il controller restituisce il formato OData, il corpo della risposta conterrà un collegamento alla pagina di dati successiva:</span><span class="sxs-lookup"><span data-stu-id="91b3d-175">If your controller returns OData format, the response body will contain a link to the next page of data:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

<span data-ttu-id="91b3d-176">Il client può usare questo collegamento per recuperare la pagina successiva.</span><span class="sxs-lookup"><span data-stu-id="91b3d-176">The client can use this link to fetch the next page.</span></span> <span data-ttu-id="91b3d-177">Per conoscere il numero totale di voci nel set di risultati, il client può impostare l'opzione query $inlinecount con il valore "AllPages".</span><span class="sxs-lookup"><span data-stu-id="91b3d-177">To learn the total number of entries in the result set, the client can set the $inlinecount query option with the value "allpages".</span></span>

`http://localhost/Products?$inlinecount=allpages`

<span data-ttu-id="91b3d-178">Il valore "AllPages" indica al server di includere il conteggio totale nella risposta:</span><span class="sxs-lookup"><span data-stu-id="91b3d-178">The value "allpages" tells the server to include the total count in the response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> <span data-ttu-id="91b3d-179">I collegamenti a pagina successiva e il conteggio inline richiedono entrambi il formato OData.</span><span class="sxs-lookup"><span data-stu-id="91b3d-179">Next-page links and inline count both require OData format.</span></span> <span data-ttu-id="91b3d-180">Il motivo è che OData definisce campi speciali nel corpo della risposta per conservare il collegamento e il conteggio.</span><span class="sxs-lookup"><span data-stu-id="91b3d-180">The reason is that OData defines special fields in the response body to hold the link and count.</span></span>

<span data-ttu-id="91b3d-181">Per i formati non OData, è ancora possibile supportare i collegamenti della pagina successiva e il conteggio inline, eseguendo il wrapping dei risultati della query in un oggetto \*\*PageResult &lt; T &gt; \*\* .</span><span class="sxs-lookup"><span data-stu-id="91b3d-181">For non-OData formats, it is still possible to support next-page links and inline count, by wrapping the query results in a **PageResult&lt;T&gt;** object.</span></span> <span data-ttu-id="91b3d-182">Tuttavia, richiede un po' di codice.</span><span class="sxs-lookup"><span data-stu-id="91b3d-182">However, it requires a bit more code.</span></span> <span data-ttu-id="91b3d-183">Esempio:</span><span class="sxs-lookup"><span data-stu-id="91b3d-183">Here is an example:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

<span data-ttu-id="91b3d-184">Di seguito è riportato un esempio di risposta JSON:</span><span class="sxs-lookup"><span data-stu-id="91b3d-184">Here is an example JSON response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a><span data-ttu-id="91b3d-185">Limitazione delle opzioni di query</span><span class="sxs-lookup"><span data-stu-id="91b3d-185">Limiting the Query Options</span></span>

<span data-ttu-id="91b3d-186">Le opzioni di query offrono al client un elevato controllo sulla query eseguita sul server.</span><span class="sxs-lookup"><span data-stu-id="91b3d-186">The query options give the client a lot of control over the query that is run on the server.</span></span> <span data-ttu-id="91b3d-187">In alcuni casi, potrebbe essere necessario limitare le opzioni disponibili per motivi di sicurezza o di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="91b3d-187">In some cases, you might want to limit the available options for security or performance reasons.</span></span> <span data-ttu-id="91b3d-188">L'attributo **[Queryable]** include alcune proprietà predefinite per questo oggetto.</span><span class="sxs-lookup"><span data-stu-id="91b3d-188">The **[Queryable]** attribute has some built in properties for this.</span></span> <span data-ttu-id="91b3d-189">Di seguito sono riportati alcuni esempi.</span><span class="sxs-lookup"><span data-stu-id="91b3d-189">Here are some examples.</span></span>

<span data-ttu-id="91b3d-190">Consenti solo $skip e $top, per supportare il paging e nient'altro:</span><span class="sxs-lookup"><span data-stu-id="91b3d-190">Allow only $skip and $top, to support paging and nothing else:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

<span data-ttu-id="91b3d-191">Consentire l'ordinamento solo in base a determinate proprietà, per impedire l'ordinamento delle proprietà non indicizzate nel database:</span><span class="sxs-lookup"><span data-stu-id="91b3d-191">Allow ordering only by certain properties, to prevent sorting on properties that are not indexed in the database:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

<span data-ttu-id="91b3d-192">Consenti la funzione logica "EQ", ma non altre funzioni logiche:</span><span class="sxs-lookup"><span data-stu-id="91b3d-192">Allow the "eq" logical function but no other logical functions:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

<span data-ttu-id="91b3d-193">Non consentire operatori aritmetici:</span><span class="sxs-lookup"><span data-stu-id="91b3d-193">Do not allow any arithmetic operators:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

<span data-ttu-id="91b3d-194">È possibile limitare le opzioni a livello globale costruendo un'istanza di **QueryableAttribute** e passandola alla funzione **EnableQuerySupport** :</span><span class="sxs-lookup"><span data-stu-id="91b3d-194">You can restrict options globally by constructing a **QueryableAttribute** instance and passing it to the **EnableQuerySupport** function:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a><span data-ttu-id="91b3d-195">Richiamo diretto delle opzioni query</span><span class="sxs-lookup"><span data-stu-id="91b3d-195">Invoking Query Options Directly</span></span>

<span data-ttu-id="91b3d-196">Anziché usare l'attributo **[Queryable]** , è possibile richiamare le opzioni di query direttamente nel controller.</span><span class="sxs-lookup"><span data-stu-id="91b3d-196">Instead of using the **[Queryable]** attribute, you can invoke the query options directly in your controller.</span></span> <span data-ttu-id="91b3d-197">A tale scopo, aggiungere un parametro **ODataQueryOptions** al metodo controller.</span><span class="sxs-lookup"><span data-stu-id="91b3d-197">To do so, add an **ODataQueryOptions** parameter to the controller method.</span></span> <span data-ttu-id="91b3d-198">In questo caso, non è necessario l'attributo **[Queryable]** .</span><span class="sxs-lookup"><span data-stu-id="91b3d-198">In this case, you don't need the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

<span data-ttu-id="91b3d-199">L'API Web popola **ODataQueryOptions** dalla stringa di query URI.</span><span class="sxs-lookup"><span data-stu-id="91b3d-199">Web API populates the **ODataQueryOptions** from the URI query string.</span></span> <span data-ttu-id="91b3d-200">Per applicare la query, passare un oggetto **IQueryable** al metodo **ApplyTo** .</span><span class="sxs-lookup"><span data-stu-id="91b3d-200">To apply the query, pass an **IQueryable** to the **ApplyTo** method.</span></span> <span data-ttu-id="91b3d-201">Il metodo restituisce un altro oggetto **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="91b3d-201">The method returns another **IQueryable**.</span></span>

<span data-ttu-id="91b3d-202">Per gli scenari avanzati, se non si dispone di un provider di query **IQueryable** , è possibile esaminare il **ODataQueryOptions** e tradurre le opzioni di query in un altro formato.</span><span class="sxs-lookup"><span data-stu-id="91b3d-202">For advanced scenarios, if you do not have an **IQueryable** query provider, you can examine the **ODataQueryOptions** and translate the query options into another form.</span></span> <span data-ttu-id="91b3d-203">Ad esempio, vedere il post di Blog di Nigro per la [conversione di query OData in HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), che include anche un [esempio](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).</span><span class="sxs-lookup"><span data-stu-id="91b3d-203">(For example, see RaghuRam Nadiminti's blog post [Translating OData queries to HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), which also includes a [sample](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span></span>

<a id="query-validation"></a>
## <a name="query-validation"></a><span data-ttu-id="91b3d-204">Convalida query</span><span class="sxs-lookup"><span data-stu-id="91b3d-204">Query Validation</span></span>

<span data-ttu-id="91b3d-205">L'attributo **[Queryable]** convalida la query prima di eseguirla.</span><span class="sxs-lookup"><span data-stu-id="91b3d-205">The **[Queryable]** attribute validates the query before executing it.</span></span> <span data-ttu-id="91b3d-206">Il passaggio di convalida viene eseguito nel metodo **QueryableAttribute. ValidateQuery** .</span><span class="sxs-lookup"><span data-stu-id="91b3d-206">The validation step is performed in the **QueryableAttribute.ValidateQuery** method.</span></span> <span data-ttu-id="91b3d-207">È anche possibile personalizzare il processo di convalida.</span><span class="sxs-lookup"><span data-stu-id="91b3d-207">You can also customize the validation process.</span></span>

<span data-ttu-id="91b3d-208">Vedere anche [informazioni aggiuntive sulla sicurezza di OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="91b3d-208">Also see [OData Security Guidance](odata-security-guidance.md).</span></span>

<span data-ttu-id="91b3d-209">In primo luogo, eseguire l'override di una delle classi Validator definite nello spazio dei nomi **Web. http. OData. query. Validators** .</span><span class="sxs-lookup"><span data-stu-id="91b3d-209">First, override one of the validator classes that is defined in the **Web.Http.OData.Query.Validators** namespace.</span></span> <span data-ttu-id="91b3d-210">Ad esempio, la classe Validator seguente disabilita l'opzione "DESC" per l'opzione $orderby.</span><span class="sxs-lookup"><span data-stu-id="91b3d-210">For example, the following validator class disables the 'desc' option for the $orderby option.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

<span data-ttu-id="91b3d-211">Sottoclassare l'attributo **[Queryable]** per eseguire l'override del metodo **ValidateQuery** .</span><span class="sxs-lookup"><span data-stu-id="91b3d-211">Subclass the **[Queryable]** attribute to override the **ValidateQuery** method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

<span data-ttu-id="91b3d-212">Impostare quindi l'attributo personalizzato globalmente o per controller:</span><span class="sxs-lookup"><span data-stu-id="91b3d-212">Then set your custom attribute either globally or per-controller:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

<span data-ttu-id="91b3d-213">Se si usa direttamente **ODataQueryOptions** , impostare il validator sulle opzioni:</span><span class="sxs-lookup"><span data-stu-id="91b3d-213">If you are using **ODataQueryOptions** directly, set the validator on the options:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
