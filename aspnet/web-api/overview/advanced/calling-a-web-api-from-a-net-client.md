---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Chiamare un'API Web da un Client .NET (C#)-ASP.NET 4.x
author: MikeWasson
description: Questa esercitazione illustra come chiamare un'API web da un'applicazione di .NET 4.x.
ms.author: riande
ms.date: 11/24/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: ca3b9424f30f48c7b8c71b850ffeca64244b123b
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65112838"
---
# <a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="6b38f-103">Chiamare un'API Web da un Client .NET (c#)</span><span class="sxs-lookup"><span data-stu-id="6b38f-103">Call a Web API From a .NET Client (C#)</span></span>

<span data-ttu-id="6b38f-104">dal [Mike Wasson](https://github.com/MikeWasson) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6b38f-104">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6b38f-105">[Download progetto completato](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span><span class="sxs-lookup"><span data-stu-id="6b38f-105">[Download Completed Project](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span></span> <span data-ttu-id="6b38f-106">[Istruzioni per il download](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="6b38f-106">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> 

<span data-ttu-id="6b38f-107">Questa esercitazione illustra come chiamare un'API web da un'applicazione .NET, usando [HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="6b38f-107">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="6b38f-108">In questa esercitazione viene scritta un'app client che utilizza l'API web seguente:</span><span class="sxs-lookup"><span data-stu-id="6b38f-108">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="6b38f-109">Operazione</span><span class="sxs-lookup"><span data-stu-id="6b38f-109">Action</span></span> | <span data-ttu-id="6b38f-110">Metodo HTTP</span><span class="sxs-lookup"><span data-stu-id="6b38f-110">HTTP method</span></span> | <span data-ttu-id="6b38f-111">URI relativo</span><span class="sxs-lookup"><span data-stu-id="6b38f-111">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6b38f-112">Ottenere un prodotto base all'ID</span><span class="sxs-lookup"><span data-stu-id="6b38f-112">Get a product by ID</span></span> | <span data-ttu-id="6b38f-113">GET</span><span class="sxs-lookup"><span data-stu-id="6b38f-113">GET</span></span> | <span data-ttu-id="6b38f-114">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="6b38f-114">/api/products/*id*</span></span> |
| <span data-ttu-id="6b38f-115">Creare un nuovo prodotto</span><span class="sxs-lookup"><span data-stu-id="6b38f-115">Create a new product</span></span> | <span data-ttu-id="6b38f-116">INSERISCI</span><span class="sxs-lookup"><span data-stu-id="6b38f-116">POST</span></span> | <span data-ttu-id="6b38f-117">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="6b38f-117">/api/products</span></span> |
| <span data-ttu-id="6b38f-118">Aggiornare un prodotto</span><span class="sxs-lookup"><span data-stu-id="6b38f-118">Update a product</span></span> | <span data-ttu-id="6b38f-119">PUT</span><span class="sxs-lookup"><span data-stu-id="6b38f-119">PUT</span></span> | <span data-ttu-id="6b38f-120">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="6b38f-120">/api/products/*id*</span></span> |
| <span data-ttu-id="6b38f-121">Eliminare un prodotto</span><span class="sxs-lookup"><span data-stu-id="6b38f-121">Delete a product</span></span> | <span data-ttu-id="6b38f-122">DELETE</span><span class="sxs-lookup"><span data-stu-id="6b38f-122">DELETE</span></span> | <span data-ttu-id="6b38f-123">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="6b38f-123">/api/products/*id*</span></span> |

<span data-ttu-id="6b38f-124">Per informazioni su come implementare questa API con l'API Web ASP.NET, vedere [creazione di un'API Web che supporta le operazioni CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="6b38f-124">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="6b38f-125">Per semplicità, l'applicazione client in questa esercitazione è un'applicazione console di Windows.</span><span class="sxs-lookup"><span data-stu-id="6b38f-125">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="6b38f-126">**HttpClient** è supportata anche per le app di Windows Phone e Windows Store.</span><span class="sxs-lookup"><span data-stu-id="6b38f-126">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="6b38f-127">Per altre informazioni, vedere [scrittura di codice di Client API Web per più piattaforme usando le librerie portabili](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="6b38f-127">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="6b38f-128">Creare l'applicazione Console</span><span class="sxs-lookup"><span data-stu-id="6b38f-128">Create the Console Application</span></span>

<span data-ttu-id="6b38f-129">In Visual Studio, creare una nuova app console Windows denominata **HttpClientSample** e incollare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="6b38f-129">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="6b38f-130">Il codice precedente è l'app client completa.</span><span class="sxs-lookup"><span data-stu-id="6b38f-130">The preceding code is the complete client app.</span></span>

<span data-ttu-id="6b38f-131">`RunAsync` viene eseguito e si blocca fino al completamento.</span><span class="sxs-lookup"><span data-stu-id="6b38f-131">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="6b38f-132">La maggior parte degli **HttpClient** metodi sono asincroni, poiché eseguono i/o rete.</span><span class="sxs-lookup"><span data-stu-id="6b38f-132">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="6b38f-133">Tutte le attività asincrone vengono eseguite all'interno di `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="6b38f-133">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="6b38f-134">In genere un'app non blocca il thread principale, ma questa app non consente alcuna interazione.</span><span class="sxs-lookup"><span data-stu-id="6b38f-134">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="6b38f-135">Installare le librerie Client dell'API Web</span><span class="sxs-lookup"><span data-stu-id="6b38f-135">Install the Web API Client Libraries</span></span>

<span data-ttu-id="6b38f-136">Usare Gestione pacchetti NuGet per installare il pacchetto di librerie Client API Web.</span><span class="sxs-lookup"><span data-stu-id="6b38f-136">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="6b38f-137">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="6b38f-137">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="6b38f-138">In Package Manager Console (PMC), digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="6b38f-138">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="6b38f-139">Il comando precedente consente di aggiungere i pacchetti NuGet seguenti al progetto:</span><span class="sxs-lookup"><span data-stu-id="6b38f-139">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="6b38f-140">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="6b38f-140">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="6b38f-141">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="6b38f-141">Newtonsoft.Json</span></span>

<span data-ttu-id="6b38f-142">Json.NET è un framework JSON ad alte prestazioni più diffusi per .NET.</span><span class="sxs-lookup"><span data-stu-id="6b38f-142">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="6b38f-143">Aggiungere una classe modello</span><span class="sxs-lookup"><span data-stu-id="6b38f-143">Add a Model Class</span></span>

<span data-ttu-id="6b38f-144">Esaminare la classe `Product`:</span><span class="sxs-lookup"><span data-stu-id="6b38f-144">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="6b38f-145">Questa classe corrisponde al modello di dati usato dall'API web.</span><span class="sxs-lookup"><span data-stu-id="6b38f-145">This class matches the data model used by the web API.</span></span> <span data-ttu-id="6b38f-146">Un'app può usare **HttpClient** per leggere un `Product` istanza da una risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6b38f-146">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="6b38f-147">L'app non deve scrivere alcun codice di deserializzazione.</span><span class="sxs-lookup"><span data-stu-id="6b38f-147">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="6b38f-148">Creare e inizializzare HttpClient</span><span class="sxs-lookup"><span data-stu-id="6b38f-148">Create and Initialize HttpClient</span></span>

<span data-ttu-id="6b38f-149">Esaminare il metodo statico **HttpClient** proprietà:</span><span class="sxs-lookup"><span data-stu-id="6b38f-149">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="6b38f-150">**HttpClient** è destinato a essere creata un'istanza di una volta e riutilizzate per tutta la durata di un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="6b38f-150">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="6b38f-151">Le condizioni seguenti possono comportare **SocketException** errori:</span><span class="sxs-lookup"><span data-stu-id="6b38f-151">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="6b38f-152">Creazione di una nuova **HttpClient** istanza per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="6b38f-152">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="6b38f-153">Server con un carico pesante.</span><span class="sxs-lookup"><span data-stu-id="6b38f-153">Server under heavy load.</span></span>

<span data-ttu-id="6b38f-154">Creazione di una nuova **HttpClient** istanza per ogni richiesta può esaurire i socket disponibili.</span><span class="sxs-lookup"><span data-stu-id="6b38f-154">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="6b38f-155">Il codice seguente consente di inizializzare il **HttpClient** istanza:</span><span class="sxs-lookup"><span data-stu-id="6b38f-155">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="6b38f-156">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="6b38f-156">The preceding code:</span></span>

* <span data-ttu-id="6b38f-157">Imposta l'URI di base per le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6b38f-157">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="6b38f-158">Modificare il numero di porta per la porta utilizzata per l'app server.</span><span class="sxs-lookup"><span data-stu-id="6b38f-158">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="6b38f-159">L'app non funzionerà a meno che non viene utilizzata la porta per l'app server.</span><span class="sxs-lookup"><span data-stu-id="6b38f-159">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="6b38f-160">Imposta l'intestazione Accept su "application/json".</span><span class="sxs-lookup"><span data-stu-id="6b38f-160">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="6b38f-161">L'impostazione di questa intestazione indica al server di inviare i dati in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="6b38f-161">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="6b38f-162">Inviare una richiesta GET per recuperare una risorsa</span><span class="sxs-lookup"><span data-stu-id="6b38f-162">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="6b38f-163">Il codice seguente invia una richiesta GET per un prodotto:</span><span class="sxs-lookup"><span data-stu-id="6b38f-163">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="6b38f-164">Il **GetAsync** metodo invia la richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="6b38f-164">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="6b38f-165">Al termine, il metodo restituisce un **HttpResponseMessage** che contiene la risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6b38f-165">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="6b38f-166">Se il codice di stato nella risposta è un codice di riuscita, il corpo della risposta contiene la rappresentazione JSON di un prodotto.</span><span class="sxs-lookup"><span data-stu-id="6b38f-166">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="6b38f-167">Chiamare **ReadAsAsync** deserializzare il payload JSON per un `Product` istanza.</span><span class="sxs-lookup"><span data-stu-id="6b38f-167">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="6b38f-168">Il **ReadAsAsync** metodo è asincrono, poiché il corpo della risposta può essere arbitrariamente grande.</span><span class="sxs-lookup"><span data-stu-id="6b38f-168">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="6b38f-169">**HttpClient** non genera un'eccezione quando la risposta HTTP contiene un codice di errore.</span><span class="sxs-lookup"><span data-stu-id="6b38f-169">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="6b38f-170">Al contrario, il **IsSuccessStatusCode** è di proprietà **false** se lo stato è un codice di errore.</span><span class="sxs-lookup"><span data-stu-id="6b38f-170">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="6b38f-171">Se si preferisce gestire i codici di errore HTTP come eccezioni, chiamare [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) nell'oggetto della risposta.</span><span class="sxs-lookup"><span data-stu-id="6b38f-171">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="6b38f-172">`EnsureSuccessStatusCode` genera un'eccezione se il codice di stato non è compreso nell'intervallo di 200&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="6b38f-172">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="6b38f-173">Si noti che **HttpClient** può generare eccezioni per altri motivi &mdash; ad esempio, se la richiesta scade.</span><span class="sxs-lookup"><span data-stu-id="6b38f-173">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="6b38f-174">Formattatori di Media Type da deserializzare</span><span class="sxs-lookup"><span data-stu-id="6b38f-174">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="6b38f-175">Quando **ReadAsAsync** viene chiamata senza parametri, viene utilizzato un set predefinito di *formattatori di media* per leggere il corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="6b38f-175">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="6b38f-176">I formattatori predefiniti supportano JSON, XML e i dati codificati negli url di Form.</span><span class="sxs-lookup"><span data-stu-id="6b38f-176">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="6b38f-177">Invece di usare i formattatori predefiniti, è possibile fornire un elenco di formattatori per la **ReadAsAsync** (metodo).</span><span class="sxs-lookup"><span data-stu-id="6b38f-177">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="6b38f-178">Usando un elenco di formattatori è utile se si dispone di un formattatore di media type personalizzato:</span><span class="sxs-lookup"><span data-stu-id="6b38f-178">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="6b38f-179">Per altre informazioni, vedere [formattatori di Media in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="6b38f-179">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="6b38f-180">Inviare una richiesta POST per creare una risorsa</span><span class="sxs-lookup"><span data-stu-id="6b38f-180">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="6b38f-181">Il codice seguente invia una richiesta POST contenente un `Product` istanza nel formato JSON:</span><span class="sxs-lookup"><span data-stu-id="6b38f-181">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="6b38f-182">Il **PostAsJsonAsync** metodo:</span><span class="sxs-lookup"><span data-stu-id="6b38f-182">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="6b38f-183">Serializza un oggetto in JSON.</span><span class="sxs-lookup"><span data-stu-id="6b38f-183">Serializes an object to JSON.</span></span>
* <span data-ttu-id="6b38f-184">Invia il payload JSON in una richiesta POST.</span><span class="sxs-lookup"><span data-stu-id="6b38f-184">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="6b38f-185">Se la richiesta ha esito positivo:</span><span class="sxs-lookup"><span data-stu-id="6b38f-185">If the request succeeds:</span></span>

* <span data-ttu-id="6b38f-186">Deve restituire una risposta 201 (creato).</span><span class="sxs-lookup"><span data-stu-id="6b38f-186">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="6b38f-187">La risposta deve includere l'URL delle risorse create nell'intestazione Location.</span><span class="sxs-lookup"><span data-stu-id="6b38f-187">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="6b38f-188">Invia una richiesta PUT per aggiornare una risorsa</span><span class="sxs-lookup"><span data-stu-id="6b38f-188">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="6b38f-189">Il codice seguente invia una richiesta PUT per aggiornare un prodotto:</span><span class="sxs-lookup"><span data-stu-id="6b38f-189">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="6b38f-190">Il **PutAsJsonAsync** metodo è paragonabile **PostAsJsonAsync**, ad eccezione del fatto che viene inviata una richiesta PUT anziché POST.</span><span class="sxs-lookup"><span data-stu-id="6b38f-190">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="6b38f-191">Invia una richiesta DELETE per eliminare una risorsa</span><span class="sxs-lookup"><span data-stu-id="6b38f-191">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="6b38f-192">Il codice seguente invia una richiesta DELETE per eliminare un prodotto:</span><span class="sxs-lookup"><span data-stu-id="6b38f-192">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="6b38f-193">Ad esempio GET, una richiesta di eliminazione non è un corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="6b38f-193">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="6b38f-194">Non è necessario specificare il formato JSON o XML con l'istruzione DELETE.</span><span class="sxs-lookup"><span data-stu-id="6b38f-194">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="6b38f-195">Testare il codice di esempio</span><span class="sxs-lookup"><span data-stu-id="6b38f-195">Test the sample</span></span>

<span data-ttu-id="6b38f-196">Per testare l'app client:</span><span class="sxs-lookup"><span data-stu-id="6b38f-196">To test the client app:</span></span>

1. <span data-ttu-id="6b38f-197">[Scaricare](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) ed eseguire l'app server.</span><span class="sxs-lookup"><span data-stu-id="6b38f-197">[Download](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="6b38f-198">[Istruzioni per il download](/aspnet/core/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="6b38f-198">[Download instructions](/aspnet/core/#how-to-download-a-sample).</span></span> <span data-ttu-id="6b38f-199">Verificare il che funzionamento dell'app server.</span><span class="sxs-lookup"><span data-stu-id="6b38f-199">Verify the server app is working.</span></span> <span data-ttu-id="6b38f-200">Ad esempio, `http://localhost:64195/api/products` deve restituire un elenco di prodotti.</span><span class="sxs-lookup"><span data-stu-id="6b38f-200">For example, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="6b38f-201">Impostare l'URI di base per le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6b38f-201">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="6b38f-202">Modificare il numero di porta per la porta utilizzata per l'app server.</span><span class="sxs-lookup"><span data-stu-id="6b38f-202">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="6b38f-203">Eseguire l'app client.</span><span class="sxs-lookup"><span data-stu-id="6b38f-203">Run the client app.</span></span> <span data-ttu-id="6b38f-204">Viene generato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="6b38f-204">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
