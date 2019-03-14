---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
title: Esaminare i metodi di modifica e visualizzazione di modifica (VB) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 5cb3c59b-1e96-464b-b3a8-c55607201872
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 509f362a15990486cc5fa4f2f666c3d0de2434dc
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57055968"
---
<a name="examining-the-edit-methods-and-edit-view-vb"></a>Analisi dei metodi di modifica e della visualizzazione di modifica (VB)
====================
da [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul collegamento seguente: [Installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul collegamento seguente: [Prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente Visual Basic.NET è disponibile a complemento di questo argomento. [Scaricare la versione VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce c#, passare al [c# versione](../cs/examining-the-edit-methods-and-edit-view.md) di questa esercitazione.


In questa sezione si esamina i metodi di azione generata e visualizzazioni per il controller di film. Quindi si aggiungerà una pagina di ricerca personalizzato.

Eseguire l'applicazione e passare al `Movies` controller aggiungendo */Movies* all'URL nella barra degli indirizzi del browser. Sposta il puntatore del mouse su un **modifica** collegamento per visualizzare l'URL a essa collegate.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

Il **Edit** collegamento è stato generato dal `Html.ActionLink` metodo nel *Views\Movies\Index.vbhtml* visualizzazione:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image3.png)](examining-the-edit-methods-and-edit-view/_static/image2.png)

Il `Html` oggetto è un helper esposto mediante una proprietà nel `WebViewPage` classe di base. Il `ActionLink` metodo dell'helper consente di generare in modo dinamico i collegamenti ipertestuali HTML che si collegano ai metodi di azione nel controller. Il primo argomento per il `ActionLink` metodo è il testo del collegamento per eseguire il rendering (ad esempio, `<a>Edit Me</a>`). Il secondo argomento è il nome del metodo di azione da richiamare. L'argomento finale è un [oggetto anonimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) che genera i dati della route (in questo caso, l'ID di 4).

Il collegamento generato illustrato nell'immagine precedente è `http://localhost:xxxxx/Movies/Edit/4`. La route predefinita ha il modello di URL `{controller}/{action}/{id}`. Pertanto, ASP.NET si traduce `http://localhost:xxxxx/Movies/Edit/4` in una richiesta per il `Edit` metodo di azione del `Movies` controller con il parametro `ID` pari a 4.

È anche possibile passare parametri di metodo di azione usando una stringa di query. Ad esempio, l'URL `http://localhost:xxxxx/Movies/Edit?ID=4` passa inoltre il parametro `ID` pari a 4 per il `Edit` metodo di azione del `Movies` controller.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image5.png)](examining-the-edit-methods-and-edit-view/_static/image4.png)

Aprire il `Movies` controller. I due `Edit` i metodi di azione sono illustrati di seguito.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample3.vb)]

Si noti che il secondo metodo di azione `Edit` è preceduto dall'attributo `HttpPost`. Questo attributo specifica che eseguono l'overload del `Edit` metodo può essere richiamato solo per le richieste POST. È possibile applicare il `HttpGet` attributo al primo metodo di modifica, ma che non è necessario perché è il valore predefinito. (Si farà riferimento ai metodi di azione che vengono assegnati in modo implicito il `HttpGet` dell'attributo come `HttpGet` metodi.)

Il `HttpGet` `Edit` metodo accetta il parametro ID film, Cerca il film tramite Entity Framework `Find` (metodo) e restituisce il film selezionato nella visualizzazione di modifica. Quando il sistema di scaffolding ha creato la vista Edit, ha esaminato la classe `Movie` e il codice creato per eseguire il rendering degli elementi `<label>` e `<input>` per ogni proprietà della classe. Nell'esempio seguente mostra la visualizzazione di modifica che è stata generata:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.vbhtml)]

Si noti come il modello di vista ha un `@ModelType MvcMovie.Models.Movie` istruzione all'inizio del file: Specifica che il modello per il modello di vista sia del tipo previsto dalla vista `Movie`.

Il codice con scaffolding Usa diversi *metodi helper* per semplificare il markup HTML. Il [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) helper Visualizza il nome del campo (&quot;Title&quot;, &quot;ReleaseDate&quot;, &quot;Genre&quot;, o &quot;prezzo &quot;). Il [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) helper Visualizza HTML `<input>` elemento. Il [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) helper Visualizza gli eventuali messaggi di convalida associati alla proprietà.

Eseguire l'applicazione e passare al */Movies* URL. Fare clic su un collegamento **Edit** (Modifica). Nel browser visualizzare l'origine per la pagina. Il codice HTML nella pagina sarà simile all'esempio seguente. (Per maggiore chiarezza è stato escluso il markup di menu).

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html)]

Il `<input>` elementi si trovano in un elemento HTML `<form>` elemento la cui `action` attributo è impostato su post per il */Movies/Edit* URL. I dati del modulo verranno inviati al server quando il **modifica** si fa clic sul pulsante.

## <a name="processing-the-post-request"></a>Elaborazione della richiesta POST

Nell'elenco seguente viene indicata la versione `HttpPost` del metodo di azione `Edit`.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample6.vb)]

Il raccoglitore di modelli ASP.NET framework accetta i valori di modulo e crea un `Movie` che viene passato come il `movie` parametro. Il `ModelState.IsValid` archivia il codice verifica che i dati inviati nel formato possono essere utilizzati per modificare un `Movie` oggetto. Se i dati sono validi, il codice consente di salvare i dati dei film per i `Movies` raccolta del `MovieDBContext` istanza. Il codice quindi Salva i nuovi dati del film nel database chiamando il `SaveChanges` metodo `MovieDBContext`, che mantiene le modifiche al database. Dopo aver salvato i dati, il codice reindirizza l'utente per il `Index` metodo di azione del `MoviesController` classe, che fa sì che il film aggiornato da visualizzare nell'elenco di film.

Se i valori inviati non sono validi, essi verranno nuovamente visualizzati nel formato. Il `Html.ValidationMessageFor` helper nel *Edit.vbhtml* vista modello ci occupiamo della visualizzazione dei messaggi di errore appropriato.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image7.png)](examining-the-edit-methods-and-edit-view/_static/image6.png)

> **Nota sulle impostazioni internazionali** se si usano in genere le impostazioni locali diverse dall'inglese, vedere [che supportano ASP.NET MVC 3 convalida con le impostazioni locali diverse dall'inglese.](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>Rendere più affidabile il metodo di modifica

Il `HttpGet` `Edit` metodo generato dal sistema di scaffolding non verifica che l'ID che viene passato al metodo è valido. Se un utente rimuove il segmento ID dall'URL (`http://localhost:xxxxx/Movies/Edit`), viene visualizzato l'errore seguente:

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image9.png)](examining-the-edit-methods-and-edit-view/_static/image8.png)

Un utente può anche passare un ID che non esiste nel database, ad esempio `http://localhost:xxxxx/Movies/Edit/1234`. È possibile apportare due modifiche per il `HttpGet` `Edit` metodo di azione per risolvere questa limitazione. In primo luogo, modificare il `ID` parametro in modo che un valore predefinito pari a zero quando non viene passato in modo esplicito un ID. È anche possibile verificare che il `Find` metodo effettivamente trovato un film prima di restituire l'oggetto film al modello di visualizzazione. Aggiornato `Edit` metodo è illustrato di seguito.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample7.vb)]

Se non viene trovato alcun film, il `HttpNotFound` viene chiamato il metodo.

Tutti i `HttpGet` metodi seguono un modello simile. Ricevono un oggetto film (o un elenco di oggetti, nel caso di `Index`) e passare il modello alla visualizzazione. Il `Create` metodo passa un oggetto vuoto per la visualizzazione di creazione. Tutti i metodi che creano, modificano, eliminano o cambiano in altro modo i dati, eseguono questa operazione nell'overload `HttpPost` del metodo. Modifica dei dati in un metodo HTTP GET è un rischio per la sicurezza, come descritto nella voce del post di blog [ASP.NET MVC suggerimento & 46: non usare i collegamenti di eliminazione poiché creano le falle](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modifica dei dati in un metodo GET viola anche le procedure consigliate HTTP e il modello architetturale REST, che specifica che le richieste GET non devono modificherà lo stato dell'applicazione. In altre parole, eseguendo un'operazione GET deve essere un'operazione sicura che non ha effetti collaterali.

## <a name="adding-a-search-method-and-search-view"></a>Aggiunta di un metodo di ricerca e visualizzazione di ricerca

In questa sezione si aggiungerà un `SearchIndex` metodo di azione che consente di ricercare i film per genere o un nome. Questo sarà disponibile tramite il */Movies/SearchIndex* URL. La richiesta verrà visualizzato un form HTML che contiene gli elementi di input che un utente può inserire per cercare un film. Quando un utente invia il form, il metodo di azione verrà ottenere i valori di ricerca registrati dall'utente e usare i valori per eseguire ricerche nel database.

![SearchIndx_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

## <a name="displaying-the-searchindex-form"></a>Visualizzazione di Form SearchIndex

Iniziare aggiungendo un `SearchIndex` metodo di azione esistente `MoviesController` classe. Il metodo restituisce una visualizzazione contenente un form HTML. Ecco il codice:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample8.vb)]

La prima riga del `SearchIndex` metodo creati i seguenti elementi [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query per selezionare i film:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample9.vb)]

La query viene definita in questo punto, ma non è ancora stata eseguita a fronte dell'archivio dati.

Se il `searchString` parametro contiene una stringa, la query dei film viene modificata per filtrare il valore della stringa di ricerca, usando il codice seguente:

If Not String.IsNullOrEmpty(searchString) Then   
 film = film. In cui (funzioni incluse s.Title.Contains(searchString))   
 End If

Le query LINQ non vengono eseguite al momento della definizione o quando vengono modificati chiamando un metodo, ad esempio `Where` o `OrderBy`. Al contrario, viene rinviato l'esecuzione di query, il che significa che la valutazione di un'espressione viene ritardata finché non viene effettivamente un'iterazione il relativo valore realizzato o la [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) viene chiamato il metodo. Nel `SearchIndex` esempio, la query viene eseguita nella visualizzazione SearchIndex. Per altre informazioni sull'esecuzione posticipata di query, vedere [Esecuzione di query](https://msdn.microsoft.com/library/bb738633.aspx).

A questo punto è possibile implementare il `SearchIndex` vista in cui verrà visualizzato il form per l'utente. Pulsante destro del mouse all'interno di `SearchIndex` metodo e quindi fare clic su **Aggiungi visualizzazione**. Nel **Aggiungi visualizzazione** finestra di dialogo, specificare che si intende passare un `Movie` oggetto per il modello di vista della relativa classe di modello. Nel **modelli di scaffolding** casella di riepilogo **elenco**, quindi fare clic su **Aggiungi**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

Quando si fa clic il **Add** pulsante, il *Views\Movies\SearchIndex.vbhtml* viene creato il modello di visualizzazione. Perché è stato selezionato **elenco** nel **modello Scaffold** elencare, Visual Web Developer generata automaticamente (sottoposto a scaffolding) del contenuto predefinito nella visualizzazione. Lo scaffolding creato un form HTML. Viene esaminata la `Movie` classe e il codice creato per eseguire il rendering `<label>` elementi per ogni proprietà della classe. Il codice seguente mostra la visualizzazione di creazione che è stata generata:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.vbhtml)]

Eseguire l'applicazione e passare a */Movies/SearchIndex*. Accodare una stringa di query, ad esempio `?searchString=ghost`, all'URL. Vengono visualizzati i film filtrati.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

Se si modifica la firma del `SearchIndex` metodo di avere un parametro denominato `id`, il `id` corrisponderà a parametro il `{id}` segnaposto per il valore predefinito consente di indirizzare set nel *Global. asax* file.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

Modificato `SearchIndex` metodo sarà simile alla seguente:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample12.vb)]

È ora possibile passare il titolo della ricerca come dati di route (un segmento di URL), anziché come valore della stringa di query.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

Tuttavia, non è possibile supporre che gli utenti modifichino l'URL ogni volta che desiderano cercare un film. Quindi, ora si aggiungerai dell'interfaccia utente per consentire loro filtrare i film. Se è stata modificata la firma del `SearchIndex` metodo da testare come passare il parametro ID associato alla route, modificarlo in modo che i `SearchIndex` metodo accetta un parametro di stringa denominato `searchString`:

Aprire il *Views\Movies\SearchIndex.vbhtml* del file e subito dopo `@Html.ActionLink("Create New", "Create")`, aggiungere quanto segue:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample13.vbhtml)]

Il `Html.BeginForm` helper crea un'apertura `<form>` tag. Il `Html.BeginForm` helper, il form inviare a se stessa quando l'utente invia il form scegliendo il **filtro** pulsante.

Eseguire l'applicazione e provare a cercare un film.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

È presente alcun `HttpPost` eseguire l'overload del `SearchIndex` (metodo). Non è necessario, poiché il metodo non modifica lo stato dell'applicazione, ma filtra solo i dati. Se è stato aggiunto quanto segue `HttpPost` `SearchIndex` metodo, l'invoker dell'azione corrisponderebbe la `HttpPost` `SearchIndex` metodo e il `HttpPost` `SearchIndex` metodo viene eseguito come illustrato nell'immagine seguente.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample14.vb)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

## <a name="adding-search-by-genre"></a>Aggiunta di ricerca in base al genere

Se è stato aggiunto il `HttpPost` versione del `SearchIndex` metodo, eliminarlo subito.

Successivamente, si aggiungerà una funzionalità per consentire agli utenti di eseguire la ricerca di film in base al genere. Sostituire il metodo `SearchIndex` con il codice seguente:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample15.vb)]

Questa versione del `SearchIndex` metodo accetta un parametro aggiuntivo, vale a dire `movieGenre`. Creano le prime righe di codice un `List` oggetto per contenere cinematografici dal database.

Il codice seguente è una query LINQ che recupera tutti i generi dal database.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample16.vb)]

Il codice Usa il `AddRange` metodo del tipo generico `List` raccolta a cui aggiungere tutti i generi distinti all'elenco. (Senza il `Distinct` modificatore, verrebbero aggiunti generi duplicati, ad esempio, commedie verrebbero aggiunto due volte nel nostro esempio). Il codice quindi archivia l'elenco dei generi nel `ViewBag` oggetto.

Il codice seguente viene illustrato come controllare il `movieGenre` parametro. Se non è vuota ulteriormente il codice vincola la query dei film per limitare il film selezionato per il genere specificato.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample17.vb)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Aggiunta di Markup alla visualizzazione SearchIndex per supportare la ricerca in base al genere

Aggiungere un `Html.DropDownList` helper per il *Views\Movies\SearchIndex.vbhtml* nel file, subito prima di `TextBox` helper. Di seguito è riportato il markup completato:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.vbhtml)]

Eseguire l'applicazione e passare a */Movies/SearchIndex*. Eseguire una ricerca in base al genere dal nome del film e da entrambi i criteri.

In questa sezione sono stati esaminati i metodi di azione CRUD e visualizzazioni generate dal framework. È stato creato un metodo di azione di ricerca e visualizzazione che consentono agli utenti di ricerca in base al titolo del film e genere. Nella sezione successiva, vedrà come aggiungere una proprietà per il `Movie` modello e come aggiungere un inizializzatore che creerà automaticamente un database di test.

> [!div class="step-by-step"]
> [Precedente](accessing-your-models-data-from-a-controller.md)
> [Successivo](adding-a-new-field.md)
