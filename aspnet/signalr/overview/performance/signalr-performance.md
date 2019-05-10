---
uid: signalr/overview/performance/signalr-performance
title: Prestazioni di SignalR | Microsoft Docs
author: bradygaster
description: Prestazioni di SignalR
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: b8a44f4c924c94cdfff1ce7630539b45fe269bbf
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65113585"
---
# <a name="signalr-performance"></a><span data-ttu-id="b0df0-103">Prestazioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="b0df0-103">SignalR Performance</span></span>

<span data-ttu-id="b0df0-104">da [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="b0df0-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="b0df0-105">In questo argomento viene descritto come progettare per misurare e migliorare le prestazioni in un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="b0df0-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="b0df0-106">Versioni del software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="b0df0-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="b0df0-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b0df0-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="b0df0-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b0df0-108">.NET 4.5</span></span>
> - <span data-ttu-id="b0df0-109">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="b0df0-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="b0df0-110">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="b0df0-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="b0df0-111">Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="b0df0-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="b0df0-112">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="b0df0-112">Questions and comments</span></span>
>
> <span data-ttu-id="b0df0-113">Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="b0df0-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="b0df0-114">Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="b0df0-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="b0df0-115">Per una presentazione recenti sulle prestazioni di SignalR e scalabilità, vedere [scala Web in tempo reale con ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="b0df0-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="b0df0-116">Di seguito sono elencate le diverse sezioni di questo argomento:</span><span class="sxs-lookup"><span data-stu-id="b0df0-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="b0df0-117">Considerazioni di progettazione</span><span class="sxs-lookup"><span data-stu-id="b0df0-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="b0df0-118">Ottimizzazione del server di SignalR per le prestazioni</span><span class="sxs-lookup"><span data-stu-id="b0df0-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="b0df0-119">Risoluzione dei problemi di prestazioni</span><span class="sxs-lookup"><span data-stu-id="b0df0-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="b0df0-120">Usando i contatori delle prestazioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="b0df0-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="b0df0-121">Uso di altri contatori delle prestazioni</span><span class="sxs-lookup"><span data-stu-id="b0df0-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="b0df0-122">Altre risorse</span><span class="sxs-lookup"><span data-stu-id="b0df0-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="b0df0-123">Considerazioni di progettazione</span><span class="sxs-lookup"><span data-stu-id="b0df0-123">Design considerations</span></span>

<span data-ttu-id="b0df0-124">In questa sezione vengono descritti i modelli che possono essere implementati durante la progettazione di un'applicazione di SignalR, per garantire che le prestazioni non sia in corso ostacolata generando traffico di rete superfluo.</span><span class="sxs-lookup"><span data-stu-id="b0df0-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="b0df0-125">La limitazione della frequenza di messaggio</span><span class="sxs-lookup"><span data-stu-id="b0df0-125">Throttling message frequency</span></span>

<span data-ttu-id="b0df0-126">Anche in un'applicazione che invia i messaggi con una frequenza elevata (ad esempio un'applicazione di gioco in tempo reale), la maggior parte delle applicazioni non necessario inviare più messaggi al secondo.</span><span class="sxs-lookup"><span data-stu-id="b0df0-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="b0df0-127">Per ridurre la quantità di traffico che ogni client che genera l'errore, un ciclo di messaggi può essere implementato che le code e invia i messaggi spesso non più di una tariffa fissa (vale a dire, fino a un certo numero di messaggi verranno inviati al secondo, se sono presenti messaggi in quel momento in tervallo invio).</span><span class="sxs-lookup"><span data-stu-id="b0df0-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="b0df0-128">Per un'applicazione di esempio che limita i messaggi a una velocità determinata (da client e server), vedere [messaggistica ad alta frequenza con SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="b0df0-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="b0df0-129">Riduzione delle dimensioni dei messaggi</span><span class="sxs-lookup"><span data-stu-id="b0df0-129">Reducing message size</span></span>

<span data-ttu-id="b0df0-130">È possibile ridurre le dimensioni di un messaggio SignalR riducendo le dimensioni degli oggetti serializzati.</span><span class="sxs-lookup"><span data-stu-id="b0df0-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="b0df0-131">Nel codice server, se si sta inviando un oggetto che contiene le proprietà che non devono essere trasmessi, impedire che tali proprietà viene serializzata utilizzando il `JsonIgnore` attributo.</span><span class="sxs-lookup"><span data-stu-id="b0df0-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="b0df0-132">I nomi delle proprietà vengono archiviati anche nel messaggio. i nomi delle proprietà possono essere abbreviati utilizzando il `JsonProperty` attributo.</span><span class="sxs-lookup"><span data-stu-id="b0df0-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="b0df0-133">Esempio di codice seguente viene illustrato come escludere una proprietà che vengano inviati al client e come abbreviare i nomi delle proprietà:</span><span class="sxs-lookup"><span data-stu-id="b0df0-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="b0df0-134">**Codice del server .NET che dimostra l'attributo JsonIgnore per escludere dati che vengano inviati al client e l'attributo JsonProperty per ridurre la dimensione del messaggio**</span><span class="sxs-lookup"><span data-stu-id="b0df0-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="b0df0-135">Per mantenere la leggibilità / manutenibilità del codice client, i nomi delle proprietà abbreviato può essere rimappato a Converto nomi dopo viene ricevuto il messaggio.</span><span class="sxs-lookup"><span data-stu-id="b0df0-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="b0df0-136">Esempio di codice seguente viene illustrato uno dei possibili modi di modifica del mapping di nomi abbreviati per quelle più lunghe, definendo un contratto di messaggio (mapping) e l'uso di `reMap` funzione da applicare il contratto per la classe messaggio ottimizzata:</span><span class="sxs-lookup"><span data-stu-id="b0df0-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="b0df0-137">**Codice JavaScript lato client che esegue un nuovo mapping abbreviato i nomi delle proprietà ai nomi leggibili**</span><span class="sxs-lookup"><span data-stu-id="b0df0-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="b0df0-138">I nomi possono essere abbreviati nei messaggi dal client al server, anche con lo stesso metodo.</span><span class="sxs-lookup"><span data-stu-id="b0df0-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="b0df0-139">Ridurre il footprint di memoria (vale a dire, la quantità di memoria usata per il messaggio) del messaggio di oggetto può anche migliorare le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="b0df0-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="b0df0-140">Ad esempio, se la gamma completa di un `int` non è necessaria, una `short` o `byte` è invece possibile usare.</span><span class="sxs-lookup"><span data-stu-id="b0df0-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="b0df0-141">Poiché i messaggi vengono archiviati nel bus di messaggi in memoria del server, riducendo le dimensioni dei messaggi può anche risolvere i problemi di memoria di server.</span><span class="sxs-lookup"><span data-stu-id="b0df0-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="b0df0-142">Ottimizzazione del server di SignalR per le prestazioni</span><span class="sxs-lookup"><span data-stu-id="b0df0-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="b0df0-143">Le impostazioni di configurazione seguente sono utilizzabile per ottimizzare il server per migliorare le prestazioni in un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="b0df0-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="b0df0-144">Per informazioni generali su come migliorare le prestazioni in un'applicazione ASP.NET, vedere [miglioramento delle prestazioni ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="b0df0-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="b0df0-145">**Impostazioni di configurazione di SignalR**</span><span class="sxs-lookup"><span data-stu-id="b0df0-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="b0df0-146">**DefaultMessageBufferSize**: Per impostazione predefinita, SignalR mantiene 1000 messaggi in memoria per ogni hub per ogni connessione.</span><span class="sxs-lookup"><span data-stu-id="b0df0-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="b0df0-147">Se vengono utilizzati messaggi di grandi dimensioni, ciò può creare problemi di memoria che possono essere mitigati mediante la riduzione di questo valore.</span><span class="sxs-lookup"><span data-stu-id="b0df0-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="b0df0-148">Questa impostazione può essere impostata `Application_Start` gestore dell'evento in un'applicazione ASP.NET o nel `Configuration` metodo di una classe di avvio OWIN in un'applicazione self-hosted.</span><span class="sxs-lookup"><span data-stu-id="b0df0-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="b0df0-149">L'esempio seguente viene illustrato come ridurre questo valore per ridurre il footprint di memoria dell'applicazione per ridurre la quantità di memoria del server usata:</span><span class="sxs-lookup"><span data-stu-id="b0df0-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="b0df0-150">**Codice server .NET nel file Startup.cs per la riduzione delle dimensioni del buffer di messaggio predefinita**</span><span class="sxs-lookup"><span data-stu-id="b0df0-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="b0df0-151">**Impostazioni di configurazione di IIS**</span><span class="sxs-lookup"><span data-stu-id="b0df0-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="b0df0-152">**Numero massimo di richieste simultanee per ogni applicazione**: L'aumento del numero di IIS simultanee richieste aumenterà le risorse server disponibili per soddisfare le richieste.</span><span class="sxs-lookup"><span data-stu-id="b0df0-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="b0df0-153">Il valore predefinito è 5000; Per aumentare questa impostazione, eseguire i comandi seguenti in un prompt dei comandi con privilegi elevati:</span><span class="sxs-lookup"><span data-stu-id="b0df0-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="b0df0-154">**ApplicationPool QueueLength**: Questo è il numero massimo di richieste che mette in coda HTTP. sys per il pool di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="b0df0-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="b0df0-155">Quando la coda è piena, le nuove richieste ricevono una risposta 503 "Servizio non disponibile".</span><span class="sxs-lookup"><span data-stu-id="b0df0-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="b0df0-156">Il valore predefinito è 1000.</span><span class="sxs-lookup"><span data-stu-id="b0df0-156">The default value is 1000.</span></span>

    <span data-ttu-id="b0df0-157">Ridurre la lunghezza della coda per il processo di lavoro nel pool di applicazioni che ospitano l'applicazione verrà conservare le risorse di memoria.</span><span class="sxs-lookup"><span data-stu-id="b0df0-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="b0df0-158">Per altre informazioni, vedere [gestione dell'ottimizzazione e la configurazione di pool di applicazioni](https://technet.microsoft.com/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="b0df0-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="b0df0-159">**Impostazioni di configurazione ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="b0df0-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="b0df0-160">In questa sezione include le impostazioni di configurazione che possono essere impostate nella `aspnet.config` file.</span><span class="sxs-lookup"><span data-stu-id="b0df0-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="b0df0-161">Questo file si trova in uno dei due percorsi, a seconda della piattaforma:</span><span class="sxs-lookup"><span data-stu-id="b0df0-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="b0df0-162">Impostazioni di ASP.NET che possono migliorare le prestazioni di SignalR includono quanto segue:</span><span class="sxs-lookup"><span data-stu-id="b0df0-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="b0df0-163">**Numero massimo di richieste simultaneo per CPU**: Aumentare questa impostazione può ridurre i colli di bottiglia delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="b0df0-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="b0df0-164">Per aumentare questa impostazione, aggiungere la seguente impostazione di configurazione per il `aspnet.config` file:</span><span class="sxs-lookup"><span data-stu-id="b0df0-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="b0df0-165">**Limite coda richieste**: Quando il numero totale di connessioni supera le `maxConcurrentRequestsPerCPU` impostazione, ASP.NET inizierà a limitazione delle richieste tramite una coda.</span><span class="sxs-lookup"><span data-stu-id="b0df0-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="b0df0-166">Per aumentare le dimensioni della coda, è possibile aumentare il `requestQueueLimit` impostazione.</span><span class="sxs-lookup"><span data-stu-id="b0df0-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="b0df0-167">A tale scopo, aggiungere la seguente impostazione di configurazione per il `processModel` nodo `config/machine.config` (anziché `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="b0df0-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="b0df0-168">Risoluzione dei problemi di prestazioni</span><span class="sxs-lookup"><span data-stu-id="b0df0-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="b0df0-169">In questa sezione vengono descritti i modi per trovare i colli di bottiglia nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b0df0-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="b0df0-170">Verifica che venga usato WebSocket</span><span class="sxs-lookup"><span data-stu-id="b0df0-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="b0df0-171">Sebbene SignalR è possibile usare un'ampia gamma di trasporti per le comunicazioni tra client e server, WebSocket presenta un vantaggio significativo delle prestazioni e deve essere utilizzata se il client e il server lo supportano.</span><span class="sxs-lookup"><span data-stu-id="b0df0-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="b0df0-172">Per determinare se il client e il server soddisfa i requisiti per WebSocket, vedere [trasporti e i fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="b0df0-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="b0df0-173">Per determinare quali il trasporto utilizzato nell'applicazione, è possibile usare gli strumenti di sviluppo del browser ed esaminare i registri per verificare quali il trasporto viene usato per la connessione.</span><span class="sxs-lookup"><span data-stu-id="b0df0-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="b0df0-174">Per informazioni sull'uso degli strumenti di sviluppo del browser in Internet Explorer e Chrome, vedere [trasporti e i fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="b0df0-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="b0df0-175">Usando i contatori delle prestazioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="b0df0-175">Using SignalR performance counters</span></span>

<span data-ttu-id="b0df0-176">Questa sezione descrive come abilitare e usare contatori delle prestazioni di SignalR, disponibili nel `Microsoft.AspNet.SignalR.Utils` pacchetto.</span><span class="sxs-lookup"><span data-stu-id="b0df0-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="b0df0-177">Installazione signalr.exe</span><span class="sxs-lookup"><span data-stu-id="b0df0-177">Installing signalr.exe</span></span>

<span data-ttu-id="b0df0-178">I contatori delle prestazioni possono essere aggiunti al server usando un'utilità denominata SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="b0df0-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="b0df0-179">Per installare questa utilità, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="b0df0-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="b0df0-180">In Visual Studio, selezionare **strumenti di** > **Gestione pacchetti NuGet** > **Gestisci pacchetti NuGet per soluzione**</span><span class="sxs-lookup"><span data-stu-id="b0df0-180">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="b0df0-181">Cercare **signalr.utils**e scegliere Installa.</span><span class="sxs-lookup"><span data-stu-id="b0df0-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="b0df0-182">Accettare il contratto di licenza per installare il pacchetto.</span><span class="sxs-lookup"><span data-stu-id="b0df0-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="b0df0-183">Sarà installato SignalR.exe `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="b0df0-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="b0df0-184">Installazione dei contatori delle prestazioni con SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="b0df0-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="b0df0-185">Per installare i contatori delle prestazioni di SignalR, eseguire SignalR.exe in un prompt dei comandi con privilegi elevati con il parametro seguente:</span><span class="sxs-lookup"><span data-stu-id="b0df0-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="b0df0-186">Per rimuovere i contatori delle prestazioni di SignalR, eseguire SignalR.exe in un prompt dei comandi con privilegi elevati con il parametro seguente:</span><span class="sxs-lookup"><span data-stu-id="b0df0-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="b0df0-187">Contatori delle prestazioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="b0df0-187">SignalR Performance counters</span></span>

<span data-ttu-id="b0df0-188">Il pacchetto di utilità installa i contatori delle prestazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="b0df0-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="b0df0-189">I contatori "Totale" misurano il numero di eventi dopo l'ultimo pool di applicazioni o server riavviare.</span><span class="sxs-lookup"><span data-stu-id="b0df0-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="b0df0-190">**Metriche relative alla connessione**</span><span class="sxs-lookup"><span data-stu-id="b0df0-190">**Connection metrics**</span></span>

<span data-ttu-id="b0df0-191">Le metriche seguenti misurano gli eventi di durata connessione che si verificano.</span><span class="sxs-lookup"><span data-stu-id="b0df0-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="b0df0-192">Per altre informazioni, vedere [comprensione e la gestione degli eventi di durata connessione](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="b0df0-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="b0df0-193">**Connessioni connesse**</span><span class="sxs-lookup"><span data-stu-id="b0df0-193">**Connections Connected**</span></span>
- <span data-ttu-id="b0df0-194">**Connessioni riconnesse**</span><span class="sxs-lookup"><span data-stu-id="b0df0-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="b0df0-195">**Connessioni disconnesse**</span><span class="sxs-lookup"><span data-stu-id="b0df0-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="b0df0-196">**Connessioni correnti**</span><span class="sxs-lookup"><span data-stu-id="b0df0-196">**Connections Current**</span></span>

<span data-ttu-id="b0df0-197">**Metrica messaggio**</span><span class="sxs-lookup"><span data-stu-id="b0df0-197">**Message metrics**</span></span>

<span data-ttu-id="b0df0-198">Le seguenti metriche misurano il traffico di messaggi generato da SignalR.</span><span class="sxs-lookup"><span data-stu-id="b0df0-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="b0df0-199">**Totale messaggi ricevuti connessione**</span><span class="sxs-lookup"><span data-stu-id="b0df0-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="b0df0-200">**Totale inviati messaggi di connessione**</span><span class="sxs-lookup"><span data-stu-id="b0df0-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="b0df0-201">**Connessione dei messaggi ricevuti/Sec**</span><span class="sxs-lookup"><span data-stu-id="b0df0-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="b0df0-202">**Connessione dei messaggi inviati/Sec**</span><span class="sxs-lookup"><span data-stu-id="b0df0-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="b0df0-203">**Metriche del bus di messaggi**</span><span class="sxs-lookup"><span data-stu-id="b0df0-203">**Message bus metrics**</span></span>

<span data-ttu-id="b0df0-204">Le seguenti metriche misurano il traffico attraverso il bus di messaggi SignalR interno, la coda in cui vengono inseriti tutti i messaggi SignalR in ingresso e in uscita.</span><span class="sxs-lookup"><span data-stu-id="b0df0-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="b0df0-205">È un messaggio **Published** quando viene inviato o trasmettere.</span><span class="sxs-lookup"><span data-stu-id="b0df0-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="b0df0-206">Oggetto **sottoscrittore** in questo contesto è una sottoscrizione del bus di messaggi, questo deve essere uguale al numero di client più il server stesso.</span><span class="sxs-lookup"><span data-stu-id="b0df0-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="b0df0-207">Un' **ruolo di lavoro allocati** è un componente che invia i dati per le connessioni attive; un **ruolo di lavoro occupati** è quello che sta inviando attivamente un messaggio.</span><span class="sxs-lookup"><span data-stu-id="b0df0-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="b0df0-208">**Totale ricevuti messaggi di Bus di messaggi**</span><span class="sxs-lookup"><span data-stu-id="b0df0-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="b0df0-209">**Messaggio Bus di messaggi ricevuti/Sec**</span><span class="sxs-lookup"><span data-stu-id="b0df0-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="b0df0-210">**I messaggi del Bus di messaggi pubblicati totale**</span><span class="sxs-lookup"><span data-stu-id="b0df0-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="b0df0-211">**Messaggio Bus di messaggi pubblicati/Sec**</span><span class="sxs-lookup"><span data-stu-id="b0df0-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="b0df0-212">**Messaggio del Bus sottoscrittori corrente**</span><span class="sxs-lookup"><span data-stu-id="b0df0-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="b0df0-213">**Totale di sottoscrizione del Bus di messaggi**</span><span class="sxs-lookup"><span data-stu-id="b0df0-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="b0df0-214">**I server di sottoscrizione del Bus di messaggi/Sec**</span><span class="sxs-lookup"><span data-stu-id="b0df0-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="b0df0-215">**Bus di messaggi allocati i ruoli di lavoro**</span><span class="sxs-lookup"><span data-stu-id="b0df0-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="b0df0-216">**Messaggio del Bus thread di lavoro occupati**</span><span class="sxs-lookup"><span data-stu-id="b0df0-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="b0df0-217">**Corrente di argomenti del Bus di messaggi**</span><span class="sxs-lookup"><span data-stu-id="b0df0-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="b0df0-218">**Metriche di errore**</span><span class="sxs-lookup"><span data-stu-id="b0df0-218">**Error metrics**</span></span>

<span data-ttu-id="b0df0-219">Le metriche seguenti misurano gli errori generati dal traffico di messaggi SignalR.</span><span class="sxs-lookup"><span data-stu-id="b0df0-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="b0df0-220">**Risoluzione dell'hub** si verificano errori quando un hub o un metodo dell'hub non può essere risolto.</span><span class="sxs-lookup"><span data-stu-id="b0df0-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="b0df0-221">**Chiamata dell'hub** gli errori sono le eccezioni generate durante la chiamata di un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="b0df0-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="b0df0-222">**Trasporto** gli errori di connessione generata un'eccezione durante una richiesta o risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="b0df0-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="b0df0-223">**Errori: Tutti i totali**</span><span class="sxs-lookup"><span data-stu-id="b0df0-223">**Errors: All Total**</span></span>
- <span data-ttu-id="b0df0-224">**Errori: All/Sec**</span><span class="sxs-lookup"><span data-stu-id="b0df0-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="b0df0-225">**Errori: Totale di risoluzione dell'hub**</span><span class="sxs-lookup"><span data-stu-id="b0df0-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="b0df0-226">**Errori: Risoluzione dell'hub al secondo**</span><span class="sxs-lookup"><span data-stu-id="b0df0-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="b0df0-227">**Errori: Hub Invocation Total**</span><span class="sxs-lookup"><span data-stu-id="b0df0-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="b0df0-228">**Errori: Chiamata dell'hub al secondo**</span><span class="sxs-lookup"><span data-stu-id="b0df0-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="b0df0-229">**Errori: Totale trasporto**</span><span class="sxs-lookup"><span data-stu-id="b0df0-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="b0df0-230">**Errori: Messaggi trasporto/Sec**</span><span class="sxs-lookup"><span data-stu-id="b0df0-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="b0df0-231">**Metrica di scalabilità orizzontale**</span><span class="sxs-lookup"><span data-stu-id="b0df0-231">**Scaleout metrics**</span></span>

<span data-ttu-id="b0df0-232">Le seguenti metriche misurano il traffico e gli errori generati dal provider di scalabilità orizzontale.</span><span class="sxs-lookup"><span data-stu-id="b0df0-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="b0df0-233">Oggetto **Stream** in questo contesto è un'unità di scala utilizzata dal provider di scalabilità orizzontale; si tratta di una tabella se viene utilizzato SQL Server, un argomento se si usa il Bus di servizio e una sottoscrizione se Redis è usato.</span><span class="sxs-lookup"><span data-stu-id="b0df0-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="b0df0-234">Ogni flusso garantisce ordinato operazioni di lettura e scrittura; un singolo flusso è un potenziale collo di bottiglia di scalabilità, pertanto il numero dei flussi può essere aumentato per ridurre tale collo di bottiglia.</span><span class="sxs-lookup"><span data-stu-id="b0df0-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="b0df0-235">Se si utilizzano più flussi, SignalR distribuirà automaticamente i messaggi (partizione) tra questi flussi in modo da assicurarsi che i messaggi inviati da qualsiasi connessione specificata sono in ordine.</span><span class="sxs-lookup"><span data-stu-id="b0df0-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="b0df0-236">Il [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) permette di controllare la lunghezza della coda di invio con scalabilità orizzontale gestita da SignalR.</span><span class="sxs-lookup"><span data-stu-id="b0df0-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="b0df0-237">Impostandola su un valore maggiore di 0 inseriranno tutti i messaggi in una coda di trasmissione da inviare al backplane di messaggistica configurato una alla volta.</span><span class="sxs-lookup"><span data-stu-id="b0df0-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="b0df0-238">Se le dimensioni della coda supera la lunghezza configurata, le chiamate successive per l'invio verrà subito esito negativo con un [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) fino a quando il numero di messaggi nella coda è minore dell'impostazione nuovamente.</span><span class="sxs-lookup"><span data-stu-id="b0df0-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="b0df0-239">Accodamento messaggi sono disabilitata per impostazione predefinita perché i ripiani posteriori di implementazione delle dispone in genere il proprio controllo del flusso o Accodamento messaggi posto.</span><span class="sxs-lookup"><span data-stu-id="b0df0-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="b0df0-240">Nel caso di SQL Server, pool di connessioni in modo efficace limita il numero delle trasmissioni in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="b0df0-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="b0df0-241">Per impostazione predefinita, viene usato un solo flusso per SQL Server e Redis, per il Bus di servizio, vengono usati i cinque flussi e accodamento è disabilitata, ma queste impostazioni possono essere modificate tramite la configurazione su SQL Server e del Bus di servizio:</span><span class="sxs-lookup"><span data-stu-id="b0df0-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="b0df0-242">**Codice del server .NET per la configurazione di lunghezza nella tabella di conteggio e di Accodamento per backplane di SQL Server**</span><span class="sxs-lookup"><span data-stu-id="b0df0-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="b0df0-243">**Codice del server .NET per la configurazione di lunghezza argomento di conteggio e di Accodamento per backplane del Bus di servizio**</span><span class="sxs-lookup"><span data-stu-id="b0df0-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="b0df0-244">Oggetto **Buffering** flusso è quello che è entrato in uno stato di errore; quando il flusso è in stato di errore, tutti i messaggi inviati la backplane non riuscirà finché il flusso non è più eventi di errore.</span><span class="sxs-lookup"><span data-stu-id="b0df0-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="b0df0-245">Il **lunghezza coda di invio** è il numero di messaggi che sono stati registrati ma non ancora inviati.</span><span class="sxs-lookup"><span data-stu-id="b0df0-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="b0df0-246">**Bus di messaggi con scalabilità orizzontale dei messaggi ricevuti/Sec**</span><span class="sxs-lookup"><span data-stu-id="b0df0-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="b0df0-247">**Totale flussi con scalabilità orizzontale**</span><span class="sxs-lookup"><span data-stu-id="b0df0-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="b0df0-248">**Scalabilità orizzontale trasmette aperto**</span><span class="sxs-lookup"><span data-stu-id="b0df0-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="b0df0-249">**Memorizzazione nel buffer i flussi di scalabilità orizzontale**</span><span class="sxs-lookup"><span data-stu-id="b0df0-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="b0df0-250">**Totale errori di scalabilità orizzontale**</span><span class="sxs-lookup"><span data-stu-id="b0df0-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="b0df0-251">**Errori di scalabilità orizzontale/Sec**</span><span class="sxs-lookup"><span data-stu-id="b0df0-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="b0df0-252">**Lunghezza coda di invii di scalabilità orizzontale**</span><span class="sxs-lookup"><span data-stu-id="b0df0-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="b0df0-253">Per altre informazioni su ciò che siano misurando questi contatori, vedere [SignalR Scaleout con il Bus di servizio di Azure](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="b0df0-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="b0df0-254">Uso di altri contatori delle prestazioni</span><span class="sxs-lookup"><span data-stu-id="b0df0-254">Using other performance counters</span></span>

<span data-ttu-id="b0df0-255">Contatori delle prestazioni seguenti possono rivelarsi utili per monitorare le prestazioni dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b0df0-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="b0df0-256">**Memoria**</span><span class="sxs-lookup"><span data-stu-id="b0df0-256">**Memory**</span></span>

- <span data-ttu-id="b0df0-257">Memoria CLR .NET\\byte in tutti gli heap (per w3wp)</span><span class="sxs-lookup"><span data-stu-id="b0df0-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="b0df0-258">**ASP.NET 2.0**</span><span class="sxs-lookup"><span data-stu-id="b0df0-258">**ASP.NET**</span></span>

- <span data-ttu-id="b0df0-259">ASP.NET\Requests Current</span><span class="sxs-lookup"><span data-stu-id="b0df0-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="b0df0-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="b0df0-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="b0df0-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="b0df0-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="b0df0-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="b0df0-262">**CPU**</span></span>

- <span data-ttu-id="b0df0-263">Tempo processore Information\Processor</span><span class="sxs-lookup"><span data-stu-id="b0df0-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="b0df0-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="b0df0-264">**TCP/IP**</span></span>

- <span data-ttu-id="b0df0-265">TCPv6/connessioni stabilite</span><span class="sxs-lookup"><span data-stu-id="b0df0-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="b0df0-266">TCPv4/connessioni stabilite</span><span class="sxs-lookup"><span data-stu-id="b0df0-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="b0df0-267">**Servizio Web**</span><span class="sxs-lookup"><span data-stu-id="b0df0-267">**Web Service**</span></span>

- <span data-ttu-id="b0df0-268">Connessioni Web servizio web\connessioni correnti</span><span class="sxs-lookup"><span data-stu-id="b0df0-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="b0df0-269">Connessioni Service\Maximum Web</span><span class="sxs-lookup"><span data-stu-id="b0df0-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="b0df0-270">**Threading**</span><span class="sxs-lookup"><span data-stu-id="b0df0-270">**Threading**</span></span>

- <span data-ttu-id="b0df0-271">.NET Common Language Runtime blocca thread e\\n. di thread logici attuali</span><span class="sxs-lookup"><span data-stu-id="b0df0-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="b0df0-272">.NET Common Language Runtime blocca thread e\\thread fisici attuali</span><span class="sxs-lookup"><span data-stu-id="b0df0-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="b0df0-273">Altre risorse</span><span class="sxs-lookup"><span data-stu-id="b0df0-273">Other Resources</span></span>

<span data-ttu-id="b0df0-274">Per altre informazioni sul monitoraggio e ottimizzazione delle prestazioni di ASP.NET, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="b0df0-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="b0df0-275">[Cenni preliminari sulle prestazioni di ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="b0df0-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="b0df0-276">ASP.NET Thread Usage on IIS 7.0, IIS 7.5 e IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="b0df0-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="b0df0-277">&lt;applicationPool&gt; (impostazioni Web)</span><span class="sxs-lookup"><span data-stu-id="b0df0-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
