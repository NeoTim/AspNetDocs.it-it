---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: "Appendice: L'applicazione di esempio Fix It (Creazione di app cloud reali con Azure) Documenti Microsoft"
author: MikeWasson
description: L'e-book Building Real World Cloud Apps with Azure si basa su una presentazione sviluppata da Scott Guthrie. Spiega 13 schemi e pratiche che possono...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 896196bdb6a6b0d12a6c798ead510e37dd38a9fc
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675992"
---
# <a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Appendice: L'applicazione di esempio Fix It (Creazione di app cloud reali con Azure)Appendix: The Fix It Sample Application (Building Real-World Cloud Apps with Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Scarica the Fix It Project](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> L'e-book **Building Real World Cloud Apps with Azure** si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare con successo app Web per il cloud. Per informazioni sull'e-book, vedere [il primo capitolo](introduction.md).

Questa appendice all'e-book Building Real World Cloud Apps with Azure contiene le sezioni seguenti che forniscono informazioni aggiuntive sull'applicazione di esempio Fix It che è possibile scaricare:

- [Problemi noti](#knownissues)
- [Procedure consigliate](#bestpractices)
- [Come eseguire l'app da Visual Studio nel computer localeHow to run the app from Visual Studio on your local computer](#run-in-vs)
- [Come distribuire l'app di base in App Web del servizio app di Azure usando gli script di Windows PowerShell](#deploybase)
- [Risoluzione dei problemi relativi agli script di Windows PowerShellTroubleshooting the Windows PowerShell scripts](#troubleshooting)
- [Come distribuire l'app con l'elaborazione della coda alle app Web del servizio app di Azure e a un servizio cloud di AzureHow to deploy the app with queue processing to Azure App Web Apps and an Azure Cloud Service](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Problemi noti

L'applicazione Fix It è stato originariamente sviluppato al fine di illustrare nel modo più semplice possibile alcuni dei modelli presentati in questo e-book. Tuttavia, dal momento che l'e-book riguarda la creazione di applicazioni reali, abbiamo sottoposto il codice Fix It a un processo di revisione e test simile a quello che faremmo per il software rilasciato. Abbiamo trovato una serie di problemi, e come con qualsiasi applicazione reale, alcuni di loro abbiamo risolto e alcuni di loro abbiamo rinviato a una versione successiva.

L'elenco seguente include i problemi che devono essere risolti in un'applicazione di produzione, ma per un motivo o per l'altro abbiamo deciso di non risolvere nella versione iniziale dell'applicazione di esempio Fix It.

### <a name="security"></a>Sicurezza

- Assicurarsi di non poter assegnare un'attività a un proprietario inesistente.
- Assicurarsi di poter visualizzare e modificare solo le attività create o assegnate all'utente.
- Utilizzare HTTPS per le pagine di accesso e i cookie di autenticazione.
- Specificare un limite di tempo per i cookie di autenticazione.

### <a name="input-validation"></a>Convalida dell'input

In generale, un'app di produzione eseguirebbe più convalida dell'input rispetto all'app Fix It. Ad esempio, la dimensione del file immagine / immagine consentita per il caricamento deve essere limitata.

### <a name="administrator-functionality"></a>Funzionalità dell'amministratore

Un amministratore deve essere in grado di modificare la proprietà delle attività esistenti. Ad esempio, il creatore di un'attività potrebbe lasciare l'azienda, senza che nessuno abbia l'autorità per mantenere l'attività a meno che non sia abilitato l'accesso amministrativo.

### <a name="queue-message-processing"></a>Elaborazione dei messaggi della coda

L'elaborazione dei messaggi in coda nell'app Fix It è stata progettata per essere semplice per illustrare il modello di lavoro incentrato sulla coda con una quantità minima di codice. Questo codice semplice non sarebbe adeguato per un'applicazione di produzione effettiva.

- Il codice non garantisce che ogni messaggio della coda verrà elaborato al massimo una volta. Quando si ottiene un messaggio dalla coda, si verifica un periodo di timeout, durante il quale il messaggio è invisibile ad altri listener della coda. Se il timeout scade prima che il messaggio venga eliminato, il messaggio diventa nuovamente visibile. Pertanto, se un'istanza del ruolo di lavoro impiega molto tempo nell'elaborazione di un messaggio, è teoricamente possibile che lo stesso messaggio venga elaborato due volte, generando un'attività duplicata nel database. Per altre informazioni su questo problema, vedere Uso delle code di Archiviazione di Azure .For more information about this issue, see [Using Azure Storage Queues](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- La logica di polling della coda potrebbe essere più conveniente, eseguendo in batch il recupero dei messaggi. Ogni volta che chiami [CloudQueue.GetMessageAsync,](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx)c'è un costo di transazione. Al contrario, è possibile chiamare [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (notare il plurale 's'), che ottiene più messaggi in una singola transazione. I costi di transazione per le code di archiviazione di Azure sono molto bassi, pertanto l'impatto sui costi non è sostanziale nella maggior parte degli scenari.
- Il ciclo stretto nel codice di elaborazione dei messaggi della coda causa l'affinità della CPU, che non utilizza macchine virtuali multi-core in modo efficiente. Una progettazione migliore userebbe il parallelismo delle attività per eseguire diverse attività asincrone in parallelo.
- L'elaborazione dei messaggi in coda ha solo una gestione rudimentale delle eccezioni. Ad esempio, il codice non gestisce i [messaggi non elaborabili.](https://msdn.microsoft.com/library/ms789028.aspx) Quando l'elaborazione dei messaggi causa un'eccezione, è necessario registrare l'errore ed eliminare il messaggio, altrimenti il ruolo di lavoro tenterà di elaborarlo nuovamente e il ciclo continuerà all'infinito.

### <a name="sql-queries-are-unbounded"></a>Le query SQL sono senza limiti

Il codice Fix It corrente non pone alcun limite al numero di righe che le query per le pagine di indice potrebbero restituire. Se nel database viene immesso un volume elevato di attività, le dimensioni degli elenchi ricevuti potrebbero causare problemi di prestazioni. La soluzione consiste nell'implementare il paging. Per un esempio, vedere [Ordinamento, filtro e paging con Entity Framework in un'applicazione MVC ASP.NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Visualizza i modelli consigliati

L'app Fix It usa la classe di entità FixItTask per passare informazioni tra il controller e la visualizzazione. È consigliabile utilizzare i modelli di visualizzazione. Il modello di dominio (ad esempio, la classe di entità FixItTask) è progettato in base a ciò che è necessario per la persistenza dei dati, mentre un modello di visualizzazione può essere progettato per la presentazione dei dati. Per ulteriori informazioni, vedere [12 ASP.NET MVC Best Practices](https://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Blob di immagini sicuro consigliatoSecure image blob recommended

L'app Fix It archivia le immagini caricate come pubbliche, il che significa che chiunque trovi l'URL può accedere alle immagini. Le immagini potrebbero essere protette invece che pubbliche.

### <a name="no-powershell-automation-scripts-for-queues"></a>Nessun script di automazione di PowerShell per le code

Gli script di automazione di PowerShell di esempio sono stati scritti solo per la versione di base di Fix It che viene eseguita interamente nelle app Web del servizio app di Azure.Sample PowerShell automation scripts were written only for the base version of Fix It that runs entirely in Azure App Service Web Apps. Non sono stati forniti script per l'impostazione e la distribuzione nell'app Web, oltre all'ambiente del servizio cloud necessario per l'elaborazione delle code.

### <a name="special-handling-for-html-codes-in-user-input"></a>Gestione speciale per i codici HTML nell'input dell'utente

ASP.NET impedisce automaticamente molti modi in cui gli utenti malintenzionati potrebbero tentare attacchi di cross-site scripting immettendo script nelle caselle di testo di input dell'utente. E l'helper MVC `DisplayFor` utilizzato per visualizzare i titoli delle attività e le note codifica automaticamente automaticamente i valori che invia al browser. Ma in un'app di produzione potresti voler adottare misure aggiuntive. Per ulteriori informazioni, vedere Convalida delle richieste [in ASP.NET](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Procedure consigliate

Di seguito sono riportati alcuni problemi risolti dopo essere stati individuati nella revisione del codice e nel test della versione originale dell'app Fix It. Alcuni sono stati causati dal programmatore originale non essendo a conoscenza di una particolare best practice, alcuni semplicemente perché il codice è stato scritto rapidamente e non era destinato per il software rilasciato. Stiamo elencando i problemi qui nel caso in cui ci sia qualcosa che abbiamo imparato da questa recensione e test che potrebbe essere utile per gli altri che stanno anche sviluppando applicazioni web.

### <a name="dispose-the-database-repository"></a>Eliminare l'archivio del database

La `FixItTaskRepository` classe deve eliminare `DbContext` l'istanza di Entity Framework. Abbiamo fatto questo `IDisposable` implementando nella `FixItTaskRepository` classe:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Si noti che AutoFac rimuoverà automaticamente l'istanza, `FixItTaskRepository` pertanto non è necessario eliminarla in modo esplicito.

Un'altra opzione `DbContext` consiste nel `FixItTaskRepository`rimuovere la variabile `DbContext` membro da , e `using` creare invece una variabile locale all'interno di ogni metodo del repository, all'interno di un'istruzione . Ad esempio:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Registrare i singleton come tali con DI

Poiché è necessaria `PhotoService` una `Logger` sola istanza della classe e della classe, queste classi devono essere [registrate come istanze singole per l'inserimento](https://code.google.com/p/autofac/wiki/InstanceScope) delle dipendenze in *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Sicurezza: non mostrare dettagli sull'errore agli utenti

L'app Fix It originale non disponeva di una pagina di errore generica e lasciava che tutte le eccezioni si propagasse all'interfaccia utente, pertanto alcune eccezioni, ad esempio errori di connessione al database, potrebbero causare la visualizzazione di un'analisi dello stack completa nel browser. Informazioni dettagliate sugli errori possono talvolta facilitare gli attacchi da parte di utenti malintenzionati. La soluzione consiste nel registrare i dettagli dell'eccezione e visualizzare una pagina di errore per l'utente che non include i dettagli dell'errore. L'app Fix It era già la registrazione e per `<customErrors mode=On>` visualizzare una pagina di errore, è stato aggiunto nel file Web.config.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Per impostazione predefinita, per gli errori viene visualizzata la visualizzazione di *visualizzazioni, ovvero Shared, Errorhtml.* È possibile personalizzare *Error.cshtml* o creare una `defaultRedirect` visualizzazione della pagina di errore personalizzata e aggiungere un attributo. È inoltre possibile specificare pagine di errore diverse per errori specifici.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Sicurezza: consente solo a un'attività di essere modificata dal suo creatore

La pagina Indice dashboard mostra solo le attività create dall'utente connesso, ma un utente malintenzionato potrebbe creare un URL con un ID per l'attività di un altro utente. Abbiamo aggiunto il codice in *DashboardController.cs* per restituire un 404 in questo caso:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>Non ingoiare eccezioni

L'applicazione Fix It originale appena restituito null dopo la registrazione di un'eccezione che ha causato da una query SQL:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

In questo modo sarebbe guardare all'utente come se la query ha avuto esito positivo, ma semplicemente non ha restituito alcuna riga. La soluzione consiste nel generare nuovamente l'eccezione dopo l'intercettazione e la registrazione:Solution is to re-throw the exception after catching and logging:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Intercettare tutte le eccezioni nei ruoli di lavoro

Eventuali eccezioni non gestite in un ruolo di lavoro causeranno il riciclo della macchina virtuale, pertanto si desidera eseguire il wrapping di tutte le operazioni in un blocco try-catch e gestire tutte le eccezioni.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Specificare la lunghezza per le proprietà stringa nelle classi di entitàSpecify length for string properties in entity classes

Per visualizzare codice semplice, la versione originale dell'app Fix It non ha specificato lunghezze per i campi dell'entità FixItTask e di conseguenza sono state definite come varchar(max) nel database. Di conseguenza, l'interfaccia utente accetterebbe quasi qualsiasi quantità di input. Specificando lunghezze vengono impostati i limiti che si applicano sia all'input dell'utente nella pagina Web che alle dimensioni delle colonne nel database:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Contrassegnare i membri privati come readonly quando non è previsto che cambino

Ad esempio, `DashboardController` nella classe `FixItTaskRepository` viene creata un'istanza di che non è prevista la modifica, pertanto è stata definita [readonly](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Lista d'uso. Any() al posto di list. Count() &gt; 0

Se si è interessati solo se uno o più elementi in [Any](https://msdn.microsoft.com/library/bb534972.aspx) un elenco soddisfano i criteri specificati, utilizzare il `Count` Any metodo, perché restituisce non appena viene trovato un elemento che si adatta ai criteri, mentre il metodo deve sempre scorrere ogni elemento. Il file Dashboard *Index.cshtml* originariamente aveva questo codice:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

L'abbiamo cambiato in questo:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Generare URL nelle visualizzazioni MVC usando gli helper MVCGenerate URLs in MVC views using MVC helpers

Per il pulsante **Crea una correzione** nella home page, l'app Fix It è hardcoded un elemento di ancoraggio:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Per i collegamenti visualizzazione/azione come questo è preferibile utilizzare l'helper HTML [Url.Action,](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) ad esempio:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Usare Task.Delay anziché Thread.Sleep nel ruolo di lavoroUse Task.Delay instead of Thread.Sleep in worker role

Il modello di `Thread.Sleep` nuovo progetto inserisce il codice di esempio per un ruolo di lavoro, ma causando la sospensione del thread può causare il pool di thread generare thread non necessari aggiuntivi. È possibile evitare che utilizzando [Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx) invece.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Evitare l'annullamento asincronoAvoid async void

Se un metodo asincrono non deve restituire `Task` un valore, restituire un tipo anziché `void`.

Questo esempio è `FixItQueueManager` tratto dalla classe :This example is from the class:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

È consigliabile utilizzare `async void` solo per i gestori eventi di primo livello. Se si definisce `async void`un metodo come , il chiamante non può **attendere** il metodo o intercettare eventuali eccezioni generate dal metodo. Per ulteriori informazioni, vedere [Procedure consigliate nella programmazione asincrona](https://msdn.microsoft.com/magazine/jj991977.aspx).

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Usare un token di annullamento per interrompere il ciclo del ruolo di lavoroUse a cancellation token to break from worker role loop

In genere, il **run** metodo su un ruolo di lavoro contiene un ciclo infinito. Quando il ruolo di lavoro viene arrestato, viene chiamato il metodo [RoleEntryPoint.OnStop.](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) È necessario utilizzare questo metodo per annullare il lavoro che viene eseguito all'interno di **Run** metodo e uscire normalmente. In caso contrario, il processo potrebbe essere terminato durante un'operazione.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Disattivare la procedura di sniffing MIME automatico

In alcuni casi, Internet Explorer segnala un tipo MIME diverso da quello specificato dal server Web. Ad esempio, se Internet Explorer trova contenuto HTML in un file recapitato con l'intestazione di risposta HTTP Content-Type: text/plain, Internet Explorer determina che il rendering del contenuto deve essere eseguito come HTML. Sfortunatamente, questo "sniffing MIME" può anche causare problemi di sicurezza per i server che ospitano contenuto non attendibile. Per combattere questo problema, Internet Explorer 8 ha apportato una serie di modifiche al codice di determinazione di tipo MIME e consente agli sviluppatori di applicazioni di [rifiutare esplicitamente lo sniffing MIME.](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx) Il codice seguente è stato aggiunto al file *Web.config.*

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Abilitare l'aggregazione e la minimizzazione

Quando Visual Studio crea un nuovo progetto web, l'aggregazione e la minimizzazione dei file JavaScript non sono abilitate per impostazione predefinita. È stata aggiunta una riga di codice in BundleConfig.cs:We added a line of code in BundleConfig.cs:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Impostare un timeout di scadenza per i cookie di autenticazione

Per impostazione predefinita, i cookie di autenticazione scadono tra due settimane. Un tempo più breve è più sicuro. È possibile modificare questa impostazione in *StartupAuth.cs:*

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Come eseguire l'app da Visual Studio nel computer localeHow to run the app from Visual Studio on your local computer

Esistono due modi per eseguire l'app Fix It:

- Eseguire l'applicazione di base che scrive nuove attività direttamente nel database SQL.
- Eseguire l'applicazione usando una coda più un servizio back-end per creare attività. Il modello di coda è descritto nel capitolo Modello di [lavoro Centra-Centrico](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Eseguire l'applicazione di base

1. Installare [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).
2. Installare [Azure SDK per .NET per Visual Studio](https://azure.microsoft.com/downloads/).
3. Scaricare il file .zip da [MSDN Code Gallery](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. In Esplora file fare clic con il pulsante destro del mouse sul file .zip e scegliere Proprietà, quindi nella finestra Proprietà fare clic su Sblocca.
5. Decomprimere il file.
6. Fare doppio clic sul file sln per avviare Visual Studio.
7. Scegliere Gestione pacchetti **NuGet**dal menu **Strumenti** , quindi **Console gestione pacchetti**.
8. Nella Console di gestione pacchetti (PMC), fare clic su Ripristina.
9. Uscire da Visual Studio.
10. Avviare l'emulatore di [archiviazione di Azure.](/azure/storage/common/storage-use-emulator)
11. Riavviare Visual Studio, aprendo il file di soluzione chiuso nel passaggio precedente.
12. Assicurarsi che il progetto FixIt è impostato come progetto di avvio e quindi premere CTRL e F5 per eseguire il progetto.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Eseguire l'applicazione con l'elaborazione della codaRun the application with queue processing

1. Seguire le istruzioni per [Eseguire l'applicazione di base](#runbase), quindi chiudere il browser e chiudere Visual Studio.
2. Avviare Visual Studio con privilegi di amministratore. Si userà l'emulatore di calcolo di Azure e ciò richiede privilegi di amministratore.
3. Nel file *Web.config* dell'applicazione nel progetto *MyFixIt* (il `appSettings/UseQueues` progetto Web), modificare il valore di su "true":

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Se [l'emulatore](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) di archiviazione di Azure non è ancora in esecuzione, riavviarlo.
5. Eseguire contemporaneamente il progetto Web FixIt e il progetto MyFixItCloudService.

    Utilizzo di Visual Studio:Using Visual Studio:

   1. Premere **F5** per eseguire il progetto FixIt.
   2. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto MyFixItCloudService , quindi scegliere Avvia **debug** > **nuova istanza**.

    Utilizzo di Visual Studio 2013 Express per Web:

   3. In Esplora soluzioni fare clic con il pulsante destro del mouse sulla soluzione FixIt e scegliere **Proprietà**.
   4. Selezionare **Più progetti di avvio**.
   5. Nell'elenco a discesa **Azione** in MyFixIt e MyFixItCloudService selezionare **Start**.
   6. Fare clic su **OK**.
   7. Premere **F5** per eseguire entrambi i progetti.

      Quando si esegue il progetto MyFixItCloudService, Visual Studio avvia l'emulatore di calcolo di Azure.When you run the MyFixItCloudService project, Visual Studio starts the Azure compute emulator. A seconda della configurazione del firewall, potrebbe essere necessario consentire l'emulatore attraverso il firewall.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Come distribuire l'app di base in App Web del servizio app di Azure usando gli script di Windows PowerShell

Per illustrare il modello [Automatizza tutto,](automate-everything.md) l'app Fix It viene fornita con script che configurano un ambiente in Azure e distribuiscono il progetto nel nuovo ambiente. Le istruzioni seguenti spiegano come utilizzare gli script.

Se si vuole eseguire in Azure senza usare le code e le modifiche sono state apportate per l'esecuzione locale con le code, assicurarsi di impostare il valore useQueues appSetting su false prima di procedere con le istruzioni seguenti.

Queste istruzioni presuppongono che la soluzione Fix It sia già stata scaricata ed eseguita in locale e che si disponga di un account Azure o di una sottoscrizione di Azure che si è autorizzati a gestire.

1. Installare la console di **Azure PowerShell.Install** the Azure PowerShell console. Per istruzioni, vedere [Come installare e configurare Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Questa console personalizzata è configurata per funzionare con la sottoscrizione di Azure.This customized console is configured to work with your Azure subscription. Il modulo di Azure viene installato nella directory Programmi e viene importato automaticamente in ogni utilizzo della console di Azure PowerShell.The Azure module is installed in the *Program Files* directory and is automatically imported on every used of the Azure PowerShell console.

    Se si preferisce utilizzare un programma host diverso, ad esempio Windows PowerShell ISE, assicurarsi di usare il cmdlet [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) per importare il modulo di Azure o usare un comando nel modulo di Azure per attivare l'importazione automatica del modulo.
2. Avviare Azure PowerShell con l'opzione **Esegui come amministratore.**
3. Eseguire il cmdlet [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) per impostare `RemoteSigned`i criteri di esecuzione di Azure PowerShell su . Immettere **Y** (per Sì) per completare la modifica del criterio.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Questa impostazione consente di eseguire script locali senza firma digitale. È anche possibile impostare `Unrestricted`i criteri di esecuzione su , eliminando la necessità di un passaggio di sblocco in un secondo momento, ma questa operazione non è consigliata per motivi di sicurezza.
4. Eseguire `Add-AzureAccount` il cmdlet per configurare PowerShell con le credenziali per l'account.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Queste credenziali scadono dopo un periodo di tempo ed `Add-AzureAccount` è necessario eseguire nuovamente il cmdlet. Mentre questo e-book è in fase di scrittura, il limite di tempo prima della scadenza delle credenziali è di 12 ore.
5. Se si dispone di più sottoscrizioni, usare il cmdlet Select-AzureSubscription per specificare la sottoscrizione in cui si vuole creare l'ambiente di test.
6. Importare un certificato di gestione per `Get-AzurePublishSettingsFile` la `Import-AzurePublishSettingsFile` stessa sottoscrizione di Azure usando i cmdlet e . Il primo di questi cmdlet scarica un file di certificato e nel secondo si specifica il percorso di tale file per importarlo. > [!IMPORTANT]
   > Mantenere il file scaricato in un percorso sicuro o eliminarlo al termine, perché contiene un certificato che può essere usato per gestire i servizi di Azure.Keep the downloaded file in a safe location or delete it when you're done with it, because it contains a certificate that can be used to manage your Azure services.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    Il certificato viene utilizzato per una chiamata all'API REST che rileva l'indirizzo IP del computer di sviluppo per impostare una regola del firewall nel server di database SQL.
7. Eseguire il cmdlet [Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) (gli `chdir`alias `sl`sono `cd`, e ) per passare alla directory che contiene gli script. (Si trovano nella cartella *di automazione* nella cartella della soluzione Fix It.) Inserire il percorso tra virgolette se uno dei nomi di directory contiene spazi. Ad esempio, per `c:\Sample Apps\FixIt\Automation` passare alla directory è possibile immettere il comando seguente:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Per consentire a Windows PowerShell di eseguire questi script, utilizzare il cmdlet [Unblock-File.](https://go.microsoft.com/fwlink/p/?linkid=294021) Gli script vengono bloccati perché sono stati scaricati da Internet.

    > [!WARNING]
    > Sicurezza - `Unblock-File` Prima di eseguire su qualsiasi script o file eseguibile, aprire il file nel Blocco note, esaminare i comandi e verificare che non contengano codice dannoso.

    Ad esempio, il comando `Unblock-File` seguente esegue il cmdlet su tutti gli script nella directory corrente.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Per creare l'app Web per l'app Fix It di base (nessuna elaborazione delle code), eseguire lo script di creazione dell'ambiente.

    Il `Name` parametro obbligatorio specifica il nome del database e viene utilizzato anche per l'account di archiviazione creato dallo script. Il nome deve essere univoco a livello globale all'interno del dominio azurewebsites.net. Se si specifica un nome non univoco, ad esempio Fixit o Test (o `New-AzureWebsite` anche come nell'esempio, fixitdemo), il cmdlet ha esito negativo con un errore interno che segnala un conflitto. Lo script converte il nome in lettere minuscole in modo che sia conforme ai requisiti dei nomi per le app Web, gli account di archiviazione e i database.

    Il `SqlDatabasePassword` parametro obbligatorio consente di specificare la password per l'account amministratore che verrà creato per il database SQL. Non includere caratteri XML speciali nella&amp; &lt; &gt; password (;). Si tratta di una limitazione del modo in cui sono stati scritti gli script, non di Azure.This is a limitation of the way the scripts were written, not a limitation of Azure.

    Ad esempio, se si desidera creare un'app Web denominata "fixitdemo" e utilizzare una password di amministratore di SQL Server di "Passw0rd1", è possibile immettere il comando seguente:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    Il nome deve essere univoco nel dominio azurewebsites.net e la password deve soddisfare i requisiti del database SQL per la complessità della password. (L'esempio Passw0rd1 soddisfa i requisiti.)

    Si noti che il comando inizia con ". \". Per impedire l'esecuzione di script dannosi, Windows PowerShell richiede di fornire il percorso completo del file di script quando si esegue uno script. È possibile utilizzare un punto per\"indicare la directory corrente (". ) o fornire il percorso completo, ad esempio:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Per ulteriori informazioni sullo script, utilizzare il `Get-Help` cmdlet.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    È possibile `Detailed`utilizzare `Full` `Parameters`i `Examples` parametri , , e del cmdlet Get-Help per filtrare la Guida restituita.

    Se lo script ha esito negativo o genera errori, ad esempio "New-AzureWebsite: Call Set-AzureSubscription e Select-AzureSubscription prima", è possibile che la configurazione di Azure PowerShell non sia stata completata.

    Al termine dello script, è possibile usare il portale di gestione di Azure per visualizzare le risorse create, come illustrato nel capitolo [Automatizzare tutto.](automate-everything.md)
10. Per distribuire il progetto FixIt nel nuovo ambiente Azure, usare lo script *AzureWebsite.ps1.To* deploy the FixIt project to the new Azure environment, use the AzureWebsite.ps1 script. Ad esempio:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Al termine della distribuzione, il browser viene aperto con Fix It in esecuzione in Azure.When deployment is done, the browser opens with Fix It running in Azure.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Risoluzione dei problemi relativi agli script di Windows PowerShellTroubleshooting the Windows PowerShell scripts

Gli errori più comuni riscontrati durante l'esecuzione di questi script sono correlati alle autorizzazioni. Assicurarsi `Add-AzureAccount` che `Import-AzurePublishSettingsFile` e che hanno avuto esito positivo e che sono stati usati per la stessa sottoscrizione di Azure.Make sure that and were successful and that you used them for the same Azure subscription. Anche `Add-AzureAccount` se ha avuto successo potrebbe essere necessario eseguirlo di nuovo. Le autorizzazioni `Add-AzureAccount` aggiunte da scadranno tra 12 ore.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Riferimento oggetto non impostato su un'istanza di un oggetto.

Se lo script restituisce errori, ad esempio "Riferimento all'oggetto non impostato su un'istanza di un oggetto", il che `Add-AzureAccount` significa che Windows PowerShell non riesce a trovare un oggetto da elaborare (si tratta di un'eccezione di riferimento null), eseguire il cmdlet e riprovare a eseguire lo script.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError: il server ha rilevato un errore interno.

Il `New-AzureWebsite` cmdlet restituisce un errore interno quando il nome non è univoco nel dominio azurewebsites.net. Per risolvere l'errore, usare un valore diverso per il nome, che si trova nel parametro Name di *New-AzureWebsiteEnv.ps1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Riavvio dello script

Se è necessario riavviare lo script *New-AzureWebsiteEnv.ps1* perché non è riuscito prima di stampare il messaggio "Script è stato completato", è possibile eliminare le risorse create dallo script prima dell'arresto. Ad esempio, se lo script ha già creato l'app Web ContosoFixItDemo e lo si esegue nuovamente con lo stesso nome, lo script avrà esito negativo perché il nome è in uso.

Per determinare le risorse create dallo script prima dell'arresto, utilizzare i cmdlet seguenti:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: per eseguire questo cmdlet, assegnare un'epipeiazione al nome del server di database: `Get-AzureSqlDatabase``Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Per eliminare queste risorse, utilizzare i comandi seguenti. Si noti che se si elimina il server di database, si eliminano automaticamente i database associati al server.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Come distribuire l'app con l'elaborazione della coda alle app Web del servizio app di Azure e a un servizio cloud di AzureHow to deploy the app with queue processing to Azure App Web Apps and an Azure Cloud Service

Per abilitare le code, apportare la seguente modifica nel file MyFixIt-Web.config. In `appSettings`, modificare `UseQueues` il valore di "true":

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Distribuire quindi l'applicazione MVC in un'app Web nel servizio app di Azure, come descritto [in precedenza.](#deploybase)

Successivamente, creare un nuovo servizio cloud di Azure.Next, create a new Azure cloud service. Gli script inclusi nell'app Fix It non creano o distribuiscono il servizio cloud, pertanto è necessario usare il portale di Azure. Nel portale fare clic su **Nuova** -- **configurazione** -**Creazione rapida**servizio --  **cloud**e quindi immettere un URL e un percorso del data center. Usare lo stesso data center in cui è stata distribuita l'app Web.

![](the-fix-it-sample-application/_static/image1.png)

Prima di poter distribuire il servizio cloud, è necessario aggiornare alcuni dei file di configurazione.

In MyFixIt.WorkerRole.app.config, `connectionStrings`in , sostituire `appdb` il valore della stringa di connessione con la stringa di connessione effettiva per il database SQL. È possibile ottenere la stringa di connessione dal portale. Nel portale fare clic su **Sql Databases** - **appdb** - Visualizza stringhe di**connessione database SQL per ADO .Net, ODBC, PHP e JDBC**. Copiare la stringa di connessione ADO.NET e incollare il valore nel file app.config. Sostituirlo con\_\_la password del database. Supponendo che siano stati usati gli script per distribuire l'app MVC, è stata specificata la password del database nel parametro `SqlDatabasePassword` script.

Il risultato dovrebbe essere simile al seguente:The result should look like the following:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

Nello stesso file MyFixIt.WorkerRole.app.config, in `appSettings`, sostituire i due valori segnaposto per l'account di archiviazione di Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

È possibile ottenere la chiave di accesso dal portale. Vedere [Come gestire gli account di archiviazione](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

In MyFixItCloudService,ServiceConfiguration.Cloud.cscfg sostituire gli stessi due valori di segnaposto per l'account di archiviazione di Azure.In MyFixItCloudService'ServiceConfiguration.Cloud.cscfg, replace the same two placeholders values for the Azure storage account.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

A questo punto si è pronti per distribuire il servizio cloud. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto MyFixItCloudService e scegliere **Pubblica**. Per altre informazioni, vedere "[Distribuire l'applicazione in Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", che è nella parte 2 di questa [esercitazione.](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36)

> [!div class="step-by-step"]
> [Indietro](more-patterns-and-guidance.md)
