---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Creare un'API REST con routing degli attributi in API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 6eac36767bf34857d5341188d0653e7fec7cade2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "86188850"
---
# <a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="25104-102">Creare un'API REST con routing degli attributi in API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="25104-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>

<span data-ttu-id="25104-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="25104-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="25104-104">L'API Web 2 supporta un nuovo tipo di routing, denominato *routing degli attributi*.</span><span class="sxs-lookup"><span data-stu-id="25104-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="25104-105">Per una panoramica generale del routing degli attributi, vedere [routing degli attributi nell'API Web 2](attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="25104-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="25104-106">In questa esercitazione si userà il routing degli attributi per creare un'API REST per una raccolta di libri.</span><span class="sxs-lookup"><span data-stu-id="25104-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="25104-107">L'API supporterà le azioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="25104-107">The API will support the following actions:</span></span>

| <span data-ttu-id="25104-108">Azione</span><span class="sxs-lookup"><span data-stu-id="25104-108">Action</span></span> | <span data-ttu-id="25104-109">URI di esempio</span><span class="sxs-lookup"><span data-stu-id="25104-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="25104-110">Ottenere un elenco di tutti i libri.</span><span class="sxs-lookup"><span data-stu-id="25104-110">Get a list of all books.</span></span> | <span data-ttu-id="25104-111">/api/books</span><span class="sxs-lookup"><span data-stu-id="25104-111">/api/books</span></span> |
| <span data-ttu-id="25104-112">Ottenere un libro in base all'ID.</span><span class="sxs-lookup"><span data-stu-id="25104-112">Get a book by ID.</span></span> | <span data-ttu-id="25104-113">/api/books/1</span><span class="sxs-lookup"><span data-stu-id="25104-113">/api/books/1</span></span> |
| <span data-ttu-id="25104-114">Ottenere i dettagli di un libro.</span><span class="sxs-lookup"><span data-stu-id="25104-114">Get the details of a book.</span></span> | <span data-ttu-id="25104-115">/api/books/1/details</span><span class="sxs-lookup"><span data-stu-id="25104-115">/api/books/1/details</span></span> |
| <span data-ttu-id="25104-116">Ottenere un elenco di libri per genere.</span><span class="sxs-lookup"><span data-stu-id="25104-116">Get a list of books by genre.</span></span> | <span data-ttu-id="25104-117">/api/books/fantasy</span><span class="sxs-lookup"><span data-stu-id="25104-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="25104-118">Ottenere un elenco di libri in base alla data di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="25104-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="25104-119">/API/Books/date/2013-02-16/API/Books/date/2013/02/16 (form alternativo)</span><span class="sxs-lookup"><span data-stu-id="25104-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="25104-120">Ottenere un elenco di libri da un particolare autore.</span><span class="sxs-lookup"><span data-stu-id="25104-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="25104-121">/api/authors/1/books</span><span class="sxs-lookup"><span data-stu-id="25104-121">/api/authors/1/books</span></span> |

<span data-ttu-id="25104-122">Tutti i metodi sono di sola lettura (richieste HTTP GET).</span><span class="sxs-lookup"><span data-stu-id="25104-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="25104-123">Per il livello dati verrà usato Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="25104-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="25104-124">I record di libri avranno i seguenti campi:</span><span class="sxs-lookup"><span data-stu-id="25104-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="25104-125">ID</span><span class="sxs-lookup"><span data-stu-id="25104-125">ID</span></span>
- <span data-ttu-id="25104-126">Titolo</span><span class="sxs-lookup"><span data-stu-id="25104-126">Title</span></span>
- <span data-ttu-id="25104-127">Genre</span><span class="sxs-lookup"><span data-stu-id="25104-127">Genre</span></span>
- <span data-ttu-id="25104-128">Data di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="25104-128">Publication date</span></span>
- <span data-ttu-id="25104-129">Prezzo</span><span class="sxs-lookup"><span data-stu-id="25104-129">Price</span></span>
- <span data-ttu-id="25104-130">Descrizione</span><span class="sxs-lookup"><span data-stu-id="25104-130">Description</span></span>
- <span data-ttu-id="25104-131">Autorizzazione (chiave esterna per una tabella authors)</span><span class="sxs-lookup"><span data-stu-id="25104-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="25104-132">Per la maggior parte delle richieste, tuttavia, l'API restituirà un subset di questi dati (titolo, autore e genere).</span><span class="sxs-lookup"><span data-stu-id="25104-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="25104-133">Per ottenere il record completo, il client richiede `/api/books/{id}/details` .</span><span class="sxs-lookup"><span data-stu-id="25104-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25104-134">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="25104-134">Prerequisites</span></span>

<span data-ttu-id="25104-135">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise Edition.</span><span class="sxs-lookup"><span data-stu-id="25104-135">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="25104-136">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="25104-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="25104-137">Per iniziare, esegui Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25104-137">Start by running Visual Studio.</span></span> <span data-ttu-id="25104-138">Scegliere **Nuovo** dal menu **File** e quindi selezionare **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="25104-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="25104-139">Espandere la categoria **installato**di  >  **Visual C#** .</span><span class="sxs-lookup"><span data-stu-id="25104-139">Expand the **Installed** > **Visual C#** category.</span></span> <span data-ttu-id="25104-140">In **Visual C#** selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="25104-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="25104-141">Nell'elenco dei modelli di progetto selezionare **applicazione Web ASP.NET (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="25104-141">In the list of project templates, select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="25104-142">Denominare il progetto &quot; BooksAPI &quot; .</span><span class="sxs-lookup"><span data-stu-id="25104-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="25104-143">Nella finestra di dialogo **nuova applicazione Web ASP.NET** selezionare il modello **vuoto** .</span><span class="sxs-lookup"><span data-stu-id="25104-143">In the **New ASP.NET Web Application** dialog, select the **Empty** template.</span></span> <span data-ttu-id="25104-144">In "Aggiungi cartelle e riferimenti principali per" selezionare la casella di controllo **API Web** .</span><span class="sxs-lookup"><span data-stu-id="25104-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="25104-145">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="25104-145">Click **OK**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="25104-146">Viene creato un progetto Skeleton configurato per le funzionalità dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="25104-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="25104-147">Modelli di dominio</span><span class="sxs-lookup"><span data-stu-id="25104-147">Domain Models</span></span>

<span data-ttu-id="25104-148">Aggiungere quindi le classi per i modelli di dominio.</span><span class="sxs-lookup"><span data-stu-id="25104-148">Next, add classes for domain models.</span></span> <span data-ttu-id="25104-149">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Modelli.</span><span class="sxs-lookup"><span data-stu-id="25104-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="25104-150">Selezionare **Aggiungi**e quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="25104-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="25104-151">Denominare la classe `Author`.</span><span class="sxs-lookup"><span data-stu-id="25104-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="25104-152">Sostituire il codice in Author.cs con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="25104-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="25104-153">Aggiungere ora un'altra classe denominata `Book` .</span><span class="sxs-lookup"><span data-stu-id="25104-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="25104-154">Aggiungere un controller API Web</span><span class="sxs-lookup"><span data-stu-id="25104-154">Add a Web API Controller</span></span>

<span data-ttu-id="25104-155">In questo passaggio si aggiungerà un controller API Web che usa Entity Framework come livello dati.</span><span class="sxs-lookup"><span data-stu-id="25104-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="25104-156">Premere CTRL+MAIUSC+B per compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="25104-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="25104-157">Entity Framework usa la reflection per individuare le proprietà dei modelli, quindi richiede un assembly compilato per creare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="25104-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="25104-158">In Esplora soluzioni fare clic sulla cartella Controller.</span><span class="sxs-lookup"><span data-stu-id="25104-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="25104-159">Selezionare **Aggiungi**, quindi **controller**.</span><span class="sxs-lookup"><span data-stu-id="25104-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="25104-160">Nella finestra di dialogo **Aggiungi impalcatura** selezionare **Controller Web API 2 con azioni, usando Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="25104-160">In the **Add Scaffold** dialog, select **Web API 2 Controller with actions, using Entity Framework**.</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="25104-161">Nella finestra di dialogo **Aggiungi controller** , per **nome controller**, &quot; immettere &quot; BooksController.</span><span class="sxs-lookup"><span data-stu-id="25104-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="25104-162">Selezionare la &quot; casella di controllo Use Async controller Actions &quot; .</span><span class="sxs-lookup"><span data-stu-id="25104-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="25104-163">Per **classe modello**selezionare &quot; libro &quot; .</span><span class="sxs-lookup"><span data-stu-id="25104-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="25104-164">Se non viene visualizzata la `Book` classe elencata nell'elenco a discesa, assicurarsi di aver compilato il progetto. Fare quindi clic sul pulsante "+".</span><span class="sxs-lookup"><span data-stu-id="25104-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="25104-165">Fare clic su **Aggiungi** nella finestra di dialogo **nuovo contesto dati** .</span><span class="sxs-lookup"><span data-stu-id="25104-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="25104-166">Fare clic su **Aggiungi** nella finestra di dialogo **Aggiungi controller** .</span><span class="sxs-lookup"><span data-stu-id="25104-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="25104-167">L'impalcatura aggiunge una classe denominata `BooksController` che definisce il controller API.</span><span class="sxs-lookup"><span data-stu-id="25104-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="25104-168">Viene inoltre aggiunta una classe denominata `BooksAPIContext` nella cartella Models, che definisce il contesto dei dati per Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="25104-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="25104-169">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="25104-169">Seed the Database</span></span>

<span data-ttu-id="25104-170">Dal menu Strumenti selezionare **Gestione pacchetti NuGet**, quindi selezionare **console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="25104-170">From the Tools menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="25104-171">Nella finestra Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="25104-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="25104-172">Questo comando crea una cartella Migrations e aggiunge un nuovo file di codice denominato Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="25104-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="25104-173">Aprire il file e aggiungere il codice seguente al `Configuration.Seed` metodo.</span><span class="sxs-lookup"><span data-stu-id="25104-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="25104-174">Nella finestra console di gestione pacchetti digitare i comandi seguenti.</span><span class="sxs-lookup"><span data-stu-id="25104-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="25104-175">Questi comandi creano un database locale e richiamano il metodo Seed per popolare il database.</span><span class="sxs-lookup"><span data-stu-id="25104-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="25104-176">Aggiungi classi DTO</span><span class="sxs-lookup"><span data-stu-id="25104-176">Add DTO Classes</span></span>

<span data-ttu-id="25104-177">Se si esegue ora l'applicazione e si invia una richiesta GET a/API/Books/1, la risposta sarà simile alla seguente.</span><span class="sxs-lookup"><span data-stu-id="25104-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="25104-178">(Il rientro è stato aggiunto per migliorare la leggibilità).</span><span class="sxs-lookup"><span data-stu-id="25104-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="25104-179">Al contrario, desidero che questa richiesta restituisca un subset dei campi.</span><span class="sxs-lookup"><span data-stu-id="25104-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="25104-180">Desidero anche che restituisca il nome dell'autore, anziché l'ID autore.</span><span class="sxs-lookup"><span data-stu-id="25104-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="25104-181">A tale scopo, i metodi controller verranno modificati in modo da restituire un oggetto DTO ( *Data Transfer Object* ) invece del modello EF.</span><span class="sxs-lookup"><span data-stu-id="25104-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="25104-182">Un DTO è un oggetto progettato solo per il trasporto di dati.</span><span class="sxs-lookup"><span data-stu-id="25104-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="25104-183">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi**  |  **nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="25104-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="25104-184">Denominare la cartella &quot; dto &quot; .</span><span class="sxs-lookup"><span data-stu-id="25104-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="25104-185">Aggiungere una classe denominata `BookDto` alla cartella dto con la definizione seguente:</span><span class="sxs-lookup"><span data-stu-id="25104-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="25104-186">Aggiungere un'altra classe denominata `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="25104-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="25104-187">Aggiornare quindi la `BooksController` classe per restituire le `BookDto` istanze.</span><span class="sxs-lookup"><span data-stu-id="25104-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="25104-188">Verrà usato il metodo [Queryable. Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) per proiettare le `Book` istanze nelle `BookDto` istanze.</span><span class="sxs-lookup"><span data-stu-id="25104-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="25104-189">Ecco il codice aggiornato per la classe controller.</span><span class="sxs-lookup"><span data-stu-id="25104-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="25104-190">I `PutBook` metodi, e sono stati eliminati `PostBook` `DeleteBook` perché non sono necessari per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="25104-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>

<span data-ttu-id="25104-191">A questo punto, se si esegue l'applicazione e si richiede/API/Books/1, il corpo della risposta sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="25104-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="25104-192">Aggiungere gli attributi di route</span><span class="sxs-lookup"><span data-stu-id="25104-192">Add Route Attributes</span></span>

<span data-ttu-id="25104-193">Si convertirà quindi il controller in modo da usare il routing degli attributi.</span><span class="sxs-lookup"><span data-stu-id="25104-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="25104-194">In primo luogo, aggiungere un attributo **RoutePrefix** al controller.</span><span class="sxs-lookup"><span data-stu-id="25104-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="25104-195">Questo attributo definisce i segmenti URI iniziali per tutti i metodi del controller.</span><span class="sxs-lookup"><span data-stu-id="25104-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="25104-196">Aggiungere quindi gli attributi **[route]** alle azioni del controller, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="25104-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="25104-197">Il modello di route per ogni metodo controller è il prefisso più la stringa specificata nell'attributo **Route** .</span><span class="sxs-lookup"><span data-stu-id="25104-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="25104-198">Per il `GetBook` metodo, il modello di route include la stringa con parametri &quot; {ID: int} &quot; , che corrisponde a se il segmento URI contiene un valore integer.</span><span class="sxs-lookup"><span data-stu-id="25104-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="25104-199">Metodo</span><span class="sxs-lookup"><span data-stu-id="25104-199">Method</span></span> | <span data-ttu-id="25104-200">Modello di route</span><span class="sxs-lookup"><span data-stu-id="25104-200">Route Template</span></span> | <span data-ttu-id="25104-201">URI di esempio</span><span class="sxs-lookup"><span data-stu-id="25104-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="25104-202">"API/libri"</span><span class="sxs-lookup"><span data-stu-id="25104-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="25104-203">"API/Books/{ID: int}"</span><span class="sxs-lookup"><span data-stu-id="25104-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="25104-204">Ottenere i dettagli del libro</span><span class="sxs-lookup"><span data-stu-id="25104-204">Get Book Details</span></span>

<span data-ttu-id="25104-205">Per ottenere i dettagli del libro, il client invierà una richiesta GET a `/api/books/{id}/details` , dove *{ID}* è l'ID del libro.</span><span class="sxs-lookup"><span data-stu-id="25104-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="25104-206">Aggiungere il metodo seguente alla classe `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="25104-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="25104-207">Se si richiede `/api/books/1/details` , la risposta ha un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="25104-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="25104-208">Ottenere libri per genere</span><span class="sxs-lookup"><span data-stu-id="25104-208">Get Books By Genre</span></span>

<span data-ttu-id="25104-209">Per ottenere un elenco di libri in un genere specifico, il client invierà una richiesta GET a `/api/books/genre` , dove *genre* è il nome del genere.</span><span class="sxs-lookup"><span data-stu-id="25104-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="25104-210">ad esempio `/api/books/fantasy`.</span><span class="sxs-lookup"><span data-stu-id="25104-210">(For example, `/api/books/fantasy`.)</span></span>

<span data-ttu-id="25104-211">Aggiungere il metodo seguente a `BooksController` .</span><span class="sxs-lookup"><span data-stu-id="25104-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="25104-212">Qui viene definita una route che contiene un parametro {genre} nel modello URI.</span><span class="sxs-lookup"><span data-stu-id="25104-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="25104-213">Si noti che l'API Web è in grado di distinguere questi due URI e indirizzarli a metodi diversi:</span><span class="sxs-lookup"><span data-stu-id="25104-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="25104-214">Questo perché il `GetBook` metodo include un vincolo che il segmento "ID" deve essere un valore integer:</span><span class="sxs-lookup"><span data-stu-id="25104-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="25104-215">Se si richiede/API/Books/Fantasy, la risposta ha un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="25104-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="25104-216">Ottenere libri per autore</span><span class="sxs-lookup"><span data-stu-id="25104-216">Get Books By Author</span></span>

<span data-ttu-id="25104-217">Per ottenere un elenco di libri per uno specifico autore, il client invierà una richiesta GET a `/api/authors/id/books` , dove *ID* è l'ID dell'autore.</span><span class="sxs-lookup"><span data-stu-id="25104-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="25104-218">Aggiungere il metodo seguente a `BooksController` .</span><span class="sxs-lookup"><span data-stu-id="25104-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="25104-219">Questo esempio è interessante perché &quot; &quot; i libri sono trattati come una risorsa figlio degli &quot; autori &quot; .</span><span class="sxs-lookup"><span data-stu-id="25104-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="25104-220">Questo modello è piuttosto comune nelle API RESTful.</span><span class="sxs-lookup"><span data-stu-id="25104-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="25104-221">La tilde (~) nel modello di route sostituisce il prefisso della route nell'attributo **RoutePrefix** .</span><span class="sxs-lookup"><span data-stu-id="25104-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="25104-222">Ottenere i libri in base alla data di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="25104-222">Get Books By Publication Date</span></span>

<span data-ttu-id="25104-223">Per ottenere un elenco di libri in base alla data di pubblicazione, il client invierà una richiesta GET a `/api/books/date/yyyy-mm-dd` , dove *aaaa-mm-gg* è la data.</span><span class="sxs-lookup"><span data-stu-id="25104-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="25104-224">Ecco un modo per eseguire questa operazione:</span><span class="sxs-lookup"><span data-stu-id="25104-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="25104-225">Il `{pubdate:datetime}` parametro è vincolato in modo da corrispondere a un valore **DateTime** .</span><span class="sxs-lookup"><span data-stu-id="25104-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="25104-226">Questa operazione funziona, ma è effettivamente più permissiva di quanto si desideri.</span><span class="sxs-lookup"><span data-stu-id="25104-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="25104-227">Ad esempio, questi URI corrisponderanno anche alla route:</span><span class="sxs-lookup"><span data-stu-id="25104-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="25104-228">Non c'è niente di sbagliato per consentire questi URI.</span><span class="sxs-lookup"><span data-stu-id="25104-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="25104-229">Tuttavia, è possibile limitare la route a un particolare formato aggiungendo un vincolo di espressione regolare al modello di route:</span><span class="sxs-lookup"><span data-stu-id="25104-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="25104-230">A questo punto, solo le date nel formato &quot; aaaa-mm-gg &quot; corrisponderanno.</span><span class="sxs-lookup"><span data-stu-id="25104-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="25104-231">Si noti che non viene usata l'espressione regolare per verificare che sia stata ottenuta una data reale.</span><span class="sxs-lookup"><span data-stu-id="25104-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="25104-232">Gestito quando l'API Web tenta di convertire il segmento URI in un'istanza **DateTime** .</span><span class="sxs-lookup"><span data-stu-id="25104-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="25104-233">Una data non valida, ad esempio "2012-47-99", non verrà convertita e il client riceverà un errore 404.</span><span class="sxs-lookup"><span data-stu-id="25104-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="25104-234">È anche possibile supportare un separatore di barra ( `/api/books/date/yyyy/mm/dd` ) aggiungendo un altro attributo **[route]** con un'espressione regolare diversa.</span><span class="sxs-lookup"><span data-stu-id="25104-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="25104-235">Qui è disponibile un sottile ma importante dettaglio.</span><span class="sxs-lookup"><span data-stu-id="25104-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="25104-236">Il secondo modello di route ha un carattere jolly ( \* ) all'inizio del parametro {pubDate}:</span><span class="sxs-lookup"><span data-stu-id="25104-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

<span data-ttu-id="25104-237">Indica al motore di routing che {pubdate} deve corrispondere al resto dell'URI.</span><span class="sxs-lookup"><span data-stu-id="25104-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="25104-238">Per impostazione predefinita, un parametro di modello corrisponde a un singolo segmento URI.</span><span class="sxs-lookup"><span data-stu-id="25104-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="25104-239">In questo caso, è necessario che {pubdate} estenda diversi segmenti URI:</span><span class="sxs-lookup"><span data-stu-id="25104-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="25104-240">Codice controller</span><span class="sxs-lookup"><span data-stu-id="25104-240">Controller Code</span></span>

<span data-ttu-id="25104-241">Ecco il codice completo per la classe BooksController.</span><span class="sxs-lookup"><span data-stu-id="25104-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="25104-242">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="25104-242">Summary</span></span>

<span data-ttu-id="25104-243">Il routing degli attributi offre maggiore controllo e maggiore flessibilità durante la progettazione degli URI per l'API.</span><span class="sxs-lookup"><span data-stu-id="25104-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
