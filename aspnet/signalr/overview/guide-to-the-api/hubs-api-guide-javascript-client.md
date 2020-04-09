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
# <a name="aspnet-signalr-hubs-api-guide---javascript-client"></a>Guida all'API di ASP.NET SignalR Hubs - Client JavaScript

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo documento fornisce un'introduzione all'uso dell'API Hubs per SignalR versione 2 nei client JavaScript, ad esempio browser e applicazioni Windows Store (WinJS).
>
> L'API Hubs SignalR consente di effettuare chiamate di procedura remota (RPC) da un server ai client connessi e dai client al server. Nel codice server, si definiscono i metodi che possono essere chiamati dai client e si chiamano i metodi eseguiti sul client. Nel codice client si definiscono i metodi che possono essere chiamati dal server e si chiamano i metodi eseguiti sul server. SignalR si occupa di tutto l'impianto idraulico client-server per voi.
>
> SignalR offre anche un'API di livello inferiore denominata connessioni permanenti. Per un'introduzione a SignalR, Hub e connessioni permanenti, vedere [Introduzione a SignalR](../getting-started/introduction-to-signalr.md).
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

Questo documento contiene le seguenti sezioni:

- [Il proxy generato e quello che fa per voi](#genproxy)

    - [Quando utilizzare il proxy generato](#cantusegenproxy)
- [Configurazione client](#clientsetup)

    - [Come fare riferimento al proxy generato dinamicamente](#dynamicproxy)
    - [Come creare un file fisico per il proxy generato SignalR](#manualproxy)
- [Come stabilire una connessione](#establishconnection)

    - [L'oggetto che è stato creato da .connection.hub](#connequivalence)
    - [Esecuzione asincrona del metodo start](#asyncstart)
- [Come stabilire una connessione tra domini](#crossdomain)
- [Come configurare la connessione](#configureconnection)

    - [Come specificare i parametri della stringa di queryHow to specify query string parameters](#querystring)
    - [Come specificare il metodo di trasporto](#transport)
- [Come ottenere un proxy per una classe HubHow to get a proxy for a Hub class](#getproxy)
- [Come definire i metodi sul client che il server può chiamare](#callclient)
- [Come chiamare i metodi server dal client](#callserver)
- [Come gestire gli eventi di durata della connessioneHow to handle connection lifetime events](#connectionlifetime)
- [Come gestire gli errori](#handleerrors)
- [Come abilitare la registrazione lato clientHow to enable client-side logging](#logging)

Per la documentazione su come programmare il server o i client .NET, vedere le risorse seguenti:

- [Guida all'API di SignalR Hubs - Server](hubs-api-guide-server.md)
- [Guida all'API di SignalR Hubs - Client .NET](hubs-api-guide-net-client.md)

Il componente server SignalR 2 è disponibile solo in .NET 4.5 (anche se è presente un client .NET per SignalR 2 in .NET 4.0).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>Il proxy generato e quello che fa per voi

È possibile programmare un client JavaScript per comunicare con un servizio SignalR con o senza un proxy generato da SignalR. Il proxy fa per te è semplificare la sintassi del codice utilizzato per connettersi, scrivere i metodi che il server chiama e chiamare i metodi sul server.

Quando si scrive codice per chiamare i metodi del server, il proxy generato consente di utilizzare `serverMethod(arg1, arg2)` una `invoke('serverMethod', arg1, arg2)`sintassi simile a quella di una funzione locale: è possibile scrivere anziché . La sintassi del proxy generata consente inoltre un errore sul lato client immediato e comprensibile se si digita in modo errato il nome di un metodo server. E se si crea manualmente il file che definisce i proxy, è anche possibile ottenere il supporto IntelliSense per la scrittura di codice che chiama i metodi server.

Si supponga, ad esempio, di avere la seguente classe Hub sul server:

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

Negli esempi di codice seguenti viene illustrato l'aspetto del codice JavaScript per richiamare il `NewContosoChatMessage` metodo sul server e ricevere chiamate del `addContosoChatMessageToPage` metodo dal server.

**Con il proxy generato**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Senza il proxy generato**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Quando utilizzare il proxy generato

Se si desidera registrare più gestori eventi per un metodo client chiamato dal server, non è possibile utilizzare il proxy generato. In caso contrario, è possibile scegliere di utilizzare il proxy generato o meno in base alle preferenze di codifica. Se si sceglie di non utilizzarlo, non è necessario fare riferimento all'URL "signalr/hubs" in un `script` elemento nel codice client.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Configurazione client

Un client JavaScript richiede riferimenti a jQuery e al file JavaScript di base DiR. La versione jQuery deve essere 1.6.4 o versioni successive principali, ad esempio 1.7.2, 1.8.2 o 1.9.1.The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2 or 1.9.1. Se si decide di utilizzare il proxy generato, è necessario anche un riferimento al file JavaScript del proxy generato da SignalR. Nell'esempio seguente viene illustrato l'aspetto dei riferimenti in una pagina HTML che utilizza il proxy generato.

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

Questi riferimenti devono essere inclusi in questo ordine: jQuery first, SignalR core dopo che, e SignalR proxy per ultimo.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Come fare riferimento al proxy generato dinamicamente

Nell'esempio precedente, il riferimento al proxy generato SignalR è al codice JavaScript generato dinamicamente, non a un file fisico. SignalR crea il codice JavaScript per il proxy in tempo reale e lo serve al client in risposta all'URL "/signalr/hubs". Se è stato specificato un URL di base `MapSignalR` diverso per le connessioni SignalR sul server nel metodo, l'URL per il file proxy generato dinamicamente è l'URL personalizzato con "/hubs" aggiunto.

> [!NOTE]
> Per i client JavaScript di Windows 8 (Windows Store), usa il file proxy fisico invece di quello generato dinamicamente. Per ulteriori informazioni, vedere [Come creare un file fisico per il proxy generato SignalR](#manualproxy) più avanti in questo argomento.

In una ASP.NET visualizzazione Razor MVC 4 o 5, utilizzare la tilde per fare riferimento alla radice dell'applicazione nel riferimento al file proxy:In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

Per ulteriori informazioni sull'utilizzo di SignalR in MVC 5, vedere [Introduzione a SignalR e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

In una ASP.NET visualizzazione Razor `Url.Content` MVC 3, utilizzare per il riferimento al file proxy:

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

In un'applicazione Web `ResolveClientUrl` Form ASP.NET, utilizzare per il riferimento al file proxy o registrarlo tramite ScriptManager utilizzando un percorso relativo radice dell'app (a partire da una tilde):

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

Come regola generale, utilizzare lo stesso metodo per specificare l'URL "/signalr/hubs" utilizzato per i file CSS o JavaScript. Se si specifica un URL senza utilizzare una tilde, in alcuni scenari l'applicazione funzionerà correttamente quando si esegue il test in Visual Studio utilizzando IIS Express, ma avrà esito negativo con un errore 404 quando si esegue la distribuzione in IIS completo. Per ulteriori informazioni, vedere **Risoluzione dei riferimenti alle risorse a livello radice** nei server Web in Visual Studio per ASP.NET progetti [Web](https://msdn.microsoft.com/library/58wxa9w5.aspx) nel sito MSDN.

Quando si esegue un progetto web in Visual Studio 2017 in modalità di debug e se si utilizza Internet Explorer come browser, è possibile visualizzare il file proxy in **Esplora soluzioni** in **Script**.

Per visualizzare il contenuto del file, fare doppio clic su **hub.** Se non si utilizza Visual Studio 2012 o 2013 e Internet Explorer o se non si è in modalità di debug, è anche possibile ottenere il contenuto del file individuando l'URL "/signalR/hubs". Ad esempio, se il `http://localhost:56699`sito è `http://localhost:56699/SignalR/hubs` in esecuzione in , vai a nel browser.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Come creare un file fisico per il proxy generato SignalR

In alternativa al proxy generato dinamicamente, è possibile creare un file fisico con il codice proxy e fare riferimento a tale file. È possibile eseguire questa operazione per il controllo sul comportamento di memorizzazione nella cache o bundling o per ottenere IntelliSense durante la codifica di chiamate ai metodi server.

Per creare un file proxy, attenersi alla seguente procedura:

1. Installare il pacchetto [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet.
2. Aprire un prompt dei comandi e individuare la cartella *degli strumenti* che contiene il file SignalR.exe. La cartella degli strumenti si trova nel seguente percorso:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. Immettere il comando seguente:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    Il percorso del *file DLL* è in genere la cartella *bin* nella cartella del progetto.

    Questo comando crea un file denominato *server.js* nella stessa cartella di *signalr.exe*.
4. Inserire il file *server.js* in una cartella appropriata nel progetto, rinominarlo come appropriato per l'applicazione e aggiungere un riferimento a esso al posto del riferimento "signalr/hubs".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Come stabilire una connessione

Prima di poter stabilire una connessione, è necessario creare un oggetto connessione, creare un proxy e registrare i gestori eventi per i metodi che possono essere chiamati dal server. Quando il proxy e i gestori eventi sono `start` impostati, stabilire la connessione chiamando il metodo .

Se si utilizza il proxy generato, non è necessario creare l'oggetto connessione nel codice perché il codice proxy generato esegue automaticamente l'oggetto collegamento.

<a id="nogenconnection"></a>

**Stabilire una connessione (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Stabilire una connessione (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

Il codice di esempio usa l'URL predefinito "/signalr" per connettersi al servizio SignalR. Per informazioni su come specificare un URL di base diverso, vedere [Guida all'API di ASP.NET SignalR Hubs - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).

Per impostazione predefinita, il percorso dell'hub è il server corrente; se ci si connette a un server diverso, specificare l'URL prima di chiamare il `start` metodo, come illustrato nell'esempio seguente:

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> In genere si registrano `start` i gestori eventi prima di chiamare il metodo per stabilire la connessione. Se si desidera registrare alcuni gestori eventi dopo aver stabilito la connessione, è possibile farlo, ma è `start` necessario registrare almeno uno dei gestori eventi prima di chiamare il metodo. Uno dei motivi è che ci possono essere molti hub in un'applicazione, ma non si desidera attivare l'evento `OnConnected` su ogni Hub se si intende utilizzare solo a uno di essi. Quando viene stabilita la connessione, la presenza di un metodo client sul `OnConnected` proxy di un hub è ciò che indica a SignalR di attivare l'evento. Se non si registra alcun gestore `start` eventi prima di chiamare il metodo, sarà possibile richiamare i metodi sull'hub, ma il metodo dell'hub `OnConnected` non verrà chiamato e non verranno richiamati metodi client dal server.

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>L'oggetto che è stato creato da .connection.hub

Come si può vedere dagli esempi, quando `$.connection.hub` si utilizza il proxy generato, fa riferimento all'oggetto connessione. Si tratta dello stesso oggetto `$.hubConnection()` che si ottiene chiamando quando non si utilizza il proxy generato. Il codice proxy generato crea automaticamente la connessione eseguendo l'istruzione seguente:

![Creazione di una connessione nel file proxy generato](hubs-api-guide-javascript-client/_static/image3.png)

Quando si utilizza il proxy generato, è `$.connection.hub` possibile eseguire qualsiasi operazione con un oggetto connessione quando non si utilizza il proxy generato.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Esecuzione asincrona del metodo start

Il `start` metodo viene eseguito in modo asincrono. Restituisce un [oggetto jQuery Deferred](http://api.jquery.com/category/deferred-object/), il che significa che `pipe` `done`è `fail`possibile aggiungere funzioni di callback chiamando metodi quali , e . Se si desidera eseguire il codice dopo aver stabilito la connessione, ad esempio una chiamata a un metodo server, inserire tale codice in una funzione di callback o chiamarlo da una funzione di callback. Il `.done` metodo di callback viene eseguito al termine dell'esecuzione della `OnConnected` connessione e dopo il completamento dell'esecuzione del codice nel metodo del gestore eventi sul server.

Se si inserisce l'istruzione "Now connected" dell'esempio precedente `start` come riga di `.done` codice `console.log` successiva dopo la chiamata al metodo (non in un callback), la riga verrà eseguita prima che venga stabilita la connessione, come illustrato nell'esempio seguente:

![Modo errato per scrivere codice che viene eseguito dopo la connessione è stata stabilita](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Come stabilire una connessione tra domini

In genere se il `http://contoso.com`browser carica una pagina da , `http://contoso.com/signalr`la connessione SignalR si trova nello stesso dominio, in . Se la `http://contoso.com` pagina da `http://fabrikam.com/signalr`effettua una connessione a , si tratta di una connessione tra domini. Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita.

In SignalR 1.x, le richieste tra domini erano controllate da un singolo flag EnableCrossDomain. Questo flag controllava le richieste JSONP e CORS. Per una maggiore flessibilità, tutto il supporto CORS è stato rimosso dal componente server di SignalR (i client JavaScript usano ancora CORS normalmente se viene rilevato che il browser lo supporta) e il nuovo middleware OWIN è stato reso disponibile per supportare questi scenari.

Se JSONP è necessario nel client (per supportare le richieste tra domini nei `EnableJSONP` browser `HubConfiguration` meno `true`recenti), dovrà essere abilitato in modo esplicito impostando l'oggetto su , come illustrato di seguito. JSONP è disabilitato per impostazione predefinita, in quanto è meno sicuro di CORS.

**Aggiunta di Microsoft.Owin.Cors al progetto:** Per installare questa libreria, eseguire il comando seguente nella Console di gestione pacchetti:

`Install-Package Microsoft.Owin.Cors`

Questo comando aggiungerà la versione 2.1.0 del pacchetto al progetto.

### <a name="calling-usecors"></a>Chiamata di UseCorsCalling UseCors

 Il frammento di codice seguente illustra come implementare connessioni tra domini in SignalR 2.The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.

**Implementazione di richieste tra domini in SignalR 2Implementing cross-domain requests in SignalR 2**

Il codice seguente illustra come abilitare CORS o JSONP in un progetto SignalR 2.The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project. In questo `Map` esempio `RunSignalR` di `MapSignalR`codice viene utilizzato e anziché , in modo che il middleware CORS venga eseguito solo per le richieste SignalR che richiedono il supporto CORS (anziché per tutto il traffico nel percorso specificato in `MapSignalR`.) Map può essere utilizzato anche per qualsiasi altro middleware che deve essere eseguito per un prefisso URL specifico, anziché per l'intera applicazione.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - Non impostare `jQuery.support.cors` su true nel codice.
>
>     ![Non impostare jQuery.support.cors su true](hubs-api-guide-javascript-client/_static/image7.png)
>
>     SignalR gestisce l'uso di CORS. L'impostazione `jQuery.support.cors` su true disabilita JSONP perché fa sì che SignalR presupponga che il browser supporti CORS.
> - Quando ti connetti a un URL localhost, Internet Explorer 10 non lo considererà una connessione tra domini, quindi l'applicazione funzionerà localmente con Internet Explorer 10 anche se non hai abilitato le connessioni tra domini sul server.
> - Per informazioni sull'utilizzo di connessioni tra domini con Internet Explorer 9, vedere [questo thread StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Per informazioni sull'utilizzo di connessioni tra domini con Chrome, consultate [questo thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - Il codice di esempio usa l'URL predefinito "/signalr" per connettersi al servizio SignalR. Per informazioni su come specificare un URL di base diverso, vedere [Guida all'API di ASP.NET SignalR Hubs - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Come configurare la connessione

Prima di stabilire una connessione, è possibile specificare i parametri della stringa di query o il metodo di trasporto.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Come specificare i parametri della stringa di queryHow to specify query string parameters

Se si desidera inviare dati al server quando il client si connette, è possibile aggiungere parametri di stringa di query all'oggetto connessione. Negli esempi seguenti viene illustrato come impostare un parametro di stringa di query nel codice client.

**Impostare un valore di stringa di query prima di chiamare il metodo start (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

**Impostare un valore di stringa di query prima di chiamare il metodo start (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

Nell'esempio seguente viene illustrato come leggere un parametro di stringa di query nel codice server.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Come specificare il metodo di trasporto

Come parte del processo di connessione, un client SignalR in genere negozia con il server per determinare il trasporto migliore supportato sia dal server che dal client. Se si conosce già il trasporto che si desidera utilizzare, è possibile ignorare `start` questo processo di negoziazione specificando il metodo di trasporto quando si chiama il metodo .

**Codice client che specifica il metodo di trasporto (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

**Codice client che specifica il metodo di trasporto (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

In alternativa, è possibile specificare più metodi di trasporto nell'ordine in cui si desidera che SignalR li provi:

**Codice client che specifica uno schema di fallback del trasporto personalizzato (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Codice client che specifica uno schema di fallback del trasporto personalizzato (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

È possibile utilizzare i seguenti valori per specificare il metodo di trasporto:

- "webSockets"
- "ForeverFrame"
- "serverSentEvents"
- "longPolling"

Negli esempi seguenti viene illustrato come individuare il metodo di trasporto utilizzato da una connessione.

**Codice client che visualizza il metodo di trasporto utilizzato da una connessione (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

**Codice client che visualizza il metodo di trasporto utilizzato da una connessione (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

Per informazioni su come controllare il metodo di trasporto nel codice server, vedere [ASP.NET Guida all'API di HubS SignalR - Server - Come ottenere informazioni sul client dalla proprietà Context](hubs-api-guide-server.md#contextproperty). Per ulteriori informazioni sui trasporti e i fallback, vedere [Introduzione a SignalR - Trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Come ottenere un proxy per una classe HubHow to get a proxy for a Hub class

Ogni oggetto connessione creato incapsula informazioni su una connessione a un servizio SignalR che contiene una o più classi Hub. Per comunicare con una classe Hub, si usa un oggetto proxy creato dall'utente (se non si utilizza il proxy generato) o generato automaticamente.

Sul client il nome del proxy è una versione con maiuscole/minuscole camel del nome della classe Hub. SignalR apporta automaticamente questa modifica in modo che il codice JavaScript possa essere conforme alle convenzioni JavaScript.

**Classe Hub sul server**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

**Ottenere un riferimento al proxy client generato per l'hubGet a reference to the generated client proxy for the Hub**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

**Creare il proxy client per la classe Hub (senza proxy generato)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

Se decori la classe `HubName` Hub con un attributo, usa il nome esatto senza modificare la distinzione tra maiuscole e minuscole.

**Classe Hub nel server con attributo HubName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

**Ottenere un riferimento al proxy client generato per l'hubGet a reference to the generated client proxy for the Hub**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

**Creare il proxy client per la classe Hub (senza proxy generato)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Come definire i metodi sul client che il server può chiamare

Per definire un metodo che il server può chiamare da un hub, `client` aggiungere un gestore eventi `on` al proxy Hub utilizzando la proprietà del proxy generato o chiamare il metodo se non si utilizza il proxy generato. I parametri possono essere oggetti complessi.

Aggiungere il gestore eventi `start` prima di chiamare il metodo per stabilire la connessione. Se si desidera aggiungere gestori eventi `start` dopo aver chiamato il metodo, vedere la nota in [Come stabilire una connessione](#establishconnection) in precedenza in questo documento e utilizzare la sintassi illustrata per definire un metodo senza utilizzare il proxy generato.

La corrispondenza del nome del metodo non fa distinzione tra maiuscole e minuscole. Ad `Clients.All.addContosoChatMessageToPage` esempio, sul server `AddContosoChatMessageToPage` `addContosoChatMessageToPage`verrà `addcontosochatmessagetopage` eseguito , o sul client.

**Definire il metodo sul client (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

**Metodo alternativo per definire il metodo nel client (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

**Definire il metodo sul client (senza il proxy generato o quando si aggiunge dopo la chiamata al metodo start)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

**Codice server che chiama il metodo client**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

Gli esempi seguenti includono un oggetto complesso come parametro del metodo.

**Metodo Define sul client che accetta un oggetto complesso (con il proxy generato)Define method on client that takes a complex object (with the generated proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

**Metodo Define sul client che accetta un oggetto complesso (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

**Codice server che definisce l'oggetto complesso**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

**Codice server che chiama il metodo client utilizzando un oggetto complesso**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Come chiamare i metodi server dal client

Per chiamare un metodo server dal `server` client, utilizzare la `invoke` proprietà del proxy generato o il metodo sul proxy Hub se non si utilizza il proxy generato. Il valore restituito o i parametri possono essere oggetti complessi.

Passare una versione con maiuscole/minuscole del nome del metodo nell'hub. SignalR apporta automaticamente questa modifica in modo che il codice JavaScript possa essere conforme alle convenzioni JavaScript.

Negli esempi seguenti viene illustrato come chiamare un metodo server che non dispone di un valore restituito e come chiamare un metodo server che dispone di un valore restituito.

**Metodo server senza attributo HubMethodName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

**Codice server che definisce l'oggetto complesso passato in un parametro**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

**Codice client che richiama il metodo server (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

**Codice client che richiama il metodo server (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

Se il metodo Hub `HubMethodName` è stato decorato con un attributo, utilizzare tale nome senza modificare la distinzione tra maiuscole e minuscole.

**Metodo server** con un attributo HubMethodName

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

**Codice client che richiama il metodo server (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

**Codice client che richiama il metodo server (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

Negli esempi precedenti viene illustrato come chiamare un metodo server che non ha alcun valore restituito. Negli esempi seguenti viene illustrato come chiamare un metodo server con un valore restituito.

**Codice server per un metodo con un valore restituitoServer code for a method that has a return value**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

**La classe Stock utilizzata per il** valore restituito

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

**Codice client che richiama il metodo server (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

**Codice client che richiama il metodo server (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Come gestire gli eventi di durata della connessioneHow to handle connection lifetime events

SignalR fornisce i seguenti eventi di durata della connessione che è possibile gestire:

- `starting`: generato prima dell'invio di dati tramite la connessione.
- `received`: generato quando vengono ricevuti dati sulla connessione. Fornisce i dati ricevuti.
- `connectionSlow`: generato quando il client rileva una connessione lenta o frequente.
- `reconnecting`: generato quando il trasporto sottostante inizia la riconnessione.
- `reconnected`: generato quando il trasporto sottostante è stato riconnesso.
- `stateChanged`: generato quando cambia lo stato della connessione. Fornisce il vecchio stato e il nuovo stato (Connessione, Connesso, Riconnessione o Disconnesso).
- `disconnected`: generato quando la connessione si è disconnessa.

Ad esempio, se si desidera visualizzare messaggi di avviso quando si verificano problemi di connessione che potrebbero causare ritardi evidenti, gestire l'evento. `connectionSlow`

**Gestire l'evento connectionSlow (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

**Gestire l'evento connectionSlow (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

Per ulteriori informazioni, vedere Informazioni e gestione degli eventi di durata della [connessione in SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Come gestire gli errori

Il client JavaScript SignalR fornisce un `error` evento per il quale è possibile aggiungere un gestore. È inoltre possibile utilizzare il metodo fail per aggiungere un gestore per gli errori derivanti da una chiamata al metodo server.

Se non si abilitano in modo esplicito i messaggi di errore dettagliati sul server, l'oggetto eccezione restituito da SignalR dopo che un errore contiene informazioni minime sull'errore. Ad esempio, se `newContosoChatMessage` una chiamata a ha esito`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`negativo, il messaggio di errore nell'oggetto di errore contiene " " L'invio di messaggi di errore dettagliati ai client nell'ambiente di produzione non è consigliato per motivi di sicurezza, ma se si desidera abilitare i messaggi di errore dettagliati per la risoluzione dei problemi, utilizzare il codice seguente sul server.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

Nell'esempio seguente viene illustrato come aggiungere un gestore per l'evento di errore.

**Aggiungere un gestore errori (con il proxy generato)Add an error handler (with the generated proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

**Aggiungere un gestore errori (senza il proxy generato)Add an error handler (without the generated proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

Nell'esempio seguente viene illustrato come gestire un errore da una chiamata al metodo.

**Gestire un errore da una chiamata al metodo (con il proxy generato)Handle an error from a method invocation (with the generated proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

**Gestire un errore da una chiamata al metodo (senza il proxy generato)Handle an error from a method invocation (without the generated proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Se una chiamata al `error` metodo ha esito negativo, `error` viene generato anche `.fail` l'evento, in modo che il codice nel gestore del metodo e nel callback del metodo venga eseguito.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Come abilitare la registrazione lato clientHow to enable client-side logging

Per abilitare la registrazione lato `logging` client su una connessione, `start` impostare la proprietà sull'oggetto connessione prima di chiamare il metodo per stabilire la connessione.

**Abilitare la registrazione (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

**Abilitare la registrazione (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Per visualizzare i log, apri gli strumenti di sviluppo del browser e vai alla scheda Console. Per un'esercitazione che mostra istruzioni dettagliate e schermate che mostrano come eseguire questa operazione, vedere [Trasmissione server con ASP.NET Signalr - Abilitare](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging)la registrazione .
