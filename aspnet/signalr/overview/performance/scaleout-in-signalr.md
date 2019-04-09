---
uid: signalr/overview/performance/scaleout-in-signalr
title: Introduzione alla scalabilità orizzontale in SignalR | Microsoft Docs
author: bradygaster
description: Le versioni del software utilizzato in questo argomento di Visual Studio 2013 .NET 4.5 SignalR le versioni precedenti la versione 2 di questo argomento per informazioni sulle versioni precedenti di...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 7e781fc1-1c1f-45a8-bc1d-338e96dbe9c9
msc.legacyurl: /signalr/overview/performance/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0d17308d1e97279c0870ea02933a42400ef338c9
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59411553"
---
# <a name="introduction-to-scaleout-in-signalr"></a>Introduzione a scale-out in SignalR

dal [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software utilizzate in questo argomento
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR versione 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
>
> Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).


In generale, esistono due modi per scalare un'applicazione web: *aumentare* e *scalare in orizzontale*.

- Scalare in verticale indica l'utilizzo di un server più grande (o una macchina virtuale più grande) con più di RAM, CPU e così via.
- La scalabilità orizzontale implica l'inserimento di più server per gestire il carico.

Il problema con la scalabilità verticale sia premuto rapidamente un limite per le dimensioni della macchina. Tuttavia, è necessario scalare orizzontalmente. Tuttavia, quando si scala orizzontalmente, i client possono ottenere indirizzati a server diversi. Un client connesso a un server non riceverà i messaggi inviati da un altro server.

![](scaleout-in-signalr/_static/image1.png)

Una soluzione consiste nell'inoltrare i messaggi tra server, usando un componente chiamato un' *backplane*. Con un backplane abilitato, ogni istanza dell'applicazione invia messaggi al backplane e li inoltra il backplane alle altre istanze dell'applicazione. (In elettronica, un backplane è un gruppo di connettori paralleli. Per analogia, backplane SignalR si connette più server.)

![](scaleout-in-signalr/_static/image2.png)

SignalR è attualmente forniscono i ripiani posteriori tre delle:

- **Azure Service Bus**. Service Bus è un'infrastruttura di messaggistica che consente ai componenti di inviare messaggi a regime.
- **Redis**. Redis è un archivio chiave-valore in memoria. Redis supporta un modello publish/subscribe ("pub/sub") per l'invio di messaggi.
- **SQL Server**. Il backplane di SQL Server scrive i messaggi delle tabelle SQL. Backplane utilizza Service Broker di messaggistica efficiente. Tuttavia funziona anche se Service Broker non è abilitato.

Se si distribuisce l'applicazione in Azure, è consigliabile usare il backplane di Redis usando [Cache Redis di Azure](https://azure.microsoft.com/services/cache/). Se si distribuisce per la propria server farm, è consigliabile SQL Server o i ripiani posteriori delle Redis.

Gli argomenti seguenti contengono le esercitazioni dettagliate per ogni backplane:

- [Scale-out di SignalR con il bus di servizio di Azure](scaleout-with-windows-azure-service-bus.md)
- [Scale-out di SignalR con Redis](scaleout-with-redis.md)
- [Scale-out di SignalR con SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>Implementazione

In SignalR, ogni messaggio viene inviato tramite un bus di messaggi. Un bus di messaggi implementa il [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interfaccia, che fornisce un'astrazione di pubblicazione/sottoscrizione. Sostituendo il valore predefinito di lavoro dei ripiani posteriori delle **IMessageBus** con bus progettato per il backplane. È ad esempio il bus di messaggi per Redis [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), e Usa Redis [pub/sub](http://redis.io/topics/pubsub) meccanismo per inviare e ricevere messaggi.

Ogni istanza del server si connette al backplane tramite il bus. Quando viene inviato un messaggio, passa al backplane e backplane lo invia a ogni server. Quando un server riceve un messaggio da un backplane, inserisce il messaggio nella propria cache locale. Il server recapita quindi i messaggi al client dalla cache locale.

Per ogni connessione client, lo stato di avanzamento del client nella lettura del flusso di messaggi viene monitorata mediante un cursore. (Un cursore rappresenta una posizione nel flusso di messaggi). Se un client si disconnette e quindi si riconnette, viene richiesto il bus di eventuali messaggi ricevuti dopo il valore del cursore del client. Lo stesso avviene quando si usa una connessione [polling lungo](../getting-started/introduction-to-signalr.md#transports). Dopo il completamento di una richiesta di polling lungo, il client apre una nuova connessione e richiede di messaggi ricevuti dopo il cursore.

Il funzionamento del meccanismo cursore anche se un client viene indirizzato a un server diverso in riconnessione. Backplane è a conoscenza di tutti i server e non importa quale server si connette a un client.

## <a name="limitations"></a>Limitazioni

Usa un backplane, la velocità effettiva massima dei messaggi è inferiore rispetto a quando i client comunicano direttamente con un singolo nodo server. Ciò avviene perché il backplane inoltra tutti i messaggi per ogni nodo, in modo backplane può diventare un collo di bottiglia. Se questa limitazione è un problema dipende dall'applicazione. Ad esempio, ecco alcuni scenari tipici di SignalR:

- [Trasmissione del server](../getting-started/tutorial-server-broadcast-with-signalr.md) (ad esempio, le quotazioni di borsa): I ripiani posteriori delle funziona bene per questo scenario, perché il server controlla la frequenza con cui vengono inviati i messaggi.
- [Da client a](../getting-started/tutorial-getting-started-with-signalr.md) (ad esempio, avvia una chat): In questo scenario, il backplane potrebbe essere un collo di bottiglia se il numero di messaggi viene ridimensionato con il numero di client. vale a dire, se la frequenza dei messaggi aumenta in modo i client in modo proporzionale man mano join.
- [In tempo reale ad alta frequenza](../getting-started/tutorial-high-frequency-realtime-with-signalr.md) (ad esempio i giochi in tempo reale): Backplane non è consigliato per questo scenario.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Abilitazione della traccia per la scalabilità orizzontale di SignalR

Per abilitare la traccia per il ripiani posteriori delle, aggiungere le sezioni seguenti nel file Web. config, sotto la radice **configurazione** elemento:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
