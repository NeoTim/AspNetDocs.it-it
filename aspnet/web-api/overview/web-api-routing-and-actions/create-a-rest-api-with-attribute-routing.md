---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Creare un'API REST con routing mediante attributi n API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: a58daa96410de734619bf65f84346137c7d3cf44
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59393301"
---
# <a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="05b41-102">Creare un'API REST con Routing degli attributi nell'API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="05b41-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>

<span data-ttu-id="05b41-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="05b41-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="05b41-104">API Web 2 supporta un nuovo tipo di routing, chiamato *routing con attributi*.</span><span class="sxs-lookup"><span data-stu-id="05b41-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="05b41-105">Per una panoramica generale di routing con attributi, vedere [Routing con attributi nell'API Web 2](attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="05b41-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="05b41-106">In questa esercitazione si userà il routing con attributi per creare un'API REST per una raccolta di documentazione.</span><span class="sxs-lookup"><span data-stu-id="05b41-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="05b41-107">L'API supporterà le azioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="05b41-107">The API will support the following actions:</span></span>

| <span data-ttu-id="05b41-108">Operazione</span><span class="sxs-lookup"><span data-stu-id="05b41-108">Action</span></span> | <span data-ttu-id="05b41-109">URI di esempio</span><span class="sxs-lookup"><span data-stu-id="05b41-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="05b41-110">Ottenere un elenco di tutti i libri.</span><span class="sxs-lookup"><span data-stu-id="05b41-110">Get a list of all books.</span></span> | <span data-ttu-id="05b41-111">/ api/documentazione</span><span class="sxs-lookup"><span data-stu-id="05b41-111">/api/books</span></span> |
| <span data-ttu-id="05b41-112">Ottenere un libro di ID.</span><span class="sxs-lookup"><span data-stu-id="05b41-112">Get a book by ID.</span></span> | <span data-ttu-id="05b41-113">/api/books/1</span><span class="sxs-lookup"><span data-stu-id="05b41-113">/api/books/1</span></span> |
| <span data-ttu-id="05b41-114">Ottenere i dettagli di un libro.</span><span class="sxs-lookup"><span data-stu-id="05b41-114">Get the details of a book.</span></span> | <span data-ttu-id="05b41-115">/API/books/1/Details</span><span class="sxs-lookup"><span data-stu-id="05b41-115">/api/books/1/details</span></span> |
| <span data-ttu-id="05b41-116">Ottenere un elenco di libri in base al genere.</span><span class="sxs-lookup"><span data-stu-id="05b41-116">Get a list of books by genre.</span></span> | <span data-ttu-id="05b41-117">/api/books/fantasy</span><span class="sxs-lookup"><span data-stu-id="05b41-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="05b41-118">Ottenere un elenco di libri per data di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="05b41-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="05b41-119">/API/books/date/2013-02-16 /api/books/date/2013/02/16 (formato alternativo)</span><span class="sxs-lookup"><span data-stu-id="05b41-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="05b41-120">Ottenere un elenco di libri di uno specifico autore.</span><span class="sxs-lookup"><span data-stu-id="05b41-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="05b41-121">/API/authors/1/books</span><span class="sxs-lookup"><span data-stu-id="05b41-121">/api/authors/1/books</span></span> |

<span data-ttu-id="05b41-122">Tutti i metodi sono di sola lettura (richieste HTTP GET).</span><span class="sxs-lookup"><span data-stu-id="05b41-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="05b41-123">Per il livello di dati, si userà Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="05b41-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="05b41-124">I record dei libri avrà i campi seguenti:</span><span class="sxs-lookup"><span data-stu-id="05b41-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="05b41-125">Id</span><span class="sxs-lookup"><span data-stu-id="05b41-125">ID</span></span>
- <span data-ttu-id="05b41-126">Titolo</span><span class="sxs-lookup"><span data-stu-id="05b41-126">Title</span></span>
- <span data-ttu-id="05b41-127">Genre</span><span class="sxs-lookup"><span data-stu-id="05b41-127">Genre</span></span>
- <span data-ttu-id="05b41-128">Data di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="05b41-128">Publication date</span></span>
- <span data-ttu-id="05b41-129">Prezzo</span><span class="sxs-lookup"><span data-stu-id="05b41-129">Price</span></span>
- <span data-ttu-id="05b41-130">Descrizione</span><span class="sxs-lookup"><span data-stu-id="05b41-130">Description</span></span>
- <span data-ttu-id="05b41-131">AuthorID (chiave esterna per una tabella Authors)</span><span class="sxs-lookup"><span data-stu-id="05b41-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="05b41-132">Per la maggior parte delle richieste, tuttavia, l'API restituirà un subset dei dati (titolo, autore e il genere).</span><span class="sxs-lookup"><span data-stu-id="05b41-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="05b41-133">Per ottenere il record completo, il client richieste `/api/books/{id}/details`.</span><span class="sxs-lookup"><span data-stu-id="05b41-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="05b41-134">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="05b41-134">Prerequisites</span></span>

<span data-ttu-id="05b41-135">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise edition.</span><span class="sxs-lookup"><span data-stu-id="05b41-135">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="05b41-136">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="05b41-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="05b41-137">Iniziare eseguendo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="05b41-137">Start by running Visual Studio.</span></span> <span data-ttu-id="05b41-138">Dal **File** dal menu **New** e quindi selezionare **progetto**.</span><span class="sxs-lookup"><span data-stu-id="05b41-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="05b41-139">Espandere la **Installed** > **Visual c#** categoria.</span><span class="sxs-lookup"><span data-stu-id="05b41-139">Expand the **Installed** > **Visual C#** category.</span></span> <span data-ttu-id="05b41-140">Sotto **Visual c#**, selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="05b41-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="05b41-141">Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="05b41-141">In the list of project templates, select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="05b41-142">Denominare il progetto &quot;BooksAPI&quot;.</span><span class="sxs-lookup"><span data-stu-id="05b41-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="05b41-143">Nel **nuova applicazione Web ASP.NET** finestra di dialogo, seleziona la **vuota** modello.</span><span class="sxs-lookup"><span data-stu-id="05b41-143">In the **New ASP.NET Web Application** dialog, select the **Empty** template.</span></span> <span data-ttu-id="05b41-144">In "Aggiungere cartelle e riferimenti per la funzionalità di base", selezionare la **API Web** casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="05b41-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="05b41-145">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="05b41-145">Click **OK**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="05b41-146">Verrà creato un progetto bozza configurato per la funzionalità dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="05b41-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="05b41-147">Modelli di dominio</span><span class="sxs-lookup"><span data-stu-id="05b41-147">Domain Models</span></span>

<span data-ttu-id="05b41-148">Successivamente, aggiungere le classi per i modelli di dominio.</span><span class="sxs-lookup"><span data-stu-id="05b41-148">Next, add classes for domain models.</span></span> <span data-ttu-id="05b41-149">In Esplora soluzioni fare clic sulla cartella Models.</span><span class="sxs-lookup"><span data-stu-id="05b41-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="05b41-150">Selezionare **Add**, quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="05b41-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="05b41-151">Assegnare alla classe il nome `Author`.</span><span class="sxs-lookup"><span data-stu-id="05b41-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="05b41-152">Sostituire il codice in Author.cs con gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="05b41-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="05b41-153">A questo punto aggiungere un'altra classe denominata `Book`.</span><span class="sxs-lookup"><span data-stu-id="05b41-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="05b41-154">Aggiungere un Controller API Web</span><span class="sxs-lookup"><span data-stu-id="05b41-154">Add a Web API Controller</span></span>

<span data-ttu-id="05b41-155">In questo passaggio si aggiungerà un controller API Web che usa Entity Framework come livello dati.</span><span class="sxs-lookup"><span data-stu-id="05b41-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="05b41-156">Premere CTRL+MAIUSC+B per compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="05b41-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="05b41-157">Entity Framework Usa la reflection per individuare le proprietà dei modelli, pertanto è necessario un assembly compilato creare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="05b41-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="05b41-158">In Esplora soluzioni fare clic sulla cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="05b41-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="05b41-159">Selezionare **Add**, quindi selezionare **Controller**.</span><span class="sxs-lookup"><span data-stu-id="05b41-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="05b41-160">Nel **Add Scaffold** finestra di dialogo, seleziona **Controller Web API 2 con azioni, che usa Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="05b41-160">In the **Add Scaffold** dialog, select **Web API 2 Controller with actions, using Entity Framework**.</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="05b41-161">Nel **Aggiungi Controller** finestra di dialogo per **nome del Controller**, immettere &quot;BooksController&quot;.</span><span class="sxs-lookup"><span data-stu-id="05b41-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="05b41-162">Selezionare il &quot;usare le azioni del controller asincrono&quot; casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="05b41-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="05b41-163">Per la **classe modello**, selezionare &quot;libro&quot;.</span><span class="sxs-lookup"><span data-stu-id="05b41-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="05b41-164">(Se non viene visualizzato il `Book` classe elencati nell'elenco a discesa, assicurarsi che è stato compilato il progetto.) Fare clic sul pulsante "+".</span><span class="sxs-lookup"><span data-stu-id="05b41-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="05b41-165">Fare clic su **Add** nel **nuovo contesto dati** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="05b41-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="05b41-166">Fare clic su **Add** nel **Aggiungi Controller** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="05b41-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="05b41-167">Lo scaffolding consente di aggiungere una classe denominata `BooksController` che definisce il controller API.</span><span class="sxs-lookup"><span data-stu-id="05b41-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="05b41-168">Aggiunge anche una classe denominata `BooksAPIContext` nella cartella Models, che definisce il contesto dei dati per Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="05b41-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="05b41-169">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="05b41-169">Seed the Database</span></span>

<span data-ttu-id="05b41-170">Dal menu Strumenti, selezionare **Gestione pacchetti NuGet**, quindi selezionare **la Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="05b41-170">From the Tools menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="05b41-171">Nella finestra della Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="05b41-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="05b41-172">Questo comando crea una cartella Migrations e aggiunge un nuovo file di codice Configuration.cs denominato.</span><span class="sxs-lookup"><span data-stu-id="05b41-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="05b41-173">Aprire il file e aggiungere il codice seguente per il `Configuration.Seed` (metodo).</span><span class="sxs-lookup"><span data-stu-id="05b41-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="05b41-174">Nella finestra della Console di gestione pacchetti, digitare i comandi seguenti.</span><span class="sxs-lookup"><span data-stu-id="05b41-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="05b41-175">Questi comandi creano un database locale e richiamano il metodo di inizializzazione per popolare il database.</span><span class="sxs-lookup"><span data-stu-id="05b41-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="05b41-176">Aggiungere le classi DTO</span><span class="sxs-lookup"><span data-stu-id="05b41-176">Add DTO Classes</span></span>

<span data-ttu-id="05b41-177">Se si esegue l'applicazione ora e inviarla una richiesta GET a /api/books/1, la risposta sarà simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="05b41-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="05b41-178">(Aggiunto rientro per una migliore leggibilità).</span><span class="sxs-lookup"><span data-stu-id="05b41-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="05b41-179">Invece, voglio che questa richiesta per restituire un subset dei campi.</span><span class="sxs-lookup"><span data-stu-id="05b41-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="05b41-180">Inoltre, voglio che venga restituito il nome dell'autore, anziché l'ID autore.</span><span class="sxs-lookup"><span data-stu-id="05b41-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="05b41-181">A tale scopo, questo punto, modificheremo i metodi del controller per restituire un *oggetto di trasferimento dati* (DTO) anziché il modello di EF.</span><span class="sxs-lookup"><span data-stu-id="05b41-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="05b41-182">Un oggetto DTO è un oggetto che può trasportare solo i dati.</span><span class="sxs-lookup"><span data-stu-id="05b41-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="05b41-183">In Esplora soluzioni fare clic sul progetto e selezionare **Add** | **nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="05b41-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="05b41-184">Denominare la cartella &quot;DTO&quot;.</span><span class="sxs-lookup"><span data-stu-id="05b41-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="05b41-185">Aggiungere una classe denominata `BookDto` nella cartella oggetti DTO, con la definizione seguente:</span><span class="sxs-lookup"><span data-stu-id="05b41-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="05b41-186">Aggiungere un'altra classe denominata `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="05b41-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="05b41-187">A questo punto, aggiornare il `BooksController` classe per restituire `BookDto` istanze.</span><span class="sxs-lookup"><span data-stu-id="05b41-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="05b41-188">Si userà il [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) metodo al progetto `Book` alle istanze `BookDto` istanze.</span><span class="sxs-lookup"><span data-stu-id="05b41-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="05b41-189">Ecco il codice aggiornato per la classe controller.</span><span class="sxs-lookup"><span data-stu-id="05b41-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="05b41-190">Dopo aver eliminato il `PutBook`, `PostBook`, e `DeleteBook` metodi, perché non sono necessari per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="05b41-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>


<span data-ttu-id="05b41-191">Ora se si esegue l'applicazione e richiedere /api/books/1, il corpo della risposta dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="05b41-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="05b41-192">Aggiungere gli attributi di Route</span><span class="sxs-lookup"><span data-stu-id="05b41-192">Add Route Attributes</span></span>

<span data-ttu-id="05b41-193">Successivamente, verrà convertito il controller per usare il routing con attributi.</span><span class="sxs-lookup"><span data-stu-id="05b41-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="05b41-194">Aggiungere prima di tutto una **RoutePrefix** attributo al controller.</span><span class="sxs-lookup"><span data-stu-id="05b41-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="05b41-195">Questo attributo definisce i segmenti URI iniziali per tutti i metodi nel controller.</span><span class="sxs-lookup"><span data-stu-id="05b41-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="05b41-196">Aggiungere quindi **[Route]** attributi per le azioni del controller, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="05b41-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="05b41-197">Il modello di route per ogni metodo del controller è il prefisso più la stringa specificata nel **Route** attributo.</span><span class="sxs-lookup"><span data-stu-id="05b41-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="05b41-198">Per il `GetBook` metodo, il modello di route include la stringa con parametri &quot;{id: int}&quot;, che corrisponde a se il segmento URI contiene un valore intero.</span><span class="sxs-lookup"><span data-stu-id="05b41-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="05b41-199">Metodo</span><span class="sxs-lookup"><span data-stu-id="05b41-199">Method</span></span> | <span data-ttu-id="05b41-200">Modello di route</span><span class="sxs-lookup"><span data-stu-id="05b41-200">Route Template</span></span> | <span data-ttu-id="05b41-201">URI di esempio</span><span class="sxs-lookup"><span data-stu-id="05b41-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="05b41-202">"api/documentazione"</span><span class="sxs-lookup"><span data-stu-id="05b41-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="05b41-203">"api/books/{id:int}"</span><span class="sxs-lookup"><span data-stu-id="05b41-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="05b41-204">Ottenere i dettagli della Rubrica</span><span class="sxs-lookup"><span data-stu-id="05b41-204">Get Book Details</span></span>

<span data-ttu-id="05b41-205">Per ottenere i dettagli della Rubrica, il client invia una richiesta GET a `/api/books/{id}/details`, dove *{id}* è l'ID del libro.</span><span class="sxs-lookup"><span data-stu-id="05b41-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="05b41-206">Aggiungere il metodo seguente alla classe `BooksController` .</span><span class="sxs-lookup"><span data-stu-id="05b41-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="05b41-207">Se si richiedono `/api/books/1/details`, la risposta sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="05b41-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="05b41-208">Ottieni documentazione in base al genere</span><span class="sxs-lookup"><span data-stu-id="05b41-208">Get Books By Genre</span></span>

<span data-ttu-id="05b41-209">Per ottenere un elenco di libri in genere specifico, il client invia una richiesta GET a `/api/books/genre`, dove *genre* è il nome del genere.</span><span class="sxs-lookup"><span data-stu-id="05b41-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="05b41-210">ad esempio `/api/books/fantasy`.</span><span class="sxs-lookup"><span data-stu-id="05b41-210">(For example, `/api/books/fantasy`.)</span></span>

<span data-ttu-id="05b41-211">Aggiungere il metodo seguente alla `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="05b41-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="05b41-212">Di seguito viene definita una route che contiene un parametro di {genre} nel modello URI.</span><span class="sxs-lookup"><span data-stu-id="05b41-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="05b41-213">Si noti che l'API Web è in grado di distinguere questi due URI e indirizzarle a diversi metodi:</span><span class="sxs-lookup"><span data-stu-id="05b41-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="05b41-214">Infatti il `GetBook` metodo include un vincolo che il segmento "id" deve essere un valore integer:</span><span class="sxs-lookup"><span data-stu-id="05b41-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="05b41-215">Se si richiede /api/books/fantasy, la risposta sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="05b41-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="05b41-216">Ottieni documentazione dall'autore</span><span class="sxs-lookup"><span data-stu-id="05b41-216">Get Books By Author</span></span>

<span data-ttu-id="05b41-217">Per ottenere un elenco di una documentazione per un determinato autore, il client invia una richiesta GET a `/api/authors/id/books`, dove *id* è l'ID dell'autore.</span><span class="sxs-lookup"><span data-stu-id="05b41-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="05b41-218">Aggiungere il metodo seguente alla `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="05b41-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="05b41-219">In questo esempio è interessante perché &quot;libri&quot; è considerato una risorsa figlio di &quot;autori&quot;.</span><span class="sxs-lookup"><span data-stu-id="05b41-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="05b41-220">Questo modello è piuttosto comune per le API RESTful.</span><span class="sxs-lookup"><span data-stu-id="05b41-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="05b41-221">La tilde (~) nel modello di route esegue l'override di prefisso della route nel **RoutePrefix** attributo.</span><span class="sxs-lookup"><span data-stu-id="05b41-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="05b41-222">Ottieni documentazione dalla data di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="05b41-222">Get Books By Publication Date</span></span>

<span data-ttu-id="05b41-223">Per ottenere un elenco di libri per data di pubblicazione, il client invia una richiesta GET a `/api/books/date/yyyy-mm-dd`, dove *aaaa-mm-gg* è la data.</span><span class="sxs-lookup"><span data-stu-id="05b41-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="05b41-224">Ecco un modo per eseguire questa operazione:</span><span class="sxs-lookup"><span data-stu-id="05b41-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="05b41-225">Il `{pubdate:datetime}` parametro è vincolato in modo che corrisponda un **DateTime** valore.</span><span class="sxs-lookup"><span data-stu-id="05b41-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="05b41-226">Questo procedimento funziona, ma è effettivamente più permissivo ci piacerebbe.</span><span class="sxs-lookup"><span data-stu-id="05b41-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="05b41-227">Ad esempio, questi URI corrisponderà anche la route:</span><span class="sxs-lookup"><span data-stu-id="05b41-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="05b41-228">Non è niente di sbagliato consentire gli URI.</span><span class="sxs-lookup"><span data-stu-id="05b41-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="05b41-229">Tuttavia, è possibile limitare la route in un formato specifico mediante l'aggiunta di un vincolo di espressione regolare per il modello di route:</span><span class="sxs-lookup"><span data-stu-id="05b41-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="05b41-230">Attualmente solo le date nel formato &quot;aaaa-mm-gg&quot; corrisponderanno.</span><span class="sxs-lookup"><span data-stu-id="05b41-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="05b41-231">Si noti che non utilizziamo l'espressione regolare per convalidare che abbiamo una data reale.</span><span class="sxs-lookup"><span data-stu-id="05b41-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="05b41-232">Quando si tenta di convertire il segmento URI in API Web, che viene gestito un **DateTime** istanza.</span><span class="sxs-lookup"><span data-stu-id="05b41-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="05b41-233">Una data non valida, ad esempio ' 2012-47-99 avrà esito negativo da convertire e il client otterrà un errore 404.</span><span class="sxs-lookup"><span data-stu-id="05b41-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="05b41-234">È anche possibile supportare un separatore di barra (`/api/books/date/yyyy/mm/dd`) tramite l'aggiunta di un altro **[Route]** attributo con un'altra espressione regolare.</span><span class="sxs-lookup"><span data-stu-id="05b41-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="05b41-235">Sono un sottile ma importante dettaglio di seguito.</span><span class="sxs-lookup"><span data-stu-id="05b41-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="05b41-236">Il secondo modello di route contiene un carattere jolly (\*) all'inizio del parametro {pubdate}:</span><span class="sxs-lookup"><span data-stu-id="05b41-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

<span data-ttu-id="05b41-237">In questo modo il motore di routing che {pubdate} deve corrispondere il resto dell'URI.</span><span class="sxs-lookup"><span data-stu-id="05b41-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="05b41-238">Per impostazione predefinita, un parametro di modello corrisponde a un singolo segmento URI.</span><span class="sxs-lookup"><span data-stu-id="05b41-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="05b41-239">In questo caso, si desidera {pubdate} estendersi su più segmenti URI:</span><span class="sxs-lookup"><span data-stu-id="05b41-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="05b41-240">Codice del controller</span><span class="sxs-lookup"><span data-stu-id="05b41-240">Controller Code</span></span>

<span data-ttu-id="05b41-241">Ecco il codice completo per la classe BooksController.</span><span class="sxs-lookup"><span data-stu-id="05b41-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="05b41-242">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="05b41-242">Summary</span></span>

<span data-ttu-id="05b41-243">Routing con attributi offre maggiore controllo e una maggiore flessibilità quando si progettano gli URI per l'API.</span><span class="sxs-lookup"><span data-stu-id="05b41-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
