---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: Miglioramento delle prestazioni con Output di memorizzazione nella cache (c#) | Microsoft Docs
author: microsoft
description: In questa esercitazione descrive come è possibile migliorare notevolmente le prestazioni delle applicazioni web ASP.NET MVC, sfruttando i vantaggi della memorizzazione nella cache di output. È...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: 516c370941b8f7e5f3528953491057973679586d
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57049348"
---
<a name="improving-performance-with-output-caching-c"></a><span data-ttu-id="7e847-104">Miglioramento delle prestazioni con la cache di output (C#)</span><span class="sxs-lookup"><span data-stu-id="7e847-104">Improving Performance with Output Caching (C#)</span></span>
====================
<span data-ttu-id="7e847-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="7e847-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="7e847-106">In questa esercitazione descrive come è possibile migliorare notevolmente le prestazioni delle applicazioni web ASP.NET MVC, sfruttando i vantaggi della memorizzazione nella cache di output.</span><span class="sxs-lookup"><span data-stu-id="7e847-106">In this tutorial, you learn how you can dramatically improve the performance of your ASP.NET MVC web applications by taking advantage of output caching.</span></span> <span data-ttu-id="7e847-107">Informazioni su come memorizzare nella cache il risultato restituito da un'azione del controller in modo che lo stesso contenuto non devono essere creati ogni volta che un nuovo utente richiama l'azione.</span><span class="sxs-lookup"><span data-stu-id="7e847-107">You learn how to cache the result returned from a controller action so that the same content does not need to be created each and every time a new user invokes the action.</span></span>


<span data-ttu-id="7e847-108">L'obiettivo di questa esercitazione è spiegare come si possono migliorare notevolmente le prestazioni di un'applicazione ASP.NET MVC, sfruttando i vantaggi della cache di output.</span><span class="sxs-lookup"><span data-stu-id="7e847-108">The goal of this tutorial is to explain how you can dramatically improve the performance of an ASP.NET MVC application by taking advantage of the output cache.</span></span> <span data-ttu-id="7e847-109">La cache di output consente di memorizzare nella cache il contenuto restituito da un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="7e847-109">The output cache enables you to cache the content returned by a controller action.</span></span> <span data-ttu-id="7e847-110">In questo modo, lo stesso contenuto non dovrà essere generato ogni volta che viene richiamata l'azione del controller stesso.</span><span class="sxs-lookup"><span data-stu-id="7e847-110">That way, the same content does not need to be generated each and every time the same controller action is invoked.</span></span>

<span data-ttu-id="7e847-111">Si supponga, ad esempio, che l'applicazione ASP.NET MVC Visualizza un elenco di record di database in una visualizzazione denominata indice.</span><span class="sxs-lookup"><span data-stu-id="7e847-111">Imagine, for example, that your ASP.NET MVC application displays a list of database records in a view named Index.</span></span> <span data-ttu-id="7e847-112">In genere, ogni volta che un utente richiama l'azione del controller che restituisce la visualizzazione dell'indice, il set di record di database di tipo deve essere recuperato dal database eseguendo una query sul database.</span><span class="sxs-lookup"><span data-stu-id="7e847-112">Normally, each and every time that a user invokes the controller action that returns the Index view, the set of database records must be retrieved from the database by executing a database query.</span></span>

<span data-ttu-id="7e847-113">Se, d'altra parte, è possibile sfruttare la cache di output è possibile evitare l'esecuzione di una query sul database ogni volta che un utente richiama l'azione del controller stesso.</span><span class="sxs-lookup"><span data-stu-id="7e847-113">If, on the other hand, you take advantage of the output cache then you can avoid executing a database query every time any user invokes the same controller action.</span></span> <span data-ttu-id="7e847-114">La vista può essere recuperata dalla cache anziché essere rigenerato dall'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="7e847-114">The view can be retrieved from the cache instead of being regenerated from the controller action.</span></span> <span data-ttu-id="7e847-115">La memorizzazione nella cache attiva è possibile evitare di eseguire ridondanti funzionano nel server.</span><span class="sxs-lookup"><span data-stu-id="7e847-115">Caching enables you to avoid performing redundant work on the server.</span></span>

## <a name="enabling-output-caching"></a><span data-ttu-id="7e847-116">Abilitare la memorizzazione nella cache di Output</span><span class="sxs-lookup"><span data-stu-id="7e847-116">Enabling Output Caching</span></span>

<span data-ttu-id="7e847-117">Abilitare la memorizzazione nella cache di output mediante l'aggiunta di un attributo [OutputCache] a un'azione di singoli controller o una classe intero controller.</span><span class="sxs-lookup"><span data-stu-id="7e847-117">You enable output caching by adding an [OutputCache] attribute to either an individual controller action or an entire controller class.</span></span> <span data-ttu-id="7e847-118">Ad esempio, il controller nel listato 1 espone un'azione denominata index ().</span><span class="sxs-lookup"><span data-stu-id="7e847-118">For example, the controller in Listing 1 exposes an action named Index().</span></span> <span data-ttu-id="7e847-119">L'output dell'azione Index () viene memorizzato nella cache per 10 secondi.</span><span class="sxs-lookup"><span data-stu-id="7e847-119">The output of the Index() action is cached for 10 seconds.</span></span>

<span data-ttu-id="7e847-120">**Listato 1 – controllers\homecontroller.cs.**</span><span class="sxs-lookup"><span data-stu-id="7e847-120">**Listing 1 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

<span data-ttu-id="7e847-121">Nelle versioni Beta di ASP.NET MVC, la memorizzazione nella cache di output non funziona per un URL come [ http://www.MySite.com/ ](http://www.mysite.com/).</span><span class="sxs-lookup"><span data-stu-id="7e847-121">In the Beta versions of ASP.NET MVC, output caching does not work for a URL like [http://www.MySite.com/](http://www.mysite.com/).</span></span> <span data-ttu-id="7e847-122">In alternativa, è necessario immettere un URL come [ http://www.MySite.com/Home/Index ](http://www.mysite.com/Home/Index).</span><span class="sxs-lookup"><span data-stu-id="7e847-122">Instead, you must enter a URL like [http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index).</span></span> 

<span data-ttu-id="7e847-123">Nel listato 1, l'output dell'azione Index () viene memorizzato nella cache per 10 secondi.</span><span class="sxs-lookup"><span data-stu-id="7e847-123">In Listing 1, the output of the Index() action is cached for 10 seconds.</span></span> <span data-ttu-id="7e847-124">Se si preferisce, è possibile specificare una durata molto più tempo della cache.</span><span class="sxs-lookup"><span data-stu-id="7e847-124">If you prefer, you can specify a much longer cache duration.</span></span> <span data-ttu-id="7e847-125">Ad esempio, se si vuole memorizzare nella cache l'output di un'azione del controller per un giorno è possibile specificare una durata della cache di 86400 secondi (60 secondi \* 60 minuti \* 24 ore).</span><span class="sxs-lookup"><span data-stu-id="7e847-125">For example, if you want to cache the output of a controller action for one day then you can specify a cache duration of 86400 seconds (60 seconds \* 60 minutes \* 24 hours).</span></span>

<span data-ttu-id="7e847-126">Vi è alcuna garanzia che contenuto non nella cache per la quantità di tempo specificato.</span><span class="sxs-lookup"><span data-stu-id="7e847-126">There is no guarantee that content will be cached for the amount of time that you specify.</span></span> <span data-ttu-id="7e847-127">Quando le risorse di memoria diventano insufficiente, la cache viene avviata automaticamente la rimozione del contenuto.</span><span class="sxs-lookup"><span data-stu-id="7e847-127">When memory resources become low, the cache starts evicting content automatically.</span></span>

<span data-ttu-id="7e847-128">Il controller Home nel listato 1 restituisce la visualizzazione dell'indice nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="7e847-128">The Home controller in Listing 1 returns the Index view in Listing 2.</span></span> <span data-ttu-id="7e847-129">Non è niente di speciale su questa vista.</span><span class="sxs-lookup"><span data-stu-id="7e847-129">There is nothing special about this view.</span></span> <span data-ttu-id="7e847-130">La visualizzazione dell'indice visualizza semplicemente l'ora corrente (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="7e847-130">The Index view simply displays the current time (see Figure 1).</span></span>

<span data-ttu-id="7e847-131">**Listato 2 – Views\Home\Index.aspx.**</span><span class="sxs-lookup"><span data-stu-id="7e847-131">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

<span data-ttu-id="7e847-132">**Figura 1 – vista Index memorizzata nella cache**</span><span class="sxs-lookup"><span data-stu-id="7e847-132">**Figure 1 – Cached Index view**</span></span>

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

<span data-ttu-id="7e847-134">Se si richiama l'azione Index () più volte immettendo l'URL avremo/indice nella barra degli indirizzi del browser e premendo il pulsante di aggiornamento/ricaricamento nel browser più volte, l'ora visualizzata per la visualizzazione dell'indice non verrà modificato per 10 secondi.</span><span class="sxs-lookup"><span data-stu-id="7e847-134">If you invoke the Index() action multiple times by entering the URL /Home/Index in the address bar of your browser and hitting the Refresh/Reload button in your browser repeatedly, then the time displayed by the Index view won't change for 10 seconds.</span></span> <span data-ttu-id="7e847-135">Contemporaneamente viene visualizzato perché la vista viene memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="7e847-135">The same time is displayed because the view is cached.</span></span>

<span data-ttu-id="7e847-136">È importante comprendere che la stessa vista viene memorizzato nella cache per chiunque visiti l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7e847-136">It is important to understand that the same view is cached for everyone who visits your application.</span></span> <span data-ttu-id="7e847-137">Tutti coloro che richiama l'azione Index () otterrà la stessa versione memorizzata nella cache della visualizzazione Index.</span><span class="sxs-lookup"><span data-stu-id="7e847-137">Anyone who invokes the Index() action will get the same cached version of the Index view.</span></span> <span data-ttu-id="7e847-138">Ciò significa che la quantità di lavoro che il server web deve eseguire per servire la visualizzazione dell'indice viene notevolmente ridotto.</span><span class="sxs-lookup"><span data-stu-id="7e847-138">This means that the amount of work that the web server must perform to serve the Index view is dramatically reduced.</span></span>

<span data-ttu-id="7e847-139">La visualizzazione nel listato 2 si verifica per eseguire un'operazione molto semplice.</span><span class="sxs-lookup"><span data-stu-id="7e847-139">The view in Listing 2 happens to be doing something really simple.</span></span> <span data-ttu-id="7e847-140">La visualizzazione Mostra solo l'ora corrente.</span><span class="sxs-lookup"><span data-stu-id="7e847-140">The view just displays the current time.</span></span> <span data-ttu-id="7e847-141">Tuttavia, è possibile solo come cache con facilità una vista che viene visualizzato un set di record di database.</span><span class="sxs-lookup"><span data-stu-id="7e847-141">However, you could just as easily cache a view that displays a set of database records.</span></span> <span data-ttu-id="7e847-142">In tal caso, il set di record del database non sarebbe necessario da recuperare dal database ogni volta che viene richiamata l'azione del controller che restituisce la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="7e847-142">In that case, the set of database records would not need to be retrieved from the database each and every time the controller action that returns the view is invoked.</span></span> <span data-ttu-id="7e847-143">La memorizzazione nella cache, è possibile ridurre la quantità di lavoro che deve eseguire il server web e server di database.</span><span class="sxs-lookup"><span data-stu-id="7e847-143">Caching can reduce the amount of work that both your web server and database server must perform.</span></span>

<span data-ttu-id="7e847-144">Non utilizzare la pagina &lt;% @ OutputCache %&gt; direttiva in una visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="7e847-144">Don't use the page &lt;%@ OutputCache %&gt; directive in an MVC view.</span></span> <span data-ttu-id="7e847-145">Questa direttiva sanguinare dal mondo Web Form e non deve essere utilizzata in un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7e847-145">This directive is bleeding over from the Web Forms world and should not be used in an ASP.NET MVC application.</span></span>

## <a name="where-content-is-cached"></a><span data-ttu-id="7e847-146">In cui viene memorizzato nella cache il contenuto</span><span class="sxs-lookup"><span data-stu-id="7e847-146">Where Content is Cached</span></span>

<span data-ttu-id="7e847-147">Per impostazione predefinita, quando si usa l'attributo [OutputCache], il contenuto viene memorizzato nella cache in tre posizioni: il server web, tutti i server proxy e il web browser.</span><span class="sxs-lookup"><span data-stu-id="7e847-147">By default, when you use the [OutputCache] attribute, content is cached in three locations: the web server, any proxy servers, and the web browser.</span></span> <span data-ttu-id="7e847-148">È possibile controllare esattamente in cui il contenuto viene memorizzato nella cache modificando la proprietà Location dell'attributo [OutputCache].</span><span class="sxs-lookup"><span data-stu-id="7e847-148">You can control exactly where content is cached by modifying the Location property of the [OutputCache] attribute.</span></span>

<span data-ttu-id="7e847-149">È possibile impostare la proprietà Location a uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="7e847-149">You can set the Location property to any one of the following values:</span></span>

> <span data-ttu-id="7e847-150">· Qualsiasi</span><span class="sxs-lookup"><span data-stu-id="7e847-150">· Any</span></span>
> 
> <span data-ttu-id="7e847-151">· Client</span><span class="sxs-lookup"><span data-stu-id="7e847-151">· Client</span></span>
> 
> <span data-ttu-id="7e847-152">· Downstream</span><span class="sxs-lookup"><span data-stu-id="7e847-152">· Downstream</span></span>
> 
> <span data-ttu-id="7e847-153">· Server</span><span class="sxs-lookup"><span data-stu-id="7e847-153">· Server</span></span>
> 
> <span data-ttu-id="7e847-154">· Nessuno</span><span class="sxs-lookup"><span data-stu-id="7e847-154">· None</span></span>
> 
> <span data-ttu-id="7e847-155">· ServerAndClient</span><span class="sxs-lookup"><span data-stu-id="7e847-155">· ServerAndClient</span></span>


<span data-ttu-id="7e847-156">Per impostazione predefinita, la proprietà Location contiene il valore Any.</span><span class="sxs-lookup"><span data-stu-id="7e847-156">By default, the Location property has the value Any.</span></span> <span data-ttu-id="7e847-157">Tuttavia, esistono situazioni in cui potrebbe voler cache solo in browser o solo sul server.</span><span class="sxs-lookup"><span data-stu-id="7e847-157">However, there are situations in which you might want to cache only on the browser or only on the server.</span></span> <span data-ttu-id="7e847-158">Ad esempio, se si memorizza informazioni personalizzati per ogni utente quindi è necessario non memorizzare nella cache le informazioni sul server.</span><span class="sxs-lookup"><span data-stu-id="7e847-158">For example, if you are caching information that is personalized for each user then you should not cache the information on the server.</span></span> <span data-ttu-id="7e847-159">Se si visualizzano informazioni diverse a utenti diversi è necessario memorizzare nella cache le informazioni solo sul client.</span><span class="sxs-lookup"><span data-stu-id="7e847-159">If you are displaying different information to different users then you should cache the information only on the client.</span></span>

<span data-ttu-id="7e847-160">Ad esempio, il controller nel listato 3 espone un'azione denominata GetName() che restituisce il nome dell'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="7e847-160">For example, the controller in Listing 3 exposes an action named GetName() that returns the current user name.</span></span> <span data-ttu-id="7e847-161">Se Jack accede al sito Web e richiama l'azione GetName() quindi l'azione restituisce la stringa "Hi Jack".</span><span class="sxs-lookup"><span data-stu-id="7e847-161">If Jack logs into the website and invokes the GetName() action then the action returns the string "Hi Jack".</span></span> <span data-ttu-id="7e847-162">Se, successivamente, Jill accede al sito Web e richiama l'azione GetName() quindi Lei anche otterrà la stringa "Hi Jack".</span><span class="sxs-lookup"><span data-stu-id="7e847-162">If, subsequently, Jill logs into the website and invokes the GetName() action then she also will get the string "Hi Jack".</span></span> <span data-ttu-id="7e847-163">La stringa viene memorizzato nella cache sul server web per tutti gli utenti dopo Jack richiama inizialmente l'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="7e847-163">The string is cached on the web server for all users after Jack initially invokes the controller action.</span></span>

<span data-ttu-id="7e847-164">**Listato 3 – Controllers\BadUserController.cs**</span><span class="sxs-lookup"><span data-stu-id="7e847-164">**Listing 3 – Controllers\BadUserController.cs**</span></span>

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

<span data-ttu-id="7e847-165">Molto probabilmente, il controller nel listato 3 non funzionare nel modo desiderato.</span><span class="sxs-lookup"><span data-stu-id="7e847-165">Most likely, the controller in Listing 3 does not work the way that you want.</span></span> <span data-ttu-id="7e847-166">Non si desidera visualizzare il messaggio "Jack Hi" a Jill.</span><span class="sxs-lookup"><span data-stu-id="7e847-166">You don't want to display the message "Hi Jack" to Jill.</span></span>

<span data-ttu-id="7e847-167">Non si devono mai memorizzare nella cache di contenuti personalizzati nella cache del server.</span><span class="sxs-lookup"><span data-stu-id="7e847-167">You should never cache personalized content in the server cache.</span></span> <span data-ttu-id="7e847-168">Tuttavia, si potrebbe voler memorizzare nella cache il contenuto personalizzato nella cache del browser per migliorare le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="7e847-168">However, you might want to cache the personalized content in the browser cache to improve performance.</span></span> <span data-ttu-id="7e847-169">Se si memorizzano nella cache il contenuto nel browser e un utente richiama l'azione del controller stesso più volte, il contenuto può essere recuperato dalla cache del browser invece del server.</span><span class="sxs-lookup"><span data-stu-id="7e847-169">If you cache content in the browser, and a user invokes the same controller action multiple times, then the content can be retrieved from the browser cache instead of the server.</span></span>

<span data-ttu-id="7e847-170">Il controller modificato nel listato 4 memorizza nella cache l'output dell'azione GetName().</span><span class="sxs-lookup"><span data-stu-id="7e847-170">The modified controller in Listing 4 caches the output of the GetName() action.</span></span> <span data-ttu-id="7e847-171">Tuttavia, il contenuto rimane nella cache solo in browser e non nel server.</span><span class="sxs-lookup"><span data-stu-id="7e847-171">However, the content is cached only on the browser and not on the server.</span></span> <span data-ttu-id="7e847-172">In questo modo, quando più utenti richiamano il metodo GetName(), ogni persona che ottiene il proprio nome utente e nome utente della persona non in un'altra.</span><span class="sxs-lookup"><span data-stu-id="7e847-172">That way, when multiple users invoke the GetName() method, each person gets their own user name and not another person's user name.</span></span>

<span data-ttu-id="7e847-173">**Listato 4 – Controllers\UserController.cs**</span><span class="sxs-lookup"><span data-stu-id="7e847-173">**Listing 4 – Controllers\UserController.cs**</span></span>

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

<span data-ttu-id="7e847-174">Si noti che l'attributo [OutputCache] nel listato 4 include una proprietà di posizione impostata sul valore OutputCacheLocation.Client.</span><span class="sxs-lookup"><span data-stu-id="7e847-174">Notice that the [OutputCache] attribute in Listing 4 includes a Location property set to the value OutputCacheLocation.Client.</span></span> <span data-ttu-id="7e847-175">L'attributo [OutputCache] include anche una proprietà NoStore.</span><span class="sxs-lookup"><span data-stu-id="7e847-175">The [OutputCache] attribute also includes a NoStore property.</span></span> <span data-ttu-id="7e847-176">La proprietà NoStore viene utilizzata per indicare di browser e server proxy che non devono archiviare una copia permanente del contenuto memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="7e847-176">The NoStore property is used to inform proxy servers and browser that they should not store a permanent copy of the cached content.</span></span>

## <a name="varying-the-output-cache"></a><span data-ttu-id="7e847-177">Variare la Cache di Output</span><span class="sxs-lookup"><span data-stu-id="7e847-177">Varying the Output Cache</span></span>

<span data-ttu-id="7e847-178">In alcune situazioni, è possibile utilizzare diverse versioni memorizzate nella cache il contenuto stesso.</span><span class="sxs-lookup"><span data-stu-id="7e847-178">In some situations, you might want different cached versions of the very same content.</span></span> <span data-ttu-id="7e847-179">Si supponga, ad esempio, che si sta creando una pagina master-details.</span><span class="sxs-lookup"><span data-stu-id="7e847-179">Imagine, for example, that you are creating a master/detail page.</span></span> <span data-ttu-id="7e847-180">La pagina master consente di visualizzare un elenco di titoli di film.</span><span class="sxs-lookup"><span data-stu-id="7e847-180">The master page displays a list of movie titles.</span></span> <span data-ttu-id="7e847-181">Quando si fa clic su un titolo, si ricevono dettagli per il film selezionato.</span><span class="sxs-lookup"><span data-stu-id="7e847-181">When you click a title, you get details for the selected movie.</span></span>

<span data-ttu-id="7e847-182">Se si memorizza nella cache la pagina dei dettagli, i dettagli per lo stesso film verranno quindi visualizzati indipendentemente da quale film si fa clic su.</span><span class="sxs-lookup"><span data-stu-id="7e847-182">If you cache the details page, then the details for the same movie will be displayed no matter which movie you click.</span></span> <span data-ttu-id="7e847-183">Verrà visualizzato il primo film selezionato dall'utente prima a tutti gli utenti future.</span><span class="sxs-lookup"><span data-stu-id="7e847-183">The first movie selected by the first user will be displayed to all future users.</span></span>

<span data-ttu-id="7e847-184">È possibile risolvere questo problema, sfruttando i vantaggi della proprietà VaryByParam dell'attributo [OutputCache].</span><span class="sxs-lookup"><span data-stu-id="7e847-184">You can fix this problem by taking advantage of the VaryByParam property of the [OutputCache] attribute.</span></span> <span data-ttu-id="7e847-185">Questa proprietà consente di creare diverse versioni memorizzate nella cache del contenuto molto stesso quando un parametro per il form o parametro della stringa di query varia.</span><span class="sxs-lookup"><span data-stu-id="7e847-185">This property enables you to create different cached versions of the very same content when a form parameter or query string parameter varies.</span></span>

<span data-ttu-id="7e847-186">Ad esempio, il controller nel listato 5 espone due azioni denominate Master() e Details().</span><span class="sxs-lookup"><span data-stu-id="7e847-186">For example, the controller in Listing 5 exposes two actions named Master() and Details().</span></span> <span data-ttu-id="7e847-187">L'azione Master() restituisce un elenco di titoli di film e l'azione Details() restituisce i dettagli per il film selezionato.</span><span class="sxs-lookup"><span data-stu-id="7e847-187">The Master() action returns a list of movie titles and the Details() action returns the details for the selected movie.</span></span>

<span data-ttu-id="7e847-188">**Listato 5 – Controllers\MoviesController.cs**</span><span class="sxs-lookup"><span data-stu-id="7e847-188">**Listing 5 – Controllers\MoviesController.cs**</span></span>

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

<span data-ttu-id="7e847-189">L'azione Master() includa una proprietà VaryByParam con il valore "none".</span><span class="sxs-lookup"><span data-stu-id="7e847-189">The Master() action includes a VaryByParam property with the value "none".</span></span> <span data-ttu-id="7e847-190">Viene restituito quando visualizzano il Master() viene richiamata l'azione, la stessa versione memorizzata nella cache del Master.</span><span class="sxs-lookup"><span data-stu-id="7e847-190">When the Master() action is invoked, the same cached version of the Master view is returned.</span></span> <span data-ttu-id="7e847-191">Eventuali parametri del modulo o la stringa di query i parametri vengono ignorati (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="7e847-191">Any form parameters or query string parameters are ignored (see Figure 2).</span></span>

<span data-ttu-id="7e847-192">**Figura 2: la vista /Movies/Master**</span><span class="sxs-lookup"><span data-stu-id="7e847-192">**Figure 2 – The /Movies/Master view**</span></span>

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

<span data-ttu-id="7e847-194">**Figura 3 – vista Dettagli/Movies /**</span><span class="sxs-lookup"><span data-stu-id="7e847-194">**Figure 3 – The /Movies/Details view**</span></span>

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

<span data-ttu-id="7e847-196">L'azione Details() include una proprietà VaryByParam con il valore "Id".</span><span class="sxs-lookup"><span data-stu-id="7e847-196">The Details() action includes a VaryByParam property with the value "Id".</span></span> <span data-ttu-id="7e847-197">Quando vengono passati valori diversi del parametro Id per l'azione del controller, vengono generate diverse versioni memorizzate nella cache della vista Dettagli.</span><span class="sxs-lookup"><span data-stu-id="7e847-197">When different values of the Id parameter are passed to the controller action, different cached versions of the Details view are generated.</span></span>

<span data-ttu-id="7e847-198">È importante comprendere che con la proprietà VaryByParam determina la memorizzazione nella cache più e meno.</span><span class="sxs-lookup"><span data-stu-id="7e847-198">It is important to understand that using the VaryByParam property results in more caching and not less.</span></span> <span data-ttu-id="7e847-199">Una versione memorizzata nella cache diversa della visualizzazione dettagli viene creata per ogni versione del parametro Id.</span><span class="sxs-lookup"><span data-stu-id="7e847-199">A different cached version of the Details view is created for each different version of the Id parameter.</span></span>

<span data-ttu-id="7e847-200">È possibile impostare la proprietà VaryByParam sui valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="7e847-200">You can set the VaryByParam property to the following values:</span></span>

> <span data-ttu-id="7e847-201">\* = Crea una versione memorizzata nella cache diversa ogni volta che varia in un parametro di stringa di formato o la query.</span><span class="sxs-lookup"><span data-stu-id="7e847-201">\* = Create a different cached version whenever a form or query string parameter varies.</span></span>
> 
> <span data-ttu-id="7e847-202">None = mai creare diverse versioni memorizzate nella cache</span><span class="sxs-lookup"><span data-stu-id="7e847-202">none = Never create different cached versions</span></span>
> 
> <span data-ttu-id="7e847-203">Elenco di punti e virgola di parametri = crea diverse versioni memorizzate nella cache, ogni volta che uno dei parametri di stringa query o di modulo nell'elenco varia</span><span class="sxs-lookup"><span data-stu-id="7e847-203">Semicolon list of parameters = Create different cached versions whenever any of the form or query string parameters in the list varies</span></span>


## <a name="creating-a-cache-profile"></a><span data-ttu-id="7e847-204">Creazione di un profilo della Cache</span><span class="sxs-lookup"><span data-stu-id="7e847-204">Creating a Cache Profile</span></span>

<span data-ttu-id="7e847-205">Come alternativa alla configurazione delle proprietà della cache di output modificando le proprietà dell'attributo [OutputCache], è possibile creare un profilo della cache nel file di configurazione (Web. config) web.</span><span class="sxs-lookup"><span data-stu-id="7e847-205">As an alternative to configuring output cache properties by modifying properties of the [OutputCache] attribute, you can create a cache profile in the web configuration (web.config) file.</span></span> <span data-ttu-id="7e847-206">Creazione di un profilo di cache nel file di configurazione web offre due vantaggi importanti.</span><span class="sxs-lookup"><span data-stu-id="7e847-206">Creating a cache profile in the web configuration file offers a couple of important advantages.</span></span>

<span data-ttu-id="7e847-207">In primo luogo, configurando la memorizzazione nella cache di output nel file di configurazione web, è possibile controllare come le azioni del controller nella cache il contenuto in un'unica posizione centrale.</span><span class="sxs-lookup"><span data-stu-id="7e847-207">First, by configuring output caching in the web configuration file, you can control how controller actions cache content in one central location.</span></span> <span data-ttu-id="7e847-208">È possibile creare un profilo della cache e applicare il profilo ai diversi controller o azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="7e847-208">You can create one cache profile and apply the profile to several controllers or controller actions.</span></span>

<span data-ttu-id="7e847-209">In secondo luogo, è possibile modificare il file di configurazione web senza ricompilare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7e847-209">Second, you can modify the web configuration file without recompiling your application.</span></span> <span data-ttu-id="7e847-210">Se è necessario disabilitare la memorizzazione nella cache per un'applicazione che è già stata distribuita nell'ambiente di produzione, è possibile modificare semplicemente i profili della cache definiti nel file di configurazione web.</span><span class="sxs-lookup"><span data-stu-id="7e847-210">If you need to disable caching for an application that has already been deployed to production, then you can simply modify the cache profiles defined in the web configuration file.</span></span> <span data-ttu-id="7e847-211">Verranno rilevate automaticamente e applicare eventuali modifiche al file di configurazione web.</span><span class="sxs-lookup"><span data-stu-id="7e847-211">Any changes to the web configuration file will be detected automatically and applied.</span></span>

<span data-ttu-id="7e847-212">Ad esempio, il &lt;memorizzazione nella cache&gt; sezione di configurazione web nel listato 6 definisce un profilo della cache denominato Cache1Hour.</span><span class="sxs-lookup"><span data-stu-id="7e847-212">For example, the &lt;caching&gt; web configuration section in Listing 6 defines a cache profile named Cache1Hour.</span></span> <span data-ttu-id="7e847-213">Il &lt;memorizzazione nella cache&gt; sezione deve essere racchiuso tra i &lt;System. Web&gt; sezione di un file di configurazione web.</span><span class="sxs-lookup"><span data-stu-id="7e847-213">The &lt;caching&gt; section must appear within the &lt;system.web&gt; section of a web configuration file.</span></span>

<span data-ttu-id="7e847-214">**Listato 6 – sezione memorizzazione nella cache per Web. config**</span><span class="sxs-lookup"><span data-stu-id="7e847-214">**Listing 6 – Caching section for web.config**</span></span>

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

<span data-ttu-id="7e847-215">Il controller nel listato 7 viene illustrato come è possibile applicare il profilo Cache1Hour a un'azione del controller con l'attributo [OutputCache].</span><span class="sxs-lookup"><span data-stu-id="7e847-215">The controller in Listing 7 illustrates how you can apply the Cache1Hour profile to a controller action with the [OutputCache] attribute.</span></span>

<span data-ttu-id="7e847-216">**Listato 7 – Controllers\ProfileController.cs**</span><span class="sxs-lookup"><span data-stu-id="7e847-216">**Listing 7 – Controllers\ProfileController.cs**</span></span>

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

<span data-ttu-id="7e847-217">Se si richiama l'azione Index () esposto dal controller nel listato 7, verrà restituito contemporaneamente per 1 ora.</span><span class="sxs-lookup"><span data-stu-id="7e847-217">If you invoke the Index() action exposed by the controller in Listing 7 then the same time will be returned for 1 hour.</span></span>

## <a name="summary"></a><span data-ttu-id="7e847-218">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="7e847-218">Summary</span></span>

<span data-ttu-id="7e847-219">La memorizzazione nella cache di output fornisce un metodo molto semplice di migliorare notevolmente le prestazioni delle applicazioni ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7e847-219">Output caching provides you with a very easy method of dramatically improving the performance of your ASP.NET MVC applications.</span></span> <span data-ttu-id="7e847-220">In questa esercitazione è stato descritto come usare l'attributo [OutputCache] per memorizzare nella cache l'output di azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="7e847-220">In this tutorial, you learned how to use the [OutputCache] attribute to cache the output of controller actions.</span></span> <span data-ttu-id="7e847-221">Inoltre appreso come modificare le proprietà dell'attributo [OutputCache] ad esempio le proprietà di durata e VaryByParam per modificare la modalità con cui viene memorizzato nella cache il contenuto.</span><span class="sxs-lookup"><span data-stu-id="7e847-221">You also learned how to modify properties of the [OutputCache] attribute such as the Duration and VaryByParam properties to modify how content gets cached.</span></span> <span data-ttu-id="7e847-222">Infine, si è appreso come definire profili per la cache nel file di configurazione web.</span><span class="sxs-lookup"><span data-stu-id="7e847-222">Finally, you learned how to define cache profiles in the web configuration file.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7e847-223">[Precedente](understanding-action-filters-cs.md)
> [Successivo](adding-dynamic-content-to-a-cached-page-cs.md)</span><span class="sxs-lookup"><span data-stu-id="7e847-223">[Previous](understanding-action-filters-cs.md)
[Next](adding-dynamic-content-to-a-cached-page-cs.md)</span></span>