---
uid: signalr/overview/guide-to-the-api/handling-connection-lifetime-events
title: Comprensione e gestione degli eventi di durata della connessione in SignalR Documenti Microsoft
author: bradygaster
description: Questo articolo descrive come usare gli eventi esposti dall'API Hubs.This article describes how to use the events exposed by the Hubs API.
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: 03960de2-8d95-4444-9169-4426dcc64913
msc.legacyurl: /signalr/overview/guide-to-the-api/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 5bdf20549fccab5d644e35fdf4ce351540c8620d
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676216"
---
# <a name="understanding-and-handling-connection-lifetime-events-in-signalr"></a>Comprensione e gestione degli eventi di durata della connessione in SignalR

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In questo articolo viene fornita una panoramica degli eventi di connessione, riconnessione e disconnessione di SignalR che è possibile gestire e delle impostazioni di timeout e keepalive che è possibile configurare.
>
> L'articolo presuppone che tu abbia già una certa conoscenza di SignalR e degli eventi di durata della connessione. Per un'introduzione a SignalR, vedere [Introduzione a SignalR](../getting-started/introduction-to-signalr.md). Per gli elenchi degli eventi di durata della connessione, vedere le risorse seguenti:For lists of connection lifetime events, see the following resources:
>
> - [Come gestire gli eventi di durata della connessione nella classe HubHow to handle connection lifetime events in the Hub class](hubs-api-guide-server.md#connectionlifetime)
> - [Come gestire gli eventi di durata della connessione nei client JavaScriptHow to handle connection lifetime events in JavaScript clients](hubs-api-guide-javascript-client.md#connectionlifetime)
> - [Come gestire gli eventi di durata della connessione nei client .NETHow to handle connection lifetime events in .NET clients](hubs-api-guide-net-client.md#connectionlifetime)
>
> ## <a name="software-versions-used-in-this-topic"></a>Versioni software utilizzate in questo argomento
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
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

## <a name="overview"></a>Panoramica

In questo articolo sono contenute le sezioni seguenti:

- [Terminologia e scenari sulla durata della connessione](#terminology)

    - [Connessioni SignalR, connessioni di trasporto e connessioni fisiche](#signalrvstransport)
    - [Scenari di disconnessione del trasportoTransport disconnection scenarios](#transportdisconnect)
    - [Scenari di disconnessione client](#clientdisconnect)
    - [Scenari di disconnessione del server](#serverdisconnect)
- [Impostazioni di timeout e keepalive](#timeoutkeepalive)

    - [Connectiontimeout](#connectiontimeout)
    - [DisconnectTimeout (Timeout di disconnessione](#disconnecttimeout)
    - [Keepalive](#keepalive)
    - [Come modificare le impostazioni di timeout e keepalive](#changetimeout)
- [Come notificare all'utente le disconnessioni](#notifydisconnect)
- [Come riconnettersi continuamente](#continuousreconnect)
- [Come disconnettere un client nel codice server](#disconnectclientfromserver)
- [Rilevamento del motivo di una disconnessione](#detectingreasonfordisconnection)

I collegamenti agli argomenti di riferimento per le API sono relativi alla versione .NET 4.5 dell'API. Se si utilizza .NET 4, vedere [la versione .NET 4 degli argomenti API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Terminologia e scenari sulla durata della connessione

Il `OnReconnected` gestore eventi in un hub `OnConnected` SignalR `OnDisconnected` può essere eseguito direttamente dopo ma non dopo per un determinato client. Il motivo per cui è possibile avere una riconnessione senza una disconnessione è che ci sono diversi modi in cui la parola "connessione" viene utilizzata in SignalR.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>Connessioni SignalR, connessioni di trasporto e connessioni fisiche

In questo articolo si distinguono tra *le connessioni SignalR,* le connessioni di *trasporto*e le *connessioni fisiche*:

- **La connessione SignalR** si riferisce a una relazione logica tra un client e un URL del server, gestita dall'API SignalR e identificata in modo univoco da un ID di connessione. I dati relativi a questa relazione vengono gestiti da SignalR e vengono utilizzati per stabilire una connessione di trasporto. La relazione termina e SignalR elimina i dati `Stop` quando il client chiama il metodo o viene raggiunto un limite di timeout mentre SignalR sta tentando di ristabilire una connessione di trasporto persa.
- **La connessione** di trasporto si riferisce a una relazione logica tra un client e un server, gestita da una delle quattro API di trasporto: WebSockets, eventi inviati dal server, frame per sempre o polling lungo. SignalR utilizza l'API di trasporto per creare una connessione di trasporto e l'API di trasporto dipende dall'esistenza di una connessione di rete fisica per creare la connessione di trasporto. La connessione di trasporto termina quando SignalR termina o quando l'API di trasporto rileva che la connessione fisica è interrotta.
- **La connessione fisica** si riferisce ai collegamenti di rete fisici (fili, segnali wireless, router, ecc.) che facilitano la comunicazione tra un computer client e un computer server. La connessione fisica deve essere presente per stabilire una connessione di trasporto ed è necessario stabilire una connessione di trasporto per stabilire una connessione SignalR. Tuttavia, l'interruzione della connessione fisica non termina sempre immediatamente la connessione di trasporto o la connessione SignalR, come verrà spiegato più avanti in questo argomento.

Nel diagramma seguente, la connessione SignalR è rappresentata dal livello SignalR dell'API Hubs e PersistentConnection, la connessione di trasporto è rappresentata dal livello Transports e la connessione fisica è rappresentata dalle linee tra il server e i client.

![Diagramma dell'architettura SignalR](handling-connection-lifetime-events/_static/image1.png)

Quando si `Start` chiama il metodo in un client SignalR, si fornisce il codice client SignalR con tutte le informazioni necessarie per stabilire una connessione fisica a un server. Il codice client SignalR utilizza queste informazioni per effettuare una richiesta HTTP e stabilire una connessione fisica che utilizza uno dei quattro metodi di trasporto. Se la connessione di trasporto non riesce o il server non riesce, la connessione SignalR non scompare immediatamente perché il client dispone ancora delle informazioni necessarie per ristabilire automaticamente una nuova connessione di trasporto allo stesso URL SignalR. In questo scenario, non è coinvolto alcun intervento dall'applicazione utente e quando il codice client SignalR stabilisce una nuova connessione di trasporto, non avvia una nuova connessione SignalR. La continuità della connessione SignalR si riflette nel fatto che l'ID `Start` di connessione, che viene creato quando si chiama il metodo, non cambia.

Il `OnReconnected` gestore eventi nell'Hub viene eseguito quando una connessione di trasporto viene ristabilita automaticamente dopo essere stata persa. Il `OnDisconnected` gestore eventi viene eseguito alla fine di una connessione SignalR. Una connessione SignalR può terminare in uno dei seguenti modi:

- Se il client `Stop` chiama il metodo, viene inviato un messaggio di arresto al server e sia il client che il server terminano immediatamente la connessione SignalR.
- Dopo la perdita della connettività tra client e server, il client tenta di riconnettersi e il server attende la riconnessione del client. Se i tentativi di riconnessione hanno esito negativo e il timeout di disconnessione termina, sia il client che il server terminano la connessione SignalR. Il client interrompe il tentativo di riconnessione e il server elimina la rappresentazione della connessione SignalR.
- Se l'esecuzione del client viene `Stop` interrotta senza avere la possibilità di chiamare il metodo, il server attende che il client si riconnetta e quindi termina la connessione SignalR dopo il periodo di timeout della disconnessione.
- Se l'esecuzione del server viene interrotta, il client tenta di riconnettersi (ricreare la connessione di trasporto) e quindi termina la connessione SignalR dopo il periodo di timeout della disconnessione.

Quando non ci sono problemi di connessione e l'applicazione `Stop` utente termina la connessione SignalR chiamando il metodo, la connessione SignalR e la connessione di trasporto iniziano e terminano all'incirca nello stesso momento. Nelle sezioni seguenti vengono descritti in modo più dettagliato gli altri scenari.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Scenari di disconnessione del trasportoTransport disconnection scenarios

Le connessioni fisiche potrebbero essere lente o potrebbero verificarsi interruzioni nella connettività. A seconda di fattori quali la durata dell'interruzione, la connessione di trasporto potrebbe essere interrotta. SignalR tenta quindi di ristabilire la connessione di trasporto. A volte l'API di connessione di trasporto rileva l'interruzione ed elimina la connessione di trasporto e SignalR rileva immediatamente che la connessione viene persa. In altri scenari, né l'API di connessione di trasporto né SignalR viene immediatamente a conoscenza che la connettività è stata persa. Per tutti i trasporti ad eccezione del polling lungo, il client SignalR utilizza una funzione denominata *keepalive* per verificare la perdita di connettività che l'API di trasporto non è in grado di rilevare. Per informazioni sulle connessioni di polling prolungate, vedere [Impostazioni di timeout e keepalive](#timeoutkeepalive) più avanti in questo argomento.

Quando una connessione è inattiva, periodicamente il server invia un pacchetto keepalive al client. Alla data in cui viene scritto questo articolo, la frequenza predefinita è ogni 10 secondi. Ascoltando questi pacchetti, i client possono sapere se c'è un problema di connessione. Se un pacchetto keepalive non viene ricevuto quando previsto, dopo un breve periodo di tempo il client presuppone che vi siano problemi di connessione, ad esempio lentezza o interruzioni. Se il keepalive non viene ancora ricevuto dopo un periodo di tempo più lungo, il client presuppone che la connessione sia stata interrotta e inizia a tentare di riconnettersi.

Il diagramma seguente illustra gli eventi client e server generati in uno scenario tipico in caso di problemi con la connessione fisica che non vengono immediatamente riconosciuti dall'API di trasporto. Il diagramma si applica alle seguenti circostanze:

- Il trasporto è WebSockets, per sempre frame, o eventi inviati dal server.
- Esistono diversi periodi di interruzione nella connessione di rete fisica.
- L'API di trasporto non viene a conoscenza delle interruzioni, pertanto SignalR si basa sulla funzionalità keepalive per rilevarle.

![Disconnessioni di trasporto](handling-connection-lifetime-events/_static/image2.png)

Se il client passa alla modalità di riconnessione ma non riesce a stabilire una connessione di trasporto entro il limite di timeout di disconnessione, il server termina la connessione SignalR. In questo caso, il server esegue `OnDisconnected` il metodo dell'hub e accoda un messaggio di disconnessione da inviare al client nel caso in cui il client riesca a connettersi in un secondo momento. Se il client quindi si riconnette, riceve il `Stop` comando di disconnessione e chiama il metodo . In questo `OnReconnected` scenario, non viene eseguito quando `OnDisconnected` il client si riconnette `Stop`e non viene eseguito quando il client chiama . Il diagramma seguente illustra questo scenario.

![Interruzioni del trasporto - timeout del serverTransport disruptions - server timeout](handling-connection-lifetime-events/_static/image3.png)

Gli eventi di durata della connessione SignalR che possono essere generati sul client sono i seguenti:

- `ConnectionSlow`evento client.

    Generato quando è trascorsa una proporzione preimpostata del periodo di timeout keepalive da quando è stato ricevuto l'ultimo messaggio o ping keepalive. Il periodo di avviso di timeout keepalive predefinito è 2/3 del timeout keepalive. Il timeout keepalive è di 20 secondi, pertanto l'avviso viene generato a circa 13 secondi.

    Per impostazione predefinita, il server invia ping keepalive ogni 10 secondi e il client verifica la presenza di ping keepalive ogni 2 secondi (un terzo della differenza tra il valore di timeout keepalive e il valore di avviso di timeout keepalive).

    Se l'API di trasporto viene a conoscenza di una disconnessione, SignalR potrebbe essere informato della disconnessione prima che il periodo di avviso di timeout keepalive passi. In tal caso, l'evento `ConnectionSlow` non verrebbe generato e `Reconnecting` SignalR andrebbe direttamente all'evento.
- `Reconnecting`evento client.

    Generato quando (a) l'API di trasporto rileva che la connessione è stata persa o (b) il periodo di timeout keepalive è passato da quando è stato ricevuto l'ultimo messaggio o ping keepalive. Il codice client SignalR inizia a tentare di riconnettersi. È possibile gestire questo evento se si desidera che l'applicazione esedisca quando viene persa una connessione di trasporto. Il periodo di timeout keepalive predefinito è attualmente di 20 secondi.

    Se il codice client tenta di chiamare un metodo Hub mentre SignalR è in modalità di riconnessione, SignalR tenterà di inviare il comando. Nella maggior parte dei casi, tali tentativi falliranno, ma in alcune circostanze potrebbero avere successo. Per gli eventi inviati dal server, i trasporti per sempre frame e di polling lungo, SignalR utilizza due canali di comunicazione, uno che il client utilizza per inviare messaggi e uno che utilizza per ricevere i messaggi. Il canale utilizzato per la ricezione è quello permanentemente aperto, e questo è quello che viene chiuso quando la connessione fisica viene interrotta. Il canale utilizzato per l'invio rimane disponibile, pertanto se viene ripristinata la connettività fisica, una chiamata al metodo dal client al server potrebbe avere esito positivo prima che il canale di ricezione venga ristabilito. Il valore restituito non verrà ricevuto fino a quando SignalR non riapre il canale utilizzato per la ricezione.
- `Reconnected`evento client.

    Generato quando la connessione di trasporto viene ristabilita. Il `OnReconnected` gestore eventi nell'Hub viene eseguito.
- `Closed`evento client`disconnected` (evento in JavaScript).

    Generato quando il periodo di timeout di disconnessione scade mentre il codice client SignalR sta tentando di riconnettersi dopo aver perso la connessione di trasporto. Il timeout di disconnessione predefinito è 30 secondi. Questo evento viene generato anche quando `Stop` la connessione termina perché viene chiamato il metodo.

Le interruzioni della connessione di trasporto che non vengono rilevate dall'API di trasporto e non ritardano la ricezione di ping keepalive dal server per un periodo di tempo superiore al periodo di avviso di timeout keepalive potrebbero non causare la generato di eventi di durata della connessione.

Alcuni ambienti di rete deliberatamente chiudere le connessioni inattive, e un'altra funzione dei pacchetti keepalive è quello di aiutare a prevenire questo facendo sapere a queste reti che una connessione SignalR è in uso. In casi estremi la frequenza predefinita dei ping keepalive potrebbe non essere sufficiente per impedire connessioni chiuse. In tal caso è possibile configurare i ping keepalive in modo che vengano inviati più spesso. Per altre informazioni, vedere [Impostazioni di timeout e keepalive](#timeoutkeepalive) più avanti in questo argomento.

> [!NOTE]
>
> **Importante**: La sequenza degli eventi qui descritti non è garantita. SignalR fa ogni tentativo di generare gli eventi di durata della connessione in modo prevedibile secondo questo schema, ma esistono molte varianti di eventi di rete e molti modi in cui i framework di comunicazione sottostanti, ad esempio le API di trasporto, li gestiscono. Ad esempio, `Reconnected` l'evento potrebbe non essere generato quando `OnConnected` il client si riconnette o il gestore sul server potrebbe essere eseguito quando il tentativo di stabilire una connessione ha esito negativo. In questo argomento vengono descritti solo gli effetti che normalmente verrebbero prodotti da determinate circostanze tipiche.

<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Scenari di disconnessione client

In un client browser, il codice client SignalR che gestisce una connessione SignalR viene eseguito nel contesto JavaScript di una pagina Web. Ecco perché la connessione SignalR deve terminare quando si passa da una pagina all'altra, ed è per questo che si dispone di più connessioni con più ID di connessione se ci si connette da più finestre del browser o schede. Quando l'utente chiude una finestra del browser o una scheda o passa a una nuova pagina o aggiorna la pagina, la `Stop` connessione SignalR termina immediatamente perché il codice client SignalR gestisce l'evento del browser per l'utente e chiama il metodo . In questi scenari o in qualsiasi piattaforma `Stop` client quando `OnDisconnected` l'applicazione chiama il metodo, il `Closed` gestore eventi `disconnected` viene eseguito immediatamente sul server e il client genera l'evento (l'evento è denominato in JavaScript).

Se un'applicazione client o il computer su cui è in esecuzione si blocca o passa alla modalità di sospensione (ad esempio, quando l'utente chiude il computer portatile), il server non viene informato su ciò che è accaduto. Per quanto il server sa, la perdita del client potrebbe essere dovuta a un'interruzione della connettività e il client potrebbe tentare di riconnettersi. Pertanto, in questi scenari il server attende per dare `OnDisconnected` al client la possibilità di riconnettersi e non viene eseguito fino alla scadenza del periodo di timeout di disconnessione (circa 30 secondi per impostazione predefinita). Il diagramma seguente illustra questo scenario.

![Errore del computer client](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Scenari di disconnessione del server

Quando un server non è in linea - si riavvia, non riesce, il dominio dell'app ricicla e così via - il risultato potrebbe essere simile a `ConnectionSlow` una connessione persa o l'API di trasporto e SignalR potrebbe sapere immediatamente che il server non è più disponibile e SignalR potrebbe iniziare a provare a riconnettersi senza generare l'evento. Se il client passa alla modalità di riconnessione e se il server viene ripristinato o riavviato o viene portato in linea un nuovo server prima della scadenza del periodo di timeout di disconnessione, il client si riconnetterà al server ripristinato o nuovo. In tal caso, la connessione SignalR `Reconnected` continua sul client e viene generato l'evento. Sul primo server, `OnDisconnected` non viene mai eseguito `OnReconnected` e sul `OnConnected` nuovo server, viene eseguito anche se non è mai stato eseguito per quel client prima. L'effetto è lo stesso se il client si riconnette allo stesso server dopo un riavvio o un riciclo del dominio dell'app, perché al riavvio del server non dispone di memoria dell'attività di connessione precedente. Il diagramma seguente presuppone che l'API di trasporto venga `ConnectionSlow` immediatamente a conoscenza della connessione persa, pertanto l'evento non viene generato.

![Errore del server e riconnessione](handling-connection-lifetime-events/_static/image5.png)

Se un server non diventa disponibile entro il periodo di timeout della disconnessione, la connessione SignalR termina. In questo scenario, `Closed` `disconnected` l'evento (nei client JavaScript) viene generato sul client ma `OnDisconnected` non viene mai chiamato sul server. Il diagramma seguente presuppone che l'API di trasporto non venga a conoscenza della connessione `ConnectionSlow` persa, pertanto viene rilevata dalla funzionalità keepalive SignalR e viene generato l'evento.

![Errore e timeout del server](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Impostazioni di timeout e keepalive

I `ConnectionTimeout`valori `DisconnectTimeout`predefiniti `KeepAlive` , e sono appropriati per la maggior parte degli scenari, ma possono essere modificati se l'ambiente ha esigenze particolari. Ad esempio, se l'ambiente di rete chiude le connessioni inattive per 5 secondi, potrebbe essere necessario ridurre il valore keepalive.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

Questa impostazione rappresenta la quantità di tempo per lasciare aperta una connessione di trasporto e in attesa di una risposta prima di chiuderla e aprirne una nuova. Il valore predefinito è 110 secondi.

Questa impostazione si applica solo quando la funzionalità keepalive è disabilitata, che in genere si applica solo al trasporto di polling lungo. Nel diagramma seguente viene illustrato l'effetto di questa impostazione su una connessione di trasporto di polling lunga.

![Connessione di trasporto polling lungo](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout (Timeout di disconnessione

Questa impostazione rappresenta la quantità di tempo di attesa `Disconnected` dopo la perdita di una connessione di trasporto prima di generare l'evento. Il valore predefinito è 30 secondi. Quando si `DisconnectTimeout` `KeepAlive` imposta , viene impostato automaticamente `DisconnectTimeout` su 1/3 del valore.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

Questa impostazione rappresenta la quantità di tempo di attesa prima di inviare un pacchetto keepalive tramite una connessione inattiva. Il valore predefinito è 10 secondi. Questo valore non deve essere maggiore `DisconnectTimeout` di 1/3 del valore.

Se si desidera `DisconnectTimeout` impostare `KeepAlive` entrambi `DisconnectTimeout`e `KeepAlive`, impostare dopo . In `KeepAlive` caso contrario, `DisconnectTimeout` l'impostazione verrà sovrascritta quando viene automaticamente impostato su `KeepAlive` 1/3 del valore di timeout.

Se si desidera disabilitare la `KeepAlive` funzionalità keepalive, impostare su null. La funzionalità Keepalive viene disabilitata automaticamente per il trasporto polling lungo.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Come modificare le impostazioni di timeout e keepalive

Per modificare i valori predefiniti per `Application_Start` queste impostazioni, impostarli nel file *Global.asax,* come illustrato nell'esempio seguente. I valori visualizzati nel codice di esempio sono gli stessi dei valori predefiniti.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Come notificare all'utente le disconnessioni

In alcune applicazioni è possibile visualizzare un messaggio all'utente in caso di problemi di connettività. Sono disponibili diverse opzioni per come e quando eseguire questa operazione. Gli esempi di codice seguenti sono per un client JavaScript che usa il proxy generato.

- Gestire `connectionSlow` l'evento per visualizzare un messaggio non appena SignalR è a conoscenza di problemi di connessione, prima di passare alla modalità di riconnessione.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Gestire `reconnecting` l'evento per visualizzare un messaggio quando SignalR è a conoscenza di una disconnessione e sta entrando in modalità di riconnessione.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Gestire `disconnected` l'evento per visualizzare un messaggio quando si verifica il timeout di un tentativo di riconnessione. In questo scenario, l'unico modo per ristabilire una connessione con il server `Start` nuovamente consiste nel riavviare la connessione SignalR chiamando il metodo , che creerà un nuovo ID di connessione. Nell'esempio di codice seguente viene utilizzato un flag per assicurarsi di emettere la notifica solo dopo `Stop` un timeout di riconnessione, non dopo una normale fine alla connessione SignalR causata dalla chiamata al metodo.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Come riconnettersi continuamente

In alcune applicazioni potrebbe essere necessario ristabilire automaticamente una connessione dopo che è stata persa e il tentativo di riconnessione è scaduto. A tale scopo, è `Start` possibile `Closed` chiamare il`disconnected` metodo dal gestore eventi ( gestore eventi sui client JavaScript). È possibile attendere un periodo di `Start` tempo prima di chiamare per evitare di eseguire questa operazione troppo frequentemente quando il server o la connessione fisica non sono disponibili. L'esempio di codice seguente è per un client JavaScript che usa il proxy generato.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Un potenziale problema da tenere presente nei client mobili è che i tentativi di riconnessione continua quando il server o la connessione fisica non è disponibile potrebbero causare un inutile consumo della batteria.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Come disconnettere un client nel codice server

SignalR versione 2 non dispone di un'API server incorporata per disconnettere i client. Sono [previsti piani per l'aggiunta](https://github.com/SignalR/SignalR/issues/2101)di questa funzionalità in futuro. Nella versione corrente di SignalR, il modo più semplice per disconnettere un client dal server consiste nell'implementare un metodo di disconnessione sul client e chiamare tale metodo dal server. Nell'esempio di codice seguente viene illustrato un metodo di disconnessione per un client JavaScript che usa il proxy generato.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Sicurezza: né questo metodo per disconnettere i client né l'API incorporata proposta affronteranno lo scenario dei client compensati `stopClient` che eseguono codice dannoso, poiché i client potrebbero riconnettersi o il codice violato potrebbe rimuovere il metodo o modificare le operazioni eseguite. La posizione appropriata per implementare la protezione DOS (State-ing-Service) non si trova nel framework o nel livello del server, ma piuttosto nell'infrastruttura front-end.

<a id="detectingreasonfordisconnection"></a>
## <a name="detecting-the-reason-for-a-disconnection"></a>Rilevamento del motivo di una disconnessione

SignalR 2.1 aggiunge un `OnDisconnect` overload all'evento del server che indica se il client è deliberatamente disconnesso anziché timeout. Il `StopCalled` parametro è true se il client ha chiuso in modo esplicito la connessione. In JavaScript, se un errore del server ha portato il client `$.connection.hub.lastError`a disconnettersi, le informazioni sull'errore verranno passate al client come .

**Codice del server `stopCalled` C: parametro**

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample7.cs?highlight=1,3)]

**Codice client JavaScript: `lastError` accesso `disconnect` all'evento.**

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample8.js?highlight=2-3)]
