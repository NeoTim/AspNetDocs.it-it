---
title: Supporto di WebSocket in ASP.NET Core
author: rick-anderson
description: Introduzione all'uso di oggetti WebSocket in ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/17/2019
uid: fundamentals/websockets
ms.openlocfilehash: 76acb9c96ed5e8bbbaf39eeb6cb23307bb44fb8d
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57031408"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="2ac71-103">Supporto di WebSocket in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ac71-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="2ac71-104">Di [Tom Dykstra](https://github.com/tdykstra) e [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="2ac71-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="2ac71-105">L'articolo contiene l'introduzione all'uso di oggetti WebSocket in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2ac71-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="2ac71-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) è un protocollo che consente canali di comunicazione persistente bidirezionale su connessioni TCP.</span><span class="sxs-lookup"><span data-stu-id="2ac71-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="2ac71-107">Viene usato nelle app che sfruttano comunicazioni veloci e in tempo reale, ad esempio le app di chat, i dashboard e le app di gioco.</span><span class="sxs-lookup"><span data-stu-id="2ac71-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="2ac71-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2ac71-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="2ac71-109">Per altre informazioni, vedere la sezione [Passaggi successivi](#next-steps).</span><span class="sxs-lookup"><span data-stu-id="2ac71-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2ac71-110">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="2ac71-110">Prerequisites</span></span>

* <span data-ttu-id="2ac71-111">ASP.NET Core 1.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="2ac71-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="2ac71-112">Qualsiasi sistema operativo che supporta ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="2ac71-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="2ac71-113">Windows 7/Windows Server 2008 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="2ac71-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="2ac71-114">Linux</span><span class="sxs-lookup"><span data-stu-id="2ac71-114">Linux</span></span>
  * <span data-ttu-id="2ac71-115">macOS</span><span class="sxs-lookup"><span data-stu-id="2ac71-115">macOS</span></span>
  
* <span data-ttu-id="2ac71-116">Se l'app viene eseguita su Windows con IIS:</span><span class="sxs-lookup"><span data-stu-id="2ac71-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="2ac71-117">Windows 8/Windows Server 2012 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="2ac71-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="2ac71-118">IIS 8/IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="2ac71-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="2ac71-119">Gli oggetti WebSocket devono essere abilitati (vedere la sezione [Supporto di IIS/IIS Express](#iisiis-express-support)).</span><span class="sxs-lookup"><span data-stu-id="2ac71-119">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="2ac71-120">Se l'app viene eseguita su [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="2ac71-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="2ac71-121">Windows 8/Windows Server 2012 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="2ac71-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="2ac71-122">Per i browser supportati, vedere https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="2ac71-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="2ac71-123">Quando usare i WebSocket</span><span class="sxs-lookup"><span data-stu-id="2ac71-123">When to use WebSockets</span></span>

<span data-ttu-id="2ac71-124">Usare gli oggetti WebSocket per operare direttamente con una connessione socket.</span><span class="sxs-lookup"><span data-stu-id="2ac71-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="2ac71-125">Ad esempio usare i WebSocket per avere le prestazioni migliori possibili per un gioco in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="2ac71-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="2ac71-126">[ASP.NET Core SignalR](xref:signalr/introduction) è una libreria che consente di aggiungere in modo più semplice funzionalità Web in tempo reale alle app.</span><span class="sxs-lookup"><span data-stu-id="2ac71-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="2ac71-127">Laddove possibile, usa oggetti WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2ac71-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="2ac71-128">Come usare oggetti WebSocket</span><span class="sxs-lookup"><span data-stu-id="2ac71-128">How to use WebSockets</span></span>

* <span data-ttu-id="2ac71-129">Installare il pacchetto [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="2ac71-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="2ac71-130">Configurare il middleware.</span><span class="sxs-lookup"><span data-stu-id="2ac71-130">Configure the middleware.</span></span>
* <span data-ttu-id="2ac71-131">Accettare le richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2ac71-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="2ac71-132">Inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="2ac71-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="2ac71-133">Configurare il middleware</span><span class="sxs-lookup"><span data-stu-id="2ac71-133">Configure the middleware</span></span>

<span data-ttu-id="2ac71-134">Aggiungere il middleware degli oggetti WebSocket nel metodo `Configure` della classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="2ac71-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="2ac71-135">È possibile configurare le impostazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="2ac71-135">The following settings can be configured:</span></span>

* <span data-ttu-id="2ac71-136">`KeepAliveInterval`: la frequenza di invio di frame "ping" al client per garantire che i proxy tengano aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="2ac71-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="2ac71-137">Il valore predefinito è due minuti.</span><span class="sxs-lookup"><span data-stu-id="2ac71-137">The default is two minutes.</span></span>
* <span data-ttu-id="2ac71-138">`ReceiveBufferSize`: le dimensioni del buffer usato per ricevere dati.</span><span class="sxs-lookup"><span data-stu-id="2ac71-138">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="2ac71-139">Gli utenti avanzati possono avere l'esigenza di modificare questa impostazione per ottimizzare le prestazioni in base alle dimensioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="2ac71-139">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="2ac71-140">L'impostazione predefinita è 4 KB.</span><span class="sxs-lookup"><span data-stu-id="2ac71-140">The default is 4 KB.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2ac71-141">È possibile configurare le impostazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="2ac71-141">The following settings can be configured:</span></span>

* <span data-ttu-id="2ac71-142">`KeepAliveInterval`: la frequenza di invio di frame "ping" al client per garantire che i proxy tengano aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="2ac71-142">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="2ac71-143">Il valore predefinito è due minuti.</span><span class="sxs-lookup"><span data-stu-id="2ac71-143">The default is two minutes.</span></span>
* <span data-ttu-id="2ac71-144">`ReceiveBufferSize`: le dimensioni del buffer usato per ricevere dati.</span><span class="sxs-lookup"><span data-stu-id="2ac71-144">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="2ac71-145">Gli utenti avanzati possono avere l'esigenza di modificare questa impostazione per ottimizzare le prestazioni in base alle dimensioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="2ac71-145">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="2ac71-146">L'impostazione predefinita è 4 KB.</span><span class="sxs-lookup"><span data-stu-id="2ac71-146">The default is 4 KB.</span></span>
* <span data-ttu-id="2ac71-147">`AllowedOrigins`: elenco di valori dell'intestazione Origin consentiti per le richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2ac71-147">`AllowedOrigins` - A list of allowed Origin header values for WebSocket requests.</span></span> <span data-ttu-id="2ac71-148">Per impostazione predefinita, tutte le origini sono consentite.</span><span class="sxs-lookup"><span data-stu-id="2ac71-148">By default, all origins are allowed.</span></span> <span data-ttu-id="2ac71-149">Per informazioni dettagliate, vedere più avanti "Restrizione per le origini WebSocket".</span><span class="sxs-lookup"><span data-stu-id="2ac71-149">See "WebSocket origin restriction" below for details.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a><span data-ttu-id="2ac71-150">Accettare le richieste WebSocket</span><span class="sxs-lookup"><span data-stu-id="2ac71-150">Accept WebSocket requests</span></span>

<span data-ttu-id="2ac71-151">In un momento successivo nel ciclo di vita della richiesta, più avanti nel metodo `Configure` o in un'azione MVC, ad esempio, verificare che si tratti di una richiesta WebSocket e accettarla.</span><span class="sxs-lookup"><span data-stu-id="2ac71-151">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="2ac71-152">L'esempio seguente è tratto da un punto successivo nel metodo `Configure`:</span><span class="sxs-lookup"><span data-stu-id="2ac71-152">The following example is from later in the `Configure` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

<span data-ttu-id="2ac71-153">Una richiesta WebSocket può arrivare in qualsiasi URL, ma questo codice di esempio accetta solo le richieste per `/ws`.</span><span class="sxs-lookup"><span data-stu-id="2ac71-153">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="2ac71-154">Inviare e ricevere messaggi</span><span class="sxs-lookup"><span data-stu-id="2ac71-154">Send and receive messages</span></span>

<span data-ttu-id="2ac71-155">Il metodo `AcceptWebSocketAsync` consente di aggiornare la connessione TCP a una connessione WebSocket e offre un oggetto [WebSocket](/dotnet/core/api/system.net.websockets.websocket).</span><span class="sxs-lookup"><span data-stu-id="2ac71-155">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="2ac71-156">Usare l'oggetto `WebSocket` per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="2ac71-156">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="2ac71-157">Il codice illustrato in precedenza che accetta la richiesta WebSocket passa l'oggetto `WebSocket` a un metodo `Echo`.</span><span class="sxs-lookup"><span data-stu-id="2ac71-157">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="2ac71-158">Il codice riceve un messaggio e lo reinvia immediatamente.</span><span class="sxs-lookup"><span data-stu-id="2ac71-158">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="2ac71-159">I messaggi vengono inviati e ricevuti in un ciclo, fino a quando il client non chiude la connessione:</span><span class="sxs-lookup"><span data-stu-id="2ac71-159">Messages are sent and received in a loop until the client closes the connection:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

<span data-ttu-id="2ac71-160">Quando si accetta la connessione WebSocket prima che inizi il ciclo, la pipeline del middleware termina.</span><span class="sxs-lookup"><span data-stu-id="2ac71-160">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="2ac71-161">Dopo la chiusura del socket, la pipeline si arresta,</span><span class="sxs-lookup"><span data-stu-id="2ac71-161">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="2ac71-162">In altri termini, quando viene accettato il WebSocket, lo spostamento in avanti della richiesta nella pipeline si interrompe.</span><span class="sxs-lookup"><span data-stu-id="2ac71-162">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="2ac71-163">Quando il ciclo viene completato e il socket viene chiuso, la richiesta torna ad avanzare nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="2ac71-163">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="handle-client-disconnects"></a><span data-ttu-id="2ac71-164">Gestire le disconnessioni del client</span><span class="sxs-lookup"><span data-stu-id="2ac71-164">Handle client disconnects</span></span>

<span data-ttu-id="2ac71-165">Il server non viene informato automaticamente quando il client si disconnette a causa della perdita di connettività.</span><span class="sxs-lookup"><span data-stu-id="2ac71-165">The server is not automatically informed when the client disconnects due to loss of connectivity.</span></span> <span data-ttu-id="2ac71-166">Il server riceve un messaggio di disconnessione solo se inviato dal client, ma questo non è possibile in caso di interruzione della connessione Internet.</span><span class="sxs-lookup"><span data-stu-id="2ac71-166">The server receives a disconnect message only if the client sends it, which can't be done if the internet connection is lost.</span></span> <span data-ttu-id="2ac71-167">Se si vuole intervenire quando si verifica una situazione di questo tipo, impostare un timeout per segnalare che non sono stati ricevuti messaggi dal client entro un determinato intervallo di tempo.</span><span class="sxs-lookup"><span data-stu-id="2ac71-167">If you want to take some action when that happens, set a timeout after nothing is received from the client within a certain time window.</span></span>

<span data-ttu-id="2ac71-168">Se il client non invia messaggi con una certa frequenza e non si vuole impostare un timeout solo perché la connessione diventa inattiva, configurare un timer nel client in modo da inviare un messaggio ping ogni X secondi.</span><span class="sxs-lookup"><span data-stu-id="2ac71-168">If the client isn't always sending messages and you don't want to timeout just because the connection goes idle, have the client use a timer to send a ping message every X seconds.</span></span> <span data-ttu-id="2ac71-169">Nel server, se un messaggio non è arrivato entro 2\*X secondi dal precedente, terminare la connessione e segnalare che il client si è disconnesso.</span><span class="sxs-lookup"><span data-stu-id="2ac71-169">On the server, if a message hasn't arrived within 2\*X seconds after the previous one, terminate the connection and report that the client disconnected.</span></span> <span data-ttu-id="2ac71-170">Attendere il doppio del tempo previsto per tenere conto di eventuali ritardi della rete che potrebbero trattenere il messaggio ping.</span><span class="sxs-lookup"><span data-stu-id="2ac71-170">Wait for twice the expected time interval to leave extra time for network delays that might hold up the ping message.</span></span>

### <a name="websocket-origin-restriction"></a><span data-ttu-id="2ac71-171">Restrizione per le origini WebSocket</span><span class="sxs-lookup"><span data-stu-id="2ac71-171">WebSocket origin restriction</span></span>

<span data-ttu-id="2ac71-172">La protezione fornita da CORS non si applica agli oggetti WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2ac71-172">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="2ac71-173">I browser **non**:</span><span class="sxs-lookup"><span data-stu-id="2ac71-173">Browsers do **not**:</span></span>

* <span data-ttu-id="2ac71-174">Eseguono richieste CORS preventive.</span><span class="sxs-lookup"><span data-stu-id="2ac71-174">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="2ac71-175">Rispettano le restrizioni specificate nelle intestazioni `Access-Control` quando eseguono richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2ac71-175">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="2ac71-176">I browser, tuttavia, inviano l'intestazione `Origin` quando rilasciano richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2ac71-176">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="2ac71-177">Le applicazioni devono essere configurate per la convalida di queste intestazioni per assicurarsi che siano consentiti solo WebSocket provenienti dalle origini previste.</span><span class="sxs-lookup"><span data-stu-id="2ac71-177">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="2ac71-178">Se si ospita il server in "https://server.com" e il client in "https://client.com", aggiungere "https://client.com" all'elenco `AllowedOrigins` per consentire la verifica dei WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2ac71-178">If you're hosting your server on "https://server.com" and hosting your client on "https://client.com", add "https://client.com" to the `AllowedOrigins` list for WebSockets to verify.</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptionsAO&highlight=6-7)]

> [!NOTE]
> <span data-ttu-id="2ac71-179">L'intestazione `Origin` viene controllata dal client e, come l'intestazione `Referer`, può essere falsificata.</span><span class="sxs-lookup"><span data-stu-id="2ac71-179">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="2ac71-180">**Non** usare queste intestazioni come meccanismo di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="2ac71-180">Do **not** use these headers as an authentication mechanism.</span></span>

::: moniker-end

## <a name="iisiis-express-support"></a><span data-ttu-id="2ac71-181">Supporto di IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="2ac71-181">IIS/IIS Express support</span></span>

<span data-ttu-id="2ac71-182">Windows Server 2012 o versioni successive e Windows 8 o versioni successive con IIS/IIS Express 8 o versioni successive includono il supporto per il protocollo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2ac71-182">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="2ac71-183">Gli oggetti WebSocket sono sempre abilitati quando si usa IIS Express.</span><span class="sxs-lookup"><span data-stu-id="2ac71-183">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="2ac71-184">Abilitazione di oggetti WebSocket in IIS</span><span class="sxs-lookup"><span data-stu-id="2ac71-184">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="2ac71-185">Per abilitare il supporto per il protocollo WebSocket in Windows Server 2012 o versioni successive:</span><span class="sxs-lookup"><span data-stu-id="2ac71-185">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="2ac71-186">Questi passaggi non sono necessari quando si usa IIS Express</span><span class="sxs-lookup"><span data-stu-id="2ac71-186">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="2ac71-187">Usare la procedura guidata **Aggiungi ruoli e funzionalità** accessibile tramite il menu **Gestisci** o il collegamento in **Server Manager**.</span><span class="sxs-lookup"><span data-stu-id="2ac71-187">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="2ac71-188">Selezionare **Installazione basata su ruoli o basata su funzionalità**.</span><span class="sxs-lookup"><span data-stu-id="2ac71-188">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="2ac71-189">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="2ac71-189">Select **Next**.</span></span>
1. <span data-ttu-id="2ac71-190">Selezionare il server appropriato (il server locale è selezionato per impostazione predefinita).</span><span class="sxs-lookup"><span data-stu-id="2ac71-190">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="2ac71-191">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="2ac71-191">Select **Next**.</span></span>
1. <span data-ttu-id="2ac71-192">Espandere **Server Web (IIS)** nella struttura **Ruoli**, espandere **Server Web**, quindi **Sviluppo applicazioni**.</span><span class="sxs-lookup"><span data-stu-id="2ac71-192">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="2ac71-193">Selezionare **Protocollo WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="2ac71-193">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="2ac71-194">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="2ac71-194">Select **Next**.</span></span>
1. <span data-ttu-id="2ac71-195">Se non sono necessarie le funzionalità aggiuntive, selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="2ac71-195">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="2ac71-196">Selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="2ac71-196">Select **Install**.</span></span>
1. <span data-ttu-id="2ac71-197">Al termine dell'installazione, selezionare **Chiudi** per chiudere la procedura guidata.</span><span class="sxs-lookup"><span data-stu-id="2ac71-197">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="2ac71-198">Per abilitare il supporto per il protocollo WebSocket in Windows 8 o versioni successive:</span><span class="sxs-lookup"><span data-stu-id="2ac71-198">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="2ac71-199">Questi passaggi non sono necessari quando si usa IIS Express</span><span class="sxs-lookup"><span data-stu-id="2ac71-199">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="2ac71-200">Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).</span><span class="sxs-lookup"><span data-stu-id="2ac71-200">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="2ac71-201">Aprire i nodi seguenti: **Internet Information Services** > **Servizi Web** > **Funzionalità per lo sviluppo di applicazioni**.</span><span class="sxs-lookup"><span data-stu-id="2ac71-201">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="2ac71-202">Selezionare la funzionalità **Protocollo WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="2ac71-202">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="2ac71-203">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="2ac71-203">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="2ac71-204">Disabilitare WebSocket quando si usa socket.io su node.js</span><span class="sxs-lookup"><span data-stu-id="2ac71-204">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="2ac71-205">Se si usa il supporto WebSocket in [socket.io](https://socket.io/) su [Node.js](https://nodejs.org/), disabilitare il modulo WebSocket IIS predefinito mediante l'elemento `webSocket` in *web.config* o in *applicationHost.config*. Se non viene eseguito questo passaggio, il modulo IIS WebSocket prova a gestire le comunicazioni WebSocket anziché Node.js e l'app.</span><span class="sxs-lookup"><span data-stu-id="2ac71-205">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="2ac71-206">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="2ac71-206">Next steps</span></span>

<span data-ttu-id="2ac71-207">L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) inclusa in questo articolo è un'app echo.</span><span class="sxs-lookup"><span data-stu-id="2ac71-207">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="2ac71-208">Ha una pagina Web che consente la connessione WebSocket e il server reinvia al client tutti i messaggi ricevuti.</span><span class="sxs-lookup"><span data-stu-id="2ac71-208">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="2ac71-209">Eseguire l'app da un prompt dei comandi, in quanto non è configurata per l'esecuzione da Visual Studio con IIS Express, quindi passare a http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="2ac71-209">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="2ac71-210">Nella parte superiore sinistra della pagina Web viene visualizzato lo stato della connessione:</span><span class="sxs-lookup"><span data-stu-id="2ac71-210">The web page shows the connection status in the upper left:</span></span>

![Stato iniziale della pagina Web](websockets/_static/start.png)

<span data-ttu-id="2ac71-212">Selezionare **Connect** (Connetti) per inviare una richiesta WebSocket per l'URL indicato.</span><span class="sxs-lookup"><span data-stu-id="2ac71-212">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="2ac71-213">Immettere un messaggio di prova e selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="2ac71-213">Enter a test message and select **Send**.</span></span> <span data-ttu-id="2ac71-214">Al termine dell'operazione, selezionare **Close Socket** (Chiudi socket).</span><span class="sxs-lookup"><span data-stu-id="2ac71-214">When done, select **Close Socket**.</span></span> <span data-ttu-id="2ac71-215">La sezione **Comminication Log** (Registrazione comunicazione) riporta tutte le azioni di apertura, invio e chiusura nel momento in cui vengono eseguite.</span><span class="sxs-lookup"><span data-stu-id="2ac71-215">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Stato iniziale della pagina Web](websockets/_static/end.png)
