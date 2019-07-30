---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Usare migrazioni Code First per effettuare il seeding del Database | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 257bd06848adb949330856cc71eeb3d685e9d036
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59421667"
---
# <a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="4a770-102">Usare migrazioni Code First per effettuare il seeding del Database</span><span class="sxs-lookup"><span data-stu-id="4a770-102">Use Code First Migrations to Seed the Database</span></span>

<span data-ttu-id="4a770-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4a770-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="4a770-104">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="4a770-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="4a770-105">In questa sezione si userà [migrazioni Code First](https://msdn.microsoft.com/data/jj591621) in Entity Framework per effettuare il seeding del database con dati di test.</span><span class="sxs-lookup"><span data-stu-id="4a770-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="4a770-106">Dal **degli strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="4a770-106">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="4a770-107">Nella finestra della Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="4a770-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="4a770-108">Questo comando aggiunge una cartella denominata migrazioni al progetto, oltre a un file di codice denominato Configuration.cs nella cartella migrazioni.</span><span class="sxs-lookup"><span data-stu-id="4a770-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="4a770-109">Aprire il file Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="4a770-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="4a770-110">Aggiungere la seguente istruzione **using**.</span><span class="sxs-lookup"><span data-stu-id="4a770-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="4a770-111">Quindi aggiungere il codice seguente per il **Configuration.Seed** metodo:</span><span class="sxs-lookup"><span data-stu-id="4a770-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="4a770-112">Nella finestra della Console di gestione pacchetti, digitare i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="4a770-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="4a770-113">Il primo comando genera codice che crea il database e il secondo comando esegue tale codice.</span><span class="sxs-lookup"><span data-stu-id="4a770-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="4a770-114">Il database viene creato in locale, usando [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span><span class="sxs-lookup"><span data-stu-id="4a770-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="4a770-115">Esplorare l'API (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="4a770-115">Explore the API (Optional)</span></span>

<span data-ttu-id="4a770-116">Premere F5 per eseguire l'applicazione in modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="4a770-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="4a770-117">Visual Studio avvia IIS Express ed esegue l'app web.</span><span class="sxs-lookup"><span data-stu-id="4a770-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="4a770-118">Quindi, Visual Studio avvia un browser e verrà visualizzata la home page dell'app.</span><span class="sxs-lookup"><span data-stu-id="4a770-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="4a770-119">Quando Visual Studio viene eseguito un progetto web, assegna un numero di porta.</span><span class="sxs-lookup"><span data-stu-id="4a770-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="4a770-120">Nell'immagine seguente, il numero di porta è 50524.</span><span class="sxs-lookup"><span data-stu-id="4a770-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="4a770-121">Quando si esegue l'applicazione, si noterà un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="4a770-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="4a770-122">Home page viene implementata tramite ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4a770-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="4a770-123">Nella parte superiore della pagina, è presente un collegamento con la dicitura "API".</span><span class="sxs-lookup"><span data-stu-id="4a770-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="4a770-124">Questo collegamento consente di accedere a una pagina della Guida generata automaticamente per l'API web.</span><span class="sxs-lookup"><span data-stu-id="4a770-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="4a770-125">(Per altre modalità di generazione di questa pagina della Guida e come è possibile aggiungere il proprio documentazione alla pagina, vedere [creazione di pagine della Guida per l'API Web ASP.NET](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) È possibile fare clic su Guida di collegamenti della pagina per visualizzare informazioni dettagliate sulle API, incluso il formato della richiesta e risposta.</span><span class="sxs-lookup"><span data-stu-id="4a770-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="4a770-126">L'API consente le operazioni CRUD sul database.</span><span class="sxs-lookup"><span data-stu-id="4a770-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="4a770-127">Di seguito sono riepilogate le API.</span><span class="sxs-lookup"><span data-stu-id="4a770-127">The following summarizes the API.</span></span>

| <span data-ttu-id="4a770-128">Autori</span><span class="sxs-lookup"><span data-stu-id="4a770-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="4a770-129">GET api/authors</span><span class="sxs-lookup"><span data-stu-id="4a770-129">GET api/authors</span></span> | <span data-ttu-id="4a770-130">Ottenere tutti gli autori.</span><span class="sxs-lookup"><span data-stu-id="4a770-130">Get all authors.</span></span> |
| <span data-ttu-id="4a770-131">GET api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="4a770-131">GET api/authors/{id}</span></span> | <span data-ttu-id="4a770-132">Ottenere un autore per ID.</span><span class="sxs-lookup"><span data-stu-id="4a770-132">Get an author by ID.</span></span> |
| <span data-ttu-id="4a770-133">POST /api/authors</span><span class="sxs-lookup"><span data-stu-id="4a770-133">POST /api/authors</span></span> | <span data-ttu-id="4a770-134">Creare un nuovo autore.</span><span class="sxs-lookup"><span data-stu-id="4a770-134">Create a new author.</span></span> |
| <span data-ttu-id="4a770-135">PUT /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="4a770-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="4a770-136">Aggiornare un autore esistente.</span><span class="sxs-lookup"><span data-stu-id="4a770-136">Update an existing author.</span></span> |
| <span data-ttu-id="4a770-137">DELETE /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="4a770-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="4a770-138">Eliminare un autore.</span><span class="sxs-lookup"><span data-stu-id="4a770-138">Delete an author.</span></span> |

| <span data-ttu-id="4a770-139">Libri</span><span class="sxs-lookup"><span data-stu-id="4a770-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="4a770-140">GET /api/books</span><span class="sxs-lookup"><span data-stu-id="4a770-140">GET /api/books</span></span> | <span data-ttu-id="4a770-141">Ottenere tutti i libri.</span><span class="sxs-lookup"><span data-stu-id="4a770-141">Get all books.</span></span> |
| <span data-ttu-id="4a770-142">GET /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="4a770-142">GET /api/books/{id}</span></span> | <span data-ttu-id="4a770-143">Ottenere un libro di ID.</span><span class="sxs-lookup"><span data-stu-id="4a770-143">Get a book by ID.</span></span> |
| <span data-ttu-id="4a770-144">POST /api/books</span><span class="sxs-lookup"><span data-stu-id="4a770-144">POST /api/books</span></span> | <span data-ttu-id="4a770-145">Creare un nuovo libro.</span><span class="sxs-lookup"><span data-stu-id="4a770-145">Create a new book.</span></span> |
| <span data-ttu-id="4a770-146">PUT /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="4a770-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="4a770-147">Aggiornare un libro esistente.</span><span class="sxs-lookup"><span data-stu-id="4a770-147">Update an existing book.</span></span> |
| <span data-ttu-id="4a770-148">DELETE /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="4a770-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="4a770-149">Eliminare un libro.</span><span class="sxs-lookup"><span data-stu-id="4a770-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="4a770-150">Visualizzare il Database (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="4a770-150">View the Database (Optional)</span></span>

<span data-ttu-id="4a770-151">Quando è stato eseguito il comando Update-Database, Entity Framework è stato creato il database e chiamato il `Seed` (metodo).</span><span class="sxs-lookup"><span data-stu-id="4a770-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="4a770-152">Quando si esegue l'applicazione in locale, Usa EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="4a770-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="4a770-153">È possibile visualizzare il database in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4a770-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="4a770-154">Dal **View** dal menu **Esplora oggetti di SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="4a770-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="4a770-155">Nel **Connetti al Server** finestra di dialogo, nella **nome Server** casella di modifica, digitare "(localdb) \v11.0".</span><span class="sxs-lookup"><span data-stu-id="4a770-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="4a770-156">Lasciare il **autenticazione** opzione come "Autenticazione di Windows".</span><span class="sxs-lookup"><span data-stu-id="4a770-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="4a770-157">Fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="4a770-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="4a770-158">Visual Studio si connette al database locale e visualizza i database esistenti nella finestra di Esplora oggetti di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4a770-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="4a770-159">È possibile espandere i nodi per visualizzare le tabelle create di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4a770-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="4a770-160">Per visualizzare i dati, fare doppio clic su una tabella e selezionare **dati della visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="4a770-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="4a770-161">Lo screenshot seguente mostra i risultati per la tabella di libri.</span><span class="sxs-lookup"><span data-stu-id="4a770-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="4a770-162">Si noti che EF popolato il database con i dati di seeding e la tabella contiene la chiave esterna alla tabella degli autori.</span><span class="sxs-lookup"><span data-stu-id="4a770-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> <span data-ttu-id="4a770-163">[Precedente](part-2.md)
> [Successivo](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="4a770-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
