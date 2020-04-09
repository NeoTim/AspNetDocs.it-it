---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: ASP.NET Guida all'API di SignalR Hubs - Client .NET (C) Documenti Microsoft
author: bradygaster
description: Questo documento fornisce un'introduzione all'uso dell'API Hubs per SignalR versione 2 nei client .NET, ad esempio Windows Store (WinRT), WPF, Silverlight e contro...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: d3536f1c15cd7dad7cd660becf0577e5c131f707
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676335"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-c"></a>Guida all'API di ASP.NET Dividi Di SignalR - Client .NET (C )The SignalR Hubs API Guide - .NET Client (C

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo documento fornisce un'introduzione all'uso dell'API Hubs per SignalR versione 2 nei client .NET, ad esempio Windows Store (WinRT), WPF, Silverlight e applicazioni console.
>
> L'API Hubs SignalR consente di effettuare chiamate di procedura remota (RPC) da un server ai client connessi e dai client al server. Nel codice server, si definiscono i metodi che possono essere chiamati dai client e si chiamano i metodi eseguiti sul client. Nel codice client si definiscono i metodi che possono essere chiamati dal server e si chiamano i metodi eseguiti sul server. SignalR si occupa di tutto l'impianto idraulico client-server per voi.
>
> SignalR offre anche un'API di livello inferiore denominata connessioni permanenti. Per un'introduzione a SignalR, Hub e connessioni permanenti oppure per un'esercitazione che illustra come compilare un'applicazione SignalR completa, vedere [SignalR - Getting Started](../getting-started/index.md).
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

- [Configurazione client](#clientsetup)
- [Come stabilire una connessione](#establishconnection)

    - [Connessioni tra domini da client Silverlight](#slcrossdomain)
- [Come configurare la connessione](#configureconnection)

    - [Come impostare il numero massimo di connessioni simultanee nei client WPF](#maxconnections)
    - [Come specificare i parametri della stringa di queryHow to specify query string parameters](#querystring)
    - [Come specificare il metodo di trasporto](#transport)
    - [Come specificare le intestazioni HTTP](#httpheaders)
    - [Come specificare i certificati client](#clientcertificate)
- [Come creare il proxy hub](#proxy)
- [Come definire i metodi sul client che il server può chiamare](#callclient)

    - [Metodi senza parametri](#clientmethodswithoutparms)
    - [Metodi con parametri, specifica dei tipi di parametro](#clientmethodswithparmtypes)
    - [Metodi con parametri, specifica di oggetti dinamici per i parametri](#clientmethodswithdynamparms)
    - [Come rimuovere un gestore](#removehandler)
- [Come chiamare i metodi server dal client](#callserver)
- [Come gestire gli eventi di durata della connessioneHow to handle connection lifetime events](#connectionlifetime)
- [Come gestire gli errori](#handleerrors)
- [Come abilitare la registrazione lato clientHow to enable client-side logging](#logging)
- [Esempi di codice WPF, Silverlight e applicazione console per i metodi client che il server può chiamare](#wpfsl)

Per progetti client .NET di esempio, vedere le risorse seguenti:For a sample .NET client projects, see the following resources:

- [gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) in GitHub.com (WinRT, Silverlight, esempi di app console).
- [DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) in GitHub.com (esempio WPF).
- [SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) su GitHub.com (esempio di applicazione Console).

Per la documentazione su come programmare i client server o JavaScript, vedere le risorse seguenti:

- [Guida all'API di SignalR Hubs - Server](hubs-api-guide-server.md)
- [Guida all'API di SignalR Hubs - Client JavaScript](hubs-api-guide-javascript-client.md)

I collegamenti agli argomenti di riferimento per le API sono relativi alla versione .NET 4.5 dell'API. Se si utilizza .NET 4, vedere [la versione .NET 4 degli argomenti API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>Configurazione client

Installare il pacchetto [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet (non il pacchetto [Microsoft.AspNet.SignalR).](http://nuget.org/packages/microsoft.aspnet.signalr) Questo pacchetto supporta winRT, Silverlight, WPF, applicazione console e client Windows Phone, sia per .NET 4 che per .NET 4.5.

Se la versione di SignalR che hai sul client è diversa dalla versione che hai sul server, SignalR è spesso in grado di adattarsi alla differenza. Ad esempio, un server che esegue SignalR versione 2 supporterà i client in cui è installato 1.1.x, nonché i client in cui è installata la versione 2. Se la differenza tra la versione sul server e la versione sul client è troppo grande o se `InvalidOperationException` il client è più recente del server, SignalR genera un'eccezione quando il client tenta di stabilire una connessione. Il messaggio di`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`errore è " ".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Come stabilire una connessione

Prima di poter stabilire una connessione, `HubConnection` è necessario creare un oggetto e un proxy. Per stabilire la connessione, chiamare il `Start` metodo sull'oggetto. `HubConnection`

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,5)]

> [!NOTE]
> Per i client JavaScript è necessario registrare `Start` almeno un gestore eventi prima di chiamare il metodo per stabilire la connessione. Questa operazione non è necessaria per i client .NET. Per i client JavaScript, il codice proxy generato crea automaticamente proxy per tutti gli hub presenti nel server e la registrazione di un gestore indica quale Hub intende utilizzare il client. Ma per un client .NET si creano proxy Hub manualmente, in modo SignalR presuppone che si utilizzerà qualsiasi Hub che si crea un proxy per.

Il codice di esempio usa l'URL predefinito "/signalr" per connettersi al servizio SignalR. Per informazioni su come specificare un URL di base diverso, vedere [Guida all'API di ASP.NET SignalR Hubs - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).

Il `Start` metodo viene eseguito in modo asincrono. Per assicurarsi che le righe di codice successive non vengano eseguite fino a quando non viene stabilita la connessione, utilizzare `await` in un metodo asincrono ASP.NET 4.5 o `.Wait()` in un metodo sincrono. Non utilizzare `.Wait()` in un client WinRT.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Connessioni tra domini da client Silverlight

Per informazioni su come abilitare le connessioni tra domini dai client Silverlight, vedere [Rendere un servizio disponibile attraverso i limiti](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx)del dominio .

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Come configurare la connessione

Prima di stabilire una connessione, è possibile specificare una delle seguenti opzioni:

- Limite di connessioni simultanee.
- Parametri della stringa di query.
- Metodo di trasporto.
- Intestazioni HTTP.
- Certificati client.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>Come impostare il numero massimo di connessioni simultanee nei client WPF

Nei client WPFWPF potrebbe essere necessario aumentare il numero massimo di connessioni simultanee rispetto al valore predefinito pari a 2.In WPFWPF client, you might have to increase the maximum number of concurrent connections from its default value of 2. Il valore consigliato è 10.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=5)]

Per ulteriori informazioni, vedere [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Come specificare i parametri della stringa di queryHow to specify query string parameters

Se si desidera inviare dati al server quando il client si connette, è possibile aggiungere parametri di stringa di query all'oggetto connessione. Nell'esempio seguente viene illustrato come impostare un parametro della stringa di query nel codice client.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

Nell'esempio seguente viene illustrato come leggere un parametro di stringa di query nel codice server.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Come specificare il metodo di trasporto

Come parte del processo di connessione, un client SignalR in genere negozia con il server per determinare il trasporto migliore supportato sia dal server che dal client. Se si conosce già il trasporto che si desidera utilizzare, è possibile ignorare questo processo di negoziazione. Per specificare il metodo di trasporto, passare un oggetto di trasporto al metodo Start.To specify the transport method, pass in a transport object to the Start method. Nell'esempio seguente viene illustrato come specificare il metodo di trasporto nel codice client.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=5)]

Lo spazio dei nomi [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) include le classi seguenti che è possibile utilizzare per specificare il trasporto.

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport (Informazioni su due)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponibile solo quando sia il server che il client utilizzano .NET 4.5.)
- [AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (sceglie automaticamente il trasporto migliore supportato sia dal client che dal server. Questo è il trasporto predefinito. Passare questo al `Start` metodo ha lo stesso effetto di non passare nulla.)

Il trasporto ForeverFrame non è incluso in questo elenco perché viene utilizzato solo dai browser.

Per informazioni su come controllare il metodo di trasporto nel codice server, vedere [ASP.NET Guida all'API di HubS SignalR - Server - Come ottenere informazioni sul client dalla proprietà Context](hubs-api-guide-server.md#contextproperty). Per ulteriori informazioni sui trasporti e i fallback, vedere [Introduzione a SignalR - Trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Come specificare le intestazioni HTTP

Per impostare le `Headers` intestazioni HTTP, utilizzare la proprietà nell'oggetto connessione. Nell'esempio seguente viene illustrato come aggiungere un'intestazione HTTP.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Come specificare i certificati client

Per aggiungere certificati client, utilizzare il `AddClientCertificate` metodo sull'oggetto connessione.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Come creare il proxy hub

Per definire i metodi sul client che un hub può chiamare dal server e richiamare i metodi su `CreateHubProxy` un hub nel server, creare un proxy per l'hub chiamando sull'oggetto connessione. La stringa passata `CreateHubProxy` a è il nome della classe Hub `HubName` o il nome specificato dall'attributo, se ne è stato utilizzato uno sul server. La corrispondenza dei nomi non prevede alcuna distinzione tra maiuscole e minuscole.

**Classe Hub sul server**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Creare il proxy client per la classe HubCreate client proxy for the Hub class**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=3)]

Se decori la classe `HubName` Hub con un attributo, usa quel nome.

**Classe Hub sul server**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

**Creare il proxy client per la classe HubCreate client proxy for the Hub class**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=3)]

Se si `HubConnection.CreateHubProxy` chiama più `hubName`volte con lo `IHubProxy` stesso , si ottiene lo stesso oggetto memorizzato nella cache.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Come definire i metodi sul client che il server può chiamare

Per definire un metodo che il server può `On` chiamare, utilizzare il metodo del proxy per registrare un gestore eventi.

La corrispondenza del nome del metodo non fa distinzione tra maiuscole e minuscole. Ad `Clients.All.UpdateStockPrice` esempio, sul server `updateStockPrice` `updatestockprice`verrà `UpdateStockPrice` eseguito , o sul client.

Piattaforme client diverse hanno requisiti diversi per la modalità di scrittura del codice del metodo per aggiornare l'interfaccia utente. Gli esempi illustrati sono per i client WinRT (Windows Store .NET). Wpf, Silverlight e esempi di applicazioni console sono disponibili in [una sezione separata più avanti in questo argomento.](#wpfsl)

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Metodi senza parametri

Se il metodo che si sta gestendo non dispone di `On` parametri, utilizzare l'overload non generico del metodo:

**Codice server che chiama il metodo client senza parametriServer code calling client method without parameters**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**Codice client WinRT per il metodo chiamato dal server senza parametri[(vedere esempi wpf e Silverlight più avanti in questo argomento](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Metodi con parametri, che specificano i tipi di parametro

Se il metodo che si sta gestendo dispone di parametri, `On` specificare i tipi dei parametri come tipi generici del metodo. Esistono overload generici `On` del metodo per consentire di specificare fino a 8 parametri (4 in Windows Phone 7). Nell'esempio seguente, un parametro `UpdateStockPrice` viene inviato al metodo.

**Codice server che chiama il metodo client con un parametroServer code calling client method with a parameter**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**La classe Stock utilizzata per il parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

**Codice client WinRT per un metodo chiamato dal server con un parametro ([vedere esempi wpf e Silverlight più avanti in questo argomento](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Metodi con parametri, specifica di oggetti dinamici per i parametri

In alternativa alla specifica dei parametri `On` come tipi generici del metodo, è possibile specificare i parametri come oggetti dinamici:

**Codice server che chiama il metodo client con un parametroServer code calling client method with a parameter**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**La classe Stock utilizzata per il parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

**Codice client WinRT per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro[(vedere esempi WPF e Silverlight più avanti in questo argomento](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Come rimuovere un gestore

Per rimuovere un gestore, chiamare il relativo `Dispose` metodo.

**Codice client per un metodo chiamato dal server**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Codice client per rimuovere il gestore**

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Come chiamare i metodi server dal client

Per chiamare un metodo sul `Invoke` server, utilizzare il metodo sul proxy Hub.

Se il metodo server non ha alcun valore restituito, utilizzare l'overload non generico del `Invoke` metodo.

**Codice server per un metodo senza valore restituito**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Codice client che chiama un metodo che non ha alcun valore restituito**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Se il metodo server ha un valore restituito, specificare `Invoke` il tipo restituito come tipo generico del metodo.

**Codice server per un metodo che ha un valore restituito e accetta un parametro di tipo complessoServer code for a method that has a return value and takes a complex type parameter**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**La classe Stock utilizzata per il parametro e il valore restituito**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

**Codice client che chiama un metodo che ha un valore restituito e accetta un parametro di tipo complesso, in un ASP.NET 4.5 metodo asincrono**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**Codice client che chiama un metodo che ha un valore restituito e accetta un parametro di tipo complesso, in un metodo sincrono**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

Il `Invoke` metodo viene eseguito `Task` in modo asincrono e restituisce un oggetto. Se non si `await` specifica `.Wait()`o , la riga di codice successiva verrà eseguita prima che il metodo richiamato abbia terminato l'esecuzione.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Come gestire gli eventi di durata della connessioneHow to handle connection lifetime events

SignalR fornisce i seguenti eventi di durata della connessione che è possibile gestire:

- `Received`: generato quando vengono ricevuti dati sulla connessione. Fornisce i dati ricevuti.
- `ConnectionSlow`: generato quando il client rileva una connessione lenta o frequente.
- `Reconnecting`: generato quando il trasporto sottostante inizia la riconnessione.
- `Reconnected`: generato quando il trasporto sottostante è stato riconnesso.
- `StateChanged`: generato quando cambia lo stato della connessione. Fornisce lo stato precedente e il nuovo stato. Per informazioni sui valori dello stato di connessione, vedere [ConnectionState Enumerazione](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: generato quando la connessione si è disconnessa.

Ad esempio, se si desidera visualizzare messaggi di avviso per gli errori che non sono irreversibili ma `ConnectionSlow` causano problemi di connessione intermittenti, ad esempio lentezza o frequente eliminazione della connessione, gestire l'evento.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

Per ulteriori informazioni, vedere Informazioni e gestione degli eventi di durata della [connessione in SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Come gestire gli errori

Se non si abilitano in modo esplicito i messaggi di errore dettagliati sul server, l'oggetto eccezione restituito da SignalR dopo che un errore contiene informazioni minime sull'errore. Ad esempio, se `newContosoChatMessage` una chiamata a ha esito`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`negativo, il messaggio di errore nell'oggetto di errore contiene " " L'invio di messaggi di errore dettagliati ai client nell'ambiente di produzione non è consigliato per motivi di sicurezza, ma se si desidera abilitare i messaggi di errore dettagliati per la risoluzione dei problemi, utilizzare il codice seguente sul server.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Per gestire gli errori che SignalR genera, `Error` è possibile aggiungere un gestore per l'evento nell'oggetto connessione.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

Per gestire gli errori dalle chiamate al metodo, eseguire il wrapping del codice in un blocco try-catch.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Come abilitare la registrazione lato clientHow to enable client-side logging

Per abilitare la registrazione `TraceLevel` sul `TraceWriter` lato client, impostare le proprietà e sull'oggetto connessione.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=3-4)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>Esempi di codice WPF, Silverlight e applicazione console per i metodi client che il server può chiamare

Gli esempi di codice illustrati in precedenza per la definizione dei metodi client che il server può chiamare si applicano ai client WinRT. Negli esempi seguenti viene illustrato il codice equivalente per WPF, Silverlight e i client dell'applicazione console.

### <a name="methods-without-parameters"></a>Metodi senza parametri

**Codice client WPF per il metodo chiamato dal server senza parametriWPF client code for method called from server without parameters**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Codice client Silverlight per il metodo chiamato dal server senza parametri**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Codice client dell'applicazione console per il metodo chiamato dal server senza parametri**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Metodi con parametri, che specificano i tipi di parametro

**Codice client WPF per un metodo chiamato dal server con un parametroWPF client code for a method called from server with a parameter**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Codice client Silverlight per un metodo chiamato dal server con un parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Codice client dell'applicazione console per un metodo chiamato dal server con un parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Metodi con parametri, specifica di oggetti dinamici per i parametri

**Codice client WPF per un metodo chiamato dal server con un parametro, usando un oggetto dinamico per il parametroWPF client code for a method called from server with a parameter, using a dynamic object for the parameter**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Codice client Silverlight per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Codice client dell'applicazione console per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
