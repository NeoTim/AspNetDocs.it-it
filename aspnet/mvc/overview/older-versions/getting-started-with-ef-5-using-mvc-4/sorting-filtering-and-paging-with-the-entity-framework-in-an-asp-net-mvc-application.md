---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Ordinamento, filtro e paging con la Entity Framework in un'applicazione MVC ASP.NET (3 di 10) | Microsoft Docs
author: tdykstra
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando il Entity Framework 5 Code First e Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 48938b378a741a0f1c351c2cb1d33b5140c6cf93
ms.sourcegitcommit: 8d34fb54e790cfba2d64097afc8276da5b22283e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/27/2020
ms.locfileid: "85484613"
---
# <a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Ordinamento, filtro e paging con la Entity Framework in un'applicazione MVC ASP.NET (3 di 10)

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto completato](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando i Entity Framework 5 Code First e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni dall'inizio o [scaricare un progetto iniziale per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si riscontra un problema che non è possibile risolvere, [scaricare il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. È in genere possibile trovare la soluzione al problema confrontando il codice con il codice completato. Per alcuni errori comuni e per informazioni su come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Nell'esercitazione precedente è stato implementato un set di pagine Web per le operazioni CRUD di base per le `Student` entità. In questa esercitazione verranno aggiunte le funzionalità di ordinamento, filtro e paging alla pagina di indice **students** . Verrà anche creata una pagina che esegue il raggruppamento semplice.

La figura seguente illustra l'aspetto della pagina al termine dell'operazione. Le intestazioni di colonna sono collegamenti su cui l'utente può fare clic per eseguire l'ordinamento in base alla colonna. Facendo clic più volte su un'intestazione di colonna è possibile passare dall'ordinamento crescente a quello decrescente e viceversa.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Aggiungere collegamenti per l'ordinamento di colonna alla pagina Student Index (Indice degli studenti)

Per aggiungere l'ordinamento alla pagina Student index, è necessario modificare il `Index` metodo del `Student` controller e aggiungere il codice alla `Student` visualizzazione index.

### <a name="add-sorting-functionality-to-the-index-method"></a>Aggiungere la funzionalità di ordinamento al metodo index

In *Controllers\StudentController.cs*sostituire il `Index` metodo con il codice seguente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Questo codice riceve un parametro `sortOrder` dalla stringa di query nell'URL. Il valore della stringa di query viene fornito da ASP.NET MVC come parametro al metodo di azione. Il parametro sarà una stringa "Name" o "Date", seguito facoltativamente da un carattere di sottolineatura e dalla stringa "desc" per specificare l'ordine decrescente. Per impostazione predefinita, l'ordinamento è crescente.

La prima volta che viene richiesta la pagina di indice, non è presente alcuna stringa di query. Gli studenti vengono visualizzati in ordine crescente per `LastName` , che è l'impostazione predefinita stabilita dal caso di rientri nell' `switch` istruzione. Quando l'utente fa clic sul collegamento ipertestuale di un'intestazione di colonna, nella stringa di query viene specificato il valore `sortOrder` appropriato.

Le due `ViewBag` variabili vengono usate in modo che la visualizzazione possa configurare i collegamenti ipertestuali dell'intestazione di colonna con i valori della stringa di query appropriati:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Si tratta di istruzioni ternarie. Il primo specifica che, se il `sortOrder` parametro è null o vuoto, `ViewBag.NameSortParm` deve essere impostato su "Name \_ DESC"; in caso contrario, deve essere impostato su una stringa vuota. Queste due istruzioni consentono alla visualizzazione di impostare i collegamenti ipertestuali dell'intestazione di colonna come indicato di seguito:

| Ordinamento corrente | Collegamento ipertestuale cognome | Collegamento ipertestuale data |
| --- | --- | --- |
| Cognome in ordine crescente | descending | ascending |
| Cognome in ordine decrescente | ascending | ascending |
| Data in ordine crescente | ascending | descending |
| Data in ordine decrescente | ascending | ascending |

Il metodo utilizza [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) per specificare la colonna in base alla quale eseguire l'ordinamento. Il codice crea una variabile [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) prima dell' `switch` istruzione, la modifica nell' `switch` istruzione e chiama il `ToList` metodo dopo l' `switch` istruzione. Quando si creano e modificano variabili `IQueryable`, nessuna query viene inviata al database. La query non viene eseguita finché l'oggetto non viene convertito `IQueryable` in una raccolta chiamando un metodo, ad esempio `ToList` . Pertanto, questo codice genera una singola query che non viene eseguita fino all' `return View` istruzione.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Aggiungere collegamenti ipertestuali dell'intestazione di colonna alla visualizzazione dell'indice Student

In *Views\Student\Index.cshtml*sostituire gli `<tr>` elementi e `<th>` per la riga di intestazione con il codice evidenziato:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Questo codice usa le informazioni contenute nelle `ViewBag` proprietà per impostare i collegamenti ipertestuali con i valori della stringa di query appropriati.

Eseguire la pagina e fare clic sulle intestazioni di colonna **Last Name** e **Data di registrazione** per verificare il corretto funzionamento dell'ordinamento.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Dopo aver fatto clic sull'intestazione **Last Name** , gli studenti vengono visualizzati in ordine decrescente in base al cognome.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Aggiungere una casella di ricerca alla pagina di indice degli studenti

Per aggiungere filtri alla pagina Student Index (Indice degli studenti), è necessario aggiungere alla visualizzazione una casella di testo e un pulsante di invio e apportare le modifiche corrispondenti nel metodo `Index`. La casella di testo consente di immettere una stringa per eseguire la ricerca nei campi di nome e cognome.

### <a name="add-filtering-functionality-to-the-index-method"></a>Aggiungere la funzionalità di filtro al metodo index

In *Controllers\StudentController.cs*sostituire il `Index` metodo con il codice seguente (le modifiche sono evidenziate):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

È stato aggiunto un parametro `searchString` al metodo `Index`. È stata aggiunta anche all'istruzione LINQ una `where` clausola che seleziona solo gli studenti il cui nome o cognome contiene la stringa di ricerca. Il valore della stringa di ricerca viene ricevuto da una casella di testo che verrà aggiunta alla visualizzazione dell'indice. L'istruzione che aggiunge la clausola [where](https://msdn.microsoft.com/library/bb535040.aspx) viene eseguita solo se è presente un valore da cercare.

> [!NOTE]
> In molti casi è possibile chiamare lo stesso metodo in un set di entità Entity Framework o come metodo di estensione in una raccolta in memoria. I risultati sono in genere uguali, ma in alcuni casi potrebbero essere diversi. Ad esempio, l'implementazione .NET Framework del `Contains` metodo restituisce tutte le righe quando viene passata una stringa vuota, ma il provider Entity Framework per SQL Server Compact 4,0 restituisce zero righe per le stringhe vuote. Pertanto, il codice nell'esempio, inserendo l' `Where` istruzione all'interno di un' `if` istruzione, assicura che si ottengano gli stessi risultati per tutte le versioni di SQL Server. Inoltre, l'implementazione .NET Framework del `Contains` metodo esegue un confronto con distinzione tra maiuscole e minuscole per impostazione predefinita, ma Entity Framework provider SQL Server eseguono confronti senza distinzione tra maiuscole e minuscole per impostazione predefinita. Pertanto, chiamando il `ToUpper` metodo per fare in modo che il test senza distinzione tra maiuscole e minuscole garantisce che i risultati non cambiano quando si modifica il codice in un secondo momento per usare un repository, che restituirà una `IEnumerable` raccolta anziché un `IQueryable` oggetto. Quando il metodo `Contains` viene chiamato su una raccolta `IEnumerable`, si ottiene l'implementazione di .NET Framework; quando viene chiamato su un oggetto `IQueryable`, si ottiene l'implementazione del provider di database.

### <a name="add-a-search-box-to-the-student-index-view"></a>Aggiungere una casella di ricerca alla visualizzazione Student Index (Indice degli studenti)

In *Views\Student\Index.cshtml*aggiungere il codice evidenziato immediatamente prima del tag di apertura per `table` creare una didascalia, una casella di testo e un pulsante di **ricerca** .

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Eseguire la pagina, immettere una stringa di ricerca e fare clic su **Cerca** per verificare che il filtro sia funzionante.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Si noti che l'URL non contiene la stringa di ricerca "an", il che significa che se si aggiunge un segnalibro a questa pagina, non si otterrà l'elenco filtrato quando si usa il segnalibro. Il pulsante **Cerca** verrà modificato in modo da usare le stringhe di query per i criteri di filtro più avanti nell'esercitazione.

## <a name="add-paging-to-the-students-index-page"></a>Aggiungere il paging alla pagina di indice degli studenti

Per aggiungere il paging alla pagina di indice students, iniziare installando il pacchetto NuGet **PagedList. Mvc** . Si apportano quindi ulteriori modifiche al `Index` metodo e si aggiungono collegamenti di paging alla `Index` visualizzazione. **PagedList. Mvc** è uno dei molti pacchetti di paging e ordinamento validi per ASP.NET MVC e il suo utilizzo è previsto solo come esempio, non come una raccomandazione per le altre opzioni. Nella figura seguente sono illustrati i collegamenti di paging.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Installare il pacchetto NuGet PagedList. MVC

Il pacchetto NuGet **PagedList. Mvc** installa automaticamente il pacchetto **PagedList** come dipendenza. Il pacchetto **PagedList** installa un `PagedList` tipo di raccolta e i metodi di estensione per le `IQueryable` `IEnumerable` raccolte e. I metodi di estensione creano una singola pagina di dati in una `PagedList` raccolta fuori da `IQueryable` o `IEnumerable` e la `PagedList` raccolta fornisce diverse proprietà e metodi che facilitano il paging. Il pacchetto **PagedList. Mvc** installa un helper di paging che Visualizza i pulsanti di paging.

Dal menu **strumenti** selezionare **Gestione pacchetti NuGet** e quindi **gestire i pacchetti NuGet per la soluzione**.

Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic sulla scheda **online** a sinistra e quindi immettere "paging" nella casella di ricerca. Quando viene visualizzato il pacchetto **PagedList. Mvc** , fare clic su **Installa**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Nella casella **Seleziona progetti** fare clic su **OK**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Aggiungere la funzionalità di paging al metodo index

In *Controllers\StudentController.cs*aggiungere un' `using` istruzione per lo `PagedList` spazio dei nomi:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Sostituire il metodo `Index` con il codice seguente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Questo codice aggiunge un parametro `page` , un parametro di ordinamento corrente e un parametro di filtro corrente alla firma del metodo, come illustrato di seguito:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

La prima volta che viene visualizzata la pagina o se l'utente non ha selezionato un collegamento di suddivisione in pagine o di ordinamento, tutti i parametri saranno Null. Se si fa clic su un collegamento di paging, la variabile conterrà `page` il numero di pagina da visualizzare.

`A ViewBag`la proprietà fornisce la visualizzazione con l'ordinamento corrente, perché deve essere incluso nei collegamenti di paging per mantenerlo nello stesso modo durante il paging:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Un'altra proprietà, `ViewBag.CurrentFilter` , fornisce la visualizzazione con la stringa di filtro corrente. Questo valore deve essere incluso nei collegamenti di suddivisione in pagine per mantenere le impostazioni di filtro nella suddivisione in pagine e deve essere ripristinato nella casella di testo quando la pagina viene nuovamente visualizzata. Se la stringa di ricerca viene modificata nella suddivisione in pagine, la pagina deve essere reimpostata su 1, poiché il nuovo filtro può comportare la visualizzazione di dati diversi. La stringa di ricerca viene modificata quando si immette un valore nella casella di testo e si preme il pulsante Invia. In tal caso, il `searchString` parametro non è null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Alla fine del metodo, il metodo di `ToPagedList` estensione nell'oggetto students `IQueryable` converte la query Student in una singola pagina di students in un tipo di raccolta che supporta il paging. Questa singola pagina di studenti viene quindi passata alla visualizzazione:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Il metodo `ToPagedList` accetta un numero di pagina. I due punti interrogativi rappresentano l' [operatore che unisce i valori null](https://msdn.microsoft.com/library/ms173224.aspx). L'operatore null-coalescing definisce un valore predefinito per un tipo nullable. L'espressione `(page ?? 1)` significa restituzione del valore di `page` se ha un valore oppure restituzione di 1 se `page` è Null.

### <a name="add-paging-links-to-the-student-index-view"></a>Aggiungere collegamenti di paging alla visualizzazione index Student

In *Views\Student\Index.cshtml*sostituire il codice esistente con il codice seguente:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

L'istruzione `@model` nella parte superiore della pagina specifica che la vista ottiene ora un oggetto `PagedList` anziché un oggetto `List`.

L' `using` istruzione per `PagedList.Mvc` fornisce l'accesso all'helper MVC per i pulsanti di paging.

Il codice usa un overload di [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) che consente di specificare [FormMethod. Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Il [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) predefinito invia i dati del modulo con un post, il che significa che i parametri vengono passati nel corpo del messaggio http e non nell'URL come stringhe di query. Quando si specifica HTTP GET, i dati del modulo vengono passati nell'URL come stringhe di query, il che consente agli utenti di inserire l'URL tra i segnalibri. Le [linee guida W3C per l'uso di http Get](http://www.w3.org/2001/tag/doc/whenToUseGet.html) specificano che è necessario usare Get quando l'azione non genera un aggiornamento.

La casella di testo viene inizializzata con la stringa di ricerca corrente, quindi quando si fa clic su una nuova pagina è possibile visualizzare la stringa di ricerca corrente.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

I collegamenti delle intestazioni di colonna usano la stringa di query per passare la stringa di ricerca corrente al controller in modo che l'utente possa procedere all'ordinamento all'interno dei risultati di filtro:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

Viene visualizzata la pagina corrente e il numero totale di pagine.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Se non sono presenti pagine da visualizzare, viene visualizzato "pagina 0 di 0". (In questo caso il numero di pagina è maggiore del conteggio delle pagine perché `Model.PageNumber` è 1 e `Model.PageCount` è 0).

I pulsanti di paging vengono visualizzati dall' `PagedListPager` Helper:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

L' `PagedListPager` helper fornisce diverse opzioni che è possibile personalizzare, inclusi URL e stile. Per ulteriori informazioni, vedere [TroyGoode/PagedList](https://github.com/TroyGoode/PagedList) nel sito github.

Eseguire la pagina.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Fare clic sui collegamenti di suddivisione in pagine in diversi tipi di ordinamento per verificare che la suddivisione in pagine funzioni. Immettere quindi una stringa di ricerca e provare nuovamente la suddivisione in pagine per verificare che funzioni correttamente anche con l'ordinamento e il filtro.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Creare una pagina informazioni che mostra le statistiche degli studenti

Per la pagina About (Informazioni) del sito Web Contoso University verrà visualizzato il numero di studenti iscritti per ogni data di iscrizione. Questa operazione richiede calcoli di raggruppamento e semplici sui gruppi. Per completare questa procedura, è necessario eseguire le operazioni seguenti:

- Creare una classe modello di visualizzazione per i dati che è necessario passare alla visualizzazione.
- Modificare il `About` metodo nel `Home` controller.
- Modificare la `About` vista.

### <a name="create-the-view-model"></a>Creare il modello di visualizzazione

Creare una cartella *ViewModels* . In tale cartella aggiungere un file di classe *EnrollmentDateGroup.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modificare il controller Home

In *HomeController.cs*aggiungere le istruzioni seguenti `using` all'inizio del file:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Aggiungere una variabile di classe per il contesto di database immediatamente dopo la parentesi graffa di apertura per la classe:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Sostituire il metodo `About` con il codice seguente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

L'istruzione LINQ raggruppa le entità di studenti per data di registrazione, calcola il numero di entità in ogni gruppo e archivia i risultati in una raccolta di oggetti di modello della visualizzazione `EnrollmentDateGroup`.

Aggiungere un `Dispose` Metodo:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modificare la visualizzazione della pagina About (Informazioni)

Sostituire il codice nel file *Views\Home\About.cshtml* con il codice seguente:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Eseguire l'app e fare clic sul collegamento **About (informazioni** ). Il numero di studenti per ogni data di registrazione viene visualizzato in una tabella.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Facoltativo: distribuire l'app in Windows Azure

Fino a questo punto l'applicazione è stata eseguita localmente in IIS Express nel computer di sviluppo. Per renderlo disponibile per l'uso da parte di altri utenti su Internet, è necessario distribuirlo a un provider di hosting Web. In questa sezione facoltativa dell'esercitazione verrà distribuita in un sito Web di Microsoft Azure.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Utilizzo di Migrazioni Code First per distribuire il database

Per distribuire il database, usare Migrazioni Code First. Quando si crea il profilo di pubblicazione usato per configurare le impostazioni per la distribuzione da Visual Studio, selezionare una casella di controllo denominata **Execute migrazioni Code First (eseguito all'avvio dell'applicazione)**. Questa impostazione fa in modo che il processo di distribuzione configuri automaticamente il file dell'applicazione *Web.config* nel server di destinazione in modo che Code First utilizzi la `MigrateDatabaseToLatestVersion` classe inizializzatore.

Durante il processo di distribuzione, Visual Studio non esegue alcuna operazione con il database. Quando l'applicazione distribuita accede al database per la prima volta dopo la distribuzione, Code First crea automaticamente il database o aggiorna lo schema del database alla versione più recente. Se l'applicazione implementa un metodo Migrations `Seed` , il metodo viene eseguito dopo la creazione del database o l'aggiornamento dello schema.

Il metodo Migrations `Seed` inserisce i dati di test. Se si esegue la distribuzione in un ambiente di produzione, è necessario modificare il `Seed` metodo in modo che inserisca solo i dati che si desidera inserire nel database di produzione. Nel modello di dati corrente, ad esempio, potrebbe essere necessario disporre di corsi reali ma studenti fittizi nel database di sviluppo. È possibile scrivere un `Seed` metodo per il caricamento in fase di sviluppo e quindi impostare come commento gli studenti fittizi prima della distribuzione nell'ambiente di produzione. In alternativa, è possibile scrivere un `Seed` metodo per caricare solo i corsi e immettere manualmente gli studenti fittizi nel database di test usando l'interfaccia utente dell'applicazione.

### <a name="get-a-windows-azure-account"></a>Ottenere un account Windows Azure

È necessario un account Windows Azure. Se non si dispone già di un account, è possibile creare un account di valutazione gratuito in pochi minuti. Per i dettagli, vedere [Versione di valutazione gratuita](https://azure.microsoft.com/free/dotnet/).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Creazione di un sito Web e di un database SQL in Windows Azure

Il sito Web di Windows Azure verrà eseguito in un ambiente host condiviso, ovvero viene eseguito in macchine virtuali (VM) condivise con altri client Windows Azure. Un ambiente di hosting condiviso è un punto di partenza economicamente conveniente per iniziare a utilizzare il cloud. In seguito, in caso di incremento del traffico Web, è possibile scalare l'applicazione in modo da soddisfare le nuove esigenze tramite l'esecuzione su macchine virtuali dedicate. Se è necessaria un'architettura più complessa, è possibile eseguire la migrazione a un servizio cloud di Windows Azure. I servizi cloud vengono eseguiti su macchine virtuali dedicate, che possono essere configurate in base alle proprie esigenze specifiche.

Il database SQL di Windows Azure è un servizio di database relazionale basato sul cloud basato su tecnologie SQL Server. Gli strumenti e le applicazioni che funzionano con SQL Server funzionano anche con il database SQL.

1. Nel [portale di gestione di Azure](https://manage.windowsazure.com/)fare clic su **siti Web** nella scheda a sinistra, quindi fare clic su **nuovo**.

    ![Pulsante nuovo in portale di gestione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Fare clic su **creazione personalizzata**.

    ![Crea con collegamento al database in portale di gestione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   Si apre la creazione guidata **nuovo sito web-creazione personalizzata** .
3. Nel passaggio **nuovo sito Web** della procedura guidata immettere una stringa nella casella **URL** da usare come URL univoco per l'applicazione. L'URL completo sarà costituito da quanto immesso in questa casella e dal suffisso visualizzato accanto alla casella di testo. L'illustrazione mostra "ConU", ma è probabile che l'URL venga scelto in modo che sia necessario sceglierne uno diverso.

    ![Crea con collegamento al database in portale di gestione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. Nell'elenco a discesa **Region (area** ) scegliere un'area vicino all'utente. Questa impostazione specifica quali data center verrà eseguito il sito Web.
5. Nell'elenco a discesa **database** scegliere **Crea un database SQL gratuito da 20 MB**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. Nel **nome della stringa di connessione del database**immettere *schoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Fare clic sulla freccia che punta a destra nella parte inferiore della casella. La procedura guidata consente di passare al passaggio **Impostazioni database** .
8. Nella casella **nome** immettere *ContosoUniversityDB*.
9. Nella casella **Server** selezionare **nuovo server di database SQL**. In alternativa, se in precedenza è stato creato un server, è possibile selezionare il server dall'elenco a discesa.
10. Immettere un **nome di accesso** e una **password**per l'amministratore. Se è stata selezionata l'opzione **Nuovo server di database SQL**, in questo passaggio non sarà necessario immettere un nome e una password esistenti, ma un nuovo nome e una nuova password, definiti ora e da utilizzare in seguito per l'accesso al database. Se è stato selezionato un server creato in precedenza, immettere le credenziali per tale server. Per questa esercitazione non si seleziona la casella di controllo ***Avanzate*** . Le opzioni ***Avanzate*** consentono di impostare le regole di [confronto](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx)del database.
11. Scegliere la stessa **area** selezionata per il sito Web.
12. Fare clic sul segno di spunta nella parte inferiore destra della casella per indicare che l'operazione è terminata.   
  
    ![Passaggio impostazioni database del nuovo sito Web-Creazione guidata database](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    Nell'immagine seguente viene illustrato l'utilizzo di un SQL Server esistente e di un account di accesso.   
  
    ![Passaggio impostazioni database del nuovo sito Web-Creazione guidata database](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    Il portale di gestione torna alla pagina siti Web e la colonna **stato** indica che è in corso la creazione del sito. Dopo un periodo di tempo (in genere inferiore a un minuto), nella colonna **stato** viene indicato che il sito è stato creato correttamente. Sulla barra di spostamento a sinistra, il numero di siti presenti nell'account viene visualizzato accanto all'icona **siti Web** e il numero di database viene visualizzato accanto all'icona **database SQL** .

## <a name="deploy-the-application-to-windows-azure"></a>Distribuire l'applicazione in Windows Azure

1. In Visual Studio fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Pubblica** dal menu di scelta rapida.  
  
    ![Opzione Pubblica nel menu di scelta rapida del progetto](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. Nella scheda **profilo** della procedura guidata **Pubblica sito Web** fare clic su **Importa**.  
  
    ![Importazione delle impostazioni di pubblicazione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Se in precedenza non è stata aggiunta la sottoscrizione di Windows Azure in Visual Studio, seguire questa procedura. In questa procedura viene aggiunta la sottoscrizione in modo che l'elenco a discesa in **Importa da un sito Web di Microsoft Azure** includa il sito Web.

    a. Nella finestra di dialogo **Importa profilo di pubblicazione** fare clic su **Importa da un sito Web di Microsoft Azure**e quindi fare clic su **Aggiungi sottoscrizione Windows Azure**.

    ![Aggiungi sottoscrizione Windows Azure](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. Nella finestra di dialogo **Importa sottoscrizioni Windows Azure** fare clic su **Scarica file di sottoscrizione**.

    ![Scarica file di sottoscrizione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. Nella finestra del browser salvare il file con *estensione publishsettings* .

    ![scaricare il file con estensione publishsettings](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Sicurezza: il file *publishsettings* contiene le credenziali (non codificate) utilizzate per amministrare i servizi e le sottoscrizioni Windows Azure. La procedura consigliata di sicurezza per questo file consiste nell'archiviarlo temporaneamente all'esterno delle directory di origine, ad esempio nella cartella *raccolte\documenti* , e quindi eliminarlo al termine dell'importazione. Un utente malintenzionato che accede al `.publishsettings` file può modificare, creare ed eliminare i servizi di Windows Azure.

    d. Nella finestra di dialogo **Importa sottoscrizioni Windows Azure** fare clic su **Sfoglia** e passare al file con *estensione publishsettings* .

    ![Scarica Sub](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. Fare clic su **Importa**.

    ![importazione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. Nella finestra di dialogo **Importa profilo di pubblicazione** selezionare **Importa da un sito Web di Microsoft Azure**, selezionare il sito Web dall'elenco a discesa, quindi fare clic su **OK**.  
  
    ![Importa profilo di pubblicazione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. Nella scheda **connessione** fare clic su **convalida connessione** per assicurarsi che le impostazioni siano corrette.  
  
    ![Convalida connessione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Quando la connessione è stata convalidata, accanto al pulsante **convalida connessione** viene visualizzato un segno di spunta verde. Fare clic su **Avanti**.  
  
    ![La connessione è stata convalidata](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Aprire l'elenco a discesa **stringa di connessione remota** in **schoolContext** e selezionare la stringa di connessione per il database creato.
8. Selezionare **esegui migrazioni Code First (eseguito all'avvio dell'applicazione)**.
9. Deselezionare **Usa questa stringa di connessione in** fase di esecuzione per **UserContext (DefaultConnection)**, perché l'applicazione non usa il database delle appartenenze.   
  
    ![Scheda Settings](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Fare clic su **Avanti**.
11. Nella scheda **Anteprima** fare clic su **Avvia anteprima**.  
  
    ![Pulsante StartPreview nella scheda Anteprima](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    Nella scheda viene visualizzato un elenco dei file che verranno copiati nel server. La visualizzazione dell'anteprima non è necessaria per pubblicare l'applicazione ma è una funzione utile da tenere presente. In questo caso, non è necessario eseguire alcuna operazione con l'elenco di file visualizzato. Alla successiva distribuzione dell'applicazione, verranno elencati solo i file modificati.  
  
    ![Output del file StartPreview](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Fare clic su **Pubblica**.  
    Visual Studio avvia il processo di copia dei file nel server Windows Azure.
13. Nella finestra **Output** vengono indicate le azioni di distribuzione effettuate e viene segnalato il corretto completamento della distribuzione.  
  
    ![Finestra di output che segnala la distribuzione riuscita](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Al completamento della distribuzione, il browser predefinito si apre automaticamente all'URL del sito Web distribuito.  
    L'applicazione creata verrà ora eseguita nel cloud. Fare clic sulla scheda Students.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

A questo punto il database *schoolContext* è stato creato nel database SQL di Windows Azure perché è stata selezionata l'opzione **Esegui migrazioni Code First (esecuzione all'avvio dell'app)**. Il file *Web.config* nel sito Web distribuito è stato modificato in modo da consentire l'esecuzione dell'inizializzatore [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) la prima volta che il codice legge o scrive i dati nel database, che si è verificato quando è stata selezionata la scheda **students (studenti** ):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

Il processo di distribuzione ha creato anche una nuova stringa di connessione *(schoolContext \_ DatabasePublish*) per migrazioni Code First da usare per l'aggiornamento dello schema del database e il seeding del database.

![Stringa di connessione Database_Publish](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

La stringa di connessione *DefaultConnection* è per il database delle appartenenze (che non verrà usato in questa esercitazione). La stringa di connessione *schoolContext* è per il database ContosoUniversity.

È possibile trovare la versione distribuita del file di Web.config nel computer in uso *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. È possibile accedere al file di *Web.config* distribuito utilizzando FTP. Per istruzioni, vedere [distribuzione Web ASP.NET con Visual Studio: distribuzione di un aggiornamento del codice](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Seguire le istruzioni che iniziano con "per usare uno strumento FTP, sono necessari tre elementi: l'URL FTP, il nome utente e la password".

> [!NOTE]
> L'app Web non implementa la sicurezza, pertanto chiunque trovi l'URL può modificare i dati. Per istruzioni su come proteggere il sito Web, vedere [distribuire un'app ASP.NET MVC sicura con appartenenza, OAuth e database SQL in un sito Web di Microsoft Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Per arrestare il sito, è possibile impedire ad altri utenti di utilizzare il sito utilizzando il portale di gestione o **Esplora server** di Microsoft Azure in Visual Studio.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Inizializzatori di Code First

Nella sezione Deployment è stato rilevato l'inizializzatore [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) in uso. Code First fornisce anche altri inizializzatori che è possibile usare, tra cui [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (impostazione predefinita), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) e [DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). L' `DropCreateAlways` inizializzatore può essere utile per configurare le condizioni per gli unit test. È anche possibile scrivere inizializzatori personalizzati ed è possibile chiamare un inizializzatore in modo esplicito se non si vuole attendere che l'applicazione legga o scriva nel database. Per una spiegazione completa degli inizializzatori, vedere il capitolo 6 del libro [programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) di Julie Lerman e Rowan Miller.

## <a name="summary"></a>Summary

In questa esercitazione è stato illustrato come creare un modello di dati e implementare funzionalità CRUD, di ordinamento, di filtro, di paging e di raggruppamento di base. Nell'esercitazione successiva si inizierà a esaminare gli argomenti più avanzati espandendo il modello di dati.

I collegamenti ad altre risorse Entity Framework sono disponibili nella [mappa del contenuto di ASP.NET Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md) 
>  [Avanti](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
