---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: ASP.NET Guida all'API di SignalR Hubs - Client .NET (C) Documenti Microsoft
author: bradygaster
description: Questo documento fornisce un'introduzione all'uso dell'API Hubs per SignalR versione 2 nei client .NET, ad esempio Windows Store (WinRT), WPF, Silverlight e contro...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: d3536f1c15cd7dad7cd660becf0577e5c131f707
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676335"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-c"></a><span data-ttu-id="fcda9-103">Guida all'API di ASP.NET Dividi Di SignalR - Client .NET (C )The SignalR Hubs API Guide - .NET Client (C</span><span class="sxs-lookup"><span data-stu-id="fcda9-103">ASP.NET SignalR Hubs API Guide - .NET Client (C#)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="fcda9-104">Questo documento fornisce un'introduzione all'uso dell'API Hubs per SignalR versione 2 nei client .NET, ad esempio Windows Store (WinRT), WPF, Silverlight e applicazioni console.</span><span class="sxs-lookup"><span data-stu-id="fcda9-104">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
>
> <span data-ttu-id="fcda9-105">L'API Hubs SignalR consente di effettuare chiamate di procedura remota (RPC) da un server ai client connessi e dai client al server.</span><span class="sxs-lookup"><span data-stu-id="fcda9-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="fcda9-106">Nel codice server, si definiscono i metodi che possono essere chiamati dai client e si chiamano i metodi eseguiti sul client.</span><span class="sxs-lookup"><span data-stu-id="fcda9-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="fcda9-107">Nel codice client si definiscono i metodi che possono essere chiamati dal server e si chiamano i metodi eseguiti sul server.</span><span class="sxs-lookup"><span data-stu-id="fcda9-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="fcda9-108">SignalR si occupa di tutto l'impianto idraulico client-server per voi.</span><span class="sxs-lookup"><span data-stu-id="fcda9-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="fcda9-109">SignalR offre anche un'API di livello inferiore denominata connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="fcda9-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="fcda9-110">Per un'introduzione a SignalR, Hub e connessioni permanenti oppure per un'esercitazione che illustra come compilare un'applicazione SignalR completa, vedere [SignalR - Getting Started](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="fcda9-110">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="fcda9-111">Versioni software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="fcda9-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="fcda9-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="fcda9-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="fcda9-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="fcda9-113">.NET 4.5</span></span>
> - <span data-ttu-id="fcda9-114">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="fcda9-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="fcda9-115">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="fcda9-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="fcda9-116">Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR Older Versions](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="fcda9-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="fcda9-117">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="fcda9-117">Questions and comments</span></span>
>
> <span data-ttu-id="fcda9-118">Si prega di lasciare un feedback su come ti è piaciuto questo tutorial e cosa potremmo migliorare nei commenti in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="fcda9-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="fcda9-119">Se avete domande che non sono direttamente correlati al tutorial, è possibile pubblicarli al [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="fcda9-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="fcda9-120">Panoramica</span><span class="sxs-lookup"><span data-stu-id="fcda9-120">Overview</span></span>

<span data-ttu-id="fcda9-121">Questo documento contiene le seguenti sezioni:</span><span class="sxs-lookup"><span data-stu-id="fcda9-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="fcda9-122">Configurazione client</span><span class="sxs-lookup"><span data-stu-id="fcda9-122">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="fcda9-123">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="fcda9-123">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="fcda9-124">Connessioni tra domini da client Silverlight</span><span class="sxs-lookup"><span data-stu-id="fcda9-124">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="fcda9-125">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="fcda9-125">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="fcda9-126">Come impostare il numero massimo di connessioni simultanee nei client WPF</span><span class="sxs-lookup"><span data-stu-id="fcda9-126">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="fcda9-127">Come specificare i parametri della stringa di queryHow to specify query string parameters</span><span class="sxs-lookup"><span data-stu-id="fcda9-127">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="fcda9-128">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="fcda9-128">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="fcda9-129">Come specificare le intestazioni HTTP</span><span class="sxs-lookup"><span data-stu-id="fcda9-129">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="fcda9-130">Come specificare i certificati client</span><span class="sxs-lookup"><span data-stu-id="fcda9-130">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="fcda9-131">Come creare il proxy hub</span><span class="sxs-lookup"><span data-stu-id="fcda9-131">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="fcda9-132">Come definire i metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="fcda9-132">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="fcda9-133">Metodi senza parametri</span><span class="sxs-lookup"><span data-stu-id="fcda9-133">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="fcda9-134">Metodi con parametri, specifica dei tipi di parametro</span><span class="sxs-lookup"><span data-stu-id="fcda9-134">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="fcda9-135">Metodi con parametri, specifica di oggetti dinamici per i parametri</span><span class="sxs-lookup"><span data-stu-id="fcda9-135">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="fcda9-136">Come rimuovere un gestore</span><span class="sxs-lookup"><span data-stu-id="fcda9-136">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="fcda9-137">Come chiamare i metodi server dal client</span><span class="sxs-lookup"><span data-stu-id="fcda9-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="fcda9-138">Come gestire gli eventi di durata della connessioneHow to handle connection lifetime events</span><span class="sxs-lookup"><span data-stu-id="fcda9-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="fcda9-139">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="fcda9-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="fcda9-140">Come abilitare la registrazione lato clientHow to enable client-side logging</span><span class="sxs-lookup"><span data-stu-id="fcda9-140">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="fcda9-141">Esempi di codice WPF, Silverlight e applicazione console per i metodi client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="fcda9-141">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="fcda9-142">Per progetti client .NET di esempio, vedere le risorse seguenti:For a sample .NET client projects, see the following resources:</span><span class="sxs-lookup"><span data-stu-id="fcda9-142">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="fcda9-143">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) in GitHub.com (WinRT, Silverlight, esempi di app console).</span><span class="sxs-lookup"><span data-stu-id="fcda9-143">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="fcda9-144">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) in GitHub.com (esempio WPF).</span><span class="sxs-lookup"><span data-stu-id="fcda9-144">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="fcda9-145">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) su GitHub.com (esempio di applicazione Console).</span><span class="sxs-lookup"><span data-stu-id="fcda9-145">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="fcda9-146">Per la documentazione su come programmare i client server o JavaScript, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="fcda9-146">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="fcda9-147">Guida all'API di SignalR Hubs - Server</span><span class="sxs-lookup"><span data-stu-id="fcda9-147">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="fcda9-148">Guida all'API di SignalR Hubs - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="fcda9-148">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)

<span data-ttu-id="fcda9-149">I collegamenti agli argomenti di riferimento per le API sono relativi alla versione .NET 4.5 dell'API.</span><span class="sxs-lookup"><span data-stu-id="fcda9-149">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="fcda9-150">Se si utilizza .NET 4, vedere [la versione .NET 4 degli argomenti API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="fcda9-150">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="fcda9-151">Configurazione client</span><span class="sxs-lookup"><span data-stu-id="fcda9-151">Client setup</span></span>

<span data-ttu-id="fcda9-152">Installare il pacchetto [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet (non il pacchetto [Microsoft.AspNet.SignalR).](http://nuget.org/packages/microsoft.aspnet.signalr)</span><span class="sxs-lookup"><span data-stu-id="fcda9-152">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="fcda9-153">Questo pacchetto supporta winRT, Silverlight, WPF, applicazione console e client Windows Phone, sia per .NET 4 che per .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="fcda9-153">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="fcda9-154">Se la versione di SignalR che hai sul client è diversa dalla versione che hai sul server, SignalR è spesso in grado di adattarsi alla differenza.</span><span class="sxs-lookup"><span data-stu-id="fcda9-154">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="fcda9-155">Ad esempio, un server che esegue SignalR versione 2 supporterà i client in cui è installato 1.1.x, nonché i client in cui è installata la versione 2.</span><span class="sxs-lookup"><span data-stu-id="fcda9-155">For example, a server running SignalR version 2 will support clients that have 1.1.x installed as well as clients that have version 2 installed.</span></span> <span data-ttu-id="fcda9-156">Se la differenza tra la versione sul server e la versione sul client è troppo grande o se `InvalidOperationException` il client è più recente del server, SignalR genera un'eccezione quando il client tenta di stabilire una connessione.</span><span class="sxs-lookup"><span data-stu-id="fcda9-156">If the difference between the version on the server and the version on the client is too great, or if the client is newer than the server, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="fcda9-157">Il messaggio di`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`errore è " ".</span><span class="sxs-lookup"><span data-stu-id="fcda9-157">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="fcda9-158">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="fcda9-158">How to establish a connection</span></span>

<span data-ttu-id="fcda9-159">Prima di poter stabilire una connessione, `HubConnection` è necessario creare un oggetto e un proxy.</span><span class="sxs-lookup"><span data-stu-id="fcda9-159">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="fcda9-160">Per stabilire la connessione, chiamare il `Start` metodo sull'oggetto. `HubConnection`</span><span class="sxs-lookup"><span data-stu-id="fcda9-160">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,5)]

> [!NOTE]
> <span data-ttu-id="fcda9-161">Per i client JavaScript è necessario registrare `Start` almeno un gestore eventi prima di chiamare il metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="fcda9-161">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="fcda9-162">Questa operazione non è necessaria per i client .NET.</span><span class="sxs-lookup"><span data-stu-id="fcda9-162">This is not necessary for .NET clients.</span></span> <span data-ttu-id="fcda9-163">Per i client JavaScript, il codice proxy generato crea automaticamente proxy per tutti gli hub presenti nel server e la registrazione di un gestore indica quale Hub intende utilizzare il client.</span><span class="sxs-lookup"><span data-stu-id="fcda9-163">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="fcda9-164">Ma per un client .NET si creano proxy Hub manualmente, in modo SignalR presuppone che si utilizzerà qualsiasi Hub che si crea un proxy per.</span><span class="sxs-lookup"><span data-stu-id="fcda9-164">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>

<span data-ttu-id="fcda9-165">Il codice di esempio usa l'URL predefinito "/signalr" per connettersi al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="fcda9-165">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="fcda9-166">Per informazioni su come specificare un URL di base diverso, vedere [Guida all'API di ASP.NET SignalR Hubs - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="fcda9-166">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="fcda9-167">Il `Start` metodo viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="fcda9-167">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="fcda9-168">Per assicurarsi che le righe di codice successive non vengano eseguite fino a quando non viene stabilita la connessione, utilizzare `await` in un metodo asincrono ASP.NET 4.5 o `.Wait()` in un metodo sincrono.</span><span class="sxs-lookup"><span data-stu-id="fcda9-168">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="fcda9-169">Non utilizzare `.Wait()` in un client WinRT.</span><span class="sxs-lookup"><span data-stu-id="fcda9-169">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="fcda9-170">Connessioni tra domini da client Silverlight</span><span class="sxs-lookup"><span data-stu-id="fcda9-170">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="fcda9-171">Per informazioni su come abilitare le connessioni tra domini dai client Silverlight, vedere [Rendere un servizio disponibile attraverso i limiti](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx)del dominio .</span><span class="sxs-lookup"><span data-stu-id="fcda9-171">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="fcda9-172">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="fcda9-172">How to configure the connection</span></span>

<span data-ttu-id="fcda9-173">Prima di stabilire una connessione, è possibile specificare una delle seguenti opzioni:</span><span class="sxs-lookup"><span data-stu-id="fcda9-173">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="fcda9-174">Limite di connessioni simultanee.</span><span class="sxs-lookup"><span data-stu-id="fcda9-174">Concurrent connections limit.</span></span>
- <span data-ttu-id="fcda9-175">Parametri della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="fcda9-175">Query string parameters.</span></span>
- <span data-ttu-id="fcda9-176">Metodo di trasporto.</span><span class="sxs-lookup"><span data-stu-id="fcda9-176">The transport method.</span></span>
- <span data-ttu-id="fcda9-177">Intestazioni HTTP.</span><span class="sxs-lookup"><span data-stu-id="fcda9-177">HTTP headers.</span></span>
- <span data-ttu-id="fcda9-178">Certificati client.</span><span class="sxs-lookup"><span data-stu-id="fcda9-178">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="fcda9-179">Come impostare il numero massimo di connessioni simultanee nei client WPF</span><span class="sxs-lookup"><span data-stu-id="fcda9-179">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="fcda9-180">Nei client WPFWPF potrebbe essere necessario aumentare il numero massimo di connessioni simultanee rispetto al valore predefinito pari a 2.In WPFWPF client, you might have to increase the maximum number of concurrent connections from its default value of 2.</span><span class="sxs-lookup"><span data-stu-id="fcda9-180">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="fcda9-181">Il valore consigliato è 10.</span><span class="sxs-lookup"><span data-stu-id="fcda9-181">The recommended value is 10.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=5)]

<span data-ttu-id="fcda9-182">Per ulteriori informazioni, vedere [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="fcda9-182">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="fcda9-183">Come specificare i parametri della stringa di queryHow to specify query string parameters</span><span class="sxs-lookup"><span data-stu-id="fcda9-183">How to specify query string parameters</span></span>

<span data-ttu-id="fcda9-184">Se si desidera inviare dati al server quando il client si connette, è possibile aggiungere parametri di stringa di query all'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="fcda9-184">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="fcda9-185">Nell'esempio seguente viene illustrato come impostare un parametro della stringa di query nel codice client.</span><span class="sxs-lookup"><span data-stu-id="fcda9-185">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="fcda9-186">Nell'esempio seguente viene illustrato come leggere un parametro di stringa di query nel codice server.</span><span class="sxs-lookup"><span data-stu-id="fcda9-186">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="fcda9-187">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="fcda9-187">How to specify the transport method</span></span>

<span data-ttu-id="fcda9-188">Come parte del processo di connessione, un client SignalR in genere negozia con il server per determinare il trasporto migliore supportato sia dal server che dal client.</span><span class="sxs-lookup"><span data-stu-id="fcda9-188">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="fcda9-189">Se si conosce già il trasporto che si desidera utilizzare, è possibile ignorare questo processo di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="fcda9-189">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="fcda9-190">Per specificare il metodo di trasporto, passare un oggetto di trasporto al metodo Start.To specify the transport method, pass in a transport object to the Start method.</span><span class="sxs-lookup"><span data-stu-id="fcda9-190">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="fcda9-191">Nell'esempio seguente viene illustrato come specificare il metodo di trasporto nel codice client.</span><span class="sxs-lookup"><span data-stu-id="fcda9-191">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=5)]

<span data-ttu-id="fcda9-192">Lo spazio dei nomi [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) include le classi seguenti che è possibile utilizzare per specificare il trasporto.</span><span class="sxs-lookup"><span data-stu-id="fcda9-192">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="fcda9-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="fcda9-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="fcda9-194">[ServerSentEventsTransport (Informazioni su due)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="fcda9-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="fcda9-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponibile solo quando sia il server che il client utilizzano .NET 4.5.)</span><span class="sxs-lookup"><span data-stu-id="fcda9-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="fcda9-196">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (sceglie automaticamente il trasporto migliore supportato sia dal client che dal server.</span><span class="sxs-lookup"><span data-stu-id="fcda9-196">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="fcda9-197">Questo è il trasporto predefinito.</span><span class="sxs-lookup"><span data-stu-id="fcda9-197">This is the default transport.</span></span> <span data-ttu-id="fcda9-198">Passare questo al `Start` metodo ha lo stesso effetto di non passare nulla.)</span><span class="sxs-lookup"><span data-stu-id="fcda9-198">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="fcda9-199">Il trasporto ForeverFrame non è incluso in questo elenco perché viene utilizzato solo dai browser.</span><span class="sxs-lookup"><span data-stu-id="fcda9-199">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="fcda9-200">Per informazioni su come controllare il metodo di trasporto nel codice server, vedere [ASP.NET Guida all'API di HubS SignalR - Server - Come ottenere informazioni sul client dalla proprietà Context](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="fcda9-200">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="fcda9-201">Per ulteriori informazioni sui trasporti e i fallback, vedere [Introduzione a SignalR - Trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="fcda9-201">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="fcda9-202">Come specificare le intestazioni HTTP</span><span class="sxs-lookup"><span data-stu-id="fcda9-202">How to specify HTTP headers</span></span>

<span data-ttu-id="fcda9-203">Per impostare le `Headers` intestazioni HTTP, utilizzare la proprietà nell'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="fcda9-203">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="fcda9-204">Nell'esempio seguente viene illustrato come aggiungere un'intestazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="fcda9-204">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="fcda9-205">Come specificare i certificati client</span><span class="sxs-lookup"><span data-stu-id="fcda9-205">How to specify client certificates</span></span>

<span data-ttu-id="fcda9-206">Per aggiungere certificati client, utilizzare il `AddClientCertificate` metodo sull'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="fcda9-206">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="fcda9-207">Come creare il proxy hub</span><span class="sxs-lookup"><span data-stu-id="fcda9-207">How to create the Hub proxy</span></span>

<span data-ttu-id="fcda9-208">Per definire i metodi sul client che un hub può chiamare dal server e richiamare i metodi su `CreateHubProxy` un hub nel server, creare un proxy per l'hub chiamando sull'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="fcda9-208">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="fcda9-209">La stringa passata `CreateHubProxy` a è il nome della classe Hub `HubName` o il nome specificato dall'attributo, se ne è stato utilizzato uno sul server.</span><span class="sxs-lookup"><span data-stu-id="fcda9-209">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="fcda9-210">La corrispondenza dei nomi non prevede alcuna distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="fcda9-210">Name matching is case-insensitive.</span></span>

<span data-ttu-id="fcda9-211">**Classe Hub sul server**</span><span class="sxs-lookup"><span data-stu-id="fcda9-211">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="fcda9-212">**Creare il proxy client per la classe HubCreate client proxy for the Hub class**</span><span class="sxs-lookup"><span data-stu-id="fcda9-212">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=3)]

<span data-ttu-id="fcda9-213">Se decori la classe `HubName` Hub con un attributo, usa quel nome.</span><span class="sxs-lookup"><span data-stu-id="fcda9-213">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="fcda9-214">**Classe Hub sul server**</span><span class="sxs-lookup"><span data-stu-id="fcda9-214">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="fcda9-215">**Creare il proxy client per la classe HubCreate client proxy for the Hub class**</span><span class="sxs-lookup"><span data-stu-id="fcda9-215">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=3)]

<span data-ttu-id="fcda9-216">Se si `HubConnection.CreateHubProxy` chiama più `hubName`volte con lo `IHubProxy` stesso , si ottiene lo stesso oggetto memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="fcda9-216">If you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="fcda9-217">Come definire i metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="fcda9-217">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="fcda9-218">Per definire un metodo che il server può `On` chiamare, utilizzare il metodo del proxy per registrare un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="fcda9-218">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="fcda9-219">La corrispondenza del nome del metodo non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="fcda9-219">Method name matching is case-insensitive.</span></span> <span data-ttu-id="fcda9-220">Ad `Clients.All.UpdateStockPrice` esempio, sul server `updateStockPrice` `updatestockprice`verrà `UpdateStockPrice` eseguito , o sul client.</span><span class="sxs-lookup"><span data-stu-id="fcda9-220">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="fcda9-221">Piattaforme client diverse hanno requisiti diversi per la modalità di scrittura del codice del metodo per aggiornare l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="fcda9-221">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="fcda9-222">Gli esempi illustrati sono per i client WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="fcda9-222">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="fcda9-223">Wpf, Silverlight e esempi di applicazioni console sono disponibili in [una sezione separata più avanti in questo argomento.](#wpfsl)</span><span class="sxs-lookup"><span data-stu-id="fcda9-223">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="fcda9-224">Metodi senza parametri</span><span class="sxs-lookup"><span data-stu-id="fcda9-224">Methods without parameters</span></span>

<span data-ttu-id="fcda9-225">Se il metodo che si sta gestendo non dispone di `On` parametri, utilizzare l'overload non generico del metodo:</span><span class="sxs-lookup"><span data-stu-id="fcda9-225">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="fcda9-226">**Codice server che chiama il metodo client senza parametriServer code calling client method without parameters**</span><span class="sxs-lookup"><span data-stu-id="fcda9-226">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="fcda9-227">**Codice client WinRT per il metodo chiamato dal server senza parametri[(vedere esempi wpf e Silverlight più avanti in questo argomento](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="fcda9-227">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="fcda9-228">Metodi con parametri, che specificano i tipi di parametro</span><span class="sxs-lookup"><span data-stu-id="fcda9-228">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="fcda9-229">Se il metodo che si sta gestendo dispone di parametri, `On` specificare i tipi dei parametri come tipi generici del metodo.</span><span class="sxs-lookup"><span data-stu-id="fcda9-229">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="fcda9-230">Esistono overload generici `On` del metodo per consentire di specificare fino a 8 parametri (4 in Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="fcda9-230">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="fcda9-231">Nell'esempio seguente, un parametro `UpdateStockPrice` viene inviato al metodo.</span><span class="sxs-lookup"><span data-stu-id="fcda9-231">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="fcda9-232">**Codice server che chiama il metodo client con un parametroServer code calling client method with a parameter**</span><span class="sxs-lookup"><span data-stu-id="fcda9-232">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="fcda9-233">**La classe Stock utilizzata per il parametro**</span><span class="sxs-lookup"><span data-stu-id="fcda9-233">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="fcda9-234">**Codice client WinRT per un metodo chiamato dal server con un parametro ([vedere esempi wpf e Silverlight più avanti in questo argomento](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="fcda9-234">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="fcda9-235">Metodi con parametri, specifica di oggetti dinamici per i parametri</span><span class="sxs-lookup"><span data-stu-id="fcda9-235">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="fcda9-236">In alternativa alla specifica dei parametri `On` come tipi generici del metodo, è possibile specificare i parametri come oggetti dinamici:</span><span class="sxs-lookup"><span data-stu-id="fcda9-236">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="fcda9-237">**Codice server che chiama il metodo client con un parametroServer code calling client method with a parameter**</span><span class="sxs-lookup"><span data-stu-id="fcda9-237">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="fcda9-238">**La classe Stock utilizzata per il parametro**</span><span class="sxs-lookup"><span data-stu-id="fcda9-238">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="fcda9-239">**Codice client WinRT per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro[(vedere esempi WPF e Silverlight più avanti in questo argomento](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="fcda9-239">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="fcda9-240">Come rimuovere un gestore</span><span class="sxs-lookup"><span data-stu-id="fcda9-240">How to remove a handler</span></span>

<span data-ttu-id="fcda9-241">Per rimuovere un gestore, chiamare il relativo `Dispose` metodo.</span><span class="sxs-lookup"><span data-stu-id="fcda9-241">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="fcda9-242">**Codice client per un metodo chiamato dal server**</span><span class="sxs-lookup"><span data-stu-id="fcda9-242">**Client code for a method called from server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="fcda9-243">**Codice client per rimuovere il gestore**</span><span class="sxs-lookup"><span data-stu-id="fcda9-243">**Client code to remove the handler**</span></span>

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="fcda9-244">Come chiamare i metodi server dal client</span><span class="sxs-lookup"><span data-stu-id="fcda9-244">How to call server methods from the client</span></span>

<span data-ttu-id="fcda9-245">Per chiamare un metodo sul `Invoke` server, utilizzare il metodo sul proxy Hub.</span><span class="sxs-lookup"><span data-stu-id="fcda9-245">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="fcda9-246">Se il metodo server non ha alcun valore restituito, utilizzare l'overload non generico del `Invoke` metodo.</span><span class="sxs-lookup"><span data-stu-id="fcda9-246">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="fcda9-247">**Codice server per un metodo senza valore restituito**</span><span class="sxs-lookup"><span data-stu-id="fcda9-247">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="fcda9-248">**Codice client che chiama un metodo che non ha alcun valore restituito**</span><span class="sxs-lookup"><span data-stu-id="fcda9-248">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="fcda9-249">Se il metodo server ha un valore restituito, specificare `Invoke` il tipo restituito come tipo generico del metodo.</span><span class="sxs-lookup"><span data-stu-id="fcda9-249">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="fcda9-250">**Codice server per un metodo che ha un valore restituito e accetta un parametro di tipo complessoServer code for a method that has a return value and takes a complex type parameter**</span><span class="sxs-lookup"><span data-stu-id="fcda9-250">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="fcda9-251">**La classe Stock utilizzata per il parametro e il valore restituito**</span><span class="sxs-lookup"><span data-stu-id="fcda9-251">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="fcda9-252">**Codice client che chiama un metodo che ha un valore restituito e accetta un parametro di tipo complesso, in un ASP.NET 4.5 metodo asincrono**</span><span class="sxs-lookup"><span data-stu-id="fcda9-252">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="fcda9-253">**Codice client che chiama un metodo che ha un valore restituito e accetta un parametro di tipo complesso, in un metodo sincrono**</span><span class="sxs-lookup"><span data-stu-id="fcda9-253">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="fcda9-254">Il `Invoke` metodo viene eseguito `Task` in modo asincrono e restituisce un oggetto.</span><span class="sxs-lookup"><span data-stu-id="fcda9-254">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="fcda9-255">Se non si `await` specifica `.Wait()`o , la riga di codice successiva verrà eseguita prima che il metodo richiamato abbia terminato l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="fcda9-255">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="fcda9-256">Come gestire gli eventi di durata della connessioneHow to handle connection lifetime events</span><span class="sxs-lookup"><span data-stu-id="fcda9-256">How to handle connection lifetime events</span></span>

<span data-ttu-id="fcda9-257">SignalR fornisce i seguenti eventi di durata della connessione che è possibile gestire:</span><span class="sxs-lookup"><span data-stu-id="fcda9-257">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="fcda9-258">`Received`: generato quando vengono ricevuti dati sulla connessione.</span><span class="sxs-lookup"><span data-stu-id="fcda9-258">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="fcda9-259">Fornisce i dati ricevuti.</span><span class="sxs-lookup"><span data-stu-id="fcda9-259">Provides the received data.</span></span>
- <span data-ttu-id="fcda9-260">`ConnectionSlow`: generato quando il client rileva una connessione lenta o frequente.</span><span class="sxs-lookup"><span data-stu-id="fcda9-260">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="fcda9-261">`Reconnecting`: generato quando il trasporto sottostante inizia la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="fcda9-261">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="fcda9-262">`Reconnected`: generato quando il trasporto sottostante è stato riconnesso.</span><span class="sxs-lookup"><span data-stu-id="fcda9-262">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="fcda9-263">`StateChanged`: generato quando cambia lo stato della connessione.</span><span class="sxs-lookup"><span data-stu-id="fcda9-263">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="fcda9-264">Fornisce lo stato precedente e il nuovo stato.</span><span class="sxs-lookup"><span data-stu-id="fcda9-264">Provides the old state and the new state.</span></span> <span data-ttu-id="fcda9-265">Per informazioni sui valori dello stato di connessione, vedere [ConnectionState Enumerazione](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="fcda9-265">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="fcda9-266">`Closed`: generato quando la connessione si è disconnessa.</span><span class="sxs-lookup"><span data-stu-id="fcda9-266">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="fcda9-267">Ad esempio, se si desidera visualizzare messaggi di avviso per gli errori che non sono irreversibili ma `ConnectionSlow` causano problemi di connessione intermittenti, ad esempio lentezza o frequente eliminazione della connessione, gestire l'evento.</span><span class="sxs-lookup"><span data-stu-id="fcda9-267">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="fcda9-268">Per ulteriori informazioni, vedere Informazioni e gestione degli eventi di durata della [connessione in SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="fcda9-268">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="fcda9-269">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="fcda9-269">How to handle errors</span></span>

<span data-ttu-id="fcda9-270">Se non si abilitano in modo esplicito i messaggi di errore dettagliati sul server, l'oggetto eccezione restituito da SignalR dopo che un errore contiene informazioni minime sull'errore.</span><span class="sxs-lookup"><span data-stu-id="fcda9-270">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="fcda9-271">Ad esempio, se `newContosoChatMessage` una chiamata a ha esito`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`negativo, il messaggio di errore nell'oggetto di errore contiene " " L'invio di messaggi di errore dettagliati ai client nell'ambiente di produzione non è consigliato per motivi di sicurezza, ma se si desidera abilitare i messaggi di errore dettagliati per la risoluzione dei problemi, utilizzare il codice seguente sul server.</span><span class="sxs-lookup"><span data-stu-id="fcda9-271">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="fcda9-272">Per gestire gli errori che SignalR genera, `Error` è possibile aggiungere un gestore per l'evento nell'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="fcda9-272">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="fcda9-273">Per gestire gli errori dalle chiamate al metodo, eseguire il wrapping del codice in un blocco try-catch.</span><span class="sxs-lookup"><span data-stu-id="fcda9-273">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="fcda9-274">Come abilitare la registrazione lato clientHow to enable client-side logging</span><span class="sxs-lookup"><span data-stu-id="fcda9-274">How to enable client-side logging</span></span>

<span data-ttu-id="fcda9-275">Per abilitare la registrazione `TraceLevel` sul `TraceWriter` lato client, impostare le proprietà e sull'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="fcda9-275">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=3-4)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="fcda9-276">Esempi di codice WPF, Silverlight e applicazione console per i metodi client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="fcda9-276">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="fcda9-277">Gli esempi di codice illustrati in precedenza per la definizione dei metodi client che il server può chiamare si applicano ai client WinRT.</span><span class="sxs-lookup"><span data-stu-id="fcda9-277">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="fcda9-278">Negli esempi seguenti viene illustrato il codice equivalente per WPF, Silverlight e i client dell'applicazione console.</span><span class="sxs-lookup"><span data-stu-id="fcda9-278">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="fcda9-279">Metodi senza parametri</span><span class="sxs-lookup"><span data-stu-id="fcda9-279">Methods without parameters</span></span>

<span data-ttu-id="fcda9-280">**Codice client WPF per il metodo chiamato dal server senza parametriWPF client code for method called from server without parameters**</span><span class="sxs-lookup"><span data-stu-id="fcda9-280">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="fcda9-281">**Codice client Silverlight per il metodo chiamato dal server senza parametri**</span><span class="sxs-lookup"><span data-stu-id="fcda9-281">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="fcda9-282">**Codice client dell'applicazione console per il metodo chiamato dal server senza parametri**</span><span class="sxs-lookup"><span data-stu-id="fcda9-282">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="fcda9-283">Metodi con parametri, che specificano i tipi di parametro</span><span class="sxs-lookup"><span data-stu-id="fcda9-283">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="fcda9-284">**Codice client WPF per un metodo chiamato dal server con un parametroWPF client code for a method called from server with a parameter**</span><span class="sxs-lookup"><span data-stu-id="fcda9-284">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="fcda9-285">**Codice client Silverlight per un metodo chiamato dal server con un parametro**</span><span class="sxs-lookup"><span data-stu-id="fcda9-285">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="fcda9-286">**Codice client dell'applicazione console per un metodo chiamato dal server con un parametro**</span><span class="sxs-lookup"><span data-stu-id="fcda9-286">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="fcda9-287">Metodi con parametri, specifica di oggetti dinamici per i parametri</span><span class="sxs-lookup"><span data-stu-id="fcda9-287">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="fcda9-288">**Codice client WPF per un metodo chiamato dal server con un parametro, usando un oggetto dinamico per il parametroWPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span><span class="sxs-lookup"><span data-stu-id="fcda9-288">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="fcda9-289">**Codice client Silverlight per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro**</span><span class="sxs-lookup"><span data-stu-id="fcda9-289">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="fcda9-290">**Codice client dell'applicazione console per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro**</span><span class="sxs-lookup"><span data-stu-id="fcda9-290">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
