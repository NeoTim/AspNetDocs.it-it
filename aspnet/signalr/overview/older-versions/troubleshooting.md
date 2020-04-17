---
uid: signalr/overview/older-versions/troubleshooting
title: Risoluzione dei problemi di SignalR (SignalR 1.x) Documenti Microsoft
author: bradygaster
description: In questo articolo vengono descritti i problemi comuni relativi allo sviluppo di applicazioni SignalR.
ms.author: bradyg
ms.date: 06/05/2013
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: e65ce086d28cff2a36c609f47a05af632081be63
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543093"
---
# <a name="signalr-troubleshooting-signalr-1x"></a>Risoluzione dei problemi di SignalR (SignalR 1.x)

da parte [di Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In questo documento vengono descritti i problemi comuni relativi alla risoluzione dei problemi relativi a SignalR.

Questo documento contiene le sezioni seguenti.

- [La chiamata di metodi tra il client e il server ha esito negativo in modo invisibile all'utenteCalling methods between the client and server silently fails](#connection)
- [Altri problemi di connessione](#other)
- [Errori di compilazione e lato serverCompilation and server-side errors](#server)
- [Problemi di Visual Studio](#vs)
- [Problemi di Internet Information Services](#iis)
- [Problemi di AzureAzure issues](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>La chiamata di metodi tra il client e il server ha esito negativo in modo invisibile all'utenteCalling methods between the client and server silently fails

In questa sezione vengono descritte le possibili cause dell'esito negativo di una chiamata al metodo tra client e server senza un messaggio di errore significativo. In un'applicazione SignalR, il server non dispone di informazioni sui metodi implementati dal client; quando il server richiama un metodo client, il nome del metodo e i dati dei parametri vengono inviati al client e il metodo viene eseguito solo se esiste nel formato specificato dal server. Se non viene trovato alcun metodo corrispondente sul client, non accade nulla e non viene generato alcun messaggio di errore sul server.

Per analizzare ulteriormente i metodi client non vengono chiamati, è possibile attivare la registrazione prima di chiamare il metodo start sull'hub per vedere quali chiamate provengono dal server. Per abilitare la registrazione in un'applicazione JavaScript, vedere [Come abilitare la registrazione lato client (versione client JavaScript)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Per abilitare la registrazione in un'applicazione client .NET, vedere [Come abilitare la registrazione lato client (versione client .NET)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Metodo con errori di ortografia, firma del metodo non corretta o nome dell'hub non corretto

Se il nome o la firma di un metodo chiamato non corrisponde esattamente a un metodo appropriato sul client, la chiamata avrà esito negativo. Verificare che il nome del metodo chiamato dal server corrisponda al nome del metodo sul client. Inoltre, SignalR crea il proxy hub utilizzando metodi con maiuscole/minuscole camel, come appropriato in JavaScript, in modo che un metodo chiamato `SendMessage` sul server verrebbe chiamato `sendMessage` nel proxy client. Se si `HubName` utilizza l'attributo nel codice lato server, verificare che il nome utilizzato corrisponda al nome utilizzato per creare l'hub nel client. Se non si `HubName` utilizza l'attributo, verificare che il nome dell'hub in un client JavaScript sia con maiuscole/minuscole camel, ad esempio chatHub anziché ChatHub.

### <a name="duplicate-method-name-on-client"></a>Nome del metodo duplicato nel client

Verificare di non disporre di un metodo duplicato nel client che differisca solo per maiuscole e minuscole. Se l'applicazione client `sendMessage`dispone di un metodo denominato , `SendMessage` verificare che non sia stato chiamato anche un metodo.

### <a name="missing-json-parser-on-the-client"></a>Parser JSON mancante sul client

SignalR richiede che sia presente un parser JSON per serializzare le chiamate tra il server e il client. Se il client non dispone di un parser JSON incorporato (ad esempio Internet Explorer 7), è necessario includerne uno nell'applicazione. È possibile scaricare il parser JSON [qui](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Combinazione della sintassi Hub e PersistentConnection

SignalR utilizza due modelli di comunicazione: Hubs e PersistentConnections. La sintassi per chiamare questi due modelli di comunicazione è diversa nel codice client. Se è stato aggiunto un hub nel codice server, verificare che tutto il codice client utilizzi la sintassi dell'hub corretta.

**Codice client JavaScript che crea un oggetto PersistentConnection in un client JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Codice client JavaScript che crea un proxy Hub in un client Javascript**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**Codice del server C'è che esegue il mapping di una route a un oggetto PersistentConnection**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**Codice del server C ' che esegue il mapping di una route a un hub o a più hub se si dispone di più applicazioni**

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a>Connessione avviata prima dell'aggiunta di sottoscrizioni

Se la connessione dell'hub viene avviata prima che i metodi che possono essere chiamati dal server vengano aggiunti al proxy, i messaggi non verranno ricevuti. Il seguente codice JavaScript non avvierà correttamente l'hub:

**Codice client JavaScript non corretto che non consente la ricezione di messaggi Hub**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

Aggiungere invece le sottoscrizioni al metodo prima di chiamare Start:Instead, add the method subscriptions before calling Start:

**Codice client JavaScript che aggiunge correttamente sottoscrizioni a un hub**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Nome del metodo mancante nel proxy hub

Verificare che il metodo definito nel server sia sottoscritto nel client. Anche se il server definisce il metodo, deve comunque essere aggiunto al proxy client. I metodi possono essere aggiunti al proxy client nei modi seguenti `client` (si noti che il metodo viene aggiunto al membro dell'hub, non direttamente all'hub):

**Codice client JavaScript che aggiunge metodi a un proxy hub**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Metodi hub o hub non dichiarati come Public

Per essere visibili sul client, l'implementazione `public`e i metodi dell'hub devono essere dichiarati come .

### <a name="accessing-hub-from-a-different-application"></a>Accesso all'hub da un'altra applicazioneAccessing hub from a different application

È possibile accedere agli hub SignalR solo tramite applicazioni che implementano client SignalR. SignalR non può interagire con altre librerie di comunicazione (ad esempio servizi Web SOAP o WCF). Se non è disponibile alcun client SignalR per la piattaforma di destinazione, non è possibile accedere direttamente all'endpoint del server.

### <a name="manually-serializing-data"></a>Serializzazione manuale dei dati

SignalR utilizzerà automaticamente JSON per serializzare i parametri del metodo, non è necessario eseguire questa operazione manualmente.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Metodo Hub remoto non eseguito sul client nella funzione OnDisconnected

Questo comportamento dipende dalla progettazione. Quando `OnDisconnected` viene chiamato, l'hub `Disconnected` è già entrato nello stato, che non consente di chiamare ulteriori metodi dell'hub.

**Codice del server C' che esegue correttamente il codice nell'evento OnDisconnected**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a>Raggiunto limite di connessione

Quando si utilizza la versione completa di IIS su un sistema operativo client come Windows 7, viene imposto un limite di 10 connessioni. Quando si utilizza un sistema operativo client, utilizzare IIS Express invece per evitare questo limite.

### <a name="cross-domain-connection-not-set-up-properly"></a>Connessione tra domini non configurata correttamente

Se una connessione tra domini (una connessione per la quale l'URL SignalR non si trova nello stesso dominio della pagina di hosting) non è impostata correttamente, la connessione potrebbe non riuscire senza un messaggio di errore. Per informazioni su come abilitare la comunicazione tra domini, vedere [Come stabilire una connessione tra](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)domini .

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>Connessione tramite NTLM (Active Directory) non funziona nel client .NET

Una connessione in un'applicazione client .NET che utilizza la protezione del dominio potrebbe non riuscire se la connessione non è configurata correttamente. Per utilizzare SignalR in un ambiente di dominio, impostare la proprietà di connessione necessaria come segue:

**Codice client di C'è che implementa le credenziali di connessione**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a>Altri problemi di connessione

In questa sezione vengono descritte le cause e le soluzioni per sintomi o messaggi di errore specifici che si verificano durante una connessione.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>Errore "L'avvio deve essere chiamato prima dell'invio dei dati"

Questo errore si verifica comunemente se il codice fa riferimento a oggetti SignalR prima dell'avvio della connessione. Il wireup per i gestori e simili che chiamerà i metodi definiti sul server deve essere aggiunto dopo il completamento della connessione. Si noti `Start` che la chiamata a è asincrona, pertanto il codice dopo la chiamata può essere eseguito prima del completamento. Il modo migliore per aggiungere gestori dopo l'avvio completo di una connessione consiste nell'inserirli in una funzione di callback che viene passata come parametro al metodo start:The best way to add handlers after a connection starts completely is to put them into a callback function that is passed as a parameter to the start method:

**Codice client JavaScript che aggiunge correttamente gestori eventi che fanno riferimento a oggetti SignalR**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Questo errore si verifica anche se una connessione si interrompe mentre gli oggetti SignalR sono ancora referenziati.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>Errore "301 spostato in modo permanente" o "302 spostato temporaneamente"

Questo errore può essere visualizzato se il progetto contiene una cartella denominata SignalR, che interferirà con il proxy creato automaticamente. Per evitare questo errore, non `SignalR` utilizzare una cartella denominata nell'applicazione o disattivare la generazione automatica del proxy. Per ulteriori dettagli, vedere [il proxy generato e le operazioni disponibili.](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>Errore "403 accesso negato" nel client .NET o Silverlight

Questo errore può verificarsi in ambienti tra domini in cui la comunicazione tra domini non è abilitata correttamente. Per informazioni su come abilitare la comunicazione tra domini, vedere [Come stabilire una connessione tra](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)domini . Per stabilire una connessione tra domini in un client Silverlight, vedere Connessioni tra domini [da client Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>Errore "404 Non trovato"

Esistono diverse cause per questo problema. Verificare tutti gli elementi seguenti:

- **Riferimento all'indirizzo proxy hub non formattato correttamente:** Questo errore si verifica in genere se il riferimento all'indirizzo proxy dell'hub generato non è formattato correttamente. Verificare che il riferimento all'indirizzo dell'hub sia stato eseguito correttamente. Per informazioni [dettagliate, vedere Come fare riferimento al proxy generato dinamicamente.](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy)
- **Aggiunta di route all'applicazione prima di aggiungere la route hub:Adding routes to application before adding the hub route:** Se l'applicazione utilizza altre route, verificare che `MapHubs`la prima route aggiunta sia la chiamata a .

### <a name="500-internal-server-error"></a>"500 Errore interno del server"

Si tratta di un errore molto generico che potrebbe avere una vasta gamma di cause. I dettagli dell'errore devono essere visualizzati nel registro eventi del server o possono essere trovati tramite il debug del server. Informazioni più dettagliate sugli errori possono essere ottenute attivando gli errori dettagliati sul server. Per ulteriori informazioni, vedere [Come gestire gli errori nella classe Hub](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>Errore &lt;"TypeError:&gt; hubType non definito"

Questo errore si verifica `MapHubs` se la chiamata a non viene effettuata correttamente. Vedere [Come registrare il percorso SignalR e configurare](../guide-to-the-api/hubs-api-guide-server.md#route) le opzioni SignalR per ulteriori informazioni.

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException non è stato gestito dal codice utente

Verificare che i parametri inviati ai metodi non includano tipi non serializzabili, ad esempio handle di file o connessioni a database. Se è necessario utilizzare membri su un oggetto lato server che non si desidera inviare al client `JSONIgnore` (per motivi di sicurezza o per motivi di serializzazione), utilizzare l'attributo .

### <a name="protocol-error-unknown-transport-error"></a>Errore "Errore di protocollo: trasporto sconosciuto"

Questo errore può verificarsi se il client non supporta i trasporti utilizzati da SignalR. Vedere Trasporti e fallback per informazioni su quali browser possono essere utilizzati con SignalR.See Transports and [Fallbacks](../getting-started/introduction-to-signalr.md#transports) for information on which browsers can be used with SignalR.

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"La generazione del proxy dell'hub JavaScript è stata disabilitata."

Questo errore si `DisableJavaScriptProxies` verifica se viene impostato includendo anche `signalr/hubs`un riferimento al proxy generato dinamicamente in . Per ulteriori informazioni sulla creazione manuale del proxy, vedere Il proxy generato e le operazioni eseguite [automaticamente.](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>Errore "l'ID di connessione è in formato non corretto" o "l'identità dell'utente non può cambiare durante una connessione SignalR attiva"

Questo errore può essere visualizzato se viene utilizzata l'autenticazione e il client viene disconnesso prima dell'interruzione della connessione. La soluzione consiste nell'arrestare la connessione SignalR prima di disconnettere il client.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"Errore non rilevato: SignalR: jQuery non trovato. Assicurarsi che jQuery viene fatto riferimento prima dell'errore "file SignalR.js"

Il client JavaScript SignalR richiede jQuery per l'esecuzione. Verificare che il riferimento a jQuery sia corretto, che il percorso utilizzato sia valido e che il riferimento a jQuery sia prima del riferimento a SignalR.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>Errore "Uncaught TypeError:&lt;Impossibile leggere la proprietà ' proprietà&gt;' di undefined"

Questo errore deriva dalla non presenza di jQuery o del proxy hub a cui viene fatto riferimento correttamente. Verificare che il riferimento a jQuery e il proxy hub siano corretti, che il percorso utilizzato sia valido e che il riferimento a jQuery sia prima del riferimento al proxy hub. Il riferimento predefinito al proxy hub dovrebbe essere simile al seguente:The default reference to the hubs proxy should look like the following:

**Codice lato client HTML che fa riferimento correttamente al proxy Hubs**

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>Errore "RuntimeBinderException non gestito dal codice utente"

Questo errore può verificarsi `Hub.On` quando viene utilizzato l'overload non corretto di. Se il metodo ha un valore restituito, il tipo restituito deve essere specificato come parametro di tipo generico:If the method has a return value, the return type must be specified as a generic type parameter:

**Metodo definito sul client (senza proxy generato)**

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>L'ID di connessione è incoerente o la connessione si interrompe tra i caricamenti di pagina

Questo comportamento dipende dalla progettazione. Poiché l'oggetto hub è ospitato nell'oggetto pagina, l'hub viene eliminato quando la pagina viene aggiornata. Un'applicazione a più pagine deve mantenere l'associazione tra gli utenti e gli ID di connessione in modo che siano coerenti tra i caricamenti di pagina. Gli ID di connessione possono essere archiviati `ConcurrentDictionary` nel server in un oggetto o in un database.

### <a name="value-cannot-be-null-error"></a>Errore "Il valore non può essere null"

I metodi sul lato server con parametri facoltativi non sono attualmente supportati. Se il parametro facoltativo viene omesso, il metodo avrà esito negativo. Per ulteriori informazioni, vedere [Parametri facoltativi](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>Errore "Firefox non riesce a &lt;stabilire&gt;una connessione al server all'indirizzo" in Firebug

Questo messaggio di errore può essere visualizzato in Firebug se la negoziazione del trasporto WebSocket non riesce e viene invece utilizzato un altro trasporto. Questo comportamento dipende dalla progettazione.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>Errore "il certificato remoto non è valido in base alla procedura di convalida" nell'applicazione client .NET

Se il server richiede certificati client personalizzati, è possibile aggiungere un certificato x509 alla connessione prima che venga effettuata la richiesta. Aggiungere il certificato alla `Connection.AddClientCertificate`connessione utilizzando .

### <a name="connection-drops-after-authentication-times-out"></a>Interruzione della connessione dopo il timeout dell'autenticazione

Questo comportamento dipende dalla progettazione. Le credenziali di autenticazione non possono essere modificate mentre è attiva una connessione. per aggiornare le credenziali, la connessione deve essere interrotta e riavviata.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>OnConnected viene chiamato due volte quando si utilizza jQuery Mobile

La `initializePage` funzione di jQuery Mobile forza la riesecuzione degli script in ogni pagina, creando così una seconda connessione. Le soluzioni per questo problema includono:

- Includi il riferimento a jQuery Mobile prima del file JavaScript.
- Disattivare `initializePage` la `$.mobile.autoInitializePage = false`funzione impostando .
- Attendere che la pagina termini l'inizializzazione prima di avviare la connessione.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>I messaggi vengono ritardati nelle applicazioni Silverlight utilizzando gli eventi inviati dal serverMessages are delayed in Silverlight applications using Server Sent Events

I messaggi vengono ritardati quando si utilizzano eventi inviati dal server in Silverlight. Per forzare invece l'utilizzo del polling lungo, utilizzare quanto segue all'avvio della connessione:

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>"Autorizzazione negata" utilizzando il protocollo Forever Frame

Si tratta di un problema noto, descritto [qui](https://github.com/SignalR/SignalR/issues/1963). Questo sintomo può essere visualizzato utilizzando la libreria JQuery più recente; la soluzione consiste nel eseguire il downgrade dell'applicazione a JQuery 1.8.2.

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Errori di compilazione e lato serverCompilation and server-side errors

 La sezione seguente contiene le possibili soluzioni agli errori di runtime del compilatore e lato server. 

### <a name="reference-to-hub-instance-is-null"></a>Il riferimento all'istanza Hub è nullReference to Hub instance is null

Poiché viene creata un'istanza dell'hub per ogni connessione, non è possibile creare manualmente un'istanza di un hub nel codice. Per chiamare i metodi su un hub dall'esterno dell'hub stesso, vedere Come chiamare i [metodi client e gestire i gruppi dall'esterno della classe Hub](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) per informazioni su come ottenere un riferimento al contesto dell'hub.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session è null

Questo comportamento dipende dalla progettazione. SignalR non supporta lo stato di ASP.NET sessione, poiché l'abilitazione dello stato della sessione interromperebbe la messaggistica duplex.

### <a name="no-suitable-method-to-override"></a>Nessun metodo adatto per eseguire l'override

Questo errore può essere visualizzato se si utilizza codice di documentazione o blog meno recenti. Verificare che non si faccia riferimento a nomi di metodi `OnConnectedAsync`che sono stati modificati o deprecati (ad esempio ).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl è null

Questo comportamento dipende dalla progettazione. Questo membro è deprecato e non deve essere utilizzato.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>Errore "Una route denominata 'signalr.hubs' è già presente nella raccolta di route"

Questo errore verrà `MapHubs` visualizzato se viene chiamato due volte dall'applicazione. Alcune applicazioni `MapHubs` di esempio chiamano direttamente nel file dell'applicazione globale; altri effettuano la chiamata in una classe wrapper. Assicurarsi che l'applicazione non esedi entrambi.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Problemi di Visual Studio

In questa sezione vengono descritti i problemi riscontrati in Visual Studio.This section describes issues encountered in Visual Studio.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>Il nodo Documenti script non viene visualizzato in Esplora soluzioni

Alcune delle esercitazioni indirizzano l'utente al nodo "Script Documents" in Esplora soluzioni durante il debug. Questo nodo viene prodotto dal debugger JavaScript e verrà visualizzato solo durante il debug dei client browser in Internet Explorer; il nodo non verrà visualizzato se si utilizza Chrome o Firefox. Il debugger JavaScript non verrà inoltre eseguito se è in esecuzione un altro debugger client, ad esempio il debugger di Silverlight.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR non funziona su Visual Studio 2008 o versioni precedenti

Questo comportamento dipende dalla progettazione. SignalR richiede .NET Framework 4 o versione successiva; ciò richiede che le applicazioni SignalR vengano sviluppate in Visual Studio 2010 o versioni successive.

<a id="iis"></a>

## <a name="iis-issues"></a>Problemi di IIS

In questa sezione sono contenuti problemi con Internet Information Services.

### <a name="web-site-crashes-after-maphubs-call"></a>Sito Web si blocca dopo la chiamata MapHubs

Questo problema è stato risolto nell'ultima versione di SignalR. Verificare di utilizzare l'ultima versione rilasciata di SignalR aggiornando l'installazione utilizzando NuGet.

<a id="azure"></a>

## <a name="azure-issues"></a>Problemi di AzureAzure issues

Questa sezione contiene problemi con Microsoft Azure.This section contains issues with Microsoft Azure.

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>I messaggi non vengono ricevuti tramite il backplane di Azure dopo la modifica dei nomi degli argomentiMessages are not received through the Azure backplane after altering topic names

Gli argomenti usati dal backplane di Azure non sono destinati a essere configurabili dall'utente.
