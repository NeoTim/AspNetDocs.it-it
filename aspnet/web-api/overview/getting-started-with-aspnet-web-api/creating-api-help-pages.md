---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Creazione di pagine della Guida per l'API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/01/2013
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: fba368e4017fea65ff96e2540d486662cc6b45f8
ms.sourcegitcommit: 289e051cc8a90e8f7127e239fda73047bde4de12
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2019
ms.locfileid: "58423728"
---
<a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="f34cd-102">Creazione di pagine della Guida per l'API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f34cd-102">Creating Help Pages for ASP.NET Web API</span></span>
====================
<span data-ttu-id="f34cd-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f34cd-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f34cd-104">Quando si crea un'API web, è spesso utile creare una pagina della Guida, in modo che altri sviluppatori saranno in grado di chiamare l'API.</span><span class="sxs-lookup"><span data-stu-id="f34cd-104">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="f34cd-105">È possibile creare tutta la documentazione manualmente, ma è preferibile per la generazione automatica quanto più possibile.</span><span class="sxs-lookup"><span data-stu-id="f34cd-105">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span>

<span data-ttu-id="f34cd-106">Per semplificare questa attività, API Web ASP.NET fornisce una libreria per la generazione automatica di pagine della Guida in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f34cd-106">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="f34cd-107">Creazione di pagine della Guida di API</span><span class="sxs-lookup"><span data-stu-id="f34cd-107">Creating API Help Pages</span></span>

<span data-ttu-id="f34cd-108">Installare [ASP.NET e Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="f34cd-108">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="f34cd-109">Questo aggiornamento pagine della Guida si integra il modello di progetto API Web.</span><span class="sxs-lookup"><span data-stu-id="f34cd-109">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="f34cd-110">Successivamente, creare un nuovo progetto ASP.NET MVC 4 e selezionare il modello di progetto API Web.</span><span class="sxs-lookup"><span data-stu-id="f34cd-110">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="f34cd-111">Il modello di progetto crea un controller API di esempio denominato `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="f34cd-111">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="f34cd-112">Il modello crea anche le pagine della Guida dell'API.</span><span class="sxs-lookup"><span data-stu-id="f34cd-112">The template also creates the API help pages.</span></span> <span data-ttu-id="f34cd-113">Tutti i file di codice per la pagina della Guida vengono inseriti nella cartella aree del progetto.</span><span class="sxs-lookup"><span data-stu-id="f34cd-113">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="f34cd-114">Quando si esegue l'applicazione, la home page contiene un collegamento alla pagina della Guida dell'API.</span><span class="sxs-lookup"><span data-stu-id="f34cd-114">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="f34cd-115">Dalla home page, il percorso relativo è /Help.</span><span class="sxs-lookup"><span data-stu-id="f34cd-115">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="f34cd-116">Questo collegamento consente di accedere a una pagina di riepilogo di API.</span><span class="sxs-lookup"><span data-stu-id="f34cd-116">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="f34cd-117">La visualizzazione MVC per questa pagina è definita nel Areas/HelpPage/Views/Help/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="f34cd-117">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="f34cd-118">È possibile modificare questa pagina per modificare il layout, introduzione, title, stili e così via.</span><span class="sxs-lookup"><span data-stu-id="f34cd-118">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="f34cd-119">La parte principale della pagina è una tabella delle API, raggruppate dal controller.</span><span class="sxs-lookup"><span data-stu-id="f34cd-119">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="f34cd-120">Le voci della tabella vengono generate in modo dinamico, usando il **IApiExplorer** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="f34cd-120">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="f34cd-121">(Parlerò ulteriori informazioni su questa interfaccia in un secondo momento.) Se si aggiunge un nuovo controller di API, la tabella viene aggiornata automaticamente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f34cd-121">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="f34cd-122">La colonna "API" Elenca l'URI relativo e il metodo HTTP.</span><span class="sxs-lookup"><span data-stu-id="f34cd-122">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="f34cd-123">La colonna "Description" contiene la documentazione per ogni API.</span><span class="sxs-lookup"><span data-stu-id="f34cd-123">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="f34cd-124">Inizialmente, la documentazione è semplicemente testo segnaposto.</span><span class="sxs-lookup"><span data-stu-id="f34cd-124">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="f34cd-125">Nella sezione successiva, vi mostrerò come aggiungere la documentazione da commenti in formato XML.</span><span class="sxs-lookup"><span data-stu-id="f34cd-125">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="f34cd-126">Ogni API contiene un collegamento a una pagina con informazioni più dettagliate, tra cui i corpi di richiesta e risposta di esempio.</span><span class="sxs-lookup"><span data-stu-id="f34cd-126">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="f34cd-127">Aggiunta di pagine della Guida per un progetto esistente</span><span class="sxs-lookup"><span data-stu-id="f34cd-127">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="f34cd-128">È possibile aggiungere le pagine della Guida per un progetto API Web esistente usando Gestione pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="f34cd-128">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="f34cd-129">Questa opzione è utile che iniziare da un modello di progetto diverso rispetto al modello "API Web".</span><span class="sxs-lookup"><span data-stu-id="f34cd-129">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="f34cd-130">Dal **strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="f34cd-130">From the **Tools** menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="f34cd-131">Nel [Console di gestione pacchetti](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) finestra, digitare uno dei seguenti comandi:</span><span class="sxs-lookup"><span data-stu-id="f34cd-131">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="f34cd-132">Per un **c#** applicazione: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="f34cd-132">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="f34cd-133">Per un **Visual Basic** applicazione: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="f34cd-133">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="f34cd-134">Esistono due pacchetti, uno per il linguaggio c# e uno per Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="f34cd-134">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="f34cd-135">Assicurarsi di usare quello che corrisponde al progetto.</span><span class="sxs-lookup"><span data-stu-id="f34cd-135">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="f34cd-136">Questo comando Installa gli assembly necessari e aggiunge le visualizzazioni MVC per le pagine della Guida (che si trova nella cartella aree/HelpPage).</span><span class="sxs-lookup"><span data-stu-id="f34cd-136">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="f34cd-137">È necessario aggiungere manualmente un collegamento alla pagina della Guida.</span><span class="sxs-lookup"><span data-stu-id="f34cd-137">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="f34cd-138">L'URI è /Help.</span><span class="sxs-lookup"><span data-stu-id="f34cd-138">The URI is /Help.</span></span> <span data-ttu-id="f34cd-139">Per creare un collegamento in una visualizzazione razor, aggiungere quanto segue:</span><span class="sxs-lookup"><span data-stu-id="f34cd-139">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="f34cd-140">Inoltre, assicurarsi di registrare le aree.</span><span class="sxs-lookup"><span data-stu-id="f34cd-140">Also, make sure to register areas.</span></span> <span data-ttu-id="f34cd-141">Nel file Global. asax, aggiungere il codice seguente per il **Application\_avviare** metodo, se non esiste già:</span><span class="sxs-lookup"><span data-stu-id="f34cd-141">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="f34cd-142">Aggiunta di documentazione dell'API</span><span class="sxs-lookup"><span data-stu-id="f34cd-142">Adding API Documentation</span></span>

<span data-ttu-id="f34cd-143">Per impostazione predefinita, la Guida in linea nelle pagine siano stringhe di segnaposto per la documentazione.</span><span class="sxs-lookup"><span data-stu-id="f34cd-143">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="f34cd-144">È possibile usare [commenti in formato documentazione XML](https://msdn.microsoft.com/library/b2s063f7.aspx) per creare la documentazione.</span><span class="sxs-lookup"><span data-stu-id="f34cd-144">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="f34cd-145">Per abilitare questa funzionalità, aprire il file aree/HelpPage/App\_Start/HelpPageConfig.cs e rimuovere il commento la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="f34cd-145">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="f34cd-146">Abilitare ora la documentazione XML.</span><span class="sxs-lookup"><span data-stu-id="f34cd-146">Now enable XML documentation.</span></span> <span data-ttu-id="f34cd-147">In Esplora soluzioni fare clic sul progetto e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="f34cd-147">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="f34cd-148">Selezionare il **compilazione** pagina.</span><span class="sxs-lookup"><span data-stu-id="f34cd-148">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="f34cd-149">Sotto **Output**, controllare **file di documentazione XML**.</span><span class="sxs-lookup"><span data-stu-id="f34cd-149">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="f34cd-150">Nella casella di modifica, digitare "App\_Data/XmlDocument.xml".</span><span class="sxs-lookup"><span data-stu-id="f34cd-150">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="f34cd-151">Aprire quindi il codice per il `ValuesController` controller API, che viene definito in /Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="f34cd-151">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesController.cs.</span></span> <span data-ttu-id="f34cd-152">Aggiungere alcuni commenti della documentazione per i metodi del controller.</span><span class="sxs-lookup"><span data-stu-id="f34cd-152">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="f34cd-153">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f34cd-153">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="f34cd-154">Suggerimento: Se si posiziona il cursore sulla riga sopra il metodo e digitare i tre barre, Visual Studio inserisce automaticamente gli elementi XML.</span><span class="sxs-lookup"><span data-stu-id="f34cd-154">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="f34cd-155">È quindi possibile compilare i campi vuoti.</span><span class="sxs-lookup"><span data-stu-id="f34cd-155">Then you can fill in the blanks.</span></span>


<span data-ttu-id="f34cd-156">A questo punto compilare ed eseguire nuovamente l'applicazione e passare alle pagine della Guida.</span><span class="sxs-lookup"><span data-stu-id="f34cd-156">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="f34cd-157">Stringhe di documentazione verrà visualizzato nella tabella dell'API.</span><span class="sxs-lookup"><span data-stu-id="f34cd-157">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="f34cd-158">La pagina della Guida legge le stringhe dal file XML in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f34cd-158">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="f34cd-159">(Quando si distribuisce l'applicazione, assicurarsi di distribuire il file XML.)</span><span class="sxs-lookup"><span data-stu-id="f34cd-159">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="f34cd-160">Dietro le quinte</span><span class="sxs-lookup"><span data-stu-id="f34cd-160">Under the Hood</span></span>

<span data-ttu-id="f34cd-161">Le pagine della Guida vengono compilate in cima il **ApiExplorer** (classe), che fa parte del framework API Web.</span><span class="sxs-lookup"><span data-stu-id="f34cd-161">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="f34cd-162">Il **ApiExplorer** classe fornisce il materiale non elaborato per la creazione di una pagina della Guida.</span><span class="sxs-lookup"><span data-stu-id="f34cd-162">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="f34cd-163">Per ogni API **ApiExplorer** contiene un' **ApiDescription** che descrive l'API.</span><span class="sxs-lookup"><span data-stu-id="f34cd-163">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="f34cd-164">A tale scopo, un' "API" è definita come la combinazione di URI relativo e il metodo HTTP.</span><span class="sxs-lookup"><span data-stu-id="f34cd-164">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="f34cd-165">Ad esempio, ecco alcune API distinte:</span><span class="sxs-lookup"><span data-stu-id="f34cd-165">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="f34cd-166">OTTENERE /api/Products</span><span class="sxs-lookup"><span data-stu-id="f34cd-166">GET /api/Products</span></span>
- <span data-ttu-id="f34cd-167">GET/API/prodotti / {id}</span><span class="sxs-lookup"><span data-stu-id="f34cd-167">GET /api/Products/{id}</span></span>
- <span data-ttu-id="f34cd-168">POST/api/prodotti</span><span class="sxs-lookup"><span data-stu-id="f34cd-168">POST /api/Products</span></span>

<span data-ttu-id="f34cd-169">Se un'azione del controller supporta più metodi HTTP, il **ApiExplorer** considera ogni metodo come un'API distinta.</span><span class="sxs-lookup"><span data-stu-id="f34cd-169">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="f34cd-170">Per nascondere un'API dei **ApiExplorer**, aggiungere il **ApiExplorerSettings** attributo per l'azione e il set *IgnoreApi* su true.</span><span class="sxs-lookup"><span data-stu-id="f34cd-170">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="f34cd-171">È anche possibile aggiungere questo attributo nel controller, escludere l'intero controller.</span><span class="sxs-lookup"><span data-stu-id="f34cd-171">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="f34cd-172">La classe di ApiExplorer Ottiene le stringhe di documentazione dal **IDocumentationProvider** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="f34cd-172">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="f34cd-173">Come illustrato in precedenza, la libreria di pagine della Guida fornisce un' **IDocumentationProvider** che ottiene la documentazione da stringhe di documentazione XML.</span><span class="sxs-lookup"><span data-stu-id="f34cd-173">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="f34cd-174">Il codice è disponibile in /Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="f34cd-174">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="f34cd-175">È possibile ottenere la documentazione di un'altra origine scrivendo il proprio **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="f34cd-175">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="f34cd-176">Per collegare, iscrizione, chiama il **SetDocumentationProvider** metodo di estensione, definito **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="f34cd-176">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="f34cd-177">**ApiExplorer** chiama automaticamente il **IDocumentationProvider** interfaccia per recuperare le stringhe di documentazione per ogni API.</span><span class="sxs-lookup"><span data-stu-id="f34cd-177">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="f34cd-178">Li archivia nel **documentazione** proprietà delle **ApiDescription** e **ApiParameterDescription** oggetti.</span><span class="sxs-lookup"><span data-stu-id="f34cd-178">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f34cd-179">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="f34cd-179">Next Steps</span></span>

<span data-ttu-id="f34cd-180">Non è limitato alle pagine della guida illustrate di seguito.</span><span class="sxs-lookup"><span data-stu-id="f34cd-180">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="f34cd-181">In realtà **ApiExplorer** non è limitata alla creazione di pagine della Guida.</span><span class="sxs-lookup"><span data-stu-id="f34cd-181">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="f34cd-182">Yao Huang Lin ha scritto che alcuni eccezionali post di blog per ottenere che possono risultare utili predefiniti:</span><span class="sxs-lookup"><span data-stu-id="f34cd-182">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="f34cd-183">Aggiunta di un semplice Client di prova alla pagina della Guida ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="f34cd-183">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="f34cd-184">Funzionamento pagina della Guida ASP.NET Web API nei servizi self-hosted</span><span class="sxs-lookup"><span data-stu-id="f34cd-184">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="f34cd-185">Generazione in fase di progettazione della Guida pagina (o client) per l'API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f34cd-185">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="f34cd-186">Personalizzazioni avanzate di pagina della Guida</span><span class="sxs-lookup"><span data-stu-id="f34cd-186">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
