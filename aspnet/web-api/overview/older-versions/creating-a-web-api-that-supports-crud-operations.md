---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Abilitare le operazioni CRUD nell'API Web ASP.NET 1 | Microsoft Docs
author: MikeWasson
description: Questa esercitazione illustra come supportare le operazioni CRUD in un servizio HTTP tramite l'API Web ASP.NET. Versioni del software utilizzate nell'esercitazione Visual Studio 2012 Web punto di accesso...
ms.author: riande
ms.date: 01/28/2012
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: f3cb0004075ef7687ca1096bd407c342b4d0b7be
ms.sourcegitcommit: 289e051cc8a90e8f7127e239fda73047bde4de12
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2019
ms.locfileid: "58423749"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="dab2a-104">Abilitare le operazioni CRUD nell'API Web ASP.NET 1</span><span class="sxs-lookup"><span data-stu-id="dab2a-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="dab2a-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="dab2a-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="dab2a-106">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="dab2a-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="dab2a-107">Questa esercitazione illustra come supportare le operazioni CRUD in un servizio HTTP tramite l'API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="dab2a-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="dab2a-108">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="dab2a-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="dab2a-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="dab2a-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="dab2a-110">API Web 1 (funziona anche con l'API Web 2)</span><span class="sxs-lookup"><span data-stu-id="dab2a-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="dab2a-111">CRUD è l'acronimo &quot;Create, Read, Update e Delete,&quot; quali sono le quattro operazioni di base dei database.</span><span class="sxs-lookup"><span data-stu-id="dab2a-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="dab2a-112">Molti servizi HTTP anche modellare le operazioni CRUD tramite REST o l'API simili a REST.</span><span class="sxs-lookup"><span data-stu-id="dab2a-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="dab2a-113">In questa esercitazione si compilerà un'API per gestire un elenco di prodotti web molto semplice.</span><span class="sxs-lookup"><span data-stu-id="dab2a-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="dab2a-114">Ogni prodotto conterrà un nome, prezzo e la categoria (ad esempio &quot;toys&quot; oppure &quot;hardware&quot;), oltre a un ID prodotto.</span><span class="sxs-lookup"><span data-stu-id="dab2a-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="dab2a-115">I prodotti API esporrà i metodi in seguito.</span><span class="sxs-lookup"><span data-stu-id="dab2a-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="dab2a-116">Operazione</span><span class="sxs-lookup"><span data-stu-id="dab2a-116">Action</span></span> | <span data-ttu-id="dab2a-117">Metodo HTTP</span><span class="sxs-lookup"><span data-stu-id="dab2a-117">HTTP method</span></span> | <span data-ttu-id="dab2a-118">URI relativo</span><span class="sxs-lookup"><span data-stu-id="dab2a-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dab2a-119">Ottenere un elenco di tutti i prodotti</span><span class="sxs-lookup"><span data-stu-id="dab2a-119">Get a list of all products</span></span> | <span data-ttu-id="dab2a-120">GET</span><span class="sxs-lookup"><span data-stu-id="dab2a-120">GET</span></span> | <span data-ttu-id="dab2a-121">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="dab2a-121">/api/products</span></span> |
| <span data-ttu-id="dab2a-122">Ottenere un prodotto base all'ID</span><span class="sxs-lookup"><span data-stu-id="dab2a-122">Get a product by ID</span></span> | <span data-ttu-id="dab2a-123">GET</span><span class="sxs-lookup"><span data-stu-id="dab2a-123">GET</span></span> | <span data-ttu-id="dab2a-124">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="dab2a-124">/api/products/*id*</span></span> |
| <span data-ttu-id="dab2a-125">Ottenere un prodotto per categoria</span><span class="sxs-lookup"><span data-stu-id="dab2a-125">Get a product by category</span></span> | <span data-ttu-id="dab2a-126">GET</span><span class="sxs-lookup"><span data-stu-id="dab2a-126">GET</span></span> | <span data-ttu-id="dab2a-127">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="dab2a-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="dab2a-128">Creare un nuovo prodotto</span><span class="sxs-lookup"><span data-stu-id="dab2a-128">Create a new product</span></span> | <span data-ttu-id="dab2a-129">INSERISCI</span><span class="sxs-lookup"><span data-stu-id="dab2a-129">POST</span></span> | <span data-ttu-id="dab2a-130">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="dab2a-130">/api/products</span></span> |
| <span data-ttu-id="dab2a-131">Aggiornare un prodotto</span><span class="sxs-lookup"><span data-stu-id="dab2a-131">Update a product</span></span> | <span data-ttu-id="dab2a-132">PUT</span><span class="sxs-lookup"><span data-stu-id="dab2a-132">PUT</span></span> | <span data-ttu-id="dab2a-133">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="dab2a-133">/api/products/*id*</span></span> |
| <span data-ttu-id="dab2a-134">Eliminare un prodotto</span><span class="sxs-lookup"><span data-stu-id="dab2a-134">Delete a product</span></span> | <span data-ttu-id="dab2a-135">DELETE</span><span class="sxs-lookup"><span data-stu-id="dab2a-135">DELETE</span></span> | <span data-ttu-id="dab2a-136">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="dab2a-136">/api/products/*id*</span></span> |

<span data-ttu-id="dab2a-137">Si noti che alcuni degli URI includono l'ID prodotto nel percorso.</span><span class="sxs-lookup"><span data-stu-id="dab2a-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="dab2a-138">Ad esempio, per ottenere il prodotto il cui ID è 28, il client invia una richiesta GET `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="dab2a-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="dab2a-139">Risorse</span><span class="sxs-lookup"><span data-stu-id="dab2a-139">Resources</span></span>

<span data-ttu-id="dab2a-140">I prodotti API definisce gli URI per due tipi di risorse:</span><span class="sxs-lookup"><span data-stu-id="dab2a-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="dab2a-141">Risorsa</span><span class="sxs-lookup"><span data-stu-id="dab2a-141">Resource</span></span> | <span data-ttu-id="dab2a-142">URI</span><span class="sxs-lookup"><span data-stu-id="dab2a-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="dab2a-143">L'elenco di tutti i prodotti.</span><span class="sxs-lookup"><span data-stu-id="dab2a-143">The list of all the products.</span></span> | <span data-ttu-id="dab2a-144">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="dab2a-144">/api/products</span></span> |
| <span data-ttu-id="dab2a-145">Un singolo prodotto.</span><span class="sxs-lookup"><span data-stu-id="dab2a-145">An individual product.</span></span> | <span data-ttu-id="dab2a-146">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="dab2a-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="dab2a-147">Metodi</span><span class="sxs-lookup"><span data-stu-id="dab2a-147">Methods</span></span>

<span data-ttu-id="dab2a-148">I quattro principali metodi HTTP (GET, PUT, POST e DELETE) possono essere mappati a operazioni CRUD, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="dab2a-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="dab2a-149">GET recupera la rappresentazione della risorsa a un URI specificato.</span><span class="sxs-lookup"><span data-stu-id="dab2a-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="dab2a-150">GET deve non hanno effetti collaterali sul server.</span><span class="sxs-lookup"><span data-stu-id="dab2a-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="dab2a-151">PUT consente di aggiornare una risorsa a un URI specificato.</span><span class="sxs-lookup"><span data-stu-id="dab2a-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="dab2a-152">PUT è anche utilizzabile per creare una nuova risorsa a un URI specificato, se il server consente ai client di specificare nuovi URI.</span><span class="sxs-lookup"><span data-stu-id="dab2a-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="dab2a-153">Per questa esercitazione, l'API non supporterà la creazione tramite PUT.</span><span class="sxs-lookup"><span data-stu-id="dab2a-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="dab2a-154">POST crea una nuova risorsa.</span><span class="sxs-lookup"><span data-stu-id="dab2a-154">POST creates a new resource.</span></span> <span data-ttu-id="dab2a-155">Il server assegna l'URI per il nuovo oggetto e restituisce questo URI come parte del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="dab2a-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="dab2a-156">DELETE elimina una risorsa a un URI specificato.</span><span class="sxs-lookup"><span data-stu-id="dab2a-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="dab2a-157">Nota: Il metodo PUT sostituisce l'entità product intero.</span><span class="sxs-lookup"><span data-stu-id="dab2a-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="dab2a-158">Vale a dire, il client è previsto per l'invio di una rappresentazione completa del prodotto aggiornato.</span><span class="sxs-lookup"><span data-stu-id="dab2a-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="dab2a-159">Se si desidera supportare gli aggiornamenti parziali, il metodo PATCH è preferito.</span><span class="sxs-lookup"><span data-stu-id="dab2a-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="dab2a-160">Questa esercitazione può neimplementuje metodu PATCH.</span><span class="sxs-lookup"><span data-stu-id="dab2a-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="dab2a-161">Creare un nuovo progetto API Web</span><span class="sxs-lookup"><span data-stu-id="dab2a-161">Create a New Web API Project</span></span>

<span data-ttu-id="dab2a-162">Iniziare eseguendo Visual Studio e selezionare **nuovo progetto** dalle **avviare** pagina.</span><span class="sxs-lookup"><span data-stu-id="dab2a-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="dab2a-163">E viceversa, il **File** dal menu **New** e quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="dab2a-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="dab2a-164">Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo.</span><span class="sxs-lookup"><span data-stu-id="dab2a-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="dab2a-165">Sotto **Visual c#**, selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="dab2a-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="dab2a-166">Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="dab2a-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="dab2a-167">Denominare il progetto &quot;ProductStore&quot; e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="dab2a-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="dab2a-168">Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo, seleziona **API Web** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="dab2a-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="dab2a-169">Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="dab2a-169">Adding a Model</span></span>

<span data-ttu-id="dab2a-170">Un *modello* è un oggetto che rappresenta i dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="dab2a-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="dab2a-171">Nell'API Web ASP.NET, è possibile usare oggetti CLR fortemente tipizzati come modelli e si viene automaticamente serializzate in XML o JSON per il client.</span><span class="sxs-lookup"><span data-stu-id="dab2a-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="dab2a-172">Per l'API ProductStore, i dati costituita da prodotti, verrà quindi creata una nuova classe denominata `Product`.</span><span class="sxs-lookup"><span data-stu-id="dab2a-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="dab2a-173">Se Esplora soluzioni non è già visibile, scegliere il **View** menu e selezionare **Esplora soluzioni**.</span><span class="sxs-lookup"><span data-stu-id="dab2a-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="dab2a-174">In Esplora soluzioni fare doppio clic il **modelli** cartella.</span><span class="sxs-lookup"><span data-stu-id="dab2a-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="dab2a-175">Meny il contesto, selezionare **Add**, quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="dab2a-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="dab2a-176">Denominare la classe &quot;prodotto&quot;.</span><span class="sxs-lookup"><span data-stu-id="dab2a-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="dab2a-177">Aggiungere le seguenti proprietà per il `Product` classe.</span><span class="sxs-lookup"><span data-stu-id="dab2a-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="dab2a-178">Aggiunta di un Repository</span><span class="sxs-lookup"><span data-stu-id="dab2a-178">Adding a Repository</span></span>

<span data-ttu-id="dab2a-179">È necessario archiviare un insieme di prodotti.</span><span class="sxs-lookup"><span data-stu-id="dab2a-179">We need to store a collection of products.</span></span> <span data-ttu-id="dab2a-180">È un'ottima idea separare la raccolta dall'implementazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="dab2a-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="dab2a-181">In questo modo, è possibile modificare l'archivio di backup senza riscrivere la classe del servizio.</span><span class="sxs-lookup"><span data-stu-id="dab2a-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="dab2a-182">Questo tipo di progettazione viene chiamato il *repository* pattern.</span><span class="sxs-lookup"><span data-stu-id="dab2a-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="dab2a-183">Iniziare definendo un'interfaccia generica per il repository.</span><span class="sxs-lookup"><span data-stu-id="dab2a-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="dab2a-184">In Esplora soluzioni fare doppio clic il **modelli** cartella.</span><span class="sxs-lookup"><span data-stu-id="dab2a-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="dab2a-185">Selezionare **Add**, quindi selezionare **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="dab2a-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="dab2a-186">Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il nodo c#.</span><span class="sxs-lookup"><span data-stu-id="dab2a-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="dab2a-187">In c#, selezionare **codice**.</span><span class="sxs-lookup"><span data-stu-id="dab2a-187">Under C#, select **Code**.</span></span> <span data-ttu-id="dab2a-188">Nell'elenco dei modelli di codice, selezionare **interfaccia**.</span><span class="sxs-lookup"><span data-stu-id="dab2a-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="dab2a-189">Denominare l'interfaccia &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="dab2a-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="dab2a-190">Aggiungere l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="dab2a-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="dab2a-191">Aggiungere ora un'altra classe nella cartella Models, denominato &quot;ProductRepository.&quot; Questa classe implementerà l'interfaccia `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="dab2a-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRepository` interface.</span></span> <span data-ttu-id="dab2a-192">Aggiungere l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="dab2a-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="dab2a-193">Il repository mantiene l'elenco in memoria locale.</span><span class="sxs-lookup"><span data-stu-id="dab2a-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="dab2a-194">Questo è accettabile per un'esercitazione, ma in un'applicazione reale, è necessario archiviare esternamente, i dati di un database o nell'archiviazione cloud.</span><span class="sxs-lookup"><span data-stu-id="dab2a-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="dab2a-195">Lo schema repository renderà più facile modificare l'implementazione in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="dab2a-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="dab2a-196">Aggiunta di un Controller API Web</span><span class="sxs-lookup"><span data-stu-id="dab2a-196">Adding a Web API Controller</span></span>

<span data-ttu-id="dab2a-197">Se l'utente abbia familiarità con ASP.NET MVC, quindi si ha familiarità con i controller.</span><span class="sxs-lookup"><span data-stu-id="dab2a-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="dab2a-198">Nell'API Web ASP.NET, un *controller* è una classe che gestisce le richieste HTTP dal client.</span><span class="sxs-lookup"><span data-stu-id="dab2a-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="dab2a-199">La procedura guidata nuovo progetto creato due controller durante la creazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="dab2a-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="dab2a-200">Per visualizzarle, espandere la cartella controller in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="dab2a-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="dab2a-201">HomeController è un controller MVC ASP.NET tradizionale.</span><span class="sxs-lookup"><span data-stu-id="dab2a-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="dab2a-202">È responsabile per la gestione delle pagine HTML per il sito e non è correlato direttamente l'API Web.</span><span class="sxs-lookup"><span data-stu-id="dab2a-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="dab2a-203">ValuesController è un controller API Web di esempio.</span><span class="sxs-lookup"><span data-stu-id="dab2a-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="dab2a-204">Andare avanti ed eliminare ValuesController, facendo clic sul file in Esplora soluzioni e selezionando **eliminare.**</span><span class="sxs-lookup"><span data-stu-id="dab2a-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="dab2a-205">Aggiungere ora un nuovo controller, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="dab2a-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="dab2a-206">Nelle **Esplora soluzioni**, fare clic sulla cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="dab2a-206">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="dab2a-207">Selezionare **Add** e quindi selezionare **Controller**.</span><span class="sxs-lookup"><span data-stu-id="dab2a-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="dab2a-208">Nel **Aggiungi Controller** procedura guidata, nome del controller &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="dab2a-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="dab2a-209">Nel **modello** elenco a discesa, seleziona **Controller API vuoto**.</span><span class="sxs-lookup"><span data-stu-id="dab2a-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="dab2a-210">Fare quindi clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="dab2a-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="dab2a-211">Non è necessario inserire i controller in una cartella denominata controller.</span><span class="sxs-lookup"><span data-stu-id="dab2a-211">It is not necessary to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="dab2a-212">Il nome della cartella non è rilevante. è semplicemente un modo pratico per organizzare i file di origine.</span><span class="sxs-lookup"><span data-stu-id="dab2a-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="dab2a-213">Il **Aggiungi Controller** procedura guidata creerà un file denominato ProductsController.cs nella cartella controller.</span><span class="sxs-lookup"><span data-stu-id="dab2a-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="dab2a-214">Se non è già aperto questo file, fare doppio clic sul file per aprirlo.</span><span class="sxs-lookup"><span data-stu-id="dab2a-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="dab2a-215">Aggiungere il codice seguente **usando** istruzione:</span><span class="sxs-lookup"><span data-stu-id="dab2a-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="dab2a-216">Aggiungere un campo che contiene un' **IProductRepository** istanza.</span><span class="sxs-lookup"><span data-stu-id="dab2a-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="dab2a-217">La chiamata `new ProductRepository()` nel controller non è la migliore struttura, in quanto si unisce il controller a una particolare implementazione di `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="dab2a-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="dab2a-218">Per un approccio migliore, vedere [utilizzando il Resolver di dipendenza di API Web](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="dab2a-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="dab2a-219">Recupero di una risorsa</span><span class="sxs-lookup"><span data-stu-id="dab2a-219">Getting a Resource</span></span>

<span data-ttu-id="dab2a-220">L'API ProductStore esporrà diversi &quot;leggere&quot; azioni sotto forma di metodi HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="dab2a-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="dab2a-221">Ogni azione corrisponderà a un metodo nel `ProductsController` classe.</span><span class="sxs-lookup"><span data-stu-id="dab2a-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="dab2a-222">Operazione</span><span class="sxs-lookup"><span data-stu-id="dab2a-222">Action</span></span> | <span data-ttu-id="dab2a-223">Metodo HTTP</span><span class="sxs-lookup"><span data-stu-id="dab2a-223">HTTP method</span></span> | <span data-ttu-id="dab2a-224">URI relativo</span><span class="sxs-lookup"><span data-stu-id="dab2a-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dab2a-225">Ottenere un elenco di tutti i prodotti</span><span class="sxs-lookup"><span data-stu-id="dab2a-225">Get a list of all products</span></span> | <span data-ttu-id="dab2a-226">GET</span><span class="sxs-lookup"><span data-stu-id="dab2a-226">GET</span></span> | <span data-ttu-id="dab2a-227">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="dab2a-227">/api/products</span></span> |
| <span data-ttu-id="dab2a-228">Ottenere un prodotto base all'ID</span><span class="sxs-lookup"><span data-stu-id="dab2a-228">Get a product by ID</span></span> | <span data-ttu-id="dab2a-229">GET</span><span class="sxs-lookup"><span data-stu-id="dab2a-229">GET</span></span> | <span data-ttu-id="dab2a-230">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="dab2a-230">/api/products/*id*</span></span> |
| <span data-ttu-id="dab2a-231">Ottenere un prodotto per categoria</span><span class="sxs-lookup"><span data-stu-id="dab2a-231">Get a product by category</span></span> | <span data-ttu-id="dab2a-232">GET</span><span class="sxs-lookup"><span data-stu-id="dab2a-232">GET</span></span> | <span data-ttu-id="dab2a-233">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="dab2a-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="dab2a-234">Per ottenere l'elenco di tutti i prodotti, aggiungere questo metodo per il `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="dab2a-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="dab2a-235">Il nome del metodo inizia con &quot;ottenere&quot;, in modo che per convenzione viene eseguito il mapping alle richieste GET.</span><span class="sxs-lookup"><span data-stu-id="dab2a-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="dab2a-236">Inoltre, poiché il metodo non ha parametri, esegue il mapping a un URI che non contiene un' *&quot;id&quot;* segmento nel percorso.</span><span class="sxs-lookup"><span data-stu-id="dab2a-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="dab2a-237">Per ottenere un prodotto dall'ID, aggiungere questo metodo per il `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="dab2a-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="dab2a-238">Questo nome di metodo inoltre inizia con &quot;ottenere&quot;, ma il metodo ha un parametro denominato *id*. Questo parametro viene eseguito il mapping per il &quot;id&quot; segmento del percorso URI.</span><span class="sxs-lookup"><span data-stu-id="dab2a-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="dab2a-239">Il framework API Web ASP.NET converte automaticamente l'ID per il tipo di dati corretto (**int**) per il parametro.</span><span class="sxs-lookup"><span data-stu-id="dab2a-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="dab2a-240">Il metodo GetProduct genera un'eccezione di tipo **HttpResponseException** se *id* non è valido.</span><span class="sxs-lookup"><span data-stu-id="dab2a-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="dab2a-241">Questa eccezione verrà convertita dal framework in un errore 404 (non trovato).</span><span class="sxs-lookup"><span data-stu-id="dab2a-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="dab2a-242">Infine, aggiungere un metodo per trovare i prodotti per categoria:</span><span class="sxs-lookup"><span data-stu-id="dab2a-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="dab2a-243">Se l'URI della richiesta è una stringa di query, API Web cerca la corrispondenza con i parametri di query ai parametri nel metodo del controller.</span><span class="sxs-lookup"><span data-stu-id="dab2a-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="dab2a-244">Pertanto, un URI nel formato "api/prodotti? categoria =*categoria*" verrà eseguito il mapping a questo metodo.</span><span class="sxs-lookup"><span data-stu-id="dab2a-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="dab2a-245">Creazione di una risorsa</span><span class="sxs-lookup"><span data-stu-id="dab2a-245">Creating a Resource</span></span>

<span data-ttu-id="dab2a-246">Successivamente, verrà aggiunto un metodo per il `ProductsController` classe per creare un nuovo prodotto.</span><span class="sxs-lookup"><span data-stu-id="dab2a-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="dab2a-247">Ecco una semplice implementazione del metodo:</span><span class="sxs-lookup"><span data-stu-id="dab2a-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="dab2a-248">Tenere presente due aspetti di questo metodo:</span><span class="sxs-lookup"><span data-stu-id="dab2a-248">Note two things about this method:</span></span>

- <span data-ttu-id="dab2a-249">Il nome del metodo inizia con &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="dab2a-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="dab2a-250">Per creare un nuovo prodotto, il client invia una richiesta HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="dab2a-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="dab2a-251">Il metodo accetta un parametro di tipo Product.</span><span class="sxs-lookup"><span data-stu-id="dab2a-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="dab2a-252">Nell'API Web, i parametri con tipi complessi vengono deserializzati dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="dab2a-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="dab2a-253">È quindi necessario che il client invia una rappresentazione serializzata di un oggetto di prodotto, in formato XML o JSON.</span><span class="sxs-lookup"><span data-stu-id="dab2a-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="dab2a-254">Questa implementazione funzionerà, ma non è ancora completo.</span><span class="sxs-lookup"><span data-stu-id="dab2a-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="dab2a-255">In teoria, vorremmo la risposta HTTP da includere quanto segue:</span><span class="sxs-lookup"><span data-stu-id="dab2a-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="dab2a-256">**Codice di risposta:** Per impostazione predefinita, il framework API Web imposta il codice di stato risposta 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="dab2a-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="dab2a-257">Tuttavia, secondo il protocollo HTTP/1.1, quando una richiesta POST comporta la creazione di una risorsa, il server deve rispondere con stato 201 (creato).</span><span class="sxs-lookup"><span data-stu-id="dab2a-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="dab2a-258">**Percorso:** Quando il server crea una risorsa, deve includere l'URI della nuova risorsa nell'intestazione Location della risposta.</span><span class="sxs-lookup"><span data-stu-id="dab2a-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="dab2a-259">API Web ASP.NET semplifica modificare il messaggio di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="dab2a-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="dab2a-260">Ecco l'implementazione migliorata:</span><span class="sxs-lookup"><span data-stu-id="dab2a-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="dab2a-261">Si noti che il tipo restituito del metodo è ora **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="dab2a-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="dab2a-262">Tramite la restituzione di un **HttpResponseMessage** invece di un prodotto, possiamo controllare i dettagli del messaggio di risposta HTTP, tra cui il codice di stato e l'intestazione Location.</span><span class="sxs-lookup"><span data-stu-id="dab2a-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="dab2a-263">Il **CreateResponse** metodo crea un' **HttpResponseMessage** e automaticamente scrive una rappresentazione serializzata dell'oggetto prodotto nel corpo fo il messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="dab2a-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="dab2a-264">In questo esempio non convalida il `Product`.</span><span class="sxs-lookup"><span data-stu-id="dab2a-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="dab2a-265">Per informazioni sulla convalida del modello, vedere [convalida del modello in API Web ASP.NET](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="dab2a-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="dab2a-266">Aggiornamento di una risorsa</span><span class="sxs-lookup"><span data-stu-id="dab2a-266">Updating a Resource</span></span>

<span data-ttu-id="dab2a-267">L'aggiornamento di un prodotto di PUT è semplice:</span><span class="sxs-lookup"><span data-stu-id="dab2a-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="dab2a-268">Il nome del metodo inizia con &quot;inserito... &quot;, in modo che l'API Web corrisponde alle richieste PUT.</span><span class="sxs-lookup"><span data-stu-id="dab2a-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="dab2a-269">Il metodo accetta due parametri, l'ID prodotto e il prodotto aggiornato.</span><span class="sxs-lookup"><span data-stu-id="dab2a-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="dab2a-270">Il *id* parametro viene estratto dal percorso URI e il *prodotto* parametro viene deserializzato dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="dab2a-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="dab2a-271">Per impostazione predefinita, il framework API Web ASP.NET accetta i tipi di parametri semplici dalla route e i tipi complessi dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="dab2a-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="dab2a-272">L'eliminazione di una risorsa</span><span class="sxs-lookup"><span data-stu-id="dab2a-272">Deleting a Resource</span></span>

<span data-ttu-id="dab2a-273">Per eliminare una risorsa, definiscono un' "eliminazione in corso" metodo.</span><span class="sxs-lookup"><span data-stu-id="dab2a-273">To delete a resource, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="dab2a-274">Se una richiesta di eliminazione ha esito positivo, con un corpo di entità che descrive lo stato; può restituire lo stato 200 (OK) lo stato 202 (accettato) se l'eliminazione è ancora in sospeso; o di stato 204 (nessun contenuto) senza corpo entità.</span><span class="sxs-lookup"><span data-stu-id="dab2a-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="dab2a-275">In questo caso, il `DeleteProduct` metodo presenta un `void` tipo restituito, in modo che l'API Web ASP.NET converte automaticamente in stato codice 204 (nessun contenuto).</span><span class="sxs-lookup"><span data-stu-id="dab2a-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
