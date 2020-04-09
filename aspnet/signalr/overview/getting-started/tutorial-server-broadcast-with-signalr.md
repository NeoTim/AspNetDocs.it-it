---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Tutorial: Trasmissione del server con SignalR 2 Documenti Microsoft'
author: tdykstra
description: In questa esercitazione viene illustrato come creare un'applicazione Web che utilizza ASP.NET SignalR 2 per fornire funzionalità di trasmissione server.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14924109fff8db3e537e6bc08b6dc868792ee660
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676097"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a>Esercitazione: Trasmissione server con SignalR 2Tutorial: Server broadcast with SignalR 2

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

In questa esercitazione viene illustrato come creare un'applicazione Web che utilizza ASP.NET SignalR 2 per fornire funzionalità di trasmissione server. Trasmissione server significa che il server avvia le comunicazioni inviate ai client.

L'applicazione che verrà creata in questa esercitazione simula un ticker azionario, uno scenario tipico per la funzionalità di trasmissione del server. Periodicamente, il server aggiorna in modo casuale i prezzi delle azioni e trasmette gli aggiornamenti a tutti i client connessi. Nel browser, i numeri e **Change** i **%** simboli nelle colonne Cambia e Cambiano dinamicamente in risposta alle notifiche dal server. Se si aprono altri browser nello stesso URL, tutti mostrano gli stessi dati e le stesse modifiche ai dati contemporaneamente.

![Crea web](tutorial-server-broadcast-with-signalr/_static/image1.png)

In questa esercitazione:

> [!div class="checklist"]
> * Creare il progetto
> * Configurare il codice del server
> * Esaminare il codice server
> * Impostare il codice client
> * Esaminare il codice client
> * Test dell'applicazione
> * Abilitazione della registrazione

> [!IMPORTANT]
> Se non si desidera utilizzare i passaggi per la compilazione dell'applicazione, è possibile installare il pacchetto SignalR.Sample in un nuovo progetto di applicazione Web Empty ASP.NET . Se si installa il pacchetto NuGet senza eseguire i passaggi di questa esercitazione, è necessario seguire le istruzioni nel file *readme.txt.* Per eseguire il pacchetto è necessario aggiungere una `ConfigureSignalR` classe di avvio OWIN che chiama il metodo nel pacchetto installato. Se non si aggiunge la classe di avvio OWIN, verrà visualizzato un errore. Vedere la sezione [Installare l'esempio StockTicker](#install-the-stockticker-sample) di questo articolo.

## <a name="prerequisites"></a>Prerequisiti

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con il carico di lavoro **Sviluppo ASP.NET e Web**.

## <a name="create-the-project"></a>Creare il progetto

In questa sezione viene illustrato come utilizzare Visual Studio 2017 per creare un'applicazione Web vuota ASP.NET.

1. In Visual Studio creare un'applicazione Web ASP.NET.

    ![Crea web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. Nella finestra **Nuova applicazione Web ASP.NET - SignalR.StockTicker** lasciare deselezionata **Vuota** e selezionare **OK**.

## <a name="set-up-the-server-code"></a>Configurare il codice del server

In questa sezione viene impostato il codice in esecuzione nel server.

### <a name="create-the-stock-class"></a>Creare la classe Stock

Si inizia creando la classe di modello *Stock* che verrà utilizzata per archiviare e trasmettere informazioni su un magazzino.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e **scegliere Aggiungi** > **classe**.

1. Denominare la classe *Stock* e aggiungerla al progetto.

1. Sostituire il codice nel file Stock.cs con questo codice:Replace the code in the *Stock.cs* file with this code:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Le due proprietà che verranno impostate `Symbol` al momento della creazione delle `Price`scorte sono (ad esempio, MSFT per Microsoft) e . Le altre proprietà dipendono da `Price`come e quando si imposta . La prima volta `Price`che si imposta , `DayOpen`il valore viene propagato a . Successivamente, quando si `Price`imposta , l'app calcola i `Change` valori delle proprietà e `PercentChange` in base alla differenza tra `Price` e `DayOpen`.

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a>Creare le classi StockTickerHub e StockTickerCreate the StockTickerHub and StockTicker classes

Utilizzerai l'API Hub SignalR per gestire l'interazione da server a client. Una `StockTickerHub` classe che deriva dalla `Hub` classe SignalR gestirà la ricezione di connessioni e chiamate ai metodi dai client. È inoltre necessario gestire i `Timer` dati azionari ed eseguire un oggetto. L'oggetto `Timer` attiverà periodicamente gli aggiornamenti dei prezzi indipendentemente dalle connessioni client. Non è possibile inserire queste `Hub` funzioni in una classe, perché gli hub sono temporanei. L'app `Hub` crea un'istanza di classe per ogni attività nell'hub, ad esempio connessioni e chiamate dal client al server. Così il meccanismo che mantiene i dati di magazzino, aggiorna i prezzi, e trasmette gli aggiornamenti dei prezzi deve funzionare in una classe separata. Verrà denominato `StockTicker`il nome della classe .

![Trasmissione da StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

Si desidera che nel `StockTicker` server venga eseguita solo un'istanza della classe, `StockTickerHub` pertanto è necessario `StockTicker` impostare un riferimento da ogni istanza all'istanza singleton. La `StockTicker` classe deve trasmettere ai client perché ha i `StockTicker` dati azionari `Hub` e attiva gli aggiornamenti, ma non è una classe. La `StockTicker` classe deve ottenere un riferimento all'oggetto contesto di connessione SignalR Hub.The class has to get a reference to the SignalR Hub connection context object. Può quindi utilizzare l'oggetto contesto di connessione SignalR per trasmettere ai client.

#### <a name="create-stocktickerhubcs"></a>Crea StockTickerHub.cs

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e **scegliere Aggiungi** > **nuovo elemento**.

1. In **Aggiungi nuovo elemento - SignalR.StockTicker**, selezionare **Installed** > **Visual C,** > **Web** > **SignalR** , quindi selezionare **SignalR Hub Class (v2)**.

1. Denominare la classe *StockTickerHub* e aggiungerla al progetto.

    Questo passaggio consente di creare il file di classe *StockTickerHub.cs.* Contemporaneamente, aggiunge un set di file di script e riferimenti a assembly che supporta SignalR al progetto.

1. Sostituire il codice nel file *di StockTickerHub.cs* con questo codice:Replace the code in the StockTickerHub.cs file with this code:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. Salvare il file.

L'app usa la classe [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) per definire i metodi che i client possono chiamare sul server. Si sta definendo `GetAllStocks()`un metodo: . Quando un client si connette inizialmente al server, chiamerà questo metodo per ottenere un elenco di tutti i titoli azionari con i prezzi correnti. Il metodo può essere `IEnumerable<Stock>` eseguito in modo sincrono e restituito perché restituisce dati dalla memoria.

Se il metodo deve ottenere i dati eseguendo un'operazione che comporta l'attesa, `Task<IEnumerable<Stock>>` ad esempio una ricerca di database o una chiamata al servizio Web, è necessario specificare come valore restituito per abilitare l'elaborazione asincrona. Per ulteriori informazioni, vedere [Guida all'API ASP.NET HubSignalR - Server - Quando eseguire in modo asincrono.](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods)

L'attributo `HubName` specifica il modo in cui l'app farà riferimento all'Hub nel codice JavaScript sul client. Il nome predefinito sul client se non si utilizza questo attributo, è una versione camelCase del nome della classe, che in questo caso sarebbe `stockTickerHub`.

Come si vedrà più avanti `StockTicker` quando si crea la classe, l'app `Instance` crea un'istanza singleton di tale classe nella relativa proprietà statica. L'istanza singleton di `StockTicker` è in memoria indipendentemente dal numero di client che si connettono o si disconnettono. Tale istanza è `GetAllStocks()` ciò che il metodo utilizza per restituire le informazioni sulle scorte correnti.

#### <a name="create-stocktickercs"></a>Creare StockTicker.cs

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e **scegliere Aggiungi** > **classe**.

1. Denominare la classe *StockTicker* e aggiungerla al progetto.

1. Sostituire il codice nel file *di StockTicker.cs* con il codice seguente:Replace the code in the StockTicker.cs file with this code:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

Poiché tutti i thread eseguiranno la stessa istanza del codice StockTicker, la classe StockTicker deve essere thread-safe.

### <a name="examine-the-server-code"></a>Esaminare il codice server

Se esamini il codice server, ti aiuterà a capire come funziona l'app.

#### <a name="storing-the-singleton-instance-in-a-static-field"></a>Memorizzazione dell'istanza singleton in un campo statico

Il codice inizializza `_instance` il campo statico `Instance` che supporta la proprietà con un'istanza della classe. Poiché il costruttore è privato, è l'unica istanza della classe che l'app può creare. L'app usa [l'inizializzazione differita](/dotnet/framework/performance/lazy-initialization) per il `_instance` campo. Non è per motivi di prestazioni. È per assicurarsi che la creazione dell'istanza è thread-safe.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

Ogni volta che un client si connette al server, una nuova istanza della classe StockTickerHub in `StockTicker.Instance` esecuzione in un thread separato `StockTickerHub` ottiene l'istanza singleton StockTicker dalla proprietà statica, come illustrato in precedenza nella classe.

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Memorizzazione dei dati azionari in un ConcurrentDictionary

Il costruttore `_stocks` inizializza la raccolta con `GetAllStocks` alcuni dati di stock di esempio e restituisce le azioni. Come si è visto in precedenza, `StockTickerHub.GetAllStocks`questo insieme di azioni `Hub` viene restituito da , che è un metodo server nella classe che i client possono chiamare.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

La raccolta stocks è definita come un tipo [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) per l'indipendenza thread. In alternativa, è possibile utilizzare un oggetto [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) e bloccare in modo esplicito il dizionario quando si apportano modifiche.

Per questa applicazione di esempio, è possibile archiviare i dati dell'applicazione in `StockTicker` memoria e perdere i dati quando l'app elimina l'istanza. In un'applicazione reale, si funzionerebbe con un archivio dati back-end come un database.

#### <a name="periodically-updating-stock-prices"></a>Aggiornamento periodico dei prezzi delle azioni

Il costruttore `Timer` avvia un oggetto che chiama periodicamente metodi che aggiornano i prezzi delle azioni in modo casuale.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 `Timer`calls `UpdateStockPrices`, che passa null nel parametro state. Prima di aggiornare i prezzi, `_updateStockPricesLock` l'app accetta un blocco sull'oggetto. Il codice controlla se un altro thread sta `TryUpdateStockPrice` già aggiornando i prezzi e quindi chiama su ogni azione nell'elenco. Il `TryUpdateStockPrice` metodo decide se modificare il prezzo delle azioni e quanto modificarlo. Se il prezzo delle azioni `BroadcastStockPrice` cambia, l'app chiama per trasmettere la modifica del prezzo delle azioni a tutti i client connessi.

Flag `_updatingStockPrices` designato [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) per assicurarsi che sia thread-safe.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

In un'applicazione `TryUpdateStockPrice` reale, il metodo chiamerebbe un servizio Web per cercare il prezzo. In questo codice, l'app usa un generatore di numeri casuali per apportare modifiche in modo casuale.

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Ottenere il contesto SignalR in modo che la classe StockTicker possa trasmettere ai client

Poiché le variazioni `StockTicker` di prezzo hanno origine qui nell'oggetto, è l'oggetto che deve chiamare un `updateStockPrice` metodo su tutti i client connessi. In `Hub` una classe, si dispone di un'API per chiamare i metodi client, ma `StockTicker` non deriva dalla `Hub` classe e non dispone di un riferimento ad alcun `Hub` oggetto. Per trasmettere ai `StockTicker` client connessi, la classe deve `StockTickerHub` ottenere l'istanza del contesto SignalR per la classe e utilizzarla per chiamare i metodi sui client.

Il codice ottiene un riferimento al contesto SignalR quando crea l'istanza della classe singleton, `Clients` passa tale riferimento al costruttore e il costruttore lo inserisce nella proprietà.

Esistono due motivi per cui si desidera ottenere il contesto una sola volta: ottenere il contesto è un'attività costosa e ottenerlo una volta assicura che l'app mantenga l'ordine previsto dei messaggi inviati ai client.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

Ottenere `Clients` la proprietà del contesto `StockTickerClient` e inserirla nella proprietà consente di scrivere `Hub` codice per chiamare i metodi client che ha lo stesso aspetto di una classe. Ad esempio, per trasmettere a `Clients.All.updateStockPrice(stock)`tutti i client è possibile scrivere .

Il `updateStockPrice` metodo che si `BroadcastStockPrice` sta chiamando in non esiste ancora. Si aggiungerà in un secondo momento quando si scrive il codice che viene eseguito sul client. Puoi fare `updateStockPrice` riferimento `Clients.All` a questo perché è dinamico, il che significa che l'app valuterà l'espressione in fase di esecuzione. Quando viene eseguita questa chiamata al metodo, SignalR invierà il nome del metodo `updateStockPrice`e il valore del parametro al client e, se il client dispone di un metodo denominato , l'app chiamerà tale metodo e gli passerà il valore del parametro.

`Clients.All`significa inviare a tutti i clienti. SignalR offre altre opzioni per specificare i client o gruppi di client a cui inviare. Per ulteriori informazioni, vedere [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Registrare il percorso SignalR

Il server deve sapere quale URL intercettare e indirizzare a SignalR. A tale scopo, aggiungere una classe di avvio OWIN:To do that, add an OWIN startup class:

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e **scegliere Aggiungi** > **nuovo elemento**.

1. In **Aggiungi nuovo elemento - SignalR.StockTicker** selezionare **Installed** > **Visual C,** > **Web** , quindi **selezionare OWIN Startup Class**.

1. *Denominare* la classe Startup e selezionare **OK**.

1. Sostituire il codice predefinito nel file Startup.cs con questo codice:Replace the default code in the *Startup.cs* file with this code:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

La configurazione del codice server è stata completata. Nella sezione successiva, il client verrà configurato.

## <a name="set-up-the-client-code"></a>Impostare il codice client

In questa sezione viene impostato il codice in esecuzione sul client.

### <a name="create-the-html-page-and-javascript-file"></a>Creare la pagina HTML e il file JavaScript

La pagina HTML visualizzerà i dati e il file JavaScript organizzerà i dati.

#### <a name="create-stocktickerhtml"></a>Creare StockTicker.html

In primo luogo, si aggiungerà il client HTML.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e **scegliere Aggiungi** > **pagina HTML**.

1. Assegnare al file il nome *StockTicker* e selezionare **OK**.

1. Sostituire il codice predefinito nel file *StockTicker.html* con questo codice:

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    Il codice HTML crea una tabella con cinque colonne, una riga di intestazione e una riga di dati con una singola cella che si estende su tutte e cinque le colonne. La riga di dati mostra "caricamento..." momentaneamente all'avvio dell'app. Il codice JavaScript rimuoverà tale riga e aggiungerà al suo posto le righe con i dati azionari recuperati dal server.

    I tag di script specificano:

    * File di script jQuery.

    * Il file di script principale SignalR.

    * Il file di script dei proxy SignalR.

    * Un file di script StockTicker che verrà creato in un secondo momento.

    L'app genera dinamicamente il file di script dei proxy SignalR. Specifica l'URL "/signalr/hubs" e definisce i metodi proxy per i metodi `StockTickerHub.GetAllStocks`nella classe Hub, in questo caso per . Se si preferisce, è possibile generare questo file JavaScript manualmente utilizzando [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/). Non dimenticare di disabilitare la `MapHubs` creazione di file dinamici nella chiamata al metodo.

1. In **Esplora soluzioni**espandere **Script**.

    Le librerie di script per jQuery e SignalR sono visibili nel progetto.

    > [!IMPORTANT]
    > Il gestore di pacchetti installerà una versione successiva degli script SignalR.

1. Aggiornare i riferimenti agli script nel blocco di codice in modo che corrispondano alle versioni dei file di script nel progetto.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse su *StockTicker.html*, quindi **scegliere Imposta come pagina iniziale**.

#### <a name="create-stocktickerjs"></a>Creare StockTicker.jsCreate StockTicker.js

Ora crea il file JavaScript.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e **scegliere Aggiungi** > **file JavaScript**.

1. Assegnare al file il nome *StockTicker* e selezionare **OK**.

1. Aggiungere questo codice al file *StockTicker.js:*

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a>Esaminare il codice client

Se si esamina il codice client, verrà illustrato come il codice client interagisce con il codice server per far funzionare l'app.

#### <a name="starting-the-connection"></a>Avvio della connessione

`$.connection`si riferisce ai proxy SignalR. Il codice ottiene un riferimento `StockTickerHub` al proxy per `ticker` la classe e lo inserisce nella variabile. Il nome del proxy è il `HubName` nome impostato dall'attributo:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

Dopo aver definito tutte le variabili e le funzioni, l'ultima riga di codice `start` nel file inizializza la connessione SignalR chiamando la funzione SignalR. La `start` funzione viene eseguita in modo asincrono e restituisce un [oggetto jQuery Deferred](http://api.jquery.com/category/deferred-object/). È possibile chiamare la funzione done per specificare la funzione da chiamare quando l'app termina l'azione asincrona.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a>Ottenere tutte le scorte

La `init` funzione `getAllStocks` chiama la funzione sul server e utilizza le informazioni restituite dal server per aggiornare la tabella delle scorte. Si noti che, per impostazione predefinita, è necessario utilizzare camelCasing sul client anche se il nome del metodo è pascal-cased sul server. La regola camelCasing si applica solo ai metodi, non agli oggetti. Ad esempio, si `stock.Symbol` `stock.Price`fa `stock.symbol` riferimento `stock.price`a e , non o .

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

Nel `init` metodo, l'app crea HTML per una riga di tabella `formatStock` per ogni `stock` oggetto stock ricevuto `supplant` dal server chiamando `rowTemplate` le proprietà `stock` di formato dell'oggetto e quindi chiamando per sostituire i segnaposto nella variabile con i valori delle proprietà dell'oggetto. Il codice HTML risultante viene quindi aggiunto alla tabella azionaria.

> [!NOTE]
> Si `init` chiama passandolo come `callback` funzione che viene `start` eseguita al termine della funzione asincrona. Se si `init` chiamasse come istruzione `start`JavaScript separata dopo la chiamata, la funzione avrà esito negativo perché verrebbe eseguita immediatamente senza attendere che la funzione start completi di stabilire la connessione. In tal caso, la `init` funzione `getAllStocks` tenterebbe di chiamare la funzione prima che l'app stabilisca una connessione al server.

#### <a name="getting-updated-stock-prices"></a>Ottenere prezzi delle azioni aggiornati

Quando il server modifica il prezzo di `updateStockPrice` un titolo, chiama i client connessi su. L'app aggiunge la funzione alla `stockTicker` proprietà client del proxy per renderla disponibile per le chiamate dal server.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

La `updateStockPrice` funzione formatta un oggetto stock ricevuto dal server `init` in una riga di tabella nello stesso modo della funzione. Anziché aggiungere la riga alla tabella, trova la riga corrente del grezzo nella tabella e sostituisce tale riga con quella nuova.

## <a name="test-the-application"></a>Test dell'applicazione

Puoi testare l'app per assicurarti che funzioni. Vedrai tutte le finestre del browser visualizzare la tabella azionaria dal vivo con i prezzi delle azioni fluttuanti.

1. Nella barra degli strumenti, attivare **Debug script** e quindi selezionare il pulsante di riproduzione per eseguire l'app in modalità Debug.

    ![Screenshot dell'attivazione della modalità di debug e della selezione della riproduzione da parte dell'utente.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    Si aprirà una finestra del browser con la **Tabella stock dal vivo**. La tabella delle scorte mostra inizialmente il "caricamento..." linea, quindi, dopo un breve periodo di tempo, l'applicazione mostra i dati azionari iniziali, e poi i prezzi delle azioni iniziano a cambiare.

1. Copiare l'URL dal browser, aprire altri due browser e incollare gli URL nelle barre degli indirizzi.

    La visualizzazione iniziale delle scorte è la stessa del primo browser e le modifiche avvengono contemporaneamente.

1. Chiudere tutti i browser, aprire un nuovo browser e passare allo stesso URL.

    L'oggetto singleton StockTicker continuava a essere eseguito nel server. La **tabella Live Stock** mostra che le azioni hanno continuato a cambiare. La tabella iniziale non viene visualizzata con cifre di modifica pari a zero.

1. Chiudere il browser.

## <a name="enable-logging"></a>Abilitazione della registrazione

SignalR dispone di una funzione di registrazione incorporata che è possibile abilitare sul client per facilitare la risoluzione dei problemi. In questa sezione si abilita la registrazione e vengono visualizzati esempi che illustrano come i registri indicano quale dei seguenti metodi di trasporto Viene utilizzato da SignalR:

* [WebSockets](http://en.wikipedia.org/wiki/WebSocket), supportato da IIS 8 e browser correnti.

* [Eventi inviati dal server,](http://en.wikipedia.org/wiki/Server-sent_events)supportati da browser diversi da Internet Explorer.

* [Cornice per sempre](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supportata da Internet Explorer.

* [Ajax lungo polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supportato da tutti i browser.

Per ogni connessione specificata, SignalR sceglie il metodo di trasporto migliore che sia il server che il supporto client.

1. Aprire *StockTicker.js*.

1. Aggiungere questa riga di codice evidenziata per abilitare la registrazione immediatamente prima del codice che inizializza la connessione alla fine del file:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. Premere **F5** per eseguire il progetto.

1. Aprire la finestra degli strumenti di sviluppo del browser e selezionare la console per visualizzare i log. Potrebbe essere necessario aggiornare la pagina per visualizzare i registri di SignalR che negoziano il metodo di trasporto per una nuova connessione.

    * Se si esegue Internet Explorer 10 in Windows 8 (IIS 8), il metodo di trasporto è **WebSockets**.

    * Se si esegue Internet Explorer 10 in Windows 7 (IIS 7.5), il metodo di trasporto è **iframe**.

    * Se si esegue Firefox 19 in Windows 8 (IIS 8), il metodo di trasporto è **WebSockets**.

        > [!TIP]
        > In Firefox, installare il componente aggiuntivo Firebug per ottenere una finestra della console.

    * Se si esegue Firefox 19 in Windows 7 (IIS 7.5), il metodo di trasporto è eventi **inviati dal server.**

## <a name="install-the-stockticker-sample"></a>Installare l'esempio StockTickerInstall the StockTicker sample

[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installa l'applicazione StockTicker. Il pacchetto NuGet include più funzionalità rispetto alla versione semplificata creata da zero. In questa sezione dell'esercitazione si installa il pacchetto NuGet e si esaminano le nuove funzionalità e il codice che le implementa.

> [!IMPORTANT]
> Se si installa il pacchetto senza eseguire i passaggi precedenti di questa esercitazione, è necessario aggiungere una classe di avvio OWIN al progetto. Questo file readme.txt per il pacchetto NuGet illustra questo passaggio.

### <a name="install-the-signalrsample-nuget-package"></a>Installare il pacchetto SignalR.Sample NuGet

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e **scegliere Gestisci pacchetti NuGet**.

1. In **Gestione pacchetti NuGet: SignalR.StockTicker**, selezionare **Sfoglia**.

1. In **Origine pacchetto**selezionare **nuget.org**.

1. Immettere *SignalR.Sample* nella casella di ricerca e selezionare **Microsoft.AspNet.SignalR.Sample** > **Install**.

1. In **Esplora soluzioni**espandere la cartella *SignalR.Sample.*

    L'installazione del pacchetto SignalR.Sample ha creato la cartella e il relativo contenuto.

1. Nella cartella *SignalR.Sample* fare clic con il pulsante destro del mouse su *StockTicker.html*, quindi **scegliere Imposta come pagina iniziale**.

    > [!NOTE]
    > L'installazione del pacchetto SignalR.Sample NuGet potrebbe modificare la versione di jQuery presente nella cartella *Scripts.* Il nuovo file *StockTicker.html* che il pacchetto installa nella cartella *SignalR.Sample* sarà sincronizzato con la versione jQuery installata dal pacchetto, ma se si desidera eseguire nuovamente il file *StockTicker.html* originale, potrebbe essere necessario aggiornare prima il riferimento jQuery nel tag di script.

### <a name="run-the-application"></a>Eseguire l'applicazione

 La tabella che hai visto nella prima app aveva caratteristiche utili. L'applicazione ticker stock completa mostra nuove funzionalità: una finestra a scorrimento orizzontale che mostra i dati azionari e le scorte che cambiano colore mentre salgono e diminuiscono.

1. Premere **F5** per eseguire lo script.

     Quando esegui l'app per la prima volta, il "mercato" è "chiuso" e viene visualizzata una tabella statica e una finestra di ticker che non scorre.

1. Selezionare **Open Market**.

    ![Screenshot del live ticker.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * La casella **Live Stock Ticker** inizia a scorrere orizzontalmente e il server inizia a trasmettere periodicamente le variazioni dei prezzi delle azioni in modo casuale.

    * Ogni volta che il prezzo di un'azione cambia, l'app aggiorna sia la **Live Stock Table** che la Live Stock **Ticker**.

    * Quando la variazione di prezzo di un'azione è positiva, l'app mostra il titolo con uno sfondo verde.

    * Quando la modifica è negativa, l'app mostra il supporto con uno sfondo rosso.

1. Selezionare **Chiudi mercato**.

    * Gli aggiornamenti della tabella vengono interrotti.

    * Il ticker smette di scorrere.

1. Selezionare **Reimposta**.

    * Tutti i dati delle scorte vengono reimpostati.

    * L'app ripristina lo stato iniziale prima dell'avvio delle modifiche dei prezzi.

1. Copiare l'URL dal browser, aprire altri due browser e incollare gli URL nelle barre degli indirizzi.

1. Gli stessi dati vengono aggiornati dinamicamente contemporaneamente in ogni browser.

1. Quando si seleziona uno dei controlli, tutti i browser rispondono contemporaneamente.

### <a name="live-stock-ticker-display"></a>Visualizzazione Live Stock Ticker

La visualizzazione **Di viole** dei colori `<div>` reale è un elenco non ordinato in un elemento formattato in un'unica riga dagli stili CSS. L'app inizializza e aggiorna il ticker nello stesso modo della `<li>` tabella: sostituendo i `<li>` segnaposto `<ul>` in una stringa modello e aggiungendo dinamicamente gli elementi all'elemento. L'app include lo scorrimento `animate` utilizzando la funzione jQuery per variare `<div>`il margine a sinistra dell'elenco non ordinato all'interno di .

#### <a name="signalrsample-stocktickerhtml"></a>SignalR.Sample StockTicker.html

Il codice HTML stock ticker:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a>SignalR.Sample StockTicker.css

Il codice CSS stock ticker:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a>SignalR.Sample SignalR.StockTicker.js

Il codice jQuery che lo fa scorrere:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Metodi aggiuntivi sul server che il client può chiamare

Per aggiungere flessibilità all'app, esistono metodi aggiuntivi che l'app può chiamare.

#### <a name="signalrsample-stocktickerhubcs"></a>SignalR.Sample StockTickerHub.cs

La `StockTickerHub` classe definisce quattro metodi aggiuntivi che il client può chiamare:The class defines four additional methods that the client can call:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

L'app `OpenMarket` `CloseMarket`chiama `Reset` , e in risposta ai pulsanti nella parte superiore della pagina. Illustrano il modello di un client che attiva una modifica dello stato immediatamente propagato a tutti i client. Ognuno di questi metodi `StockTicker` chiama un metodo nella classe che causa il cambiamento di stato del mercato e quindi trasmette il nuovo stato.

#### <a name="signalrsample-stocktickercs"></a>SignalR.Sample StockTicker.cs

Nella `StockTicker` classe, l'app mantiene lo stato `MarketState` del mercato `MarketState` con una proprietà che restituisce un valore di enumerazione:In the class, the app maintains the state of the market with a property that returns a enum value:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Ognuno dei metodi che modificano lo stato del `StockTicker` mercato lo fanno all'interno di un blocco di blocco perché la classe deve essere thread-safe:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Per assicurarsi che questo codice `_marketState` sia thread-safe, il campo che supporta la `MarketState` proprietà designata `volatile`:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

I `BroadcastMarketStateChange` `BroadcastMarketReset` metodi e sono simili al metodo BroadcastStockPrice già visto, ad eccezione del fatto che chiamano diversi metodi definiti nel client:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Funzioni aggiuntive sul client che il server può chiamare

La `updateStockPrice` funzione ora gestisce sia il tavolo che `jQuery.Color` il display del ticker e viene utilizzato per far lampeggiare i colori rosso e verde.

Nuove funzioni in *SignalR.StockTicker.js* abilitano e disabilitano i pulsanti in base allo stato del mercato. Inoltre, arrestano o avviano lo scorrimento orizzontale **di Live Stock Ticker.** Poiché molte funzioni `ticker.client`vengono aggiunte a , l'app usa la [funzione di estensione jQuery](http://api.jquery.com/jQuery.extend/) per aggiungerle.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Configurazione client aggiuntiva dopo aver stabilito la connessione

Dopo che il client stabilisce la connessione, ha alcune operazioni aggiuntive da eseguire:

* Scopri se il mercato è aperto o `marketOpened` `marketClosed` chiuso per chiamare la funzione o l'appropriata.

* Collegare le chiamate al metodo server ai pulsanti.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

I metodi del server non sono collegati ai pulsanti fino a quando l'app stabilisce la connessione. È in modo che il codice non può chiamare i metodi del server prima che siano disponibili.

## <a name="additional-resources"></a>Risorse aggiuntive

In questa esercitazione si è appreso come programmare un'applicazione SignalR che trasmette i messaggi dal server a tutti i client connessi. Ora è possibile trasmettere messaggi su base periodica e in risposta alle notifiche da qualsiasi client. È possibile utilizzare il concetto di istanza singleton multithread per mantenere lo stato del server in scenari di gioco online multigiocatore. Per un esempio, vedere [il gioco ShootR basato su SignalR](https://github.com/NTaylorMullen/ShootR).

Per esercitazioni che illustrano scenari di comunicazione peer-to-peer, vedere [Introduzione a SignalR](introduction-to-signalr.md) e [Aggiornamento in tempo reale con SignalR](tutorial-high-frequency-realtime-with-signalr.md).

Per altre informazioni su SignalR, vedere le risorse seguenti:For more about SignalR, see the following resources:

* [ASP.NET SignalR](../../index.md)
* [Progetto SignalR](http://signalr.net/)
* [SignalR GitHub and Samples](https://github.com/SignalR/SignalR)
* [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione:

> [!div class="checklist"]
> * Creazione del progetto
> * Configurare il codice del server
> * Esaminato il codice server
> * Impostare il codice client
> * Esaminato il codice client
> * Testare l'applicazione
> * Registrazione abilitata

Passare all'articolo successivo per informazioni su come creare un'applicazione Web in tempo reale che utilizza ASP.NET SignalR 2.
> [!div class="nextstepaction"]
> [Crea un'app Web in tempo reale con SignalR](real-time-web-applications-with-signalr.md)
