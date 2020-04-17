---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET e Web Tools 2013.2 per le note sulla versione di Visual Studio 2013 Documenti Microsoft
author: rick-anderson
description: ''
ms.author: riande
ms.date: 03/06/2014
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 41b0f3ad43846bc5799ecd398188b0f6ffcc0d66
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543626"
---
# <a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>Note sulla versione di ASP.NET and Web Tools 2013.2 per Visual Studio 2013

da parte [di Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Note di installazione

ASP.NET e Web Tools per Visual Studio 2013.2 sono inclusi nel programma di installazione principale e possono essere scaricati come parte di [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Documentazione

Esercitazioni e altre informazioni su ASP.NET e Web Tools per Visual Studio 2013.2 sono disponibili sul [sito Web ASP.NET](https://www.asp.net/).

## <a name="software-requirements"></a>Requisiti software

ASP.NET e Web Tools per Visual Studio 2013.2 richiede Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Nuove funzionalità in ASP.NET e Web Tools per Visual Studio 2013.2

Nelle sezioni seguenti vengono descritte le funzionalità introdotte nella versione.

- [Modello di progetto ASP.NET un ASP.NET](#oneaspnet)
- [Supporto SSL all'avvio di applicazioni Web su IIS Express](#ssl)
- [Miglioramenti di Visual Studio Web Editor](#vswebeditor)
- [Browser Link](#browserlink)
- [Supporto per le app Web del servizio app di Azure in Visual StudioSupport for Azure App Service Web Apps in Visual Studio](#waws)
- [Creare risorse di Azure remote durante la creazione di un nuovo progetto WebCreate remote Azure resources when creating a new Web project](#AzureResources)
- [Miglioramenti della pubblicazione sul Web](#webpublish)
- [ASP.NET Scaffolding](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [Web Form ASP.NET](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.NET API Web 2.1.2](#webapi)
- [Pagine Web ASP.NET 3.1.2](#webpages)
- [Quadro di Entità 6.1](#ef)
- [Identità ASP.NET 2.0.0](#identity)
- [Componenti di Microsoft OWIN](#owin)
- [ASP.NET SignalR 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Modello di progetto ASP.NET un ASP.NET

- Aggiornamenti a ASP.NETi modelli di progetto per supportare la conferma dell'account e la reimpostazione della password.
- Aggiornare ASP.NET modello di API Web per supportare l'autenticazione utilizzando gli account dell'organizzazione locale.
- Il modello SPA ASP.NET ora contiene l'autenticazione basata su MVC e le visualizzazioni lato server. Il modello dispone di un controller WebAPI a cui possono accedere solo utenti autenticati.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Supporto SSL all'avvio di applicazioni Web su IIS Express

Per eliminare l'avviso di sicurezza durante l'esplorazione e il debug di HTTPS su localhost, è stata aggiunta una finestra di dialogo per consentire a Internet Explorer e Chrome di considerare attendibile il certificato SSL IIS express autofirmato.

Ad esempio, una proprietà del progetto Web può essere impostata per l'utilizzo di SSL. Fare clic su F4 per visualizzare la finestra di dialogo delle proprietà. Impostare **SSL abilitato su** true. Copiare l'URL SSL.

![Ssl Enabled Proprietà](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Impostare la scheda Web della pagina delle proprietà del progetto `https://localhost:44300/` Web per utilizzare l'URL basato su HTTPS (l'URL SSL sarà a meno che non siano stati creati siti Web SSL in precedenza).

![Imposta URL progetto (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Premere CTRL+F5 per eseguire l'applicazione. Seguire le istruzioni per considerare attendibile il certificato autofirmato generato da IIS Express.

![Avviso SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Leggere la finestra di dialogo **Avviso** di sicurezza e quindi fare clic su **Sì** se si desidera installare il certificato che rappresenta localhost.

![Avviso di sicurezza](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

Il sito verrà visualizzato in Internet IE o Chrome senza l'avviso del certificato nel browser.

![Pagina HTTPS senza avvisi](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox utilizza il proprio archivio certificati, in modo da visualizzare un avviso.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Miglioramenti di Visual Studio Web Editor

- **Nuovo elemento di progetto JSON e editor:** sono stati aggiunti un elemento di progetto JSON e un editor a Visual Studio.New JSON project item and editor : We have added a JSON project item and editor to Visual Studio. Le funzionalità dell'editor JSON corrente includono la colorazione, la convalida della sintassi, il completamento delle parentesi graffe, la struttura, l'impostazione delle opzioni degli strumenti e altro ancora.

    ![JSON Editor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense supporta ora [JSON Schema](http://json-schema.org/) v3 e v4. È disponibile una casella combinata dello schema per scegliere gli schemi esistenti, modificare il percorso dello schema locale o semplicemente trascinare un file JSON di progetto per ottenere il percorso relativo.

    ![JSON Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![Editor di schema JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Nuovo editor Sass (SCSS):** abbiamo aggiunto LESS in VS2013 RTM, e ora abbiamo un elemento di progetto Sass e un editor. Le funzionalità dell'editor Sass sono paragonabili all'editor LESS e includono colorazione, variabile e Mixins IntelliSense, commento/rimuovi commento, informazioni rapide, formattazione, convalida della sintassi, struttura, definizione goto, selezione colore, impostazione delle opzioni degli strumenti, ecc.

    ![Aggiungi nuovo elemento: foglio di stile SCSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Editor fogli di stile](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Nuovo selettore URL nei documenti HTML, Razor, CSS, LESS e Sass:** VS 2013 viene fornito senza selezione URL all'esterno delle pagine Web Form. Il nuovo selettore URL per gli editor HTML, Razor, CSS, LESS e Sass è un selettore di digitazione fluente e privo di finestre di dialogo che riconosce '.' e filtri elenchi di file in modo appropriato per i tag img e collegamenti.

    ![Selezione URL per il tag immagine](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![Selezione URL per le visualizzazioni](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Selezione URL per CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Aggiornamenti all'editor LESS aggiungendo altre funzionalità**
- **Knockout Intellisense Upgrade**: È stata aggiunta una sintassi KnockOut non standard per VS IntelliSense, sintassi "ko-vs-editor viewModel:". Può essere utilizzato per eseguire l'associazione a più modelli di visualizzazione in una pagina utilizzando i commenti nel modulo:

    ![Knockout Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    È stato inoltre aggiunto il supporto per ViewModel IntelliSense annidato, pertanto è possibile eseguire il drill-down di oggetti annidati nel ViewModel.We also added support for nested ViewModel IntelliSense, so you may drill into deeply nested objects on the ViewModel.

    `<div data-bind="text: foo.bar.baz.etc" />`

    IntelliSense visualizzato è l'IntelliSense completo dell'oggetto JavaScript.

    ![Intellisense che mostra l'oggetto JavaScript completo](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Nuovo selettore URL in HTML, Razor, CSS, LESS e Sass documenti**: VS 2013 fornito con nessunselettore URL al di fuori delle pagine Web Form. Il nuovo selettore URL per gli editor HTML, Razor, CSS, LESS e Sass è un selettore di digitazione fluente e privo di finestre di dialogo che riconosce '.' e filtri elenchi di file in modo appropriato per i tag img e collegamenti.

    ![Selezione URL per il tag immagine](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![Selezione URL per le visualizzazioni](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Selezione URL per CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Browser Link

- Browser Link ora supporta le connessioni HTTPS e lo elencherà in Dashboard con altre connessioni, purché il certificato sia considerato attendibile dal browser.
- Mapping di origine HTML statica
- Supporto SPA per la mappatura dei dati
- Aggiornamento automatico dei dati di mapping

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Supporto per le app Web del servizio app di Azure in Visual StudioSupport for Azure App Service Web Apps in Visual Studio

- **Supportare l'accesso ad Azure.Support Azure sign in.**
- **Debug remoto e Visualizzazione remota per le app Web:** è ora [supportato il debug remoto per le app Web nel servizio app di Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) e la visualizzazione remota dei file di contenuto delle app Web in Esplora server.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Creare risorse di Azure remote durante la creazione di un nuovo progetto WebCreate remote Azure resources when creating a new Web project

È stata aggiunta una casella di controllo "Crea risorse remote" di Azure nella finestra di dialogo della nuova applicazione Web.We added an Azure ["Create Remote Resources"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) checkbox on the new web application dialog. Scegliendolo, sarà possibile integrare l'esperienza di creazione di una nuova applicazione Web, configurare il sito di pubblicazione di Azure per il test e creare il profilo di pubblicazione in pochi semplici passaggi.

![Nuovo progetto con risorse di AzureNew Project with Azure resources](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Pubblicazione in Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Miglioramenti della pubblicazione sul Web

- Migliorare l'esperienza utente per la pubblicazione.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

- **Supporto enum:** Se il modello utilizza Enums, mvc Scaffolder genererà elenco a discesa per Enum. In questo modo vengono utilizzati gli helper Enum in MVC.
- **Supporto Bootstrap**: aggiornato l'EditorPer i modelli in SCAffolding MVC in modo che utilizzino le classi Bootstrap.
- **Supporto dei pacchetti:** le Shellffolder MVC e API Web aggiungeranno pacchetti 5.1 per MVC e API Web

Le schermate seguenti illustrano i modelli di scaffolding.

- Codice modello:

     ![Codice modello](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Compilare il codice del modello, fare clic con il pulsante destro del mouse e scegliere **Aggiungi**, **Nuovo elemento con scaffolding**.

     ![Aggiungi nuovo elemento scaffolding](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Scegliere **Controller MVC5 con visualizzazioni, usando Entity Framework:**

     ![Aggiungere un nuovo controller MVC5 con visualizzazioni](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Aggiungere Controller** usando il modello:Add Controller using the model:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Controllare il codice generato, ad esempio Views/WeekdayModels/Edit.cshtml contiene `@Html.EnumDropDownListFor`: ![visualizzazione contenente EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Eseguire la pagina per visualizzare la casella combinata enum generata, si noti che se un valore può essere null, è possibile scegliere una stringa vuota per la casella combinata. Ad esempio, la pagina **Crea** mostra quanto segue:

    ![Casella combinata che consente una stringa vuota](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 RTM sarà rilasciato nel mese di aprile 2014. Ecco i punti salienti delle note sulla versione, ma si prega di controllare [le note di rilascio complete](http://docs.nuget.org/docs/release-notes/nuget-2.8) per ulteriori informazioni su queste modifiche.

- **Applicazioni Windows Phone 8.1**di destinazione : NuGet 2.8.1 ora supporta le applicazioni Windows Phone 8.1 che utilizzano i moniker del framework di destinazione 'WindowsPhoneApp', 'WPA', 'WindowsPhoneApp81' e 'WPA81'.

- **Patch Resolution for Dependencies**: Durante la risoluzione delle dipendenze del pacchetto, NuGet ha storicamente implementato una strategia di selezione della versione del pacchetto principale e secondaria più bassa che soddisfa le dipendenze dal pacchetto. A differenza della versione principale e secondaria, tuttavia, la versione della patch è sempre stata risolta alla versione più alta. Anche se il comportamento era ben intenzionato, ha creato una mancanza di determinismo per l'installazione di pacchetti con dipendenze.
- **DependencyVersion Switch**: Anche se NuGet 2.8 modifica il comportamento *predefinito* per la risoluzione delle dipendenze, aggiunge anche un controllo più preciso sul processo di risoluzione delle dipendenze tramite l'opzione -DependencyVersion nella console di gestione dei pacchetti. L'opzione consente di risolvere le dipendenze alla versione più bassa possibile (comportamento predefinito), alla versione più alta possibile o alla versione secondaria o patch più alta. Questa opzione funziona solo per install-package nel comando powershell.
- **Attributo DependencyVersion**: Oltre all'opzione -DependencyVersion descritta in precedenza, NuGet ha anche consentito di impostare un nuovo attributo nel file nuget.config che definisce il valore predefinito, se l'opzione -DependencyVersion non è specificata in una chiamata di install-package. Questo valore verrà rispettato anche dalla finestra di dialogo NuGet Package Manager per tutte le operazioni di installazione del pacchetto. Per impostare questo valore, aggiungere l'attributo seguente al file nuget.config:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Anteprima NuGet Operations With -WhatIf**: Alcuni pacchetti NuGet possono avere grafici di dipendenza profonda, e come tale, può essere utile durante un'operazione di installazione, disinstallazione o aggiornamento per vedere prima cosa accadrà. NuGet 2.8 aggiunge lo standard PowerShell -what if switch to the install-package, uninstall-package, and update-package commands to enable visualizing the entire closure of packages to which the command will be applied.
- **Pacchetto di downgrade**: Non è raro installare una versione non definitiva di un pacchetto per esaminare le nuove funzionalità e quindi decidere di ripristinare l'ultima versione stabile. Prima di NuGet 2.8, questo era un processo in più passaggi di disinstallazione del pacchetto non definitivo e delle relative dipendenze e quindi l'installazione della versione precedente. Con NuGet 2.8, tuttavia, il pacchetto di aggiornamento eseguirà ora il rollback dell'intera chiusura del pacchetto (ad esempio l'albero delle dipendenze del pacchetto) alla versione precedente.
- **Dipendenze**di sviluppo : Molti tipi diversi di funzionalità possono essere forniti come pacchetti NuGet, inclusi gli strumenti utilizzati per ottimizzare il processo di sviluppo. Questi componenti, sebbene possano essere fondamentali per lo sviluppo di un nuovo pacchetto, non devono essere considerati una dipendenza del nuovo pacchetto quando viene pubblicato in un secondo momento. NuGet 2.8 consente a un pacchetto di identificarsi nel file .nuspec come developmentDependency. Una volta installati, questi metadati verranno aggiunti anche al file packages.config del progetto in cui è stato installato il pacchetto. Quando il file packages.config viene successivamente analizzato per le dipendenze NuGet durante il pacchetto nuget.exe, escluderà le dipendenze contrassegnate come dipendenze di sviluppo.
- **Singoli file packages.config per piattaforme diverse:** quando si sviluppano applicazioni per più piattaforme di destinazione, è comune avere file di progetto diversi per ognuno dei rispettivi ambienti di compilazione. È anche comune utilizzare pacchetti NuGet diversi in file di progetto diversi, poiché i pacchetti hanno diversi livelli di supporto per piattaforme diverse. NuGet 2.8 fornisce un supporto migliorato per questo scenario creando file packages.config diversi per diversi file di progetto specifici della piattaforma.
- **Fallback alla cache locale:** anche se i pacchetti NuGet vengono in genere utilizzati da una raccolta remota, ad esempio la [raccolta NuGet](http://www.nuget.org) usando una connessione di rete, esistono molti scenari in cui il client non è connesso. Senza una connessione di rete, il client NuGet non è stato in grado di installare correttamente i pacchetti, anche quando tali pacchetti erano già nel computer del client nella cache NuGet locale. NuGet 2.8 aggiunge il fallback automatico della cache alla console di gestione dei pacchetti.

    La funzionalità di fallback della cache non richiede argomenti di comando specifici. Inoltre, il fallback della cache attualmente funziona solo nella console di gestione dei pacchetti: il comportamento non funziona attualmente nella finestra di dialogo di gestione dei pacchetti.
- **Correzioni**di bug : Una delle principali correzioni di bug effettuate è stato il miglioramento delle prestazioni nel comando update-package -reinstall.

    Oltre a queste funzionalità e alla correzione delle prestazioni di cui sopra, questa versione di NuGet include anche molte altre correzioni di bug. Ci sono stati 181 problemi totali risolti nella versione. Per un elenco completo degli elementi di lavoro corretti in NuGet 2.8, vedere [NuGet Issue Tracker](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) per questa versione.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>Web Form ASP.NET

- I modelli Web Form ora mostrano come eseguire la conferma dell'account e la reimpostazione della password per ASP.NETidentità.
- Il controllo Origine dati entità e il provider Dynamic Data per Entity Framework 6. Per ulteriori informazioni, vedere il blog MSDN seguente: [provider Dynamic Data e controllo EntityDataSource per Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Miglioramenti al routing degli attributi](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Supporto Bootstrap per i modelli di editor](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Supporto dell'enumerazione nelle visualizzazioni](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Supporto discreto per gli attributi MinLength/ MaxLength](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Sostenere il contesto 'this' in Unobtrusive Ajax](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Varie [correzioni di bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET API Web 2.1.2

- [Gestione globale degli errori](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Miglioramenti del routing degli attributi](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Miglioramenti alla pagina della Guida](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Supporto di IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Formattatore di tipo multimediale BSON](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Migliore supporto per i filtri asincroni](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Analisi delle query per la libreria di formattazione clientQuery parsing for the client formatting library](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Varie [correzioni di bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>Pagine Web ASP.NET 3.1.2

- Varie [correzioni di bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Quadro di Entità 6.1

Entity Framework è stato aggiornato alla versione 6.1 sia per il runtime che per gli strumenti. Entity Framework (EF) 6.1 è un aggiornamento secondario di Entity Framework 6 e include una serie di correzioni di bug e nuove funzionalità. Per informazioni dettagliate su EF6.1, inclusi i collegamenti alla documentazione per le nuove funzionalità, vedere [cronologia delle versioni](https://msdn.microsoft.com/data/jj574253)di Entity Framework . Le nuove funzionalità di questa versione includono:The new features in this release include:

- **Consolidamento di strumenti** fornisce un modo coerente per creare un nuovo modello di EF. Questa funzionalità estende la procedura guidata ADO.NET Entity Data Model per supportare la creazione di modelli Code First, incluso il reverse engineering da un database esistente. Queste funzionalità erano precedentemente disponibili nella qualità Beta in EF Power Tools.
- **La gestione degli errori** di commit delle transazioni fornisce il nuovo [System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) che utilizza la nuova capacità introdotta di intercettare le operazioni di transazione. **Il CommitFailureHandler** consente il ripristino automatico da errori di connessione durante il commit di una transazione.
- **IndexAttribute** consente di specificare gli indici inserendo un attributo in una proprietà (o proprietà) nel modello Code First. Code First creerà quindi un indice corrispondente nel database.
- **L'API di mapping pubblico** fornisce l'accesso alle informazioni di EF su come le proprietà e i tipi vengono mappati a colonne e tabelle nel database. Nelle versioni precedenti questa API era interna.
- **Possibilità di configurare gli intercettori tramite il file App/Web.config**(consentendo l'aggiunta di intercettori senza ricompilare l'applicazione).
- **DatabaseLogger** è un nuovo intercettore che semplifica il log di tutte le operazioni di database in un file. In combinazione con la funzionalità precedente, questo consente di attivare facilmente la registrazione delle operazioni di database per un'applicazione distribuita, senza la necessità di ricompilare.
- **Il rilevamento delle modifiche** del modello di migrazione è stato migliorato in modo che le migrazioni con scaffolding siano più accurate. anche le prestazioni del processo di rilevamento delle modifiche sono state notevolmente migliorate.
- **Miglioramenti delle prestazioni,** incluse operazioni di database ridotte durante l'inizializzazione, ottimizzazioni per il confronto di uguaglianza null nelle query LINQ, generazione di visualizzazioni più rapida (creazione di modelli) in più scenari e materializzazione più efficiente delle entità rilevate con più associazioni.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>Identità ASP.NET 2.0.0

- **Autenticazione**a due fattori: ASP.NET Identity ora supporta l'autenticazione a due fattori. L'autenticazione a due fattori fornisce un ulteriore livello di sicurezza agli account utente nel caso in cui la password venga compromessa. C'è anche la protezione per gli attacchi di forza bruta contro i codici a due fattori.
- **Blocco account:** Fornisce un modo per bloccare l'utente se l'utente immette la password o i codici a due fattori in modo non corretto. È possibile configurare il numero di tentativi non validi e l'intervallo di tempo per gli utenti. Uno sviluppatore può facoltativamente disattivare il blocco dell'account per determinati account utente in caso di necessità.
- **Conferma account:** Il sistema di ASP.NET Identity supporta ora la conferma dell'account. Questo è uno scenario abbastanza comune nella maggior parte dei siti web oggi in cui, quando ti registri per un nuovo account sul sito web, devi confermare la tua email prima di poter fare qualsiasi cosa nel sito web. La conferma e-mail è utile perché impedisce la creazione di account fasulli. Ciò è estremamente utile se si utilizza la posta elettronica come metodo di comunicazione con gli utenti del sito Web, ad esempio siti di forum, servizi bancari, e-commerce o siti Web di social networking.
- **Reimpostazione password:** Reimpostazione password è una funzione in cui l'utente può reimpostare le password se ha dimenticato la password.
- **Timbro di sicurezza (Esci ovunque):** Supporta un modo per rigenerare il token di sicurezza per l'utente nei casi in cui l'Utente modifica la password o qualsiasi altra informazione relativa alla sicurezza, ad esempio la rimozione di un account di accesso associato (ad esempio Facebook, Google, Microsoft Account e così via). Ciò è necessario per garantire che tutti i token generati con la vecchia password vengano invalidati. Nel progetto di esempio, se si modifica la password dell'utente, viene generato un nuovo token per l'utente e tutti i token precedenti vengono invalidati. Questa funzionalità fornisce un ulteriore livello di sicurezza per l'applicazione poiché quando si modifica la password, si verrà disconnessi da qualsiasi luogo (tutti gli altri browser) in cui è stato effettuato l'accesso a questa applicazione.
- **Rendere il tipo di chiave primaria estensibile per Utenti e ruoli:** in ASP.NET Identity 1.0, il tipo di chiave primaria per la tabella Utenti e ruoli era costituito da stringhe. Ciò significa che quando il sistema di identità ASP.NET è stato reso persistente in SQL Server utilizzando Entity Framework, si usa nvarchar. Ci sono state molte discussioni intorno a questa implementazione predefinita su Overflow dello Stack e in base al feedback in arrivo. È stato fornito un hook di estendibilità in cui è possibile specificare quale deve essere la chiave primaria della tabella Utenti e ruoli. Questo hook di estensibilità è particolarmente utile se si esegue la migrazione dell'applicazione e l'applicazione archiviava gli ID utente sono GUID o int.
- **Support IQueryable on Users and Roles**: Aggiunto il supporto per IQueryable su UsersStore e RolesStore, è possibile ottenere facilmente l'elenco di utenti e ruoli.
- **Supportare l'operazione di eliminazione tramite UserManagerSupport Delete operation through the UserManager**
- **Indicizzazione su UserName:** in ASP.NET'implementazione di Identity Entity Framework, è stato aggiunto un indice univoco nel nome utente utilizzando il nuovo IndexAttribute in Entity Framework 6.1.0. Questo assicura che i nomi utente siano sempre univoci e non vi sia alcuna condizione di gara in cui si potrebbe finire con nomi utente duplicati.
- **Convalida password avanzata:** Il validatore della password fornito in ASP.NET Identity 1.0 era un validatore di password abbastanza semplice che convalidava solo la lunghezza minima. C'è un nuovo validatore di password che ti dà un maggiore controllo sulla complessità della password. Tieni presente che anche se attivi tutte le impostazioni di questa password, ti invitiamo a abilitare l'autenticazione a due fattori per gli account utente.
- **Middleware IdentityFactory/ CreatePerOwinContext**:

    - **User Manager**: È possibile utilizzare l'implementazione Factory per ottenere un'istanza di UserManager dal contesto OWIN. Questo modello è simile a quello usato per ottenere AuthenticationManager dal contesto OWIN per SignIn e SignOut.This pattern is similar to what we use for getting AuthenticationManager from OWIN context for SignIn and SignOut. Si tratta di un modo consigliato per ottenere un'istanza di UserManager per ogni richiesta per l'applicazione.
    - **DbContextFactory:** ASP.NET Identity utilizza Entity Framework per rendere persistente il sistema di identità in SQL Server. To do this the Identity System has a reference to the ApplicationDbContext. Il Middleware DbContextFactory restituisce un'istanza di ApplicationDbContext per ogni richiesta che è possibile utilizzare nell'applicazione.
- **ASP.NET pacchetto Identity Samples NuGet**: il pacchetto Samples NuGet può semplificare l'installazione e l'esecuzione di esempi per ASP.NET'identità e seguire le procedure consigliate. Si tratta di un esempio ASP.NET applicazione MVC. Modificare il codice in base all'applicazione prima di distribuirlo nell'ambiente di produzione. L'esempio deve essere installato in un'applicazione di ASP.NET vuota. Per ulteriori informazioni sul pacchetto, visitare il seguente post di blog: [Annuncio RTM di ASP.NET identità 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Componenti Microsoft OWIN

Ci sono stati molti bug che sono stati corretti in questa versione. Per informazioni più dettagliate, vedere [le note sulla versione per la versione 2.1.0.](https://katanaproject.codeplex.com/releases/view/113281)

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

Ci sono stati molti bug che sono stati corretti in questa versione. Per informazioni più dettagliate, vedere [le note sulla versione per la versione 2.0.2.](https://github.com/SignalR/SignalR/releases/tag/2.0.2)
