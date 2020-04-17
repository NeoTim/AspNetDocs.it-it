---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
title: Creazione di classi di modello con Entity Framework (C Documenti Microsoft
author: rick-anderson
description: In questa esercitazione si apprenderà come usare ASP.NET MVC con Microsoft Entity Framework. Informazioni su come utilizzare la Creazione guidata entità per creare un'entità ADO.NET Da...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 61644169-e8b1-45dd-bf96-9c2301b69879
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
msc.type: authoredcontent
ms.openlocfilehash: 716d34167258b1005b25b1cd11bfaa6d80763320
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542664"
---
# <a name="creating-model-classes-with-the-entity-framework-c"></a>Creazione di classi di modelli con Entity Framework (C#)

da parte [di Microsoft](https://github.com/microsoft)

> In questa esercitazione si apprenderà come usare ASP.NET MVC con Microsoft Entity Framework. Viene illustrato come utilizzare la Creazione guidata entità per creare un ADO.NET Entity Data Model. Nel corso di questa esercitazione viene compilata un'applicazione Web che illustra come selezionare, inserire, aggiornare ed eliminare i dati del database tramite Entity Framework.

L'obiettivo di questa esercitazione è spiegare come è possibile creare classi di accesso ai dati utilizzando Microsoft Entity Framework durante la compilazione di un'applicazione MVC ASP.NET. In questa esercitazione si presuppone che non sia stata previa conoscenza di Microsoft Entity Framework. Alla fine di questa esercitazione, si capirà come utilizzare Entity Framework per selezionare, inserire, aggiornare ed eliminare i record del database.

Microsoft Entity Framework è uno strumento O/RM (Object Relational Mapping) che consente di generare automaticamente un livello di accesso ai dati da un database. Entity Framework consente di evitare il noioso lavoro di compilazione delle classi di accesso ai dati a mano.

Per illustrare come è possibile utilizzare Microsoft Entity Framework con ASP.NET MVC, verrà compilata una semplice applicazione di esempio. Verrà creata un'applicazione Movie Database che consente di visualizzare e modificare i record del database dei film.

In questa esercitazione si presuppone che si dispone di Visual Studio 2008 o Visual Web Developer 2008 con Service Pack 1. È necessario il Service Pack 1 per utilizzare Entity Framework. È possibile scaricare Visual Studio 2008 Service Pack 1 o Visual Web Developer con Service Pack 1 dal seguente indirizzo:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)

> [!NOTE] 
> 
> Non esiste una connessione essenziale tra ASP.NET MVC e Microsoft Entity Framework. Esistono diverse alternative a Entity Framework che è possibile utilizzare con ASP.NET MVC. Ad esempio, è possibile compilare le classi del modello MVC utilizzando altri strumenti O/RM, ad esempio Microsoft LINQ to SQL, NHibernate o SubSonic.For example, you can build your MVC Model classes using other O/RM tools such as Microsoft LINQ to SQL, NHibernate, or SubSonic.

## <a name="creating-the-movie-sample-database"></a>Creazione del database di esempio di filmatoCreating the Movie Sample Database

L'applicazione Movie Database utilizza una tabella di database denominata Movies che contiene le seguenti colonne:

| Nome colonna | Tipo di dati | Consentire valori Null? | La chiave primaria è? |
| --- | --- | --- | --- |
| ID | INT | False | True |
| Titolo | nvarchar(100) | False | False |
| Responsabile | nvarchar(100) | False | False |

È possibile aggiungere questa tabella a un progetto MVC ASP.NET attenendosi alla seguente procedura:

1. Fare clic con\_il pulsante destro del mouse sulla cartella Dati app nella finestra Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, Nuovo elemento.**
2. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare Database di SQL **Server**, assegnare al database il nome MoviesDB.mdf e fare clic sul pulsante **Aggiungi.**
3. Fare doppio clic sul file MoviesDB.mdf per aprire la finestra Esplora server/Esplora database.
4. Espandere la connessione al database MoviesDB.mdf, fare clic con il pulsante destro del mouse sulla cartella Tabelle e selezionare l'opzione di menu **Aggiungi nuova tabella**.
5. In Progettazione tabelle aggiungere le colonne Id, Titolo e Director.
6. Fare clic sul pulsante **Salva** (con l'icona del dischetto) per salvare la nuova tabella con il nome Film.

Dopo aver creato la tabella di database Movies, è necessario aggiungere alcuni dati di esempio alla tabella. Fare clic con il pulsante destro del mouse sulla tabella Film e selezionare l'opzione di menu **Mostra dati tabella**. È possibile immettere dati di film falsi nella griglia visualizzata.

## <a name="creating-the-adonet-entity-data-model"></a>Creazione del ADO.NET Entity Data Model

Per utilizzare Entity Framework, è necessario creare un Entity Data Model. È possibile sfruttare la procedura guidata Di Visual Studio *Entity Data Model* per generare automaticamente un Entity Data Model da un database.

A tale scopo, seguire questa procedura:

1. Fare clic con il pulsante destro del mouse sulla cartella Modelli nella finestra Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, Nuovo elemento**.
2. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare la categoria Dati (vedere la figura 1).
3. Selezionare il modello **ADO.NET Entity Data Model,** assegnare a Entity Data Model il nome MoviesDBModel.edmx e fare clic sul pulsante **Aggiungi.** Facendo clic sul pulsante **Aggiungi** viene avviata la Creazione guidata modello di dati.
4. Nel passaggio **Scegli contenuto modello** scegliere l'opzione Genera da un **database** e fare clic sul pulsante **Avanti** (vedere la Figura 2).
5. Nel passaggio Scegliere la **connessione dati** selezionare la connessione al database MoviesDB.mdf, immettere il nome delle impostazioni di connessione delle entità MoviesDBEntities e fare clic sul pulsante **Avanti** (vedere la figura 3).
6. Nel passaggio Scegli oggetti di **database** selezionare la tabella di database Movie e fare clic sul pulsante **Fine** (vedere la Figura 4).

Dopo aver completato questi passaggi, verrà aperto ADO.NET Entity Data Model Designer (Entity Designer).

**Figura 1 – Creazione di un nuovo Entity Data Model**

![clip_image002](creating-model-classes-with-the-entity-framework-cs/_static/image1.jpg)

**Figura 2 – Scegliere il passaggio del contenuto del modello**

![clip_image004](creating-model-classes-with-the-entity-framework-cs/_static/image2.jpg)

**Figura 3 – Scegliere la connessione dati**

![clip_image006](creating-model-classes-with-the-entity-framework-cs/_static/image3.jpg)

**Figura 4 – Scegliere gli oggetti di databaseFigure 4 – Choose Your Database Objects**

![clip_image008](creating-model-classes-with-the-entity-framework-cs/_static/image4.jpg)

#### <a name="modifying-the-adonet-entity-data-model"></a>Modifica del modello Entity Data Model ADO.NET

Dopo aver creato un Entity Data Model, è possibile modificare il modello sfruttando Entity Designer (vedere La Figura 5). È possibile aprire Entity Designer in qualsiasi momento facendo doppio clic sul file MoviesDBModel.edmx contenuto nella cartella Models all'interno della finestra Esplora soluzioni.

**Figura 5 – Il ADO.NET Entity Data Model Designer**

![clip_image010](creating-model-classes-with-the-entity-framework-cs/_static/image5.jpg)

Ad esempio, è possibile utilizzare Entity Designer per modificare i nomi delle classi generate dalla Creazione guidata dati entity Model. La procedura guidata ha creato una nuova classe di accesso ai dati denominata Movies.The Wizard created a new data access class named Movies. In altre parole, la procedura guidata ha assegnato alla classe lo stesso nome della tabella di database. Poiché useremo questa classe per rappresentare una particolare istanza Movie, è necessario rinominare la classe da Movies a Movie.

Se si desidera rinominare una classe di entità, è possibile fare doppio clic sul nome della classe in Entity Designer e immettere un nuovo nome (vedere Figura 6). In alternativa, è possibile modificare il nome di un'entità nella finestra Proprietà dopo aver selezionato un'entità in Entity Designer.

**Figura 6 – Modifica del nome di un'entità**

![clip_image012](creating-model-classes-with-the-entity-framework-cs/_static/image6.jpg)

Ricordarsi di salvare Entity Data Model dopo aver apportato una modifica facendo clic sul pulsante Salva (l'icona del disco floppy). Dietro le quinte, Entity Designer genera un set di classi C . È possibile visualizzare queste classi aprendo il file di MoviesDBModel.Designer.cs dalla finestra Esplora soluzioni.

Non modificare il codice nel file di Designer.cs poiché le modifiche verranno sovrascritte al successivo utilizzo di Entity Designer. Se si desidera estendere la funzionalità delle classi di entità definite nel file Designer.cs, è possibile creare *classi parziali* in file separati.

#### <a name="selecting-database-records-with-the-entity-framework"></a>Selezione di record di database con Entity Framework

Iniziamo a creare l'applicazione Movie Database creando una pagina che visualizza un elenco di record di film. Il controller Home nel listato 1 espone un'azione denominata Index(). L'azione Index() restituisce tutti i record del filmato dalla tabella di database Movie sfruttando Entity Framework.

**Listato 1 – Controller-HomeController.cs**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample1.cs)]

Si noti che il controller nel listato 1 include un costruttore. Il costruttore inizializza un campo \_a livello di classe denominato db. Il \_campo db rappresenta le entità di database generate da Microsoft Entity Framework. Il \_campo db è un'istanza della classe MoviesDBEntities generata da Entity Designer.

Per utilizzare la classe MoviesDBEntities nel controller Home, è necessario importare lo spazio dei nomi MovieEntityApp.Models (*MVCProjectName*. modelli).

Il \_campo db viene utilizzato all'interno dell'azione Index() per recuperare i record dalla tabella di database Movies. Espressione \_db. MovieSet rappresenta tutti i record della tabella di database Movies. Il metodo ToList() viene utilizzato per convertire l'insieme di&lt;filmati&gt;in una raccolta generica di oggetti Movie (List Movie ).

I record del filmato vengono recuperati con l'aiuto di LINQ to Entities. L'azione Index() nel listato 1 utilizza la *sintassi* del metodo LINQ per recuperare il set di record di database. Se si preferisce, è possibile utilizzare la *sintassi* di query LINQ. Le due istruzioni seguenti fanno la stessa cosa:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample2.cs)]

Utilizzare la sintassi LINQ, ovvero la sintassi del metodo o la sintassi della query, che si ritiene più intuitiva. Non c'è differenza di prestazioni tra i due approcci - l'unica differenza è lo stile.

La vista nel listato 2 viene utilizzata per visualizzare i record del filmato.

**Listato 2 – Visualizzazioni, Home, Indice.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample3.aspx)]

La visualizzazione nel listato 2 contiene un ciclo **foreach** che scorre ogni record di filmato e visualizza i valori delle proprietà Title e Director del record del filmato. Si noti che accanto a ogni record viene visualizzato un collegamento Modifica ed Elimina. Inoltre, un collegamento Aggiungi filmato viene visualizzato nella parte inferiore della visualizzazione (vedere Figura 7).

**Figura 7 – Vista dell'indice**

![clip_image014](creating-model-classes-with-the-entity-framework-cs/_static/image7.jpg)

La vista Indice è una *vista tipizzata.* La visualizzazione Indice &lt;include una&gt; direttiva % % di pagina con un attributo *Inherits* che esegue&lt;il cast della proprietà Model in una raccolta di oggetti List generica fortemente tipizzata di oggetti Movie (List Movie).

## <a name="inserting-database-records-with-the-entity-framework"></a>Inserimento di record di database con Entity Framework

È possibile utilizzare Entity Framework per semplificare l'inserimento di nuovi record in una tabella di database. Il listato 3 contiene due nuove azioni aggiunte alla classe Controller Home che è possibile utilizzare per inserire nuovi record nella tabella di database Movie.

**Listato 3 – Controllers-HomeController.cs (metodi di aggiunta)Listing 3 – Controllers-HomeController.cs (Add methods)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample4.cs)]

La prima azione Add() restituisce semplicemente una visualizzazione. La visualizzazione contiene un modulo per l'aggiunta di un nuovo record di database di film (vedere Figura 8). Quando si invia il modulo, viene richiamata la seconda azione Add().

Si noti che la seconda azione Add() è decorata con l'attributo AcceptVerbs. Questa azione può essere richiamata solo quando si esegue un'operazione HTTP POST. In altre parole, questa azione può essere richiamata solo quando si pubblica un modulo HTML.

La seconda azione Add() crea una nuova istanza della classe Entity Framework Movie con l'aiuto del metodo ASP.NET metodo MVC TryUpdateModel(). Il metodo TryUpdateModel() accetta i campi di FormCollection passati al metodo Add() e assegna i valori di questi campi modulo HTML alla classe Movie.

Quando si usa Entity Framework, è necessario fornire una "white list" di proprietà quando si usano i metodi TryUpdateModel o UpdateModel per aggiornare le proprietà di una classe di entità.

Successivamente, l'azione Add() esegue una semplice convalida del form. L'azione verifica che entrambe le proprietà Title e Director abbiano valori. Se si verifica un errore di convalida, viene aggiunto un messaggio di errore di convalida a ModelState.If there is a validation error, then a validation error message is added to ModelState.

Se non sono presenti errori di convalida, un nuovo record di filmato viene aggiunto alla tabella di database Movies con l'aiuto di Entity Framework. Il nuovo record viene aggiunto al database con le due righe di codice seguenti:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample5.cs)]

La prima riga di codice aggiunge la nuova entità Movie al set di film rilevati da Entity Framework. La seconda riga di codice salva tutte le modifiche apportate ai film registrati nel database sottostante.

**Figura 8 – La visualizzazione Aggiungi**

![clip_image016](creating-model-classes-with-the-entity-framework-cs/_static/image8.jpg)

#### <a name="updating-database-records-with-the-entity-framework"></a>Aggiornamento dei record di database con Entity Framework

È possibile seguire quasi lo stesso approccio per modificare un record di database con Entity Framework come approccio appena seguito per inserire un nuovo record di database. Il listato 4 contiene due nuove azioni controller denominate Edit(). La prima azione Edit() restituisce un modulo HTML per la modifica di un record del filmato. La seconda azione Edit() tenta di aggiornare il database.

**Listato 4 – Controllers-HomeController.cs (metodi di modifica)Listing 4 – Controllers-HomeController.cs (Edit methods)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample6.cs)]

La seconda azione Edit() inizia recuperando il record Movie dal database che corrisponde all'ID del filmato in fase di modifica. The following LINQ to Entities statement grabs the first database record that matches a particular Id:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample7.cs)]

Successivamente, il metodo TryUpdateModel() viene utilizzato per assegnare i valori dei campi modulo HTML alle proprietà dell'entità filmato. Si noti che viene fornita una white list per specificare le proprietà esatte da aggiornare.

Successivamente, viene eseguita una convalida semplice per verificare che entrambe le proprietà Titolo filmato e Director abbiano valori. Se in una proprietà manca un valore, viene aggiunto un messaggio di errore di convalida a ModelState e ModelState.IsValid restituisce il valore false.

Infine, se non sono presenti errori di convalida, la tabella di database Movies sottostante viene aggiornata con le modifiche chiamando il SaveChanges() metodo.

Quando si modificano i record del database, è necessario passare l'ID del record in corso di modifica all'azione del controller che esegue l'aggiornamento del database. In caso contrario, l'azione del controller non saprà quale record aggiornare nel database sottostante. La visualizzazione Modifica, contenuta nel listato 5, include un campo modulo nascosto che rappresenta l'ID del record del database in corso di modifica.

**Listato 5 – Visualizzazioni, Home, Modifica.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Eliminazione di record di database con Entity Framework

L'operazione finale del database, che è necessario affrontare in questa esercitazione, consiste nell'eliminazione dei record del database. È possibile utilizzare l'azione del controller nel listato 6 per eliminare un record di database specifico.

**Listato 6 -- - Controllers , HomeController.cs (azione Elimina)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample9.cs)]

L'azione Delete() recupera innanzitutto l'entità Movie che corrisponde all'ID passato all'azione. Successivamente, il filmato viene eliminato dal database chiamando il metodo DeleteObject() seguito dal metodo SaveChanges(). Infine, l'utente viene reindirizzato alla visualizzazione indice.

## <a name="summary"></a>Riepilogo

Lo scopo di questa esercitazione era dimostrare come è possibile compilare applicazioni Web basate su database sfruttando ASP.NET MVC e Microsoft Entity Framework. Si è appreso come creare un'applicazione che consente di selezionare, inserire, aggiornare ed eliminare record di database.

In primo luogo, è stato illustrato come è possibile utilizzare la procedura guidata Entity Data Model per generare un Entity Data Model da Visual Studio. Successivamente, si apprenderà come utilizzare LINQ to Entities per recuperare un set di record di database da una tabella di database. Infine, è stato usato Entity Framework per inserire, aggiornare ed eliminare i record del database.

> [!div class="step-by-step"]
> [Avanti](creating-model-classes-with-linq-to-sql-cs.md)
