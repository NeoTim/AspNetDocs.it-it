---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Monitoraggio e telemetria (creazione di app cloud reali con Azure) Documenti Microsoft
author: MikeWasson
description: L'e-book Building Real World Cloud Apps with Azure si basa su una presentazione sviluppata da Scott Guthrie. Spiega 13 schemi e pratiche che possono...
ms.author: riande
ms.date: 07/09/2015
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: f61810ea7b486b2fa0bbb234edea7541eedde835
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676034"
---
# <a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Monitoraggio e telemetria (creazione di app cloud reali con Azure)Monitoring and Telemetry (Building Real-World Cloud Apps with Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Scarica Fix It Project](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica E-book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> L'e-book **Building Real World Cloud Apps with Azure** si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare con successo app Web per il cloud. Per informazioni sull'e-book, vedere [il primo capitolo](introduction.md).

Molte persone si affidano ai clienti per far loro sapere quando la loro applicazione è inattivo. Questa non è davvero una best practice da nessuna parte, e soprattutto non nel cloud. Non c'è garanzia di una notifica rapida e quando ricevi una notifica, spesso ottieni dati minimi o fuorvianti su ciò che è successo. Con buoni sistemi di telemetria e registrazione è possibile essere consapevoli di ciò che sta succedendo con l'app e quando qualcosa va storto si scopre subito e avere informazioni utili per la risoluzione dei problemi con cui lavorare.

## <a name="buy-or-rent-a-telemetry-solution"></a>Acquistare o noleggiare una soluzione di telemetriaBuy or rent a telemetry solution

> [!NOTE]
> Questo articolo è stato scritto prima del rilascio di [Application Insights.This](/azure/application-insights/app-insights-overview) article was written before Application Insights was released. Application Insights is the preferred approach for telemetry solutions on Azure. Per altre informazioni, vedere [Configurare Application Insights per il sito Web ASP.NET.](/azure/application-insights/app-insights-asp-net)

Una delle cose che è grande circa l'ambiente cloud è che è davvero facile da acquistare o affittare la strada per la vittoria. La telemetria è un esempio. Senza un sacco di sforzo è possibile ottenere un sistema di telemetria davvero buono e funzionante, molto conveniente. Ci sono un sacco di grandi partner che si integrano con Azure, e alcuni di loro hanno livelli gratuiti - in modo da poter ottenere telemetria di base per niente. Ecco alcuni di quelli attualmente disponibili in Azure:Here are just a few of the currently available on Azure:

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

[Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) include anche funzionalità di monitoraggio.

Esamineremo rapidamente la configurazione di New Relic per mostrare quanto sia facile usare un sistema di telemetria.

Nel portale di gestione di Azure iscriversi al servizio. Fare clic su **Nuovo**e quindi su **Store**. Viene visualizzata la finestra **di dialogo Scegli componente aggiuntivo.** Scorrere verso il basso e fare clic su **Nuova reliquia**.

![Scegliere un componente aggiuntivo](monitoring-and-telemetry/_static/image1.png)

Fare clic sulla freccia destra e scegliere il livello di servizio desiderato. Per questa demo useremo il livello gratuito.

![Personalizzare il componente aggiuntivo](monitoring-and-telemetry/_static/image2.png)

Fare clic sulla freccia destra, confermare l'"acquisto" e Nuova reliquia viene ora visualizzata come componente aggiuntivo nel portale.

![Rivedi acquisto](monitoring-and-telemetry/_static/image3.png)

![Nuovo componente aggiuntivo Relic nel portale di gestione](monitoring-and-telemetry/_static/image4.png)

Fare clic su **Informazioni di connessione**e copiare il codice di licenza.

![Informazioni di connessione](monitoring-and-telemetry/_static/image5.png)

Passare alla scheda **Configura** per l'app Web nel portale, impostare **Monitoraggio prestazioni** su **Componente aggiuntivo**e **impostare** l'elenco a discesa Scegli componente aggiuntivo su **Nuova reliquia**. Quindi fare clic su **Salva**.

![Nuova reliquia nella scheda Configura](monitoring-and-telemetry/_static/image6.png)

In Visual Studio installa il pacchetto New Relic NuGet nell'app.

![Analisi degli sviluppatori nella scheda ConfiguraDeveloper analytics in Configure tab](monitoring-and-telemetry/_static/image7.png)

Distribuire l'app in Azure e iniziare a usarla. Creare alcune attività Fix It per fornire alcune attività per New Relic da monitorare.

Tornare quindi alla pagina **Nuova reliquia** nella scheda **Componenti aggiuntivi** del portale e fare clic su **Gestisci**. Il portale invia l'utente al portale di gestione New Relic, utilizzando l'accesso Single Sign-On per l'autenticazione in modo da non dover immettere nuovamente le credenziali. La pagina Panoramica presenta una varietà di statistiche sulle prestazioni. (Fare clic sull'immagine per visualizzare la pagina di panoramica a dimensioni intere.)

[![Nuova scheda Monitoraggio reliquia](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Ecco alcune delle statistiche che puoi vedere:

- Tempo di risposta medio in diverse ore del giorno.

    ![Tempo di risposta](monitoring-and-telemetry/_static/image10.png)
- Velocità di velocità effettiva (in richieste al minuto) in diverse ore del giorno.

    ![Velocità effettiva](monitoring-and-telemetry/_static/image11.png)
- Tempo di CPU del server impiegato per la gestione di diverse richieste HTTP.

    ![Tempi delle transazioni Web](monitoring-and-telemetry/_static/image12.png)
- Tempo CPU trascorso in diverse parti del codice dell'applicazione:

    ![Dettagli traccia](monitoring-and-telemetry/_static/image13.png)
- Statistiche cronologiche sulle prestazioni.

    ![Prestazioni storiche](monitoring-and-telemetry/_static/image14.png)
- Chiamate a servizi esterni, ad esempio il servizio BLOB e statistiche su quanto sia stato affidabile e reattivo il servizio.

    ![Servizi esterni](monitoring-and-telemetry/_static/image15.png)

    ![Servizi esterni](monitoring-and-telemetry/_static/image16.png)

    ![Servizio esterno](monitoring-and-telemetry/_static/image17.png)
- Informazioni su dove nel mondo o da dove proviene il traffico delle app web degli Stati Uniti.

    ![Area geografica](monitoring-and-telemetry/_static/image18.png)

È inoltre possibile impostare report ed eventi. Ad esempio, è possibile pronunciare ogni volta che si iniziano a visualizzare errori, inviare un messaggio di posta elettronica per avvisare il personale di supporto al problema.

![Report](monitoring-and-telemetry/_static/image19.png)

New Relic è solo un esempio di un sistema di telemetria; è possibile ottenere tutto questo da altri servizi pure. La bellezza del cloud è che senza dover scrivere alcun codice, e per spese minime o non, improvvisamente si possono ottenere molte più informazioni su come viene utilizzata l'applicazione e ciò che i clienti stanno effettivamente vivendo.

<a id="log"></a>
## <a name="log-for-insight"></a>Log per approfondimenti

Un pacchetto di telemetria è un buon primo passo, ma è comunque necessario instrumentare il proprio codice. Il servizio di telemetria indica quando si verifica un problema e indica cosa si verificano i clienti, ma potrebbe non fornire molte informazioni dettagliate su ciò che sta succedendo nel codice.

Non è necessario eseguire remoto in un server di produzione per vedere cosa sta facendo l'app. Questo potrebbe essere pratico quando hai un server, ma che dire quando hai scalato a centinaia di server e non sai quali è necessario remoto in? La registrazione deve fornire informazioni sufficienti per non essere mai necessario in remoto nei server di produzione per analizzare ed eseguire il debug dei problemi. È necessario registrare informazioni sufficienti in modo da poter isolare i problemi esclusivamente tramite i registri.

### <a name="log-in-production"></a>Log in produzione

Molte persone attivano la traccia nell'ambiente di produzione solo quando si verifica un problema e desiderano eseguire il debug. Questo può introdurre un notevole ritardo tra il momento in cui si è a conoscenza di un problema e il momento in cui si ottengono utili informazioni per la risoluzione dei problemi su di esso. E le informazioni che si ottiene potrebbero non essere utili per gli errori intermittenti.

Ciò che è consigliabile nell'ambiente cloud in cui lo spazio di archiviazione è economico è che si lascia sempre l'accesso nell'ambiente di produzione. In questo modo quando si verificano errori, sono già stati registrati e si dispone di dati cronologici che consentono di analizzare i problemi che si sviluppano nel tempo o che si verificano regolarmente in momenti diversi. È possibile automatizzare un processo di eliminazione per eliminare i vecchi log, ma è possibile che sia più costoso configurare un processo di questo tipo piuttosto che mantenere i log.

La spesa aggiuntiva di registrazione è banale rispetto alla quantità di tempo e denaro per la risoluzione dei problemi che è possibile risparmiare avendo tutte le informazioni necessarie già disponibili quando qualcosa va storto. Poi, quando qualcuno ti dice che hanno avuto un errore casuale intorno alle 8:00 di ieri sera, ma non ricordano l'errore, puoi facilmente scoprire qual era il problema.

Per meno di 4 dollari al mese è possibile mantenere 50 gigabyte di log a portata di mano e l'impatto sulle prestazioni della registrazione è banale finché si tiene presente una cosa : per evitare colli di bottiglia delle prestazioni, assicurarsi che la libreria di registrazione sia asincrona.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Differenzia i log che informano dai log che richiedono un'azioneDifferentiate logs that inform from logs that require action

I registri hanno lo scopo di INFORM (voglio che tu sappia qualcosa) o ACT (voglio che tu faccia qualcosa). Prestare attenzione a scrivere solo i registri ACT per i problemi che richiedono realmente l'intervento di una persona o di un processo automatizzato. Troppi registri ACT creeranno rumore, richiedendo troppo lavoro per setacciare tutto per trovare problemi genuini. E se i log ACT attivano automaticamente alcune azioni, ad esempio l'invio di posta elettronica al personale di supporto, evitare di consentire che migliaia di tali azioni vengano attivate da un singolo problema.

Nell'analisi System.Diagnostics di .NET ai log possono essere assegnati livelli di errore, avviso, informazioni e debug/verbose. È possibile differenziare ACT dai registri INFORM riservando il livello di errore per i registri ACT e utilizzando i livelli inferiori per i registri INFORM.

![Livelli di registrazione](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Configurare i livelli di registrazione in fase di esecuzioneConfigure logging levels at run time

Anche se vale la pena di avere la registrazione sempre attiva nell'ambiente di produzione, un'altra procedura consigliata consiste nell'implementare un framework di registrazione che consente di regolare in fase di esecuzione il livello di dettaglio che si sta registrando, senza ridistribuire o riavviare l'applicazione. Ad esempio, quando si `System.Diagnostics` utilizza la funzionalità di analisi in è possibile creare registri di errore, avviso, informazioni e debug/verbose. È consigliabile registrare sempre i registri errori, avvisi e informazioni nell'ambiente di produzione e si desidera poter aggiungere dinamicamente la registrazione Debug/Verbose per la risoluzione dei problemi caso per caso.

Le app Web nel servizio app di `System.Diagnostics` Azure dispongono di supporto incorporato per la scrittura di log nel file system, nell'archiviazione tabelle o nell'archiviazione BLOB. È possibile selezionare livelli di registrazione diversi per ogni destinazione di archiviazione ed è possibile modificare il livello di registrazione in tempo reale senza riavviare l'applicazione. Il supporto dell'archiviazione BLOB semplifica l'esecuzione dei processi di analisi [HDInsight](https://docs.microsoft.com/azure/hdinsight/) nei log dell'applicazione, perché HDInsight sa come usare direttamente l'archiviazione BLOB.

### <a name="log-exceptions"></a>Registrare eccezioni

Non fare *solo eccezioni. ToString()* nel codice di registrazione. Questo esclude informazioni contestuali. Nel caso di errori SQL, lascia il numero di errore SQL. Per tutte le eccezioni, includere le informazioni sul contesto, l'eccezione stessa e le eccezioni interne per assicurarsi di fornire tutto ciò che sarà necessario per la risoluzione dei problemi. Ad esempio, le informazioni sul contesto possono includere il nome del server, un identificatore di transazione e un nome utente (ma non la password o i segreti!).

Se ti affidi a ogni sviluppatore per fare la cosa giusta con la registrazione delle eccezioni, alcuni di loro non lo faranno. Per garantire che venga eseguito nel modo corretto ogni volta, compilare la gestione delle eccezioni direttamente nell'interfaccia del logger: passare l'oggetto eccezione stesso alla classe logger e registrare correttamente i dati dell'eccezione nella classe logger.

### <a name="log-calls-to-services"></a>Registrare le chiamate ai servizi

È consigliabile scrivere un log ogni volta che l'app chiama un servizio, sia che si tratti di un database, di un'API REST o di qualsiasi servizio esterno. Includere nei log non solo un'indicazione di esito positivo o negativo, ma anche il tempo impiegato da ogni richiesta. Nell'ambiente cloud si verificano spesso problemi correlati a rallentamenti anziché interruzioni complete. Qualcosa che normalmente richiede 10 millisecondi potrebbe improvvisamente iniziare a prendere un secondo. Quando qualcuno indica che l'app è lenta, si vuole essere in grado di esaminare New Relic o qualsiasi servizio di telemetria di cui si dispone e convalidare la propria esperienza, quindi si vuole essere in grado di cercare i propri log per approfondire i dettagli del motivo per cui è lento.

### <a name="use-an-ilogger-interface"></a>Utilizzare un'interfaccia ILogger

È consigliabile eseguire un'operazione quando si crea un'applicazione di produzione consiste nel creare una semplice interfaccia *ILogger* e inserirvi alcuni metodi. In questo modo è più semplice modificare l'implementazione della registrazione in un secondo momento e non è necessario esaminare tutto il codice per eseguire questa operazione. Potremmo usare la `System.Diagnostics.Trace` classe in tutta l'app Fix It, ma la stiamo usando sotto le coperte in una classe di registrazione che implementa *ILogger*e effettuiamo chiamate al metodo *ILogger* in tutta l'app.

In questo modo, se si desidera rendere la [`System.Diagnostics.Trace`](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) registrazione più ricca, è possibile sostituire con qualsiasi meccanismo di registrazione che si desidera. Ad esempio, man mano che l'app cresce, è possibile decidere di usare un pacchetto di registrazione più completo, ad esempio [NLog](http://nlog-project.org/) o [Enterprise Library Logging Application Block](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). [Log4Net](http://logging.apache.org/log4net/) è un altro framework di registrazione comune, ma non esegue la registrazione asincrona.

Un possibile motivo per l'utilizzo di un framework come NLog è quello di facilitare la divisione dell'output di registrazione in archivi dati separati ad alto volume e di valore elevato. Ciò consente di archiviare in modo efficiente grandi volumi di dati INFORM su cui non è necessario eseguire query veloci, mantenendo al contempo un accesso rapido ai dati ACT.

### <a name="semantic-logging"></a>Registrazione semantica

Per un modo relativamente nuovo per eseguire la registrazione in grado di produrre informazioni diagnostiche più utili, vedere [Enterprise Library Semantic Logging Application Block (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). SLAB utilizza il supporto [Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) e [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) in .NET 4.5 per consentire la creazione di log più strutturati e queryable. Si definisce un metodo diverso per ogni tipo di evento che si registra, che consente di personalizzare le informazioni scritte. Ad esempio, per registrare un errore `LogSQLDatabaseError` del database SQL è possibile chiamare un metodo. Per questo tipo di eccezione, si sa che un'informazione chiave è il numero di errore, pertanto è possibile includere un parametro del numero di errore nella firma del metodo e registrare il numero di errore come campo separato nel record di log scritto. Poiché il numero si trova in un campo separato, è possibile ottenere report più facilmente e in modo affidabile in base ai numeri di errore SQL rispetto a quando si desidera concatenare il numero di errore in una stringa di messaggio.

## <a name="logging-in-the-fix-it-app"></a>Accesso all'app Fix It

### <a name="the-ilogger-interface"></a>L'interfaccia ILogger

Ecco l'interfaccia *ILogger* nell'app Fix It.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Questi metodi consentono di scrivere log agli stessi quattro livelli supportati da *System.Diagnostics*. I metodi TraceApi consentono di registrare le chiamate al servizio esterno con informazioni sulla latenza. È anche possibile aggiungere un set di metodi per il livello Debug/Verbose.You could also add a set of methods for Debug/Verbose level.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>Implementazione del logger dell'interfaccia ILogger

L'implementazione dell'interfaccia è davvero semplice. Fondamentalmente si chiama solo i metodi *System.Diagnostics* standard. Il frammento di codice seguente mostra tutti e tre i metodi di informazioni e uno ciascuno degli altri.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Chiamata ai metodi ILoggerCalling the ILogger methods

Ogni volta che il codice nell'app Fix It rileva un'eccezione, chiama un metodo *ILogger* per registrare i dettagli dell'eccezione. E ogni volta che effettua una chiamata al database, al servizio BLOB o a un'API REST, avvia un cronometro prima della chiamata, interrompe il cronometro quando il servizio viene restituito e registra il tempo trascorso insieme alle informazioni sull'esito positivo o negativo.

Si noti che il messaggio di log include il nome della classe e il nome del metodo. È consigliabile assicurarsi che i messaggi di log identifichino la parte del codice dell'applicazione che li ha scritti.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Così ora per ogni volta che l'applicazione Fix It ha effettuato una chiamata al Database SQL, è possibile vedere la chiamata, il metodo che lo ha chiamato, e esattamente quanto tempo ci è voluto.

![Query del database SQL nei log](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Se si passa sfogliando i registri, si può vedere che il tempo di chiamate al database è variabile. Queste informazioni potrebbero essere utili: poiché l'app registra tutto questo è possibile analizzare le tendenze cronologiche nel funzionamento del servizio di database nel tempo. Ad esempio, un servizio potrebbe essere veloce la maggior parte del tempo, ma le richieste potrebbero non riuscire o le risposte potrebbero rallentare in determinate ore del giorno.

È possibile eseguire la stessa operazione per il servizio BLOB: ogni volta che l'app carica un nuovo file, è presente un log e si può vedere esattamente il tempo necessario per caricare ogni file.

![Log di caricamento BLOB](monitoring-and-telemetry/_static/image23.png)

E 'solo un paio di righe extra di codice per scrivere ogni volta che si chiama un servizio, e ora ogni volta che qualcuno dice che hanno incontrato un problema, si sa esattamente che cosa il problema era, se si trattava di un errore, o anche se era solo in esecuzione lento. È possibile individuare l'origine del problema senza dover remoto in un server o attivare la registrazione dopo che si verifica l'errore e sperare di ricrearlo.

## <a name="dependency-injection-in-the-fix-it-app"></a>Inserimento delle dipendenze nell'app Fix It

Ci si potrebbe chiedere come il costruttore del repository nell'esempio illustrato in precedenza ottiene l'implementazione dell'interfaccia logger:You might wonder how the repository constructor in the example shown above gets the logger interface implementation:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Per collegare l'interfaccia all'implementazione, l'app usa [l'inserimento](http://en.wikipedia.org/wiki/Dependency_injection)delle dipendenze (DI) con [AutoFac](http://autofac.org/). DI consente di utilizzare un oggetto basato su un'interfaccia in molte posizioni in tutto il codice e deve specificare solo in un'unica posizione l'implementazione che viene utilizzata quando viene creata un'istanza dell'interfaccia. In questo modo è più semplice modificare l'implementazione: ad esempio, è possibile sostituire il logger System.Diagnostics con un logger NLog. Oppure per i test automatizzati si potrebbe desiderare di sostituire una versione fittizia del logger.

L'applicazione Fix It utilizza DI in tutti i repository e in tutti i controller. I costruttori delle classi controller ottengono un'interfaccia *ITaskRepository* nello stesso modo in cui il repository ottiene un'interfaccia logger:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

L'app usa la libreria AutoFac DI per fornire automaticamente le istanze *TaskRepository* e *Logger* per questi costruttori.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Questo codice dice fondamentalmente che ovunque un costruttore ha bisogno *di un'interfaccia ILogger,* passare un'istanza della classe *Logger* e ogni volta che è necessaria un'interfaccia *IFixItTaskRepository,* passare un'istanza della classe *FixItTaskRepository.*

[AutoFac](http://autofac.org/) è uno dei molti framework di inserimento delle dipendenze che è possibile utilizzare. Un altro popolare è [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), che è consigliato e supportato da Microsoft Patterns and Practices.

## <a name="built-in-logging-support-in-azure"></a>Supporto per la registrazione integrata in AzureBuilt-in logging support in Azure

Azure supporta i tipi di registrazione seguenti per le app Web nel servizio app di [Azure:](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)

- System.Diagnostics tracing (è possibile attivare e disattivare i livelli in tempo reale senza riavviare il sito).
- Eventi di Windows.
- Registri IIS (HTTP/FREB).

Azure supporta i seguenti tipi di [registrazione nei servizi cloud:](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics)

- Analisi di System.Diagnostics.
- Contatori delle prestazioni.
- Eventi di Windows.
- Registri IIS (HTTP/FREB).
- Monitoraggio personalizzato della directory.

L'app Fix It utilizza l'analisi System.Diagnostics.The Fix It app uses System.Diagnostics tracing. Tutto quello che devi fare per abilitare la registrazione di System.Diagnostics in un'app Web è capovolgere un interruttore nel portale o chiamare l'API REST. Nel portale fare clic sulla scheda **Configurazione** per il sito e scorrere verso il basso per visualizzare la sezione **Diagnostica applicazione.** È possibile attivare o disattivare la registrazione e selezionare il livello di registrazione desiderato. È possibile fare in modo che Azure scriva i log nel file system o in un account di archiviazione.

![Diagnostica delle app e diagnostica del sito nella scheda Configura](monitoring-and-telemetry/_static/image24.png)

Dopo aver abilitato la registrazione in Azure, è possibile visualizzare i log nella finestra Output di Visual Studio man mano che vengono creati.

![Menu Log di streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menu Log di streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

È anche possibile scrivere log nell'account di archiviazione e visualizzarli con qualsiasi strumento che può accedere al servizio Tabella di archiviazione di Azure, ad esempio **Esplora server** in Visual Studio o Azure [Storage Explorer.](https://azure.microsoft.com/features/storage-explorer/)

![Registri in Esplora server](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Riepilogo

È molto semplice implementare un sistema di telemetria out-of-the-box, instrumentare la registrazione nel proprio codice e configurare la registrazione in Azure.It's really simple to implement an out-of-the-box telemetry system, instrument logging in your own code, and configure logging in Azure. E quando si verificano problemi di produzione, la combinazione di un sistema di telemetria e log personalizzati consente di risolvere rapidamente i problemi prima che diventino problemi principali per i clienti.

Nel [prossimo capitolo](transient-fault-handling.md) verrà illustrato come gestire gli errori temporanei in modo che non diventino problemi di produzione che è necessario analizzare.

## <a name="resources"></a>Risorse

Per ulteriori informazioni, vedere le risorse seguenti.

Documentazione principalmente sulla telemetria:

- [Modelli e procedure Microsoft - Indicazioni di Azure](https://msdn.microsoft.com/library/dn568099.aspx).Microsoft Patterns and Practices - Azure Guidance . Vedere Linee guida per la strumentazione e la telemetria, le linee guida per il misurazione del servizio, il modello di monitoraggio degli endpoint di integrità e il modello di riconfigurazione del runtime.
- [Penny Pinching nel cloud: abilitazione del monitoraggio delle prestazioni di nuove reliquie nei siti Web](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx)di Azure .
- [Procedure consigliate per la progettazione di servizi su larga scala in Servizi cloud](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)di Azure. White paper di Mark Simms e Michael Thomassy. Vedere la sezione Telemetria e diagnostica.
- [Sviluppo di nuova generazione con Application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). Articolo di MSDN Magazine.

Documentazione principalmente sulla registrazione:

- [Blocco applicazione registrazione semantica (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie presenta la proprietà per la registrazione semantica con SLAB.
- [Creazione di registri strutturati e significativi con registrazione semantica](https://channel9.msdn.com/Events/Build/2013/3-336). (Video) Julian Dominguez presenta la proprietà per la registrazione semantica con SLAB.
- [Registrazione SQL EF6 - Parte 1: Registrazione semplice](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers viene illustrato come registrare le query eseguite da Entity Framework in Entity Framework 6.
- Resilienza della connessione e intercettazione dei comandi [con Entity Framework in un'applicazione MVC ASP.NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Quarto in una serie di esercitazioni in nove parti, viene illustrato come utilizzare la funzionalità di intercettazione dei comandi di Entity Framework 6 per registrare i comandi SQL inviati al database da Entity Framework.
- Migliorare la registrazione utilizzando gli attributi di informazioni del chiamante di [C .NET 5.0](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Come registrare facilmente il nome del metodo chiamante senza codificarsi in valori letterali o utilizzando la reflection per ottenerlo manualmente.

Documentazione principalmente sulla risoluzione dei problemi:

- [Blog &amp; sul debug per la risoluzione dei problemi di Azure](https://blogs.msdn.com/b/kwill/).
- [AzureTools: utilità di diagnostica utilizzata dal team](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true)di supporto per sviluppatori di Azure. Introduce e fornisce un collegamento di download per uno strumento che può essere usato in una macchina virtuale di Azure per scaricare ed eseguire un'ampia gamma di strumenti di diagnostica e monitoraggio. Utile quando è necessario diagnosticare un problema in una determinata macchina virtuale.
- [Risolvere i problemi di un'app Web nel servizio app](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)di Azure usando Visual Studio . Un'esercitazione dettagliata per iniziare a utilizzare l'analisi System.Diagnostics e il debug remoto.

Video:

- [FailSafe: Creazione di servizi cloud scalabili e resilienti](https://channel9.msdn.com/Series/FailSafe). Serie in nove parti di Ulrich Homann, Marc Mercuri e Mark Simms. Presenta concetti di alto livello e principi architetturali in modo molto accessibile e interessante, con storie tratte dall'esperienza del team di consulenza clienti Microsoft (CAT) con i clienti effettivi. Gli episodi 4 e 9 riguardano il monitoraggio e la telemetria. Episodio 9 include una panoramica dei servizi di monitoraggio MetricsHub, AppDynamics, New Relic e PagerDuty.
- [Building Big: Lezioni apprese dai clienti](https://channel9.msdn.com/Events/Build/2012/3-030)di Azure - Parte II . Mark Simms parla di progettazione per il fallimento e la strumentazione di tutto. Simile alla serie Failsafe, ma va in più dettagli procedura.

Esempio di codice:Code sample:

- [Nozioni fondamentali sui](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)servizi cloud in Azure. Applicazione di esempio creata dal team di consulenza clienti di Microsoft Azure.Sample application created by the Microsoft Azure Customer Advisory Team. Vengono illustrate le procedure di telemetria e registrazione, come illustrato negli articoli seguenti. L'esempio implementa la registrazione dell'applicazione utilizzando [NLog](http://nlog-project.org/). Per la documentazione correlata, vedere la [serie di quattro articoli wiki TechNet sulla telemetria e la registrazione](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Successivo](design-to-survive-failures.md)
> [precedente](transient-fault-handling.md)
