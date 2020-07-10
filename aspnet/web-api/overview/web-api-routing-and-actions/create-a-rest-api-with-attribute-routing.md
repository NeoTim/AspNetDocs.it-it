---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Creare un'API REST con routing degli attributi in API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 6eac36767bf34857d5341188d0653e7fec7cade2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "86188850"
---
# <a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>Creare un'API REST con routing degli attributi in API Web ASP.NET 2

di [Mike Wasson](https://github.com/MikeWasson)

L'API Web 2 supporta un nuovo tipo di routing, denominato *routing degli attributi*. Per una panoramica generale del routing degli attributi, vedere [routing degli attributi nell'API Web 2](attribute-routing-in-web-api-2.md). In questa esercitazione si userà il routing degli attributi per creare un'API REST per una raccolta di libri. L'API supporterà le azioni seguenti:

| Azione | URI di esempio |
| --- | --- |
| Ottenere un elenco di tutti i libri. | /api/books |
| Ottenere un libro in base all'ID. | /api/books/1 |
| Ottenere i dettagli di un libro. | /api/books/1/details |
| Ottenere un elenco di libri per genere. | /api/books/fantasy |
| Ottenere un elenco di libri in base alla data di pubblicazione. | /API/Books/date/2013-02-16/API/Books/date/2013/02/16 (form alternativo) |
| Ottenere un elenco di libri da un particolare autore. | /api/authors/1/books |

Tutti i metodi sono di sola lettura (richieste HTTP GET).

Per il livello dati verrà usato Entity Framework. I record di libri avranno i seguenti campi:

- ID
- Titolo
- Genre
- Data di pubblicazione
- Prezzo
- Descrizione
- Autorizzazione (chiave esterna per una tabella authors)

Per la maggior parte delle richieste, tuttavia, l'API restituirà un subset di questi dati (titolo, autore e genere). Per ottenere il record completo, il client richiede `/api/books/{id}/details` .

## <a name="prerequisites"></a>Prerequisiti

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise Edition.

## <a name="create-the-visual-studio-project"></a>Creare il progetto di Visual Studio

Per iniziare, esegui Visual Studio. Scegliere **Nuovo** dal menu **File** e quindi selezionare **Progetto**.

Espandere la categoria **installato**di  >  **Visual C#** . In **Visual C#** selezionare **Web**. Nell'elenco dei modelli di progetto selezionare **applicazione Web ASP.NET (.NET Framework)**. Denominare il progetto &quot; BooksAPI &quot; .

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

Nella finestra di dialogo **nuova applicazione Web ASP.NET** selezionare il modello **vuoto** . In "Aggiungi cartelle e riferimenti principali per" selezionare la casella di controllo **API Web** . Fare clic su **OK**.

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

Viene creato un progetto Skeleton configurato per le funzionalità dell'API Web.

### <a name="domain-models"></a>Modelli di dominio

Aggiungere quindi le classi per i modelli di dominio. In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Modelli. Selezionare **Aggiungi**e quindi selezionare **classe**. Denominare la classe `Author`.

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

Sostituire il codice in Author.cs con quanto segue:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

Aggiungere ora un'altra classe denominata `Book` .

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>Aggiungere un controller API Web

In questo passaggio si aggiungerà un controller API Web che usa Entity Framework come livello dati.

Premere CTRL+MAIUSC+B per compilare il progetto. Entity Framework usa la reflection per individuare le proprietà dei modelli, quindi richiede un assembly compilato per creare lo schema del database.

In Esplora soluzioni fare clic sulla cartella Controller. Selezionare **Aggiungi**, quindi **controller**.

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

Nella finestra di dialogo **Aggiungi impalcatura** selezionare **Controller Web API 2 con azioni, usando Entity Framework**.

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

Nella finestra di dialogo **Aggiungi controller** , per **nome controller**, &quot; immettere &quot; BooksController. Selezionare la &quot; casella di controllo Use Async controller Actions &quot; . Per **classe modello**selezionare &quot; libro &quot; . Se non viene visualizzata la `Book` classe elencata nell'elenco a discesa, assicurarsi di aver compilato il progetto. Fare quindi clic sul pulsante "+".

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

Fare clic su **Aggiungi** nella finestra di dialogo **nuovo contesto dati** .

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

Fare clic su **Aggiungi** nella finestra di dialogo **Aggiungi controller** . L'impalcatura aggiunge una classe denominata `BooksController` che definisce il controller API. Viene inoltre aggiunta una classe denominata `BooksAPIContext` nella cartella Models, che definisce il contesto dei dati per Entity Framework.

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>Specificare il valore di inizializzazione del database

Dal menu Strumenti selezionare **Gestione pacchetti NuGet**, quindi selezionare **console di gestione pacchetti**.

Nella finestra Console di gestione pacchetti immettere il comando seguente:

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

Questo comando crea una cartella Migrations e aggiunge un nuovo file di codice denominato Configuration.cs. Aprire il file e aggiungere il codice seguente al `Configuration.Seed` metodo.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

Nella finestra console di gestione pacchetti digitare i comandi seguenti.

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

Questi comandi creano un database locale e richiamano il metodo Seed per popolare il database.

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>Aggiungi classi DTO

Se si esegue ora l'applicazione e si invia una richiesta GET a/API/Books/1, la risposta sarà simile alla seguente. (Il rientro è stato aggiunto per migliorare la leggibilità).

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

Al contrario, desidero che questa richiesta restituisca un subset dei campi. Desidero anche che restituisca il nome dell'autore, anziché l'ID autore. A tale scopo, i metodi controller verranno modificati in modo da restituire un oggetto DTO ( *Data Transfer Object* ) invece del modello EF. Un DTO è un oggetto progettato solo per il trasporto di dati.

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi**  |  **nuova cartella**. Denominare la cartella &quot; dto &quot; . Aggiungere una classe denominata `BookDto` alla cartella dto con la definizione seguente:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

Aggiungere un'altra classe denominata `BookDetailDto`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

Aggiornare quindi la `BooksController` classe per restituire le `BookDto` istanze. Verrà usato il metodo [Queryable. Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) per proiettare le `Book` istanze nelle `BookDto` istanze. Ecco il codice aggiornato per la classe controller.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> I `PutBook` metodi, e sono stati eliminati `PostBook` `DeleteBook` perché non sono necessari per questa esercitazione.

A questo punto, se si esegue l'applicazione e si richiede/API/Books/1, il corpo della risposta sarà simile al seguente:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>Aggiungere gli attributi di route

Si convertirà quindi il controller in modo da usare il routing degli attributi. In primo luogo, aggiungere un attributo **RoutePrefix** al controller. Questo attributo definisce i segmenti URI iniziali per tutti i metodi del controller.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

Aggiungere quindi gli attributi **[route]** alle azioni del controller, come indicato di seguito:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

Il modello di route per ogni metodo controller è il prefisso più la stringa specificata nell'attributo **Route** . Per il `GetBook` metodo, il modello di route include la stringa con parametri &quot; {ID: int} &quot; , che corrisponde a se il segmento URI contiene un valore integer.

| Metodo | Modello di route | URI di esempio |
| --- | --- | --- |
| `GetBooks` | "API/libri" | `http://localhost/api/books` |
| `GetBook` | "API/Books/{ID: int}" | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>Ottenere i dettagli del libro

Per ottenere i dettagli del libro, il client invierà una richiesta GET a `/api/books/{id}/details` , dove *{ID}* è l'ID del libro.

Aggiungere il metodo seguente alla classe `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

Se si richiede `/api/books/1/details` , la risposta ha un aspetto simile al seguente:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>Ottenere libri per genere

Per ottenere un elenco di libri in un genere specifico, il client invierà una richiesta GET a `/api/books/genre` , dove *genre* è il nome del genere. ad esempio `/api/books/fantasy`.

Aggiungere il metodo seguente a `BooksController` .

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

Qui viene definita una route che contiene un parametro {genre} nel modello URI. Si noti che l'API Web è in grado di distinguere questi due URI e indirizzarli a metodi diversi:

`/api/books/1`

`/api/books/fantasy`

Questo perché il `GetBook` metodo include un vincolo che il segmento "ID" deve essere un valore integer:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

Se si richiede/API/Books/Fantasy, la risposta ha un aspetto simile al seguente:

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>Ottenere libri per autore

Per ottenere un elenco di libri per uno specifico autore, il client invierà una richiesta GET a `/api/authors/id/books` , dove *ID* è l'ID dell'autore.

Aggiungere il metodo seguente a `BooksController` .

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

Questo esempio è interessante perché &quot; &quot; i libri sono trattati come una risorsa figlio degli &quot; autori &quot; . Questo modello è piuttosto comune nelle API RESTful.

La tilde (~) nel modello di route sostituisce il prefisso della route nell'attributo **RoutePrefix** .

## <a name="get-books-by-publication-date"></a>Ottenere i libri in base alla data di pubblicazione

Per ottenere un elenco di libri in base alla data di pubblicazione, il client invierà una richiesta GET a `/api/books/date/yyyy-mm-dd` , dove *aaaa-mm-gg* è la data.

Ecco un modo per eseguire questa operazione:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

Il `{pubdate:datetime}` parametro è vincolato in modo da corrispondere a un valore **DateTime** . Questa operazione funziona, ma è effettivamente più permissiva di quanto si desideri. Ad esempio, questi URI corrisponderanno anche alla route:

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

Non c'è niente di sbagliato per consentire questi URI. Tuttavia, è possibile limitare la route a un particolare formato aggiungendo un vincolo di espressione regolare al modello di route:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

A questo punto, solo le date nel formato &quot; aaaa-mm-gg &quot; corrisponderanno. Si noti che non viene usata l'espressione regolare per verificare che sia stata ottenuta una data reale. Gestito quando l'API Web tenta di convertire il segmento URI in un'istanza **DateTime** . Una data non valida, ad esempio "2012-47-99", non verrà convertita e il client riceverà un errore 404.

È anche possibile supportare un separatore di barra ( `/api/books/date/yyyy/mm/dd` ) aggiungendo un altro attributo **[route]** con un'espressione regolare diversa.

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

Qui è disponibile un sottile ma importante dettaglio. Il secondo modello di route ha un carattere jolly ( \* ) all'inizio del parametro {pubDate}:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

Indica al motore di routing che {pubdate} deve corrispondere al resto dell'URI. Per impostazione predefinita, un parametro di modello corrisponde a un singolo segmento URI. In questo caso, è necessario che {pubdate} estenda diversi segmenti URI:

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>Codice controller

Ecco il codice completo per la classe BooksController.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>Riepilogo

Il routing degli attributi offre maggiore controllo e maggiore flessibilità durante la progettazione degli URI per l'API.
