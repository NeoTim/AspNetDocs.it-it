---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: La chiamata a un servizio OData da un Client .NET (c#) | Microsoft Docs
author: MikeWasson
description: Questa esercitazione illustra come chiamare un servizio OData da un'applicazione client in c#. Versioni del software utilizzate nell'esercitazione Visual Studio 2013 (compatibile con Visual S...
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: d35c0057f5c29e399e45d0a58467de7f106d9994
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59389973"
---
# <a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="0843e-104">Chiamata di un servizio OData da un client .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="0843e-104">Calling an OData Service From a .NET Client (C#)</span></span>

<span data-ttu-id="0843e-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0843e-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="0843e-106">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="0843e-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="0843e-107">Questa esercitazione illustra come chiamare un servizio OData da un'applicazione client in c#.</span><span class="sxs-lookup"><span data-stu-id="0843e-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0843e-108">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="0843e-108">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="0843e-109">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (funziona con Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="0843e-109">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="0843e-110">Libreria client WCF Data Services</span><span class="sxs-lookup"><span data-stu-id="0843e-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="0843e-111">API Web 2.</span><span class="sxs-lookup"><span data-stu-id="0843e-111">Web API 2.</span></span> <span data-ttu-id="0843e-112">(Il servizio OData di esempio viene compilato usando l'API Web 2, ma l'applicazione client non dipende da API Web).</span><span class="sxs-lookup"><span data-stu-id="0843e-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>


<span data-ttu-id="0843e-113">In questa esercitazione verrà illustrato come creare un'applicazione client che chiama un servizio OData.</span><span class="sxs-lookup"><span data-stu-id="0843e-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="0843e-114">Il servizio OData espone le entità seguenti:</span><span class="sxs-lookup"><span data-stu-id="0843e-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="0843e-115">Gli articoli seguenti descrivono come implementare il servizio OData nell'API Web.</span><span class="sxs-lookup"><span data-stu-id="0843e-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="0843e-116">(Non è necessario leggere in modo da tenere presente, tuttavia questa esercitazione.)</span><span class="sxs-lookup"><span data-stu-id="0843e-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="0843e-117">Creazione di un OData Endpoint nell'API Web 2</span><span class="sxs-lookup"><span data-stu-id="0843e-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="0843e-118">Relazioni di entità OData nell'API Web 2</span><span class="sxs-lookup"><span data-stu-id="0843e-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="0843e-119">Azioni OData nell'API Web 2</span><span class="sxs-lookup"><span data-stu-id="0843e-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="0843e-120">Generare il Proxy del servizio</span><span class="sxs-lookup"><span data-stu-id="0843e-120">Generate the Service Proxy</span></span>

<span data-ttu-id="0843e-121">Il primo passaggio è generare un proxy del servizio.</span><span class="sxs-lookup"><span data-stu-id="0843e-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="0843e-122">Il proxy del servizio è una classe .NET che definisce i metodi per l'accesso al servizio OData.</span><span class="sxs-lookup"><span data-stu-id="0843e-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="0843e-123">Il proxy converte le chiamate di metodo nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="0843e-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="0843e-124">Iniziare aprendo il progetto di servizio OData in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0843e-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="0843e-125">Premere CTRL+F5 per eseguire il servizio localmente in IIS Express.</span><span class="sxs-lookup"><span data-stu-id="0843e-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="0843e-126">Annotare l'indirizzo locale, incluso il numero di porta assegnato dal Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0843e-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="0843e-127">È questo indirizzo sarà necessario quando si crea il proxy.</span><span class="sxs-lookup"><span data-stu-id="0843e-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="0843e-128">Successivamente, aprire un'altra istanza di Visual Studio e creare un progetto di applicazione console.</span><span class="sxs-lookup"><span data-stu-id="0843e-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="0843e-129">L'applicazione console sarà l'applicazione client OData.</span><span class="sxs-lookup"><span data-stu-id="0843e-129">The console application will be our OData client application.</span></span> <span data-ttu-id="0843e-130">(È possibile anche aggiungere il progetto per la stessa soluzione del servizio.)</span><span class="sxs-lookup"><span data-stu-id="0843e-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="0843e-131">I passaggi rimanenti, vedere il progetto console.</span><span class="sxs-lookup"><span data-stu-id="0843e-131">The remaining steps refer the console project.</span></span>


<span data-ttu-id="0843e-132">In Esplora soluzioni fare doppio clic su **riferimenti** e selezionare **Aggiungi riferimento al servizio**.</span><span class="sxs-lookup"><span data-stu-id="0843e-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="0843e-133">Nel **Aggiungi riferimento al servizio** finestra di dialogo digitare l'indirizzo del servizio OData:</span><span class="sxs-lookup"><span data-stu-id="0843e-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="0843e-134">in cui *porta* è il numero di porta.</span><span class="sxs-lookup"><span data-stu-id="0843e-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="0843e-135">Per la **Namespace**, digitare "ProductService".</span><span class="sxs-lookup"><span data-stu-id="0843e-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="0843e-136">Questa opzione consente di definire lo spazio dei nomi della classe proxy.</span><span class="sxs-lookup"><span data-stu-id="0843e-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="0843e-137">Fare clic su **Vai**.</span><span class="sxs-lookup"><span data-stu-id="0843e-137">Click **Go**.</span></span> <span data-ttu-id="0843e-138">Visual Studio legge il documento di metadati OData per individuare le entità nel servizio.</span><span class="sxs-lookup"><span data-stu-id="0843e-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="0843e-139">Fare clic su **OK** per aggiungere la classe proxy al progetto.</span><span class="sxs-lookup"><span data-stu-id="0843e-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="0843e-140">Creare un'istanza della classe Proxy del servizio</span><span class="sxs-lookup"><span data-stu-id="0843e-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="0843e-141">All'interno di `Main` metodo, creare una nuova istanza della classe proxy, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="0843e-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="0843e-142">Anche in questo caso, usare il numero di porta effettivo in cui è in esecuzione il servizio.</span><span class="sxs-lookup"><span data-stu-id="0843e-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="0843e-143">Quando si distribuisce il servizio, si userà l'URI del servizio in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="0843e-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="0843e-144">Non è necessario aggiornare il proxy.</span><span class="sxs-lookup"><span data-stu-id="0843e-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="0843e-145">Il codice seguente aggiunge un gestore eventi che consente di stampare gli URI di richiesta alla finestra della console.</span><span class="sxs-lookup"><span data-stu-id="0843e-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="0843e-146">Questo passaggio non è obbligatorio, ma è interessante visualizzare gli URI per ogni query.</span><span class="sxs-lookup"><span data-stu-id="0843e-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="0843e-147">Eseguire query sul servizio</span><span class="sxs-lookup"><span data-stu-id="0843e-147">Query the Service</span></span>

<span data-ttu-id="0843e-148">Il codice seguente ottiene l'elenco dei prodotti dal servizio OData.</span><span class="sxs-lookup"><span data-stu-id="0843e-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="0843e-149">Si noti che non devi scrivere codice per inviare la richiesta HTTP o analizzare la risposta.</span><span class="sxs-lookup"><span data-stu-id="0843e-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="0843e-150">La classe proxy non comporta automaticamente quando si enumerano le `Container.Products` insieme nella **foreach** ciclo.</span><span class="sxs-lookup"><span data-stu-id="0843e-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="0843e-151">Quando si esegue l'applicazione, l'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="0843e-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="0843e-152">Per ottenere un'entità dall'ID, utilizzare un `where` clausola.</span><span class="sxs-lookup"><span data-stu-id="0843e-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="0843e-153">Per il resto di questo argomento, non verrà visualizzata l'intera `Main` funzioni, solo il codice necessario per chiamare il servizio.</span><span class="sxs-lookup"><span data-stu-id="0843e-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="0843e-154">Applicare le opzioni di Query</span><span class="sxs-lookup"><span data-stu-id="0843e-154">Apply Query Options</span></span>

<span data-ttu-id="0843e-155">OData definisce [opzioni query](../supporting-odata-query-options.md) che può essere utilizzato da filtro, ordinamento, paging dei dati e così via.</span><span class="sxs-lookup"><span data-stu-id="0843e-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="0843e-156">Nel proxy del servizio, è possibile applicare queste opzioni tramite diverse espressioni LINQ.</span><span class="sxs-lookup"><span data-stu-id="0843e-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="0843e-157">In questa sezione verrà illustrato negli esempi brevi.</span><span class="sxs-lookup"><span data-stu-id="0843e-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="0843e-158">Per altre informazioni, vedere l'argomento [considerazioni su LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) su MSDN.</span><span class="sxs-lookup"><span data-stu-id="0843e-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="0843e-159">Filtro ($filter)</span><span class="sxs-lookup"><span data-stu-id="0843e-159">Filtering ($filter)</span></span>

<span data-ttu-id="0843e-160">Per filtrare, usare un `where` clausola.</span><span class="sxs-lookup"><span data-stu-id="0843e-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="0843e-161">L'esempio seguente Filtra per categoria di prodotto.</span><span class="sxs-lookup"><span data-stu-id="0843e-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="0843e-162">Questo codice corrisponde alla query OData seguenti.</span><span class="sxs-lookup"><span data-stu-id="0843e-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="0843e-163">Si noti che il proxy consente di convertire le `where` clausola OData `$filter` espressione.</span><span class="sxs-lookup"><span data-stu-id="0843e-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="0843e-164">L'ordinamento ($orderby)</span><span class="sxs-lookup"><span data-stu-id="0843e-164">Sorting ($orderby)</span></span>

<span data-ttu-id="0843e-165">Per ordinare, utilizzare un `orderby` clausola.</span><span class="sxs-lookup"><span data-stu-id="0843e-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="0843e-166">Nell'esempio seguente vengono ordinati in base al prezzo, dal maggiore al minore.</span><span class="sxs-lookup"><span data-stu-id="0843e-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="0843e-167">Ecco la richiesta di OData corrispondente.</span><span class="sxs-lookup"><span data-stu-id="0843e-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="0843e-168">Paging sul lato client ($skip e $top)</span><span class="sxs-lookup"><span data-stu-id="0843e-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="0843e-169">Per i set di entità di grandi dimensioni, il client potrebbe voler limitare il numero di risultati.</span><span class="sxs-lookup"><span data-stu-id="0843e-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="0843e-170">Ad esempio, un client potrebbe mostrare 10 voci alla volta.</span><span class="sxs-lookup"><span data-stu-id="0843e-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="0843e-171">Questa operazione viene definita *paging sul lato client*.</span><span class="sxs-lookup"><span data-stu-id="0843e-171">This is called *client-side paging*.</span></span> <span data-ttu-id="0843e-172">(È anche disponibile [paging lato server](../supporting-odata-query-options.md#server-paging), in cui il server limita il numero di risultati.) Per eseguire il paging del lato client, utilizzare LINQ **Skip** e **richiedere** metodi.</span><span class="sxs-lookup"><span data-stu-id="0843e-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="0843e-173">Nell'esempio seguente ignora i primi 40 risultati e accetta i 10 successivi.</span><span class="sxs-lookup"><span data-stu-id="0843e-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="0843e-174">Ecco la richiesta di OData corrispondente:</span><span class="sxs-lookup"><span data-stu-id="0843e-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="0843e-175">($Select) Select ed Expand ($expand)</span><span class="sxs-lookup"><span data-stu-id="0843e-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="0843e-176">Per includere le entità correlate, usare il `DataServiceQuery<t>.Expand` (metodo).</span><span class="sxs-lookup"><span data-stu-id="0843e-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="0843e-177">Ad esempio, per includere il `Supplier` per ogni `Product`:</span><span class="sxs-lookup"><span data-stu-id="0843e-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="0843e-178">Ecco la richiesta di OData corrispondente:</span><span class="sxs-lookup"><span data-stu-id="0843e-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="0843e-179">Per modificare la forma della risposta, utilizzare LINQ **seleziona** clausola.</span><span class="sxs-lookup"><span data-stu-id="0843e-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="0843e-180">Nell'esempio seguente ottiene solo il nome di ogni prodotto, senza altre proprietà.</span><span class="sxs-lookup"><span data-stu-id="0843e-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="0843e-181">Ecco la richiesta di OData corrispondente:</span><span class="sxs-lookup"><span data-stu-id="0843e-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="0843e-182">Una clausola select può includere le entità correlate.</span><span class="sxs-lookup"><span data-stu-id="0843e-182">A select clause can include related entities.</span></span> <span data-ttu-id="0843e-183">In tal caso, non chiamare **Espandi**; il proxy include automaticamente l'espansione in questo caso.</span><span class="sxs-lookup"><span data-stu-id="0843e-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="0843e-184">Nell'esempio seguente ottiene il nome e il fornitore di ogni prodotto.</span><span class="sxs-lookup"><span data-stu-id="0843e-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="0843e-185">Ecco la richiesta di OData corrispondente.</span><span class="sxs-lookup"><span data-stu-id="0843e-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="0843e-186">Si noti che include il **$expand** opzione.</span><span class="sxs-lookup"><span data-stu-id="0843e-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="0843e-187">Per altre informazioni su $select e $espandere, vedere [Usa $select, $expand, $value nell'API Web 2 e](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="0843e-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="0843e-188">Aggiungere una nuova entità</span><span class="sxs-lookup"><span data-stu-id="0843e-188">Add a New Entity</span></span>

<span data-ttu-id="0843e-189">Per aggiungere una nuova entità in un set di entità, chiamare `AddToEntitySet`, dove *EntitySet* è il nome del set di entità.</span><span class="sxs-lookup"><span data-stu-id="0843e-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="0843e-190">Ad esempio, `AddToProducts` aggiunge un nuovo `Product` per il `Products` set di entità.</span><span class="sxs-lookup"><span data-stu-id="0843e-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="0843e-191">Quando si genera il proxy, WCF Data Services crea automaticamente questi fortemente tipizzate **AddTo** metodi.</span><span class="sxs-lookup"><span data-stu-id="0843e-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="0843e-192">Per aggiungere un collegamento tra due entità, usare il **AddLink** e **SetLink può essere utilizzato** metodi.</span><span class="sxs-lookup"><span data-stu-id="0843e-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="0843e-193">Il codice seguente aggiunge un nuovo fornitore e un nuovo prodotto e quindi crea i collegamenti tra di essi.</span><span class="sxs-lookup"><span data-stu-id="0843e-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="0843e-194">Uso **AddLink** quando la proprietà di navigazione è una raccolta.</span><span class="sxs-lookup"><span data-stu-id="0843e-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="0843e-195">In questo esempio, sarà presto disponibile un prodotto per il `Products` insieme al fornitore.</span><span class="sxs-lookup"><span data-stu-id="0843e-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="0843e-196">Uso **SetLink può essere utilizzato** quando la proprietà di navigazione è una singola entità.</span><span class="sxs-lookup"><span data-stu-id="0843e-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="0843e-197">In questo esempio viene impostata la `Supplier` proprietà su un prodotto.</span><span class="sxs-lookup"><span data-stu-id="0843e-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="0843e-198">Aggiornamento / Patch</span><span class="sxs-lookup"><span data-stu-id="0843e-198">Update / Patch</span></span>

<span data-ttu-id="0843e-199">Per aggiornare un'entità, chiamare il **UpdateObject** (metodo).</span><span class="sxs-lookup"><span data-stu-id="0843e-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="0843e-200">L'aggiornamento viene eseguito quando si chiama **SaveChanges**.</span><span class="sxs-lookup"><span data-stu-id="0843e-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="0843e-201">Per impostazione predefinita, WCF invia una richiesta HTTP MERGE.</span><span class="sxs-lookup"><span data-stu-id="0843e-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="0843e-202">Il **PatchOnUpdate** opzione indica a WCF per inviare invece un HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="0843e-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="0843e-203">Il motivo per cui applicare la PATCH e MERGE?</span><span class="sxs-lookup"><span data-stu-id="0843e-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="0843e-204">La specifica HTTP 1.1 originale ([2616 RCF](http://tools.ietf.org/html/rfc2616)) non è stato definito alcun metodo HTTP con la semantica di "aggiornamento parziale".</span><span class="sxs-lookup"><span data-stu-id="0843e-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="0843e-205">Per supportare gli aggiornamenti parziali, la specifica OData definito il metodo MERGE.</span><span class="sxs-lookup"><span data-stu-id="0843e-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="0843e-206">Nel 2010 [RFC 5789](http://tools.ietf.org/html/rfc5789) definito il metodo PATCH per gli aggiornamenti parziali.</span><span class="sxs-lookup"><span data-stu-id="0843e-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="0843e-207">È possibile leggere alcuni della cronologia in questo [post di blog](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) sul Blog di WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="0843e-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="0843e-208">Oggi, PATCH è preferibile rispetto a MERGE.</span><span class="sxs-lookup"><span data-stu-id="0843e-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="0843e-209">Il controller OData creato tramite lo scaffolding API Web supporta entrambi i metodi.</span><span class="sxs-lookup"><span data-stu-id="0843e-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>


<span data-ttu-id="0843e-210">Se si desidera sostituire l'intera entità (la semantica di PUT), specificare il **ReplaceOnUpdate** opzione.</span><span class="sxs-lookup"><span data-stu-id="0843e-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="0843e-211">Ciò fa sì che WCF inviare una richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="0843e-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="0843e-212">Eliminare un'entità</span><span class="sxs-lookup"><span data-stu-id="0843e-212">Delete an Entity</span></span>

<span data-ttu-id="0843e-213">Per eliminare un'entità, chiamare **DeleteObject**.</span><span class="sxs-lookup"><span data-stu-id="0843e-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="0843e-214">Richiamare un'azione OData</span><span class="sxs-lookup"><span data-stu-id="0843e-214">Invoke an OData Action</span></span>

<span data-ttu-id="0843e-215">In OData [azioni](odata-actions.md) rappresentano un modo per aggiungere comportamenti lato server che non sono definiti facilmente come operazioni CRUD su entità.</span><span class="sxs-lookup"><span data-stu-id="0843e-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="0843e-216">Anche se il documento di metadati OData descrive le azioni, la classe proxy non crea alcun metodo fortemente tipizzata per loro.</span><span class="sxs-lookup"><span data-stu-id="0843e-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="0843e-217">È comunque possibile richiamare un'azione OData utilizzando il tipo generico **Execute** (metodo).</span><span class="sxs-lookup"><span data-stu-id="0843e-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="0843e-218">Tuttavia, è necessario conoscere i tipi di dati dei parametri e il valore restituito.</span><span class="sxs-lookup"><span data-stu-id="0843e-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="0843e-219">Ad esempio, il `RateProduct` azione accetta parametro denominato "Rating" di tipo `Int32` e restituisce un `double`.</span><span class="sxs-lookup"><span data-stu-id="0843e-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="0843e-220">Il codice seguente viene illustrato come richiamare l'azione.</span><span class="sxs-lookup"><span data-stu-id="0843e-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="0843e-221">Per altre informazioni, vedere[chiamare operazioni del servizio e le azioni](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="0843e-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="0843e-222">È possibile estendere il **contenitore** classe per fornire un metodo fortemente tipizzato che richiama l'azione:</span><span class="sxs-lookup"><span data-stu-id="0843e-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
