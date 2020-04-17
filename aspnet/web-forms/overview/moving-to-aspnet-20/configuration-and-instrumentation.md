---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Configurazione e Strumentazione Documenti Microsoft
author: rick-anderson
description: La configurazione e la strumentazione sono state apportate in ASP.NET 2.0. La nuova API di configurazione ASP.NET consente di apportare modifiche alla configurazione pr...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: 30ea2ffd3d055c5373a33615bc19a8f68fb568ab
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543054"
---
# <a name="configuration-and-instrumentation"></a>Configurazione e strumentazione

da parte [di Microsoft](https://github.com/microsoft)

> La configurazione e la strumentazione sono state apportate in ASP.NET 2.0. La nuova API di configurazione ASP.NET consente di apportare modifiche alla configurazione a livello di codice. Inoltre, esistono molte nuove impostazioni di configurazione che consentono nuove configurazioni e strumentazione.

La configurazione e la strumentazione sono state apportate in ASP.NET 2.0. La nuova API di configurazione ASP.NET consente di apportare modifiche alla configurazione a livello di codice. Inoltre, esistono molte nuove impostazioni di configurazione che consentono nuove configurazioni e strumentazione.

In questo modulo, discuteremo ASP.NET'API di configurazione in relazione alla lettura e scrittura nei file di configurazione di ASP.NET, e tratteremo anche ASP.NET strumentazione. Parleremo anche delle nuove funzionalità disponibili nel ASP.NET tracciamento.

## <a name="aspnet-configuration-api"></a>API di configurazione di ASP.NET

L'API di configurazione ASP.NET consente di sviluppare, distribuire e gestire i dati di configurazione delle applicazioni utilizzando un'unica interfaccia di programmazione. È possibile utilizzare l'API di configurazione per sviluppare e modificare configurazioni complete ASP.NET a livello di codice senza modificare direttamente il codice XML nei file di configurazione. Inoltre, è possibile utilizzare l'API di configurazione nelle applicazioni console e negli script sviluppati, negli strumenti di gestione basati sul Web e negli snap-in di Microsoft Management Console (MMC).

I due strumenti di gestione della configurazione seguenti utilizzano l'API di configurazione e sono inclusi in .NET Framework versione 2.0:

- Lo snap-in MMC ASP.NET, che utilizza l'API di configurazione per semplificare le attività amministrative, fornendo una visualizzazione integrata dei dati di configurazione locali da tutti i livelli della gerarchia di configurazione.
- Lo strumento Amministrazione sito Web, che consente di gestire le impostazioni di configurazione per le applicazioni locali e remote, inclusi i siti ospitati.

L'API di configurazione ASP.NET comprende un set di oggetti di gestione ASP.NET che è possibile utilizzare per configurare siti Web e applicazioni a livello di codice. Gli oggetti di gestione vengono implementati come libreria di classi .NET Framework.Management objects are implemented as a .NET Framework class library. Il modello di programmazione dell'API di configurazione consente di garantire la coerenza e l'affidabilità del codice applicando i tipi di dati in fase di compilazione. Per semplificare la gestione delle configurazioni dell'applicazione, l'API di configurazione consente di visualizzare i dati uniti da tutti i punti della gerarchia di configurazione come una singola raccolta, anziché visualizzare i dati come raccolte separate da file di configurazione diversi. Inoltre, l'API di configurazione consente di modificare intere configurazioni dell'applicazione senza modificare direttamente il codice XML nei file di configurazione. Infine, l'API semplifica le attività di configurazione supportando gli strumenti di amministrazione, ad esempio lo strumento Amministrazione sito Web. L'API di configurazione semplifica la distribuzione supportando la creazione di file di configurazione in un computer ed eseguendo script di configurazione su più computer.

> [!NOTE]
> L'API di configurazione non supporta la creazione di applicazioni IIS.

## <a name="working-with-local-and-remote-configuration-settings"></a>Utilizzo delle impostazioni di configurazione locale e remota

Oggetto Configuration oggetto rappresenta la visualizzazione unita delle impostazioni di configurazione che si applicano a un'entità fisica specifica, ad esempio un computer, o a un'entità logica, ad esempio un'applicazione o un sito Web. L'entità logica specificata può esistere nel computer locale o in un server remoto. Quando non esiste alcun file di configurazione per un'entità specificata, l'oggetto Configuration rappresenta le impostazioni di configurazione predefinite definite dal file Machine.config.

È possibile ottenere un oggetto Configuration utilizzando uno dei metodi di configurazione aperti dalle classi seguenti:You can get a Configuration object by using one of the open configuration methods from the following classes:

1. La classe ConfigurationManager, se l'entità è un'applicazione client.
2. La classe WebConfigurationManager, se l'entità è un'applicazione Web.

Questi metodi restituiranno un Configuration oggetto, che a sua volta fornisce i metodi e le proprietà necessarie per gestire i file di configurazione sottostanti. È possibile accedere a questi file per la lettura o la scrittura.

### <a name="reading"></a>Lettura

Utilizzare il GetSection o GetSectionGroup metodo per leggere le informazioni di configurazione. L'utente o il processo che legge deve disporre delle autorizzazioni di lettura per tutti i file di configurazione nella gerarchia.

> [!NOTE]
> Se si utilizza un metodo GetSection statico che accetta un parametro path, il parametro path deve fare riferimento all'applicazione in cui è in esecuzione il codice. In caso contrario, il parametro viene ignorato e vengono restituite le informazioni di configurazione per l'applicazione attualmente in esecuzione.

### <a name="writing"></a>Scrittura

Utilizzare uno dei metodi Save per scrivere le informazioni di configurazione. L'utente o il processo che scrive deve disporre delle autorizzazioni di scrittura per il file di configurazione e la directory al livello corrente della gerarchia di configurazione, nonché delle autorizzazioni di lettura per tutti i file di configurazione nella gerarchia.

Per generare un file di configurazione che rappresenta le impostazioni di configurazione ereditate per un'entità specificata, utilizzare uno dei seguenti metodi di configurazione di salvataggio:

1. Il Save metodo per creare un nuovo file di configurazione.
2. Il SaveAs metodo per generare un nuovo file di configurazione in un'altra posizione.

## <a name="configuration-classes-and-namespaces"></a>Classi di configurazione e spazi dei nomiConfiguration Classes and Namespaces

Molte classi e metodi di configurazione sono simili tra loro. Nella tabella seguente vengono descritte le classi di configurazione e gli spazi dei nomi utilizzati più di più di più di seguito.

| **Classe o spazio dei nomi di configurazione** | **Descrizione** |
| --- | --- |
| Spazio dei nomi [System.Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) | Contiene le classi di configurazione principali per tutte le applicazioni .NET Framework. Section handler classes are used to obtain configuration data for a section from methods, such as GetSection and GetSectionGroup. Questi due metodi non sono statici. |
| Classe System.Configuration.Configuration | Rappresenta un set di dati di configurazione per un computer, un'applicazione, una directory Web o un'altra risorsa. Questa classe contiene metodi utili, ad esempio GetSection e GetSectionGroup, per aggiornare le impostazioni di configurazione e ottenere riferimenti a sezioni e gruppi di sezioni. Questa classe viene utilizzata come tipo restituito per i metodi che ottengono i dati di configurazione in fase di progettazione, ad esempio i metodi delle classi WebConfigurationManager e ConfigurationManager. |
| Spazio dei nomi System.Web.Configuration | Contiene le classi del gestore di sezione per le sezioni di configurazione ASP.NET definite in [ASP.NET Impostazioni di configurazione](https://msdn.microsoft.com/library/b5ysx397.aspx). Section handler classes are used to obtain configuration data for a section from methods, such as GetSection and GetSectionGroup. |
| Classe System.Web.Configuration.WebConfigurationManager | Fornisce metodi utili per ottenere riferimenti alle impostazioni di configurazione in fase di esecuzione e in fase di progettazione. Questi metodi utilizzano la classe System.Configuration.Configuration come tipo restituito. È possibile utilizzare il metodo statico GetSection metodo di questa classe o il non statico GetSection metodo il System.Configuration.ConfigurationManager classe in modo intercambiabile. Per le configurazioni di applicazioni Web, è consigliata la classe System.Web.Configuration.WebConfigurationManager anziché la classe System.Configuration.ConfigurationManager . |
| Spazio dei nomi [System.Configuration.Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) | Fornisce un modo per personalizzare ed estendere il provider di configurazione. Questa è la classe base per tutte le classi di provider nel sistema di configurazione. |
| Spazio dei nomi [System.Web.Management](https://msdn.microsoft.com/library/system.web.management.aspx) | Contiene classi e interfacce per la gestione e il monitoraggio dell'integrità delle applicazioni Web. In senso stretto, questo spazio dei nomi non è considerato parte dell'API di configurazione. Ad esempio, la traccia e la generazione di eventi vengono eseguite dalle classi in questo spazio dei nomi. |
| Spazio dei nomi [System.Management.Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) | Fornisce alle classi necessarie per la strumentazione delle applicazioni di esporre le informazioni di gestione e gli eventi tramite Strumentazione gestione Windows (WMI) ai potenziali consumer. ASP.NET monitoraggio dello stato utilizza WMI per recapitare gli eventi. In senso stretto, questo spazio dei nomi non è considerato parte dell'API di configurazione. |

## <a name="reading-from-aspnet-configuration-files"></a>Lettura da ASP.NET file di configurazione

La classe WebConfigurationManager è la classe principale per la lettura da ASP.NET file di configurazione. Esistono essenzialmente tre passaggi per leggere i file di configurazione ASP.NET:There are essentially three steps to reading ASP.NET configuration files:

1. Ottenere un oggetto Configuration utilizzando il metodo OpenWebConfiguration.
2. Ottenere un riferimento alla sezione desiderata nel file di configurazione.
3. Leggere le informazioni desiderate dal file di configurazione.

Il Configuration oggetto rappresenta non rappresenta un particolare file di configurazione. Rappresenta invece una visualizzazione unita della configurazione di un computer, un'applicazione o un sito Web. Nell'esempio di codice riportato di seguito viene creata un'istanza di un oggetto Configuration che rappresenta la configurazione di un'applicazione Web denominata *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Si noti che se il percorso /ProductInfo non esiste, il codice precedente restituirà la configurazione predefinita specificata nel file machine.config.

Dopo aver ottenuto l'oggetto Configuration, è possibile utilizzare il metodo GetSection o GetSectionGroup per esaminare le impostazioni di configurazione. L'esempio seguente ottiene un riferimento alle impostazioni di rappresentazione per l'applicazione ProductInfo precedente:The following example gets a reference to the impersonation settings for the above ProductInfo application:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Scrittura nei file di configurazione di ASP.NET

Come nella lettura dai file di configurazione, la classe WebConfigurationManager è il nucleo per la scrittura in Asp.NET file di configurazione. Sono inoltre disponibili tre passaggi per scrivere nei file di configurazione ASP.NET.

1. Ottenere un oggetto Configuration utilizzando il metodo OpenWebConfiguration.
2. Ottenere un riferimento alla sezione desiderata nel file di configurazione.
3. Scrivere le informazioni desiderate dal file di configurazione utilizzando il metodo Save o SaveAs.

Il codice seguente modifica l'attributo **debug** dell'elemento &lt;compilation&gt; su false:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Quando viene eseguito questo **debug** codice, &lt;l'attributo debug dell'elemento compilation&gt; verrà impostato su false per il file web.config dell'applicazione *webApp.*

## <a name="systemwebmanagement-namespace"></a>Spazio dei nomi System.Web.Management

Lo spazio dei nomi System.Web.Management fornisce le classi e le interfacce per la gestione e il monitoraggio dell'integrità delle applicazioni ASP.NET.

La registrazione viene eseguita definendo una regola che associa gli eventi a un provider. La regola definisce il tipo di eventi inviati al provider. Per la registrazione sono disponibili i seguenti eventi di base:

| **Webbaseevent** | Classe di evento di base per tutti gli eventi. Contiene le proprietà necessarie per tutti gli eventi, ad esempio il codice evento, il codice di dettaglio dell'evento, la data e l'ora in cui l'evento è stato generato, il numero di sequenza, il messaggio dell'evento e i dettagli dell'evento. |
| --- | --- |
| **Evento WebManagementEvent** | Classe di eventi di base per gli eventi di gestione, ad esempio eventi di durata, richiesta, errore e controllo dell'applicazione. |
| **Evento WebHeartbeatEvent** | Evento generato dall'applicazione a intervalli regolari per acquisire informazioni utili sullo stato di runtime. |
| **Webauditevent** | Classe base per gli eventi di controllo di sicurezza, utilizzati per contrassegnare condizioni quali l'errore di autorizzazione, l'errore di decrittografia *e così via.* |
| **Webrequestevent** | Classe base per tutti gli eventi di richiesta informativa. |
| **Evento WebBaseErrorEvent** | Classe base per tutti gli eventi che indicano le condizioni di errore. |

I tipi di provider disponibili consentono di inviare l'output degli eventi al Visualizzatore eventi, SQL Server, Strumentazione gestione Windows (WMI) e alla posta elettronica. I provider preconfigurati e i mapping degli eventi riducono la quantità di lavoro necessaria per registrare l'output degli eventi.

ASP.NET 2.0 utilizza il provider del registro eventi out-of-the-box per registrare gli eventi in base all'avvio e all'arresto dei domini applicazione, nonché registrare eventuali eccezioni non gestite. Questo aiuta a coprire alcuni degli scenari di base. Si supponga, ad esempio, che l'applicazione genera un'eccezione, ma l'utente non salva l'errore e non è possibile riprodurlo. Con la regola predefinita del registro eventi, si sarebbe in grado di raccogliere l'eccezione e le informazioni sullo stack per avere un'idea migliore del tipo di errore che si è verificato. Un altro esempio si applica se l'applicazione perde lo stato della sessione. In tal caso, è possibile esaminare il registro eventi per determinare se il dominio applicazione è in grado di eseguire il riciclo e il motivo per cui il dominio applicazione è stato arrestato in primo luogo.

Inoltre, il sistema di monitoraggio dello stato è estensibile. Ad esempio, è possibile definire eventi Web personalizzati, generarli all'interno dell'applicazione e quindi definire una regola per inviare le informazioni sull'evento a un provider, ad esempio il messaggio di posta elettronica. Ciò consente di collegare facilmente la strumentazione ai provider di monitoraggio dello stato. Come altro esempio, è possibile generare un evento ogni volta che viene elaborato un ordine e impostare una regola che invia ogni evento al database di SQL Server.As another example, you could fire an event each time an order is processed and set up a rule that sends each event to the SQL Server database. È inoltre possibile generare un evento quando un utente non riesce ad accedere più volte di seguito e impostare l'evento per l'utilizzo dei provider basati su posta elettronica.

La configurazione per i provider e gli eventi predefiniti viene archiviata nel file Web.config globale. Nel file Web.config globale vengono archiviate in ASP.NET 1x tutte le impostazioni basate sul Web archiviate nel file Machine.config. Il file Web.config globale si trova nella seguente directory:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

La &lt;sezione&gt; healthMonitoring del file Web.config globale fornisce le impostazioni di configurazione predefinite. È possibile eseguire l'override di queste &lt;impostazioni&gt; o configurare impostazioni personalizzate implementando la sezione healthMonitoring nel file Web.config per l'applicazione.

La &lt;sezione&gt; healthMonitoring del file Web.config globale contiene gli elementi seguenti:

| **Provider** | Contiene i provider impostati per il Visualizzatore eventi, WMI e SQL Server. |
| --- | --- |
| **Eventmappings** | Contiene i mapping per le varie classi WebBase. È possibile estendere questo elenco se si genera una classe di evento personalizzata. La generazione di una classe di evento personalizzata offre una granularità più fine rispetto ai provider a cui si inviano informazioni. Ad esempio, è possibile configurare le eccezioni non gestite da inviare a SQL ServerSQL Server, inviando al messaggio di posta elettronica eventi personalizzati. |
| **Regole** | Collega eventMappings al provider. |
| **Buffer** | Utilizzato con SQL ServerSQL Server e i provider di posta elettronica per determinare la frequenza con cui scaricare gli eventi nel provider. |

Di seguito è riportato un esempio di codice dal file Web.config globale.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Come archiviare gli eventi nel Visualizzatore eventi

Come accennato in precedenza, il provider per la registrazione degli eventi nel Visualizzatore eventi è configurato automaticamente nel file Web.config globale. Per impostazione predefinita, vengono registrati tutti gli eventi basati su **WebBaseErrorEvent** e **WebFailureAuditEvent.** È possibile aggiungere ulteriori regole per registrare informazioni aggiuntive nel registro eventi. Ad esempio, se si desidera registrare tutti gli eventi *(ad esempio*, ogni evento basato su **WebBaseEvent**), è possibile aggiungere la seguente regola al file Web.config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Questa regola collegherebbe la mappa **eventi Tutti gli eventi** al provider del registro eventi. Sia eventMapping che il provider sono inclusi nel file Web.config globale.

## <a name="how-to-store-events-to-sql-server"></a>Come archiviare gli eventi in SQL ServerHow to store events to SQL Server

Questo metodo utilizza il database **ASPNETDB,** generato\_dallo strumento Aspnet regsql.exe. Il provider predefinito utilizza la stringa di connessione LocalSqlServer, che\_utilizza un database basato su file nella cartella dati dell'app o l'istanza SQLExpress locale di SQL Server. Sia la stringa di connessione LocalSqlServer che l'Oggetto SqlProvider vengono configurati nel file Web.config globale.

La stringa di connessione LocalSqlServer nel file Web.config globale è simile alla seguente:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Se si desidera utilizzare un'altra istanza di SQL Server, è necessario utilizzare lo strumento Aspnet\_regsql.exe, disponibile in %windir%. \* nella cartella. Utilizzare lo\_strumento Aspnet regsql.exe per generare un database **ASPNETDB** personalizzato nell'istanza di SQL Server, quindi aggiungere la stringa di connessione al file di configurazione delle applicazioni e quindi aggiungere un provider utilizzando la nuova stringa di connessione. Dopo aver creato il database **ASPNETDB,** è necessario impostare una regola per collegare un eventMapping a sqlProvider.

Se si utilizza il sqlProvider predefinito o configurare il proprio provider, è necessario aggiungere una regola che collega il provider con una mappa eventi. La regola seguente collega il nuovo provider creato in precedenza alla mappa eventi **Tutti gli eventi.** Questa regola registrerà tutti gli eventi basati su **WebBaseEvent** e li invierà a MySqlWebEventProvider che utilizzerà la stringa di connessione MYASPNETDB. Il codice seguente aggiunge una regola per collegare il provider a una mappa eventi:The following code adds a rule to link the provider with an event map:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Se si desidera inviare solo errori a SQL Server, è possibile aggiungere la regola seguente:If you wanted to only send errors to SQL ServerSQL Server, you could add the following rule:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Come inoltrare gli eventi a WMI

È inoltre possibile inoltrare gli eventi a WMI. Il provider WMI è configurato per l'utente nel file Web.config globale per impostazione predefinita.

Nell'esempio di codice riportato di seguito viene aggiunta una regola per l'inoltro degli eventi a WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Sarà necessario aggiungere una regola per associare un eventMapping al provider e anche un'applicazione listener WMI per l'ascolto degli eventi. Nell'esempio di codice riportato di seguito viene aggiunta una regola per collegare il provider WMI alla mappa **eventi Tutti gli eventi:**

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Come inoltrare gli eventi alla posta elettronica

Puoi anche inoltrare gli eventi alla posta elettronica. Prestare attenzione alle regole di evento mappate al provider di posta elettronica, in quanto è possibile inviare inavvertitamente molte informazioni che potrebbero essere più adatte per SQL Server o il registro eventi. Ci sono due provider di posta elettronica; SimpleMailWebEventProvider e TemplatedMailWebEventProvider. Ognuno ha gli stessi attributi di configurazione, ad eccezione degli attributi "template" e "detailedTemplateErrors", entrambi disponibili solo nel TemplatedMailWebEventProvider.

> [!NOTE]
> Nessuno di questi provider di posta elettronica è configurato per l'utente. Sarà necessario aggiungerli al file Web.config.

La differenza principale tra questi due provider di posta elettronica è che SimpleMailWebEventProvider invia messaggi di posta elettronica in un modello generico che non può essere modificato. Il file Web.config di esempio aggiunge questo provider di posta elettronica all'elenco dei provider configurati utilizzando la regola seguente:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

Viene inoltre aggiunta la regola seguente per legare il provider di posta elettronica alla mappa **eventi Tutti gli eventi:The** following rule is also added to tie the email provider to the All Events event map:

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>Traccia di ASP.NET 2.0

Sono stati apportati tre miglioramenti principali alla traccia in ASP.NET 2.0.There are three major enhancements to tracing in ASP.NET 2.0.

1. Funzionalità di tracciamento integrata
2. Accesso a livello di codice ai messaggi di tracciaProgrammatic access to trace messages
3. Traccia a livello di applicazione migliorataImproved application-level tracing

## <a name="integrated-tracing-functionality"></a>Funzionalità di traccia integrata

È ora possibile instradare i messaggi generati dalla classe System.Diagnostics.Trace a ASP.NET'output di analisi e i messaggi generati dallASP.NET'analisi a System.Diagnostics.Trace. È inoltre possibile inoltrare ASP.NET eventi di strumentazione a System.Diagnostics.Trace.You can also forward ASP.NET instrumentation events to System.Diagnostics.Trace. Questa funzionalità viene fornita dal nuovo attributo &lt;&gt; **writeToDiagnosticsTrace** dell'elemento di traccia. Quando questo valore booleano è true, i messaggi di traccia ASP.NET vengono inoltrati all'infrastruttura di traccia System.Diagnostics per l'utilizzo da parte di qualsiasi listener registrato per visualizzare i messaggi di traccia.

## <a name="programmatic-access-to-trace-messages"></a>Accesso a livello di codice ai messaggi di tracciaProgrammatic Access to Trace Messages

ASP.NET 2.0 consente l'accesso a livello di codice a tutti i messaggi di traccia tramite la classe **TraceContextRecord** e l'insieme **TraceRecords.** Il modo più efficiente di accedere ai messaggi di traccia consiste nel registrare un delegato **TraceContextEventHandler** (anche nuovo in ASP.NET 2.0) per gestire il nuovo evento **TraceFinished.** È quindi possibile scorrere i messaggi di traccia come si desidera.

Nell'esempio di codice seguente viene illustrato quanto segue:The following code sample illustrates this:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

Nell'esempio precedente, scorrere in ciclo la raccolta TraceRecords e quindi scrivere ogni messaggio nel flusso di risposta.

## <a name="improved-application-level-tracing"></a>Traccia a livello di applicazione migliorataImproved Application-Level Tracing

L'analisi a livello di applicazione è stata migliorata&gt; tramite l'introduzione del nuovo attributo **mostRecent** dell'elemento &lt;di traccia. Questo attributo specifica se viene visualizzato l'output di traccia a livello di applicazione più recente e i dati di traccia meno recenti oltre i limiti indicati da requestLimit vengono eliminati. Se false, i dati di traccia vengono visualizzati per le richieste fino a quando non viene raggiunto l'attributo requestLimit.

## <a name="aspnet-command-line-tools"></a>Strumenti della riga di comando di ASP.NET

Sono disponibili diversi strumenti della riga di comando per facilitare la configurazione dei ASP.NET. ASP.NET sviluppatori devono avere familiarità con lo strumento aspnet\_regiis.exe. ASP.NET 2.0 fornisce altri tre strumenti della riga di comando per facilitare la configurazione.

Sono disponibili i seguenti strumenti da riga di comando:

| **Strumento** | **Utilizzare** |
| --- | --- |
| **aspnet\_regiis.exe** | Consente la registrazione di ASP.NET con IIS. Esistono due versioni di questi strumenti forniti con ASP.NET 2.0, una per i sistemi a 32 bit (nella cartella Framework) e una per i sistemi a 64 bit (nella cartella Framework64). La versione a 64 bit non verrà installata in un sistema operativo a 32 bit. |
| **aspnet\_regsql.exe** | Lo strumento di registrazione ASP.NET SQL Server viene utilizzato per creare un database di Microsoft SQL Server per l'utilizzo da parte dei provider di SQL Server in ASP.NET o per aggiungere o rimuovere opzioni da un database esistente. Il file\_Aspnet regsql.exe si trova nella cartella [unità:] WINDOWS. |
| **aspnet\_regbrowsers.exe** | Lo strumento ASP.NET registrazione browser analizza e compila tutte le definizioni del browser a livello di sistema in un assembly e installa l'assembly nella Global Assembly Cache. Lo strumento utilizza i file di definizione del browser (. BROWSER) dalla sottodirectory .NET Framework Browsers. Lo strumento è reperibile nella directory %SystemRoot%. |
| **compilatore\_aspnet.exe** | Lo strumento di compilazione ASP.NET consente di compilare un'applicazione Web ASP.NET, sul posto o per la distribuzione in un percorso di destinazione, ad esempio un server di produzione. La compilazione sul posto consente le prestazioni dell'applicazione perché gli utenti finali non riscontrano un ritardo nella prima richiesta all'applicazione durante la compilazione dell'applicazione. |

Poiché lo\_strumento aspnet regiis.exe non è una novità per ASP.NET 2.0, non ne parleremo qui.

## <a name="aspnet-sql-server-registration-tool---aspnet_regsqlexe"></a>ASP.NET strumento di registrazione\_di SQL Server - aspnet regsql.exe

È possibile impostare diversi tipi di opzioni utilizzando lo strumento di registrazione di ASP.NET SQL Server.You can set several types of options using the ASP.NET SQL Server Registration tool. È possibile specificare una connessione SQL, specificare quali ASP.NET servizi dell'applicazione utilizzano SQL Server per gestire le informazioni, indicare il database o la tabella utilizzata per la dipendenza della cache SQL e aggiungere o rimuovere il supporto per l'utilizzo di SQL Server per archiviare le procedure e lo stato della sessione.

Diversi ASP.NET servizi applicativi si basano su un provider per gestire l'archiviazione e il recupero dei dati da un'origine dati. Ogni provider è specifico dell'origine dati. ASP.NET include un provider SQL Server per le funzionalità di ASP.NET seguenti:To includes a SQL Server provider for the following ASP.NET features:

- Appartenenza (la classe [SqlMembershipProvider).](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx)
- Gestione dei ruoli (la classe [SqlRoleProvider).](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx)
- Profile (la classe [SqlProfileProvider).](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx)
- Personalizzazione web part (classe [SqlPersonalizationProvider).](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx)
- Eventi Web (la classe [SqlWebEventProvider).](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx)

Quando si installa ASP.NET, il file Machine.config per il server include elementi di configurazione che specificano i provider SQL Server per ognuna delle funzionalità di ASP.NET che si basano su un provider. Questi provider sono configurati, per impostazione predefinita, per connettersi a un'istanza utente locale di SQL Server Express 2005. Se si modifica la stringa di connessione predefinita utilizzata dai provider, prima di poter utilizzare una delle funzionalità ASP.NET configurate nella configurazione\_del computer, è necessario installare il database di SQL Server e gli elementi di database per la funzionalità scelta utilizzando Aspnet regsql.exe. Se il database specificato con lo strumento di registrazione SQL non esiste già (aspnetdb sarà il database predefinito se non ne viene specificato uno nella riga di comando), l'utente corrente deve disporre dei diritti per creare database in SQL Server e per creare elementi dello schema all'interno di un database.

### <a name="sql-cache-dependency"></a>Dipendenza dalla cache SQL

Una funzionalità avanzata della memorizzazione nella cache di output di ASP.NET è la dipendenza della cache SQL. La dipendenza della cache SQL supporta due diverse modalità di funzionamento: una che utilizza un'implementazione ASP.NET del polling delle tabelle e una seconda modalità che utilizza le funzionalità di notifica delle query di SQL Server 2005. Lo strumento di registrazione SQL può essere utilizzato per configurare la modalità di funzionamento di polling delle tabelle.

### <a name="session-state"></a>Stato della sessione

Per impostazione predefinita, i valori e le informazioni dello stato sessione vengono archiviati in memoria all'interno del processo ASP.NET. In alternativa, è possibile archiviare i dati della sessione in un database di SQL Server, dove possono essere condivisi da più server Web. Se il database specificato per lo stato sessione con lo strumento di registrazione SQL non esiste già, l'utente corrente deve disporre dei diritti per creare database in SQL Server e per creare elementi dello schema all'interno di un database. Se il database esiste, l'utente corrente deve disporre dei diritti per creare elementi dello schema nel database esistente.

Per installare il database dello stato sessione\_in SQL Server, eseguire lo strumento Aspnet regsql.exe e fornire le seguenti informazioni con il comando:

- Nome dell'istanza di SQL ServerSQL Server , utilizzando l'opzione **-S.**
- Credenziali di accesso per un account che dispone dell'autorizzazione per creare un database in un computer che esegue SQL Server. Utilizzare l'opzione **-E** per utilizzare l'utente attualmente connesso oppure l'opzione **-U** per specificare un ID utente insieme all'opzione **-P** per specificare una password.
- L'opzione della riga di comando **-ssadd** per aggiungere il database dello stato sessione.

Per impostazione predefinita, non\_è possibile utilizzare lo strumento Aspnet regsql.exe per installare il database dello stato sessione in un computer che esegue SQL Server 2005 Express Edition.

### <a name="the-aspnet-browser-registration-tool---aspnet_regbrowsersexe"></a>Lo strumento di registrazione\_del browser ASP.NET - aspnet regbrowsers.exe

In ASP.NET versione 1.1, il file Machine.config conteneva una sezione denominata &lt;browserCaps&gt;. Questa sezione contiene una serie di voci XML che definisce le configurazioni per vari browser in base a un'espressione regolare. Per ASP.NET versione 2.0, un nuovo file . BROWSER definisce i parametri di un particolare browser utilizzando le voci XML. È possibile aggiungere informazioni su un nuovo browser aggiungendo un nuovo file . BROWSER nella cartella che si trova in %SystemRoot% .

Poiché un'applicazione non legge un file con estensione config ogni volta che richiede informazioni sul browser, è possibile creare un nuovo file . BROWSER ed eseguire\_Aspnet regbrowsers.exe per aggiungere le modifiche necessarie all'assembly. Ciò consente al server di accedere immediatamente alle nuove informazioni del browser in modo da non dover chiudere nessuna delle applicazioni per raccogliere le informazioni. Un'applicazione può accedere alle funzionalità del browser tramite la proprietà Browser dell'oggetto HttpRequest corrente.

Quando si esegue aspnet\_regbrowser.exe, sono disponibili le seguenti opzioni:

| **Opzione** | **Descrizione** |
| --- | --- |
| **-?** | Visualizza il\_testo della Guida Aspnet regbbrowsers.exe nella finestra di comando. |
| **-i** | Crea l'assembly delle funzionalità del browser runtime e lo installa nella Global Assembly Cache. |
| **-u** | Disinstalla l'assembly delle funzionalità del browser runtime dalla Global Assembly Cache. |

## <a name="the-aspnet-compilation-tool---aspnet_compilerexe"></a>Strumento di compilazione\_ASP.NET - compilatore aspnet.exe

Lo strumento di compilazione ASP.NET può essere utilizzato in due modi generali: per la compilazione sul posto e la compilazione per la distribuzione, in cui viene specificata una directory di output di destinazione.

### <a name="compiling-an-application-in-place"></a>[Compilazione di un'applicazione sul postoCompiling an Application in Place](https://msdn.microsoft.com/library/ms229863.aspx)

Il ASP.NET strumento di compilazione può compilare un'applicazione sul posto, ovvero simula il comportamento di effettuare più richieste all'applicazione, causando così la compilazione regolare. Gli utenti di un sito precompilato non subiranno un ritardo causato dalla compilazione della pagina alla prima richiesta.

Quando si precompila un sito sul posto, si applicano i seguenti elementi:

- Il sito mantiene i file e la struttura di directory.
- È necessario disporre di compilatori per tutti i linguaggi di programmazione utilizzati dal sito nel server.
- Se la compilazione di un file non riesce, la compilazione dell'intero sito non riesce.

È inoltre possibile ricompilare un'applicazione sul posto dopo aver aggiunto nuovi file di origine. Lo strumento compila solo i file nuovi o modificati, a meno che non si includa l'opzione **-c.**

> [!NOTE]
> La compilazione di un'applicazione che contiene un'applicazione annidata non compila l'applicazione annidata. L'applicazione annidata deve essere compilata separatamente.

### <a name="compiling-an-application-for-deployment"></a>[Compilazione di un'applicazione per la distribuzioneCompiling an Application for Deployment](https://msdn.microsoft.com/library/ms229863.aspx)

Compilare un'applicazione per la distribuzione (compilazione in un percorso di destinazione) specificando il targetDir parametro. il targetDir può essere il percorso finale per l'applicazione Web o l'applicazione compilata può essere distribuita ulteriormente. L'utilizzo dell'opzione **-u** consente di compilare l'applicazione in modo che sia possibile apportare modifiche a determinati file nell'applicazione compilata senza ricompilarla. Aspnet\_compiler.exe distingue tra tipi di file statici e dinamici e li gestisce in modo diverso durante la creazione dell'applicazione risultante.

- I tipi di file statici sono quelli a cui non è associato un compilatore o un provider di compilazione, ad esempio file il cui nome ha estensioni quali css, gif, htm, html, jpg, js e così via. Questi file vengono semplicemente copiati nella posizione di destinazione, con le relative posizioni nella struttura di directory mantenute.
- I tipi di file dinamici sono quelli a cui è associato un compilatore o un provider di compilazione, inclusi i file con estensioni di file specifiche di ASP.NET, ad esempio asax, ascx, ashx, aspx, browser, master e così via. Lo strumento di compilazione ASP.NET genera assembly da questi file. Se l'opzione **-u** viene omessa, lo strumento crea anche file con estensione . COMPILED che eseguono il mapping dei file di origine originali all'assembly. Per garantire che la struttura di directory dell'origine dell'applicazione venga mantenuta, lo strumento genera file segnaposto nei percorsi corrispondenti nell'applicazione di destinazione.

È necessario utilizzare l'opzione **-u** per indicare che il contenuto dell'applicazione compilata può essere modificato. In caso contrario, le modifiche successive vengono ignorate o causano errori di runtime.

Nella tabella seguente viene descritto come lo strumento di compilazione ASP.NET gestisce diversi tipi di file quando è inclusa l'opzione **-u.**

| **Tipo file** | **Azione del compilatore** |
| --- | --- |
| .ascx, .aspx, .master | Questi file sono suddivisi in markup e codice sorgente, che include sia &lt;i file code-behind che qualsiasi codice racchiuso in script runat : elementi "server".&gt; Il codice sorgente viene compilato in assembly, con nomi derivati da un algoritmo hash, e gli assembly vengono inseriti nella directory Bin. Qualsiasi codice inline, ovvero codice racchiuso ** % ** tra le ** &lt; ** parentesi quadre e , viene incluso nel markup e non compilato. I nuovi file con lo stesso nome dei file di origine vengono creati per contenere il markup e inseriti nelle directory di output corrispondenti. |
| .ashx, .asmx | Questi file non vengono compilati e vengono spostati nelle directory di output così come sono e non vengono compilati. Se si desidera che il codice del gestore venga compilato, inserire il codice nei file di codice sorgente nella directory del\_codice dell'app. |
| .cs, .vb, .jsl, .cpp (esclusi i file code-behind per i tipi di file elencati in precedenza) | Questi file vengono compilati e inclusi come risorsa negli assembly che vi fanno riferimento. I file di origine non vengono copiati nella directory di output. Se non viene fatto riferimento a un file di codice, non viene compilato. |
| Tipi di file personalizzati | Questi file non vengono compilati. Questi file vengono copiati nelle directory di output corrispondenti. |
| File di codice\_sorgente nella sottodirectory Del codice dell'applicazioneSource code files in the App Code subdirectory | Questi file vengono compilati in assembly e inseriti nella directory Bin. |
| File con estensione resx e\_risorsa nella sottodirectory App GlobalResources | Questi file vengono compilati in assembly e inseriti nella directory Bin. Nessuna\_sottodirectory App GlobalResources viene creata nella directory di output principale e nessun file con estensione resx o resources che si trovano nella directory di origine viene copiato nelle directory di output. |
| File con estensione resx e\_risorsa nella sottodirectory App LocalResources | Questi file non vengono compilati e vengono copiati nelle directory di output corrispondenti. |
| File .skin nella\_sottodirectory App Themes | I file .skin e i file dei temi statici non vengono compilati e vengono copiati nelle directory di output corrispondenti. |
| .browser Web.config Tipi di file statici Assembly già presenti nella directory Bin | Questi file vengono copiati così come sono nelle directory di output. |

Nella tabella seguente viene descritto come lo strumento di compilazione ASP.NET gestisce diversi tipi di file quando l'opzione **-u** viene omessa.

| **Tipo file** | **Azione del compilatore** |
| --- | --- |
| aspx, asmx, ashx, master | Questi file sono suddivisi in markup e codice sorgente, che include sia &lt;i file code-behind che qualsiasi codice racchiuso in script runat : elementi "server".&gt; Il codice sorgente viene compilato in assembly, con nomi derivati da un algoritmo hash. Gli assiemi risultanti vengono posizionati nella directory Bin. Qualsiasi codice inline, ovvero codice racchiuso ** % ** tra le ** &lt; ** parentesi quadre e , viene incluso nel markup e non compilato. Il compilatore crea nuovi file per contenere il markup con lo stesso nome dei file di origine. Questi file risultanti vengono inseriti nella directory Bin. Il compilatore crea anche file con lo stesso nome dei file di origine ma con estensione . COMPILED che contengono informazioni di mapping. Le. I file COMPILED vengono inseriti nelle directory di output corrispondenti alla posizione originale dei file di origine. |
| .ascx | Questi file sono suddivisi in markup e codice sorgente. Il codice sorgente viene compilato in assembly e inserito nella directory Bin, con nomi derivati da un algoritmo hash. Non viene generato alcun file di markup. |
| .cs, .vb, .jsl, .cpp (esclusi i file code-behind per i tipi di file elencati in precedenza) | Il codice sorgente a cui fanno riferimento gli assembly generati dai file ascx, ashx o aspx viene compilato in assembly e inserito nella directory Bin. Non viene copiato alcun file di origine. |
| Tipi di file personalizzati | Questi file vengono compilati come file dinamici. A seconda del tipo di file su cui sono basati, il compilatore può inserire i file di mapping nelle directory di output. |
| File nella\_sottodirectory Del codice dell'app | I file di codice sorgente in questa sottodirectory vengono compilati in assembly e inseriti nella directory Bin. |
| File nella\_sottodirectory App GlobalResources | Questi file vengono compilati in assembly e inseriti nella directory Bin. Nella\_directory di output principale non viene creata alcuna sottodirectory App GlobalResources. Se il file di configurazione specifica appliesTo"All", i file con estensione resx e resources vengono copiati nelle directory di output. Non vengono copiati se vi viene fatto riferimento da un [oggetto BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx). |
| File con estensione resx e\_risorsa nella sottodirectory App LocalResources | Questi file vengono compilati in assembly con nomi univoci e inseriti nella directory Bin. Nessun file con estensione resx o resource viene copiato nelle directory di output. |
| File .skin nella\_sottodirectory App Themes | I temi vengono compilati in assembly e inseriti nella directory Bin. I file Stub vengono creati per i file .skin e inseriti nella directory di output corrispondente. I file statici (ad esempio css) vengono copiati nelle directory di output. |
| .browser Web.config Tipi di file statici Assembly già presenti nella directory Bin | Questi file vengono copiati così come sono nella directory di output. |

### <a name="fixed-assembly-names"></a>[Nomi di assieme fissi](https://msdn.microsoft.com/library/ms229863.aspx##)

Alcuni scenari, ad esempio la distribuzione di un'applicazione Web utilizzando MSI Windows Installer, richiedono l'utilizzo di nomi di file e contenuti coerenti, nonché strutture di directory coerenti per identificare gli assembly o le impostazioni di configurazione per gli aggiornamenti. In questi casi, è possibile utilizzare l'opzione **-fixednames** per specificare che lo strumento di compilazione ASP.NET deve compilare un assembly per ogni file di origine anziché utilizzare il punto in cui più pagine vengono compilate in assembly. Questo può portare a un numero elevato di assembly, quindi se si è interessati alla scalabilità è necessario utilizzare questa opzione con cautela.

### <a name="strong-name-compilation"></a>[Compilazione con nome sicuroStrong-Name Compilation](https://msdn.microsoft.com/library/ms229863.aspx##)

Le opzioni **-aptca**, **-delaysign**, **-keycontainer** e **-keyfile** \_vengono fornite in modo che sia possibile utilizzare il compilatore Aspnet.exe per creare assembly con nome sicuro senza utilizzare separatamente [lo strumento Nome sicuro (Sn.exe).](https://msdn.microsoft.com/library/k5b5tt23.aspx) Queste opzioni corrispondono, rispettivamente, a **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**e **AssemblyKeyFileAttribute**.

La discussione di questi attributi non rientra nell'ambito di questo corso.

## <a name="labs"></a>Lab

Ognuno dei laboratori seguenti si basa sui lab precedenti. Avrete bisogno di fare in ordine.

## <a name="lab-1-using-the-configuration-api"></a>Laboratorio 1: Utilizzo dell'API di configurazione

1. Creare un nuovo sito Web denominato *mod9lab*.
2. Aggiungere un nuovo file di configurazione Web al sito.
3. Aggiungere quanto segue al file web.config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

In questo modo si garantisce l'autorizzazione per salvare le modifiche apportate al file web.config.

1. Aggiungere un nuovo controllo Label a Default.aspx e modificare l'ID in **lblDebugStatus**.
2. Aggiungere un nuovo controllo Button a Default.aspx.Add a new Button control to Default.aspx.
3. Modificare l'ID del controllo Button in **btnToggleDebug** e il testo per **attivare/disattivare lo stato**di debug .
4. Aprire la visualizzazione codice per il file code-behind di Default.aspx e aggiungere un'istruzione **using** per **System.Web.Configuration** come segue:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Aggiungere due variabili private alla\_classe e un metodo Page Init come illustrato di seguito:Add two private variables to the class and a Page Init method as shown below:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Aggiungere il codice\_seguente a Caricamento pagina:Add the following code to Page Load:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Salvare ed esplorare default.aspx. Si noti che il Label controllo Visualizza lo stato di debug corrente.
2. Fare doppio clic sul controllo Button nella finestra di progettazione e aggiungere il codice seguente all'evento Click per il controllo Button:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Salvare ed esplorare default.aspx e fare clic sul pulsante.
2. Aprire il file web.config dopo ogni clic del &lt;&gt; pulsante e osservare l'attributo **debug** nella sezione compilation.

## <a name="lab-2-logging-application-restarts"></a>Laboratorio 2: Registrazione dei riavvii dell'applicazione

In questa esercitazione verrà creato codice che consentirà di attivare o disattivare la registrazione degli arresti, degli avvii e delle ricompilazioni dell'applicazione nel Visualizzatore eventi.

1. Aggiungere un controllo DropDownList a default.aspx e modificare l'ID in ddlLogAppEvents.
2. Impostare la proprietà **AutoPostBack** per DropDownList su **true**.
3. Aggiungere tre elementi per il Items insieme per il DropDownList. Creare il **testo** per il primo elemento *Selezionare Valore* e il valore -1. Impostare il **testo** e il **valore** del secondo elemento **True** e il **testo** e il **valore** del terzo elemento **False**.
4. Aggiungere una nuova etichetta a default.aspx.Add a new Label to default.aspx. Modificare l'ID in **lblLogAppEvents**.
5. Aprire la visualizzazione code-behind per default.aspx e aggiungere una nuova dichiarazione per una variabile di tipo HealthMonitoringSection come illustrato di seguito:Open the code-behind view for default.aspx and add a new declaration for a variable of type HealthMonitoringSection as shown below:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Aggiungere il codice seguente al\_codice esistente in Page Init:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Fare doppio clic su DropDownList e aggiungere il codice seguente all'evento SelectedIndexChanged:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Esplorare default.aspx.
2. Impostare l'elenco a discesa **su False**.
3. Cancellare il registro applicazioni nel Visualizzatore eventi.
4. Fare clic sul pulsante per modificare l'attributo Debug per l'applicazione.
5. Aggiornare il registro applicazioni nel Visualizzatore eventi. 

    1. Sono stati registrati eventi?
    2. Perché o perché no?
6. Impostare l'elenco a discesa su **True.Set** the dropdown to True.
7. Fare clic sul pulsante per attivare o disattivare l'attributo Debug per l'applicazione.
8. Aggiornare l'account di accesso dell'applicazione nel Visualizzatore eventi. 

    1. Sono stati registrati eventi?
    2. Qual è stato il motivo dell'arresto dell'app?
9. Provare ad attivare e disattivare la registrazione ed esaminare le modifiche apportate al file web.config.

## <a name="more-information"></a>Altre informazioni:

ASP.NET modello di provider di 2.0 consente di creare provider personalizzati non solo per la strumentazione dell'applicazione, ma anche per molti altri usi, ad esempio appartenenza, profili e così via. Per informazioni dettagliate sulla scrittura di un provider personalizzato per registrare gli eventi dell'applicazione in un file di testo, visitare [questo collegamento](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
