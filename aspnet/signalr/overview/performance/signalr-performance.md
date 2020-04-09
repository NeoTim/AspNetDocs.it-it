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
# <a name="signalr-performance"></a>Prestazioni di SignalR

da parte [di Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In questo argomento viene descritto come progettare, misurare e migliorare le prestazioni in un'applicazione SignalR.This topic describes how to design for, measure, and improve performance in a SignalR application.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versioni software utilizzate in questo argomento
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
> Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR Older Versions](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Si prega di lasciare un feedback su come ti è piaciuto questo tutorial e cosa potremmo migliorare nei commenti in fondo alla pagina. Se avete domande che non sono direttamente correlati al tutorial, è possibile pubblicarli al [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).

Per una presentazione recente sulle prestazioni e il ridimensionamento di SignalR, vedere [Scalabilità del Web in tempo reale con ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).

In questo argomento sono incluse le sezioni seguenti:

- [Considerazioni sulla progettazione](#design)
- [Ottimizzazione del server SignalR per le prestazioni](#tuning)
- [Risoluzione dei problemi relativi alle prestazioni](#troubleshooting)
- [Utilizzo dei contatori delle prestazioni SignalRUsing SignalR performance counters](#perfcounters)
- [Utilizzo di altri contatori delle prestazioniUsing other performance counters](#othercounters)
- [Altre risorse](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Considerazioni sulla progettazione

In questa sezione vengono descritti i modelli che possono essere implementati durante la progettazione di un'applicazione SignalR, per garantire che le prestazioni non vengano ostacolate dalla generazione di traffico di rete non necessario.

### <a name="throttling-message-frequency"></a>Frequenza dei messaggi di limitazione

Anche in un'applicazione che invia messaggi ad alta frequenza (ad esempio un'applicazione di gioco in tempo reale), la maggior parte delle applicazioni non è necessario inviare più di un secondo a più di pochi messaggi. Per ridurre la quantità di traffico generato da ogni client, è possibile implementare un ciclo di messaggi che accoda e invia i messaggi non più frequentemente di una frequenza fissa (ovvero, fino a un determinato numero di messaggi verranno inviati ogni secondo, se sono presenti messaggi in tale intervallo di tempo da inviare). Per un'applicazione di esempio che limita i messaggi a una determinata velocità (sia dal client che dal server), vedere [High-Frequency Realtime con SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Riduzione delle dimensioni dei messaggi

È possibile ridurre le dimensioni di un messaggio SignalR riducendo le dimensioni degli oggetti serializzati. Nel codice server, se si invia un oggetto che contiene proprietà che non devono essere trasmesse, `JsonIgnore` impedire che tali proprietà vengano serializzate utilizzando l'attributo . I nomi delle proprietà vengono memorizzati anche nel messaggio; i nomi delle proprietà possono `JsonProperty` essere abbreviati utilizzando l'attributo . Nell'esempio di codice riportato di seguito viene illustrato come escludere una proprietà dall'invio al client e come abbreviare i nomi delle proprietà:The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:

**Codice server .NET che illustra l'attributo JsonIgnore per escludere i dati dall'invio al client e l'attributo JsonProperty per ridurre la dimensione dei messaggi**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Per mantenere leggibilità/manutenibilità nel codice client, i nomi di proprietà abbreviati possono essere rimappati a nomi descrittivi dopo la ricezione del messaggio. Nell'esempio di codice seguente viene illustrato un possibile modo per rimappare i nomi `reMap` abbreviati a quelli più lunghi, definendo un contratto di messaggio (mapping) e utilizzando la funzione per applicare il contratto alla classe messaggio ottimizzata:

**Codice JavaScript lato client che rimappa i nomi delle proprietà abbreviate a nomi leggibili**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

I nomi possono essere abbreviati anche nei messaggi dal client al server, utilizzando lo stesso metodo.

Anche la riduzione del footprint di memoria (ovvero la quantità di memoria utilizzata per il messaggio) dell'oggetto messaggio può migliorare le prestazioni. Ad esempio, se l'intero intervallo di un oggetto `int` non è necessario, è possibile utilizzare un `short` o `byte` .

Poiché i messaggi vengono archiviati nel bus di messaggi nella memoria del server, la riduzione delle dimensioni dei messaggi può anche risolvere i problemi di memoria del server.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Ottimizzazione del server SignalR per le prestazioni

Le seguenti impostazioni di configurazione possono essere utilizzate per ottimizzare il server per migliorare le prestazioni in un'applicazione SignalR. Per informazioni generali su come migliorare le prestazioni in un'applicazione ASP.NET, vedere [Miglioramento delle prestazioni di ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).

**Impostazioni di configurazione SignalR**

- **DefaultMessageBufferSize:** per impostazione predefinita, SignalR mantiene 1000 messaggi in memoria per hub per ogni connessione. Se vengono utilizzati messaggi di grandi dimensioni, ciò può creare problemi di memoria che possono essere risolti riducendo questo valore. Questa impostazione può `Application_Start` essere impostata nel gestore `Configuration` eventi in un'applicazione ASP.NET o nel metodo di una classe di avvio OWIN in un'applicazione indipendente. Nell'esempio seguente viene illustrato come ridurre questo valore per ridurre il footprint di memoria dell'applicazione per ridurre la quantità di memoria del server utilizzata:

    **Codice server .NET in Startup.cs per ridurre la dimensione predefinita del buffer dei messaggi**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**Impostazioni di configurazione di IIS**

- Numero massimo di **richieste simultanee per applicazione:** l'aumento del numero di richieste IIS simultanee aumenterà le risorse del server disponibili per la gestione delle richieste. Il valore predefinito è 5000; per aumentare questa impostazione, eseguire i seguenti comandi in un prompt dei comandi con privilegi elevati:

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- **ApplicationPool QueueLength:** indica il numero massimo di richieste che Http.sys mette in coda per il pool di applicazioni. Quando la coda è piena, le nuove richieste ricevono una risposta 503 "Servizio non disponibile". Il valore predefinito è 1000.

    Abbreviare la lunghezza della coda per il processo di lavoro nel pool di applicazioni che ospita l'applicazione conserverà risorse di memoria. Per ulteriori informazioni, vedere [Gestione, ottimizzazione e configurazione dei pool](https://technet.microsoft.com/library/cc745955.aspx)di applicazioni .

**ASP.NET impostazioni di configurazione**

Questa sezione include le impostazioni di `aspnet.config` configurazione che possono essere impostate nel file. Questo file si trova in una delle due posizioni, a seconda della piattaforma:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

ASP.NET le impostazioni che possono migliorare le prestazioni di SignalR includono quanto segue:

- Numero massimo di **richieste simultanee per CPU:** l'aumento di questa impostazione può ridurre i colli di bottiglia delle prestazioni. Per aumentare questa impostazione, aggiungere `aspnet.config` la seguente impostazione di configurazione al file:

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Limite coda richiesta**: quando il numero `maxConcurrentRequestsPerCPU` totale di connessioni supera l'impostazione, ASP.NET inizierà a limitare le richieste utilizzando una coda. Per aumentare le dimensioni della coda, è possibile aumentare l'impostazione. `requestQueueLimit` A tale scopo, aggiungere la `processModel` seguente `config/machine.config` impostazione `aspnet.config`di configurazione al nodo in (anziché)

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Risoluzione dei problemi relativi alle prestazioni

In questa sezione vengono descritti i modi per individuare i colli di bottiglia delle prestazioni nell'applicazione.

### <a name="verifying-that-websocket-is-being-used"></a>Verifica dell'utilizzo di WebSocket

Mentre SignalR può utilizzare una varietà di trasporti per la comunicazione tra client e server, WebSocket offre un vantaggio significativo in termini di prestazioni e deve essere utilizzato se il client e il server lo supportano. Per determinare se il client e il server soddisfano i requisiti per WebSocket, vedere [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports). Per determinare quale trasporto viene utilizzato nell'applicazione, è possibile utilizzare gli strumenti di sviluppo del browser ed esaminare i log per vedere quale trasporto viene utilizzato per la connessione. Per informazioni sull'utilizzo degli strumenti di sviluppo del browser in Internet Explorer e Chrome, consultate [Trasporti e fallback.](../getting-started/introduction-to-signalr.md#transports)

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>Utilizzo dei contatori delle prestazioni SignalRUsing SignalR performance counters

In questa sezione viene descritto come abilitare e utilizzare `Microsoft.AspNet.SignalR.Utils` i contatori delle prestazioni SignalR, disponibili nel pacchetto.

### <a name="installing-signalrexe"></a>Installazione di signalr.exe

I contatori delle prestazioni possono essere aggiunti al server utilizzando un'utilità denominata SignalR.exe.Performance counters can be added to the server using a utility called SignalR.exe. Per installare questa utilità, attenersi alla seguente procedura:

1. In Visual Studio selezionare **Strumenti** > **di Gestione** > pacchetti NuGet Gestisci pacchetti**NuGet per soluzione**
2. Cercare **signalr.utils**e selezionare Installa.

    ![](signalr-performance/_static/image1.png)
3. Accettare il contratto di licenza per installare il pacchetto.
4. SignalR.exe verrà installato `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`in .

### <a name="installing-performance-counters-with-signalrexe"></a>Installazione dei contatori delle prestazioni con SignalR.exe

Per installare i contatori delle prestazioni SignalR, eseguire SignalR.exe in un prompt dei comandi con privilegi elevati con il seguente parametro:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Per rimuovere i contatori delle prestazioni SignalR, eseguire SignalR.exe in un prompt dei comandi con privilegi elevati con il seguente parametro:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>Contatori delle prestazioni SignalR

Il pacchetto di utilità installa i seguenti contatori delle prestazioni. I contatori "Totale" misurano il numero di eventi dall'ultimo riavvio del pool di applicazioni o del server.

**Metriche di connessione**

Le metriche seguenti misurano gli eventi di durata della connessione che si verificano. Per ulteriori informazioni, vedere [Informazioni e gestione degli eventi della durata](../guide-to-the-api/handling-connection-lifetime-events.md)delle connessioni .

- **Connessioni connesse**
- **Connessioni riconnesse**
- **Connessioni disconnesse**
- **Connessioni correnti**

**Metriche per i messaggi**

Le metriche seguenti misurano il traffico dei messaggi generato da SignalR.

- **Totale messaggi di connessione ricevuti**
- **Totale messaggi di connessione inviati**
- **Messaggi di connessione ricevuti/secConnection Messages Received/Sec**
- **Messaggi di connessione inviati al secondo**

**Metriche del bus dei messaggi**

Le metriche seguenti misurano il traffico attraverso il bus di messaggi SignalR interno, la coda in cui vengono inseriti tutti i messaggi SignalR in ingresso e in uscita. Un messaggio viene **pubblicato** quando viene inviato o trasmesso. Un **Sottoscrittore** in questo contesto è una sottoscrizione nel bus di messaggi. questo dovrebbe essere uguale al numero di client più il server stesso. Un **worker allocato** è un componente che invia dati alle connessioni attive; un **lavoratore occupato** è uno che sta inviando attivamente un messaggio.

- **Totale messaggi del bus dei messaggi ricevuti**
- **Messaggi del bus di messaggi ricevuti/secMessage Bus Messages Received/Sec**
- **Totale messaggi del bus di messaggi pubblicati**
- **Messaggi del bus di messaggi pubblicati al secondoMessage Bus Messages Published/sec**
- **Sottoscrittori bus di messaggi correntiMessage Bus Subscribers Current**
- **Totale sottoscrittori bus di messaggi**
- **Sottoscrittori bus di messaggi/secMessage Bus Subscribers/Sec**
- **Lavoratori allocati del bus di messaggiMessage Bus Allocated Workers**
- **Lavoratori occupati del bus dei messaggi**
- **Argomenti del bus di messaggi correnti**

**Metriche di errore**

Le metriche seguenti misurano gli errori generati dal traffico dei messaggi SignalR.The following metrics measure errors generated by SignalR message traffic. Gli errori di **risoluzione dell'hub** si verificano quando non è possibile risolvere un metodo hub o hub. Gli errori **di chiamata dell'hub** sono eccezioni generate durante la chiamata di un metodo hub. **Gli** errori di trasporto sono errori di connessione generati durante una richiesta o una risposta HTTP.

- **Errori: Tutto totale**
- **Errori: Tutti/sec**
- **Errori: totale risoluzione hubErrors: Hub Resolution Total**
- **Errori: Risoluzione hub/secErrors: Hub Resolution/Sec**
- **Errori: Totale chiamata hubErrors: Hub Invocation Total**
- **Errori: Chiamata hub/secErrors: Hub Invocation/Sec**
- **Errori: Totale trasporto**
- **Errori: trasporto/secErrors: Transport/Sec**

<a id="scaleout_metrics"></a>

**Metriche di scalabilità orizzontaleScaleout metrics**

Le metriche seguenti misurano il traffico e gli errori generati dal provider di scalabilità orizzontale. Un **flusso** in questo contesto è un'unità di scala utilizzata dal provider di scalabilità orizzontale. si tratta di una tabella se viene utilizzato SQL Server, un argomento se viene utilizzato il bus di servizio e una sottoscrizione se viene utilizzata Redis.This is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used. Ogni flusso garantisce operazioni di lettura e scrittura ordinate; un singolo flusso è un potenziale collo di bottiglia di scalabilità, pertanto il numero di flussi può essere aumentato per ridurre tale collo di bottiglia. Se vengono utilizzati più flussi, SignalR distribuirà automaticamente i messaggi (shard) tra questi flussi in modo da garantire che i messaggi inviati da una determinata connessione siano in ordine.

L'impostazione [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) controlla la lunghezza della coda di invio con scalabilità orizzontale gestita da SignalR. Se si imposta un valore maggiore di 0, tutti i messaggi di una coda di invio verranno inviati uno alla volta al backplane di messaggistica configurato. Se la dimensione della coda supera la lunghezza configurata, le chiamate successive all'invio avranno esito negativo immediatamente con un [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) fino a quando il numero di messaggi nella coda è inferiore all'impostazione nuovamente. L'accodamento è disabilitato per impostazione predefinita perché i backplane implementati in genere dispongono di un proprio controllo di accodamento o di flusso. Nel caso di SQL Server, il pool di connessioni limita in modo efficace il numero di invii in corso in qualsiasi momento.

Per impostazione predefinita, per SQL Server sql Server e Redis vengono utilizzati cinque flussi e l'accodamento è disabilitato, ma queste impostazioni possono essere modificate tramite la configurazione in SQL Server SQL Server e il bus di servizio:By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:

**Codice server .NET per la configurazione del numero di tabelle e della lunghezza della coda per il backplane di SQL Server**

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

**Codice server .NET per la configurazione del numero di argomenti e della lunghezza della coda per il backplane del bus di servizio**

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

Un flusso **di buffering** è uno che è entrato in uno stato di errore; quando il flusso è in stato di errore, tutti i messaggi inviati al backplane avranno esito negativo immediatamente fino a quando il flusso non è più in errore. Lunghezza **coda di invio** indica il numero di messaggi che sono stati inviati ma non ancora inviati.

- **Scalabilità orizzontale dei messaggi del bus di messaggi ricevuti/secScaleout Message Bus Messages Received/Sec**
- **Totale flussi di scalabilità orizzontaleScaleout Streams Total**
- **Flussi di scalabilità orizzontale apertiScaleout Streams Open**
- **Buffering dei flussi di scalabilità orizzontaleScaleout Streams Buffering**
- **Totale errori di scalabilità orizzontaleScaleout Errors Total**
- **Errori di scalabilità orizzontale/secScaleout Errors/Sec**
- **Lunghezza coda di invio con scalabilità orizzontaleScaleout Send Queue Length**

Per altre informazioni su quali contatori vengono misurati, vedere [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Utilizzo di altri contatori delle prestazioniUsing other performance counters

I contatori delle prestazioni seguenti possono essere utili anche per monitorare le prestazioni dell'applicazione.

**Memoria**

- Memoria\\CLR .NET - byte in tutti gli heap (per w3wp)

**ASP.NET**

- ASP.NET: richieste correnti
- ASP.NET: in coda
- ASP.NET: rifiutato

**CPU**

- Informazioni processore: tempo processore

**TCP/IP**

- TCPv6/Connessioni stabilite
- TCPv4/Connessioni stabilite

**Servizio Web**

- Servizio Web - Connessioni correnti
- Servizio Web - Numero massimo di connessioni

**Threading**

- Blocchi e thread\\CLR .NET delle thread logiche correnti
- Blocchi e thread\\CLR .NET : dei thread fisici correnti

<a id="otherresources"></a>

## <a name="other-resources"></a>Risorse aggiuntive

Per altre informazioni sul monitoraggio e l'ottimizzazione delle prestazioni di ASP.NET, vedere gli argomenti seguenti:For more information on performance monitoring and tuning, see the following topics:

- [Cenni preliminari sulle prestazioni di ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [ASP.NET utilizzo dei thread in IIS 7.5, IIS 7.0 e IIS 6.0](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;Elemento&gt; applicationPool (impostazioni Web)](https://msdn.microsoft.com/library/dd560842.aspx)
