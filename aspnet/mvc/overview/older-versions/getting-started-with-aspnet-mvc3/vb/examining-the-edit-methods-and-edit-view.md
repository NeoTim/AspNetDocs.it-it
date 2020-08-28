---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
title: Analisi dei metodi di modifica e della visualizzazione di modifica (VB) | Microsoft Docs
author: Rick-Anderson
description: In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 5cb3c59b-1e96-464b-b3a8-c55607201872
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: fc4d59323ab3cc1e4a454676a0ec498f3c5246fd
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2020
ms.locfileid: "89044597"
---
# <a name="examining-the-edit-methods-and-edit-view-vb"></a>Analisi dei metodi di modifica e della visualizzazione di modifica (VB)

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, una versione gratuita di Microsoft Visual Studio. Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito. È possibile installarli tutti facendo clic sul collegamento seguente: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aggiornamento degli strumenti di ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supporto di runtime + Tools)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti facendo clic sul collegamento seguente: [prerequisiti di Visual studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Per accompagnare questo argomento, è disponibile un progetto Visual Web Developer con codice sorgente VB.NET. [Scaricare la versione di VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce C#, passare alla [versione c#](../cs/examining-the-edit-methods-and-edit-view.md) di questa esercitazione.

In questa sezione verranno esaminati i metodi di azione e le visualizzazioni generati per il controller di film. Si aggiungerà quindi una pagina di ricerca personalizzata.

Eseguire l'applicazione e passare al `Movies` controller aggiungendo */Movies* all'URL nella barra degli indirizzi del browser. Mantenere il puntatore del mouse su un collegamento **Edit (modifica** ) per visualizzare l'URL a cui si collega.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

Il collegamento di **modifica** è stato generato dal `Html.ActionLink` metodo nella visualizzazione *Views\Movies\Index.vbhtml* :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image3.png)](examining-the-edit-methods-and-edit-view/_static/image2.png)

L' `Html` oggetto è un helper esposto utilizzando una proprietà nella `WebViewPage` classe base. Il `ActionLink` metodo dell'helper semplifica la generazione dinamica di collegamenti ipertestuali HTML che si collegano ai metodi di azione nei controller. Il primo argomento del `ActionLink` metodo è il testo del collegamento di cui eseguire il rendering (ad esempio, `<a>Edit Me</a>` ). Il secondo argomento è il nome del metodo di azione da richiamare. L'argomento finale è un [oggetto anonimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) che genera i dati della route (in questo caso, l'ID 4).

Il collegamento generato illustrato nell'immagine precedente è `http://localhost:xxxxx/Movies/Edit/4` . La route predefinita accetta il modello di URL `{controller}/{action}/{id}` . Pertanto, ASP.NET converte `http://localhost:xxxxx/Movies/Edit/4` in una richiesta al `Edit` metodo di azione del `Movies` controller con il parametro `ID` uguale a 4.

È anche possibile passare i parametri del metodo di azione usando una stringa di query. Ad esempio, l'URL `http://localhost:xxxxx/Movies/Edit?ID=4` passa anche il parametro `ID` di 4 al `Edit` metodo di azione del `Movies` controller.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image5.png)](examining-the-edit-methods-and-edit-view/_static/image4.png)

Aprire il `Movies` controller. Di `Edit` seguito sono riportati i due metodi di azione.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample3.vb)]

Si noti che il secondo metodo di azione `Edit` è preceduto dall'attributo `HttpPost`. Questo attributo specifica che l'overload del `Edit` metodo può essere richiamato solo per le richieste post. È possibile applicare l' `HttpGet` attributo al primo metodo di modifica, ma ciò non è necessario perché è l'impostazione predefinita. Si farà riferimento ai metodi di azione assegnati in modo implicito all' `HttpGet` attributo come `HttpGet` metodi.

Il `HttpGet` `Edit` metodo accetta il parametro Movie ID, Cerca il film usando il metodo Entity Framework `Find` e restituisce il filmato selezionato alla visualizzazione di modifica. Quando il sistema di scaffolding ha creato la vista Edit, ha esaminato la classe `Movie` e il codice creato per eseguire il rendering degli elementi `<label>` e `<input>` per ogni proprietà della classe. Nell'esempio seguente viene illustrata la visualizzazione di modifica generata:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.vbhtml)]

Si noti che il modello di visualizzazione include un' `@ModelType MvcMovie.Models.Movie` istruzione all'inizio del file. in questo modo si specifica che la vista prevede che il modello per il modello di vista sia di tipo `Movie` .

Il codice con impalcature usa diversi *metodi helper* per semplificare il markup HTML. L' [`Html.LabelFor`](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Helper Visualizza il nome del campo ( &quot; titolo &quot; , &quot; rilasciato &quot; , &quot; genere &quot; o &quot; prezzo &quot; ). L' [`Html.EditorFor`](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Helper Visualizza un elemento HTML `<input>` . L' [`Html.ValidationMessageFor`](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Helper Visualizza tutti i messaggi di convalida associati a tale proprietà.

Eseguire l'applicazione e passare all'URL */Movies* . Fare clic su un collegamento di **modifica** . Nel browser visualizzare l'origine per la pagina. Il codice HTML nella pagina ha un aspetto simile all'esempio seguente. Il markup del menu è stato escluso per maggiore chiarezza.

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html)]

Gli `<input>` elementi si trovano in un `<form>` elemento HTML il cui `action` attributo è impostato su post sull'URL */Movies/Edit* . I dati del modulo verranno inviati al server quando si fa clic sul pulsante **modifica** .

## <a name="processing-the-post-request"></a>Elaborazione della richiesta POST

Nell'elenco seguente viene indicata la versione `HttpPost` del metodo di azione `Edit`.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample6.vb)]

Lo strumento di associazione di modelli di ASP.NET Framework accetta i valori del modulo inviati e crea un `Movie` oggetto passato come `movie` parametro. Il `ModelState.IsValid` controllo del codice verifica che i dati inviati nel form possano essere usati per modificare un `Movie` oggetto. Se i dati sono validi, il codice salva i dati del film nella `Movies` raccolta dell' `MovieDBContext` istanza. Il codice salva quindi i nuovi dati del film nel database chiamando il `SaveChanges` metodo di `MovieDBContext` , che rende permanente le modifiche apportate al database. Dopo aver salvato i dati, il codice reindirizza l'utente al `Index` metodo di azione della `MoviesController` classe, che fa sì che il filmato aggiornato venga visualizzato nell'elenco dei filmati.

Se i valori inviati non sono validi, vengono visualizzati nuovamente nel form. Gli `Html.ValidationMessageFor` helper nel modello di vista *Edit. vbhtml* si occupano della visualizzazione dei messaggi di errore appropriati.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image7.png)](examining-the-edit-methods-and-edit-view/_static/image6.png)

> **Nota sulle impostazioni locali** Se normalmente si lavora con impostazioni locali diverse dall'inglese, vedere [supporto della convalida di ASP.NET MVC 3 con impostazioni locali diverse dall'inglese.](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)

## <a name="making-the-edit-method-more-robust"></a>Rendere più affidabile il metodo di modifica

Il `HttpGet` `Edit` metodo generato dal sistema di ponteggi non verifica che l'ID passato sia valido. Se un utente rimuove il segmento ID dall'URL ( `http://localhost:xxxxx/Movies/Edit` ), viene visualizzato l'errore seguente:

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image9.png)](examining-the-edit-methods-and-edit-view/_static/image8.png)

Un utente può anche passare un ID che non esiste nel database, ad esempio `http://localhost:xxxxx/Movies/Edit/1234` . Per risolvere questa limitazione, è possibile apportare due modifiche al `HttpGet` `Edit` metodo di azione. Per prima cosa, modificare il `ID` parametro in modo che il valore predefinito sia zero quando un ID non viene passato in modo esplicito. È anche possibile verificare che il `Find` metodo abbia trovato un film prima di restituire l'oggetto Movie al modello di visualizzazione. Il `Edit` metodo aggiornato è illustrato di seguito.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample7.vb)]

Se non viene trovato alcun film, `HttpNotFound` viene chiamato il metodo.

Tutti i `HttpGet` metodi seguono un modello simile. Ottengono un oggetto Movie (o un elenco di oggetti nel caso di `Index` ) e passano il modello alla visualizzazione. Il `Create` metodo passa un oggetto Movie vuoto alla visualizzazione create. Tutti i metodi che creano, modificano, eliminano o cambiano in altro modo i dati, eseguono questa operazione nell'overload `HttpPost` del metodo. La modifica dei dati in un metodo HTTP GET costituisce un rischio per la sicurezza, come descritto nel post di Blog relativo all'introduzione a [ASP.NET MVC Tip #46 – non usare i collegamenti Delete perché creano buchi di sicurezza](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). La modifica dei dati in un metodo GET viola anche le procedure consigliate HTTP e il modello REST architetturale, che specifica che le richieste GET non devono modificare lo stato dell'applicazione. In altre parole, l'esecuzione di un'operazione GET deve essere un'operazione sicura senza effetti collaterali.

## <a name="adding-a-search-method-and-search-view"></a>Aggiunta di un metodo di ricerca e di una visualizzazione di ricerca

In questa sezione si aggiungerà un `SearchIndex` metodo di azione che consente di cercare i film in base al genere o al nome. Questa operazione sarà disponibile usando l'URL */Movies/SearchIndex* . Nella richiesta viene visualizzato un form HTML che contiene gli elementi di input che un utente può compilare per cercare un film. Quando un utente invia il modulo, il metodo di azione otterrà i valori di ricerca inviati dall'utente e utilizzerà i valori per eseguire ricerche nel database.

![SearchIndx_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

## <a name="displaying-the-searchindex-form"></a>Visualizzazione del modulo SearchIndex

Per iniziare, aggiungere un `SearchIndex` metodo di azione alla `MoviesController` classe esistente. Il metodo restituirà una visualizzazione contenente un form HTML. Ecco il codice:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample8.vb)]

La prima riga del `SearchIndex` metodo crea la seguente query [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) per selezionare i film:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample9.vb)]

La query è definita a questo punto, ma non è ancora stata eseguita sull'archivio dati.

Se il `searchString` parametro contiene una stringa, la query Movies viene modificata per filtrare in base al valore della stringa di ricerca, usando il codice seguente:

In caso contrario, String. IsNullOrEmpty (searchString)   
 Movies = Movies. Dove (Function/s. title. Contains (searchString))   
 Termina se

Le query LINQ non vengono eseguite quando vengono definite o quando vengono modificate chiamando un metodo come `Where` o `OrderBy` . Al contrario, l'esecuzione della query viene posticipata, il che significa che la valutazione di un'espressione viene posticipata finché il relativo valore realizzato non viene effettivamente iterato o [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) viene chiamato il metodo. Nell' `SearchIndex` esempio, la query viene eseguita nella vista SearchIndex. Per altre informazioni sull'esecuzione posticipata di query, vedere [Esecuzione di query](https://msdn.microsoft.com/library/bb738633.aspx).

A questo punto è possibile implementare la `SearchIndex` visualizzazione che visualizzerà il modulo all'utente. Fare clic con il pulsante destro del mouse all'interno del `SearchIndex` metodo, quindi scegliere **Aggiungi visualizzazione**. Nella finestra di dialogo **Aggiungi visualizzazione** specificare che si intende passare un `Movie` oggetto al modello di visualizzazione come classe del modello. Nell'elenco **modello di impalcatura** scegliere **elenco**, quindi fare clic su **Aggiungi**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

Quando si fa clic sul pulsante **Aggiungi** , viene creato il modello di visualizzazione *Views\Movies\SearchIndex.vbhtml* . Poiché **l'elenco è stato selezionato nell'** elenco dei modelli di **impalcatura** , Visual Web Developer genera automaticamente (con impalcatura) alcuni contenuti predefiniti nella visualizzazione. L'impalcatura ha creato un form HTML. Ha esaminato la `Movie` classe e creato il codice per eseguire il rendering `<label>` degli elementi per ogni proprietà della classe. Nell'elenco seguente viene illustrata la creazione della vista generata:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.vbhtml)]

Eseguire l'applicazione e passare a */Movies/SearchIndex*. Accodare una stringa di query, ad esempio `?searchString=ghost`, all'URL. Vengono visualizzati i film filtrati.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

Se si modifica la firma del `SearchIndex` metodo in modo che disponga di un parametro denominato `id` , il `id` parametro corrisponderà al `{id}` segnaposto per le route predefinite impostate nel file *Global. asax* .

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.txt)]

Il `SearchIndex` metodo modificato avrà un aspetto simile al seguente:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample12.vb)]

È ora possibile passare il titolo della ricerca come dati di route (un segmento di URL), anziché come valore della stringa di query.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

Tuttavia, non è possibile supporre che gli utenti modifichino l'URL ogni volta che desiderano cercare un film. A questo punto si aggiungerà un'interfaccia utente che consente di filtrare i film. Se è stata modificata la firma del `SearchIndex` metodo per testare come passare il parametro ID associato alla route, modificarlo di nuovo in modo che il `SearchIndex` Metodo accetti un parametro di stringa denominato `searchString` :

Aprire il file *Views\Movies\SearchIndex.vbhtml* e, subito dopo `@Html.ActionLink("Create New", "Create")` , aggiungere quanto segue:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample13.vbhtml)]

L' `Html.BeginForm` Helper crea un tag di apertura `<form>` . L' `Html.BeginForm` helper fa in modo che il form invii a se stesso quando l'utente invia il modulo facendo clic sul pulsante **filtro** .

Eseguire l'applicazione e provare a cercare un film.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

Nessun `HttpPost` Overload del `SearchIndex` metodo. Non è necessario, perché il metodo non modifica lo stato dell'applicazione, limitando a filtrare i dati. Se è stato aggiunto il `HttpPost` `SearchIndex` metodo seguente, l'invoker dell'azione corrisponderebbe al `HttpPost` `SearchIndex` metodo e il `HttpPost` `SearchIndex` Metodo verrebbe eseguito come illustrato nell'immagine seguente.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample14.vb)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

## <a name="adding-search-by-genre"></a>Aggiunta della ricerca per genere

Se è stata aggiunta la `HttpPost` versione del `SearchIndex` metodo, eliminarla ora.

Si aggiungerà quindi una funzionalità che consente agli utenti di cercare i film in base al genere. Sostituire il metodo `SearchIndex` con il codice seguente:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample15.vb)]

Questa versione del `SearchIndex` metodo accetta un parametro aggiuntivo, ovvero `movieGenre` . Le prime righe di codice creano un `List` oggetto per mantenere i generi di film dal database.

Il codice seguente è una query LINQ che recupera tutti i generi dal database.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample16.vb)]

Il codice usa il `AddRange` metodo della raccolta generica `List` per aggiungere tutti i generi distinti all'elenco. (Senza il `Distinct` modificatore, verranno aggiunti generi duplicati, ad esempio, la commedia verrebbe aggiunta due volte nell'esempio). Il codice archivia quindi l'elenco dei generi nell' `ViewBag` oggetto.

Il codice seguente illustra come controllare il `movieGenre` parametro. Se non è vuoto, il codice vincola ulteriormente la query dei film per limitare i film selezionati al genere specificato.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample17.vb)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Aggiunta di markup alla visualizzazione SearchIndex per supportare la ricerca per genere

Aggiungere un `Html.DropDownList` helper al file *Views\Movies\SearchIndex.vbhtml* , immediatamente prima dell' `TextBox` Helper. Il markup completato è illustrato di seguito:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.vbhtml)]

Eseguire l'applicazione e passare a */Movies/SearchIndex*. Provare a eseguire una ricerca in base al genere, al nome del film e a entrambi i criteri.

In questa sezione sono stati esaminati i metodi di azione CRUD e le visualizzazioni generate dal Framework. Sono stati creati un metodo e una visualizzazione di azione di ricerca che consentono agli utenti di eseguire ricerche in base al titolo e al genere. Nella sezione successiva verrà illustrato come aggiungere una proprietà al `Movie` modello e come aggiungere un inizializzatore che creerà automaticamente un database di prova.

> [!div class="step-by-step"]
> [Precedente](accessing-your-models-data-from-a-controller.md) 
>  [Avanti](adding-a-new-field.md)
