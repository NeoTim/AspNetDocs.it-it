---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: Guida all'API di ASP.NET SignalR Hubs - Client .NET (SignalR 1.x) | Microsoft Docs
author: bradygaster
description: Questo documento viene fornita un'introduzione all'uso dell'API di hub per la versione 2 nel client .NET, ad esempio Windows Store (WinRT), WPF, Silverlight e gli svantaggi di SignalR...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: dbf62b2f9851e3612885aa5375cd2c3432570643
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57025528"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a><span data-ttu-id="ade59-103">Guida all'API di ASP.NET SignalR Hubs - Client .NET (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="ade59-103">ASP.NET SignalR Hubs API Guide - .NET Client (SignalR 1.x)</span></span>
====================
<span data-ttu-id="ade59-104">dal [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ade59-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="ade59-105">Questo documento fornisce un'introduzione all'uso dell'API di hub per SignalR versione 2 nel client .NET, ad esempio applicazioni console, WPF, Silverlight e Windows Store (WinRT).</span><span class="sxs-lookup"><span data-stu-id="ade59-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="ade59-106">L'API di hub SignalR consente di eseguire chiamate di procedure remote (RPC) da un server ai client connessi e dai client al server.</span><span class="sxs-lookup"><span data-stu-id="ade59-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="ade59-107">Nel codice server, si definiscono i metodi che possono essere chiamati da parte dei client e si chiamano i metodi eseguiti nel client.</span><span class="sxs-lookup"><span data-stu-id="ade59-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="ade59-108">Nel codice client, si definiscono i metodi che possono essere chiamati dal server e si chiamano metodi che eseguono sul server.</span><span class="sxs-lookup"><span data-stu-id="ade59-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="ade59-109">SignalR si occupa di tutte le attività client-server.</span><span class="sxs-lookup"><span data-stu-id="ade59-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="ade59-110">SignalR offre inoltre un'API di livello inferiore denominata connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="ade59-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="ade59-111">Per un'introduzione a SignalR hub e connessioni permanenti o per un'esercitazione che illustra come compilare un'applicazione di SignalR completa, vedere [SignalR - Guida introduttiva](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="ade59-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="ade59-112">Panoramica</span><span class="sxs-lookup"><span data-stu-id="ade59-112">Overview</span></span>

<span data-ttu-id="ade59-113">Questo documento contiene le seguenti sezioni:</span><span class="sxs-lookup"><span data-stu-id="ade59-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="ade59-114">Programma di installazione client</span><span class="sxs-lookup"><span data-stu-id="ade59-114">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="ade59-115">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="ade59-115">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="ade59-116">Connessioni tra domini da client Silverlight</span><span class="sxs-lookup"><span data-stu-id="ade59-116">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="ade59-117">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="ade59-117">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="ade59-118">Come impostare il numero massimo di connessioni simultanee nei client WPF</span><span class="sxs-lookup"><span data-stu-id="ade59-118">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="ade59-119">Come specificare i parametri di stringa di query</span><span class="sxs-lookup"><span data-stu-id="ade59-119">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="ade59-120">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="ade59-120">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="ade59-121">Come specificare le intestazioni HTTP</span><span class="sxs-lookup"><span data-stu-id="ade59-121">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="ade59-122">Come specificare i certificati client</span><span class="sxs-lookup"><span data-stu-id="ade59-122">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="ade59-123">Come creare il proxy dell'Hub</span><span class="sxs-lookup"><span data-stu-id="ade59-123">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="ade59-124">Come definire i metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="ade59-124">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="ade59-125">Metodi senza parametri</span><span class="sxs-lookup"><span data-stu-id="ade59-125">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="ade59-126">Metodi con parametri, che specificano i tipi di parametro</span><span class="sxs-lookup"><span data-stu-id="ade59-126">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="ade59-127">Metodi con parametri, che specificano gli oggetti dinamici per i parametri</span><span class="sxs-lookup"><span data-stu-id="ade59-127">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="ade59-128">Come rimuovere un gestore</span><span class="sxs-lookup"><span data-stu-id="ade59-128">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="ade59-129">Come chiamare i metodi del server dal client</span><span class="sxs-lookup"><span data-stu-id="ade59-129">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="ade59-130">Come gestire gli eventi di durata connessione</span><span class="sxs-lookup"><span data-stu-id="ade59-130">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="ade59-131">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="ade59-131">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="ade59-132">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="ade59-132">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="ade59-133">Esempi di codice di applicazione console per i metodi di client che il server può chiamare, Silverlight e WPF</span><span class="sxs-lookup"><span data-stu-id="ade59-133">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="ade59-134">Per un progetto di client .NET di esempio, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="ade59-134">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="ade59-135">[Gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) in GitHub.com (esempi di app console di WinRT, Silverlight,).</span><span class="sxs-lookup"><span data-stu-id="ade59-135">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="ade59-136">[DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) in GitHub.com (ad esempio WPF).</span><span class="sxs-lookup"><span data-stu-id="ade59-136">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="ade59-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) in GitHub.com (esempio di app Console).</span><span class="sxs-lookup"><span data-stu-id="ade59-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="ade59-138">Per informazioni su come programmare il server o client JavaScript, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="ade59-138">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="ade59-139">Guida all'API di SignalR Hubs - Server</span><span class="sxs-lookup"><span data-stu-id="ade59-139">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="ade59-140">Guida all'API di SignalR Hubs - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="ade59-140">SignalR Hubs API Guide - JavaScript Client</span></span>](../guide-to-the-api/hubs-api-guide-javascript-client.md)

<span data-ttu-id="ade59-141">I collegamenti agli argomenti di riferimento all'API sono alla versione dell'API .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="ade59-141">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="ade59-142">Se si usa .NET 4, vedere [la versione di .NET 4 degli argomenti API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="ade59-142">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="ade59-143">Programma di installazione client</span><span class="sxs-lookup"><span data-stu-id="ade59-143">Client setup</span></span>

<span data-ttu-id="ade59-144">Installare il [ASPNET](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) pacchetto NuGet (non il [ASPNET](http://nuget.org/packages/microsoft.aspnet.signalr) pacchetto).</span><span class="sxs-lookup"><span data-stu-id="ade59-144">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="ade59-145">Questo pacchetto supporta WinRT, Silverlight, WPF, applicazione console e i client di Windows Phone, per .NET 4 e .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="ade59-145">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="ade59-146">Se la versione di SignalR in uso sul client è diversa da quella che si dispone sul server, SignalR è spesso in grado di adattarsi alla differenza.</span><span class="sxs-lookup"><span data-stu-id="ade59-146">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="ade59-147">Ad esempio, quando viene rilasciato SignalR versione 2.0 e si installano che nel server, il server supporterà i client che hanno installato, nonché i client che hanno installato 2.0 1.1.x.</span><span class="sxs-lookup"><span data-stu-id="ade59-147">For example, when SignalR version 2.0 is released and you install that on the server, the server will support clients that have 1.1.x installed as well as clients that have 2.0 installed.</span></span> <span data-ttu-id="ade59-148">Se la differenza tra la versione sul server e la versione del client è eccessiva, SignalR genera un `InvalidOperationException` eccezione quando il client prova a stabilire una connessione.</span><span class="sxs-lookup"><span data-stu-id="ade59-148">If the difference between the version on the server and the version on the client is too great, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="ade59-149">Messaggio di errore "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="ade59-149">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="ade59-150">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="ade59-150">How to establish a connection</span></span>

<span data-ttu-id="ade59-151">Prima di stabilire una connessione, è necessario creare un `HubConnection` dell'oggetto e creare un proxy.</span><span class="sxs-lookup"><span data-stu-id="ade59-151">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="ade59-152">Per stabilire la connessione, chiamare il `Start` metodo di `HubConnection` oggetto.</span><span class="sxs-lookup"><span data-stu-id="ade59-152">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="ade59-153">Per i client JavaScript è necessario registrare almeno un gestore di eventi prima di chiamare il `Start` metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="ade59-153">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="ade59-154">Non è necessario per i client .NET.</span><span class="sxs-lookup"><span data-stu-id="ade59-154">This is not necessary for .NET clients.</span></span> <span data-ttu-id="ade59-155">Per i client JavaScript, il codice proxy generato automaticamente Crea proxy per tutti gli hub che esiste nel server e la registrazione di un gestore di è consente di indicare quali hub il client intende utilizzare.</span><span class="sxs-lookup"><span data-stu-id="ade59-155">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="ade59-156">Ma per un client .NET è creare i proxy di Hub manualmente, in modo da SignalR presuppone che si userà qualsiasi Hub che si crea un proxy per.</span><span class="sxs-lookup"><span data-stu-id="ade59-156">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="ade59-157">Il codice di esempio Usa il valore predefinito "/ signalr" URL per la connessione al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="ade59-157">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="ade59-158">Per informazioni su come specificare un URL di base diversi, vedere [Guida all'API di hub di ASP.NET SignalR - Server - URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="ade59-158">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="ade59-159">Il `Start` metodo viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="ade59-159">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="ade59-160">Per assicurarsi che le successive righe di codice non eseguire fino a dopo aver stabilita la connessione, usare `await` in un metodo asincrono di ASP.NET 4.5 o `.Wait()` in un metodo sincrono.</span><span class="sxs-lookup"><span data-stu-id="ade59-160">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="ade59-161">Non usare `.Wait()` in un client di WinRT.</span><span class="sxs-lookup"><span data-stu-id="ade59-161">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<span data-ttu-id="ade59-162">La classe `HubConnection` è thread-safe.</span><span class="sxs-lookup"><span data-stu-id="ade59-162">The `HubConnection` class is thread-safe.</span></span>

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="ade59-163">Connessioni tra domini da client Silverlight</span><span class="sxs-lookup"><span data-stu-id="ade59-163">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="ade59-164">Per informazioni su come abilitare le connessioni tra domini da client Silverlight, vedere [rendendo un servizio disponibile attraverso i limiti del dominio](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="ade59-164">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="ade59-165">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="ade59-165">How to configure the connection</span></span>

<span data-ttu-id="ade59-166">Prima stabilire una connessione, è possibile specificare una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="ade59-166">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="ade59-167">Limite di connessioni simultanee.</span><span class="sxs-lookup"><span data-stu-id="ade59-167">Concurrent connections limit.</span></span>
- <span data-ttu-id="ade59-168">I parametri della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="ade59-168">Query string parameters.</span></span>
- <span data-ttu-id="ade59-169">Il metodo di trasporto.</span><span class="sxs-lookup"><span data-stu-id="ade59-169">The transport method.</span></span>
- <span data-ttu-id="ade59-170">Intestazioni HTTP.</span><span class="sxs-lookup"><span data-stu-id="ade59-170">HTTP headers.</span></span>
- <span data-ttu-id="ade59-171">Certificati client.</span><span class="sxs-lookup"><span data-stu-id="ade59-171">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="ade59-172">Come impostare il numero massimo di connessioni simultanee nei client WPF</span><span class="sxs-lookup"><span data-stu-id="ade59-172">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="ade59-173">Nei client WPF, potrebbe essere necessario aumentare il numero massimo di connessioni simultanee rispetto al valore predefinito di 2.</span><span class="sxs-lookup"><span data-stu-id="ade59-173">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="ade59-174">Il valore consigliato è 10.</span><span class="sxs-lookup"><span data-stu-id="ade59-174">The recommended value is 10.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="ade59-175">Per altre informazioni, vedere [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="ade59-175">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="ade59-176">Come specificare i parametri di stringa di query</span><span class="sxs-lookup"><span data-stu-id="ade59-176">How to specify query string parameters</span></span>

<span data-ttu-id="ade59-177">Se si desidera inviare dati al server quando il client si connette, è possibile aggiungere parametri della stringa di query all'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="ade59-177">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="ade59-178">Nell'esempio seguente viene illustrato come impostare un parametro di stringa di query nel codice client.</span><span class="sxs-lookup"><span data-stu-id="ade59-178">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="ade59-179">Nell'esempio seguente viene illustrato come leggere un parametro di stringa di query nel codice server.</span><span class="sxs-lookup"><span data-stu-id="ade59-179">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="ade59-180">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="ade59-180">How to specify the transport method</span></span>

<span data-ttu-id="ade59-181">Durante il processo di connessione, un client SignalR normalmente negozia con il server per determinare il migliore trasporto supportato dal server e client.</span><span class="sxs-lookup"><span data-stu-id="ade59-181">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="ade59-182">Se si conosce già il trasporto da usare, è possibile ignorare questo processo di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="ade59-182">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="ade59-183">Per specificare il metodo di trasporto, passare un oggetto di trasporto per il metodo di avvio.</span><span class="sxs-lookup"><span data-stu-id="ade59-183">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="ade59-184">Nell'esempio seguente viene illustrato come specificare il metodo di trasporto nel codice client.</span><span class="sxs-lookup"><span data-stu-id="ade59-184">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="ade59-185">Il [Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) dello spazio dei nomi include le classi seguenti che è possibile usare per specificare il trasporto.</span><span class="sxs-lookup"><span data-stu-id="ade59-185">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="ade59-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="ade59-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="ade59-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="ade59-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="ade59-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponibile solo quando i server e client usare .NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="ade59-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="ade59-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (sceglie automaticamente il migliore trasporto supportato dal client sia sul server.</span><span class="sxs-lookup"><span data-stu-id="ade59-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="ade59-190">Questo è il trasporto predefinito.</span><span class="sxs-lookup"><span data-stu-id="ade59-190">This is the default transport.</span></span> <span data-ttu-id="ade59-191">Il passaggio di questa opzione in per il `Start` metodo ha lo stesso effetto come non conformi in un elemento.)</span><span class="sxs-lookup"><span data-stu-id="ade59-191">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="ade59-192">Il trasporto ForeverFrame non è incluso nell'elenco perché è utilizzato solo dai browser.</span><span class="sxs-lookup"><span data-stu-id="ade59-192">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="ade59-193">Per informazioni su come controllare il metodo di trasporto nel codice server, vedere [- Server - Guida all'API Hubs SignalR di ASP.NET come ottenere informazioni relative al client dalla proprietà di contesto](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="ade59-193">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="ade59-194">Per altre informazioni sui trasporti e i fallback, vedere [Introduzione a SignalR - trasporti e i fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="ade59-194">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="ade59-195">Come specificare le intestazioni HTTP</span><span class="sxs-lookup"><span data-stu-id="ade59-195">How to specify HTTP headers</span></span>

<span data-ttu-id="ade59-196">Per impostare le intestazioni HTTP, usare il `Headers` proprietà sull'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="ade59-196">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="ade59-197">Nell'esempio seguente viene illustrato come aggiungere un'intestazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="ade59-197">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="ade59-198">Come specificare i certificati client</span><span class="sxs-lookup"><span data-stu-id="ade59-198">How to specify client certificates</span></span>

<span data-ttu-id="ade59-199">Per aggiungere i certificati client, usare il `AddClientCertificate` metodo sull'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="ade59-199">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="ade59-200">Come creare il proxy dell'Hub</span><span class="sxs-lookup"><span data-stu-id="ade59-200">How to create the Hub proxy</span></span>

<span data-ttu-id="ade59-201">Per definire i metodi sul client che un Hub è possibile chiamare dal server e per richiamare metodi su un Hub sul server, creare un proxy per l'Hub chiamando `CreateHubProxy` sull'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="ade59-201">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="ade59-202">La stringa è passare al `CreateHubProxy` è il nome della classe Hub o il nome specificato da di `HubName` attributo se è stata utilizzata nel server.</span><span class="sxs-lookup"><span data-stu-id="ade59-202">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="ade59-203">Nome viene applicata alcuna distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="ade59-203">Name matching is case-insensitive.</span></span>

<span data-ttu-id="ade59-204">**Classe dell'hub sul server**</span><span class="sxs-lookup"><span data-stu-id="ade59-204">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="ade59-205">**Creare il proxy client per la classe dell'Hub**</span><span class="sxs-lookup"><span data-stu-id="ade59-205">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="ade59-206">Se si decora la classe di Hub con un `HubName` attributo, usare il nome.</span><span class="sxs-lookup"><span data-stu-id="ade59-206">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="ade59-207">**Classe dell'hub sul server**</span><span class="sxs-lookup"><span data-stu-id="ade59-207">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="ade59-208">**Creare il proxy client per la classe dell'Hub**</span><span class="sxs-lookup"><span data-stu-id="ade59-208">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="ade59-209">L'oggetto proxy è thread-safe.</span><span class="sxs-lookup"><span data-stu-id="ade59-209">The proxy object is thread-safe.</span></span> <span data-ttu-id="ade59-210">In effetti, se si chiama `HubConnection.CreateHubProxy` più volte con lo stesso `hubName`, si ottiene lo stesso memorizzato nella cache `IHubProxy` oggetto.</span><span class="sxs-lookup"><span data-stu-id="ade59-210">In fact, if you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="ade59-211">Come definire i metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="ade59-211">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="ade59-212">Per definire un metodo che può chiamare il server, usare il proxy `On` metodo per registrare un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="ade59-212">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="ade59-213">Metodo nome viene applicata alcuna distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="ade59-213">Method name matching is case-insensitive.</span></span> <span data-ttu-id="ade59-214">Ad esempio, `Clients.All.UpdateStockPrice` sul server eseguirà `updateStockPrice`, `updatestockprice`, o `UpdateStockPrice` sul client.</span><span class="sxs-lookup"><span data-stu-id="ade59-214">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="ade59-215">Piattaforme client diverse hanno requisiti diversi per la modalità di scrittura codice del metodo per aggiornare l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="ade59-215">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="ade59-216">Gli esempi illustrati sono per i client di WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="ade59-216">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="ade59-217">WPF, Silverlight ed esempi di applicazione console vengono forniti [una sezione separata più avanti in questo argomento](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="ade59-217">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="ade59-218">Metodi senza parametri</span><span class="sxs-lookup"><span data-stu-id="ade59-218">Methods without parameters</span></span>

<span data-ttu-id="ade59-219">Se il metodo in fase di gestione non dispone di parametri, usare l'overload del metodo non generico di `On` metodo:</span><span class="sxs-lookup"><span data-stu-id="ade59-219">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="ade59-220">**Codice lato server chiamata client metodo senza parametri**</span><span class="sxs-lookup"><span data-stu-id="ade59-220">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="ade59-221">**Codice di WinRT Client per il metodo chiamato dal server senza parametri ([vedere gli esempi WPF e Silverlight più avanti in questo argomento](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="ade59-221">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="ade59-222">Metodi con parametri, che specificano i tipi di parametro</span><span class="sxs-lookup"><span data-stu-id="ade59-222">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="ade59-223">Se il metodo in fase di gestione dispone di parametri, specificare i tipi dei parametri come i tipi generici del `On` (metodo).</span><span class="sxs-lookup"><span data-stu-id="ade59-223">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="ade59-224">Esistono overload generici del `On` metodo per consentire di specificare i parametri fino a 8 (4 in Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="ade59-224">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="ade59-225">Nell'esempio seguente, viene inviato un solo parametro per il `UpdateStockPrice` (metodo).</span><span class="sxs-lookup"><span data-stu-id="ade59-225">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="ade59-226">**Metodo client con un parametro di chiamata del codice server**</span><span class="sxs-lookup"><span data-stu-id="ade59-226">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="ade59-227">**La classe Stock utilizzata per il parametro**</span><span class="sxs-lookup"><span data-stu-id="ade59-227">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="ade59-228">**Il codice WinRT Client per un metodo chiamato dal server con un parametro ([vedere gli esempi WPF e Silverlight più avanti in questo argomento](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="ade59-228">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="ade59-229">Metodi con parametri, che specificano gli oggetti dinamici per i parametri</span><span class="sxs-lookup"><span data-stu-id="ade59-229">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="ade59-230">Come alternativa per specificare parametri come tipi generici del `On` metodo, è possibile specificare parametri come oggetti dinamici:</span><span class="sxs-lookup"><span data-stu-id="ade59-230">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="ade59-231">**Metodo client con un parametro di chiamata del codice server**</span><span class="sxs-lookup"><span data-stu-id="ade59-231">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="ade59-232">**La classe Stock utilizzata per il parametro**</span><span class="sxs-lookup"><span data-stu-id="ade59-232">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="ade59-233">**Il codice WinRT Client per un metodo chiamato dal server con un parametro, usando un oggetto dinamico per il parametro ([vedere gli esempi WPF e Silverlight più avanti in questo argomento](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="ade59-233">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="ade59-234">Come rimuovere un gestore</span><span class="sxs-lookup"><span data-stu-id="ade59-234">How to remove a handler</span></span>

<span data-ttu-id="ade59-235">Per rimuovere un gestore, chiamare il `Dispose` (metodo).</span><span class="sxs-lookup"><span data-stu-id="ade59-235">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="ade59-236">**Codice client per un metodo chiamato dal server**</span><span class="sxs-lookup"><span data-stu-id="ade59-236">**Client code for a method called from server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="ade59-237">**Codice client per rimuovere il gestore**</span><span class="sxs-lookup"><span data-stu-id="ade59-237">**Client code to remove the handler**</span></span>

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="ade59-238">Come chiamare i metodi del server dal client</span><span class="sxs-lookup"><span data-stu-id="ade59-238">How to call server methods from the client</span></span>

<span data-ttu-id="ade59-239">Per chiamare un metodo nel server, usare il `Invoke` metodo sul proxy dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="ade59-239">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="ade59-240">Se il metodo del server non restituisce alcun valore, usare l'overload del metodo non generico di `Invoke` (metodo).</span><span class="sxs-lookup"><span data-stu-id="ade59-240">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="ade59-241">**Codice del server per un metodo che non restituisce alcun valore**</span><span class="sxs-lookup"><span data-stu-id="ade59-241">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="ade59-242">**Codice client chiama un metodo che non restituisce alcun valore**</span><span class="sxs-lookup"><span data-stu-id="ade59-242">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="ade59-243">Se il metodo del server ha un valore restituito, specificare il tipo restituito come tipo generico del `Invoke` (metodo).</span><span class="sxs-lookup"><span data-stu-id="ade59-243">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="ade59-244">**Codice server per un metodo che presenta un valore restituito e accetta un parametro di tipo complesso**</span><span class="sxs-lookup"><span data-stu-id="ade59-244">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="ade59-245">**La classe Stock utilizzato per il parametro e valore restituito**</span><span class="sxs-lookup"><span data-stu-id="ade59-245">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="ade59-246">**Codice client chiama un metodo che presenta un valore restituito e accetta un parametro di tipo complesso, in un metodo asincrono di ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="ade59-246">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="ade59-247">**Codice client chiama un metodo che presenta un valore restituito e accetta un parametro di tipo complesso, in un metodo sincrono**</span><span class="sxs-lookup"><span data-stu-id="ade59-247">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="ade59-248">Il `Invoke` metodo viene eseguito in modo asincrono e restituisce un `Task` oggetto.</span><span class="sxs-lookup"><span data-stu-id="ade59-248">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="ade59-249">Se non si specifica `await` o `.Wait()`, la riga successiva del codice verrà eseguito prima il metodo che si richiama ha terminato l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="ade59-249">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="ade59-250">Come gestire gli eventi di durata connessione</span><span class="sxs-lookup"><span data-stu-id="ade59-250">How to handle connection lifetime events</span></span>

<span data-ttu-id="ade59-251">SignalR fornisce il collegamento seguente gli eventi di durata che è possibile gestire:</span><span class="sxs-lookup"><span data-stu-id="ade59-251">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="ade59-252">`Received`: Generato quando vengono ricevuti tutti i dati nella connessione.</span><span class="sxs-lookup"><span data-stu-id="ade59-252">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="ade59-253">Fornisce i dati ricevuti.</span><span class="sxs-lookup"><span data-stu-id="ade59-253">Provides the received data.</span></span>
- <span data-ttu-id="ade59-254">`ConnectionSlow`: Generato quando il client rileva una connessione lenta o frequentemente eliminazione.</span><span class="sxs-lookup"><span data-stu-id="ade59-254">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="ade59-255">`Reconnecting`: Generato quando il trasporto sottostante inizia la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="ade59-255">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="ade59-256">`Reconnected`: Generato quando il trasporto sottostante è stata riconnessa.</span><span class="sxs-lookup"><span data-stu-id="ade59-256">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="ade59-257">`StateChanged`: Generato quando cambia lo stato della connessione.</span><span class="sxs-lookup"><span data-stu-id="ade59-257">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="ade59-258">Fornisce lo stato precedente e il nuovo stato.</span><span class="sxs-lookup"><span data-stu-id="ade59-258">Provides the old state and the new state.</span></span> <span data-ttu-id="ade59-259">Per informazioni sulla connessione di vedere i valori dello stato [enumerazione ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="ade59-259">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="ade59-260">`Closed`: Generato quando la connessione è disconnesso.</span><span class="sxs-lookup"><span data-stu-id="ade59-260">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="ade59-261">Ad esempio, se si desidera visualizzare i messaggi di avviso per gli errori non irreversibili ma causano problemi di connessione intermittente, ad esempio come lentezza o di frequenza eliminazione della connessione, gestire il `ConnectionSlow` evento.</span><span class="sxs-lookup"><span data-stu-id="ade59-261">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="ade59-262">Per altre informazioni, vedere [comprensione e la gestione degli eventi di durata connessione in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="ade59-262">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="ade59-263">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="ade59-263">How to handle errors</span></span>

<span data-ttu-id="ade59-264">Se si non abilita in modo esplicito i messaggi di errore dettagliato nel server, l'oggetto eccezione SignalR restituito dopo un errore contiene informazioni minime sull'errore.</span><span class="sxs-lookup"><span data-stu-id="ade59-264">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="ade59-265">Ad esempio, se una chiamata a `newContosoChatMessage` ha esito negativo, il messaggio di errore nell'oggetto errore contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" invio di messaggi di errore dettagliati per i client nell'ambiente di produzione non è consigliato per motivi di sicurezza, ma se si vuole abilitare i messaggi di errore dettagliato per risoluzione dei problemi, usare il codice seguente nel server.</span><span class="sxs-lookup"><span data-stu-id="ade59-265">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="ade59-266">Per gestire gli errori che genera SignalR, è possibile aggiungere un gestore per il `Error` evento sull'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="ade59-266">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="ade59-267">Per gestire gli errori di chiamate al metodo, eseguire il wrapping di codice in un blocco try-catch.</span><span class="sxs-lookup"><span data-stu-id="ade59-267">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="ade59-268">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="ade59-268">How to enable client-side logging</span></span>

<span data-ttu-id="ade59-269">Per abilitare la registrazione lato client, impostare il `TraceLevel` e `TraceWriter` le proprietà dell'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="ade59-269">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="ade59-270">Esempi di codice di applicazione console per i metodi di client che il server può chiamare, Silverlight e WPF</span><span class="sxs-lookup"><span data-stu-id="ade59-270">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="ade59-271">Gli esempi di codice illustrati in precedenza per la definizione di metodi di client può chiamare il server si applicano ai client di WinRT.</span><span class="sxs-lookup"><span data-stu-id="ade59-271">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="ade59-272">Gli esempi seguenti mostrano il codice equivalente per WPF, Silverlight e i client di applicazione console.</span><span class="sxs-lookup"><span data-stu-id="ade59-272">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="ade59-273">Metodi senza parametri</span><span class="sxs-lookup"><span data-stu-id="ade59-273">Methods without parameters</span></span>

<span data-ttu-id="ade59-274">**Codice client WPF per il metodo chiamato dal server senza parametri**</span><span class="sxs-lookup"><span data-stu-id="ade59-274">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="ade59-275">**Codice client Silverlight per il metodo chiamato dal server senza parametri**</span><span class="sxs-lookup"><span data-stu-id="ade59-275">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="ade59-276">**Il codice client dell'applicazione console per il metodo chiamato dal server senza parametri**</span><span class="sxs-lookup"><span data-stu-id="ade59-276">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="ade59-277">Metodi con parametri, che specificano i tipi di parametro</span><span class="sxs-lookup"><span data-stu-id="ade59-277">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="ade59-278">**Codice client WPF per un metodo chiamato dal server con un parametro**</span><span class="sxs-lookup"><span data-stu-id="ade59-278">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="ade59-279">**Codice client Silverlight per un metodo chiamato dal server con un parametro**</span><span class="sxs-lookup"><span data-stu-id="ade59-279">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="ade59-280">**Il codice client dell'applicazione console per un metodo chiamato dal server con un parametro**</span><span class="sxs-lookup"><span data-stu-id="ade59-280">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="ade59-281">Metodi con parametri, che specificano gli oggetti dinamici per i parametri</span><span class="sxs-lookup"><span data-stu-id="ade59-281">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="ade59-282">**Codice client WPF per un metodo chiamato dal server con un parametro, usando un oggetto dinamico per il parametro**</span><span class="sxs-lookup"><span data-stu-id="ade59-282">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="ade59-283">**Codice client Silverlight per un metodo chiamato dal server con un parametro, usando un oggetto dinamico per il parametro**</span><span class="sxs-lookup"><span data-stu-id="ade59-283">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="ade59-284">**Il codice client dell'applicazione console per un metodo chiamato dal server con un parametro, usando un oggetto dinamico per il parametro**</span><span class="sxs-lookup"><span data-stu-id="ade59-284">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
