---
uid: signalr/overview/performance/signalr-performance
title: Prestazioni di SignalR Documenti Microsoft
author: bradygaster
description: Prestazioni di SignalR
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: b8a44f4c924c94cdfff1ce7630539b45fe269bbf
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676090"
---
# <a name="signalr-performance"></a><span data-ttu-id="82cc1-103">Prestazioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="82cc1-103">SignalR Performance</span></span>

<span data-ttu-id="82cc1-104">da parte [di Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="82cc1-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="82cc1-105">In questo argomento viene descritto come progettare, misurare e migliorare le prestazioni in un'applicazione SignalR.This topic describes how to design for, measure, and improve performance in a SignalR application.</span><span class="sxs-lookup"><span data-stu-id="82cc1-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="82cc1-106">Versioni software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="82cc1-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="82cc1-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="82cc1-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="82cc1-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="82cc1-108">.NET 4.5</span></span>
> - <span data-ttu-id="82cc1-109">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="82cc1-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="82cc1-110">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="82cc1-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="82cc1-111">Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR Older Versions](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="82cc1-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="82cc1-112">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="82cc1-112">Questions and comments</span></span>
>
> <span data-ttu-id="82cc1-113">Si prega di lasciare un feedback su come ti è piaciuto questo tutorial e cosa potremmo migliorare nei commenti in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="82cc1-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="82cc1-114">Se avete domande che non sono direttamente correlati al tutorial, è possibile pubblicarli al [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="82cc1-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="82cc1-115">Per una presentazione recente sulle prestazioni e il ridimensionamento di SignalR, vedere [Scalabilità del Web in tempo reale con ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="82cc1-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="82cc1-116">In questo argomento sono incluse le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="82cc1-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="82cc1-117">Considerazioni sulla progettazione</span><span class="sxs-lookup"><span data-stu-id="82cc1-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="82cc1-118">Ottimizzazione del server SignalR per le prestazioni</span><span class="sxs-lookup"><span data-stu-id="82cc1-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="82cc1-119">Risoluzione dei problemi relativi alle prestazioni</span><span class="sxs-lookup"><span data-stu-id="82cc1-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="82cc1-120">Utilizzo dei contatori delle prestazioni SignalRUsing SignalR performance counters</span><span class="sxs-lookup"><span data-stu-id="82cc1-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="82cc1-121">Utilizzo di altri contatori delle prestazioniUsing other performance counters</span><span class="sxs-lookup"><span data-stu-id="82cc1-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="82cc1-122">Altre risorse</span><span class="sxs-lookup"><span data-stu-id="82cc1-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="82cc1-123">Considerazioni sulla progettazione</span><span class="sxs-lookup"><span data-stu-id="82cc1-123">Design considerations</span></span>

<span data-ttu-id="82cc1-124">In questa sezione vengono descritti i modelli che possono essere implementati durante la progettazione di un'applicazione SignalR, per garantire che le prestazioni non vengano ostacolate dalla generazione di traffico di rete non necessario.</span><span class="sxs-lookup"><span data-stu-id="82cc1-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="82cc1-125">Frequenza dei messaggi di limitazione</span><span class="sxs-lookup"><span data-stu-id="82cc1-125">Throttling message frequency</span></span>

<span data-ttu-id="82cc1-126">Anche in un'applicazione che invia messaggi ad alta frequenza (ad esempio un'applicazione di gioco in tempo reale), la maggior parte delle applicazioni non è necessario inviare più di un secondo a più di pochi messaggi.</span><span class="sxs-lookup"><span data-stu-id="82cc1-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="82cc1-127">Per ridurre la quantità di traffico generato da ogni client, è possibile implementare un ciclo di messaggi che accoda e invia i messaggi non più frequentemente di una frequenza fissa (ovvero, fino a un determinato numero di messaggi verranno inviati ogni secondo, se sono presenti messaggi in tale intervallo di tempo da inviare).</span><span class="sxs-lookup"><span data-stu-id="82cc1-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="82cc1-128">Per un'applicazione di esempio che limita i messaggi a una determinata velocità (sia dal client che dal server), vedere [High-Frequency Realtime con SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="82cc1-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="82cc1-129">Riduzione delle dimensioni dei messaggi</span><span class="sxs-lookup"><span data-stu-id="82cc1-129">Reducing message size</span></span>

<span data-ttu-id="82cc1-130">È possibile ridurre le dimensioni di un messaggio SignalR riducendo le dimensioni degli oggetti serializzati.</span><span class="sxs-lookup"><span data-stu-id="82cc1-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="82cc1-131">Nel codice server, se si invia un oggetto che contiene proprietà che non devono essere trasmesse, `JsonIgnore` impedire che tali proprietà vengano serializzate utilizzando l'attributo .</span><span class="sxs-lookup"><span data-stu-id="82cc1-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="82cc1-132">I nomi delle proprietà vengono memorizzati anche nel messaggio; i nomi delle proprietà possono `JsonProperty` essere abbreviati utilizzando l'attributo .</span><span class="sxs-lookup"><span data-stu-id="82cc1-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="82cc1-133">Nell'esempio di codice riportato di seguito viene illustrato come escludere una proprietà dall'invio al client e come abbreviare i nomi delle proprietà:The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span><span class="sxs-lookup"><span data-stu-id="82cc1-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="82cc1-134">**Codice server .NET che illustra l'attributo JsonIgnore per escludere i dati dall'invio al client e l'attributo JsonProperty per ridurre la dimensione dei messaggi**</span><span class="sxs-lookup"><span data-stu-id="82cc1-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="82cc1-135">Per mantenere leggibilità/manutenibilità nel codice client, i nomi di proprietà abbreviati possono essere rimappati a nomi descrittivi dopo la ricezione del messaggio.</span><span class="sxs-lookup"><span data-stu-id="82cc1-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="82cc1-136">Nell'esempio di codice seguente viene illustrato un possibile modo per rimappare i nomi `reMap` abbreviati a quelli più lunghi, definendo un contratto di messaggio (mapping) e utilizzando la funzione per applicare il contratto alla classe messaggio ottimizzata:</span><span class="sxs-lookup"><span data-stu-id="82cc1-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="82cc1-137">**Codice JavaScript lato client che rimappa i nomi delle proprietà abbreviate a nomi leggibili**</span><span class="sxs-lookup"><span data-stu-id="82cc1-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="82cc1-138">I nomi possono essere abbreviati anche nei messaggi dal client al server, utilizzando lo stesso metodo.</span><span class="sxs-lookup"><span data-stu-id="82cc1-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="82cc1-139">Anche la riduzione del footprint di memoria (ovvero la quantità di memoria utilizzata per il messaggio) dell'oggetto messaggio può migliorare le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="82cc1-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="82cc1-140">Ad esempio, se l'intero intervallo di un oggetto `int` non è necessario, è possibile utilizzare un `short` o `byte` .</span><span class="sxs-lookup"><span data-stu-id="82cc1-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="82cc1-141">Poiché i messaggi vengono archiviati nel bus di messaggi nella memoria del server, la riduzione delle dimensioni dei messaggi può anche risolvere i problemi di memoria del server.</span><span class="sxs-lookup"><span data-stu-id="82cc1-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="82cc1-142">Ottimizzazione del server SignalR per le prestazioni</span><span class="sxs-lookup"><span data-stu-id="82cc1-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="82cc1-143">Le seguenti impostazioni di configurazione possono essere utilizzate per ottimizzare il server per migliorare le prestazioni in un'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="82cc1-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="82cc1-144">Per informazioni generali su come migliorare le prestazioni in un'applicazione ASP.NET, vedere [Miglioramento delle prestazioni di ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="82cc1-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="82cc1-145">**Impostazioni di configurazione SignalR**</span><span class="sxs-lookup"><span data-stu-id="82cc1-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="82cc1-146">**DefaultMessageBufferSize:** per impostazione predefinita, SignalR mantiene 1000 messaggi in memoria per hub per ogni connessione.</span><span class="sxs-lookup"><span data-stu-id="82cc1-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="82cc1-147">Se vengono utilizzati messaggi di grandi dimensioni, ciò può creare problemi di memoria che possono essere risolti riducendo questo valore.</span><span class="sxs-lookup"><span data-stu-id="82cc1-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="82cc1-148">Questa impostazione può `Application_Start` essere impostata nel gestore `Configuration` eventi in un'applicazione ASP.NET o nel metodo di una classe di avvio OWIN in un'applicazione indipendente.</span><span class="sxs-lookup"><span data-stu-id="82cc1-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="82cc1-149">Nell'esempio seguente viene illustrato come ridurre questo valore per ridurre il footprint di memoria dell'applicazione per ridurre la quantità di memoria del server utilizzata:</span><span class="sxs-lookup"><span data-stu-id="82cc1-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="82cc1-150">**Codice server .NET in Startup.cs per ridurre la dimensione predefinita del buffer dei messaggi**</span><span class="sxs-lookup"><span data-stu-id="82cc1-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="82cc1-151">**Impostazioni di configurazione di IIS**</span><span class="sxs-lookup"><span data-stu-id="82cc1-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="82cc1-152">Numero massimo di **richieste simultanee per applicazione:** l'aumento del numero di richieste IIS simultanee aumenterà le risorse del server disponibili per la gestione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="82cc1-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="82cc1-153">Il valore predefinito è 5000; per aumentare questa impostazione, eseguire i seguenti comandi in un prompt dei comandi con privilegi elevati:</span><span class="sxs-lookup"><span data-stu-id="82cc1-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="82cc1-154">**ApplicationPool QueueLength:** indica il numero massimo di richieste che Http.sys mette in coda per il pool di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="82cc1-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="82cc1-155">Quando la coda è piena, le nuove richieste ricevono una risposta 503 "Servizio non disponibile".</span><span class="sxs-lookup"><span data-stu-id="82cc1-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="82cc1-156">Il valore predefinito è 1000.</span><span class="sxs-lookup"><span data-stu-id="82cc1-156">The default value is 1000.</span></span>

    <span data-ttu-id="82cc1-157">Abbreviare la lunghezza della coda per il processo di lavoro nel pool di applicazioni che ospita l'applicazione conserverà risorse di memoria.</span><span class="sxs-lookup"><span data-stu-id="82cc1-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="82cc1-158">Per ulteriori informazioni, vedere [Gestione, ottimizzazione e configurazione dei pool](https://technet.microsoft.com/library/cc745955.aspx)di applicazioni .</span><span class="sxs-lookup"><span data-stu-id="82cc1-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="82cc1-159">**ASP.NET impostazioni di configurazione**</span><span class="sxs-lookup"><span data-stu-id="82cc1-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="82cc1-160">Questa sezione include le impostazioni di `aspnet.config` configurazione che possono essere impostate nel file.</span><span class="sxs-lookup"><span data-stu-id="82cc1-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="82cc1-161">Questo file si trova in una delle due posizioni, a seconda della piattaforma:</span><span class="sxs-lookup"><span data-stu-id="82cc1-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="82cc1-162">ASP.NET le impostazioni che possono migliorare le prestazioni di SignalR includono quanto segue:</span><span class="sxs-lookup"><span data-stu-id="82cc1-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="82cc1-163">Numero massimo di **richieste simultanee per CPU:** l'aumento di questa impostazione può ridurre i colli di bottiglia delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="82cc1-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="82cc1-164">Per aumentare questa impostazione, aggiungere `aspnet.config` la seguente impostazione di configurazione al file:</span><span class="sxs-lookup"><span data-stu-id="82cc1-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="82cc1-165">**Limite coda richiesta**: quando il numero `maxConcurrentRequestsPerCPU` totale di connessioni supera l'impostazione, ASP.NET inizierà a limitare le richieste utilizzando una coda.</span><span class="sxs-lookup"><span data-stu-id="82cc1-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="82cc1-166">Per aumentare le dimensioni della coda, è possibile aumentare l'impostazione. `requestQueueLimit`</span><span class="sxs-lookup"><span data-stu-id="82cc1-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="82cc1-167">A tale scopo, aggiungere la `processModel` seguente `config/machine.config` impostazione `aspnet.config`di configurazione al nodo in (anziché)</span><span class="sxs-lookup"><span data-stu-id="82cc1-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="82cc1-168">Risoluzione dei problemi relativi alle prestazioni</span><span class="sxs-lookup"><span data-stu-id="82cc1-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="82cc1-169">In questa sezione vengono descritti i modi per individuare i colli di bottiglia delle prestazioni nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="82cc1-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="82cc1-170">Verifica dell'utilizzo di WebSocket</span><span class="sxs-lookup"><span data-stu-id="82cc1-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="82cc1-171">Mentre SignalR può utilizzare una varietà di trasporti per la comunicazione tra client e server, WebSocket offre un vantaggio significativo in termini di prestazioni e deve essere utilizzato se il client e il server lo supportano.</span><span class="sxs-lookup"><span data-stu-id="82cc1-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="82cc1-172">Per determinare se il client e il server soddisfano i requisiti per WebSocket, vedere [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="82cc1-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="82cc1-173">Per determinare quale trasporto viene utilizzato nell'applicazione, è possibile utilizzare gli strumenti di sviluppo del browser ed esaminare i log per vedere quale trasporto viene utilizzato per la connessione.</span><span class="sxs-lookup"><span data-stu-id="82cc1-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="82cc1-174">Per informazioni sull'utilizzo degli strumenti di sviluppo del browser in Internet Explorer e Chrome, consultate [Trasporti e fallback.](../getting-started/introduction-to-signalr.md#transports)</span><span class="sxs-lookup"><span data-stu-id="82cc1-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="82cc1-175">Utilizzo dei contatori delle prestazioni SignalRUsing SignalR performance counters</span><span class="sxs-lookup"><span data-stu-id="82cc1-175">Using SignalR performance counters</span></span>

<span data-ttu-id="82cc1-176">In questa sezione viene descritto come abilitare e utilizzare `Microsoft.AspNet.SignalR.Utils` i contatori delle prestazioni SignalR, disponibili nel pacchetto.</span><span class="sxs-lookup"><span data-stu-id="82cc1-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="82cc1-177">Installazione di signalr.exe</span><span class="sxs-lookup"><span data-stu-id="82cc1-177">Installing signalr.exe</span></span>

<span data-ttu-id="82cc1-178">I contatori delle prestazioni possono essere aggiunti al server utilizzando un'utilità denominata SignalR.exe.Performance counters can be added to the server using a utility called SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="82cc1-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="82cc1-179">Per installare questa utilità, attenersi alla seguente procedura:</span><span class="sxs-lookup"><span data-stu-id="82cc1-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="82cc1-180">In Visual Studio selezionare **Strumenti** > **di Gestione** > pacchetti NuGet Gestisci pacchetti**NuGet per soluzione**</span><span class="sxs-lookup"><span data-stu-id="82cc1-180">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="82cc1-181">Cercare **signalr.utils**e selezionare Installa.</span><span class="sxs-lookup"><span data-stu-id="82cc1-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="82cc1-182">Accettare il contratto di licenza per installare il pacchetto.</span><span class="sxs-lookup"><span data-stu-id="82cc1-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="82cc1-183">SignalR.exe verrà installato `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`in .</span><span class="sxs-lookup"><span data-stu-id="82cc1-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="82cc1-184">Installazione dei contatori delle prestazioni con SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="82cc1-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="82cc1-185">Per installare i contatori delle prestazioni SignalR, eseguire SignalR.exe in un prompt dei comandi con privilegi elevati con il seguente parametro:</span><span class="sxs-lookup"><span data-stu-id="82cc1-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="82cc1-186">Per rimuovere i contatori delle prestazioni SignalR, eseguire SignalR.exe in un prompt dei comandi con privilegi elevati con il seguente parametro:</span><span class="sxs-lookup"><span data-stu-id="82cc1-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="82cc1-187">Contatori delle prestazioni SignalR</span><span class="sxs-lookup"><span data-stu-id="82cc1-187">SignalR Performance counters</span></span>

<span data-ttu-id="82cc1-188">Il pacchetto di utilità installa i seguenti contatori delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="82cc1-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="82cc1-189">I contatori "Totale" misurano il numero di eventi dall'ultimo riavvio del pool di applicazioni o del server.</span><span class="sxs-lookup"><span data-stu-id="82cc1-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="82cc1-190">**Metriche di connessione**</span><span class="sxs-lookup"><span data-stu-id="82cc1-190">**Connection metrics**</span></span>

<span data-ttu-id="82cc1-191">Le metriche seguenti misurano gli eventi di durata della connessione che si verificano.</span><span class="sxs-lookup"><span data-stu-id="82cc1-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="82cc1-192">Per ulteriori informazioni, vedere [Informazioni e gestione degli eventi della durata](../guide-to-the-api/handling-connection-lifetime-events.md)delle connessioni .</span><span class="sxs-lookup"><span data-stu-id="82cc1-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="82cc1-193">**Connessioni connesse**</span><span class="sxs-lookup"><span data-stu-id="82cc1-193">**Connections Connected**</span></span>
- <span data-ttu-id="82cc1-194">**Connessioni riconnesse**</span><span class="sxs-lookup"><span data-stu-id="82cc1-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="82cc1-195">**Connessioni disconnesse**</span><span class="sxs-lookup"><span data-stu-id="82cc1-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="82cc1-196">**Connessioni correnti**</span><span class="sxs-lookup"><span data-stu-id="82cc1-196">**Connections Current**</span></span>

<span data-ttu-id="82cc1-197">**Metriche per i messaggi**</span><span class="sxs-lookup"><span data-stu-id="82cc1-197">**Message metrics**</span></span>

<span data-ttu-id="82cc1-198">Le metriche seguenti misurano il traffico dei messaggi generato da SignalR.</span><span class="sxs-lookup"><span data-stu-id="82cc1-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="82cc1-199">**Totale messaggi di connessione ricevuti**</span><span class="sxs-lookup"><span data-stu-id="82cc1-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="82cc1-200">**Totale messaggi di connessione inviati**</span><span class="sxs-lookup"><span data-stu-id="82cc1-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="82cc1-201">**Messaggi di connessione ricevuti/secConnection Messages Received/Sec**</span><span class="sxs-lookup"><span data-stu-id="82cc1-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="82cc1-202">**Messaggi di connessione inviati al secondo**</span><span class="sxs-lookup"><span data-stu-id="82cc1-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="82cc1-203">**Metriche del bus dei messaggi**</span><span class="sxs-lookup"><span data-stu-id="82cc1-203">**Message bus metrics**</span></span>

<span data-ttu-id="82cc1-204">Le metriche seguenti misurano il traffico attraverso il bus di messaggi SignalR interno, la coda in cui vengono inseriti tutti i messaggi SignalR in ingresso e in uscita.</span><span class="sxs-lookup"><span data-stu-id="82cc1-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="82cc1-205">Un messaggio viene **pubblicato** quando viene inviato o trasmesso.</span><span class="sxs-lookup"><span data-stu-id="82cc1-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="82cc1-206">Un **Sottoscrittore** in questo contesto è una sottoscrizione nel bus di messaggi. questo dovrebbe essere uguale al numero di client più il server stesso.</span><span class="sxs-lookup"><span data-stu-id="82cc1-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="82cc1-207">Un **worker allocato** è un componente che invia dati alle connessioni attive; un **lavoratore occupato** è uno che sta inviando attivamente un messaggio.</span><span class="sxs-lookup"><span data-stu-id="82cc1-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="82cc1-208">**Totale messaggi del bus dei messaggi ricevuti**</span><span class="sxs-lookup"><span data-stu-id="82cc1-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="82cc1-209">**Messaggi del bus di messaggi ricevuti/secMessage Bus Messages Received/Sec**</span><span class="sxs-lookup"><span data-stu-id="82cc1-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="82cc1-210">**Totale messaggi del bus di messaggi pubblicati**</span><span class="sxs-lookup"><span data-stu-id="82cc1-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="82cc1-211">**Messaggi del bus di messaggi pubblicati al secondoMessage Bus Messages Published/sec**</span><span class="sxs-lookup"><span data-stu-id="82cc1-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="82cc1-212">**Sottoscrittori bus di messaggi correntiMessage Bus Subscribers Current**</span><span class="sxs-lookup"><span data-stu-id="82cc1-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="82cc1-213">**Totale sottoscrittori bus di messaggi**</span><span class="sxs-lookup"><span data-stu-id="82cc1-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="82cc1-214">**Sottoscrittori bus di messaggi/secMessage Bus Subscribers/Sec**</span><span class="sxs-lookup"><span data-stu-id="82cc1-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="82cc1-215">**Lavoratori allocati del bus di messaggiMessage Bus Allocated Workers**</span><span class="sxs-lookup"><span data-stu-id="82cc1-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="82cc1-216">**Lavoratori occupati del bus dei messaggi**</span><span class="sxs-lookup"><span data-stu-id="82cc1-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="82cc1-217">**Argomenti del bus di messaggi correnti**</span><span class="sxs-lookup"><span data-stu-id="82cc1-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="82cc1-218">**Metriche di errore**</span><span class="sxs-lookup"><span data-stu-id="82cc1-218">**Error metrics**</span></span>

<span data-ttu-id="82cc1-219">Le metriche seguenti misurano gli errori generati dal traffico dei messaggi SignalR.The following metrics measure errors generated by SignalR message traffic.</span><span class="sxs-lookup"><span data-stu-id="82cc1-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="82cc1-220">Gli errori di **risoluzione dell'hub** si verificano quando non è possibile risolvere un metodo hub o hub.</span><span class="sxs-lookup"><span data-stu-id="82cc1-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="82cc1-221">Gli errori **di chiamata dell'hub** sono eccezioni generate durante la chiamata di un metodo hub.</span><span class="sxs-lookup"><span data-stu-id="82cc1-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="82cc1-222">**Gli** errori di trasporto sono errori di connessione generati durante una richiesta o una risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="82cc1-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="82cc1-223">**Errori: Tutto totale**</span><span class="sxs-lookup"><span data-stu-id="82cc1-223">**Errors: All Total**</span></span>
- <span data-ttu-id="82cc1-224">**Errori: Tutti/sec**</span><span class="sxs-lookup"><span data-stu-id="82cc1-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="82cc1-225">**Errori: totale risoluzione hubErrors: Hub Resolution Total**</span><span class="sxs-lookup"><span data-stu-id="82cc1-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="82cc1-226">**Errori: Risoluzione hub/secErrors: Hub Resolution/Sec**</span><span class="sxs-lookup"><span data-stu-id="82cc1-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="82cc1-227">**Errori: Totale chiamata hubErrors: Hub Invocation Total**</span><span class="sxs-lookup"><span data-stu-id="82cc1-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="82cc1-228">**Errori: Chiamata hub/secErrors: Hub Invocation/Sec**</span><span class="sxs-lookup"><span data-stu-id="82cc1-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="82cc1-229">**Errori: Totale trasporto**</span><span class="sxs-lookup"><span data-stu-id="82cc1-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="82cc1-230">**Errori: trasporto/secErrors: Transport/Sec**</span><span class="sxs-lookup"><span data-stu-id="82cc1-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="82cc1-231">**Metriche di scalabilità orizzontaleScaleout metrics**</span><span class="sxs-lookup"><span data-stu-id="82cc1-231">**Scaleout metrics**</span></span>

<span data-ttu-id="82cc1-232">Le metriche seguenti misurano il traffico e gli errori generati dal provider di scalabilità orizzontale.</span><span class="sxs-lookup"><span data-stu-id="82cc1-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="82cc1-233">Un **flusso** in questo contesto è un'unità di scala utilizzata dal provider di scalabilità orizzontale. si tratta di una tabella se viene utilizzato SQL Server, un argomento se viene utilizzato il bus di servizio e una sottoscrizione se viene utilizzata Redis.This is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span><span class="sxs-lookup"><span data-stu-id="82cc1-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="82cc1-234">Ogni flusso garantisce operazioni di lettura e scrittura ordinate; un singolo flusso è un potenziale collo di bottiglia di scalabilità, pertanto il numero di flussi può essere aumentato per ridurre tale collo di bottiglia.</span><span class="sxs-lookup"><span data-stu-id="82cc1-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="82cc1-235">Se vengono utilizzati più flussi, SignalR distribuirà automaticamente i messaggi (shard) tra questi flussi in modo da garantire che i messaggi inviati da una determinata connessione siano in ordine.</span><span class="sxs-lookup"><span data-stu-id="82cc1-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="82cc1-236">L'impostazione [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) controlla la lunghezza della coda di invio con scalabilità orizzontale gestita da SignalR.</span><span class="sxs-lookup"><span data-stu-id="82cc1-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="82cc1-237">Se si imposta un valore maggiore di 0, tutti i messaggi di una coda di invio verranno inviati uno alla volta al backplane di messaggistica configurato.</span><span class="sxs-lookup"><span data-stu-id="82cc1-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="82cc1-238">Se la dimensione della coda supera la lunghezza configurata, le chiamate successive all'invio avranno esito negativo immediatamente con un [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) fino a quando il numero di messaggi nella coda è inferiore all'impostazione nuovamente.</span><span class="sxs-lookup"><span data-stu-id="82cc1-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="82cc1-239">L'accodamento è disabilitato per impostazione predefinita perché i backplane implementati in genere dispongono di un proprio controllo di accodamento o di flusso.</span><span class="sxs-lookup"><span data-stu-id="82cc1-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="82cc1-240">Nel caso di SQL Server, il pool di connessioni limita in modo efficace il numero di invii in corso in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="82cc1-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="82cc1-241">Per impostazione predefinita, per SQL Server sql Server e Redis vengono utilizzati cinque flussi e l'accodamento è disabilitato, ma queste impostazioni possono essere modificate tramite la configurazione in SQL Server SQL Server e il bus di servizio:By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span><span class="sxs-lookup"><span data-stu-id="82cc1-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="82cc1-242">**Codice server .NET per la configurazione del numero di tabelle e della lunghezza della coda per il backplane di SQL Server**</span><span class="sxs-lookup"><span data-stu-id="82cc1-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="82cc1-243">**Codice server .NET per la configurazione del numero di argomenti e della lunghezza della coda per il backplane del bus di servizio**</span><span class="sxs-lookup"><span data-stu-id="82cc1-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="82cc1-244">Un flusso **di buffering** è uno che è entrato in uno stato di errore; quando il flusso è in stato di errore, tutti i messaggi inviati al backplane avranno esito negativo immediatamente fino a quando il flusso non è più in errore.</span><span class="sxs-lookup"><span data-stu-id="82cc1-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="82cc1-245">Lunghezza **coda di invio** indica il numero di messaggi che sono stati inviati ma non ancora inviati.</span><span class="sxs-lookup"><span data-stu-id="82cc1-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="82cc1-246">**Scalabilità orizzontale dei messaggi del bus di messaggi ricevuti/secScaleout Message Bus Messages Received/Sec**</span><span class="sxs-lookup"><span data-stu-id="82cc1-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="82cc1-247">**Totale flussi di scalabilità orizzontaleScaleout Streams Total**</span><span class="sxs-lookup"><span data-stu-id="82cc1-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="82cc1-248">**Flussi di scalabilità orizzontale apertiScaleout Streams Open**</span><span class="sxs-lookup"><span data-stu-id="82cc1-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="82cc1-249">**Buffering dei flussi di scalabilità orizzontaleScaleout Streams Buffering**</span><span class="sxs-lookup"><span data-stu-id="82cc1-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="82cc1-250">**Totale errori di scalabilità orizzontaleScaleout Errors Total**</span><span class="sxs-lookup"><span data-stu-id="82cc1-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="82cc1-251">**Errori di scalabilità orizzontale/secScaleout Errors/Sec**</span><span class="sxs-lookup"><span data-stu-id="82cc1-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="82cc1-252">**Lunghezza coda di invio con scalabilità orizzontaleScaleout Send Queue Length**</span><span class="sxs-lookup"><span data-stu-id="82cc1-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="82cc1-253">Per altre informazioni su quali contatori vengono misurati, vedere [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="82cc1-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="82cc1-254">Utilizzo di altri contatori delle prestazioniUsing other performance counters</span><span class="sxs-lookup"><span data-stu-id="82cc1-254">Using other performance counters</span></span>

<span data-ttu-id="82cc1-255">I contatori delle prestazioni seguenti possono essere utili anche per monitorare le prestazioni dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="82cc1-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="82cc1-256">**Memoria**</span><span class="sxs-lookup"><span data-stu-id="82cc1-256">**Memory**</span></span>

- <span data-ttu-id="82cc1-257">Memoria\\CLR .NET - byte in tutti gli heap (per w3wp)</span><span class="sxs-lookup"><span data-stu-id="82cc1-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="82cc1-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="82cc1-258">**ASP.NET**</span></span>

- <span data-ttu-id="82cc1-259">ASP.NET: richieste correnti</span><span class="sxs-lookup"><span data-stu-id="82cc1-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="82cc1-260">ASP.NET: in coda</span><span class="sxs-lookup"><span data-stu-id="82cc1-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="82cc1-261">ASP.NET: rifiutato</span><span class="sxs-lookup"><span data-stu-id="82cc1-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="82cc1-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="82cc1-262">**CPU**</span></span>

- <span data-ttu-id="82cc1-263">Informazioni processore: tempo processore</span><span class="sxs-lookup"><span data-stu-id="82cc1-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="82cc1-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="82cc1-264">**TCP/IP**</span></span>

- <span data-ttu-id="82cc1-265">TCPv6/Connessioni stabilite</span><span class="sxs-lookup"><span data-stu-id="82cc1-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="82cc1-266">TCPv4/Connessioni stabilite</span><span class="sxs-lookup"><span data-stu-id="82cc1-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="82cc1-267">**Servizio Web**</span><span class="sxs-lookup"><span data-stu-id="82cc1-267">**Web Service**</span></span>

- <span data-ttu-id="82cc1-268">Servizio Web - Connessioni correnti</span><span class="sxs-lookup"><span data-stu-id="82cc1-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="82cc1-269">Servizio Web - Numero massimo di connessioni</span><span class="sxs-lookup"><span data-stu-id="82cc1-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="82cc1-270">**Threading**</span><span class="sxs-lookup"><span data-stu-id="82cc1-270">**Threading**</span></span>

- <span data-ttu-id="82cc1-271">Blocchi e thread\\CLR .NET delle thread logiche correnti</span><span class="sxs-lookup"><span data-stu-id="82cc1-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="82cc1-272">Blocchi e thread\\CLR .NET : dei thread fisici correnti</span><span class="sxs-lookup"><span data-stu-id="82cc1-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="82cc1-273">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="82cc1-273">Other Resources</span></span>

<span data-ttu-id="82cc1-274">Per altre informazioni sul monitoraggio e l'ottimizzazione delle prestazioni di ASP.NET, vedere gli argomenti seguenti:For more information on performance monitoring and tuning, see the following topics:</span><span class="sxs-lookup"><span data-stu-id="82cc1-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="82cc1-275">[Cenni preliminari sulle prestazioni di ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="82cc1-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="82cc1-276">ASP.NET utilizzo dei thread in IIS 7.5, IIS 7.0 e IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="82cc1-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="82cc1-277">&lt;Elemento&gt; applicationPool (impostazioni Web)</span><span class="sxs-lookup"><span data-stu-id="82cc1-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
