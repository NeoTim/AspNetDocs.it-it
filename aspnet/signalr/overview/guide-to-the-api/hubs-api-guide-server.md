---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Guida all'API di ASP.NET HubS di SignalR - Server (C ) Documenti Microsoft
author: bradygaster
description: Questo documento fornisce un'introduzione alla programmazione del lato server dell'API di ASP.NET SignalR Hubs per SignalR versione 2, con esempi di codice che dimostrano...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c681b104b15bfc4a04587c7abf685dcf20def2ca
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676188"
---
# <a name="aspnet-signalr-hubs-api-guide---server-c"></a><span data-ttu-id="197c4-103">Guida all'API di ASP.NET SignalR Hubs - Server (C )ASP.NET SignalR Hubs API Guide - Server (C</span><span class="sxs-lookup"><span data-stu-id="197c4-103">ASP.NET SignalR Hubs API Guide - Server (C#)</span></span>

<span data-ttu-id="197c4-104">di [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="197c4-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="197c4-105">Questo documento fornisce un'introduzione alla programmazione del lato server dell'API signalR Hubs ASP.NET per SignalR versione 2, con esempi di codice che illustrano le opzioni comuni.</span><span class="sxs-lookup"><span data-stu-id="197c4-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 2, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="197c4-106">L'API Hubs SignalR consente di effettuare chiamate di procedura remota (RPC) da un server ai client connessi e dai client al server.</span><span class="sxs-lookup"><span data-stu-id="197c4-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="197c4-107">Nel codice server, si definiscono i metodi che possono essere chiamati dai client e si chiamano i metodi eseguiti sul client.</span><span class="sxs-lookup"><span data-stu-id="197c4-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="197c4-108">Nel codice client si definiscono i metodi che possono essere chiamati dal server e si chiamano i metodi eseguiti sul server.</span><span class="sxs-lookup"><span data-stu-id="197c4-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="197c4-109">SignalR si occupa di tutto l'impianto idraulico client-server per voi.</span><span class="sxs-lookup"><span data-stu-id="197c4-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="197c4-110">SignalR offre anche un'API di livello inferiore denominata connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="197c4-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="197c4-111">Per un'introduzione a SignalR, Hub e connessioni permanenti, vedere [Introduzione a SignalR 2](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="197c4-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR 2](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="197c4-112">Versioni software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="197c4-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="197c4-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="197c4-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="197c4-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="197c4-114">.NET 4.5</span></span>
> - <span data-ttu-id="197c4-115">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="197c4-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="topic-versions"></a><span data-ttu-id="197c4-116">Versioni degli argomenti</span><span class="sxs-lookup"><span data-stu-id="197c4-116">Topic versions</span></span>
> 
> <span data-ttu-id="197c4-117">Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR Older Versions](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="197c4-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="197c4-118">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="197c4-118">Questions and comments</span></span>
> 
> <span data-ttu-id="197c4-119">Si prega di lasciare un feedback su come ti è piaciuto questo tutorial e cosa potremmo migliorare nei commenti in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="197c4-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="197c4-120">Se avete domande che non sono direttamente correlati al tutorial, è possibile pubblicarli al [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="197c4-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="197c4-121">Panoramica</span><span class="sxs-lookup"><span data-stu-id="197c4-121">Overview</span></span>

<span data-ttu-id="197c4-122">Questo documento contiene le seguenti sezioni:</span><span class="sxs-lookup"><span data-stu-id="197c4-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="197c4-123">Come registrare SignalR middleware</span><span class="sxs-lookup"><span data-stu-id="197c4-123">How to register SignalR middleware</span></span>](#route)

    - [<span data-ttu-id="197c4-124">L'URL /signalr</span><span class="sxs-lookup"><span data-stu-id="197c4-124">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="197c4-125">Configurazione delle opzioni SignalR</span><span class="sxs-lookup"><span data-stu-id="197c4-125">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="197c4-126">Come creare e usare le classi HubHow to create and use Hub classes</span><span class="sxs-lookup"><span data-stu-id="197c4-126">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="197c4-127">Durata dell'oggetto hub</span><span class="sxs-lookup"><span data-stu-id="197c4-127">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="197c4-128">Cammello-casing dei nomi Hub nei client JavaScript</span><span class="sxs-lookup"><span data-stu-id="197c4-128">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="197c4-129">Hub multipli</span><span class="sxs-lookup"><span data-stu-id="197c4-129">Multiple Hubs</span></span>](#multiplehubs)
    - [<span data-ttu-id="197c4-130">Hub fortemente tipati</span><span class="sxs-lookup"><span data-stu-id="197c4-130">Strongly-Typed Hubs</span></span>](#stronglytypedhubs)
- [<span data-ttu-id="197c4-131">Come definire i metodi nella classe Hub che i client possono chiamare</span><span class="sxs-lookup"><span data-stu-id="197c4-131">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="197c4-132">Cammello-casing dei nomi dei metodi nei client JavaScript</span><span class="sxs-lookup"><span data-stu-id="197c4-132">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="197c4-133">Quando eseguire in modo asincronoWhen to execute asynchronously</span><span class="sxs-lookup"><span data-stu-id="197c4-133">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="197c4-134">Definizione degli overloadDefining overloads</span><span class="sxs-lookup"><span data-stu-id="197c4-134">Defining overloads</span></span>](#overloads)
    - [<span data-ttu-id="197c4-135">Segnalazione dello stato di avanzamento dalle chiamate al metodo hubReporting progress from hub method invocations</span><span class="sxs-lookup"><span data-stu-id="197c4-135">Reporting progress from hub method invocations</span></span>](#progress)
- [<span data-ttu-id="197c4-136">Come chiamare i metodi client dalla classe HubHow to call client methods from the Hub class</span><span class="sxs-lookup"><span data-stu-id="197c4-136">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="197c4-137">Selezione dei client che riceveranno la</span><span class="sxs-lookup"><span data-stu-id="197c4-137">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="197c4-138">Nessuna convalida in fase di compilazione per i nomi dei metodi</span><span class="sxs-lookup"><span data-stu-id="197c4-138">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="197c4-139">Corrispondenza del nome del metodo senza distinzione tra maiuscole e minuscole</span><span class="sxs-lookup"><span data-stu-id="197c4-139">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="197c4-140">Esecuzione asincrona</span><span class="sxs-lookup"><span data-stu-id="197c4-140">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="197c4-141">Come gestire l'appartenenza al gruppo dalla classe Hub</span><span class="sxs-lookup"><span data-stu-id="197c4-141">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="197c4-142">Esecuzione asincrona dei metodi Add e Remove</span><span class="sxs-lookup"><span data-stu-id="197c4-142">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="197c4-143">Persistenza dell'appartenenza al gruppo</span><span class="sxs-lookup"><span data-stu-id="197c4-143">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="197c4-144">Gruppi di utenti singoli</span><span class="sxs-lookup"><span data-stu-id="197c4-144">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="197c4-145">Come gestire gli eventi di durata della connessione nella classe HubHow to handle connection lifetime events in the Hub class</span><span class="sxs-lookup"><span data-stu-id="197c4-145">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="197c4-146">Quando OnConnected, OnDisconnected e OnReconnected vengono chiamati</span><span class="sxs-lookup"><span data-stu-id="197c4-146">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="197c4-147">Stato del chiamante non popolato</span><span class="sxs-lookup"><span data-stu-id="197c4-147">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="197c4-148">Come ottenere informazioni sul client dalla proprietà Context</span><span class="sxs-lookup"><span data-stu-id="197c4-148">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="197c4-149">Come passare lo stato tra i client e la classe HubHow to pass state between clients and the Hub class</span><span class="sxs-lookup"><span data-stu-id="197c4-149">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="197c4-150">Come gestire gli errori nella classe HubHow to handle errors in the Hub class</span><span class="sxs-lookup"><span data-stu-id="197c4-150">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="197c4-151">Come chiamare i metodi client e gestire i gruppi dall'esterno della classe Hub</span><span class="sxs-lookup"><span data-stu-id="197c4-151">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="197c4-152">Chiamata di metodi clientCalling client methods</span><span class="sxs-lookup"><span data-stu-id="197c4-152">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="197c4-153">Gestione dell'appartenenza ai gruppi</span><span class="sxs-lookup"><span data-stu-id="197c4-153">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="197c4-154">Come abilitare la traccia</span><span class="sxs-lookup"><span data-stu-id="197c4-154">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="197c4-155">Come personalizzare la pipeline Hubs</span><span class="sxs-lookup"><span data-stu-id="197c4-155">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="197c4-156">Per la documentazione su come programmare i client, vedere le risorse seguenti:For documentation on how to program clients, see the following resources:</span><span class="sxs-lookup"><span data-stu-id="197c4-156">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="197c4-157">Guida all'API di SignalR Hubs - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="197c4-157">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)
- [<span data-ttu-id="197c4-158">Guida all'API di SignalR Hubs - Client .NET</span><span class="sxs-lookup"><span data-stu-id="197c4-158">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="197c4-159">I componenti server per SignalR 2 sono disponibili solo in .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="197c4-159">The server components for SignalR 2 are only available in .NET 4.5.</span></span> <span data-ttu-id="197c4-160">I server che eseguono .NET 4.0 devono utilizzare SignalR v1.x.</span><span class="sxs-lookup"><span data-stu-id="197c4-160">Servers running .NET 4.0 must use SignalR v1.x.</span></span>

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a><span data-ttu-id="197c4-161">Come registrare SignalR middleware</span><span class="sxs-lookup"><span data-stu-id="197c4-161">How to register SignalR middleware</span></span>

<span data-ttu-id="197c4-162">Per definire la route che i client utilizzeranno `MapSignalR` per connettersi all'hub, chiamare il metodo all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="197c4-162">To define the route that clients will use to connect to your Hub, call the `MapSignalR` method when the application starts.</span></span> <span data-ttu-id="197c4-163">`MapSignalR`è un [metodo](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) `OwinExtensions` di estensione per la classe.</span><span class="sxs-lookup"><span data-stu-id="197c4-163">`MapSignalR` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `OwinExtensions` class.</span></span> <span data-ttu-id="197c4-164">Nell'esempio seguente viene illustrato come definire la route SignalR Hubs utilizzando una classe di avvio OWIN.</span><span class="sxs-lookup"><span data-stu-id="197c4-164">The following example shows how to define the SignalR Hubs route using an OWIN startup class.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

<span data-ttu-id="197c4-165">Se si aggiunge la funzionalità SignalR a un'applicazione MVC ASP.NET, assicurarsi che la route SignalR venga aggiunta prima delle altre route.</span><span class="sxs-lookup"><span data-stu-id="197c4-165">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="197c4-166">Per ulteriori informazioni, vedere [Esercitazione: Introduzione a SignalR 2 e MVC 5.](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)</span><span class="sxs-lookup"><span data-stu-id="197c4-166">For more information, see [Tutorial: Getting Started with SignalR 2 and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="197c4-167">L'URL /signalr</span><span class="sxs-lookup"><span data-stu-id="197c4-167">The /signalr URL</span></span>

<span data-ttu-id="197c4-168">Per impostazione predefinita, l'URL della route che i client utilizzeranno per connettersi all'hub è "/signalr".</span><span class="sxs-lookup"><span data-stu-id="197c4-168">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="197c4-169">(Non confondere questo URL con l'URL "/signalr/hubs", che è per il file JavaScript generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="197c4-169">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="197c4-170">Per altre informazioni sul proxy generato, vedere [SignalR Hubs API Guide - JavaScript Client - Il proxy generato e le operazioni eseguite per l'utente.)](hubs-api-guide-javascript-client.md#genproxy)</span><span class="sxs-lookup"><span data-stu-id="197c4-170">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).)</span></span>

<span data-ttu-id="197c4-171">Ci potrebbero essere circostanze straordinarie che rendono questo URL di base non utilizzabile per SignalR; ad esempio, si dispone di una cartella nel progetto denominata *signalr* e non si desidera modificare il nome.</span><span class="sxs-lookup"><span data-stu-id="197c4-171">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="197c4-172">In tal caso, è possibile modificare l'URL di base, come illustrato negli esempi seguenti (sostituire "/signalr" nel codice di esempio con l'URL desiderato).</span><span class="sxs-lookup"><span data-stu-id="197c4-172">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="197c4-173">**Codice server che specifica l'URL**</span><span class="sxs-lookup"><span data-stu-id="197c4-173">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

<span data-ttu-id="197c4-174">**Codice client JavaScript che specifica l'URL (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="197c4-174">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

<span data-ttu-id="197c4-175">**Codice client JavaScript che specifica l'URL (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="197c4-175">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="197c4-176">**Codice client .NET che specifica l'URL**</span><span class="sxs-lookup"><span data-stu-id="197c4-176">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="197c4-177">Configurazione delle opzioni SignalR</span><span class="sxs-lookup"><span data-stu-id="197c4-177">Configuring SignalR Options</span></span>

<span data-ttu-id="197c4-178">Gli overload `MapSignalR` del metodo consentono di specificare un URL personalizzato, un sistema di risoluzione delle dipendenze personalizzato e le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="197c4-178">Overloads of the `MapSignalR` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="197c4-179">Abilitare le chiamate tra domini usando CORS o JSONP dai client browser.</span><span class="sxs-lookup"><span data-stu-id="197c4-179">Enable cross-domain calls using CORS or JSONP from browser clients.</span></span>

    <span data-ttu-id="197c4-180">In genere se il `http://contoso.com`browser carica una pagina da , `http://contoso.com/signalr`la connessione SignalR si trova nello stesso dominio, in .</span><span class="sxs-lookup"><span data-stu-id="197c4-180">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="197c4-181">Se la `http://contoso.com` pagina da `http://fabrikam.com/signalr`effettua una connessione a , si tratta di una connessione tra domini.</span><span class="sxs-lookup"><span data-stu-id="197c4-181">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="197c4-182">Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="197c4-182">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="197c4-183">Per ulteriori informazioni, vedere [ASP.NET Guida all'API SignalR Hubs - JavaScript Client - Come stabilire una connessione tra](hubs-api-guide-javascript-client.md#crossdomain)domini .</span><span class="sxs-lookup"><span data-stu-id="197c4-183">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](hubs-api-guide-javascript-client.md#crossdomain).</span></span>
- <span data-ttu-id="197c4-184">Abilitare i messaggi di errore dettagliati.</span><span class="sxs-lookup"><span data-stu-id="197c4-184">Enable detailed error messages.</span></span>

    <span data-ttu-id="197c4-185">Quando si verificano errori, il comportamento predefinito di SignalR consiste nell'inviare ai client un messaggio di notifica senza dettagli su ciò che è accaduto.</span><span class="sxs-lookup"><span data-stu-id="197c4-185">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="197c4-186">L'invio di informazioni dettagliate sugli errori ai client non è consigliato nell'ambiente di produzione, perché gli utenti malintenzionati potrebbero essere in grado di utilizzare le informazioni in attacchi contro l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="197c4-186">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="197c4-187">Per la risoluzione dei problemi, è possibile utilizzare questa opzione per abilitare temporaneamente la segnalazione degli errori più informativa.</span><span class="sxs-lookup"><span data-stu-id="197c4-187">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="197c4-188">Disabilitare i file proxy JavaScript generati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="197c4-188">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="197c4-189">Per impostazione predefinita, un file JavaScript con proxy per le classi Hub viene generato in risposta all'URL "/signalr/hubs".</span><span class="sxs-lookup"><span data-stu-id="197c4-189">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="197c4-190">Se non si desidera utilizzare i proxy JavaScript o se si desidera generare manualmente questo file e fare riferimento a un file fisico nei client, è possibile utilizzare questa opzione per disabilitare la generazione del proxy.</span><span class="sxs-lookup"><span data-stu-id="197c4-190">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="197c4-191">Per ulteriori informazioni, vedere [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](hubs-api-guide-javascript-client.md#manualproxy).</span><span class="sxs-lookup"><span data-stu-id="197c4-191">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](hubs-api-guide-javascript-client.md#manualproxy).</span></span>

<span data-ttu-id="197c4-192">Nell'esempio seguente viene illustrato come specificare l'URL di `MapSignalR` connessione SignalR e queste opzioni in una chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="197c4-192">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapSignalR` method.</span></span> <span data-ttu-id="197c4-193">Per specificare un URL personalizzato, sostituire "/signalr" nell'esempio con l'URL che si desidera utilizzare.</span><span class="sxs-lookup"><span data-stu-id="197c4-193">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="197c4-194">Come creare e usare le classi HubHow to create and use Hub classes</span><span class="sxs-lookup"><span data-stu-id="197c4-194">How to create and use Hub classes</span></span>

<span data-ttu-id="197c4-195">Per creare un Hub, creare una classe che deriva da [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="197c4-195">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="197c4-196">Nell'esempio seguente viene illustrata una classe Hub semplice per un'applicazione di chat.</span><span class="sxs-lookup"><span data-stu-id="197c4-196">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

<span data-ttu-id="197c4-197">In questo esempio, un client `NewContosoChatMessage` connesso può chiamare il metodo e, in tal caso, i dati ricevuti vengono trasmessi a tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="197c4-197">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="197c4-198">Durata dell'oggetto hub</span><span class="sxs-lookup"><span data-stu-id="197c4-198">Hub object lifetime</span></span>

<span data-ttu-id="197c4-199">Non si crea un'istanza della classe Hub o non si chiamano i relativi metodi dal proprio codice sul server. tutto ciò che viene fatto per voi dalla pipeline SignalR Hubs.</span><span class="sxs-lookup"><span data-stu-id="197c4-199">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="197c4-200">SignalR crea una nuova istanza della classe Hub ogni volta che è necessario gestire un'operazione Hub, ad esempio quando un client si connette, disconnette o effettua una chiamata al metodo al server.</span><span class="sxs-lookup"><span data-stu-id="197c4-200">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="197c4-201">Poiché le istanze della classe Hub sono temporanee, non è possibile usarle per mantenere lo stato da una chiamata al metodo a quella successiva.</span><span class="sxs-lookup"><span data-stu-id="197c4-201">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="197c4-202">Ogni volta che il server riceve una chiamata al metodo da un client, una nuova istanza della classe Hub elabora il messaggio.</span><span class="sxs-lookup"><span data-stu-id="197c4-202">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="197c4-203">Per mantenere lo stato tramite più connessioni e chiamate a i metodi, utilizzare un altro metodo, ad `Hub`esempio un database, una variabile statica nella classe Hub o una classe diversa che non deriva da .</span><span class="sxs-lookup"><span data-stu-id="197c4-203">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="197c4-204">Se si mantengono i dati in memoria, usando un metodo, ad esempio una variabile statica nella classe Hub, i dati andranno persi quando il dominio dell'app viene riciclato.</span><span class="sxs-lookup"><span data-stu-id="197c4-204">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="197c4-205">Se si desidera inviare messaggi ai client dal proprio codice che viene eseguito all'esterno della classe Hub, non è possibile farlo ipotificando un'istanza della classe Hub, ma è possibile farlo ottenendo un riferimento all'oggetto di contesto SignalR per la classe Hub.</span><span class="sxs-lookup"><span data-stu-id="197c4-205">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="197c4-206">Per altre informazioni, vedere [Come chiamare i metodi client e gestire i gruppi dall'esterno della classe Hub](#callfromoutsidehub) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="197c4-206">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="197c4-207">Cammello-casing dei nomi Hub nei client JavaScript</span><span class="sxs-lookup"><span data-stu-id="197c4-207">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="197c4-208">Per impostazione predefinita, i client JavaScript fanno riferimento a Hubs utilizzando una versione con maiuscole/minuscole camel del nome della classe.</span><span class="sxs-lookup"><span data-stu-id="197c4-208">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="197c4-209">SignalR apporta automaticamente questa modifica in modo che il codice JavaScript possa essere conforme alle convenzioni JavaScript.</span><span class="sxs-lookup"><span data-stu-id="197c4-209">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="197c4-210">L'esempio precedente viene `contosoChatHub` indicato come nel codice JavaScript.The previous example would be referred to as in JavaScript code.</span><span class="sxs-lookup"><span data-stu-id="197c4-210">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="197c4-211">**Server**</span><span class="sxs-lookup"><span data-stu-id="197c4-211">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

<span data-ttu-id="197c4-212">**Client JavaScript che utilizza il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="197c4-212">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

<span data-ttu-id="197c4-213">Se si desidera specificare un nome diverso `HubName` per i client da utilizzare, aggiungere l'attributo.</span><span class="sxs-lookup"><span data-stu-id="197c4-213">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="197c4-214">Quando si `HubName` utilizza un attributo, non viene apportata alcuna modifica al nome del caso camel nei client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="197c4-214">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="197c4-215">**Server**</span><span class="sxs-lookup"><span data-stu-id="197c4-215">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

<span data-ttu-id="197c4-216">**Client JavaScript che utilizza il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="197c4-216">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="197c4-217">Hub multipli</span><span class="sxs-lookup"><span data-stu-id="197c4-217">Multiple Hubs</span></span>

<span data-ttu-id="197c4-218">È possibile definire più classi Hub in un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="197c4-218">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="197c4-219">In questo caso, la connessione viene condivisa ma i gruppi sono separati:</span><span class="sxs-lookup"><span data-stu-id="197c4-219">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="197c4-220">Tutti i client utilizzeranno lo stesso URL per stabilire una connessione SignalR con il servizio ("/signalr" o l'URL personalizzato se ne è stato specificato uno) e tale connessione viene utilizzata per tutti gli Hub definiti dal servizio.</span><span class="sxs-lookup"><span data-stu-id="197c4-220">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="197c4-221">Non esiste alcuna differenza di prestazioni per più hub rispetto alla definizione di tutte le funzionalità Hub in una singola classe.</span><span class="sxs-lookup"><span data-stu-id="197c4-221">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="197c4-222">Tutti gli hub ottengono le stesse informazioni sulla richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="197c4-222">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="197c4-223">Poiché tutti gli hub condividono la stessa connessione, l'unica informazione sulla richiesta HTTP ottenuta dal server è ciò che viene fornito nella richiesta HTTP originale che stabilisce la connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="197c4-223">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="197c4-224">Se si utilizza la richiesta di connessione per passare informazioni dal client al server specificando una stringa di query, non è possibile fornire stringhe di query diverse a hub diversi.</span><span class="sxs-lookup"><span data-stu-id="197c4-224">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="197c4-225">Tutti gli hub riceveranno le stesse informazioni.</span><span class="sxs-lookup"><span data-stu-id="197c4-225">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="197c4-226">Il file dei proxy JavaScript generato conterrà proxy per tutti gli Hub in un unico file.</span><span class="sxs-lookup"><span data-stu-id="197c4-226">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="197c4-227">Per informazioni sui proxy JavaScript, vedere [SignalR Hubs API Guide - JavaScript Client - Il proxy generato e le operazioni eseguite automaticamente.](hubs-api-guide-javascript-client.md#genproxy)</span><span class="sxs-lookup"><span data-stu-id="197c4-227">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).</span></span>
- <span data-ttu-id="197c4-228">I gruppi sono definiti all'interno di Hub.</span><span class="sxs-lookup"><span data-stu-id="197c4-228">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="197c4-229">In SignalR è possibile definire gruppi denominati da trasmettere a sottoinsiemi di client connessi.</span><span class="sxs-lookup"><span data-stu-id="197c4-229">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="197c4-230">I gruppi vengono gestiti separatamente per ogni Hub.</span><span class="sxs-lookup"><span data-stu-id="197c4-230">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="197c4-231">Ad esempio, un gruppo denominato "Administrators" includerebbe `ContosoChatHub` un set di client per la classe e `StockTickerHub` lo stesso nome di gruppo farebbe riferimento a un set diverso di client per la classe.</span><span class="sxs-lookup"><span data-stu-id="197c4-231">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a><span data-ttu-id="197c4-232">Hub fortemente tipati</span><span class="sxs-lookup"><span data-stu-id="197c4-232">Strongly-Typed Hubs</span></span>

<span data-ttu-id="197c4-233">Per definire un'interfaccia per i metodi dell'hub a cui il client può `Hub<T>` fare riferimento (e abilitare `Hub`Intellisense nei metodi dell'hub), derivare l'hub da (introdotto in SignalR 2.1) anziché:</span><span class="sxs-lookup"><span data-stu-id="197c4-233">To define an interface for your hub methods that your client can reference (and enable Intellisense on your hub methods), derive your hub from `Hub<T>` (introduced in SignalR 2.1) rather than `Hub`:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="197c4-234">Come definire i metodi nella classe Hub che i client possono chiamare</span><span class="sxs-lookup"><span data-stu-id="197c4-234">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="197c4-235">Per esporre un metodo nell'hub che si desidera chiamare dal client, dichiarare un metodo pubblico, come illustrato negli esempi seguenti.</span><span class="sxs-lookup"><span data-stu-id="197c4-235">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="197c4-236">È possibile specificare un tipo restituito e i parametri, inclusi i tipi complessi e le matrici, come si farebbe in qualsiasi metodo C.</span><span class="sxs-lookup"><span data-stu-id="197c4-236">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="197c4-237">Tutti i dati ricevuti nei parametri o restituiti al chiamante vengono comunicati tra il client e il server tramite JSON e SignalR gestisce automaticamente l'associazione di oggetti complessi e matrici di oggetti.</span><span class="sxs-lookup"><span data-stu-id="197c4-237">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="197c4-238">Cammello-casing dei nomi dei metodi nei client JavaScript</span><span class="sxs-lookup"><span data-stu-id="197c4-238">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="197c4-239">Per impostazione predefinita, i client JavaScript fanno riferimento ai metodi Hub utilizzando una versione con maiuscole/minuscole camel del nome del metodo.</span><span class="sxs-lookup"><span data-stu-id="197c4-239">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="197c4-240">SignalR apporta automaticamente questa modifica in modo che il codice JavaScript possa essere conforme alle convenzioni JavaScript.</span><span class="sxs-lookup"><span data-stu-id="197c4-240">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="197c4-241">**Server**</span><span class="sxs-lookup"><span data-stu-id="197c4-241">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="197c4-242">**Client JavaScript che utilizza il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="197c4-242">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="197c4-243">Se si desidera specificare un nome diverso `HubMethodName` per i client da utilizzare, aggiungere l'attributo.</span><span class="sxs-lookup"><span data-stu-id="197c4-243">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="197c4-244">**Server**</span><span class="sxs-lookup"><span data-stu-id="197c4-244">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="197c4-245">**Client JavaScript che utilizza il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="197c4-245">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="197c4-246">Quando eseguire in modo asincronoWhen to execute asynchronously</span><span class="sxs-lookup"><span data-stu-id="197c4-246">When to execute asynchronously</span></span>

<span data-ttu-id="197c4-247">Se il metodo sarà a esecuzione prolungata o deve eseguire operazioni che comporterebbero l'attesa, ad esempio una ricerca di `void` database o una chiamata al servizio web, rendere il metodo Hub asincrono restituendo un [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (al posto di ritorno) o [Task&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) oggetto (al posto del tipo restituito). `T`</span><span class="sxs-lookup"><span data-stu-id="197c4-247">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="197c4-248">Quando si `Task` restituisce un oggetto dal metodo, `Task` SignalR attende il completamento di e quindi invia il risultato unwrapped al client, quindi non vi è alcuna differenza nel modo in cui si codifica la chiamata al metodo nel client.</span><span class="sxs-lookup"><span data-stu-id="197c4-248">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="197c4-249">Rendere asincrono un metodo Hub evita di bloccare la connessione quando utilizza il trasporto WebSocket.Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span><span class="sxs-lookup"><span data-stu-id="197c4-249">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="197c4-250">Quando un metodo Hub viene eseguito in modo sincrono e il trasporto è WebSocket, le successive chiamate dei metodi sull'hub dallo stesso client vengono bloccate fino al completamento del metodo Hub.</span><span class="sxs-lookup"><span data-stu-id="197c4-250">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="197c4-251">L'esempio seguente mostra lo stesso metodo codificato per l'esecuzione in modo sincrono o asincrono, seguito dal codice client JavaScript che funziona per chiamare entrambe le versioni.</span><span class="sxs-lookup"><span data-stu-id="197c4-251">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="197c4-252">**Sincrono**</span><span class="sxs-lookup"><span data-stu-id="197c4-252">**Synchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="197c4-253">**Asincrona**</span><span class="sxs-lookup"><span data-stu-id="197c4-253">**Asynchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="197c4-254">**Client JavaScript che utilizza il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="197c4-254">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="197c4-255">Per ulteriori informazioni su come utilizzare i metodi asincroni in ASP.NET 4.5, vedere [Utilizzo di metodi asincroni in ASP.NET MVC 4.](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)</span><span class="sxs-lookup"><span data-stu-id="197c4-255">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="197c4-256">Definizione di overloadDefining Overloads</span><span class="sxs-lookup"><span data-stu-id="197c4-256">Defining Overloads</span></span>

<span data-ttu-id="197c4-257">Se si desidera definire overload per un metodo, il numero di parametri in ogni overload deve essere diverso.</span><span class="sxs-lookup"><span data-stu-id="197c4-257">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="197c4-258">Se si differenzia un overload semplicemente specificando tipi di parametro diversi, la classe Hub verrà compilata, ma il servizio SignalR genererà un'eccezione in fase di esecuzione quando i client tentano di chiamare uno degli overload.</span><span class="sxs-lookup"><span data-stu-id="197c4-258">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a><span data-ttu-id="197c4-259">Segnalazione dello stato di avanzamento dalle chiamate al metodo hubReporting progress from hub method invocations</span><span class="sxs-lookup"><span data-stu-id="197c4-259">Reporting progress from hub method invocations</span></span>

<span data-ttu-id="197c4-260">SignalR 2.1 aggiunge il supporto per il modello di [report di stato](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introdotto in .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="197c4-260">SignalR 2.1 adds support for the [progress reporting pattern](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduced in .NET 4.5.</span></span> <span data-ttu-id="197c4-261">Per implementare la `IProgress<T>` segnalazione dello stato di avanzamento, definire un parametro per il metodo hub a cui il client può accedere:To implement progress reporting, define an parameter for your hub method that your client can access:</span><span class="sxs-lookup"><span data-stu-id="197c4-261">To implement progress reporting, define an `IProgress<T>` parameter for your hub method that your client can access:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

<span data-ttu-id="197c4-262">Quando si scrive un metodo server a esecuzione prolungata, è importante usare un modello di programmazione asincrona come Async/Await anziché bloccare il thread hub.</span><span class="sxs-lookup"><span data-stu-id="197c4-262">When writing a long-running server method, it is important to use an asynchronous programming pattern like Async/ Await rather than blocking the hub thread.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="197c4-263">Come chiamare i metodi client dalla classe HubHow to call client methods from the Hub class</span><span class="sxs-lookup"><span data-stu-id="197c4-263">How to call client methods from the Hub class</span></span>

<span data-ttu-id="197c4-264">Per chiamare i metodi client `Clients` dal server, usare la proprietà in un metodo nella classe Hub.</span><span class="sxs-lookup"><span data-stu-id="197c4-264">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="197c4-265">Nell'esempio seguente viene `addNewMessageToPage` illustrato il codice server che chiama su tutti i client connessi e il codice client che definisce il metodo in un client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="197c4-265">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="197c4-266">**Server**</span><span class="sxs-lookup"><span data-stu-id="197c4-266">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

<span data-ttu-id="197c4-267">La chiamata di un metodo client `Task`è un'operazione asincrona e restituisce un oggetto .</span><span class="sxs-lookup"><span data-stu-id="197c4-267">Invoking a client method is an asynchronous operation and returns a `Task`.</span></span> <span data-ttu-id="197c4-268">Utilizzare `await`:</span><span class="sxs-lookup"><span data-stu-id="197c4-268">Use `await`:</span></span>

* <span data-ttu-id="197c4-269">Per garantire che il messaggio venga inviato senza errori.</span><span class="sxs-lookup"><span data-stu-id="197c4-269">To ensure the message is sent without error.</span></span> 
* <span data-ttu-id="197c4-270">Per abilitare l'intercettazione e la gestione degli errori in un blocco try-catch.</span><span class="sxs-lookup"><span data-stu-id="197c4-270">To enable catching and handling errors in a try-catch block.</span></span>

<span data-ttu-id="197c4-271">**Client JavaScript che utilizza il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="197c4-271">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

<span data-ttu-id="197c4-272">Non è possibile ottenere un valore restituito da un metodo client; sintassi `int x = Clients.All.add(1,1)` come non funziona.</span><span class="sxs-lookup"><span data-stu-id="197c4-272">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="197c4-273">È possibile specificare tipi complessi e matrici per i parametri.</span><span class="sxs-lookup"><span data-stu-id="197c4-273">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="197c4-274">Nell'esempio seguente viene passato un tipo complesso al client in un parametro di metodo.</span><span class="sxs-lookup"><span data-stu-id="197c4-274">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="197c4-275">**Codice server che chiama un metodo client utilizzando un oggetto complessoServer code that calls a client method using a complex object**</span><span class="sxs-lookup"><span data-stu-id="197c4-275">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

<span data-ttu-id="197c4-276">**Codice server che definisce l'oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="197c4-276">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

<span data-ttu-id="197c4-277">**Client JavaScript che utilizza il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="197c4-277">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="197c4-278">Selezione dei client che riceveranno la</span><span class="sxs-lookup"><span data-stu-id="197c4-278">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="197c4-279">La proprietà Clients restituisce un oggetto [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) che fornisce diverse opzioni per specificare quali client riceveranno la rpc:</span><span class="sxs-lookup"><span data-stu-id="197c4-279">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="197c4-280">Tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="197c4-280">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="197c4-281">Solo il client chiamante.</span><span class="sxs-lookup"><span data-stu-id="197c4-281">Only the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="197c4-282">Tutti i client ad eccezione del client chiamante.</span><span class="sxs-lookup"><span data-stu-id="197c4-282">All clients except the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- <span data-ttu-id="197c4-283">Un client specifico identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="197c4-283">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    <span data-ttu-id="197c4-284">In questo `addContosoChatMessageToPage` esempio viene chiamato il client `Clients.Caller`chiamante e si ha lo stesso effetto dell'utilizzo di .</span><span class="sxs-lookup"><span data-stu-id="197c4-284">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="197c4-285">Tutti i client connessi ad eccezione dei client specificati, identificati dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="197c4-285">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- <span data-ttu-id="197c4-286">Tutti i client connessi in un gruppo specificato.</span><span class="sxs-lookup"><span data-stu-id="197c4-286">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- <span data-ttu-id="197c4-287">Tutti i client connessi in un gruppo specificato ad eccezione dei client specificati, identificati dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="197c4-287">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- <span data-ttu-id="197c4-288">Tutti i client connessi in un gruppo specificato ad eccezione del client chiamante.</span><span class="sxs-lookup"><span data-stu-id="197c4-288">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- <span data-ttu-id="197c4-289">Un utente specifico, identificato da userId.</span><span class="sxs-lookup"><span data-stu-id="197c4-289">A specific user, identified by userId.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    <span data-ttu-id="197c4-290">Per impostazione `IPrincipal.Identity.Name`predefinita, si tratta di , ma può essere modificato [registrando un'implementazione di IUserIdProvider con l'host globale](mapping-users-to-connections.md#IUserIdProvider).</span><span class="sxs-lookup"><span data-stu-id="197c4-290">By default, this is `IPrincipal.Identity.Name`, but this can be changed by [registering an implementation of IUserIdProvider with the global host](mapping-users-to-connections.md#IUserIdProvider).</span></span>
- <span data-ttu-id="197c4-291">Tutti i client e i gruppi in un elenco di ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="197c4-291">All clients and groups in a list of connection IDs.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- <span data-ttu-id="197c4-292">Elenco di gruppi.</span><span class="sxs-lookup"><span data-stu-id="197c4-292">A list of groups.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- <span data-ttu-id="197c4-293">Un utente per nome.</span><span class="sxs-lookup"><span data-stu-id="197c4-293">A user by name.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- <span data-ttu-id="197c4-294">Un elenco di nomi utente (introdotto in SignalR 2.1).</span><span class="sxs-lookup"><span data-stu-id="197c4-294">A list of user names (introduced in SignalR 2.1).</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="197c4-295">Nessuna convalida in fase di compilazione per i nomi dei metodi</span><span class="sxs-lookup"><span data-stu-id="197c4-295">No compile-time validation for method names</span></span>

<span data-ttu-id="197c4-296">Il nome del metodo specificato viene interpretato come un oggetto dinamico, il che significa che non esiste alcuna convalida IntelliSense o in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="197c4-296">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="197c4-297">L'espressione viene valutata in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="197c4-297">The expression is evaluated at run time.</span></span> <span data-ttu-id="197c4-298">Quando viene eseguita la chiamata al metodo, SignalR invia il nome del metodo e i valori dei parametri al client e se il client dispone di un metodo che corrisponde al nome, tale metodo viene chiamato e i valori dei parametri vengono passati ad esso.</span><span class="sxs-lookup"><span data-stu-id="197c4-298">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="197c4-299">Se non viene trovato alcun metodo corrispondente sul client, non viene generato alcun errore.</span><span class="sxs-lookup"><span data-stu-id="197c4-299">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="197c4-300">Per informazioni sul formato dei dati trasmessi da SignalR al client dietro le quinte quando si chiama un metodo client, vedere [Introduzione a SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="197c4-300">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="197c4-301">Corrispondenza del nome del metodo senza distinzione tra maiuscole e minuscole</span><span class="sxs-lookup"><span data-stu-id="197c4-301">Case-insensitive method name matching</span></span>

<span data-ttu-id="197c4-302">La corrispondenza del nome del metodo non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="197c4-302">Method name matching is case-insensitive.</span></span> <span data-ttu-id="197c4-303">Ad `Clients.All.addContosoChatMessageToPage` esempio, sul server `AddContosoChatMessageToPage` `addcontosochatmessagetopage`verrà `addContosoChatMessageToPage` eseguito , o sul client.</span><span class="sxs-lookup"><span data-stu-id="197c4-303">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="197c4-304">Esecuzione asincrona</span><span class="sxs-lookup"><span data-stu-id="197c4-304">Asynchronous execution</span></span>

<span data-ttu-id="197c4-305">Il metodo chiamato viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="197c4-305">The method that you call executes asynchronously.</span></span> <span data-ttu-id="197c4-306">Qualsiasi codice che viene dopo una chiamata al metodo a un client verrà eseguito immediatamente senza attendere SignalR per completare la trasmissione dei dati ai client, a meno che non si specifichi che le righe di codice successive devono attendere il completamento del metodo.</span><span class="sxs-lookup"><span data-stu-id="197c4-306">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="197c4-307">Nell'esempio di codice riportato di seguito viene illustrato come eseguire due metodi client in sequenza.</span><span class="sxs-lookup"><span data-stu-id="197c4-307">The following code example shows how to execute two client methods sequentially.</span></span>

<span data-ttu-id="197c4-308">**Utilizzo di Await (.NET 4.5)**</span><span class="sxs-lookup"><span data-stu-id="197c4-308">**Using Await (.NET 4.5)**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="197c4-309">Se si `await` utilizza l'utente per attendere il completamento di un metodo client prima dell'esecuzione della riga di codice successiva, ciò non significa che i client riceveranno effettivamente il messaggio prima dell'esecuzione della riga di codice successiva.</span><span class="sxs-lookup"><span data-stu-id="197c4-309">If you use `await` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="197c4-310">"Completamento" di una chiamata al metodo client significa solo che SignalR ha fatto tutto il necessario per inviare il messaggio.</span><span class="sxs-lookup"><span data-stu-id="197c4-310">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="197c4-311">Se hai bisogno di verificare che i clienti hanno ricevuto il messaggio, devi programmare questo meccanismo da solo.</span><span class="sxs-lookup"><span data-stu-id="197c4-311">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="197c4-312">Ad esempio, è `MessageReceived` possibile codificare un metodo `addContosoChatMessageToPage` nell'hub e `MessageReceived` nel metodo sul client è possibile chiamare dopo aver eseguito qualsiasi operazione è necessario eseguire sul client.</span><span class="sxs-lookup"><span data-stu-id="197c4-312">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="197c4-313">Nell'Hub `MessageReceived` è possibile eseguire qualsiasi operazione dipende dalla ricezione effettiva del client e dall'elaborazione della chiamata al metodo originale.</span><span class="sxs-lookup"><span data-stu-id="197c4-313">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="197c4-314">Come utilizzare una variabile stringa come nome del metodo</span><span class="sxs-lookup"><span data-stu-id="197c4-314">How to use a string variable as the method name</span></span>

<span data-ttu-id="197c4-315">Se si desidera richiamare un metodo client utilizzando una `Clients.All` variabile `Clients.Others`stringa `Clients.Caller`come nome `IClientProxy` del metodo, cast (o , , e così via) a e quindi chiamare [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="197c4-315">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="197c4-316">Come gestire l'appartenenza al gruppo dalla classe Hub</span><span class="sxs-lookup"><span data-stu-id="197c4-316">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="197c4-317">I gruppi in SignalR forniscono un metodo per la trasmissione di messaggi a sottoinsiemi specificati di client connessi.</span><span class="sxs-lookup"><span data-stu-id="197c4-317">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="197c4-318">Un gruppo può avere un numero qualsiasi di client e un client può essere membro di un numero qualsiasi di gruppi.</span><span class="sxs-lookup"><span data-stu-id="197c4-318">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="197c4-319">Per gestire l'appartenenza al gruppo, `Groups` utilizzare i metodi [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) e [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) forniti dalla proprietà della classe Hub.</span><span class="sxs-lookup"><span data-stu-id="197c4-319">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="197c4-320">Nell'esempio seguente `Groups.Add` `Groups.Remove` vengono illustrati i metodi e utilizzati nei metodi Hub chiamati dal codice client, seguiti dal codice client JavaScript che li chiama.</span><span class="sxs-lookup"><span data-stu-id="197c4-320">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="197c4-321">**Server**</span><span class="sxs-lookup"><span data-stu-id="197c4-321">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

<span data-ttu-id="197c4-322">**Client JavaScript che utilizza il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="197c4-322">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

<span data-ttu-id="197c4-323">Non è necessario creare gruppi in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="197c4-323">You don't have to explicitly create groups.</span></span> <span data-ttu-id="197c4-324">In effetti, un gruppo viene creato automaticamente la prima `Groups.Add`volta che si specifica il nome in una chiamata a , che viene eliminato quando si rimuove l'ultima connessione dall'appartenenza.</span><span class="sxs-lookup"><span data-stu-id="197c4-324">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="197c4-325">Non è disponibile alcuna API per ottenere un elenco di appartenenza a un gruppo o un elenco di gruppi.</span><span class="sxs-lookup"><span data-stu-id="197c4-325">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="197c4-326">SignalR invia messaggi a client e gruppi in base a un [modello pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe)e il server non gestisce elenchi di gruppi o appartenenze ai gruppi.</span><span class="sxs-lookup"><span data-stu-id="197c4-326">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="197c4-327">Ciò consente di ottimizzare la scalabilità, perché ogni volta che si aggiunge un nodo a una Web farm, qualsiasi stato mantenuto da SignalR deve essere propagato al nuovo nodo.</span><span class="sxs-lookup"><span data-stu-id="197c4-327">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="197c4-328">Esecuzione asincrona dei metodi Add e Remove</span><span class="sxs-lookup"><span data-stu-id="197c4-328">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="197c4-329">I `Groups.Add` `Groups.Remove` metodi e vengono eseguiti in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="197c4-329">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="197c4-330">Se si desidera aggiungere un client a un gruppo e inviare immediatamente un messaggio al `Groups.Add` client utilizzando il gruppo, è necessario assicurarsi che il metodo venga completato per primo.</span><span class="sxs-lookup"><span data-stu-id="197c4-330">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="197c4-331">Esempio di codice seguente viene illustrato come eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="197c4-331">The following code example shows how to do that.</span></span>

<span data-ttu-id="197c4-332">**Aggiunta di un client a un gruppo e messaggistica**</span><span class="sxs-lookup"><span data-stu-id="197c4-332">**Adding a client to a group and then messaging that client**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="197c4-333">Persistenza dell'appartenenza al gruppo</span><span class="sxs-lookup"><span data-stu-id="197c4-333">Group membership persistence</span></span>

<span data-ttu-id="197c4-334">SignalR tiene traccia delle connessioni, non degli utenti, quindi se si desidera che un utente sia `Groups.Add` nello stesso gruppo ogni volta che l'utente stabilisce una connessione, è necessario chiamare ogni volta che l'utente stabilisce una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="197c4-334">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="197c4-335">Dopo una perdita temporanea di connettività, a volte SignalR può ripristinare automaticamente la connessione.</span><span class="sxs-lookup"><span data-stu-id="197c4-335">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="197c4-336">In tal caso, SignalR sta ripristinando la stessa connessione, non stabilendo una nuova connessione e pertanto l'appartenenza al gruppo del client viene ripristinata automaticamente.</span><span class="sxs-lookup"><span data-stu-id="197c4-336">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="197c4-337">Ciò è possibile anche quando l'interruzione temporanea è il risultato di un riavvio o un errore del server, perché lo stato di connessione per ogni client, incluse le appartenenze ai gruppi, viene eseguito il round-tripped al client.</span><span class="sxs-lookup"><span data-stu-id="197c4-337">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="197c4-338">Se un server si arresta e viene sostituito da un nuovo server prima del timeout della connessione, un client può riconnettersi automaticamente al nuovo server e registrarsi nuovamente nei gruppi di cui è membro.</span><span class="sxs-lookup"><span data-stu-id="197c4-338">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="197c4-339">Quando una connessione non può essere ripristinata automaticamente dopo una perdita di connettività o quando si verifica il timeout della connessione o quando il client si disconnette (ad esempio, quando un browser passa a una nuova pagina), le appartenenze ai gruppi vengono perse.</span><span class="sxs-lookup"><span data-stu-id="197c4-339">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="197c4-340">La prossima volta che l'utente si connette sarà una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="197c4-340">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="197c4-341">Per mantenere le appartenenze ai gruppi quando lo stesso utente stabilisce una nuova connessione, l'applicazione deve tenere traccia delle associazioni tra utenti e gruppi e ripristinare le appartenenze ai gruppi ogni volta che un utente stabilisce una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="197c4-341">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="197c4-342">Per altre informazioni sulle connessioni e le riconnessioni, vedere Come gestire gli eventi di [durata della connessione nella classe Hub](#connectionlifetime) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="197c4-342">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="197c4-343">Gruppi di utenti singoli</span><span class="sxs-lookup"><span data-stu-id="197c4-343">Single-user groups</span></span>

<span data-ttu-id="197c4-344">Le applicazioni che utilizzano SignalR in genere devono tenere traccia delle associazioni tra utenti e connessioni per sapere quale utente ha inviato un messaggio e quali utenti devono ricevere un messaggio.</span><span class="sxs-lookup"><span data-stu-id="197c4-344">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="197c4-345">I gruppi vengono utilizzati in uno dei due modelli comunemente utilizzati per farlo.</span><span class="sxs-lookup"><span data-stu-id="197c4-345">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="197c4-346">Gruppi di utenti singoli.</span><span class="sxs-lookup"><span data-stu-id="197c4-346">Single-user groups.</span></span>

    <span data-ttu-id="197c4-347">È possibile specificare il nome utente come nome del gruppo e aggiungere l'ID di connessione corrente al gruppo ogni volta che l'utente si connette o si riconnette.</span><span class="sxs-lookup"><span data-stu-id="197c4-347">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="197c4-348">Per inviare messaggi all'utente inviato al gruppo.</span><span class="sxs-lookup"><span data-stu-id="197c4-348">To send messages to the user you send to the group.</span></span> <span data-ttu-id="197c4-349">Uno svantaggio di questo metodo è che il gruppo non fornisce un modo per scoprire se l'utente è online o offline.</span><span class="sxs-lookup"><span data-stu-id="197c4-349">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="197c4-350">Tenere traccia delle associazioni tra nomi utente e ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="197c4-350">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="197c4-351">È possibile archiviare un'associazione tra ogni nome utente e uno o più ID di connessione in un dizionario o in un database e aggiornare i dati archiviati ogni volta che l'utente si connette o si disconnette.</span><span class="sxs-lookup"><span data-stu-id="197c4-351">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="197c4-352">Per inviare messaggi all'utente specificare gli ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="197c4-352">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="197c4-353">Uno svantaggio di questo metodo è che richiede più memoria.</span><span class="sxs-lookup"><span data-stu-id="197c4-353">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="197c4-354">Come gestire gli eventi di durata della connessione nella classe HubHow to handle connection lifetime events in the Hub class</span><span class="sxs-lookup"><span data-stu-id="197c4-354">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="197c4-355">I motivi tipici per la gestione degli eventi di durata della connessione sono tenere traccia della connessione o meno di un utente e dell'associazione tra nomi utente e ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="197c4-355">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="197c4-356">Per eseguire il codice quando i client `OnConnected` `OnDisconnected`si `OnReconnected` connettono o si disconnettono, eseguire l'override dei metodi , e virtual della classe Hub, come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="197c4-356">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="197c4-357">Quando OnConnected, OnDisconnected e OnReconnected vengono chiamati</span><span class="sxs-lookup"><span data-stu-id="197c4-357">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="197c4-358">Ogni volta che un browser passa a una nuova pagina, è necessario stabilire `OnDisconnected` una nuova `OnConnected` connessione, il che significa che SignalR eseguirà il metodo seguito dal metodo.</span><span class="sxs-lookup"><span data-stu-id="197c4-358">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="197c4-359">SignalR crea sempre un nuovo ID di connessione quando viene stabilita una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="197c4-359">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="197c4-360">Il `OnReconnected` metodo viene chiamato quando si verifica un'interruzione temporanea della connettività da cui SignalR può recuperare automaticamente, ad esempio quando un cavo viene temporaneamente scollegato e riconnesso prima del timeout della connessione. Il `OnDisconnected` metodo viene chiamato quando il client viene disconnesso e SignalR non può riconnettersi automaticamente, ad esempio quando un browser passa a una nuova pagina.</span><span class="sxs-lookup"><span data-stu-id="197c4-360">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="197c4-361">Pertanto, una possibile sequenza di `OnConnected` `OnReconnected`eventi `OnDisconnected`per un determinato client è , , ; o `OnConnected` `OnDisconnected`, .</span><span class="sxs-lookup"><span data-stu-id="197c4-361">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="197c4-362">Non verrà visualizzata `OnConnected`la `OnDisconnected` `OnReconnected` sequenza , , per una determinata connessione.</span><span class="sxs-lookup"><span data-stu-id="197c4-362">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="197c4-363">Il `OnDisconnected` metodo non viene chiamato in alcuni scenari, ad esempio quando un server si arresta o il dominio dell'applicazione viene riciclato.</span><span class="sxs-lookup"><span data-stu-id="197c4-363">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="197c4-364">Quando un altro server viene collegato o il dominio dell'app completa il `OnReconnected` suo riciclo, alcuni client potrebbero essere in grado di riconnettersi e generare l'evento.</span><span class="sxs-lookup"><span data-stu-id="197c4-364">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="197c4-365">Per ulteriori informazioni, vedere Informazioni e gestione degli eventi di durata della [connessione in SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="197c4-365">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="197c4-366">Stato del chiamante non popolato</span><span class="sxs-lookup"><span data-stu-id="197c4-366">Caller state not populated</span></span>

<span data-ttu-id="197c4-367">I metodi del gestore eventi della durata della connessione vengono chiamati `state` dal server, il che `Caller` significa che qualsiasi stato inserito nell'oggetto sul client non verrà popolato nella proprietà sul server.</span><span class="sxs-lookup"><span data-stu-id="197c4-367">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="197c4-368">Per informazioni `state` sull'oggetto `Caller` e la proprietà, vedere [Come passare lo stato tra i client e la classe Hub](#passstate) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="197c4-368">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="197c4-369">Come ottenere informazioni sul client dalla proprietà Context</span><span class="sxs-lookup"><span data-stu-id="197c4-369">How to get information about the client from the Context property</span></span>

<span data-ttu-id="197c4-370">Per ottenere informazioni sul client, utilizzare la `Context` proprietà della classe Hub.</span><span class="sxs-lookup"><span data-stu-id="197c4-370">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="197c4-371">La `Context` proprietà restituisce un oggetto HubCallerContext che fornisce l'accesso alle informazioni seguenti:The property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span><span class="sxs-lookup"><span data-stu-id="197c4-371">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="197c4-372">ID connessione del client chiamante.</span><span class="sxs-lookup"><span data-stu-id="197c4-372">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    <span data-ttu-id="197c4-373">L'ID di connessione è un GUID assegnato da SignalR (non è possibile specificare il valore nel codice).</span><span class="sxs-lookup"><span data-stu-id="197c4-373">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="197c4-374">Esiste un ID di connessione per ogni connessione e lo stesso ID di connessione viene usato da tutti gli hub se nell'applicazione sono presenti più hub.</span><span class="sxs-lookup"><span data-stu-id="197c4-374">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="197c4-375">Dati dell'intestazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="197c4-375">HTTP header data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="197c4-376">È inoltre possibile ottenere `Context.Headers`intestazioni HTTP da .</span><span class="sxs-lookup"><span data-stu-id="197c4-376">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="197c4-377">Il motivo per più riferimenti `Context.Headers` alla stessa cosa `Context.Request` è che è `Context.Headers` stato creato prima, la proprietà è stata aggiunta in un secondo momento e è stata mantenuta per compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="197c4-377">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="197c4-378">Dati della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="197c4-378">Query string data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    <span data-ttu-id="197c4-379">È inoltre possibile ottenere `Context.QueryString`dati della stringa di query da .</span><span class="sxs-lookup"><span data-stu-id="197c4-379">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="197c4-380">La stringa di query che si ottiene in questa proprietà è quella utilizzata con la richiesta HTTP che ha stabilito la connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="197c4-380">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="197c4-381">È possibile aggiungere parametri di stringa di query nel client configurando la connessione, che è un modo pratico per passare dati sul client dal client al server.</span><span class="sxs-lookup"><span data-stu-id="197c4-381">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="197c4-382">L'esempio seguente mostra un modo per aggiungere una stringa di query in un client JavaScript quando si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="197c4-382">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    <span data-ttu-id="197c4-383">Per altre informazioni sull'impostazione dei parametri della stringa di query, vedere le guide API per i client [JavaScript](hubs-api-guide-javascript-client.md) e [.NET.](hubs-api-guide-net-client.md)</span><span class="sxs-lookup"><span data-stu-id="197c4-383">For more information about setting query string parameters, see the API guides for the [JavaScript](hubs-api-guide-javascript-client.md) and [.NET](hubs-api-guide-net-client.md) clients.</span></span>

    <span data-ttu-id="197c4-384">È possibile trovare il metodo di trasporto utilizzato per la connessione nei dati della stringa di query, insieme ad altri valori utilizzati internamente da SignalR:You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span><span class="sxs-lookup"><span data-stu-id="197c4-384">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    <span data-ttu-id="197c4-385">Il valore `transportMethod` di sarà "webSockets", "serverSentEvents", "foreverFrame" o "longPolling".</span><span class="sxs-lookup"><span data-stu-id="197c4-385">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="197c4-386">Si noti che se `OnConnected` si controlla questo valore nel metodo del gestore eventi, in alcuni scenari è possibile ottenere inizialmente un valore di trasporto che non è il metodo di trasporto negoziato finale per la connessione.</span><span class="sxs-lookup"><span data-stu-id="197c4-386">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="197c4-387">In tal caso il metodo genererà un'eccezione e verrà chiamato di nuovo in un secondo momento quando viene stabilito il metodo di trasporto finale.</span><span class="sxs-lookup"><span data-stu-id="197c4-387">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="197c4-388">Biscotti.</span><span class="sxs-lookup"><span data-stu-id="197c4-388">Cookies.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    <span data-ttu-id="197c4-389">È inoltre possibile `Context.RequestCookies`ottenere i cookie da .</span><span class="sxs-lookup"><span data-stu-id="197c4-389">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="197c4-390">informazioni utente.</span><span class="sxs-lookup"><span data-stu-id="197c4-390">User information.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- <span data-ttu-id="197c4-391">Oggetto HttpContext per la richiesta :</span><span class="sxs-lookup"><span data-stu-id="197c4-391">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    <span data-ttu-id="197c4-392">Utilizzare questo metodo `HttpContext.Current` anziché `HttpContext` ottenere l'oggetto per la connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="197c4-392">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="197c4-393">Come passare lo stato tra i client e la classe HubHow to pass state between clients and the Hub class</span><span class="sxs-lookup"><span data-stu-id="197c4-393">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="197c4-394">Il proxy client `state` fornisce un oggetto in cui è possibile archiviare i dati che si desidera trasmettere al server con ogni chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="197c4-394">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="197c4-395">Sul server è possibile accedere `Clients.Caller` a questi dati nella proprietà nei metodi Hub chiamati dai client.</span><span class="sxs-lookup"><span data-stu-id="197c4-395">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="197c4-396">La `Clients.Caller` proprietà non viene popolata per `OnConnected` `OnDisconnected`i `OnReconnected`metodi del gestore eventi della durata della connessione , , e .</span><span class="sxs-lookup"><span data-stu-id="197c4-396">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="197c4-397">La creazione o `state` l'aggiornamento `Clients.Caller` dei dati nell'oggetto e la proprietà funziona in entrambe le direzioni.</span><span class="sxs-lookup"><span data-stu-id="197c4-397">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="197c4-398">È possibile aggiornare i valori nel server e passarli al client.</span><span class="sxs-lookup"><span data-stu-id="197c4-398">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="197c4-399">Nell'esempio seguente viene illustrato il codice client JavaScript che archivia lo stato per la trasmissione al server con ogni chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="197c4-399">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

<span data-ttu-id="197c4-400">Nell'esempio seguente viene illustrato il codice equivalente in un client .NET.</span><span class="sxs-lookup"><span data-stu-id="197c4-400">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

<span data-ttu-id="197c4-401">Nella classe Hub è possibile accedere `Clients.Caller` a questi dati nella proprietà.</span><span class="sxs-lookup"><span data-stu-id="197c4-401">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="197c4-402">Nell'esempio seguente viene illustrato il codice che recupera lo stato a cui si fa riferimento nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="197c4-402">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="197c4-403">Questo meccanismo per la persistenza dello stato non è destinato `state` a `Clients.Caller` grandi quantità di dati, poiché tutto ciò che viene inserito nella proprietà o viene sottoposto a round trip con ogni chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="197c4-403">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="197c4-404">È utile per gli elementi più piccoli, ad esempio nomi utente o contatori.</span><span class="sxs-lookup"><span data-stu-id="197c4-404">It's useful for smaller items such as user names or counters.</span></span>

<span data-ttu-id="197c4-405">In VB.NET o in un hub fortemente tipizzato, non `Clients.Caller`è possibile accedere all'oggetto stato del chiamante tramite ; invece, `Clients.CallerState` utilizzare (introdotto in SignalR 2.1):</span><span class="sxs-lookup"><span data-stu-id="197c4-405">In VB.NET or in a strongly-typed hub, the caller state object can't be accessed through `Clients.Caller`; instead, use `Clients.CallerState` (introduced in SignalR 2.1):</span></span>

<span data-ttu-id="197c4-406">**Utilizzo di CallerState in CUsing CallerState in C #**</span><span class="sxs-lookup"><span data-stu-id="197c4-406">**Using CallerState in C#**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

<span data-ttu-id="197c4-407">**Utilizzo di CallerState in Visual BasicUsing CallerState in Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="197c4-407">**Using CallerState in Visual Basic**</span></span>

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="197c4-408">Come gestire gli errori nella classe HubHow to handle errors in the Hub class</span><span class="sxs-lookup"><span data-stu-id="197c4-408">How to handle errors in the Hub class</span></span>

<span data-ttu-id="197c4-409">Per gestire gli errori che si verificano nei metodi della classe Hub, assicurarsi innanzitutto di `await`"osservare" tutte le eccezioni derivanti da operazioni asincrone (ad esempio la chiamata di metodi client) utilizzando .</span><span class="sxs-lookup"><span data-stu-id="197c4-409">To handle errors that occur in your Hub class methods, first ensure you "observe" any exceptions from async operations (such as invoking client methods) by using `await`.</span></span> <span data-ttu-id="197c4-410">Utilizzare quindi uno o più dei seguenti metodi:</span><span class="sxs-lookup"><span data-stu-id="197c4-410">Then use one or more of the following methods:</span></span>

- <span data-ttu-id="197c4-411">Eseguire il wrapping del codice del metodo in blocchi try-catch e registrare l'oggetto eccezione.</span><span class="sxs-lookup"><span data-stu-id="197c4-411">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="197c4-412">Ai fini del debug è possibile inviare l'eccezione al client, ma per motivi di sicurezza non è consigliabile inviare informazioni dettagliate ai client nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="197c4-412">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="197c4-413">Creare un modulo della pipeline Hubs che gestisce il [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) metodo.</span><span class="sxs-lookup"><span data-stu-id="197c4-413">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="197c4-414">Nell'esempio seguente viene illustrato un modulo della pipeline che registra gli errori, seguito dal codice in Startup.cs che inserisce il modulo nella pipeline di Hubs.The following example shows a pipeline module that logs errors, followed by code in Startup.cs that injects the module into the Hubs pipeline.</span><span class="sxs-lookup"><span data-stu-id="197c4-414">The following example shows a pipeline module that logs errors, followed by code in Startup.cs that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- <span data-ttu-id="197c4-415">Utilizzare `HubException` la classe (introdotta in SignalR 2).</span><span class="sxs-lookup"><span data-stu-id="197c4-415">Use the `HubException` class (introduced in SignalR 2).</span></span> <span data-ttu-id="197c4-416">Questo errore può essere generato da qualsiasi chiamata all'hub.</span><span class="sxs-lookup"><span data-stu-id="197c4-416">This error can be thrown from any hub invocation.</span></span> <span data-ttu-id="197c4-417">Il `HubError` costruttore accetta un messaggio stringa e un oggetto per l'archiviazione di dati di errore aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="197c4-417">The `HubError` constructor takes a string message, and an object for storing extra error data.</span></span> <span data-ttu-id="197c4-418">SignalR serializzerà automaticamente l'eccezione e la invierà al client, dove verrà utilizzata per rifiutare o non eseguire la chiamata al metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="197c4-418">SignalR will auto-serialize the exception and send it to the client, where it will be used to reject or fail the hub method invocation.</span></span>

    <span data-ttu-id="197c4-419">Gli esempi di codice seguenti `HubException` illustrano come generare un errore durante una chiamata all'hub e come gestire l'eccezione nei client JavaScript e .NET.</span><span class="sxs-lookup"><span data-stu-id="197c4-419">The following code samples demonstrate how to throw a `HubException` during a Hub invocation, and how to handle the exception on JavaScript and .NET clients.</span></span>

    <span data-ttu-id="197c4-420">**Codice server che dimostra la classe HubException**</span><span class="sxs-lookup"><span data-stu-id="197c4-420">**Server code demonstrating the HubException class**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    <span data-ttu-id="197c4-421">**Codice client JavaScript che dimostra la risposta alla generazione di un'eccezione HubException in un hub**</span><span class="sxs-lookup"><span data-stu-id="197c4-421">**JavaScript client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    <span data-ttu-id="197c4-422">**Codice client .NET che dimostra la risposta alla generazione di un'eccezione HubException in un hub**</span><span class="sxs-lookup"><span data-stu-id="197c4-422">**.NET client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

<span data-ttu-id="197c4-423">Per altre informazioni sui moduli della pipeline hub, vedere [Come personalizzare la pipeline Hub](#hubpipeline) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="197c4-423">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="197c4-424">Come abilitare la traccia</span><span class="sxs-lookup"><span data-stu-id="197c4-424">How to enable tracing</span></span>

<span data-ttu-id="197c4-425">Per abilitare l'analisi lato server, aggiungere un elemento system.diagnostics al file Web.config, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="197c4-425">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

<span data-ttu-id="197c4-426">Quando si esegue l'applicazione in Visual Studio, è possibile visualizzare i log nel **Output** finestra.</span><span class="sxs-lookup"><span data-stu-id="197c4-426">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="197c4-427">Come chiamare i metodi client e gestire i gruppi dall'esterno della classe Hub</span><span class="sxs-lookup"><span data-stu-id="197c4-427">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="197c4-428">Per chiamare i metodi client da una classe diversa rispetto alla classe Hub, ottenere un riferimento all'oggetto di contesto SignalR per l'hub e usarlo per chiamare i metodi sul client o gestire i gruppi.</span><span class="sxs-lookup"><span data-stu-id="197c4-428">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="197c4-429">La classe `StockTicker` di esempio seguente ottiene l'oggetto di contesto, lo archivia in un'istanza della classe, archivia `updateStockPrice` l'istanza della classe in `StockTickerHub`una proprietà statica e utilizza il contesto dall'istanza della classe singleton per chiamare il metodo sui client connessi a un Hub denominato .</span><span class="sxs-lookup"><span data-stu-id="197c4-429">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

<span data-ttu-id="197c4-430">Se è necessario utilizzare il contesto più volte in un oggetto di lunga durata, ottenere il riferimento una sola volta e salvarlo anziché ottenerlo ogni volta.</span><span class="sxs-lookup"><span data-stu-id="197c4-430">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="197c4-431">Ottenere il contesto una volta assicura che SignalR invia messaggi ai client nella stessa sequenza in cui i metodi Hub rendono le chiamate al metodo client.</span><span class="sxs-lookup"><span data-stu-id="197c4-431">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="197c4-432">Per un'esercitazione che illustra come usare il contesto SignalR per un hub, vedere [Trasmissione server con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="197c4-432">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="197c4-433">Chiamata di metodi clientCalling client methods</span><span class="sxs-lookup"><span data-stu-id="197c4-433">Calling client methods</span></span>

<span data-ttu-id="197c4-434">È possibile specificare quali client riceveranno RPC, ma sono disponibili meno opzioni rispetto a quando si chiama da una classe Hub.</span><span class="sxs-lookup"><span data-stu-id="197c4-434">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="197c4-435">Il motivo è che il contesto non è associato a una particolare chiamata da un client, pertanto i metodi che richiedono la conoscenza dell'ID di connessione corrente, ad `Clients.Others`esempio , o `Clients.Caller`, o `Clients.OthersInGroup`, non sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="197c4-435">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="197c4-436">Sono disponibili le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="197c4-436">The following options are available:</span></span>

- <span data-ttu-id="197c4-437">Tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="197c4-437">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- <span data-ttu-id="197c4-438">Un client specifico identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="197c4-438">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- <span data-ttu-id="197c4-439">Tutti i client connessi ad eccezione dei client specificati, identificati dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="197c4-439">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- <span data-ttu-id="197c4-440">Tutti i client connessi in un gruppo specificato.</span><span class="sxs-lookup"><span data-stu-id="197c4-440">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- <span data-ttu-id="197c4-441">Tutti i client connessi in un gruppo specificato ad eccezione dei client specificati, identificati dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="197c4-441">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

<span data-ttu-id="197c4-442">Se si chiama la classe non Hub dai metodi della classe Hub, è possibile `Clients.Client`passare `Clients.AllExcept`l'ID `Clients.Caller` `Clients.Others`di `Clients.OthersInGroup`connessione corrente e utilizzarlo con , , o `Clients.Group` per simulare , , o .</span><span class="sxs-lookup"><span data-stu-id="197c4-442">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="197c4-443">Nell'esempio riportato `MoveShapeHub` di seguito, la `Broadcaster` classe passa `Broadcaster` l'ID di connessione alla classe in modo che la classe possa simulare `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="197c4-443">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="197c4-444">Gestione dell'appartenenza ai gruppi</span><span class="sxs-lookup"><span data-stu-id="197c4-444">Managing group membership</span></span>

<span data-ttu-id="197c4-445">Per la gestione dei gruppi hai le stesse opzioni di una classe Hub.</span><span class="sxs-lookup"><span data-stu-id="197c4-445">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="197c4-446">Aggiungere un client a un gruppo</span><span class="sxs-lookup"><span data-stu-id="197c4-446">Add a client to a group</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- <span data-ttu-id="197c4-447">Rimuovere un client da un gruppo</span><span class="sxs-lookup"><span data-stu-id="197c4-447">Remove a client from a group</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="197c4-448">Come personalizzare la pipeline Hubs</span><span class="sxs-lookup"><span data-stu-id="197c4-448">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="197c4-449">SignalR consente di inserire il proprio codice nella pipeline Hub.</span><span class="sxs-lookup"><span data-stu-id="197c4-449">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="197c4-450">Nell'esempio seguente viene illustrato un modulo di pipeline Hub personalizzato che registra ogni chiamata al metodo in ingresso ricevuta dal client e dalla chiamata al metodo in uscita richiamata sul client:The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span><span class="sxs-lookup"><span data-stu-id="197c4-450">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

<span data-ttu-id="197c4-451">Il codice seguente nel file *di Startup.cs* registra il modulo per l'esecuzione nella pipeline Hub:The following code in the file can registers the module to run in the Hub pipeline:</span><span class="sxs-lookup"><span data-stu-id="197c4-451">The following code in the *Startup.cs* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

<span data-ttu-id="197c4-452">Esistono molti metodi diversi di cui è possibile eseguire l'override.</span><span class="sxs-lookup"><span data-stu-id="197c4-452">There are many different methods that you can override.</span></span> <span data-ttu-id="197c4-453">Per un elenco completo, vedere [Metodi HubPipelineModule](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="197c4-453">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
