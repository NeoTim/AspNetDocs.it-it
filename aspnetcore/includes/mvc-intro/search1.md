---
ms.openlocfilehash: 24726fba7f431f701b264a988a8de1b67d41d8a2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048718"
---
# <a name="add-search-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="911a8-101">Aggiungere la funzionalità di ricerca a un'app ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="911a8-101">Add search to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="911a8-102">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="911a8-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="911a8-103">In questa sezione si aggiunge una funzionalità di ricerca al metodo di azione `Index` che consente di ricercare i film in base al *genere* o al *nome*.</span><span class="sxs-lookup"><span data-stu-id="911a8-103">In this section you add search capability to the `Index` action method that lets you search movies by *genre* or *name*.</span></span>

<span data-ttu-id="911a8-104">Aggiornare il metodo `Index` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="911a8-104">Update the `Index` method with the following code:</span></span>
<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

<span data-ttu-id="911a8-105">La prima riga del metodo di azione `Index` crea una query [LINQ](/dotnet/standard/using-linq) per selezionare i film:</span><span class="sxs-lookup"><span data-stu-id="911a8-105">The first line of the `Index` action method creates a [LINQ](/dotnet/standard/using-linq) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="911a8-106">La query viene *solo* definita in questo punto, ma **non** viene eseguita nel database.</span><span class="sxs-lookup"><span data-stu-id="911a8-106">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="911a8-107">Se il parametro `searchString` contiene una stringa, la query dei film viene modificata per filtrare in base al valore della stringa di ricerca:</span><span class="sxs-lookup"><span data-stu-id="911a8-107">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

<span data-ttu-id="911a8-108">Il codice `s => s.Title.Contains()` precedente è un'[espressione lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="911a8-108">The `s => s.Title.Contains()` code above is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="911a8-109">Le espressioni lambda vengono usate nelle query [LINQ](/dotnet/standard/using-linq) basate sul metodo come argomenti dei metodi di operatori di query standard, quali il metodo [Where](/dotnet/api/system.linq.enumerable.where) o `Contains` (usato nel codice precedente).</span><span class="sxs-lookup"><span data-stu-id="911a8-109">Lambdas are used in method-based [LINQ](/dotnet/standard/using-linq) queries as arguments to standard query operator methods such as the [Where](/dotnet/api/system.linq.enumerable.where) method or `Contains` (used in the code above).</span></span> <span data-ttu-id="911a8-110">Le query LINQ non vengono eseguite al momento della definizione o della modifica chiamando un metodo, ad esempio `Where`, `Contains` o `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="911a8-110">LINQ queries are not executed when they're defined or when they're modified by calling a method such as `Where`, `Contains`  or `OrderBy`.</span></span> <span data-ttu-id="911a8-111">L'esecuzione della query viene invece posticipata.</span><span class="sxs-lookup"><span data-stu-id="911a8-111">Rather, query execution is deferred.</span></span>  <span data-ttu-id="911a8-112">Per esecuzione posticipata si intende che la valutazione di un'espressione viene ritardata finché non viene eseguita l'iterazione del valore o non viene chiamato il metodo `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="911a8-112">That means that the evaluation of an expression is delayed until its realized value is actually iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="911a8-113">Per altre informazioni sull'esecuzione posticipata di query, vedere [Esecuzione di query](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="911a8-113">For more information about deferred query execution, see [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span></span>

<span data-ttu-id="911a8-114">Nota: il metodo [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) viene eseguito nel database, non nel codice C# illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="911a8-114">Note: The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the c# code shown above.</span></span> <span data-ttu-id="911a8-115">La distinzione tra maiuscole/minuscole nella query dipende dal database e dalle regole di confronto.</span><span class="sxs-lookup"><span data-stu-id="911a8-115">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="911a8-116">In SQL Server, [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) esegue il mapping a [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), che fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="911a8-116">On SQL Server, [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="911a8-117">In SQLite, con le regole di confronto predefinite si fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="911a8-117">In SQLlite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="911a8-118">Passare a `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="911a8-118">Navigate to `/Movies/Index`.</span></span> <span data-ttu-id="911a8-119">Accodare una stringa di query, ad esempio `?searchString=Ghost`, all'URL.</span><span class="sxs-lookup"><span data-stu-id="911a8-119">Append a query string such as `?searchString=Ghost` to the URL.</span></span> <span data-ttu-id="911a8-120">Vengono visualizzati i film filtrati.</span><span class="sxs-lookup"><span data-stu-id="911a8-120">The filtered movies are displayed.</span></span>

![Vista Index](~/tutorials/first-mvc-app/search/_static/ghost.png)

<span data-ttu-id="911a8-122">Se si modifica la firma del metodo `Index` in modo che contenga un parametro denominato `id`, il parametro `id` corrisponderà al segnaposto `{id}` facoltativo per le route predefinite impostate in *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="911a8-122">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the optional `{id}` placeholder for the default routes set in *Startup.cs*.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]
