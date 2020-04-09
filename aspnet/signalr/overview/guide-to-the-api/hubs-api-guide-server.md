---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Guida all'API di ASP.NET HubS di SignalR - Server (C ) Documenti Microsoft
author: bradygaster
description: Questo documento fornisce un'introduzione alla programmazione del lato server dell'API di ASP.NET SignalR Hubs per SignalR versione 2, con esempi di codice che dimostrano...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c681b104b15bfc4a04587c7abf685dcf20def2ca
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676188"
---
# <a name="aspnet-signalr-hubs-api-guide---server-c"></a>Guida all'API di ASP.NET SignalR Hubs - Server (C )ASP.NET SignalR Hubs API Guide - Server (C

di [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo documento fornisce un'introduzione alla programmazione del lato server dell'API signalR Hubs ASP.NET per SignalR versione 2, con esempi di codice che illustrano le opzioni comuni.
> 
> L'API Hubs SignalR consente di effettuare chiamate di procedura remota (RPC) da un server ai client connessi e dai client al server. Nel codice server, si definiscono i metodi che possono essere chiamati dai client e si chiamano i metodi eseguiti sul client. Nel codice client si definiscono i metodi che possono essere chiamati dal server e si chiamano i metodi eseguiti sul server. SignalR si occupa di tutto l'impianto idraulico client-server per voi.
> 
> SignalR offre anche un'API di livello inferiore denominata connessioni permanenti. Per un'introduzione a SignalR, Hub e connessioni permanenti, vedere [Introduzione a SignalR 2](../getting-started/introduction-to-signalr.md).
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versioni software utilizzate in questo argomento
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versione 2
>   
> 
> 
> ## <a name="topic-versions"></a>Versioni degli argomenti
> 
> Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR Older Versions](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Si prega di lasciare un feedback su come ti è piaciuto questo tutorial e cosa potremmo migliorare nei commenti in fondo alla pagina. Se avete domande che non sono direttamente correlati al tutorial, è possibile pubblicarli al [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Panoramica

Questo documento contiene le seguenti sezioni:

- [Come registrare SignalR middleware](#route)

    - [L'URL /signalr](#signalrurl)
    - [Configurazione delle opzioni SignalR](#options)
- [Come creare e usare le classi HubHow to create and use Hub classes](#hubclass)

    - [Durata dell'oggetto hub](#transience)
    - [Cammello-casing dei nomi Hub nei client JavaScript](#hubnames)
    - [Hub multipli](#multiplehubs)
    - [Hub fortemente tipati](#stronglytypedhubs)
- [Come definire i metodi nella classe Hub che i client possono chiamare](#hubmethods)

    - [Cammello-casing dei nomi dei metodi nei client JavaScript](#methodnames)
    - [Quando eseguire in modo asincronoWhen to execute asynchronously](#asyncmethods)
    - [Definizione degli overloadDefining overloads](#overloads)
    - [Segnalazione dello stato di avanzamento dalle chiamate al metodo hubReporting progress from hub method invocations](#progress)
- [Come chiamare i metodi client dalla classe HubHow to call client methods from the Hub class](#callfromhub)

    - [Selezione dei client che riceveranno la](#selectingclients)
    - [Nessuna convalida in fase di compilazione per i nomi dei metodi](#dynamicmethodnames)
    - [Corrispondenza del nome del metodo senza distinzione tra maiuscole e minuscole](#caseinsensitive)
    - [Esecuzione asincrona](#asyncclient)
- [Come gestire l'appartenenza al gruppo dalla classe Hub](#groupsfromhub)

    - [Esecuzione asincrona dei metodi Add e Remove](#asyncgroupmethods)
    - [Persistenza dell'appartenenza al gruppo](#grouppersistence)
    - [Gruppi di utenti singoli](#singleusergroups)
- [Come gestire gli eventi di durata della connessione nella classe HubHow to handle connection lifetime events in the Hub class](#connectionlifetime)

    - [Quando OnConnected, OnDisconnected e OnReconnected vengono chiamati](#onreconnected)
    - [Stato del chiamante non popolato](#nocallerstate)
- [Come ottenere informazioni sul client dalla proprietà Context](#contextproperty)
- [Come passare lo stato tra i client e la classe HubHow to pass state between clients and the Hub class](#passstate)
- [Come gestire gli errori nella classe HubHow to handle errors in the Hub class](#handleErrors)
- [Come chiamare i metodi client e gestire i gruppi dall'esterno della classe Hub](#callfromoutsidehub)

    - [Chiamata di metodi clientCalling client methods](#callingclientsoutsidehub)
    - [Gestione dell'appartenenza ai gruppi](#managinggroupsoutsidehub)
- [Come abilitare la traccia](#tracing)
- [Come personalizzare la pipeline Hubs](#hubpipeline)

Per la documentazione su come programmare i client, vedere le risorse seguenti:For documentation on how to program clients, see the following resources:

- [Guida all'API di SignalR Hubs - Client JavaScript](hubs-api-guide-javascript-client.md)
- [Guida all'API di SignalR Hubs - Client .NET](hubs-api-guide-net-client.md)

I componenti server per SignalR 2 sono disponibili solo in .NET 4.5. I server che eseguono .NET 4.0 devono utilizzare SignalR v1.x.

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a>Come registrare SignalR middleware

Per definire la route che i client utilizzeranno `MapSignalR` per connettersi all'hub, chiamare il metodo all'avvio dell'applicazione. `MapSignalR`è un [metodo](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) `OwinExtensions` di estensione per la classe. Nell'esempio seguente viene illustrato come definire la route SignalR Hubs utilizzando una classe di avvio OWIN.

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

Se si aggiunge la funzionalità SignalR a un'applicazione MVC ASP.NET, assicurarsi che la route SignalR venga aggiunta prima delle altre route. Per ulteriori informazioni, vedere [Esercitazione: Introduzione a SignalR 2 e MVC 5.](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>L'URL /signalr

Per impostazione predefinita, l'URL della route che i client utilizzeranno per connettersi all'hub è "/signalr". (Non confondere questo URL con l'URL "/signalr/hubs", che è per il file JavaScript generato automaticamente. Per altre informazioni sul proxy generato, vedere [SignalR Hubs API Guide - JavaScript Client - Il proxy generato e le operazioni eseguite per l'utente.)](hubs-api-guide-javascript-client.md#genproxy)

Ci potrebbero essere circostanze straordinarie che rendono questo URL di base non utilizzabile per SignalR; ad esempio, si dispone di una cartella nel progetto denominata *signalr* e non si desidera modificare il nome. In tal caso, è possibile modificare l'URL di base, come illustrato negli esempi seguenti (sostituire "/signalr" nel codice di esempio con l'URL desiderato).

**Codice server che specifica l'URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

**Codice client JavaScript che specifica l'URL (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

**Codice client JavaScript che specifica l'URL (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

**Codice client .NET che specifica l'URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>Configurazione delle opzioni SignalR

Gli overload `MapSignalR` del metodo consentono di specificare un URL personalizzato, un sistema di risoluzione delle dipendenze personalizzato e le opzioni seguenti:

- Abilitare le chiamate tra domini usando CORS o JSONP dai client browser.

    In genere se il `http://contoso.com`browser carica una pagina da , `http://contoso.com/signalr`la connessione SignalR si trova nello stesso dominio, in . Se la `http://contoso.com` pagina da `http://fabrikam.com/signalr`effettua una connessione a , si tratta di una connessione tra domini. Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita. Per ulteriori informazioni, vedere [ASP.NET Guida all'API SignalR Hubs - JavaScript Client - Come stabilire una connessione tra](hubs-api-guide-javascript-client.md#crossdomain)domini .
- Abilitare i messaggi di errore dettagliati.

    Quando si verificano errori, il comportamento predefinito di SignalR consiste nell'inviare ai client un messaggio di notifica senza dettagli su ciò che è accaduto. L'invio di informazioni dettagliate sugli errori ai client non è consigliato nell'ambiente di produzione, perché gli utenti malintenzionati potrebbero essere in grado di utilizzare le informazioni in attacchi contro l'applicazione. Per la risoluzione dei problemi, è possibile utilizzare questa opzione per abilitare temporaneamente la segnalazione degli errori più informativa.
- Disabilitare i file proxy JavaScript generati automaticamente.

    Per impostazione predefinita, un file JavaScript con proxy per le classi Hub viene generato in risposta all'URL "/signalr/hubs". Se non si desidera utilizzare i proxy JavaScript o se si desidera generare manualmente questo file e fare riferimento a un file fisico nei client, è possibile utilizzare questa opzione per disabilitare la generazione del proxy. Per ulteriori informazioni, vedere [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](hubs-api-guide-javascript-client.md#manualproxy).

Nell'esempio seguente viene illustrato come specificare l'URL di `MapSignalR` connessione SignalR e queste opzioni in una chiamata al metodo. Per specificare un URL personalizzato, sostituire "/signalr" nell'esempio con l'URL che si desidera utilizzare.

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Come creare e usare le classi HubHow to create and use Hub classes

Per creare un Hub, creare una classe che deriva da [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). Nell'esempio seguente viene illustrata una classe Hub semplice per un'applicazione di chat.

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

In questo esempio, un client `NewContosoChatMessage` connesso può chiamare il metodo e, in tal caso, i dati ricevuti vengono trasmessi a tutti i client connessi.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Durata dell'oggetto hub

Non si crea un'istanza della classe Hub o non si chiamano i relativi metodi dal proprio codice sul server. tutto ciò che viene fatto per voi dalla pipeline SignalR Hubs. SignalR crea una nuova istanza della classe Hub ogni volta che è necessario gestire un'operazione Hub, ad esempio quando un client si connette, disconnette o effettua una chiamata al metodo al server.

Poiché le istanze della classe Hub sono temporanee, non è possibile usarle per mantenere lo stato da una chiamata al metodo a quella successiva. Ogni volta che il server riceve una chiamata al metodo da un client, una nuova istanza della classe Hub elabora il messaggio. Per mantenere lo stato tramite più connessioni e chiamate a i metodi, utilizzare un altro metodo, ad `Hub`esempio un database, una variabile statica nella classe Hub o una classe diversa che non deriva da . Se si mantengono i dati in memoria, usando un metodo, ad esempio una variabile statica nella classe Hub, i dati andranno persi quando il dominio dell'app viene riciclato.

Se si desidera inviare messaggi ai client dal proprio codice che viene eseguito all'esterno della classe Hub, non è possibile farlo ipotificando un'istanza della classe Hub, ma è possibile farlo ottenendo un riferimento all'oggetto di contesto SignalR per la classe Hub. Per altre informazioni, vedere [Come chiamare i metodi client e gestire i gruppi dall'esterno della classe Hub](#callfromoutsidehub) più avanti in questo argomento.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>Cammello-casing dei nomi Hub nei client JavaScript

Per impostazione predefinita, i client JavaScript fanno riferimento a Hubs utilizzando una versione con maiuscole/minuscole camel del nome della classe. SignalR apporta automaticamente questa modifica in modo che il codice JavaScript possa essere conforme alle convenzioni JavaScript. L'esempio precedente viene `contosoChatHub` indicato come nel codice JavaScript.The previous example would be referred to as in JavaScript code.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

**Client JavaScript che utilizza il proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

Se si desidera specificare un nome diverso `HubName` per i client da utilizzare, aggiungere l'attributo. Quando si `HubName` utilizza un attributo, non viene apportata alcuna modifica al nome del caso camel nei client JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

**Client JavaScript che utilizza il proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Hub multipli

È possibile definire più classi Hub in un'applicazione. In questo caso, la connessione viene condivisa ma i gruppi sono separati:

- Tutti i client utilizzeranno lo stesso URL per stabilire una connessione SignalR con il servizio ("/signalr" o l'URL personalizzato se ne è stato specificato uno) e tale connessione viene utilizzata per tutti gli Hub definiti dal servizio.

    Non esiste alcuna differenza di prestazioni per più hub rispetto alla definizione di tutte le funzionalità Hub in una singola classe.
- Tutti gli hub ottengono le stesse informazioni sulla richiesta HTTP.

    Poiché tutti gli hub condividono la stessa connessione, l'unica informazione sulla richiesta HTTP ottenuta dal server è ciò che viene fornito nella richiesta HTTP originale che stabilisce la connessione SignalR. Se si utilizza la richiesta di connessione per passare informazioni dal client al server specificando una stringa di query, non è possibile fornire stringhe di query diverse a hub diversi. Tutti gli hub riceveranno le stesse informazioni.
- Il file dei proxy JavaScript generato conterrà proxy per tutti gli Hub in un unico file.

    Per informazioni sui proxy JavaScript, vedere [SignalR Hubs API Guide - JavaScript Client - Il proxy generato e le operazioni eseguite automaticamente.](hubs-api-guide-javascript-client.md#genproxy)
- I gruppi sono definiti all'interno di Hub.

    In SignalR è possibile definire gruppi denominati da trasmettere a sottoinsiemi di client connessi. I gruppi vengono gestiti separatamente per ogni Hub. Ad esempio, un gruppo denominato "Administrators" includerebbe `ContosoChatHub` un set di client per la classe e `StockTickerHub` lo stesso nome di gruppo farebbe riferimento a un set diverso di client per la classe.

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a>Hub fortemente tipati

Per definire un'interfaccia per i metodi dell'hub a cui il client può `Hub<T>` fare riferimento (e abilitare `Hub`Intellisense nei metodi dell'hub), derivare l'hub da (introdotto in SignalR 2.1) anziché:

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>Come definire i metodi nella classe Hub che i client possono chiamare

Per esporre un metodo nell'hub che si desidera chiamare dal client, dichiarare un metodo pubblico, come illustrato negli esempi seguenti.

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

È possibile specificare un tipo restituito e i parametri, inclusi i tipi complessi e le matrici, come si farebbe in qualsiasi metodo C. Tutti i dati ricevuti nei parametri o restituiti al chiamante vengono comunicati tra il client e il server tramite JSON e SignalR gestisce automaticamente l'associazione di oggetti complessi e matrici di oggetti.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Cammello-casing dei nomi dei metodi nei client JavaScript

Per impostazione predefinita, i client JavaScript fanno riferimento ai metodi Hub utilizzando una versione con maiuscole/minuscole camel del nome del metodo. SignalR apporta automaticamente questa modifica in modo che il codice JavaScript possa essere conforme alle convenzioni JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**Client JavaScript che utilizza il proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

Se si desidera specificare un nome diverso `HubMethodName` per i client da utilizzare, aggiungere l'attributo.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**Client JavaScript che utilizza il proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Quando eseguire in modo asincronoWhen to execute asynchronously

Se il metodo sarà a esecuzione prolungata o deve eseguire operazioni che comporterebbero l'attesa, ad esempio una ricerca di `void` database o una chiamata al servizio web, rendere il metodo Hub asincrono restituendo un [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (al posto di ritorno) o [Task&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) oggetto (al posto del tipo restituito). `T` Quando si `Task` restituisce un oggetto dal metodo, `Task` SignalR attende il completamento di e quindi invia il risultato unwrapped al client, quindi non vi è alcuna differenza nel modo in cui si codifica la chiamata al metodo nel client.

Rendere asincrono un metodo Hub evita di bloccare la connessione quando utilizza il trasporto WebSocket.Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport. Quando un metodo Hub viene eseguito in modo sincrono e il trasporto è WebSocket, le successive chiamate dei metodi sull'hub dallo stesso client vengono bloccate fino al completamento del metodo Hub.

L'esempio seguente mostra lo stesso metodo codificato per l'esecuzione in modo sincrono o asincrono, seguito dal codice client JavaScript che funziona per chiamare entrambe le versioni.

**Sincrono**

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

**Asincrona**

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**Client JavaScript che utilizza il proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

Per ulteriori informazioni su come utilizzare i metodi asincroni in ASP.NET 4.5, vedere [Utilizzo di metodi asincroni in ASP.NET MVC 4.](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)

<a id="overloads"></a>

### <a name="defining-overloads"></a>Definizione di overloadDefining Overloads

Se si desidera definire overload per un metodo, il numero di parametri in ogni overload deve essere diverso. Se si differenzia un overload semplicemente specificando tipi di parametro diversi, la classe Hub verrà compilata, ma il servizio SignalR genererà un'eccezione in fase di esecuzione quando i client tentano di chiamare uno degli overload.

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a>Segnalazione dello stato di avanzamento dalle chiamate al metodo hubReporting progress from hub method invocations

SignalR 2.1 aggiunge il supporto per il modello di [report di stato](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introdotto in .NET 4.5. Per implementare la `IProgress<T>` segnalazione dello stato di avanzamento, definire un parametro per il metodo hub a cui il client può accedere:To implement progress reporting, define an parameter for your hub method that your client can access:

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

Quando si scrive un metodo server a esecuzione prolungata, è importante usare un modello di programmazione asincrona come Async/Await anziché bloccare il thread hub.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>Come chiamare i metodi client dalla classe HubHow to call client methods from the Hub class

Per chiamare i metodi client `Clients` dal server, usare la proprietà in un metodo nella classe Hub. Nell'esempio seguente viene `addNewMessageToPage` illustrato il codice server che chiama su tutti i client connessi e il codice client che definisce il metodo in un client JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

La chiamata di un metodo client `Task`è un'operazione asincrona e restituisce un oggetto . Utilizzare `await`:

* Per garantire che il messaggio venga inviato senza errori. 
* Per abilitare l'intercettazione e la gestione degli errori in un blocco try-catch.

**Client JavaScript che utilizza il proxy generato**

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

Non è possibile ottenere un valore restituito da un metodo client; sintassi `int x = Clients.All.add(1,1)` come non funziona.

È possibile specificare tipi complessi e matrici per i parametri. Nell'esempio seguente viene passato un tipo complesso al client in un parametro di metodo.

**Codice server che chiama un metodo client utilizzando un oggetto complessoServer code that calls a client method using a complex object**

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

**Codice server che definisce l'oggetto complesso**

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

**Client JavaScript che utilizza il proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Selezione dei client che riceveranno la

La proprietà Clients restituisce un oggetto [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) che fornisce diverse opzioni per specificare quali client riceveranno la rpc:

- Tutti i client connessi.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- Solo il client chiamante.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- Tutti i client ad eccezione del client chiamante.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- Un client specifico identificato dall'ID di connessione.

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    In questo `addContosoChatMessageToPage` esempio viene chiamato il client `Clients.Caller`chiamante e si ha lo stesso effetto dell'utilizzo di .
- Tutti i client connessi ad eccezione dei client specificati, identificati dall'ID di connessione.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- Tutti i client connessi in un gruppo specificato.

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- Tutti i client connessi in un gruppo specificato ad eccezione dei client specificati, identificati dall'ID di connessione.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- Tutti i client connessi in un gruppo specificato ad eccezione del client chiamante.

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- Un utente specifico, identificato da userId.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    Per impostazione `IPrincipal.Identity.Name`predefinita, si tratta di , ma può essere modificato [registrando un'implementazione di IUserIdProvider con l'host globale](mapping-users-to-connections.md#IUserIdProvider).
- Tutti i client e i gruppi in un elenco di ID di connessione.

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- Elenco di gruppi.

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- Un utente per nome.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- Un elenco di nomi utente (introdotto in SignalR 2.1).

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>Nessuna convalida in fase di compilazione per i nomi dei metodi

Il nome del metodo specificato viene interpretato come un oggetto dinamico, il che significa che non esiste alcuna convalida IntelliSense o in fase di compilazione. L'espressione viene valutata in fase di esecuzione. Quando viene eseguita la chiamata al metodo, SignalR invia il nome del metodo e i valori dei parametri al client e se il client dispone di un metodo che corrisponde al nome, tale metodo viene chiamato e i valori dei parametri vengono passati ad esso. Se non viene trovato alcun metodo corrispondente sul client, non viene generato alcun errore. Per informazioni sul formato dei dati trasmessi da SignalR al client dietro le quinte quando si chiama un metodo client, vedere [Introduzione a SignalR](../getting-started/introduction-to-signalr.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>Corrispondenza del nome del metodo senza distinzione tra maiuscole e minuscole

La corrispondenza del nome del metodo non fa distinzione tra maiuscole e minuscole. Ad `Clients.All.addContosoChatMessageToPage` esempio, sul server `AddContosoChatMessageToPage` `addcontosochatmessagetopage`verrà `addContosoChatMessageToPage` eseguito , o sul client.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Esecuzione asincrona

Il metodo chiamato viene eseguito in modo asincrono. Qualsiasi codice che viene dopo una chiamata al metodo a un client verrà eseguito immediatamente senza attendere SignalR per completare la trasmissione dei dati ai client, a meno che non si specifichi che le righe di codice successive devono attendere il completamento del metodo. Nell'esempio di codice riportato di seguito viene illustrato come eseguire due metodi client in sequenza.

**Utilizzo di Await (.NET 4.5)**

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

Se si `await` utilizza l'utente per attendere il completamento di un metodo client prima dell'esecuzione della riga di codice successiva, ciò non significa che i client riceveranno effettivamente il messaggio prima dell'esecuzione della riga di codice successiva. "Completamento" di una chiamata al metodo client significa solo che SignalR ha fatto tutto il necessario per inviare il messaggio. Se hai bisogno di verificare che i clienti hanno ricevuto il messaggio, devi programmare questo meccanismo da solo. Ad esempio, è `MessageReceived` possibile codificare un metodo `addContosoChatMessageToPage` nell'hub e `MessageReceived` nel metodo sul client è possibile chiamare dopo aver eseguito qualsiasi operazione è necessario eseguire sul client. Nell'Hub `MessageReceived` è possibile eseguire qualsiasi operazione dipende dalla ricezione effettiva del client e dall'elaborazione della chiamata al metodo originale.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Come utilizzare una variabile stringa come nome del metodo

Se si desidera richiamare un metodo client utilizzando una `Clients.All` variabile `Clients.Others`stringa `Clients.Caller`come nome `IClientProxy` del metodo, cast (o , , e così via) a e quindi chiamare [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Come gestire l'appartenenza al gruppo dalla classe Hub

I gruppi in SignalR forniscono un metodo per la trasmissione di messaggi a sottoinsiemi specificati di client connessi. Un gruppo può avere un numero qualsiasi di client e un client può essere membro di un numero qualsiasi di gruppi.

Per gestire l'appartenenza al gruppo, `Groups` utilizzare i metodi [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) e [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) forniti dalla proprietà della classe Hub. Nell'esempio seguente `Groups.Add` `Groups.Remove` vengono illustrati i metodi e utilizzati nei metodi Hub chiamati dal codice client, seguiti dal codice client JavaScript che li chiama.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

**Client JavaScript che utilizza il proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

Non è necessario creare gruppi in modo esplicito. In effetti, un gruppo viene creato automaticamente la prima `Groups.Add`volta che si specifica il nome in una chiamata a , che viene eliminato quando si rimuove l'ultima connessione dall'appartenenza.

Non è disponibile alcuna API per ottenere un elenco di appartenenza a un gruppo o un elenco di gruppi. SignalR invia messaggi a client e gruppi in base a un [modello pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe)e il server non gestisce elenchi di gruppi o appartenenze ai gruppi. Ciò consente di ottimizzare la scalabilità, perché ogni volta che si aggiunge un nodo a una Web farm, qualsiasi stato mantenuto da SignalR deve essere propagato al nuovo nodo.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Esecuzione asincrona dei metodi Add e Remove

I `Groups.Add` `Groups.Remove` metodi e vengono eseguiti in modo asincrono. Se si desidera aggiungere un client a un gruppo e inviare immediatamente un messaggio al `Groups.Add` client utilizzando il gruppo, è necessario assicurarsi che il metodo venga completato per primo. Esempio di codice seguente viene illustrato come eseguire questa operazione.

**Aggiunta di un client a un gruppo e messaggistica**

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Persistenza dell'appartenenza al gruppo

SignalR tiene traccia delle connessioni, non degli utenti, quindi se si desidera che un utente sia `Groups.Add` nello stesso gruppo ogni volta che l'utente stabilisce una connessione, è necessario chiamare ogni volta che l'utente stabilisce una nuova connessione.

Dopo una perdita temporanea di connettività, a volte SignalR può ripristinare automaticamente la connessione. In tal caso, SignalR sta ripristinando la stessa connessione, non stabilendo una nuova connessione e pertanto l'appartenenza al gruppo del client viene ripristinata automaticamente. Ciò è possibile anche quando l'interruzione temporanea è il risultato di un riavvio o un errore del server, perché lo stato di connessione per ogni client, incluse le appartenenze ai gruppi, viene eseguito il round-tripped al client. Se un server si arresta e viene sostituito da un nuovo server prima del timeout della connessione, un client può riconnettersi automaticamente al nuovo server e registrarsi nuovamente nei gruppi di cui è membro.

Quando una connessione non può essere ripristinata automaticamente dopo una perdita di connettività o quando si verifica il timeout della connessione o quando il client si disconnette (ad esempio, quando un browser passa a una nuova pagina), le appartenenze ai gruppi vengono perse. La prossima volta che l'utente si connette sarà una nuova connessione. Per mantenere le appartenenze ai gruppi quando lo stesso utente stabilisce una nuova connessione, l'applicazione deve tenere traccia delle associazioni tra utenti e gruppi e ripristinare le appartenenze ai gruppi ogni volta che un utente stabilisce una nuova connessione.

Per altre informazioni sulle connessioni e le riconnessioni, vedere Come gestire gli eventi di [durata della connessione nella classe Hub](#connectionlifetime) più avanti in questo argomento.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Gruppi di utenti singoli

Le applicazioni che utilizzano SignalR in genere devono tenere traccia delle associazioni tra utenti e connessioni per sapere quale utente ha inviato un messaggio e quali utenti devono ricevere un messaggio. I gruppi vengono utilizzati in uno dei due modelli comunemente utilizzati per farlo.

- Gruppi di utenti singoli.

    È possibile specificare il nome utente come nome del gruppo e aggiungere l'ID di connessione corrente al gruppo ogni volta che l'utente si connette o si riconnette. Per inviare messaggi all'utente inviato al gruppo. Uno svantaggio di questo metodo è che il gruppo non fornisce un modo per scoprire se l'utente è online o offline.
- Tenere traccia delle associazioni tra nomi utente e ID di connessione.

    È possibile archiviare un'associazione tra ogni nome utente e uno o più ID di connessione in un dizionario o in un database e aggiornare i dati archiviati ogni volta che l'utente si connette o si disconnette. Per inviare messaggi all'utente specificare gli ID di connessione. Uno svantaggio di questo metodo è che richiede più memoria.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Come gestire gli eventi di durata della connessione nella classe HubHow to handle connection lifetime events in the Hub class

I motivi tipici per la gestione degli eventi di durata della connessione sono tenere traccia della connessione o meno di un utente e dell'associazione tra nomi utente e ID di connessione. Per eseguire il codice quando i client `OnConnected` `OnDisconnected`si `OnReconnected` connettono o si disconnettono, eseguire l'override dei metodi , e virtual della classe Hub, come illustrato nell'esempio seguente.

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>Quando OnConnected, OnDisconnected e OnReconnected vengono chiamati

Ogni volta che un browser passa a una nuova pagina, è necessario stabilire `OnDisconnected` una nuova `OnConnected` connessione, il che significa che SignalR eseguirà il metodo seguito dal metodo. SignalR crea sempre un nuovo ID di connessione quando viene stabilita una nuova connessione.

Il `OnReconnected` metodo viene chiamato quando si verifica un'interruzione temporanea della connettività da cui SignalR può recuperare automaticamente, ad esempio quando un cavo viene temporaneamente scollegato e riconnesso prima del timeout della connessione. Il `OnDisconnected` metodo viene chiamato quando il client viene disconnesso e SignalR non può riconnettersi automaticamente, ad esempio quando un browser passa a una nuova pagina. Pertanto, una possibile sequenza di `OnConnected` `OnReconnected`eventi `OnDisconnected`per un determinato client è , , ; o `OnConnected` `OnDisconnected`, . Non verrà visualizzata `OnConnected`la `OnDisconnected` `OnReconnected` sequenza , , per una determinata connessione.

Il `OnDisconnected` metodo non viene chiamato in alcuni scenari, ad esempio quando un server si arresta o il dominio dell'applicazione viene riciclato. Quando un altro server viene collegato o il dominio dell'app completa il `OnReconnected` suo riciclo, alcuni client potrebbero essere in grado di riconnettersi e generare l'evento.

Per ulteriori informazioni, vedere Informazioni e gestione degli eventi di durata della [connessione in SignalR](handling-connection-lifetime-events.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>Stato del chiamante non popolato

I metodi del gestore eventi della durata della connessione vengono chiamati `state` dal server, il che `Caller` significa che qualsiasi stato inserito nell'oggetto sul client non verrà popolato nella proprietà sul server. Per informazioni `state` sull'oggetto `Caller` e la proprietà, vedere [Come passare lo stato tra i client e la classe Hub](#passstate) più avanti in questo argomento.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Come ottenere informazioni sul client dalla proprietà Context

Per ottenere informazioni sul client, utilizzare la `Context` proprietà della classe Hub. La `Context` proprietà restituisce un oggetto HubCallerContext che fornisce l'accesso alle informazioni seguenti:The property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:

- ID connessione del client chiamante.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    L'ID di connessione è un GUID assegnato da SignalR (non è possibile specificare il valore nel codice). Esiste un ID di connessione per ogni connessione e lo stesso ID di connessione viene usato da tutti gli hub se nell'applicazione sono presenti più hub.
- Dati dell'intestazione HTTP.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    È inoltre possibile ottenere `Context.Headers`intestazioni HTTP da . Il motivo per più riferimenti `Context.Headers` alla stessa cosa `Context.Request` è che è `Context.Headers` stato creato prima, la proprietà è stata aggiunta in un secondo momento e è stata mantenuta per compatibilità con le versioni precedenti.
- Dati della stringa di query.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    È inoltre possibile ottenere `Context.QueryString`dati della stringa di query da .

    La stringa di query che si ottiene in questa proprietà è quella utilizzata con la richiesta HTTP che ha stabilito la connessione SignalR. È possibile aggiungere parametri di stringa di query nel client configurando la connessione, che è un modo pratico per passare dati sul client dal client al server. L'esempio seguente mostra un modo per aggiungere una stringa di query in un client JavaScript quando si usa il proxy generato.

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    Per altre informazioni sull'impostazione dei parametri della stringa di query, vedere le guide API per i client [JavaScript](hubs-api-guide-javascript-client.md) e [.NET.](hubs-api-guide-net-client.md)

    È possibile trovare il metodo di trasporto utilizzato per la connessione nei dati della stringa di query, insieme ad altri valori utilizzati internamente da SignalR:You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    Il valore `transportMethod` di sarà "webSockets", "serverSentEvents", "foreverFrame" o "longPolling". Si noti che se `OnConnected` si controlla questo valore nel metodo del gestore eventi, in alcuni scenari è possibile ottenere inizialmente un valore di trasporto che non è il metodo di trasporto negoziato finale per la connessione. In tal caso il metodo genererà un'eccezione e verrà chiamato di nuovo in un secondo momento quando viene stabilito il metodo di trasporto finale.
- Biscotti.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    È inoltre possibile `Context.RequestCookies`ottenere i cookie da .
- informazioni utente.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- Oggetto HttpContext per la richiesta :

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    Utilizzare questo metodo `HttpContext.Current` anziché `HttpContext` ottenere l'oggetto per la connessione SignalR.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Come passare lo stato tra i client e la classe HubHow to pass state between clients and the Hub class

Il proxy client `state` fornisce un oggetto in cui è possibile archiviare i dati che si desidera trasmettere al server con ogni chiamata al metodo. Sul server è possibile accedere `Clients.Caller` a questi dati nella proprietà nei metodi Hub chiamati dai client. La `Clients.Caller` proprietà non viene popolata per `OnConnected` `OnDisconnected`i `OnReconnected`metodi del gestore eventi della durata della connessione , , e .

La creazione o `state` l'aggiornamento `Clients.Caller` dei dati nell'oggetto e la proprietà funziona in entrambe le direzioni. È possibile aggiornare i valori nel server e passarli al client.

Nell'esempio seguente viene illustrato il codice client JavaScript che archivia lo stato per la trasmissione al server con ogni chiamata al metodo.

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

Nell'esempio seguente viene illustrato il codice equivalente in un client .NET.

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

Nella classe Hub è possibile accedere `Clients.Caller` a questi dati nella proprietà. Nell'esempio seguente viene illustrato il codice che recupera lo stato a cui si fa riferimento nell'esempio precedente.

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> Questo meccanismo per la persistenza dello stato non è destinato `state` a `Clients.Caller` grandi quantità di dati, poiché tutto ciò che viene inserito nella proprietà o viene sottoposto a round trip con ogni chiamata al metodo. È utile per gli elementi più piccoli, ad esempio nomi utente o contatori.

In VB.NET o in un hub fortemente tipizzato, non `Clients.Caller`è possibile accedere all'oggetto stato del chiamante tramite ; invece, `Clients.CallerState` utilizzare (introdotto in SignalR 2.1):

**Utilizzo di CallerState in CUsing CallerState in C #**

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

**Utilizzo di CallerState in Visual BasicUsing CallerState in Visual Basic**

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Come gestire gli errori nella classe HubHow to handle errors in the Hub class

Per gestire gli errori che si verificano nei metodi della classe Hub, assicurarsi innanzitutto di `await`"osservare" tutte le eccezioni derivanti da operazioni asincrone (ad esempio la chiamata di metodi client) utilizzando . Utilizzare quindi uno o più dei seguenti metodi:

- Eseguire il wrapping del codice del metodo in blocchi try-catch e registrare l'oggetto eccezione. Ai fini del debug è possibile inviare l'eccezione al client, ma per motivi di sicurezza non è consigliabile inviare informazioni dettagliate ai client nell'ambiente di produzione.
- Creare un modulo della pipeline Hubs che gestisce il [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) metodo. Nell'esempio seguente viene illustrato un modulo della pipeline che registra gli errori, seguito dal codice in Startup.cs che inserisce il modulo nella pipeline di Hubs.The following example shows a pipeline module that logs errors, followed by code in Startup.cs that injects the module into the Hubs pipeline.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- Utilizzare `HubException` la classe (introdotta in SignalR 2). Questo errore può essere generato da qualsiasi chiamata all'hub. Il `HubError` costruttore accetta un messaggio stringa e un oggetto per l'archiviazione di dati di errore aggiuntivi. SignalR serializzerà automaticamente l'eccezione e la invierà al client, dove verrà utilizzata per rifiutare o non eseguire la chiamata al metodo dell'hub.

    Gli esempi di codice seguenti `HubException` illustrano come generare un errore durante una chiamata all'hub e come gestire l'eccezione nei client JavaScript e .NET.

    **Codice server che dimostra la classe HubException**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    **Codice client JavaScript che dimostra la risposta alla generazione di un'eccezione HubException in un hub**

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    **Codice client .NET che dimostra la risposta alla generazione di un'eccezione HubException in un hub**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

Per altre informazioni sui moduli della pipeline hub, vedere [Come personalizzare la pipeline Hub](#hubpipeline) più avanti in questo argomento.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>Come abilitare la traccia

Per abilitare l'analisi lato server, aggiungere un elemento system.diagnostics al file Web.config, come illustrato nell'esempio seguente:

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

Quando si esegue l'applicazione in Visual Studio, è possibile visualizzare i log nel **Output** finestra.

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>Come chiamare i metodi client e gestire i gruppi dall'esterno della classe Hub

Per chiamare i metodi client da una classe diversa rispetto alla classe Hub, ottenere un riferimento all'oggetto di contesto SignalR per l'hub e usarlo per chiamare i metodi sul client o gestire i gruppi.

La classe `StockTicker` di esempio seguente ottiene l'oggetto di contesto, lo archivia in un'istanza della classe, archivia `updateStockPrice` l'istanza della classe in `StockTickerHub`una proprietà statica e utilizza il contesto dall'istanza della classe singleton per chiamare il metodo sui client connessi a un Hub denominato .

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

Se è necessario utilizzare il contesto più volte in un oggetto di lunga durata, ottenere il riferimento una sola volta e salvarlo anziché ottenerlo ogni volta. Ottenere il contesto una volta assicura che SignalR invia messaggi ai client nella stessa sequenza in cui i metodi Hub rendono le chiamate al metodo client. Per un'esercitazione che illustra come usare il contesto SignalR per un hub, vedere [Trasmissione server con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>Chiamata di metodi clientCalling client methods

È possibile specificare quali client riceveranno RPC, ma sono disponibili meno opzioni rispetto a quando si chiama da una classe Hub. Il motivo è che il contesto non è associato a una particolare chiamata da un client, pertanto i metodi che richiedono la conoscenza dell'ID di connessione corrente, ad `Clients.Others`esempio , o `Clients.Caller`, o `Clients.OthersInGroup`, non sono disponibili. Sono disponibili le opzioni seguenti:

- Tutti i client connessi.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- Un client specifico identificato dall'ID di connessione.

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- Tutti i client connessi ad eccezione dei client specificati, identificati dall'ID di connessione.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- Tutti i client connessi in un gruppo specificato.

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- Tutti i client connessi in un gruppo specificato ad eccezione dei client specificati, identificati dall'ID di connessione.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

Se si chiama la classe non Hub dai metodi della classe Hub, è possibile `Clients.Client`passare `Clients.AllExcept`l'ID `Clients.Caller` `Clients.Others`di `Clients.OthersInGroup`connessione corrente e utilizzarlo con , , o `Clients.Group` per simulare , , o . Nell'esempio riportato `MoveShapeHub` di seguito, la `Broadcaster` classe passa `Broadcaster` l'ID di connessione alla classe in modo che la classe possa simulare `Clients.Others`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>Gestione dell'appartenenza ai gruppi

Per la gestione dei gruppi hai le stesse opzioni di una classe Hub.

- Aggiungere un client a un gruppo

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- Rimuovere un client da un gruppo

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Come personalizzare la pipeline Hubs

SignalR consente di inserire il proprio codice nella pipeline Hub. Nell'esempio seguente viene illustrato un modulo di pipeline Hub personalizzato che registra ogni chiamata al metodo in ingresso ricevuta dal client e dalla chiamata al metodo in uscita richiamata sul client:The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

Il codice seguente nel file *di Startup.cs* registra il modulo per l'esecuzione nella pipeline Hub:The following code in the file can registers the module to run in the Hub pipeline:

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

Esistono molti metodi diversi di cui è possibile eseguire l'override. Per un elenco completo, vedere [Metodi HubPipelineModule](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).
