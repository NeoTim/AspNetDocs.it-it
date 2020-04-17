---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
title: Creazione di classi modello con LINQ to SQL (C) Documenti Microsoft
author: rick-anderson
description: L'obiettivo di questa esercitazione è illustrare un metodo di creazione di classi modello per un'applicazione MVC ASP.NET. In questa esercitazione si apprenderà come compilare il modello c...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f84b4a16-e8bb-49e8-87a0-1832879a3501
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
msc.type: authoredcontent
ms.openlocfilehash: eac6fec3a4cfd833b810378d946874a246d9a2c3
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542729"
---
# <a name="creating-model-classes-with-linq-to-sql-c"></a>Creazione di classi di modelli con LINQ to SQL (C#)

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_CS.pdf)

> L'obiettivo di questa esercitazione è illustrare un metodo di creazione di classi modello per un'applicazione MVC ASP.NET. In questa esercitazione si apprenderà come compilare classi modello ed eseguire l'accesso al database sfruttando Microsoft LINQ to SQL.

L'obiettivo di questa esercitazione è illustrare un metodo di creazione di classi modello per un'applicazione MVC ASP.NET. In questa esercitazione si apprenderà come compilare classi modello ed eseguire l'accesso al database sfruttando Microsoft LINQ to SQL

In questa esercitazione viene compilata un'applicazione di database Movie di base. Iniziamo creando l'applicazione di database Movie nel modo più veloce e semplice possibile. Eseguiamo tutti i nostri dati di accesso direttamente dalle nostre azioni controller.

Successivamente, si apprenderà come utilizzare il modello repository. L'utilizzo del modello Repository richiede un po' più di lavoro. Tuttavia, il vantaggio di adottare questo modello è che consente di creare applicazioni che sono adattabili alle modifiche e possono essere facilmente testate.

## <a name="what-is-a-model-class"></a>Che cos'è una classe modello?

Un modello MVC contiene tutta la logica dell'applicazione non contenuta in una visualizzazione MVC o in un controller MVC. In particolare, un modello MVC contiene tutta la logica di accesso ai dati e business dell'applicazione.

È possibile usare diverse tecnologie per implementare la logica di accesso ai dati. Ad esempio, è possibile compilare le classi di accesso ai dati utilizzando le classi di Microsoft Entity Framework, NHibernate, Subsonic o ADO.NET.

In questa esercitazione si usa LINQ to SQLLINQ to SQL per eseguire query e aggiornare il database. LINQ to SQLLINQ to SQL offre un metodo molto semplice per interagire con un database di Microsoft SQL Server. Tuttavia, è importante comprendere che il framework di MVC ASP.NET non è legato a LINQ to SQLLINQ to SQL in alcun modo. ASP.NET MVC è compatibile con qualsiasi tecnologia di accesso ai dati.

## <a name="create-a-movie-database"></a>Creazione di un database di filmati

In questa esercitazione, per illustrare come è possibile compilare classi modello, viene compilata una semplice applicazione di database Movie.In this tutorial -- in order to illustrate how you can build model classes -- we build a simple Movie database application. Il primo passaggio consiste nel creare un nuovo database. Fare clic con\_il pulsante destro del mouse sulla cartella Dati app nella finestra Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, Nuovo elemento**. Selezionare il modello di Database di **SQL Server,** assegnargli il nome MoviesDB.mdf e fare clic sul pulsante **Aggiungi** (vedere la Figura 1).

[![Aggiunta di un nuovo database di SQL ServerAdding a new SQL Server Database](creating-model-classes-with-linq-to-sql-cs/_static/image2.png)](creating-model-classes-with-linq-to-sql-cs/_static/image1.png)

**Figura 01**: Aggiunta di un nuovo database di SQL Server ([fare clic per visualizzare l'immagine](creating-model-classes-with-linq-to-sql-cs/_static/image3.png)a dimensioni intere )

Dopo aver creato il nuovo database, è possibile aprirlo facendo doppio clic\_sul file MoviesDB.mdf nella cartella App Data. Facendo doppio clic sul file MoviesDB.mdf si apre la finestra Esplora server (vedere la Figura 2).

La finestra Esplora server viene denominata finestra Esplora database quando si utilizza Visual Web Developer.

[![Utilizzo della finestra Esplora server](creating-model-classes-with-linq-to-sql-cs/_static/image5.png)](creating-model-classes-with-linq-to-sql-cs/_static/image4.png)

**Figura 02**: Utilizzo della finestra Esplora server ([Fare clic per visualizzare l'immagine](creating-model-classes-with-linq-to-sql-cs/_static/image6.png)a dimensioni intere )

Abbiamo bisogno di aggiungere una tabella al nostro database che rappresenta i nostri film. Fare clic con il pulsante destro del mouse sulla cartella Tabelle e selezionare l'opzione di menu **Aggiungi nuova tabella**. Se si seleziona questa opzione di menu, viene visualizzata Progettazione tabelle (vedere la Figura 3).

[![Utilizzo della finestra Esplora server](creating-model-classes-with-linq-to-sql-cs/_static/image8.png)](creating-model-classes-with-linq-to-sql-cs/_static/image7.png)

**Figura 03**: Progettazione tabelle ([Fare clic per visualizzare l'immagine a dimensioni intere](creating-model-classes-with-linq-to-sql-cs/_static/image9.png))

È necessario aggiungere le seguenti colonne alla tabella del database:

| **Nome colonna** | **Tipo di dati** | **Consenti valori NULL** |
| --- | --- | --- |
| ID | Int | False |
| Titolo | Nvarchar(200) | False |
| Responsabile | Nvarchar(50) | False |

È necessario eseguire due operazioni speciali per la colonna Id. In primo luogo, è necessario contrassegnare la colonna Id come colonna chiave primaria selezionando la colonna in Progettazione tabelle e facendo clic sull'icona di una chiave. LINQ to SQLLINQ to SQLLINQ to SQL richiede di specificare le colonne di chiave primaria quando si eseguono inserimenti o aggiornamenti nel database.

Successivamente, è necessario contrassegnare la colonna Id come colonna Identity assegnando il valore Sì alla proprietà **Is Identity** (vedere la Figura 3). Una colonna Identity è una colonna a cui viene assegnato automaticamente un nuovo numero ogni volta che si aggiunge una nuova riga di dati a una tabella.

## <a name="create-linq-to-sql-classes"></a>Creare classi LINQ to SQLCreate LINQ to SQL Classes

Il modello MVC conterrà classi LINQ to SQLLINQ to SQL che rappresentano la tabella di database tblMovie. Il modo più semplice per creare queste classi LINQ to SQLLINQ to SQL consiste nel fare clic con il pulsante destro del mouse sulla cartella Modelli, selezionare **Aggiungi, Nuovo elemento**, selezionare il modello Classi LINQ to SQL , assegnare alle classi il nome Movie.dbml e fare clic sul pulsante **Aggiungi** (vedere la Figura 4).

[![Creazione di classi LINQ to SQLCreating LINQ to SQL classes](creating-model-classes-with-linq-to-sql-cs/_static/image11.png)](creating-model-classes-with-linq-to-sql-cs/_static/image10.png)

**Figura 04**: Creazione di classi LINQ to SQL ([Fare clic per visualizzare l'immagine](creating-model-classes-with-linq-to-sql-cs/_static/image12.png)a dimensioni intere )

Immediatamente dopo aver creato le classi Movie LINQ to SQL, viene visualizzata la finestra di progettazione relazionale a oggetti. È possibile trascinare le tabelle di database dalla finestra Esplora server in Progettazione relazionale oggetti per creare classi LINQ to SQL CHE rappresentano tabelle di database specifiche. È necessario aggiungere la tabella di database tblMovie in Object Relational Designer (vedere Figura 5).

[![Utilizzo di Progettazione relazionale oggetti](creating-model-classes-with-linq-to-sql-cs/_static/image14.png)](creating-model-classes-with-linq-to-sql-cs/_static/image13.png)

**Figura 05**: Utilizzo di Object Relational Designer ([Fare clic per visualizzare l'immagine](creating-model-classes-with-linq-to-sql-cs/_static/image15.png)a dimensioni intere )

Per impostazione predefinita, Progettazione relazionale oggetti crea una classe con lo stesso nome della tabella di database trascinata nella finestra di progettazione. Tuttavia, non vogliamo chiamare `tblMovie`la nostra classe . Pertanto, fare clic sul nome della classe nella finestra di progettazione e modificare il nome della classe in Movie.

Infine, ricordarsi di fare clic sul pulsante **Salva** (l'immagine del dischetto) per salvare le classi LINQ to SQLLINQ to SQL. In caso contrario, le classi LINQ to SQLLINQ to SQL non verranno generate da Object Relational Designer.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Utilizzo di LINQ to SQL in un'azione del controllerUsing LINQ to SQL in a Controller Action

Ora che abbiamo le nostre classi LINQ to SQLLINQ to SQL, possiamo usare queste classi per recuperare i dati dal database. In questa sezione viene illustrato come usare le classi LINQ to SQLLINQ to SQL direttamente all'interno di un'azione del controller. Verrà visualizzato l'elenco di filmati dalla tabella di database tblMovies in una visualizzazione MVC.

In primo luogo, è necessario modificare la classe HomeController.First, we need to modify the HomeController class. Questa classe è disponibile nella cartella Controllers dell'applicazione. Modificare la classe in modo che abbia l'aspetto della classe nel listato 1.

**Lista torto 1 –`Controllers\HomeController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample1.cs)]

L'azione `Index()` nel listato 1 usa una `MovieDataContext`classe LINQ `MoviesDB` to SQL Linq to SQL DataContext (la ) per rappresentare il database. La `MoveDataContext` classe è stata generata da Progettazione relazionale oggetti di Visual Studio.

Viene eseguita una query LINQ su DataContext per `tblMovies` recuperare tutti i filmati dalla tabella di database. L'elenco dei filmati viene assegnato `movies`a una variabile locale denominata . Infine, l'elenco dei filmati viene passato alla visualizzazione tramite i dati di visualizzazione.

Per mostrare i filmati, è necessario modificare la vista Indice. È possibile trovare la `Views\Home\` vista Indice nella cartella. Aggiornare la vista Indice in modo che abbia l'aspetto della vista nel listato 2.

**Listato 2 –`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample2.aspx)]

Si noti che la `<%@ import namespace %>` vista indice modificata include una direttiva nella parte superiore della vista. La presente direttiva importa il `MvcApplication1.Models namespace`file . È necessario questo spazio dei `model` nomi per lavorare `Movie` con le classi, in particolare la classe, nella visualizzazione.

La visualizzazione nel listato 2 contiene un `foreach` ciclo che `ViewData.Model` scorre tutti gli elementi rappresentati dalla proprietà. Il valore `Title` della proprietà viene `movie`visualizzato per ogni oggetto .

Si noti che `ViewData.Model` viene eseguito `IEnumerable`il cast del valore della proprietà su un oggetto . Ciò è necessario per scorrere `ViewData.Model`il contenuto di . Un'altra opzione consiste nel creare `view`un file fortemente tipizzato. Quando si crea un `view`oggetto fortemente `ViewData.Model` tipizzato , si esegue il cast della proprietà su un determinato tipo nella classe code-behind di una visualizzazione.

Se si esegue l'applicazione dopo aver modificato la `HomeController` classe e la visualizzazione indice, si otterrà una pagina vuota. Si otterrà una pagina vuota perché non `tblMovies` sono presenti record di film nella tabella di database.

Per aggiungere record alla `tblMovies` tabella di database, `tblMovies` fare clic con il pulsante destro del mouse sulla tabella di database nella finestra Esplora server (finestra Esplora database in Visual Web Developer) e selezionare l'opzione di menu Mostra dati tabella. È possibile `movie` inserire record utilizzando la griglia visualizzata (vedere la figura 6).

[![Inserimento di filmati](creating-model-classes-with-linq-to-sql-cs/_static/image17.png)](creating-model-classes-with-linq-to-sql-cs/_static/image16.png)

**Figura 06**: Inserimento di filmati([Fare clic per visualizzare l'immagine a grandezza naturale](creating-model-classes-with-linq-to-sql-cs/_static/image18.png))

Dopo aver aggiunto alcuni `tblMovies` record di database alla tabella e si esegue l'applicazione, verrà visualizzata la pagina in Figura 7. Tutti i record del database dei filmati vengono visualizzati in un elenco puntato.

[![Visualizzazione di filmati con la vista Indice](creating-model-classes-with-linq-to-sql-cs/_static/image20.png)](creating-model-classes-with-linq-to-sql-cs/_static/image19.png)

**Figura 07**: Visualizzazione dei filmati con la vista Indice ([Fare clic per visualizzare l'immagine](creating-model-classes-with-linq-to-sql-cs/_static/image21.png)a dimensioni intere)

## <a name="using-the-repository-pattern"></a>Utilizzo del modello di repositoryUsing the Repository Pattern

Nella sezione precedente sono stati usati LINQ to SQLLINQ to SQL direttamente all'interno di un'azione del controller. Abbiamo usato `MovieDataContext` la classe `Index()` direttamente dall'azione del controller. Non c'è niente di sbagliato nel fare questo nel caso di una semplice applicazione. Tuttavia, l'utilizzo diretto di LINQ to SQLLINQ to SQL in una classe controller crea problemi quando è necessario compilare un'applicazione più complessa.

L'utilizzo di LINQ to SQLLINQ to SQL all'interno di una classe controller rende difficile cambiare le tecnologie di accesso ai dati in futuro. Ad esempio, è possibile decidere di passare dall'utilizzo di Microsoft LINQ to SQL all'utilizzo di Microsoft Entity Framework come tecnologia di accesso ai dati. In tal caso, è necessario riscrivere ogni controller che accede al database all'interno dell'applicazione.

L'utilizzo di LINQ to SQLLINQ to SQL all'interno di una classe controller rende inoltre difficile la compilazione di unit test per l'applicazione. In genere, non si desidera interagire con un database quando si eseguono unit test. Si desidera utilizzare gli unit test per testare la logica dell'applicazione e non il server di database.

Per compilare un'applicazione MVC più adattabile alle modifiche future e che può essere testata più facilmente, è consigliabile usare il modello Repository. Quando si usa il modello Repository, si crea una classe di repository separata che contiene tutta la logica di accesso al database.

Quando si crea la classe di repository, si crea un'interfaccia che rappresenta tutti i metodi utilizzati dalla classe di repository. All'interno dei controller, si scrive il codice sull'interfaccia anziché sul repository. In questo modo, è possibile implementare il repository utilizzando diverse tecnologie di accesso ai dati in futuro.

L'interfaccia nel listato 3 è denominata `IMovieRepository` e rappresenta un singolo metodo denominato `ListAll()`.

**Lista 3 –`Models\IMovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample3.cs)]

La classe repository nel `IMovieRepository` listato 4 implementa l'interfaccia. Si noti che `ListAll()` contiene un metodo denominato `IMovieRepository` che corrisponde al metodo richiesto dall'interfaccia.

**Lista 4 –`Models\MovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample4.cs)]

Infine, `MoviesController` la classe nel listato 5 utilizza il modello Repository.Finally, the class in Listing 5 uses the Repository pattern. Non utilizza più direttamente le classi LINQ to SQLLINQ to SQL.

**Lista 5 –`Controllers\MoviesController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample5.cs)]

Si noti che la `MoviesController` classe nel listato 5 dispone di due costruttori. Il primo costruttore, il costruttore senza parametri, viene chiamato quando l'applicazione è in esecuzione. Questo costruttore crea `MovieRepository` un'istanza della classe e la passa al secondo costruttore.

Il secondo costruttore ha `IMovieRepository` un solo parametro: un parametro. Questo costruttore assegna semplicemente il valore del parametro `_repository`a un campo a livello di classe denominato .

La `MoviesController` classe sfrutta un modello di progettazione software denominato modello di inserimento delle dipendenze. In particolare, sta usando qualcosa chiamato Constructor Dependency Injection. Puoi leggere di più su questo modello leggendo il seguente articolo di Martin Fowler:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Si noti che tutto `MoviesController` il codice nella classe (ad eccezione `IMovieRepository` del primo `MovieRepository` costruttore) interagisce con l'interfaccia anziché con la classe effettiva. Il codice interagisce con un'interfaccia astratta anziché con un'implementazione concreta dell'interfaccia.

Se si desidera modificare la tecnologia di accesso ai `IMovieRepository` dati utilizzata dall'applicazione, è sufficiente implementare l'interfaccia con una classe che utilizza la tecnologia di accesso al database alternativa. Ad esempio, è `EntityFrameworkMovieRepository` possibile creare `SubSonicMovieRepository` una classe o una classe. Poiché la classe controller è programmata in base `IMovieRepository` all'interfaccia, è possibile passare una nuova implementazione di alla classe controller e la classe continuerà a funzionare.

Inoltre, se si desidera `MoviesController` testare la classe, allora è `HomeController`possibile passare una classe di repository film falso per il . È possibile `IMovieRepository` implementare la classe con una classe che non accede effettivamente al database ma contiene tutti i metodi necessari dell'interfaccia. `IMovieRepository` In questo modo, è `MoviesController` possibile eseguire unit test della classe senza accedere effettivamente a un database reale.

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione era dimostrare come è possibile creare classi modello MVC sfruttando Microsoft LINQ to SQL. Sono state esaminate due strategie per la visualizzazione dei dati del database in un'applicazione MVC ASP.NET. In primo luogo, abbiamo creato LINQ to SQLLINQ to SQL classi e utilizzato le classi direttamente all'interno di un'azione del controller. L'utilizzo di classi LINQ to SQL all'interno di un controller consente di visualizzare rapidamente e facilmente i dati del database in un'applicazione MVC.

Successivamente, abbiamo esplorato un percorso leggermente più difficile, ma sicuramente più virtuoso, per la visualizzazione dei dati del database. Abbiamo sfruttato il modello Repository e inserito tutta la logica di accesso al database in una classe di repository separata. Nel controller, abbiamo scritto tutto il codice su un'interfaccia anziché una classe concreta. Il vantaggio del modello Repository è che ci consente di modificare facilmente le tecnologie di accesso al database in futuro e ci consente di testare facilmente le classi di controller.

> [!div class="step-by-step"]
> [Successivo](creating-model-classes-with-the-entity-framework-cs.md)
> [precedente](displaying-a-table-of-database-data-cs.md)
