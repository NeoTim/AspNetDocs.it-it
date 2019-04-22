---
uid: web-api/overview/advanced/dependency-injection
title: Inserimento delle dipendenze in ASP.NET Web API 2 - ASP.NET 4.x
author: MikeWasson
description: Questa esercitazione illustra come inserire le dipendenze nel controller dell'API Web ASP.NET per ASP.NET 4.x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 0ad0b3c63741803e05274df4da3fcbe5481d32a4
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59391934"
---
# <a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="e9a0d-103">Inserimento delle dipendenze in ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="e9a0d-103">Dependency Injection in ASP.NET Web API 2</span></span>

<span data-ttu-id="e9a0d-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e9a0d-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e9a0d-105">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="e9a0d-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="e9a0d-106">Questa esercitazione illustra come inserire le dipendenze nel controller dell'API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-106">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e9a0d-107">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="e9a0d-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="e9a0d-108">API Web 2</span><span class="sxs-lookup"><span data-stu-id="e9a0d-108">Web API 2</span></span>
> - [<span data-ttu-id="e9a0d-109">Unity Application Block</span><span class="sxs-lookup"><span data-stu-id="e9a0d-109">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="e9a0d-110">Entity Framework 6 (versione 5 funziona anche)</span><span class="sxs-lookup"><span data-stu-id="e9a0d-110">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="e9a0d-111">Che cos'è l'inserimento delle dipendenze?</span><span class="sxs-lookup"><span data-stu-id="e9a0d-111">What is Dependency Injection?</span></span>

<span data-ttu-id="e9a0d-112">Una *dipendenza* è qualsiasi oggetto richiesto da un altro oggetto.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-112">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="e9a0d-113">Ad esempio, è comune per definire un [repository](http://martinfowler.com/eaaCatalog/repository.html) che gestisce l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-113">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="e9a0d-114">Di seguito è illustrato un esempio.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-114">Let's illustrate with an example.</span></span> <span data-ttu-id="e9a0d-115">In primo luogo, si definirà un modello di dominio:</span><span class="sxs-lookup"><span data-stu-id="e9a0d-115">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="e9a0d-116">Di seguito è una classe di repository semplice che archivia gli elementi in un database usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-116">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="e9a0d-117">A questo punto è possibile definire un controller API Web che supporta le richieste GET per `Product` entità.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-117">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="e9a0d-118">(Spese di spedizione POST e altri metodi per motivi di semplicità.) Di seguito è un primo tentativo:</span><span class="sxs-lookup"><span data-stu-id="e9a0d-118">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="e9a0d-119">Si noti che la classe controller dipende `ProductRepository`, e si sta consentendo il controller crea il `ProductRepository` istanza.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-119">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="e9a0d-120">Tuttavia, è opportuno evitare di livello di codice in questo modo, la dipendenza per diversi motivi.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-120">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="e9a0d-121">Se si desidera sostituire `ProductRepository` con un'implementazione diversa, è anche necessario modificare la classe controller.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-121">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="e9a0d-122">Se il `ProductRepository` ha dipendenze, è necessario configurare questi all'interno del controller.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-122">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="e9a0d-123">Per un progetto di grandi dimensioni con più controller, il codice di configurazione diventa sparsi tra il progetto.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-123">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="e9a0d-124">È difficile allo unit test, perché il controller è hardcoded su query del database.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-124">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="e9a0d-125">Per uno unit test, è consigliabile usare un repository fittizi o stub, che non è possibile eseguire con la progettazione corrente.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-125">For a unit test, you should use a mock or stub repository, which is not possible with the current design.</span></span>

<span data-ttu-id="e9a0d-126">Possiamo risolvere questi problemi dal *inserimento* il repository nel controller.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-126">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="e9a0d-127">In primo luogo, eseguire il refactoring di `ProductRepository` classe in un'interfaccia:</span><span class="sxs-lookup"><span data-stu-id="e9a0d-127">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="e9a0d-128">Fornire quindi il `IProductRepository` come parametro di costruttore:</span><span class="sxs-lookup"><span data-stu-id="e9a0d-128">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="e9a0d-129">Questo esempio viene usato [inserimento del costruttore](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="e9a0d-129">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="e9a0d-130">È anche possibile usare *inserimento di setter*, in cui è necessario impostare la dipendenza attraverso un metodo di impostazione o una proprietà.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-130">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="e9a0d-131">Ma Ecco presentarsi un problema, perché l'applicazione non crea direttamente il controller.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-131">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="e9a0d-132">API Web crea il controller quando indirizza la richiesta e API Web non riconosce i `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-132">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="e9a0d-133">Questo è il resolver di dipendenza di API Web entra in gioco.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-133">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="e9a0d-134">Il Resolver di dipendenza di API Web</span><span class="sxs-lookup"><span data-stu-id="e9a0d-134">The Web API Dependency Resolver</span></span>

<span data-ttu-id="e9a0d-135">API Web definisce la **IDependencyResolver** interfaccia per la risoluzione delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-135">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="e9a0d-136">Ecco la definizione dell'interfaccia:</span><span class="sxs-lookup"><span data-stu-id="e9a0d-136">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="e9a0d-137">Il **IDependencyScope** interfaccia dispone di due metodi:</span><span class="sxs-lookup"><span data-stu-id="e9a0d-137">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="e9a0d-138">**GetService** crea un'istanza di un tipo.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-138">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="e9a0d-139">**GetServices** crea una raccolta di oggetti di un tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-139">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="e9a0d-140">Il **IDependencyResolver** metodo eredita **IDependencyScope** e aggiunge i **BeginScope** (metodo).</span><span class="sxs-lookup"><span data-stu-id="e9a0d-140">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="e9a0d-141">Gli ambiti saranno trattati più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-141">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="e9a0d-142">Quando l'API Web crea un'istanza del controller, viene prima chiamato **IDependencyResolver.GetService**, passando il tipo di controller.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-142">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="e9a0d-143">È possibile utilizzare questo hook di estensibilità per creare il controller, la risoluzione di eventuali dipendenze.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-143">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="e9a0d-144">Se **GetService** restituisce null, API Web cerca un costruttore senza parametri nella classe controller.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-144">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="e9a0d-145">Risoluzione delle dipendenze con il contenitore di Unity</span><span class="sxs-lookup"><span data-stu-id="e9a0d-145">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="e9a0d-146">Anche se è possibile scrivere una completa **IDependencyResolver** implementazione da zero, l'interfaccia è realmente progettato per fungere da ponte tra API Web e i contenitori IoC esistenti.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-146">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="e9a0d-147">Un contenitore IoC è un componente software che è responsabile della gestione delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-147">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="e9a0d-148">Registrare i tipi con il contenitore e quindi usare il contenitore per creare oggetti.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-148">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="e9a0d-149">Il contenitore determina automaticamente le relazioni di dipendenza.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-149">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="e9a0d-150">Numerosi contenitori IoC consentono anche di controllare elementi quali la durata degli oggetti e l'ambito.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-150">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="e9a0d-151">"IoC" è l'acronimo di "inversione di controllo", ovvero un modello generale in cui un framework chiama il codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-151">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="e9a0d-152">Un contenitore IoC costruisce oggetti, quali "inverte" il normale flusso di controllo.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-152">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="e9a0d-153">Per questa esercitazione, useremo [Unity](https://msdn.microsoft.com/library/ff647202.aspx) da Microsoft Patterns &amp; procedure consigliate.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-153">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="e9a0d-154">(Includono altre librerie molto diffuse [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), e [StructureMap ](http://structuremap.github.io/documentation/).) È possibile usare Gestione pacchetti NuGet per installare Unity.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-154">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://structuremap.github.io/documentation/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="e9a0d-155">Dal **strumenti** menu in Visual Studio, selezionare **Gestione pacchetti NuGet**, quindi **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-155">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="e9a0d-156">Nella finestra della Console di gestione pacchetti, digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e9a0d-156">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="e9a0d-157">Ecco un'implementazione di **IDependencyResolver** che esegue il wrapping di un contenitore di Unity.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-157">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="e9a0d-158">Se il **GetService** metodo non è possibile risolvere un tipo, deve restituire **null**.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-158">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="e9a0d-159">Se il **GetServices** (metodo) non è possibile risolvere un tipo, deve restituire un oggetto raccolta vuoto.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-159">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="e9a0d-160">Non generare eccezioni per i tipi sconosciuti.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-160">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="e9a0d-161">Configurare il Resolver di dipendenza</span><span class="sxs-lookup"><span data-stu-id="e9a0d-161">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="e9a0d-162">Impostare il resolver di dipendenza per il **DependencyResolver** proprietà dell'oggetto globale **HttpConfiguration** oggetto.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-162">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="e9a0d-163">Il codice seguente registra il `IProductRepository` interfacciarsi con Unity e quindi crea un `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-163">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="e9a0d-164">Ambito delle dipendenze e la durata di Controller</span><span class="sxs-lookup"><span data-stu-id="e9a0d-164">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="e9a0d-165">I controller vengono creati per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-165">Controllers are created per request.</span></span> <span data-ttu-id="e9a0d-166">Per la gestione della durata degli oggetti, **IDependencyResolver** Usa il concetto di un *ambito*.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-166">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="e9a0d-167">Il resolver di dipendenza associata ai **HttpConfiguration** oggetto ha un ambito globale.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-167">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="e9a0d-168">Quando l'API Web crea un controller, viene chiamato **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-168">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="e9a0d-169">Questo metodo restituisce un **IDependencyScope** che rappresenta un ambito figlio.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-169">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="e9a0d-170">API Web chiamerà **GetService** nell'ambito figlio per creare il controller.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-170">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="e9a0d-171">Quando richiesta risulta completata, l'API Web chiama **Dispose** nell'ambito figlio.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-171">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="e9a0d-172">Usare la **Dispose** metodo per l'eliminazione delle dipendenze del controller.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-172">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="e9a0d-173">Modalità di implementazione **BeginScope** dipende dal contenitore IoC.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-173">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="e9a0d-174">Per Unity, ambito corrisponde a un contenitore figlio:</span><span class="sxs-lookup"><span data-stu-id="e9a0d-174">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="e9a0d-175">La maggior parte dei contenitori IoC dispongono di equivalenti simile.</span><span class="sxs-lookup"><span data-stu-id="e9a0d-175">Most IoC containers have similar equivalents.</span></span>
