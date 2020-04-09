---
uid: whitepapers/aspnet-data-access-content-map
title: Accesso ai dati ASP.NET - Risorse consigliate Documenti Microsoft
author: rick-anderson
description: In questo argomento vengono forniti collegamenti a risorse di documentazione su come accedere ai dati in ASP.NET applicazioni Web, principalmente tramite Entity Framework e SQL Se...
ms.author: riande
ms.date: 07/25/2013
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 357851f195bf233c7c34a32bd156e4408d3e1b24
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676167"
---
# <a name="aspnet-data-access---recommended-resources"></a>Accesso ai dati ASP.NET - Risorse consigliate

> In questo argomento vengono forniti collegamenti a risorse di documentazione su come accedere ai dati in ASP.NET applicazioni Web, principalmente utilizzando Entity Framework e SQL Server.
> 
> Se conosci un grande post di blog, un thread [stackoverflow](http://stackoverflow.com) o qualsiasi altro link che sarebbe utile, [inviaci un'email](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) con il link.
> 
> Ultimo aggiornamento 03/04/2014

Di seguito sono elencate le diverse sezioni di questo argomento:

- [Introduzione all'accesso ai dati in ASP.NET](#gettingstarted)
- [Utilizzo di Entity Framework](#ef)

    - [Utilizzo prima del codice Entity FrameworkUsing Entity Framework Code First](#cf)
    - [Utilizzo delle migrazioni di Entity Framework Code FirstUsing Entity Framework Code First Migrations](#efcfmigrations)
    - [Utilizzo di Entity Framework Database first o modello first (progettazione Entity Framework)Using Entity Framework Database First or Model First (the EF Designer)](#efdbf)
    - [Caricamento di dati correlati in Entity Framework (caricamento lazy, caricamento eager e caricamento esplicito)](#efrelateddata)
    - [Ottimizzazione delle prestazioni di Entity FrameworkOptimizing Entity Framework Performance](#optimizingef)
    - [Gestione della concorrenza in un'applicazione Entity FrameworkHandling Concurrency in an Entity Framework Application](#efconcurrency)
    - [Libri su Entity Framework](#efbooks)
    - [Risorse aggiuntive di Entity Framework](#otherefresources)
- [Associazione dati in applicazioni Web Form ASP.NET](#wfdatabinding)

    - [Utilizzo dell'associazione di modelli Web FormUsing Web Forms Model Binding](#wfmodelbinding)
    - [Utilizzo dei controlli origine dati Web Form](#wfdsc)
    - [Utilizzo di controlli con associazione a dati Web Form ed espressioni di associazione dati](#wfdbc)
- [Utilizzo dei database di SQL Server](#sqlserver)

    - [Utilizzo dei database LocalDB di SQL Server Express](#sslocaldb)
    - [Utilizzo dei database di SQL Server Express](#sse)
    - [Utilizzo del database SQL di Windows AzureWorking with Windows Azure SQL Database](#ssdb)
    - [Scelta tra SQL Server e il database SQL di Windows AzureChoosing between SQL Server and Windows Azure SQL Database](#ssdbchoosing)
- [Utilizzo di NoSQL Database Management Systems](#nosql)
- [Utilizzo di query LINQ in applicazioni ASP.NET](#linq)
- [Utilizzo dello scaffolding di Dynamic DataUsing Dynamic Data scaffolding](#dd)
- [Protezione dell'accesso ai dati](#securing)
- [Ottimizzazione delle prestazioni di accesso ai datiOptimizing Data Access Performance](#optimizingdataaccess)
- [Distribuzione di un databaseDeploying a Database](#deploying)
- [Accesso ai dati tramite un servizio WebAccessing Data through a Web Service](#webservice)
- [Risorse aggiuntive](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Introduzione all'accesso ai dati in ASP.NET

- [Opzioni di archiviazione dati (Creazione di app cloud reali con Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Capitolo di un e-book sullo sviluppo per il cloud. Introduce i database NoSQL come alternativa che molti sviluppatori che hanno familiarità con i database relazionali tendono a trascurare. Presenta linee guida su cosa pensare quando si sceglie relazionale o NoSQL, o la scelta di una particolare piattaforma.
- [ASP.NET Opzioni di accesso ai dati](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Introduzione alle opzioni di accesso ai dati per i database relazionali per ASP.NET e indicazioni su come scegliere le piattaforme e i metodi di accesso appropriati per lo scenario.
- [Database relazionale](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). Se non si lavora con database relazionali, vedere questa pagina per un'introduzione alla terminologia e ai concetti relativi ai database relazionali. Per un'introduzione a SQL Server in particolare, vedere [Utilizzo dei database](#sqlserver) di SQL Server più avanti in questo argomento.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Utilizzo di Entity Framework

- [Approcci](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) allo sviluppo di Entity Framework (MSDN). Indicazioni su come scegliere un approccio di sviluppo di Entity Framework Database First, Model First o Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Utilizzo prima del codice Entity FrameworkUsing Entity Framework Code First

Le esercitazioni seguenti offrono applicazioni di esempio scaricabili:The following tutorials offer downloadable sample applications:

- [Introduzione a EF 6 con MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Copre un'ampia gamma di scenari di Entity Framework Code First, incluse le funzionalità di migrazione e Entity Framework 6, ad esempio resilienza della connessione, intercettazione di comandi e async. Si tratta di una versione aggiornata della [serie EF 5 / MVC 4](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). La serie precedente include un'esercitazione sul repository e modelli di unità di lavoro che non è incluso nella nuova serie.
- [Introduzione a ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Copre una gamma più ristretta di scenari di Entity Framework Code First, ma esegue un processo più completo di introduzione delle funzionalità MVC.
- [Associazione di](https://go.microsoft.com/fwlink/?LinkId=286117)modelli e Web Form . Utilizza Code First in un'applicazione Web Form.
- [Introduzione a i Web Form ASP.NET 4.5](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Introduzione ai Web Form con una certa copertura di Code First. Utilizza l'associazione di modelli.
- [MVC Music Store](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Utilizza Code First in un'applicazione MVC 3 di e-commerce che implementa anche l'appartenenza e l'autorizzazione. La versione MVC e ASP.NET l'appartenenza (autenticazione e autorizzazione) sistema utilizzato qui sono obsoleti; per ulteriori informazioni aggiornate sull'appartenenza [https://asp.net/identity](https://asp.net/identity)a ASP.NET, vedere .

Altre risorse:

- [Entity Framework - Code First to an Existing Database](https://msdn.microsoft.com/data/jj200620). Msdn. Video e procedura dettagliata che illustra come usare Code First con un database esistente.
- [Centro per sviluppatori di dati - Entity Framework](https://msdn.microsoft.com/data/ef). Msdn. Per una guida alla documentazione di Entity Framework creata e gestita dal team di Entity Framework, vedere il collegamento [Guida introduttiva.](https://msdn.microsoft.com/data/ee712907)

Vedere anche [Libri su Entity Framework](#efbooks) e risorse aggiuntive di Entity [Framework](#otherefresources) più avanti in questo argomento.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Utilizzo delle migrazioni di Entity Framework Code FirstUsing Entity Framework Code First Migrations

La maggior parte delle esercitazioni di Code First elencate in precedenza riguardano le migrazioni. Vedere anche le risorse seguenti.

- [Distribuzione di Web ASP.NET con Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Serie di esercitazioni in due parti che illustra come usare migrazioni Code First per distribuire un database.
- [Distribuire un'app Secure ASP.NET MVC 5 con appartenenza, OAuth e database SQL in un sito Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)di Windows Azure. Microsoft Azure). Come usare le migrazioni per distribuire i dati di appartenenza e applicazione in Azure.How to use migrations to deploy membership and application data to Azure.
- [Panoramica della distribuzione Web per Visual Studio e ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Vedere la sezione Configurazione della distribuzione di database in Visual Studio per una spiegazione del modo in cui le migrazioni Code First è integrato nelle funzionalità di distribuzione Web di Visual Studio.See the **Configuring Database Deployment in Visual Studio** section for an explanation of how Code First Migrations is integrated into Visual Studio web deployment features.
- [Centro per sviluppatori di dati -](https://msdn.microsoft.com/data/jj591621) Code First Migrations (MSDN). Documentazione sulle migrazioni del team di Entity Framework.
- [Serie di screencast migrazioni](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). EF). Tre video su argomenti avanzati in Code First Migrations.
- [Migrazioni code First con ASP.NET siti](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites)di pagine Web . Mikesdotnetting blog). Viene illustrato come utilizzare le migrazioni Code First con un sito di ASP.NET pagine Web inserendo il contesto dati in un progetto di libreria di classi di Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Utilizzo di Entity Framework Database first o modello first (progettazione Entity Framework)Using Entity Framework Database First or Model First (the EF Designer)

- [Introduzione al database di Entity Framework 6 utilizzando MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Eseguire uno script in Esplora server per creare un database e quindi usare Entity Framework Designer per creare il modello di dati. Viene illustrato come creare semplici pagine Web CRUD e per altre funzioni di gestione dei dati è possibile seguire una delle esercitazioni Code First poiché tutti i flussi di lavoro di EF usano la stessa API DbContext.Shows how to create simple CRUD web pages, and for other data handling functions you can follow one of the Code First tutorials since all EF workflows use the same DbContext API.

Le risorse seguenti sono meno recenti. Sono utili se si desidera utilizzare la versione 4.0 di Entity Framework e si desidera utilizzare un controllo origine dati per l'associazione dati in un'applicazione Web Form.

- [Introduzione a Entity Framework 4.0.](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) Viene illustrato come utilizzare il controllo **EntityDataSource.**
- [Continuare con Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(viene illustrato come utilizzare il controllo **ObjectDataSource.** Include un'esercitazione sulla gestione della concorrenza, un'esercitazione sulle prestazioni di EF e un'esercitazione sulle novità di EF 4.0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Gestione dei dati correlati in Entity Framework (caricamento lazy, caricamento eager e caricamento esplicito)

- [Lettura di dati correlati con Entity Framework in un'applicazione MVC ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, applicazione di esempio MVC. I metodi illustrati si applicano anche all'associazione di modelli Web Form e al flusso di lavoro Database First.
- [Centro per sviluppatori di dati - Caricamento di entità correlate](https://msdn.microsoft.com/data/jj574232) (MSDN). Documentazione del team di Entity Framework sul caricamento dei dati correlati.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Ottimizzazione delle prestazioni di Entity FrameworkOptimizing Entity Framework performance

- [Scenari avanzati di Entity Framework per un'applicazione ASP.NET.](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md) Viene illustrato come eseguire istruzioni SQL personalizzate o chiamare stored procedure personalizzate, come disabilitare il rilevamento delle modifiche e come disabilitare la convalida quando si salvano le modifiche.
- [Considerazioni sulle prestazioni per Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Considerazioni sulle prestazioni (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Ottimizzazione delle prestazioni con Entity Framework in un'applicazione Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Si applica a Entity Framework 4.0.
- Vedere anche [Ottimizzazione dell'accesso ai dati ASP.NET](#optimizingdataaccess) più avanti in questo argomento.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Gestione della concorrenza in un'applicazione Entity FrameworkHandling Concurrency in an Entity Framework Application

- [Gestione della concorrenza con Entity Framework in un'applicazione MVC ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, DbContext API, utilizzando un'applicazione di esempio MVC.
- [Data Developer Center – Modelli](https://msdn.microsoft.com/data/jj592904) di concorrenza ottimistica (MSDN). Documentazione sulla concorrenza del team di Entity Framework.
- [Gestione della concorrenza con Entity Framework in un'applicazione Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Si applica a Entity Framework 4.0. Database First, ObjectContext API, utilizzando un'applicazione di esempio Web Form.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Libri su Entity Framework

- [Programmazione di Entity Framework: DbContext](http://shop.oreilly.com/product/0636920022237.do) di Julie Lerman e Rowan Miller.
- [Programmazione Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) di Julie Lerman e Rowan Miller.

Entrambi questi libri sono aggiornati con le attuali tecniche consigliate. Forniscono un'introduzione più completa ma facile da seguire di Entity Framework rispetto a qualsiasi altro elemento disponibile su Internet. Un altro libro, [Programmazione Entity Framework](http://shop.oreilly.com/product/9780596807252.do) di Julie Lerman, è più grande e più completo, ma è più vecchio e molte delle tecniche che copre non sono più il modo consigliato per utilizzare Entity Framework. Vedere anche l'elenco dei libri consigliati dal team di Entity Framework nel [Data Developer Center - Libri](https://msdn.microsoft.com/data/aa937716) sul sito MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Altre risorse di Entity Framework

- Blog del team di [Entity Framework (ADO.NET).](https://blogs.msdn.com/b/adonet/) Una delle migliori risorse per le informazioni più aggiornate e gli annunci di nuovi miglioramenti. Per altri blog correlati a Entity Framework, vedere blogroll in Introduzione a [Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). Vedere la colonna **Punti dati,** che spesso riguarda gli argomenti correlati a Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Associazione dati in applicazioni Web Form ASP.NET

- ASP.NET Opzioni di [accesso ai dati di Web Form](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Utilizzo dell'associazione di modelli Web FormUsing Web Forms Model Binding

- [Associazione di](https://go.microsoft.com/fwlink/?LinkId=286117)modelli e Web Form . Serie di esercitazioni che usano EF Code First.Tutorial series using EF Code First.
- [Associazione modello Web Form Parte 1: Selezione dei dati](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (blog di Scott Guthrie). In questi post di blog meno recenti, la proprietà attualmente denominata ItemType è denominata ModelType, ma in caso contrario le informazioni che contengono sono valide.
- [Associazione modello Web Form Parte 2: Filtro dei dati](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (blog di Scott Guthrie).
- [Associazione di modelli Web Form Parte 3: Aggiornamento e convalida](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (blog di Scott Guthrie).
- [ASP.NET associazione di modelli Web Form 4.5](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (video).
- [Associazione modello Parte 1 - Selezione dei dati](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (video).
- [Associazione di modelli Parte 2 - Filtraggio](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (video).
- [Introduzione aASP.NET Web Form 4.5 - Visualizzazione](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md)di elementi di dati e dettagli .

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Utilizzo dei controlli origine dati Web Form

- [Controlli del server Web di origine](https://msdn.microsoft.com/library/ms247258.aspx) dati (MSDN).
- [Annuncio del rilascio del provider Dynamic Data e del controllo EntityDataSource per Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (blog di Microsoft Web Development).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Utilizzo di controlli con associazione a dati Web Form ed espressioni di associazione dati

- [Associazione di](https://go.microsoft.com/fwlink/?LinkId=286117)modelli e Web Form . Serie di esercitazioni che usa EF Code First.Tutorial series that uses EF Code First.
- [Introduzione aASP.NET Web Form 4.5 - Visualizzazione](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md)di elementi di dati e dettagli .
- [Controlli dei dati fortemente tipati](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (blog di Scott Guthrie).
- [Controlli dati fortemente tiporati](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [ASP.NET 4.5 Web Form Controlli dati tipati](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) forte (video).
- [Controlli server Web associati a dati](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Cenni preliminari](https://msdn.microsoft.com/library/ms178366.aspx) sulle espressioni di associazione dati (MSDN). Questa pagina copre solo **Eval** e **Bind**; non è stato aggiornato per includere **Item** e **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Utilizzo dei database di SQL Server

- [Funzionalità del database](https://msdn.microsoft.com/library/hh230827.aspx) di SQL Server (MSDN). Per un'introduzione generale a un'ampia gamma di argomenti di SQL Server, vedere le voci in questo nel sommario.
- [SQL Server Editions](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Un riepilogo delle edizioni di SQL Server disponibili, con collegamenti a ulteriori informazioni su ognuna di esse.)
- [Stringhe di connessione di SQL Server per ASP.NET applicazioni Web](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Utilizzo di SQL Server Compact per ASP.NET applicazioni Web](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server: esempi](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md)di prodotti di database . Esempio di database AdventureWorks.
- [Installazione di database di esempio](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Oltre ai metodi illustrati di seguito, è anche possibile scaricare uno\_dei file con estensione mdf di esempio nella cartella App Data di un progetto Web, convertire il database in LocalDB e creare una stringa di connessione LocalDB. Per informazioni su come eseguire questa operazione, vedere [procedura: eseguire l'aggiornamento a LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Vedere anche le sezioni seguenti sull'utilizzo di SQL Server Express e LocalDB e sulla scelta tra SQL Server e il database SQL.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Utilizzo dei database LocalDB di SQL Server Express

- [SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). Introduzione ufficiale di MSDN a LocalDB.
- [Stringhe di connessione di SQL Server per ASP.NET applicazioni Web](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Procedura: eseguire l'aggiornamento a LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Come eseguire la migrazione di un file con estensione mdf da una versione precedente di SQL Server Express a LocalDB. È inoltre necessario eseguire questo processo se si scarica uno dei database di esempio di [SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Introduzione a LocalDB, un microsoft SQL Express migliorato](https://go.microsoft.com/fwlink/?LinkId=234375) (blog di SQL Server Express). Ha più informazioni di base sul motivo per cui LocalDB è stato creato che è incluso in MSDN.
- [LocalDB: Dove si trova il database?](https://go.microsoft.com/fwlink/?LinkId=234376) (blog di SQL Server Express). Informazioni sulla posizione in cui vengono creati i file di database del database del database LocalDB.
- [Utilizzo di LocalDB con IIS completo, parte 1: profilo utente](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (blog di SQL Server Express). LocalDB non è progettato per funzionare con IIS. Questa serie di post del blog spiega i problemi e alcune soluzioni alternative.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Utilizzo dei database di SQL Server Express

- [Stringhe di connessione di SQL Server per ASP.NET applicazioni Web](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Se si utilizza l'impostazione della stringa di connessione AttachDBFileName con SQL Server Express, vedere in particolare la sezione Istanza utente di questa pagina.
- [Come assumere la proprietà del locale SQL Server Express 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (blog di SQL Server Express). Un problema comune è la possibilità di utilizzare i database di SQL Server Express perché non si è un amministratore dell'istanza di SQL Server Express. Per impostazione predefinita, solo la persona che ha installato SQL Server Express è un amministratore. In questo blog viene illustrato come essere un amministratore di SQL Server Express se si è un amministratore del computer.
- [L'applicazione Web ASP.NET è in grado di utilizzare un database SQL Server Express nell'ambiente di produzione?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Utilizzo del database SQL di Windows AzureWorking with Windows Azure SQL Database

- [Distribuire un'app Secure ASP.NET MVC con appartenenza, OAuth e database SQL in un sito Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) di Windows Azure (sito di Microsoft Azure).
- [Database SQL](https://docs.microsoft.com/azure/sql-database/) (sito di Microsoft Azure). Esercitazioni introduttive e guide pratiche.
- [Database SQL](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) di Windows Azure (MSDN). Nodo di primo livello del sommario per il database SQL in MSDN.
- [Indice degli articoli Wiki TechNet del database SQL di Windows Azure](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (sito Microsoft TechNet).
- [Blocco di applicazioni per la gestione degli errori temporanei](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx) Framework che consente di gestire errori di rete temporanei ed errori di connessione derivanti dalla limitazione delle richieste. Disponibile in un pacchetto NuGet: [Enterprise Library 5.0 - Transient Fault Handling Application Block](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Introduzione al database SQL e a Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Windows Azure Training Kit](https://www.microsoft.com/download/details.aspx?id=8396) (Area download Microsoft). Include laboratori pratici per il database SQL.
- [Forum della community di database SQL di Windows Azure](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Passaggio al database SQL](https://msdn.microsoft.com/library/ff803375.aspx) di Windows Azure (MSDN). Un capitolo di uno scenario end-to-end completo del team Microsoft Patterns and Practices. Viene illustrato il motivo per cui è consigliabile eseguire la migrazione e come eseguire la migrazione da SQL ServerSQL Server al database SQL.
- [Migrazione di database di SQL Server al database SQL](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) di Windows Azure (MSDN).
- [Migrazione guidata database SQL](http://sqlazuremw.codeplex.com/). Strumento open source per la migrazione di database da e verso il database SQL.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Scelta tra SQL Server e il database SQL di Windows AzureChoosing between SQL Server and Windows Azure SQL Database

- [Confrontare SQL Server con il database SQL di Windows Azure](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (sito Microsoft TechNet).
- [Migrazione dei dati al database SQL](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) di Windows Azure: strumenti e tecniche (MSDN). Include sezioni che confrontano SQL Serversql Server con il database SQL e forniscono indicazioni su quando eseguire la migrazione da SQL ServerSQL Server al database SQL.
- [Guida al recapito di database SQL](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) di Windows Azure (sito Microsoft TechNet).
- [Limitazioni delle funzionalità](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) di SQL Server (database SQL di Windows Azure) (MSDN).
- [Archiviazione tabelle](https://msdn.microsoft.com/library/jj553018.aspx) di Windows Azure e Database SQL di Windows Azure - Confronto e contrasto (MSDN). Per un'applicazione distribuita in Windows Azure, Archiviazione tabelle di Windows Azure potrebbe essere un'alternativa al database SQL di Windows Azure.For an application that you deploy to Windows Azure, Windows Azure Table storage might be an alternative to Windows Azure SQL Database. Questo argomento consente di decidere tra queste alternative.
- [Database SQL](https://msdn.microsoft.com/library/windowsazure/ee336279) di Windows Azure (MSDN).
- [Linee guida e limitazioni (database SQL di Windows Azure)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Utilizzo di NoSQL Database Management Systems

- [Windows Azure Data Services](https://www.windowsazure.com/develop/net/data/) (sito di Microsoft Azure). Vedere la [guida alle funzionalità di Servizio tabelle](https://docs.microsoft.com/azure/) e la sezione Big **Data** della pagina.
- [ASP.NET applicazione a più livelli tramite tabelle di archiviazione, code e BLOB](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (sito di Microsoft Azure). Esercitazione end-to-end con applicazione di esempio scaricabile che usa le tabelle NoSQL di archiviazione di Windows Azure.End-to-end tutorial with downloadable sample application that uses Windows Azure storage NoSQL tables.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Utilizzo di query LINQ in applicazioni ASP.NET

- [ASP.NET Opzioni di accesso ai dati](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Include un'introduzione a LINQ.
- [Video](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) di formazione LINQ (blog di Joe Stagner).
- [ASP.NET thread del Forum con collegamenti a risorse LINQ dinamiche.](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq)

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Utilizzo dello scaffolding di Dynamic DataUsing Dynamic Data Scaffolding

- [Modelli](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) di progetto Dynamic Data (MSDN). Indicazioni su quando utilizzare i progetti Dynamic Data.Guidance on when to use Dynamic Data projects.
- [ASP.NET Dynamic Data](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Protezione dell'accesso ai dati

- [Protezione dell'accesso ai dati in ASP.NET](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Considerazioni sulla sicurezza (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Procedura: proteggere le stringhe](https://msdn.microsoft.com/library/ms178372.aspx) di connessione quando si utilizzano i controlli origine dati (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Ottimizzazione delle prestazioni di accesso ai datiOptimizing Data Access Performance

- [ASP.NET Cenni preliminari sulle prestazioni](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [ASP.NET la memorizzazione nella cache](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Miglioramento delle prestazioni ASP.NET](https://msdn.microsoft.com/library/ff647787) (MSDN). Nella parte superiore di questa pagina è presente un avviso "Contenuto ritirato", ma la maggior parte delle informazioni è ancora rilevante e non esiste una risorsa aggiornata comparabile.
- [Miglioramento delle prestazioni di SQL Server](https://msdn.microsoft.com/library/ff647793) (MSDN). Stesso commento del collegamento precedente.

Vedere anche [Ottimizzazione delle prestazioni di Entity Framework](#optimizingef) più indietro in questo argomento.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Distribuzione di un databaseDeploying a Database

- [Distribuzione Web ASP.NET - Risorse consigliate](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Accesso ai dati tramite un servizio WebAccessing Data through a Web Service

- [Accesso ai dati tramite un servizio Web](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Indicazioni su quando usare l'API Web rispetto a WCF.
- [Introduzione allASP.NETAPI Web](../web-api/index.md).
- [WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Risorse aggiuntive

- [ASP.NET domande frequenti sull'accesso ai dati](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [Esercitazioni ASP.NET Web Form - Dati](../web-forms/overview/data-access/index.md). La maggior parte di questi tutorial sono relativamente vecchi; Assicurarsi di leggere ASP.NET opzioni di accesso ai [dati](https://msdn.microsoft.com/library/ms178359.aspx) e opzioni di archiviazione dei dati (Creazione di app cloud del mondo reale con [Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) in modo da non accedere troppo a un metodo di accesso ai dati non adatto allo scenario.
- [ASP.NET mappa del contenuto MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [Esercitazioni ASP.NET pagine Web - Dati](../web-pages/overview/data/index.md).
- [Accesso ai dati in Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Fornisce un elenco di collegamenti simili a questa mappa del contenuto, ma con uno stato attivo su Visual Studio anziché su ASP.NET.
