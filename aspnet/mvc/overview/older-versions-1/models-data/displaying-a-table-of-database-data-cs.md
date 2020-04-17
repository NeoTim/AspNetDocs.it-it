---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Visualizzazione di una tabella di dati di database (c Documenti Microsoft
author: rick-anderson
description: In questa esercitazione vengono illustrati due metodi per visualizzare un set di record di database. Mostro due metodi di formattazione di un set di record di database in un HTML ta...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 3fca8ac9e9b4e549ca76ffa282cc03aa4cc50e88
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542054"
---
# <a name="displaying-a-table-of-database-data-c"></a>Visualizzazione di una tabella di dati del database (C#)

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> In questa esercitazione vengono illustrati due metodi per visualizzare un set di record di database. Mostradue metodi di formattazione di un set di record di database in una tabella HTML. In primo luogo, vi mostro come è possibile formattare i record del database direttamente all'interno di una vista. Successivamente, viene illustrato come è possibile sfruttare i parziali durante la formattazione dei record del database.

L'obiettivo di questa esercitazione è spiegare come è possibile visualizzare una tabella HTML di dati di database in un'applicazione MVC ASP.NET. In primo luogo, si apprenderà come utilizzare gli strumenti di scaffolding inclusi in Visual Studio per generare una visualizzazione che visualizza automaticamente un set di record. Successivamente, si apprenderà come utilizzare un partial come modello durante la formattazione dei record del database.

## <a name="create-the-model-classes"></a>Creare le classi del modelloCreate the Model Classes

Verrà visualizzato il set di record dalla tabella di database Movies. La tabella del database Movies contiene le seguenti colonne:

<a id="0.3_table01"></a>

| **Nome colonna** | **Tipo di dati** | **Consenti valori NULL** |
| --- | --- | --- |
| ID | Int | False |
| Titolo | Nvarchar(200) | False |
| Responsabile | NVarchar(50) | False |
| Data di rilascio | Datetime | False |

Per rappresentare la tabella Movies nell'applicazione MVC ASP.NET, è necessario creare una classe modello. In questa esercitazione viene usato Microsoft Entity Framework per creare le classi del modello.

> [!NOTE] 
> 
> In questa esercitazione viene usato Microsoft Entity Framework. Tuttavia, è importante comprendere che è possibile utilizzare un'ampia gamma di tecnologie diverse per interagire con un database da un'applicazione MVC ASP.NET, tra cui LINQ to SQL, NHibernate o ADO.NET.

Per avviare la procedura guidata Entity Data Model, attenersi alla seguente procedura:

1. Fare clic con il pulsante destro del mouse sulla cartella Modelli nella finestra Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, Nuovo elemento**.
2. Selezionare la categoria **Dati** e selezionare il modello **ADO.NET Entity Data Model.**
3. Assegnare al modello di dati il nome *MoviesDBModel.edmx* e fare clic sul pulsante **Aggiungi.**

Dopo aver fatto clic sul pulsante Aggiungi , viene visualizzata la procedura guidata Entity Data Model (vedere Figura 1). Per completare la procedura guidata, attenersi alla seguente procedura:

1. Nel passaggio **Scegli contenuto modello** selezionare l'opzione Genera da **database.**
2. Nel passaggio Scegliere la **connessione dati** usare la connessione dati *MoviesDB.mdf* e il nome *MoviesDBEntities* per le impostazioni di connessione. Fare clic sul pulsante **Avanti**.
3. Nel passaggio Scegli oggetti di **database** espandere il nodo Tabelle e selezionare la tabella Film. Immettere i *modelli* dello spazio dei nomi e fare clic sul pulsante **Fine.**

[![Creazione di classi LINQ to SQLCreating LINQ to SQL classes](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)

**Figura 01:** creazione di classi LINQ to SQL ([Fare clic per visualizzare l'immagine](displaying-a-table-of-database-data-cs/_static/image2.png)a dimensioni intere )

Dopo aver completato la procedura guidata Entity Data Model, verrà aperto Entity Data Model Designer. La finestra di progettazione deve visualizzare l'entità Movies (vedere Figura 2).

[![Progettazione Entity Data Model](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)

**Figura 02**: Entity Data Model Designer ([Fare clic per visualizzare l'immagine a dimensioni intere)](displaying-a-table-of-database-data-cs/_static/image4.png)

Dobbiamo fare un cambiamento prima di continuare. La procedura guidata Entity Data genera una classe modello denominata *Movies* che rappresenta la tabella di database Movies. Poiché useremo la classe Movies per rappresentare un particolare filmato, è necessario modificare il nome della classe in modo che sia *Movie* anziché *Movies* (singolare anziché plurale).

Fare doppio clic sul nome della classe nell'area di progettazione e modificare il nome della classe da Movies a Movie. Dopo aver apportato questa modifica, fare clic sul pulsante **Salva** (l'icona del disco floppy) per generare la classe Movie.

## <a name="create-the-movies-controller"></a>Creare il controller Filmati

Ora che abbiamo un modo per rappresentare i nostri record di database, possiamo creare un controller che restituisce la raccolta di film. Nella finestra Esplora soluzioni di Visual Studio fare clic con il pulsante destro del mouse sulla cartella Controller e selezionare l'opzione di menu **Aggiungi, Controller** (vedere la Figura 3).

[![Il menu Aggiungi controller](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)

**Figura 03**: Il menu Aggiungi controller ([Fare clic per visualizzare l'immagine a dimensioni intere](displaying-a-table-of-database-data-cs/_static/image6.png))

Quando viene visualizzata la finestra di dialogo **Aggiungi Controller,** immettere il nome del controller MovieController (vedere Figura 4). Fare clic sul pulsante **Aggiungi** per aggiungere il nuovo controller.

[![Finestra di dialogo Aggiungi controller](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)

**Figura 04**: La finestra di dialogo Aggiungi controller[(Fare clic per visualizzare l'immagine](displaying-a-table-of-database-data-cs/_static/image8.png)a dimensioni intere)

È necessario modificare l'azione Index() esposta dal controller Movie in modo che restituisca il set di record del database. Modificare il controller in modo che abbia l'aspetto del controller nel listato 1.

**Listato 1 – Controller-MovieController.cs**

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

Nel listato 1, la classe MoviesDBEntities viene utilizzata per rappresentare il database MoviesDB. Per utilizzare questa classe, è necessario importare lo spazio dei nomi MvcApplication1.Models in questo modo:To use this class, you need to import the MvcApplication1.Models namespace like this:

utilizzo di MvcApplication1.Models;

Entità *dell'espressione. MovieSet.ToList()* restituisce l'insieme di tutti i filmati dalla tabella di database Movies.

## <a name="create-the-view"></a>Creazione della vista

The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.

Compilare l'applicazione selezionando l'opzione di menu **Compila, Compila soluzione**. È necessario compilare l'applicazione prima di aprire la finestra di dialogo **Aggiungi visualizzazione** o le classi di dati non verranno visualizzate nella finestra di dialogo.

Fare clic con il pulsante destro del mouse sull'azione Index() e selezionare l'opzione di menu **Aggiungi vista** (vedere la Figura 5).

[![Aggiunta di una visualizzazione](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)

**Figura 05**: Aggiunta di una vista ([Fare clic per visualizzare l'immagine a dimensioni intere](displaying-a-table-of-database-data-cs/_static/image10.png))

Nella finestra di dialogo **Aggiungi visualizzazione** selezionare la casella di controllo Crea una **visualizzazione fortemente tipizzato.** Selezionare la classe Movie come classe di dati di **visualizzazione**. Selezionare *Elenco* come contenuto della **visualizzazione** (vedere la Figura 6). Selezionando queste opzioni verrà generata una visualizzazione fortemente tipata che visualizza un elenco di filmati.

[![Finestra di dialogo Aggiungi vista](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)

**Figura 06**: La finestra di dialogo Aggiungi vista[(Fare clic per visualizzare l'immagine a dimensioni intere)](displaying-a-table-of-database-data-cs/_static/image12.png)

Dopo aver fatto clic sul pulsante **Aggiungi,** la visualizzazione nel listato 2 viene generata automaticamente. Questa visualizzazione contiene il codice necessario per scorrere la raccolta di filmati e visualizzare ognuna delle proprietà di un filmato.

**Listato 2 – Visualizzazioni, Film, Indice.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

È possibile eseguire l'applicazione selezionando l'opzione di menu **Debug, Avvia debug** (o premendo il tasto F5). L'esecuzione dell'applicazione avvia Internet Explorer. Se si passa all'URL /Movie, verrà visualizzata la pagina in Figura 7.

[![Una tabella di film](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)

**Figura 07**: Una tabella di filmati([Fare clic per visualizzare l'immagine](displaying-a-table-of-database-data-cs/_static/image14.png)a grandezza naturale )

Se non ti piace nulla circa l'aspetto della griglia dei record di database in Figura 7, allora si può semplicemente modificare la vista indice. Ad esempio, è possibile modificare l'intestazione *DateReleased* in *Data di rilascio* modificando la visualizzazione Indice.

## <a name="create-a-template-with-a-partial"></a>Creare un modello con un parzialeCreate a Template with a Partial

Quando una visualizzazione diventa troppo complicata, è consigliabile iniziare a suddividere la visualizzazione in parziali. L'utilizzo di partial semplifica la comprensione e la gestione delle visualizzazioni. Creeremo un parziale che possiamo usare come modello per formattare ciascuno dei record del database dei film.

Attenersi alla seguente procedura per creare il parziale:

1. Fare clic con il pulsante destro del mouse sulla cartella Views-Movie e selezionare l'opzione di menu **Aggiungi vista**.
2. Selezionare la casella di controllo *Crea una visualizzazione parziale (.ascx)*.
3. Assegnare al MovieTemplate il nome *parziale*.
4. Selezionare la casella di controllo **Crea una visualizzazione fortemente tipizzato**.
5. Selezionare Film come *classe di dati*di visualizzazione .
6. Selezionare Vuoto come contenuto della *visualizzazione.*
7. Fare clic sul pulsante **Aggiungi** per aggiungere il parziale al progetto.

Dopo aver completato questi passaggi, modificare il MovieTemplate parziale in modo che l'aspetto sia il listato 3.After you complete these steps, modify the MovieTemplate partial to look like Listing 3.

**Listato 3 – Visualizzazioni, Film, MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

Il parziale nel listato 3 contiene un modello per una singola riga di record.

La visualizzazione Indice modificata nel listato 4 usa il MovieTemplate parziale.

**Listato 4 – Visualizzazioni, Film, Indice.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

La visualizzazione nel listato 4 contiene un ciclo foreach che scorre tutti i filmati. Per ogni filmato, il MovieTemplate parziale viene utilizzato per formattare il filmato. Il MovieTemplate viene eseguito il rendering chiamando il RenderPartial() metodo helper.

La vista Indice modificata esegue il rendering della stessa tabella HTML di record di database. Tuttavia, la vista è stata notevolmente semplificata.

Il metodo RenderPartial() è diverso dalla maggior parte degli altri metodi helper perché non restituisce una stringa. Pertanto, è necessario chiamare il &lt;metodo RenderPartial() utilizzando % Html.RenderPartial(); %&gt; invece di &lt;%: Html.RenderPartial(); %&gt;.

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione consisteva nello spiegare come visualizzare un set di record di database in una tabella HTML. In primo luogo, si è appreso come restituire un set di record di database da un'azione del controller sfruttando Microsoft Entity Framework. Successivamente, si è appreso come utilizzare lo scaffolding di Visual Studio per generare una visualizzazione che visualizza automaticamente una raccolta di elementi. Infine, si è appreso come semplificare la visualizzazione sfruttando un parziale. Si è appreso come usare un partial come modello in modo da poter formattare ogni record del database.

> [!div class="step-by-step"]
> [Successivo](creating-model-classes-with-linq-to-sql-cs.md)
> [precedente](performing-simple-validation-cs.md)
