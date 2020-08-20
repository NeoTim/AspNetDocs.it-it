---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Scenari di Entity Framework avanzati per un'applicazione Web MVC (10 di 10) | Microsoft Docs
author: tdykstra
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando il Entity Framework 5 Code First e Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: f8f079f6d8ea663c6888456be422a2bae93a4b87
ms.sourcegitcommit: c9d9210e0d16fbb3829b7688cfb832dc263c79cc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/20/2020
ms.locfileid: "86163583"
---
# <a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Scenari di Entity Framework avanzati per un'applicazione Web MVC (10 di 10)

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto completato](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando i Entity Framework 5 Code First e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni dall'inizio o [scaricare un progetto iniziale per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si riscontra un problema che non è possibile risolvere, [scaricare il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. È in genere possibile trovare la soluzione al problema confrontando il codice con il codice completato. Per alcuni errori comuni e per informazioni su come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Nell'esercitazione precedente sono stati implementati i modelli di repository e unità di lavoro. Questa esercitazione illustra gli argomenti seguenti:

- Esecuzione di query SQL non elaborate.
- Esecuzione di query senza rilevamento.
- Analisi delle query inviate al database.
- Utilizzo delle classi proxy.
- Disabilitazione del rilevamento automatico delle modifiche.
- Disabilitazione della convalida quando si salvano le modifiche.
- [Errori e aggiramenti](#errors)

Per la maggior parte di questi argomenti, si useranno le pagine già create. Per usare SQL non elaborato per eseguire aggiornamenti in blocco, è possibile creare una nuova pagina che aggiorna il numero di crediti di tutti i corsi nel database:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Per usare una query senza rilevamento verrà aggiunta la nuova logica di convalida alla pagina di modifica del reparto:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Esecuzione di query SQL non elaborate

L'API Code First Entity Framework include metodi che consentono di passare i comandi SQL direttamente al database. Sono disponibili le opzioni seguenti:

- Usare il metodo `DbSet.SqlQuery` per le query che restituiscono tipi di entità. Gli oggetti restituiti devono essere del tipo previsto dall' `DbSet` oggetto e vengono rilevati automaticamente dal contesto di database, a meno che non si spenga la traccia. (Vedere la sezione seguente sul `AsNoTracking` metodo).
- Usare il `Database.SqlQuery` metodo per le query che restituiscono tipi che non sono entità. I dati restituiti non vengono registrati dal contesto di database, anche se il metodo viene usato per recuperare i tipi di entità.
- Usare il [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx) per i comandi non di query.

Uno dei vantaggi dell'utilizzo di Entity Framework è la mancanza di un collegamento troppo stretto del codice a un particolare metodo di archiviazione dei dati. Le query e i comandi SQL vengono generati automaticamente e non è necessario scriverli. Tuttavia, esistono scenari eccezionali in cui è necessario eseguire query SQL specifiche create manualmente e questi metodi consentono di gestire tali eccezioni.

Come avviene quando si eseguono comandi SQL in un'applicazione Web, è necessario adottare delle precauzioni per proteggere il sito dagli attacchi SQL injection. A questo scopo è possibile usare query parametrizzate per assicurarsi che le stringhe inviate da una pagina Web non possano essere interpretate come comandi SQL. In questa esercitazione verranno usate query parametrizzate quando l'input dell'utente viene integrato in una query.

### <a name="calling-a-query-that-returns-entities"></a>Chiamata di una query che restituisce entità

Si supponga di volere che la `GenericRepository` classe fornisca ulteriore flessibilità di filtro e ordinamento senza che sia necessario creare una classe derivata con metodi aggiuntivi. Un modo per ottenere questo risultato consiste nell'aggiungere un metodo che accetta una query SQL. È quindi possibile specificare qualsiasi tipo di filtro o ordinamento desiderato nel controller, ad esempio una `Where` clausola che dipende da un join o una sottoquery. In questa sezione viene illustrato come implementare un metodo di questo tipo.

Creare il `GetWithRawSql` metodo aggiungendo il codice seguente a *GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

In *CourseController.cs*chiamare il nuovo metodo dal `Details` metodo, come illustrato nell'esempio seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

In questo caso è possibile che sia stato usato il metodo `GetByID` , ma si sta usando il `GetWithRawSql` metodo per verificare che il `GetWithRawSQL` metodo funzioni.

Eseguire la pagina dei dettagli per verificare che la query di selezione funzioni (selezionare la scheda **corso** e quindi **Dettagli** per un corso).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Chiamata di una query che restituisce altri tipi di oggetti

In precedenza è stata creata una griglia delle statistiche degli studenti per la pagina About che visualizza il numero di studenti per ogni data di registrazione. Il codice che esegue questa operazione in *HomeController.cs* USA LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Si supponga di voler scrivere il codice che recupera questi dati direttamente in SQL anziché usare LINQ. A tale scopo, è necessario eseguire una query che restituisca un valore diverso da oggetti entità, il che significa che è necessario usare il `Database.SqlQuery` metodo.

In *HomeController.cs*sostituire l'istruzione LINQ nel `About` metodo con il codice seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Eseguire la pagina informazioni su. Vengono visualizzati gli stessi dati visualizzati in precedenza.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>Chiamata di una query di aggiornamento

Si supponga che gli amministratori di Contoso University vogliano essere in grado di eseguire modifiche bulk nel database, ad esempio la modifica del numero di crediti per ogni corso. Se il numero di corsi dell'università è elevato, potrebbe non essere utile recuperarli tutti come entità e modificarli singolarmente. In questa sezione verrà implementata una pagina Web che consente all'utente di specificare un fattore in base al quale modificare il numero di crediti per tutti i corsi e di apportare la modifica eseguendo un' `UPDATE` istruzione SQL. La pagina Web apparirà come segue:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

Nell'esercitazione precedente è stato usato il repository generico per leggere e aggiornare le `Course` entità nel `Course` controller. Per questa operazione di aggiornamento in blocco, è necessario creare un nuovo metodo di repository che non si trovi nel repository generico. A tale scopo, si creerà una classe dedicata `CourseRepository` che deriva dalla `GenericRepository` classe.

Nella cartella *dal* creare *CourseRepository.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

In *UnitOfWork.cs*modificare il `Course` tipo di repository da `GenericRepository<Course>` a `CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

In *CourseController.cs*aggiungere un `UpdateCourseCredits` Metodo:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Questo metodo verrà usato sia per `HttpGet` che per `HttpPost` . Quando il `HttpGet` `UpdateCourseCredits` metodo viene eseguito, la `multiplier` variabile sarà null e nella vista verranno visualizzate una casella di testo vuota e un pulsante Submit, come illustrato nella figura precedente.

Quando si fa clic sul pulsante **Aggiorna** e viene `HttpPost` eseguito il metodo, `multiplier` in verrà immesso il valore nella casella di testo. Il codice chiama quindi il metodo del repository `UpdateCourseCredits` , che restituisce il numero di righe interessate e tale valore viene archiviato nell' `ViewBag` oggetto. Quando la vista riceve il numero di righe interessate nell' `ViewBag` oggetto, viene visualizzato tale numero anziché la casella di testo e il pulsante Invia, come illustrato nella figura seguente:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Creare una visualizzazione nella cartella *Views\Course* per la pagina relativa ai crediti del corso di aggiornamento:

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

In *Views\Course\UpdateCourseCredits.cshtml*sostituire il codice del modello con il codice seguente:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Eseguire il metodo `UpdateCourseCredits` selezionando la scheda **Courses**, quindi aggiungendo "/UpdateCourseCredits" alla fine dell'URL nella barra degli indirizzi del browser (ad esempio: `http://localhost:50205/Course/UpdateCourseCredits`). Immettere un numero nella casella di testo:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Fare clic su **Update**. Viene visualizzato il numero di righe interessate:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Fare clic su **Torna all'elenco** per visualizzare l'elenco dei corsi con il numero di crediti modificato.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Per ulteriori informazioni sulle query SQL non elaborate, vedere [query SQL non elaborate](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) nel Blog del team di Entity Framework.

## <a name="no-tracking-queries"></a>senza rilevamento delle modifiche

Quando un contesto di database recupera le righe del database e crea oggetti entità che li rappresentano, per impostazione predefinita tiene traccia del fatto che le entità in memoria siano sincronizzate con le informazioni contenute nel database. I dati in memoria svolgono la funzione di una cache e vengono usati per l'aggiornamento di un'entità. Questa memorizzazione nella cache spesso non è necessaria in un'applicazione Web poiché le istanze del contesto hanno spesso una durata breve (viene creata ed eliminata una nuova istanza per ogni richiesta) e il contesto che legge un'entità viene in genere eliminato prima che l'entità venga riutilizzata.

È possibile specificare se il contesto tiene traccia degli oggetti entità per una query tramite il `AsNoTracking` metodo. Gli scenari tipici in cui viene disabilitata la registrazione includono i seguenti:

- La query recupera un volume elevato di dati che la disattivazione del rilevamento può migliorare notevolmente le prestazioni.
- Si vuole alleghi un'entità per aggiornarla, ma in precedenza è stata recuperata la stessa entità per uno scopo diverso. Poiché l'entità viene già registrata dal contesto di database, non è possibile collegare l'entità che si vuole modificare. Un modo per evitare che ciò accada consiste nell'usare l' `AsNoTracking` opzione con la query precedente.

In questa sezione verrà implementata la logica di business che illustra il secondo di questi scenari. In particolare, verrà applicata una regola business che indica che un insegnante non può essere l'amministratore di più di un reparto.

In *DepartmentController.cs*aggiungere un nuovo metodo che è possibile chiamare dai `Edit` `Create` metodi e per assicurarsi che nessuno dei due reparti disponga dello stesso amministratore:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Aggiungere il codice nel `try` blocco del `HttpPost` `Edit` metodo per chiamare questo nuovo metodo se non sono presenti errori di convalida. Il `try` blocco ha un aspetto simile all'esempio seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Eseguire la pagina di modifica del reparto e provare a modificare l'amministratore di un reparto in un insegnante che è già amministratore di un altro reparto. Viene ricevuto il messaggio di errore previsto:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

A questo punto, eseguire di nuovo la pagina di modifica del reparto e questa volta modificare l'importo del **budget** . Quando si fa clic su **Salva**, viene visualizzata una pagina di errore:

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Il messaggio di errore di eccezione è " `An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.` ". questa operazione è stata eseguita a causa della seguente sequenza di eventi:

- Il `Edit` metodo chiama il `ValidateOneAdministratorAssignmentPerInstructor` metodo, che recupera tutti i reparti con Kim Abercrombie come amministratore. Che determina la lettura del reparto inglese. Poiché si tratta del reparto modificato, non viene segnalato alcun errore. In seguito a questa operazione di lettura, tuttavia, l'entità del reparto inglese letta dal database viene ora rilevata dal contesto del database.
- Il `Edit` metodo tenta di impostare il `Modified` flag sull'entità Department inglese creata dallo strumento di associazione di modelli MVC, ma ciò non riesce perché il contesto sta già verificando un'entità per il reparto inglese.

Una soluzione a questo problema consiste nel tenere il contesto dal rilevamento delle entità del reparto in memoria recuperate dalla query di convalida. Questa operazione non è possibile, perché non verrà aggiornata o riletta in un modo che potrebbe trarre vantaggio dalla memorizzazione nella cache.

In *DepartmentController.cs*, nel `ValidateOneAdministratorAssignmentPerInstructor` metodo, specificare nessuna traccia, come illustrato di seguito:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Ripetere il tentativo di modificare l'importo del **budget** di un reparto. Questa volta l'operazione ha esito positivo e il sito viene restituito come previsto nella pagina di indice dei reparti, mostrando il valore del budget modificato.

## <a name="examining-queries-sent-to-the-database"></a>Analisi delle query inviate al database

A volte può essere utile visualizzare le query SQL inviate al database. A tale scopo, è possibile esaminare una variabile di query nel debugger o chiamare il metodo della query `ToString` . Per provare questa operazione, è possibile esaminare una semplice query e quindi osservare cosa accade quando si aggiungono opzioni quali il caricamento, l'applicazione di filtri e l'ordinamento eager.

In *Controllers/CourseController*sostituire il `Index` metodo con il codice seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

Impostare ora un punto di interruzione in *GenericRepository.cs* in `return query.ToList();` e le `return orderBy(query).ToList();` istruzioni del `Get` metodo. Eseguire il progetto in modalità di debug e selezionare la pagina relativa all'indice del corso. Quando il codice raggiunge il punto di interruzione, esaminare la `query` variabile. Viene visualizzata la query inviata a SQL Server. Si tratta di una semplice `Select` istruzione:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

Le query possono essere troppo lunghe per essere visualizzate nelle finestre di debug in Visual Studio. Per visualizzare l'intera query, è possibile copiare il valore della variabile e incollarlo in un editor di testo:

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

A questo punto si aggiungerà un elenco a discesa nella pagina di indice del corso in modo che gli utenti possano filtrare per un reparto specifico. I corsi verranno ordinati in base al titolo e si specificherà il caricamento eager per la `Department` proprietà di navigazione. In *CourseController.cs*sostituire il `Index` metodo con il codice seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

Il metodo riceve il valore selezionato dell'elenco a discesa nel `SelectedDepartment` parametro. Se non è selezionato alcun elemento, questo parametro sarà null.

Una `SelectList` raccolta che contiene tutti i reparti viene passata alla visualizzazione per l'elenco a discesa. I parametri passati al `SelectList` Costruttore specificano il nome del campo del valore, il nome del campo di testo e l'elemento selezionato.

Per il `Get` metodo del `Course` repository, il codice specifica un'espressione di filtro, un ordinamento e il caricamento eager per la `Department` proprietà di navigazione. L'espressione di filtro restituisce sempre `true` se non è selezionato alcun elemento nell'elenco a discesa (ovvero `SelectedDepartment` è null).

In *Views\Course\Index.cshtml*, immediatamente prima del `table` tag di apertura, aggiungere il codice seguente per creare l'elenco a discesa e un pulsante Submit (Invia):

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Con i punti di interruzione ancora impostati nella `GenericRepository` classe, eseguire la pagina di indice del corso. Continuare con le prime due volte che il codice raggiunge un punto di interruzione, in modo che la pagina venga visualizzata nel browser. Selezionare un reparto dall'elenco a discesa e fare clic su **filtro**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Questa volta il primo punto di interruzione sarà per la query Departments per l'elenco a discesa. Ignorarlo e visualizzare la `query` variabile la volta successiva che il codice raggiunge il punto di interruzione per visualizzare l' `Course` aspetto della query. Verrà visualizzata una schermata simile alla seguente:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

Si può notare che la query è ora una `JOIN` query che carica i `Department` dati insieme ai `Course` dati e che include una `WHERE` clausola.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Utilizzo delle classi proxy

Quando il Entity Framework crea istanze di entità, ad esempio quando si esegue una query, le crea spesso come istanze di un tipo derivato generato dinamicamente che funge da proxy per l'entità. Questo proxy esegue l'override di alcune proprietà virtuali dell'entità per inserire hook per eseguire automaticamente le azioni quando si accede alla proprietà. Questo meccanismo, ad esempio, viene utilizzato per supportare il caricamento lazy delle relazioni.

Nella maggior parte dei casi non è necessario essere a conoscenza di questo utilizzo di proxy, ma esistono alcune eccezioni:

- In alcuni scenari potrebbe essere necessario impedire al Entity Framework di creare istanze proxy. Ad esempio, la serializzazione di istanze non proxy potrebbe essere più efficiente rispetto alla serializzazione delle istanze proxy.
- Quando si crea un'istanza di una classe di entità usando l' `new` operatore, non si ottiene un'istanza del proxy. Ciò significa che non si ottengono funzionalità come il caricamento lazy e il rilevamento automatico delle modifiche. Si tratta in genere di un problema. in genere non è necessario il caricamento lazy perché si sta creando una nuova entità che non si trova nel database e in genere non è necessario il rilevamento delle modifiche se si contrassegna in modo esplicito l'entità come `Added` . Tuttavia, se è necessario il caricamento lazy ed è necessario il rilevamento delle modifiche, è possibile creare nuove istanze di entità con proxy usando il `Create` metodo della `DbSet` classe.
- Potrebbe essere necessario ottenere un tipo di entità effettivo da un tipo di proxy. È possibile utilizzare il `GetObjectType` metodo della `ObjectContext` classe per ottenere il tipo di entità effettivo di un'istanza del tipo di proxy.

Per ulteriori informazioni, vedere [utilizzo di proxy](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) nel Blog del team di Entity Framework.

## <a name="disabling-automatic-detection-of-changes"></a>Disabilitazione del rilevamento automatico delle modifiche

Entity Framework determina come è stata modificata un'entità (e di conseguenza gli aggiornamenti da inviare al database) confrontando i valori correnti di un'entità con i valori originali. I valori originali vengono archiviati quando l'entità è stata sottoposta a query o è stata collegata. I metodi che causano il rilevamento automatico delle modifiche includono:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Se si tiene traccia di un numero elevato di entità e si chiama uno di questi metodi molte volte in un ciclo, è possibile ottenere miglioramenti significativi delle prestazioni disattivando temporaneamente il rilevamento automatico delle modifiche usando la proprietà [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) . Per ulteriori informazioni, vedere [rilevamento automatico delle modifiche](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>Disabilitazione della convalida quando si salvano le modifiche

Quando si chiama il `SaveChanges` metodo, per impostazione predefinita il Entity Framework convalida i dati in tutte le proprietà di tutte le entità modificate prima di aggiornare il database. Se è stato aggiornato un numero elevato di entità e sono già stati convalidati i dati, questa operazione non è necessaria ed è possibile fare in modo che il processo di salvataggio delle modifiche imprenda meno tempo disattivando temporaneamente la convalida. Questa operazione può essere eseguita usando la proprietà [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) . Per ulteriori informazioni, vedere [convalida](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Riepilogo

Questa operazione completa questa serie di esercitazioni sull'uso del Entity Framework in un'applicazione MVC ASP.NET. I collegamenti ad altre risorse Entity Framework sono disponibili nella [mappa del contenuto di ASP.NET Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

Per ulteriori informazioni su come distribuire l'applicazione Web dopo averla compilata, vedere la pagina relativa alla [mappa del contenuto della distribuzione di ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx) in MSDN Library.

Per informazioni su altri argomenti correlati a MVC, ad esempio l'autenticazione e l'autorizzazione, vedere le [risorse consigliate per MVC](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>Ringraziamenti

- Tom Dykstra ha scritto la versione originale di questa esercitazione ed è uno scrittore di programmazione senior sul team di contenuti degli strumenti e della piattaforma Web Microsoft.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT) ) ha condiviso questa esercitazione ed ha eseguito la maggior parte delle attività di aggiornamento per EF 5 e MVC 4. Rick è Senior Programming Writer per Microsoft focalizzato su Azure e MVC.
- [Rowan Miller](http://www.romiller.com) e altri membri del team di Entity Framework hanno assistito con le revisioni del codice e hanno aiutato a eseguire il debug di molti problemi con le migrazioni emerse durante l'aggiornamento dell'esercitazione per EF 5.

## <a name="vb"></a>VB

Quando l'esercitazione è stata originariamente prodotta, sono state fornite le versioni C# e VB del progetto di download completato. Con questo aggiornamento viene fornito un progetto per il download di C# per ogni capitolo, per semplificare l'introduzione in qualsiasi punto della serie, ma a causa di limitazioni temporali e di altre priorità non eseguite per VB. Se si compila un progetto VB usando queste esercitazioni e si è disposti a condividerlo con altri utenti, è possibile inviarlo.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Errori e soluzioni alternative

### <a name="cannot-createshadow-copy"></a>Non è possibile creare/copiare copie shadow

Messaggio di errore:

*Non è possibile creare una copia shadow ' DotNetOpenAuth. OpenId ' se il file esiste già.*

Soluzione:

Attendere alcuni secondi e aggiornare la pagina.

### <a name="update-database-not-recognized"></a>Update-database non riconosciuto

Messaggio di errore:

*Il termine "Update-database" non è riconosciuto come nome di un cmdlet, di una funzione, di un file di script o di un programma eseguibile. Controllare l'ortografia del nome o, se è stato incluso un percorso, verificare che il percorso sia corretto e riprovare.* (Dal *`Update-Database`* comando in PMC).

Soluzione:

Uscire da Visual Studio. Riaprire il progetto e riprovare.

### <a name="validation-failed"></a>Convalida non riuscita

Messaggio di errore:

*Convalida non riuscita per una o più entità. Per ulteriori informazioni, vedere la proprietà' EntityValidationErrors '.* (Dal *`Update-Database`* comando in PMC).

Soluzione:

Una causa di questo problema è costituita da errori di convalida durante l' `Seed` esecuzione del metodo. Vedere [seeding and Debugging Entity Framework (EF) DB](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) per suggerimenti sul debug del `Seed` metodo.

### <a name="http-50019-error"></a>Errore HTTP 500,19

Messaggio di errore:

*Errore HTTP 500,19-errore interno del server. Impossibile accedere alla pagina richiesta perché i dati di configurazione correlati per la pagina non sono validi.*

Soluzione:

Un modo per ottenere questo errore consiste nel disporre di più copie della soluzione, ognuna con lo stesso numero di porta. In genere, è possibile risolvere questo problema chiudendo tutte le istanze di Visual Studio, quindi riavviando il progetto su cui si sta lavorando. Se l'operazione non funziona, provare a modificare il numero di porta. Fare clic con il pulsante destro del mouse sul file di progetto e quindi scegliere Proprietà. Selezionare la scheda **Web** , quindi modificare il numero di porta nella casella di testo **URL progetto** .

### <a name="error-locating-sql-server-instance"></a>Errore di individuazione dell'istanza di SQL Server

Messaggio di errore:

*Si è verificato un errore specifico dell'istanza o relativo alla rete durante il tentativo di stabilire una connessione a SQL Server. Il server non è stato trovato o non è accessibile. Verificare che il nome dell'istanza sia corretto e che SQL Server sia configurato per consentire le connessioni remote. (provider: interfacce di rete SQL, errore: 26-errore nell'individuazione del server/dell'istanza specificati)*

Soluzione:

Controllare la stringa di connessione. Se il database è stato eliminato manualmente, modificare il nome del database nella stringa di costruzione.

> [!div class="step-by-step"]
> [Precedente](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md) 
>  [Avanti](building-the-ef5-mvc4-chapter-downloads.md)
