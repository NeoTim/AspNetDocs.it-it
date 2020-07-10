---
uid: mvc/overview/getting-started/introduction/adding-search
title: Cerca | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/17/2019
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: f6d6d32a648fed453be924790a1b55698c9cf209
ms.sourcegitcommit: 0d583ed9253103f3e50b6d729276e667591cdd41
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2020
ms.locfileid: "86211478"
---
# <a name="search"></a><span data-ttu-id="4df3f-102">Ricerca</span><span class="sxs-lookup"><span data-stu-id="4df3f-102">Search</span></span>

[!INCLUDE [Tutorial Note](index.md)]

## <a name="adding-a-search-method-and-search-view"></a><span data-ttu-id="4df3f-103">Aggiunta di un metodo di ricerca e di una visualizzazione di ricerca</span><span class="sxs-lookup"><span data-stu-id="4df3f-103">Adding a Search Method and Search View</span></span>

<span data-ttu-id="4df3f-104">In questa sezione si aggiungerà la funzionalità di ricerca al `Index` metodo di azione che consente di cercare i film in base al genere o al nome.</span><span class="sxs-lookup"><span data-stu-id="4df3f-104">In this section you'll add search capability to the `Index` action method that lets you search movies by genre or name.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4df3f-105">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="4df3f-105">Prerequisites</span></span>

<span data-ttu-id="4df3f-106">Per trovare la corrispondenza con le schermate di questa sezione, è necessario eseguire l'applicazione (F5) e aggiungere i film seguenti al database.</span><span class="sxs-lookup"><span data-stu-id="4df3f-106">To match this section's screenshots, you need to run the application (F5) and add the following movies to the database.</span></span>

| <span data-ttu-id="4df3f-107">Titolo</span><span class="sxs-lookup"><span data-stu-id="4df3f-107">Title</span></span> | <span data-ttu-id="4df3f-108">Data di rilascio</span><span class="sxs-lookup"><span data-stu-id="4df3f-108">Release Date</span></span> | <span data-ttu-id="4df3f-109">Genre</span><span class="sxs-lookup"><span data-stu-id="4df3f-109">Genre</span></span> | <span data-ttu-id="4df3f-110">Prezzo</span><span class="sxs-lookup"><span data-stu-id="4df3f-110">Price</span></span> |
| ----- | ------------ | ----- | ----- |
| <span data-ttu-id="4df3f-111">Ghostbusters</span><span class="sxs-lookup"><span data-stu-id="4df3f-111">Ghostbusters</span></span> | <span data-ttu-id="4df3f-112">6/8/1984</span><span class="sxs-lookup"><span data-stu-id="4df3f-112">6/8/1984</span></span> | <span data-ttu-id="4df3f-113">Film commedia</span><span class="sxs-lookup"><span data-stu-id="4df3f-113">Comedy</span></span> | <span data-ttu-id="4df3f-114">6,99</span><span class="sxs-lookup"><span data-stu-id="4df3f-114">6.99</span></span> |
| <span data-ttu-id="4df3f-115">Ghostbusters II</span><span class="sxs-lookup"><span data-stu-id="4df3f-115">Ghostbusters II</span></span> | <span data-ttu-id="4df3f-116">6/16/1989</span><span class="sxs-lookup"><span data-stu-id="4df3f-116">6/16/1989</span></span> | <span data-ttu-id="4df3f-117">Film commedia</span><span class="sxs-lookup"><span data-stu-id="4df3f-117">Comedy</span></span> | <span data-ttu-id="4df3f-118">6,99</span><span class="sxs-lookup"><span data-stu-id="4df3f-118">6.99</span></span> |
| <span data-ttu-id="4df3f-119">Pianeta delle scimmie</span><span class="sxs-lookup"><span data-stu-id="4df3f-119">Planet of the Apes</span></span> | <span data-ttu-id="4df3f-120">3/27/1986</span><span class="sxs-lookup"><span data-stu-id="4df3f-120">3/27/1986</span></span> | <span data-ttu-id="4df3f-121">Operazione</span><span class="sxs-lookup"><span data-stu-id="4df3f-121">Action</span></span> | <span data-ttu-id="4df3f-122">5,99</span><span class="sxs-lookup"><span data-stu-id="4df3f-122">5.99</span></span> |

## <a name="updating-the-index-form"></a><span data-ttu-id="4df3f-123">Aggiornamento del modulo di indice</span><span class="sxs-lookup"><span data-stu-id="4df3f-123">Updating the Index Form</span></span>

<span data-ttu-id="4df3f-124">Per iniziare, aggiornare il `Index` metodo di azione alla `MoviesController` classe esistente.</span><span class="sxs-lookup"><span data-stu-id="4df3f-124">Start by updating the `Index` action method to the existing `MoviesController` class.</span></span> <span data-ttu-id="4df3f-125">Ecco il codice:</span><span class="sxs-lookup"><span data-stu-id="4df3f-125">Here's the code:</span></span>

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

<span data-ttu-id="4df3f-126">La prima riga del `Index` metodo crea la seguente query [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) per selezionare i film:</span><span class="sxs-lookup"><span data-stu-id="4df3f-126">The first line of the `Index` method creates the following [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query to select the movies:</span></span>

[!code-csharp[Main](adding-search/samples/sample2.cs)]

<span data-ttu-id="4df3f-127">La query è definita a questo punto, ma non è ancora stata eseguita sul database.</span><span class="sxs-lookup"><span data-stu-id="4df3f-127">The query is defined at this point, but hasn't yet been run against the database.</span></span>

<span data-ttu-id="4df3f-128">Se il `searchString` parametro contiene una stringa, la query Movies viene modificata per filtrare in base al valore della stringa di ricerca, usando il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4df3f-128">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string, using the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample3.cs)]

<span data-ttu-id="4df3f-129">Il codice `s => s.Title` precedente è un'[espressione lambda](https://msdn.microsoft.com/library/bb397687.aspx).</span><span class="sxs-lookup"><span data-stu-id="4df3f-129">The `s => s.Title` code above is a [Lambda Expression](https://msdn.microsoft.com/library/bb397687.aspx).</span></span> <span data-ttu-id="4df3f-130">Le espressioni lambda vengono usate nelle query [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) basate sul metodo come argomenti per i metodi degli operatori di query standard, ad esempio il metodo [where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) usato nel codice riportato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4df3f-130">Lambdas are used in method-based [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) method used in the above code.</span></span> <span data-ttu-id="4df3f-131">Le query LINQ non vengono eseguite quando vengono definite o quando vengono modificate chiamando un metodo come `Where` o `OrderBy` .</span><span class="sxs-lookup"><span data-stu-id="4df3f-131">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where` or `OrderBy`.</span></span> <span data-ttu-id="4df3f-132">Al contrario, l'esecuzione della query viene posticipata, il che significa che la valutazione di un'espressione viene posticipata finché il relativo valore realizzato non viene effettivamente iterato o [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) viene chiamato il metodo.</span><span class="sxs-lookup"><span data-stu-id="4df3f-132">Instead, query execution is deferred, which means that the evaluation of an expression is delayed until its realized value is actually iterated over or the [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) method is called.</span></span> <span data-ttu-id="4df3f-133">Nell' `Search` esempio, la query viene eseguita nella vista *index. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="4df3f-133">In the `Search` sample, the query is executed in the *Index.cshtml* view.</span></span> <span data-ttu-id="4df3f-134">Per altre informazioni sull'esecuzione posticipata di query, vedere [Esecuzione di query](https://msdn.microsoft.com/library/bb738633.aspx).</span><span class="sxs-lookup"><span data-stu-id="4df3f-134">For more information about deferred query execution, see [Query Execution](https://msdn.microsoft.com/library/bb738633.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="4df3f-135">Il metodo [Contains](https://msdn.microsoft.com/library/bb155125.aspx) viene eseguito sul database, non sul codice c# precedente.</span><span class="sxs-lookup"><span data-stu-id="4df3f-135">The [Contains](https://msdn.microsoft.com/library/bb155125.aspx) method is run on the database, not the c# code above.</span></span> <span data-ttu-id="4df3f-136">Nel database [contiene](https://msdn.microsoft.com/library/bb155125.aspx) mapping a [SQL like](https://msdn.microsoft.com/library/ms179859.aspx), che non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="4df3f-136">On the database, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) maps to [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), which is case insensitive.</span></span>

<span data-ttu-id="4df3f-137">A questo punto è possibile aggiornare la `Index` vista che visualizzerà il modulo all'utente.</span><span class="sxs-lookup"><span data-stu-id="4df3f-137">Now you can update the `Index` view that will display the form to the user.</span></span>

<span data-ttu-id="4df3f-138">Eseguire l'applicazione e passare a */movies/index*.</span><span class="sxs-lookup"><span data-stu-id="4df3f-138">Run the application and navigate to */Movies/Index*.</span></span> <span data-ttu-id="4df3f-139">Accodare una stringa di query, ad esempio `?searchString=ghost`, all'URL.</span><span class="sxs-lookup"><span data-stu-id="4df3f-139">Append a query string such as `?searchString=ghost` to the URL.</span></span> <span data-ttu-id="4df3f-140">Vengono visualizzati i film filtrati.</span><span class="sxs-lookup"><span data-stu-id="4df3f-140">The filtered movies are displayed.</span></span>

![SearchQryStr](adding-search/_static/image1.png)

<span data-ttu-id="4df3f-142">Se si modifica la firma del `Index` metodo in modo che disponga di un parametro denominato `id` , il `id` parametro corrisponderà al `{id}` segnaposto per le route predefinite impostate nel file \* \_ Start\RouteConfig.cs dell'app\* .</span><span class="sxs-lookup"><span data-stu-id="4df3f-142">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the `{id}` placeholder for the default routes set in the *App\_Start\RouteConfig.cs* file.</span></span>

[!code-json[Main](adding-search/samples/sample4.json)]

<span data-ttu-id="4df3f-143">Il metodo originale ha un `Index` aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="4df3f-143">The original `Index` method looks like this::</span></span>

[!code-csharp[Main](adding-search/samples/sample5.cs)]

<span data-ttu-id="4df3f-144">Il `Index` metodo modificato avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="4df3f-144">The modified `Index` method would look as follows:</span></span>

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

<span data-ttu-id="4df3f-145">È ora possibile passare il titolo della ricerca come dati di route (un segmento di URL), anziché come valore della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="4df3f-145">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![](adding-search/_static/image2.png)

<span data-ttu-id="4df3f-146">Tuttavia, non è possibile supporre che gli utenti modifichino l'URL ogni volta che desiderano cercare un film.</span><span class="sxs-lookup"><span data-stu-id="4df3f-146">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="4df3f-147">A questo punto si aggiungerà l'interfaccia utente per filtrare i film.</span><span class="sxs-lookup"><span data-stu-id="4df3f-147">So now you'll add UI to help them filter movies.</span></span> <span data-ttu-id="4df3f-148">Se è stata modificata la firma del `Index` metodo per testare come passare il parametro ID associato alla route, modificarlo di nuovo in modo che il `Index` Metodo accetti un parametro di stringa denominato `searchString` :</span><span class="sxs-lookup"><span data-stu-id="4df3f-148">If you changed the signature of the `Index` method to test how to pass the route-bound ID parameter, change it back so that your `Index` method takes a string parameter named `searchString`:</span></span>

[!code-csharp[Main](adding-search/samples/sample7.cs)]

<span data-ttu-id="4df3f-149">Aprire il file *Views\Movies\Index.cshtml* e, subito dopo `@Html.ActionLink("Create New", "Create")` , aggiungere il markup del modulo evidenziato di seguito:</span><span class="sxs-lookup"><span data-stu-id="4df3f-149">Open the *Views\Movies\Index.cshtml* file, and just after `@Html.ActionLink("Create New", "Create")`, add the form markup highlighted below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

<span data-ttu-id="4df3f-150">L' `Html.BeginForm` Helper crea un tag di apertura `<form>` .</span><span class="sxs-lookup"><span data-stu-id="4df3f-150">The `Html.BeginForm` helper creates an opening `<form>` tag.</span></span> <span data-ttu-id="4df3f-151">L' `Html.BeginForm` helper fa in modo che il form invii a se stesso quando l'utente invia il modulo facendo clic sul pulsante **filtro** .</span><span class="sxs-lookup"><span data-stu-id="4df3f-151">The `Html.BeginForm` helper causes the form to post to itself when the user submits the form by clicking the **Filter** button.</span></span>

<span data-ttu-id="4df3f-152">Visual Studio 2013 offre un miglioramento significativo durante la visualizzazione e la modifica dei file di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="4df3f-152">Visual Studio 2013 has a nice improvement when displaying and editing View files.</span></span> <span data-ttu-id="4df3f-153">Quando si esegue l'applicazione con un file di visualizzazione aperto, Visual Studio 2013 richiama il metodo di azione del controller corretto per visualizzare la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="4df3f-153">When you run the application with a view file open, Visual Studio 2013 invokes the correct controller action method to display the view.</span></span>

![](adding-search/_static/image3.png)

<span data-ttu-id="4df3f-154">Con la visualizzazione dell'indice aperta in Visual Studio (come illustrato nell'immagine precedente), toccare CTR F5 o F5 per eseguire l'applicazione, quindi provare a cercare un film.</span><span class="sxs-lookup"><span data-stu-id="4df3f-154">With the Index view open in Visual Studio (as shown in the image above), tap Ctr F5 or F5 to run the application and then try searching for a movie.</span></span>

![](adding-search/_static/image4.png)

<span data-ttu-id="4df3f-155">Nessun `HttpPost` Overload del `Index` metodo.</span><span class="sxs-lookup"><span data-stu-id="4df3f-155">There's no `HttpPost` overload of the `Index` method.</span></span> <span data-ttu-id="4df3f-156">Non è necessario, perché il metodo non modifica lo stato dell'applicazione, limitando a filtrare i dati.</span><span class="sxs-lookup"><span data-stu-id="4df3f-156">You don't need it, because the method isn't changing the state of the application, just filtering data.</span></span>

<span data-ttu-id="4df3f-157">È possibile aggiungere il metodo `HttpPost Index` seguente.</span><span class="sxs-lookup"><span data-stu-id="4df3f-157">You could add the following `HttpPost Index` method.</span></span> <span data-ttu-id="4df3f-158">In tal caso, l'invoker dell'azione corrisponderebbe al `HttpPost Index` metodo e il `HttpPost Index` Metodo verrebbe eseguito come illustrato nell'immagine seguente.</span><span class="sxs-lookup"><span data-stu-id="4df3f-158">In that case, the action invoker would match the `HttpPost Index` method, and the `HttpPost Index` method would run as shown in the image below.</span></span>

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

<span data-ttu-id="4df3f-160">Tuttavia, anche se si aggiunge questa versione `HttpPost` del metodo `Index`, esiste una limitazione sul modo sulla relativa implementazione.</span><span class="sxs-lookup"><span data-stu-id="4df3f-160">However, even if you add this `HttpPost` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="4df3f-161">Si supponga che si desideri usare come segnalibro una ricerca specifica o inviare un collegamento agli amici su cui possono fare clic per visualizzare lo stesso elenco filtrato di film.</span><span class="sxs-lookup"><span data-stu-id="4df3f-161">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="4df3f-162">Si noti che l'URL della richiesta HTTP POST è lo stesso dell'URL per la richiesta GET (localhost: xxxxx/movies/index). non sono presenti informazioni di ricerca nell'URL stesso.</span><span class="sxs-lookup"><span data-stu-id="4df3f-162">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL itself.</span></span> <span data-ttu-id="4df3f-163">A questo punto, le informazioni sulla stringa di ricerca vengono inviate al server come valore del campo del modulo.</span><span class="sxs-lookup"><span data-stu-id="4df3f-163">Right now, the search string information is sent to the server as a form field value.</span></span> <span data-ttu-id="4df3f-164">Ciò significa che non è possibile acquisire le informazioni di ricerca nel segnalibro o inviarle agli amici in un URL.</span><span class="sxs-lookup"><span data-stu-id="4df3f-164">This means you can't capture that search information to bookmark or send to friends in a URL.</span></span>

<span data-ttu-id="4df3f-165">La soluzione consiste nell'usare un overload di `BeginForm` che specifichi che la richiesta post deve aggiungere le informazioni di ricerca all'URL e che deve essere indirizzata alla `HttpGet` versione del `Index` metodo.</span><span class="sxs-lookup"><span data-stu-id="4df3f-165">The solution is to use an overload of `BeginForm` that specifies that the POST request should add the search information to the URL and that it should be routed to the `HttpGet` version of the `Index` method.</span></span> <span data-ttu-id="4df3f-166">Sostituire il `BeginForm` metodo senza parametri esistente con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="4df3f-166">Replace the existing parameterless `BeginForm` method with the following markup:</span></span>

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

<span data-ttu-id="4df3f-168">A questo punto, quando si invia una ricerca, l'URL contiene una stringa di query di ricerca.</span><span class="sxs-lookup"><span data-stu-id="4df3f-168">Now when you submit a search, the URL contains a search query string.</span></span> <span data-ttu-id="4df3f-169">La ricerca passerà anche al metodo di azione `HttpGet Index`, anche se si dispone di un metodo `HttpPost Index`.</span><span class="sxs-lookup"><span data-stu-id="4df3f-169">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a><span data-ttu-id="4df3f-171">Aggiunta della ricerca per genere</span><span class="sxs-lookup"><span data-stu-id="4df3f-171">Adding Search by Genre</span></span>

<span data-ttu-id="4df3f-172">Se è stata aggiunta la `HttpPost` versione del `Index` metodo, eliminarla ora.</span><span class="sxs-lookup"><span data-stu-id="4df3f-172">If you added the `HttpPost` version of the `Index` method, delete it now.</span></span>

<span data-ttu-id="4df3f-173">Si aggiungerà quindi una funzionalità che consente agli utenti di cercare i film in base al genere.</span><span class="sxs-lookup"><span data-stu-id="4df3f-173">Next, you'll add a feature to let users search for movies by genre.</span></span> <span data-ttu-id="4df3f-174">Sostituire il metodo `Index` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4df3f-174">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample11.cs)]

<span data-ttu-id="4df3f-175">Questa versione del `Index` metodo accetta un parametro aggiuntivo, ovvero `movieGenre` .</span><span class="sxs-lookup"><span data-stu-id="4df3f-175">This version of the `Index` method takes an additional parameter, namely `movieGenre`.</span></span> <span data-ttu-id="4df3f-176">Le prime righe di codice creano un `List` oggetto per mantenere i generi di film dal database.</span><span class="sxs-lookup"><span data-stu-id="4df3f-176">The first few lines of code create a `List` object to hold movie genres from the database.</span></span>

<span data-ttu-id="4df3f-177">Il codice seguente è una query LINQ che recupera tutti i generi dal database.</span><span class="sxs-lookup"><span data-stu-id="4df3f-177">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](adding-search/samples/sample12.cs)]

<span data-ttu-id="4df3f-178">Il codice usa il `AddRange` metodo della raccolta generica `List` per aggiungere tutti i generi distinti all'elenco.</span><span class="sxs-lookup"><span data-stu-id="4df3f-178">The code uses the `AddRange` method of the generic `List` collection to add all the distinct genres to the list.</span></span> <span data-ttu-id="4df3f-179">(Senza il `Distinct` modificatore, verranno aggiunti generi duplicati, ad esempio, la commedia verrebbe aggiunta due volte nell'esempio).</span><span class="sxs-lookup"><span data-stu-id="4df3f-179">(Without the `Distinct` modifier, duplicate genres would be added — for example, comedy would be added twice in our sample).</span></span> <span data-ttu-id="4df3f-180">Il codice archivia quindi l'elenco dei generi nell' `ViewBag.MovieGenre` oggetto.</span><span class="sxs-lookup"><span data-stu-id="4df3f-180">The code then stores the list of genres in the `ViewBag.MovieGenre` object.</span></span> <span data-ttu-id="4df3f-181">L'archiviazione dei dati di categoria, ad esempio i generi di film, come oggetto [Select](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) list in a `ViewBag` , quindi l'accesso ai dati della categoria in una casella di riepilogo a discesa è un approccio tipico per le applicazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="4df3f-181">Storing category data (such a movie genres) as a [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) object in a `ViewBag`, then accessing the category data in a dropdown list box is a typical approach for MVC applications.</span></span>

<span data-ttu-id="4df3f-182">Il codice seguente illustra come controllare il `movieGenre` parametro.</span><span class="sxs-lookup"><span data-stu-id="4df3f-182">The following code shows how to check the `movieGenre` parameter.</span></span> <span data-ttu-id="4df3f-183">Se non è vuoto, il codice vincola ulteriormente la query dei film per limitare i film selezionati al genere specificato.</span><span class="sxs-lookup"><span data-stu-id="4df3f-183">If it's not empty, the code further constrains the movies query to limit the selected movies to the specified genre.</span></span>

[!code-csharp[Main](adding-search/samples/sample13.cs)]

<span data-ttu-id="4df3f-184">Come indicato in precedenza, la query non viene eseguita nel database fino a quando non viene eseguita l'iterazione dell'elenco di film (che si verifica nella visualizzazione, dopo la `Index` restituzione del metodo di azione).</span><span class="sxs-lookup"><span data-stu-id="4df3f-184">As stated previously, the query is not run on the database until the movie list is iterated over (which happens in the View, after the `Index` action method returns).</span></span>

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a><span data-ttu-id="4df3f-185">Aggiunta di markup alla visualizzazione index per supportare la ricerca per genere</span><span class="sxs-lookup"><span data-stu-id="4df3f-185">Adding Markup to the Index View to Support Search by Genre</span></span>

<span data-ttu-id="4df3f-186">Aggiungere un `Html.DropDownList` helper al file *Views\Movies\Index.cshtml* , immediatamente prima dell' `TextBox` Helper.</span><span class="sxs-lookup"><span data-stu-id="4df3f-186">Add an `Html.DropDownList` helper to the *Views\Movies\Index.cshtml* file, just before the `TextBox` helper.</span></span> <span data-ttu-id="4df3f-187">Il markup completato è illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="4df3f-187">The completed markup is shown below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

<span data-ttu-id="4df3f-188">Nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4df3f-188">In the following code:</span></span>

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

<span data-ttu-id="4df3f-189">Il parametro "MovieGenre" fornisce la chiave per l' `DropDownList` Helper per trovare un oggetto `IEnumerable<SelectListItem>` in `ViewBag` .</span><span class="sxs-lookup"><span data-stu-id="4df3f-189">The parameter "MovieGenre" provides the key for the `DropDownList` helper to find a `IEnumerable<SelectListItem>` in the `ViewBag`.</span></span> <span data-ttu-id="4df3f-190">`ViewBag`È stato popolato nel metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="4df3f-190">The `ViewBag` was populated in the action method:</span></span>

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

<span data-ttu-id="4df3f-191">Il parametro "All" fornisce un'etichetta di opzione.</span><span class="sxs-lookup"><span data-stu-id="4df3f-191">The parameter "All" provides an option label.</span></span> <span data-ttu-id="4df3f-192">Se si esamina tale scelta nel browser, si noterà che il relativo attributo "value" è vuoto.</span><span class="sxs-lookup"><span data-stu-id="4df3f-192">If you inspect that choice in your browser, you'll see that its "value" attribute is empty.</span></span> <span data-ttu-id="4df3f-193">Poiché il controller filtra solo `if` la stringa non è `null` o vuoto, l'invio di un valore vuoto per `movieGenre` Mostra tutti i generi.</span><span class="sxs-lookup"><span data-stu-id="4df3f-193">Since our controller only filters `if` the string is not `null` or empty, submitting an empty value for `movieGenre` shows all genres.</span></span>

<span data-ttu-id="4df3f-194">È anche possibile impostare un'opzione per la selezione predefinita.</span><span class="sxs-lookup"><span data-stu-id="4df3f-194">You can also set an option to be selected by default.</span></span> <span data-ttu-id="4df3f-195">Se si vuole "Comedy" come opzione predefinita, modificare il codice nel controller come segue:</span><span class="sxs-lookup"><span data-stu-id="4df3f-195">If you wanted "Comedy" as your default option, you would change the code in the Controller like so:</span></span>

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

<span data-ttu-id="4df3f-196">Eseguire l'applicazione e passare a */movies/index*.</span><span class="sxs-lookup"><span data-stu-id="4df3f-196">Run the application and browse to */Movies/Index*.</span></span> <span data-ttu-id="4df3f-197">Provare a eseguire una ricerca in base al genere, al nome del film e a entrambi i criteri.</span><span class="sxs-lookup"><span data-stu-id="4df3f-197">Try a search by genre, by movie name, and by both criteria.</span></span>

![](adding-search/_static/image8.png)

<span data-ttu-id="4df3f-198">In questa sezione sono stati creati un metodo e una visualizzazione di azione di ricerca che consentono agli utenti di eseguire ricerche in base al titolo e al genere.</span><span class="sxs-lookup"><span data-stu-id="4df3f-198">In this section you created a search action method and view that let users search by movie title and genre.</span></span> <span data-ttu-id="4df3f-199">Nella sezione successiva verrà illustrato come aggiungere una proprietà al `Movie` modello e come aggiungere un inizializzatore che creerà automaticamente un database di prova.</span><span class="sxs-lookup"><span data-stu-id="4df3f-199">In the next section, you'll look at how to add a property to the `Movie` model and how to add an initializer that will automatically create a test database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4df3f-200">[Precedente](examining-the-edit-methods-and-edit-view.md) 
>  [Avanti](adding-a-new-field.md)</span><span class="sxs-lookup"><span data-stu-id="4df3f-200">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-a-new-field.md)</span></span>
