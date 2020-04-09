---
uid: signalr/overview/getting-started/introduction-to-signalr
title: Introduzione a SignalR Documenti Microsoft
author: bradygaster
description: In questo articolo viene descritto che cos'è SignalR e alcune delle soluzioni che è stato progettato per creare.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8dbc31a5c8d59fa55dc5b513c1a51d24d18a685f
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676349"
---
# <a name="introduction-to-signalr"></a>Introduzione a SignalR

da parte [di Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In questo articolo viene descritto che cos'è SignalR e alcune delle soluzioni che è stato progettato per creare. 
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Si prega di lasciare un feedback su come ti è piaciuto questo tutorial e cosa potremmo migliorare nei commenti in fondo alla pagina. Se avete domande che non sono direttamente correlati al tutorial, è possibile pubblicarli al [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).

## <a name="what-is-signalr"></a>Che cos'è SignalR?

ASP.NET SignalR è una libreria per sviluppatori ASP.NET che semplifica il processo di aggiunta di funzionalità Web in tempo reale alle applicazioni. La funzionalità Web in tempo reale è la possibilità di avere il codice server push contenuto ai client connessi immediatamente come diventa disponibile, piuttosto che avere il server in attesa di un client per richiedere nuovi dati.

SignalR può essere utilizzato per aggiungere qualsiasi tipo di funzionalità web "in tempo reale" all'applicazione ASP.NET. Mentre la chat è spesso usata come esempio, puoi fare molto di più. Ogni volta che un utente aggiorna una pagina Web per visualizzare nuovi dati o la pagina implementa il [polling lungo](http://en.wikipedia.org/wiki/Push_technology#Long_polling) per recuperare nuovi dati, è un candidato per l'utilizzo di SignalR. Gli esempi includono dashboard e applicazioni di monitoraggio, applicazioni collaborative (ad esempio la modifica simultanea di documenti), aggiornamenti dello stato di avanzamento dei processi e moduli in tempo reale.

SignalR consente anche tipi completamente nuovi di applicazioni web che richiedono aggiornamenti ad alta frequenza dal server, ad esempio, giochi in tempo reale.

SignalR fornisce una semplice API per la creazione di chiamate di procedura remota (RPC) da server a client che chiamano funzioni JavaScript nei browser client (e in altre piattaforme client) dal codice .NET lato server. SignalR include anche API per la gestione delle connessioni (ad esempio, gli eventi di connessione e disconnessione) e il raggruppamento delle connessioni.

![Richiamo di metodi con SignalR](introduction-to-signalr/_static/image1.png)

SignalR gestisce automaticamente la gestione delle connessioni e consente di trasmettere messaggi a tutti i client connessi contemporaneamente, come in una chat room. È anche possibile inviare messaggi a client specifici. La connessione tra client e server è persistente, a differenza di una connessione HTTP classica, che viene ristabilita per ogni comunicazione.

SignalR supporta la funzionalità "server push", in cui il codice server può chiamare il codice client nel browser utilizzando le chiamate di procedura remota (RPC), anziché il modello di richiesta-risposta comune sul Web oggi.

Le applicazioni SignalR possono essere scalabili a migliaia di client utilizzando provider di scalabilità orizzontale incorporati e di terze parti.

I provider integrati includono:
* [Bus di servizio](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3)
* [SQL Server](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
* [Redis](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Redis)

I fornitori di terze parti includono:
* [NCache](https://www.alachisoft.com/ncache/asp-net-core-signalr.html).

SignalR è open-source, accessibile tramite [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>SignalR e WebSocket

SignalR utilizza il nuovo trasporto WebSocket, se disponibile, e ricade sui trasporti meno recenti, se necessario. Mentre si potrebbe certamente scrivere la tua applicazione utilizzando WebSocket direttamente, utilizzando SignalR significa che molte delle funzionalità aggiuntive che sarebbe necessario implementare è già fatto per voi. Ancora più importante, questo significa che è possibile codificare l'app per sfruttare i vantaggi di WebSocket senza doversi preoccupare di creare un percorso di codice separato per i client meno recenti. SignalR protegge anche dal doversi preoccupare degli aggiornamenti a WebSocket, poiché SignalR viene aggiornato per supportare le modifiche nel trasporto sottostante, fornendo all'applicazione un'interfaccia coerente tra le versioni di WebSocket.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Trasporti e fallback

SignalR è un'astrazione su alcuni dei trasporti necessari per eseguire il lavoro in tempo reale tra client e server. Una connessione SignalR viene avviata come HTTP e viene quindi promossa a una connessione WebSocket, se disponibile. WebSocket è il trasporto ideale per SignalR, poiché sfrutta in modo più efficiente la memoria del server, ha la latenza più bassa e ha le funzionalità più sottostanti (ad esempio la comunicazione duplex completa tra client e server), ma ha anche i requisiti più rigorosi: WebSocket richiede che il server utilizzi Windows Server 2012 o Windows 8 e .NET Framework 4.5. Se questi requisiti non vengono soddisfatti, SignalR tenterà di utilizzare altri trasporti per effettuare i collegamenti.

### <a name="html-5-transports"></a>Trasporti HTML 5

Questi trasporti dipendono dal supporto per [HTML 5](http://en.wikipedia.org/wiki/HTML5). Se il browser client non supporta lo standard HTML 5, verranno utilizzati trasporti meno recenti.

- **WebSocket** (se sia il server che il browser indicano che possono supportare Websocket). WebSocket è l'unico trasporto che stabilisce una vera connessione permanente e bidirezionale tra client e server. Tuttavia, WebSocket ha anche i requisiti più rigorosi; è completamente supportato solo nelle versioni più recenti di Microsoft Internet Explorer, Google Chrome e Mozilla Firefox e ha solo un'implementazione parziale in altri browser come Opera e Safari.
- **Eventi inviati dal**server , noto anche come EventSource (se il browser supporta gli eventi inviati dal server, che è fondamentalmente tutti i browser ad eccezione di Internet Explorer.

### <a name="comet-transports"></a>Trasporti cometa

I trasporti seguenti sono basati sul modello di applicazione Web [Comet,](http://en.wikipedia.org/wiki/Comet_(programming)) in cui un browser o un altro client gestisce una richiesta HTTP di lunga durata, che il server può utilizzare per eseguire il push dei dati al client senza che il client lo richieda in modo specifico.

- **Forever Frame** (solo per Internet Explorer). Forever Frame crea un IFrame nascosto che effettua una richiesta a un endpoint sul server che non viene completata. Il server invia quindi continuamente lo script al client che viene eseguito immediatamente, fornendo una connessione unidirezionale in tempo reale dal server al client. La connessione da client a server utilizza una connessione separata dal server alla connessione client e, come una richiesta HTTP standard, viene creata una nuova connessione per ogni dato che deve essere inviato.
- **Ajax lungo polling**. Il polling lungo non crea una connessione permanente, ma esegue il polling del server con una richiesta che rimane aperta fino a quando il server non risponde, a quel punto la connessione viene chiusa e viene richiesta immediatamente una nuova connessione. Ciò potrebbe introdurre una certa latenza durante la reimpostazione della connessione.

Per ulteriori informazioni sui trasporti supportati in base alle configurazioni, vedere [Piattaforme supportate](supported-platforms.md).

### <a name="transport-selection-process"></a>Processo di selezione del trasporto

Nell'elenco seguente vengono illustrati i passaggi utilizzati da SignalR per decidere quale trasporto utilizzare.

1. Se il browser è Internet Explorer 8 o versioni precedenti, viene utilizzato il polling lungo.
2. Se JSONP è configurato, `jsonp` ovvero il `true` parametro è impostato su all'avvio della connessione, viene usato il polling lungo.
3. Se viene effettuata una connessione tra domini, ovvero se l'endpoint SignalR non si trova nello stesso dominio della pagina di hosting, verrà utilizzato WebSocket se vengono soddisfatti i seguenti criteri:

   - Il client supporta CORS (Cross-Origin Resource Sharing). Per informazioni dettagliate sui client che supportano CORS, vedere [CORS presso caniuse.com](http://www.caniuse.com/CORS).
   - Il client supporta WebSocket
   - Il server supporta WebSocket

     Se uno di questi criteri non viene soddisfatto, verrà utilizzato il polling lungo. Per ulteriori informazioni sulle connessioni tra domini, vedere [Come stabilire una connessione tra](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)domini .
4. Se JSONP non è configurato e la connessione non è tra domini, WebSocket verrà utilizzato se sia il client che il server lo supportano.
5. Se il client o il server non supportano WebSocket, vengono utilizzati gli eventi inviati dal server, se disponibili.
6. Se gli eventi inviati dal server non sono disponibili, viene tentato il frame Forever.
7. Se Forever Frame ha esito negativo, viene utilizzato il polling lungo.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Monitoraggio dei trasporti

È possibile determinare il trasporto utilizzato dall'applicazione abilitando la registrazione nell'hub e aprendo la finestra della console nel browser.

Per abilitare la registrazione per gli eventi dell'hub in un browser, aggiungere il comando seguente all'applicazione client:

`$.connection.hub.logging = true;`

- In Internet Explorer, aprire gli strumenti di sviluppo premendo F12 e fare clic sulla scheda Console.

    ![Console in Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- In Chrome, apri la console premendo Ctrl - Maiusc - J.

    ![Console in Google Chrome](introduction-to-signalr/_static/image3.png)

Con la console aperta e la registrazione abilitata, sarai in grado di vedere quale trasporto viene utilizzato da SignalR.

![Console in Internet Explorer con il trasporto WebSocket](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Specifica di un trasporto

La negoziazione di un trasporto richiede una certa quantità di tempo e risorse client/server. Se le funzionalità client sono note, è possibile specificare un trasporto all'avvio della connessione client. Il frammento di codice seguente illustra l'avvio di una connessione utilizzando il trasporto Ajax Long Polling, come se fosse noto che il client non supporta alcun altro protocollo:The following code snippet demonstrates starting a connection using the Ajax Long Polling transport, as would be used if it was known that the client does not support any other protocol:

`connection.start({ transport: 'longPolling' });`

È possibile specificare un ordine di fallback se si desidera che un client tenti trasporti specifici nell'ordine. Il frammento di codice seguente illustra il tentativo di WebSocket e il cui errore, passando direttamente al polling lungo.

`connection.start({ transport: ['webSockets','longPolling'] });`

Le costanti stringa per specificare i trasporti sono definite come segue:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Connessioni e hub

L'API SignalR contiene due modelli per la comunicazione tra client e server: connessioni permanenti e Hub.

Oggetto Connection rappresenta un endpoint semplice per l'invio di messaggi a singolo destinatario, raggruppati o broadcast. L'API di connessione permanente (rappresentata nel codice .NET dalla classe PersistentConnection) consente allo sviluppatore di accedere direttamente al protocollo di comunicazione di basso livello esposto da SignalR. L'utilizzo del modello di comunicazione Connessioni sarà familiare agli sviluppatori che hanno utilizzato API basate sulla connessione, ad esempio Windows Communication Foundation.Using the Connections communication model will be familiar to developers who have used connection-based APIs such as Windows Communication Foundation.

Un hub è una pipeline più di alto livello basata sull'API di connessione che consente al client e al server di chiamare direttamente i metodi l'uno sull'altro. SignalR gestisce l'invio attraverso i limiti del computer come per magia, consentendo ai client di chiamare i metodi sul server con la stessa facilità dei metodi locali e viceversa. L'utilizzo del modello di comunicazione Hubs sarà familiare agli sviluppatori che hanno utilizzato API di chiamata remota, ad esempio .NET Remoting. L'uso di un hub consente inoltre di passare parametri fortemente tipizzati ai metodi, abilitando l'associazione di modelli.

### <a name="architecture-diagram"></a>Diagramma dell'architettura

Nel diagramma seguente viene illustrata la relazione tra hub, connessioni permanenti e le tecnologie sottostanti utilizzate per i trasporti.

![Diagramma dell'architettura SignalR che mostra API, trasporti e client](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Come funzionano gli hub

Quando il codice lato server chiama un metodo sul client, un pacchetto viene inviato attraverso il trasporto attivo che contiene il nome e i parametri del metodo da chiamare (quando un oggetto viene inviato come parametro del metodo, viene serializzato utilizzando JSON). Il client associa quindi il nome del metodo ai metodi definiti nel codice lato client. Se esiste una corrispondenza, il metodo client verrà eseguito utilizzando i dati del parametro deserializzato.

La chiamata al metodo può essere monitorata utilizzando strumenti come [Fiddler.The](http://fiddler2.com/) method call can be monitored using tools like Fiddler. L'immagine seguente mostra una chiamata al metodo inviata da un server SignalR a un client Web browser nel riquadro Registri di Fiddler. La chiamata al metodo viene `MoveShapeHub`inviata da un hub denominato `updateShape`e il metodo richiamato viene chiamato .

![Visualizzazione del registro Fiddler che mostra il traffico SignalR](introduction-to-signalr/_static/image6.png)

In questo esempio, il nome `H` dell'hub viene identificato con il parametro ; il nome del metodo `M` viene identificato con il parametro e `A` i dati inviati al metodo vengono identificati con il parametro. L'applicazione che ha generato questo messaggio viene creata nell'esercitazione in [tempo reale ad alta frequenza.](tutorial-high-frequency-realtime-with-signalr.md)

### <a name="choosing-a-communication-model"></a>Scelta di un modello di comunicazione

La maggior parte delle applicazioni deve usare l'API Hubs.Most applications should use the Hubs API. L'API Connections può essere utilizzata nelle circostanze seguenti:The Connections API could be used in the following circumstances:

- È necessario specificare il formato del messaggio effettivo inviato.
- Lo sviluppatore preferisce utilizzare un modello di messaggistica e invio anziché un modello di chiamata remota.
- Un'applicazione esistente che utilizza un modello di messaggistica viene sottoposta a porting per l'utilizzo di SignalR.An existing application that uses a messaging model is being ported to use SignalR.
