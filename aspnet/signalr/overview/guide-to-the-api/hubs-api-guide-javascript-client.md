---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: Guida all'API di ASP.NET SignalR Hubs - Client JavaScript Documenti Microsoft
author: bradygaster
description: Questo documento fornisce un'introduzione all'uso dell'API Hubs per SignalR versione 2 nei client JavaScript, ad esempio browser e applicato Windows Store (WinJS)...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 8befe133c3627dac1f7d011959c68e2054d345da
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676083"
---
# <a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="39a60-103">Guida all'API di ASP.NET SignalR Hubs - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="39a60-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="39a60-104">Questo documento fornisce un'introduzione all'uso dell'API Hubs per SignalR versione 2 nei client JavaScript, ad esempio browser e applicazioni Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="39a60-104">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
>
> <span data-ttu-id="39a60-105">L'API Hubs SignalR consente di effettuare chiamate di procedura remota (RPC) da un server ai client connessi e dai client al server.</span><span class="sxs-lookup"><span data-stu-id="39a60-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="39a60-106">Nel codice server, si definiscono i metodi che possono essere chiamati dai client e si chiamano i metodi eseguiti sul client.</span><span class="sxs-lookup"><span data-stu-id="39a60-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="39a60-107">Nel codice client si definiscono i metodi che possono essere chiamati dal server e si chiamano i metodi eseguiti sul server.</span><span class="sxs-lookup"><span data-stu-id="39a60-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="39a60-108">SignalR si occupa di tutto l'impianto idraulico client-server per voi.</span><span class="sxs-lookup"><span data-stu-id="39a60-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="39a60-109">SignalR offre anche un'API di livello inferiore denominata connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="39a60-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="39a60-110">Per un'introduzione a SignalR, Hub e connessioni permanenti, vedere [Introduzione a SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="39a60-110">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="39a60-111">Versioni software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="39a60-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="39a60-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="39a60-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="39a60-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="39a60-113">.NET 4.5</span></span>
> - <span data-ttu-id="39a60-114">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="39a60-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="39a60-115">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="39a60-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="39a60-116">Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR Older Versions](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="39a60-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="39a60-117">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="39a60-117">Questions and comments</span></span>
>
> <span data-ttu-id="39a60-118">Si prega di lasciare un feedback su come ti è piaciuto questo tutorial e cosa potremmo migliorare nei commenti in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="39a60-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="39a60-119">Se avete domande che non sono direttamente correlati al tutorial, è possibile pubblicarli al [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="39a60-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="39a60-120">Panoramica</span><span class="sxs-lookup"><span data-stu-id="39a60-120">Overview</span></span>

<span data-ttu-id="39a60-121">Questo documento contiene le seguenti sezioni:</span><span class="sxs-lookup"><span data-stu-id="39a60-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="39a60-122">Il proxy generato e quello che fa per voi</span><span class="sxs-lookup"><span data-stu-id="39a60-122">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="39a60-123">Quando utilizzare il proxy generato</span><span class="sxs-lookup"><span data-stu-id="39a60-123">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="39a60-124">Configurazione client</span><span class="sxs-lookup"><span data-stu-id="39a60-124">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="39a60-125">Come fare riferimento al proxy generato dinamicamente</span><span class="sxs-lookup"><span data-stu-id="39a60-125">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="39a60-126">Come creare un file fisico per il proxy generato SignalR</span><span class="sxs-lookup"><span data-stu-id="39a60-126">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="39a60-127">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="39a60-127">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="39a60-128">L'oggetto che è stato creato da .connection.hub</span><span class="sxs-lookup"><span data-stu-id="39a60-128">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="39a60-129">Esecuzione asincrona del metodo start</span><span class="sxs-lookup"><span data-stu-id="39a60-129">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="39a60-130">Come stabilire una connessione tra domini</span><span class="sxs-lookup"><span data-stu-id="39a60-130">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="39a60-131">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="39a60-131">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="39a60-132">Come specificare i parametri della stringa di queryHow to specify query string parameters</span><span class="sxs-lookup"><span data-stu-id="39a60-132">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="39a60-133">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="39a60-133">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="39a60-134">Come ottenere un proxy per una classe HubHow to get a proxy for a Hub class</span><span class="sxs-lookup"><span data-stu-id="39a60-134">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="39a60-135">Come definire i metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="39a60-135">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="39a60-136">Come chiamare i metodi server dal client</span><span class="sxs-lookup"><span data-stu-id="39a60-136">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="39a60-137">Come gestire gli eventi di durata della connessioneHow to handle connection lifetime events</span><span class="sxs-lookup"><span data-stu-id="39a60-137">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="39a60-138">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="39a60-138">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="39a60-139">Come abilitare la registrazione lato clientHow to enable client-side logging</span><span class="sxs-lookup"><span data-stu-id="39a60-139">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="39a60-140">Per la documentazione su come programmare il server o i client .NET, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="39a60-140">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="39a60-141">Guida all'API di SignalR Hubs - Server</span><span class="sxs-lookup"><span data-stu-id="39a60-141">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="39a60-142">Guida all'API di SignalR Hubs - Client .NET</span><span class="sxs-lookup"><span data-stu-id="39a60-142">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="39a60-143">Il componente server SignalR 2 è disponibile solo in .NET 4.5 (anche se è presente un client .NET per SignalR 2 in .NET 4.0).</span><span class="sxs-lookup"><span data-stu-id="39a60-143">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="39a60-144">Il proxy generato e quello che fa per voi</span><span class="sxs-lookup"><span data-stu-id="39a60-144">The generated proxy and what it does for you</span></span>

<span data-ttu-id="39a60-145">È possibile programmare un client JavaScript per comunicare con un servizio SignalR con o senza un proxy generato da SignalR.</span><span class="sxs-lookup"><span data-stu-id="39a60-145">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="39a60-146">Il proxy fa per te è semplificare la sintassi del codice utilizzato per connettersi, scrivere i metodi che il server chiama e chiamare i metodi sul server.</span><span class="sxs-lookup"><span data-stu-id="39a60-146">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="39a60-147">Quando si scrive codice per chiamare i metodi del server, il proxy generato consente di utilizzare `serverMethod(arg1, arg2)` una `invoke('serverMethod', arg1, arg2)`sintassi simile a quella di una funzione locale: è possibile scrivere anziché .</span><span class="sxs-lookup"><span data-stu-id="39a60-147">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="39a60-148">La sintassi del proxy generata consente inoltre un errore sul lato client immediato e comprensibile se si digita in modo errato il nome di un metodo server.</span><span class="sxs-lookup"><span data-stu-id="39a60-148">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="39a60-149">E se si crea manualmente il file che definisce i proxy, è anche possibile ottenere il supporto IntelliSense per la scrittura di codice che chiama i metodi server.</span><span class="sxs-lookup"><span data-stu-id="39a60-149">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="39a60-150">Si supponga, ad esempio, di avere la seguente classe Hub sul server:</span><span class="sxs-lookup"><span data-stu-id="39a60-150">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="39a60-151">Negli esempi di codice seguenti viene illustrato l'aspetto del codice JavaScript per richiamare il `NewContosoChatMessage` metodo sul server e ricevere chiamate del `addContosoChatMessageToPage` metodo dal server.</span><span class="sxs-lookup"><span data-stu-id="39a60-151">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="39a60-152">**Con il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="39a60-152">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="39a60-153">**Senza il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="39a60-153">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="39a60-154">Quando utilizzare il proxy generato</span><span class="sxs-lookup"><span data-stu-id="39a60-154">When to use the generated proxy</span></span>

<span data-ttu-id="39a60-155">Se si desidera registrare più gestori eventi per un metodo client chiamato dal server, non è possibile utilizzare il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="39a60-155">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="39a60-156">In caso contrario, è possibile scegliere di utilizzare il proxy generato o meno in base alle preferenze di codifica.</span><span class="sxs-lookup"><span data-stu-id="39a60-156">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="39a60-157">Se si sceglie di non utilizzarlo, non è necessario fare riferimento all'URL "signalr/hubs" in un `script` elemento nel codice client.</span><span class="sxs-lookup"><span data-stu-id="39a60-157">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="39a60-158">Configurazione client</span><span class="sxs-lookup"><span data-stu-id="39a60-158">Client setup</span></span>

<span data-ttu-id="39a60-159">Un client JavaScript richiede riferimenti a jQuery e al file JavaScript di base DiR.</span><span class="sxs-lookup"><span data-stu-id="39a60-159">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="39a60-160">La versione jQuery deve essere 1.6.4 o versioni successive principali, ad esempio 1.7.2, 1.8.2 o 1.9.1.The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2 or 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="39a60-160">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="39a60-161">Se si decide di utilizzare il proxy generato, è necessario anche un riferimento al file JavaScript del proxy generato da SignalR.</span><span class="sxs-lookup"><span data-stu-id="39a60-161">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="39a60-162">Nell'esempio seguente viene illustrato l'aspetto dei riferimenti in una pagina HTML che utilizza il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="39a60-162">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="39a60-163">Questi riferimenti devono essere inclusi in questo ordine: jQuery first, SignalR core dopo che, e SignalR proxy per ultimo.</span><span class="sxs-lookup"><span data-stu-id="39a60-163">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="39a60-164">Come fare riferimento al proxy generato dinamicamente</span><span class="sxs-lookup"><span data-stu-id="39a60-164">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="39a60-165">Nell'esempio precedente, il riferimento al proxy generato SignalR è al codice JavaScript generato dinamicamente, non a un file fisico.</span><span class="sxs-lookup"><span data-stu-id="39a60-165">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="39a60-166">SignalR crea il codice JavaScript per il proxy in tempo reale e lo serve al client in risposta all'URL "/signalr/hubs".</span><span class="sxs-lookup"><span data-stu-id="39a60-166">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="39a60-167">Se è stato specificato un URL di base `MapSignalR` diverso per le connessioni SignalR sul server nel metodo, l'URL per il file proxy generato dinamicamente è l'URL personalizzato con "/hubs" aggiunto.</span><span class="sxs-lookup"><span data-stu-id="39a60-167">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="39a60-168">Per i client JavaScript di Windows 8 (Windows Store), usa il file proxy fisico invece di quello generato dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="39a60-168">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="39a60-169">Per ulteriori informazioni, vedere [Come creare un file fisico per il proxy generato SignalR](#manualproxy) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="39a60-169">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>

<span data-ttu-id="39a60-170">In una ASP.NET visualizzazione Razor MVC 4 o 5, utilizzare la tilde per fare riferimento alla radice dell'applicazione nel riferimento al file proxy:In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span><span class="sxs-lookup"><span data-stu-id="39a60-170">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="39a60-171">Per ulteriori informazioni sull'utilizzo di SignalR in MVC 5, vedere [Introduzione a SignalR e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="39a60-171">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="39a60-172">In una ASP.NET visualizzazione Razor `Url.Content` MVC 3, utilizzare per il riferimento al file proxy:</span><span class="sxs-lookup"><span data-stu-id="39a60-172">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="39a60-173">In un'applicazione Web `ResolveClientUrl` Form ASP.NET, utilizzare per il riferimento al file proxy o registrarlo tramite ScriptManager utilizzando un percorso relativo radice dell'app (a partire da una tilde):</span><span class="sxs-lookup"><span data-stu-id="39a60-173">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="39a60-174">Come regola generale, utilizzare lo stesso metodo per specificare l'URL "/signalr/hubs" utilizzato per i file CSS o JavaScript.</span><span class="sxs-lookup"><span data-stu-id="39a60-174">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="39a60-175">Se si specifica un URL senza utilizzare una tilde, in alcuni scenari l'applicazione funzionerà correttamente quando si esegue il test in Visual Studio utilizzando IIS Express, ma avrà esito negativo con un errore 404 quando si esegue la distribuzione in IIS completo.</span><span class="sxs-lookup"><span data-stu-id="39a60-175">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="39a60-176">Per ulteriori informazioni, vedere **Risoluzione dei riferimenti alle risorse a livello radice** nei server Web in Visual Studio per ASP.NET progetti [Web](https://msdn.microsoft.com/library/58wxa9w5.aspx) nel sito MSDN.</span><span class="sxs-lookup"><span data-stu-id="39a60-176">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="39a60-177">Quando si esegue un progetto web in Visual Studio 2017 in modalità di debug e se si utilizza Internet Explorer come browser, è possibile visualizzare il file proxy in **Esplora soluzioni** in **Script**.</span><span class="sxs-lookup"><span data-stu-id="39a60-177">When you run a web project in Visual Studio 2017 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Scripts**.</span></span>

<span data-ttu-id="39a60-178">Per visualizzare il contenuto del file, fare doppio clic su **hub.**</span><span class="sxs-lookup"><span data-stu-id="39a60-178">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="39a60-179">Se non si utilizza Visual Studio 2012 o 2013 e Internet Explorer o se non si è in modalità di debug, è anche possibile ottenere il contenuto del file individuando l'URL "/signalR/hubs".</span><span class="sxs-lookup"><span data-stu-id="39a60-179">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="39a60-180">Ad esempio, se il `http://localhost:56699`sito è `http://localhost:56699/SignalR/hubs` in esecuzione in , vai a nel browser.</span><span class="sxs-lookup"><span data-stu-id="39a60-180">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="39a60-181">Come creare un file fisico per il proxy generato SignalR</span><span class="sxs-lookup"><span data-stu-id="39a60-181">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="39a60-182">In alternativa al proxy generato dinamicamente, è possibile creare un file fisico con il codice proxy e fare riferimento a tale file.</span><span class="sxs-lookup"><span data-stu-id="39a60-182">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="39a60-183">È possibile eseguire questa operazione per il controllo sul comportamento di memorizzazione nella cache o bundling o per ottenere IntelliSense durante la codifica di chiamate ai metodi server.</span><span class="sxs-lookup"><span data-stu-id="39a60-183">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="39a60-184">Per creare un file proxy, attenersi alla seguente procedura:</span><span class="sxs-lookup"><span data-stu-id="39a60-184">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="39a60-185">Installare il pacchetto [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet.</span><span class="sxs-lookup"><span data-stu-id="39a60-185">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="39a60-186">Aprire un prompt dei comandi e individuare la cartella *degli strumenti* che contiene il file SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="39a60-186">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="39a60-187">La cartella degli strumenti si trova nel seguente percorso:</span><span class="sxs-lookup"><span data-stu-id="39a60-187">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="39a60-188">Immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="39a60-188">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="39a60-189">Il percorso del *file DLL* è in genere la cartella *bin* nella cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="39a60-189">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="39a60-190">Questo comando crea un file denominato *server.js* nella stessa cartella di *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="39a60-190">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="39a60-191">Inserire il file *server.js* in una cartella appropriata nel progetto, rinominarlo come appropriato per l'applicazione e aggiungere un riferimento a esso al posto del riferimento "signalr/hubs".</span><span class="sxs-lookup"><span data-stu-id="39a60-191">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="39a60-192">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="39a60-192">How to establish a connection</span></span>

<span data-ttu-id="39a60-193">Prima di poter stabilire una connessione, è necessario creare un oggetto connessione, creare un proxy e registrare i gestori eventi per i metodi che possono essere chiamati dal server.</span><span class="sxs-lookup"><span data-stu-id="39a60-193">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="39a60-194">Quando il proxy e i gestori eventi sono `start` impostati, stabilire la connessione chiamando il metodo .</span><span class="sxs-lookup"><span data-stu-id="39a60-194">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="39a60-195">Se si utilizza il proxy generato, non è necessario creare l'oggetto connessione nel codice perché il codice proxy generato esegue automaticamente l'oggetto collegamento.</span><span class="sxs-lookup"><span data-stu-id="39a60-195">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="39a60-196">**Stabilire una connessione (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-196">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="39a60-197">**Stabilire una connessione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-197">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="39a60-198">Il codice di esempio usa l'URL predefinito "/signalr" per connettersi al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="39a60-198">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="39a60-199">Per informazioni su come specificare un URL di base diverso, vedere [Guida all'API di ASP.NET SignalR Hubs - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="39a60-199">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="39a60-200">Per impostazione predefinita, il percorso dell'hub è il server corrente; se ci si connette a un server diverso, specificare l'URL prima di chiamare il `start` metodo, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="39a60-200">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="39a60-201">In genere si registrano `start` i gestori eventi prima di chiamare il metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="39a60-201">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="39a60-202">Se si desidera registrare alcuni gestori eventi dopo aver stabilito la connessione, è possibile farlo, ma è `start` necessario registrare almeno uno dei gestori eventi prima di chiamare il metodo.</span><span class="sxs-lookup"><span data-stu-id="39a60-202">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="39a60-203">Uno dei motivi è che ci possono essere molti hub in un'applicazione, ma non si desidera attivare l'evento `OnConnected` su ogni Hub se si intende utilizzare solo a uno di essi.</span><span class="sxs-lookup"><span data-stu-id="39a60-203">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="39a60-204">Quando viene stabilita la connessione, la presenza di un metodo client sul `OnConnected` proxy di un hub è ciò che indica a SignalR di attivare l'evento.</span><span class="sxs-lookup"><span data-stu-id="39a60-204">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="39a60-205">Se non si registra alcun gestore `start` eventi prima di chiamare il metodo, sarà possibile richiamare i metodi sull'hub, ma il metodo dell'hub `OnConnected` non verrà chiamato e non verranno richiamati metodi client dal server.</span><span class="sxs-lookup"><span data-stu-id="39a60-205">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="39a60-206">L'oggetto che è stato creato da .connection.hub</span><span class="sxs-lookup"><span data-stu-id="39a60-206">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="39a60-207">Come si può vedere dagli esempi, quando `$.connection.hub` si utilizza il proxy generato, fa riferimento all'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="39a60-207">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="39a60-208">Si tratta dello stesso oggetto `$.hubConnection()` che si ottiene chiamando quando non si utilizza il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="39a60-208">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="39a60-209">Il codice proxy generato crea automaticamente la connessione eseguendo l'istruzione seguente:</span><span class="sxs-lookup"><span data-stu-id="39a60-209">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Creazione di una connessione nel file proxy generato](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="39a60-211">Quando si utilizza il proxy generato, è `$.connection.hub` possibile eseguire qualsiasi operazione con un oggetto connessione quando non si utilizza il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="39a60-211">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="39a60-212">Esecuzione asincrona del metodo start</span><span class="sxs-lookup"><span data-stu-id="39a60-212">Asynchronous execution of the start method</span></span>

<span data-ttu-id="39a60-213">Il `start` metodo viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="39a60-213">The `start` method executes asynchronously.</span></span> <span data-ttu-id="39a60-214">Restituisce un [oggetto jQuery Deferred](http://api.jquery.com/category/deferred-object/), il che significa che `pipe` `done`è `fail`possibile aggiungere funzioni di callback chiamando metodi quali , e .</span><span class="sxs-lookup"><span data-stu-id="39a60-214">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="39a60-215">Se si desidera eseguire il codice dopo aver stabilito la connessione, ad esempio una chiamata a un metodo server, inserire tale codice in una funzione di callback o chiamarlo da una funzione di callback.</span><span class="sxs-lookup"><span data-stu-id="39a60-215">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="39a60-216">Il `.done` metodo di callback viene eseguito al termine dell'esecuzione della `OnConnected` connessione e dopo il completamento dell'esecuzione del codice nel metodo del gestore eventi sul server.</span><span class="sxs-lookup"><span data-stu-id="39a60-216">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="39a60-217">Se si inserisce l'istruzione "Now connected" dell'esempio precedente `start` come riga di `.done` codice `console.log` successiva dopo la chiamata al metodo (non in un callback), la riga verrà eseguita prima che venga stabilita la connessione, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="39a60-217">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Modo errato per scrivere codice che viene eseguito dopo la connessione è stata stabilita](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="39a60-219">Come stabilire una connessione tra domini</span><span class="sxs-lookup"><span data-stu-id="39a60-219">How to establish a cross-domain connection</span></span>

<span data-ttu-id="39a60-220">In genere se il `http://contoso.com`browser carica una pagina da , `http://contoso.com/signalr`la connessione SignalR si trova nello stesso dominio, in .</span><span class="sxs-lookup"><span data-stu-id="39a60-220">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="39a60-221">Se la `http://contoso.com` pagina da `http://fabrikam.com/signalr`effettua una connessione a , si tratta di una connessione tra domini.</span><span class="sxs-lookup"><span data-stu-id="39a60-221">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="39a60-222">Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="39a60-222">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="39a60-223">In SignalR 1.x, le richieste tra domini erano controllate da un singolo flag EnableCrossDomain.</span><span class="sxs-lookup"><span data-stu-id="39a60-223">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="39a60-224">Questo flag controllava le richieste JSONP e CORS.</span><span class="sxs-lookup"><span data-stu-id="39a60-224">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="39a60-225">Per una maggiore flessibilità, tutto il supporto CORS è stato rimosso dal componente server di SignalR (i client JavaScript usano ancora CORS normalmente se viene rilevato che il browser lo supporta) e il nuovo middleware OWIN è stato reso disponibile per supportare questi scenari.</span><span class="sxs-lookup"><span data-stu-id="39a60-225">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="39a60-226">Se JSONP è necessario nel client (per supportare le richieste tra domini nei `EnableJSONP` browser `HubConfiguration` meno `true`recenti), dovrà essere abilitato in modo esplicito impostando l'oggetto su , come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="39a60-226">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="39a60-227">JSONP è disabilitato per impostazione predefinita, in quanto è meno sicuro di CORS.</span><span class="sxs-lookup"><span data-stu-id="39a60-227">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="39a60-228">**Aggiunta di Microsoft.Owin.Cors al progetto:** Per installare questa libreria, eseguire il comando seguente nella Console di gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="39a60-228">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="39a60-229">Questo comando aggiungerà la versione 2.1.0 del pacchetto al progetto.</span><span class="sxs-lookup"><span data-stu-id="39a60-229">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="39a60-230">Chiamata di UseCorsCalling UseCors</span><span class="sxs-lookup"><span data-stu-id="39a60-230">Calling UseCors</span></span>

 <span data-ttu-id="39a60-231">Il frammento di codice seguente illustra come implementare connessioni tra domini in SignalR 2.The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="39a60-231">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span>

<span data-ttu-id="39a60-232">**Implementazione di richieste tra domini in SignalR 2Implementing cross-domain requests in SignalR 2**</span><span class="sxs-lookup"><span data-stu-id="39a60-232">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="39a60-233">Il codice seguente illustra come abilitare CORS o JSONP in un progetto SignalR 2.The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span><span class="sxs-lookup"><span data-stu-id="39a60-233">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="39a60-234">In questo `Map` esempio `RunSignalR` di `MapSignalR`codice viene utilizzato e anziché , in modo che il middleware CORS venga eseguito solo per le richieste SignalR che richiedono il supporto CORS (anziché per tutto il traffico nel percorso specificato in `MapSignalR`.) Map può essere utilizzato anche per qualsiasi altro middleware che deve essere eseguito per un prefisso URL specifico, anziché per l'intera applicazione.</span><span class="sxs-lookup"><span data-stu-id="39a60-234">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - <span data-ttu-id="39a60-235">Non impostare `jQuery.support.cors` su true nel codice.</span><span class="sxs-lookup"><span data-stu-id="39a60-235">Don't set `jQuery.support.cors` to true in your code.</span></span>
>
>     ![Non impostare jQuery.support.cors su true](hubs-api-guide-javascript-client/_static/image7.png)
>
>     <span data-ttu-id="39a60-237">SignalR gestisce l'uso di CORS.</span><span class="sxs-lookup"><span data-stu-id="39a60-237">SignalR handles the use of CORS.</span></span> <span data-ttu-id="39a60-238">L'impostazione `jQuery.support.cors` su true disabilita JSONP perché fa sì che SignalR presupponga che il browser supporti CORS.</span><span class="sxs-lookup"><span data-stu-id="39a60-238">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="39a60-239">Quando ti connetti a un URL localhost, Internet Explorer 10 non lo considererà una connessione tra domini, quindi l'applicazione funzionerà localmente con Internet Explorer 10 anche se non hai abilitato le connessioni tra domini sul server.</span><span class="sxs-lookup"><span data-stu-id="39a60-239">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="39a60-240">Per informazioni sull'utilizzo di connessioni tra domini con Internet Explorer 9, vedere [questo thread StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="39a60-240">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="39a60-241">Per informazioni sull'utilizzo di connessioni tra domini con Chrome, consultate [questo thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="39a60-241">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="39a60-242">Il codice di esempio usa l'URL predefinito "/signalr" per connettersi al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="39a60-242">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="39a60-243">Per informazioni su come specificare un URL di base diverso, vedere [Guida all'API di ASP.NET SignalR Hubs - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="39a60-243">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="39a60-244">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="39a60-244">How to configure the connection</span></span>

<span data-ttu-id="39a60-245">Prima di stabilire una connessione, è possibile specificare i parametri della stringa di query o il metodo di trasporto.</span><span class="sxs-lookup"><span data-stu-id="39a60-245">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="39a60-246">Come specificare i parametri della stringa di queryHow to specify query string parameters</span><span class="sxs-lookup"><span data-stu-id="39a60-246">How to specify query string parameters</span></span>

<span data-ttu-id="39a60-247">Se si desidera inviare dati al server quando il client si connette, è possibile aggiungere parametri di stringa di query all'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="39a60-247">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="39a60-248">Negli esempi seguenti viene illustrato come impostare un parametro di stringa di query nel codice client.</span><span class="sxs-lookup"><span data-stu-id="39a60-248">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="39a60-249">**Impostare un valore di stringa di query prima di chiamare il metodo start (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-249">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="39a60-250">**Impostare un valore di stringa di query prima di chiamare il metodo start (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-250">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="39a60-251">Nell'esempio seguente viene illustrato come leggere un parametro di stringa di query nel codice server.</span><span class="sxs-lookup"><span data-stu-id="39a60-251">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="39a60-252">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="39a60-252">How to specify the transport method</span></span>

<span data-ttu-id="39a60-253">Come parte del processo di connessione, un client SignalR in genere negozia con il server per determinare il trasporto migliore supportato sia dal server che dal client.</span><span class="sxs-lookup"><span data-stu-id="39a60-253">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="39a60-254">Se si conosce già il trasporto che si desidera utilizzare, è possibile ignorare `start` questo processo di negoziazione specificando il metodo di trasporto quando si chiama il metodo .</span><span class="sxs-lookup"><span data-stu-id="39a60-254">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="39a60-255">**Codice client che specifica il metodo di trasporto (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-255">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="39a60-256">**Codice client che specifica il metodo di trasporto (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-256">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="39a60-257">In alternativa, è possibile specificare più metodi di trasporto nell'ordine in cui si desidera che SignalR li provi:</span><span class="sxs-lookup"><span data-stu-id="39a60-257">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="39a60-258">**Codice client che specifica uno schema di fallback del trasporto personalizzato (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-258">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="39a60-259">**Codice client che specifica uno schema di fallback del trasporto personalizzato (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-259">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="39a60-260">È possibile utilizzare i seguenti valori per specificare il metodo di trasporto:</span><span class="sxs-lookup"><span data-stu-id="39a60-260">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="39a60-261">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="39a60-261">"webSockets"</span></span>
- <span data-ttu-id="39a60-262">"ForeverFrame"</span><span class="sxs-lookup"><span data-stu-id="39a60-262">"foreverFrame"</span></span>
- <span data-ttu-id="39a60-263">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="39a60-263">"serverSentEvents"</span></span>
- <span data-ttu-id="39a60-264">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="39a60-264">"longPolling"</span></span>

<span data-ttu-id="39a60-265">Negli esempi seguenti viene illustrato come individuare il metodo di trasporto utilizzato da una connessione.</span><span class="sxs-lookup"><span data-stu-id="39a60-265">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="39a60-266">**Codice client che visualizza il metodo di trasporto utilizzato da una connessione (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-266">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="39a60-267">**Codice client che visualizza il metodo di trasporto utilizzato da una connessione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-267">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="39a60-268">Per informazioni su come controllare il metodo di trasporto nel codice server, vedere [ASP.NET Guida all'API di HubS SignalR - Server - Come ottenere informazioni sul client dalla proprietà Context](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="39a60-268">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="39a60-269">Per ulteriori informazioni sui trasporti e i fallback, vedere [Introduzione a SignalR - Trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="39a60-269">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="39a60-270">Come ottenere un proxy per una classe HubHow to get a proxy for a Hub class</span><span class="sxs-lookup"><span data-stu-id="39a60-270">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="39a60-271">Ogni oggetto connessione creato incapsula informazioni su una connessione a un servizio SignalR che contiene una o più classi Hub.</span><span class="sxs-lookup"><span data-stu-id="39a60-271">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="39a60-272">Per comunicare con una classe Hub, si usa un oggetto proxy creato dall'utente (se non si utilizza il proxy generato) o generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="39a60-272">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="39a60-273">Sul client il nome del proxy è una versione con maiuscole/minuscole camel del nome della classe Hub.</span><span class="sxs-lookup"><span data-stu-id="39a60-273">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="39a60-274">SignalR apporta automaticamente questa modifica in modo che il codice JavaScript possa essere conforme alle convenzioni JavaScript.</span><span class="sxs-lookup"><span data-stu-id="39a60-274">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="39a60-275">**Classe Hub sul server**</span><span class="sxs-lookup"><span data-stu-id="39a60-275">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="39a60-276">**Ottenere un riferimento al proxy client generato per l'hubGet a reference to the generated client proxy for the Hub**</span><span class="sxs-lookup"><span data-stu-id="39a60-276">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="39a60-277">**Creare il proxy client per la classe Hub (senza proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-277">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="39a60-278">Se decori la classe `HubName` Hub con un attributo, usa il nome esatto senza modificare la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="39a60-278">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="39a60-279">**Classe Hub nel server con attributo HubName**</span><span class="sxs-lookup"><span data-stu-id="39a60-279">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="39a60-280">**Ottenere un riferimento al proxy client generato per l'hubGet a reference to the generated client proxy for the Hub**</span><span class="sxs-lookup"><span data-stu-id="39a60-280">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="39a60-281">**Creare il proxy client per la classe Hub (senza proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-281">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="39a60-282">Come definire i metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="39a60-282">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="39a60-283">Per definire un metodo che il server può chiamare da un hub, `client` aggiungere un gestore eventi `on` al proxy Hub utilizzando la proprietà del proxy generato o chiamare il metodo se non si utilizza il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="39a60-283">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="39a60-284">I parametri possono essere oggetti complessi.</span><span class="sxs-lookup"><span data-stu-id="39a60-284">The parameters can be complex objects.</span></span>

<span data-ttu-id="39a60-285">Aggiungere il gestore eventi `start` prima di chiamare il metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="39a60-285">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="39a60-286">Se si desidera aggiungere gestori eventi `start` dopo aver chiamato il metodo, vedere la nota in [Come stabilire una connessione](#establishconnection) in precedenza in questo documento e utilizzare la sintassi illustrata per definire un metodo senza utilizzare il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="39a60-286">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="39a60-287">La corrispondenza del nome del metodo non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="39a60-287">Method name matching is case-insensitive.</span></span> <span data-ttu-id="39a60-288">Ad `Clients.All.addContosoChatMessageToPage` esempio, sul server `AddContosoChatMessageToPage` `addContosoChatMessageToPage`verrà `addcontosochatmessagetopage` eseguito , o sul client.</span><span class="sxs-lookup"><span data-stu-id="39a60-288">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="39a60-289">**Definire il metodo sul client (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-289">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="39a60-290">**Metodo alternativo per definire il metodo nel client (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-290">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="39a60-291">**Definire il metodo sul client (senza il proxy generato o quando si aggiunge dopo la chiamata al metodo start)**</span><span class="sxs-lookup"><span data-stu-id="39a60-291">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="39a60-292">**Codice server che chiama il metodo client**</span><span class="sxs-lookup"><span data-stu-id="39a60-292">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="39a60-293">Gli esempi seguenti includono un oggetto complesso come parametro del metodo.</span><span class="sxs-lookup"><span data-stu-id="39a60-293">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="39a60-294">**Metodo Define sul client che accetta un oggetto complesso (con il proxy generato)Define method on client that takes a complex object (with the generated proxy)**</span><span class="sxs-lookup"><span data-stu-id="39a60-294">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="39a60-295">**Metodo Define sul client che accetta un oggetto complesso (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-295">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="39a60-296">**Codice server che definisce l'oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="39a60-296">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="39a60-297">**Codice server che chiama il metodo client utilizzando un oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="39a60-297">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="39a60-298">Come chiamare i metodi server dal client</span><span class="sxs-lookup"><span data-stu-id="39a60-298">How to call server methods from the client</span></span>

<span data-ttu-id="39a60-299">Per chiamare un metodo server dal `server` client, utilizzare la `invoke` proprietà del proxy generato o il metodo sul proxy Hub se non si utilizza il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="39a60-299">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="39a60-300">Il valore restituito o i parametri possono essere oggetti complessi.</span><span class="sxs-lookup"><span data-stu-id="39a60-300">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="39a60-301">Passare una versione con maiuscole/minuscole del nome del metodo nell'hub.</span><span class="sxs-lookup"><span data-stu-id="39a60-301">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="39a60-302">SignalR apporta automaticamente questa modifica in modo che il codice JavaScript possa essere conforme alle convenzioni JavaScript.</span><span class="sxs-lookup"><span data-stu-id="39a60-302">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="39a60-303">Negli esempi seguenti viene illustrato come chiamare un metodo server che non dispone di un valore restituito e come chiamare un metodo server che dispone di un valore restituito.</span><span class="sxs-lookup"><span data-stu-id="39a60-303">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="39a60-304">**Metodo server senza attributo HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="39a60-304">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="39a60-305">**Codice server che definisce l'oggetto complesso passato in un parametro**</span><span class="sxs-lookup"><span data-stu-id="39a60-305">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="39a60-306">**Codice client che richiama il metodo server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-306">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="39a60-307">**Codice client che richiama il metodo server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-307">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="39a60-308">Se il metodo Hub `HubMethodName` è stato decorato con un attributo, utilizzare tale nome senza modificare la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="39a60-308">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="39a60-309">**Metodo server** con un attributo HubMethodName</span><span class="sxs-lookup"><span data-stu-id="39a60-309">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="39a60-310">**Codice client che richiama il metodo server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-310">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="39a60-311">**Codice client che richiama il metodo server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-311">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="39a60-312">Negli esempi precedenti viene illustrato come chiamare un metodo server che non ha alcun valore restituito.</span><span class="sxs-lookup"><span data-stu-id="39a60-312">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="39a60-313">Negli esempi seguenti viene illustrato come chiamare un metodo server con un valore restituito.</span><span class="sxs-lookup"><span data-stu-id="39a60-313">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="39a60-314">**Codice server per un metodo con un valore restituitoServer code for a method that has a return value**</span><span class="sxs-lookup"><span data-stu-id="39a60-314">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="39a60-315">**La classe Stock utilizzata per il** valore restituito</span><span class="sxs-lookup"><span data-stu-id="39a60-315">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="39a60-316">**Codice client che richiama il metodo server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-316">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="39a60-317">**Codice client che richiama il metodo server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-317">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="39a60-318">Come gestire gli eventi di durata della connessioneHow to handle connection lifetime events</span><span class="sxs-lookup"><span data-stu-id="39a60-318">How to handle connection lifetime events</span></span>

<span data-ttu-id="39a60-319">SignalR fornisce i seguenti eventi di durata della connessione che è possibile gestire:</span><span class="sxs-lookup"><span data-stu-id="39a60-319">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="39a60-320">`starting`: generato prima dell'invio di dati tramite la connessione.</span><span class="sxs-lookup"><span data-stu-id="39a60-320">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="39a60-321">`received`: generato quando vengono ricevuti dati sulla connessione.</span><span class="sxs-lookup"><span data-stu-id="39a60-321">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="39a60-322">Fornisce i dati ricevuti.</span><span class="sxs-lookup"><span data-stu-id="39a60-322">Provides the received data.</span></span>
- <span data-ttu-id="39a60-323">`connectionSlow`: generato quando il client rileva una connessione lenta o frequente.</span><span class="sxs-lookup"><span data-stu-id="39a60-323">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="39a60-324">`reconnecting`: generato quando il trasporto sottostante inizia la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="39a60-324">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="39a60-325">`reconnected`: generato quando il trasporto sottostante è stato riconnesso.</span><span class="sxs-lookup"><span data-stu-id="39a60-325">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="39a60-326">`stateChanged`: generato quando cambia lo stato della connessione.</span><span class="sxs-lookup"><span data-stu-id="39a60-326">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="39a60-327">Fornisce il vecchio stato e il nuovo stato (Connessione, Connesso, Riconnessione o Disconnesso).</span><span class="sxs-lookup"><span data-stu-id="39a60-327">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="39a60-328">`disconnected`: generato quando la connessione si è disconnessa.</span><span class="sxs-lookup"><span data-stu-id="39a60-328">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="39a60-329">Ad esempio, se si desidera visualizzare messaggi di avviso quando si verificano problemi di connessione che potrebbero causare ritardi evidenti, gestire l'evento. `connectionSlow`</span><span class="sxs-lookup"><span data-stu-id="39a60-329">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="39a60-330">**Gestire l'evento connectionSlow (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-330">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="39a60-331">**Gestire l'evento connectionSlow (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-331">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="39a60-332">Per ulteriori informazioni, vedere Informazioni e gestione degli eventi di durata della [connessione in SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="39a60-332">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="39a60-333">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="39a60-333">How to handle errors</span></span>

<span data-ttu-id="39a60-334">Il client JavaScript SignalR fornisce un `error` evento per il quale è possibile aggiungere un gestore.</span><span class="sxs-lookup"><span data-stu-id="39a60-334">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="39a60-335">È inoltre possibile utilizzare il metodo fail per aggiungere un gestore per gli errori derivanti da una chiamata al metodo server.</span><span class="sxs-lookup"><span data-stu-id="39a60-335">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="39a60-336">Se non si abilitano in modo esplicito i messaggi di errore dettagliati sul server, l'oggetto eccezione restituito da SignalR dopo che un errore contiene informazioni minime sull'errore.</span><span class="sxs-lookup"><span data-stu-id="39a60-336">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="39a60-337">Ad esempio, se `newContosoChatMessage` una chiamata a ha esito`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`negativo, il messaggio di errore nell'oggetto di errore contiene " " L'invio di messaggi di errore dettagliati ai client nell'ambiente di produzione non è consigliato per motivi di sicurezza, ma se si desidera abilitare i messaggi di errore dettagliati per la risoluzione dei problemi, utilizzare il codice seguente sul server.</span><span class="sxs-lookup"><span data-stu-id="39a60-337">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="39a60-338">Nell'esempio seguente viene illustrato come aggiungere un gestore per l'evento di errore.</span><span class="sxs-lookup"><span data-stu-id="39a60-338">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="39a60-339">**Aggiungere un gestore errori (con il proxy generato)Add an error handler (with the generated proxy)**</span><span class="sxs-lookup"><span data-stu-id="39a60-339">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="39a60-340">**Aggiungere un gestore errori (senza il proxy generato)Add an error handler (without the generated proxy)**</span><span class="sxs-lookup"><span data-stu-id="39a60-340">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="39a60-341">Nell'esempio seguente viene illustrato come gestire un errore da una chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="39a60-341">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="39a60-342">**Gestire un errore da una chiamata al metodo (con il proxy generato)Handle an error from a method invocation (with the generated proxy)**</span><span class="sxs-lookup"><span data-stu-id="39a60-342">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="39a60-343">**Gestire un errore da una chiamata al metodo (senza il proxy generato)Handle an error from a method invocation (without the generated proxy)**</span><span class="sxs-lookup"><span data-stu-id="39a60-343">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="39a60-344">Se una chiamata al `error` metodo ha esito negativo, `error` viene generato anche `.fail` l'evento, in modo che il codice nel gestore del metodo e nel callback del metodo venga eseguito.</span><span class="sxs-lookup"><span data-stu-id="39a60-344">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="39a60-345">Come abilitare la registrazione lato clientHow to enable client-side logging</span><span class="sxs-lookup"><span data-stu-id="39a60-345">How to enable client-side logging</span></span>

<span data-ttu-id="39a60-346">Per abilitare la registrazione lato `logging` client su una connessione, `start` impostare la proprietà sull'oggetto connessione prima di chiamare il metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="39a60-346">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="39a60-347">**Abilitare la registrazione (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-347">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="39a60-348">**Abilitare la registrazione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="39a60-348">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="39a60-349">Per visualizzare i log, apri gli strumenti di sviluppo del browser e vai alla scheda Console. Per un'esercitazione che mostra istruzioni dettagliate e schermate che mostrano come eseguire questa operazione, vedere [Trasmissione server con ASP.NET Signalr - Abilitare](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging)la registrazione .</span><span class="sxs-lookup"><span data-stu-id="39a60-349">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span></span>
