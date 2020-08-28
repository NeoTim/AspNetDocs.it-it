---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
title: Analisi dei metodi di modifica e della visualizzazione di modifica | Microsoft Docs
author: Rick-Anderson
description: 'Nota: una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 41eb99ca-e88f-4720-ae6d-49a958da8116
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 16ce02ba00e13b4cff2d6e86b2d9e0684aab096e
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2020
ms.locfileid: "89044519"
---
# <a name="examining-the-edit-methods-and-edit-view"></a>Analisi dei metodi di modifica e della visualizzazione di modifica

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Una versione aggiornata di questa esercitazione è disponibile [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e illustra altre funzionalità.

In questa sezione verranno esaminati i metodi di azione e le visualizzazioni generati per il controller di film. Si aggiungerà quindi una pagina di ricerca personalizzata.

Eseguire l'applicazione e passare al `Movies` controller aggiungendo */Movies* all'URL nella barra degli indirizzi del browser. Mantenere il puntatore del mouse su un collegamento **Edit (modifica** ) per visualizzare l'URL a cui si collega.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

Il collegamento di **modifica** è stato generato dal `Html.ActionLink` metodo nella visualizzazione *Views\Movies\Index.cshtml* :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

![HTML. ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

L' `Html` oggetto è un helper esposto usando una proprietà nella classe di base [System. Web. Mvc. WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) . Il `ActionLink` metodo dell'helper semplifica la generazione dinamica di collegamenti ipertestuali HTML che si collegano ai metodi di azione nei controller. Il primo argomento del `ActionLink` metodo è il testo del collegamento di cui eseguire il rendering (ad esempio, `<a>Edit Me</a>` ). Il secondo argomento è il nome del metodo di azione da richiamare. L'argomento finale è un [oggetto anonimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) che genera i dati della route (in questo caso, l'ID 4).

Il collegamento generato illustrato nell'immagine precedente è `http://localhost:xxxxx/Movies/Edit/4` . La route predefinita (stabilita nell' *app \_ Start\RouteConfig.cs*) accetta il modello di URL `{controller}/{action}/{id}` . Pertanto, ASP.NET converte `http://localhost:xxxxx/Movies/Edit/4` in una richiesta al `Edit` metodo di azione del `Movies` controller con il parametro `ID` uguale a 4. Esaminare il codice seguente dal file * \_ Start\RouteConfig.cs dell'app* .

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

È anche possibile passare i parametri del metodo di azione usando una stringa di query. Ad esempio, l'URL `http://localhost:xxxxx/Movies/Edit?ID=4` passa anche il parametro `ID` di 4 al `Edit` metodo di azione del `Movies` controller.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Aprire il `Movies` controller. Di `Edit` seguito sono riportati i due metodi di azione.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cs)]

Si noti che il secondo metodo di azione `Edit` è preceduto dall'attributo `HttpPost`. Questo attributo specifica che l'overload del `Edit` metodo può essere richiamato solo per le richieste post. È possibile applicare l' `HttpGet` attributo al primo metodo di modifica, ma ciò non è necessario perché è l'impostazione predefinita. Si farà riferimento ai metodi di azione assegnati in modo implicito all' `HttpGet` attributo come `HttpGet` metodi.

Il `HttpGet` `Edit` metodo accetta il parametro Movie ID, Cerca il film usando il metodo Entity Framework `Find` e restituisce il filmato selezionato alla visualizzazione di modifica. Il parametro ID specifica un [valore predefinito](https://msdn.microsoft.com/library/dd264739.aspx) pari a zero se il `Edit` metodo viene chiamato senza un parametro. Se non è possibile trovare un film, viene restituito [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) . Quando il sistema di scaffolding ha creato la vista Edit, ha esaminato la classe `Movie` e il codice creato per eseguire il rendering degli elementi `<label>` e `<input>` per ogni proprietà della classe. Nell'esempio seguente viene illustrata la visualizzazione di modifica generata:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cshtml)]

Si noti che il modello di visualizzazione include un' `@model MvcMovie.Models.Movie` istruzione all'inizio del file. in questo modo si specifica che la vista prevede che il modello per il modello di vista sia di tipo `Movie` .

Il codice con impalcature usa diversi *metodi helper* per semplificare il markup HTML. L' [`Html.LabelFor`](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Helper Visualizza il nome del campo ( &quot; titolo &quot; , &quot; rilasciato &quot; , &quot; genere &quot; o &quot; prezzo &quot; ). L' [`Html.EditorFor`](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Helper esegue il rendering di un `<input>` elemento HTML. L' [`Html.ValidationMessageFor`](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Helper Visualizza tutti i messaggi di convalida associati a tale proprietà.

Eseguire l'applicazione e passare all'URL */Movies* . Fare clic su un collegamento di **modifica** . Nel browser visualizzare l'origine per la pagina. Il codice HTML per l'elemento del form è illustrato di seguito.

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html?highlight=7,10-11)]

Gli `<input>` elementi si trovano in un `<form>` elemento HTML il cui `action` attributo è impostato su post sull'URL */Movies/Edit* . I dati del modulo verranno inviati al server quando si fa clic sul pulsante **modifica** .

## <a name="processing-the-post-request"></a>Elaborazione della richiesta POST

Nell'elenco seguente viene indicata la versione `HttpPost` del metodo di azione `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

Lo strumento di [associazione di modelli MVC ASP.NET](https://msdn.microsoft.com/magazine/hh781022.aspx) accetta i valori del modulo inviati e crea un `Movie` oggetto passato come `movie` parametro. Il metodo `ModelState.IsValid` verifica che i dati inviati nel formato possano essere usati per cambiare (modificare o aggiornare) un oggetto `Movie`. Se i dati sono validi, i dati del film vengono salvati nella `Movies` raccolta dell' `db(MovieDBContext` istanza di. I nuovi dati del film vengono salvati nel database chiamando il `SaveChanges` metodo di `MovieDBContext` . Dopo aver salvato i dati, il codice reindirizza l'utente al `Index` metodo di azione della `MoviesController` classe, che visualizza la raccolta di filmati, incluse le modifiche appena apportate.

Se i valori inviati non sono validi, vengono visualizzati nuovamente nel form. Gli `Html.ValidationMessageFor` helper nel modello di vista *Edit. cshtml* si occupano della visualizzazione dei messaggi di errore appropriati.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

> [!NOTE]
> per supportare la convalida jQuery per le impostazioni locali diverse dall'inglese che usano una virgola ( &quot; , &quot; ) per un separatore decimale, è necessario includere *globalize.js* e il file di *impostazioni cultura/globalize.cultures.js* specifici (da [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) e JavaScript da usare `Globalize.parseFloat` . Nel codice seguente vengono illustrate le modifiche apportate al file Views\Movies\Edit.cshtml per l'utilizzo con le &quot; impostazioni cultura fr-fr &quot; :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Il campo decimale potrebbe richiedere una virgola, non un separatore decimale. Come correzione temporanea, è possibile aggiungere l'elemento di globalizzazione al file radice dei progetti web.config. Il codice seguente illustra l'elemento di globalizzazione con le impostazioni cultura impostate per Stati Uniti inglese.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.xml)]

Tutti i `HttpGet` metodi seguono un modello simile. Ottengono un oggetto Movie (o un elenco di oggetti nel caso di `Index` ) e passano il modello alla visualizzazione. Il `Create` metodo passa un oggetto Movie vuoto alla visualizzazione create. Tutti i metodi che creano, modificano, eliminano o cambiano in altro modo i dati, eseguono questa operazione nell'overload `HttpPost` del metodo. La modifica dei dati in un metodo HTTP GET costituisce un rischio per la sicurezza, come descritto nel post di Blog relativo all'introduzione a [ASP.NET MVC Tip #46 – non usare i collegamenti Delete perché creano buchi di sicurezza](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). La modifica dei dati in un metodo GET viola anche le procedure consigliate HTTP e il modello [Rest](http://en.wikipedia.org/wiki/Representational_State_Transfer) architetturale, che specifica che le richieste Get non devono modificare lo stato dell'applicazione. In altre parole, l'esecuzione di un'operazione GET deve essere sicura, senza effetti collaterali e non modificare dati persistenti.

## <a name="adding-a-search-method-and-search-view"></a>Aggiunta di un metodo di ricerca e di una visualizzazione di ricerca

In questa sezione si aggiungerà un `SearchIndex` metodo di azione che consente di cercare i film in base al genere o al nome. Questa operazione sarà disponibile usando l'URL */Movies/SearchIndex* . Nella richiesta viene visualizzato un form HTML che contiene gli elementi di input che un utente può immettere per cercare un film. Quando un utente invia il modulo, il metodo di azione otterrà i valori di ricerca inviati dall'utente e utilizzerà i valori per eseguire ricerche nel database.

## <a name="displaying-the-searchindex-form"></a>Visualizzazione del modulo SearchIndex

Per iniziare, aggiungere un `SearchIndex` metodo di azione alla `MoviesController` classe esistente. Il metodo restituirà una visualizzazione contenente un form HTML. Ecco il codice:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

La prima riga del `SearchIndex` metodo crea la seguente query [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) per selezionare i film:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cs)]

La query è definita a questo punto, ma non è ancora stata eseguita sull'archivio dati.

Se il `searchString` parametro contiene una stringa, la query Movies viene modificata per filtrare in base al valore della stringa di ricerca, usando il codice seguente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample11.cs)]

Il codice `s => s.Title` precedente è un'[espressione lambda](https://msdn.microsoft.com/library/bb397687.aspx). Le espressioni lambda vengono usate nelle query [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) basate sul metodo come argomenti per i metodi degli operatori di query standard, ad esempio il metodo [where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) usato nel codice riportato in precedenza. Le query LINQ non vengono eseguite quando vengono definite o quando vengono modificate chiamando un metodo come `Where` o `OrderBy` . Al contrario, l'esecuzione della query viene posticipata, il che significa che la valutazione di un'espressione viene posticipata finché il relativo valore realizzato non viene effettivamente iterato o [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) viene chiamato il metodo. Nell' `SearchIndex` esempio, la query viene eseguita nella vista SearchIndex. Per altre informazioni sull'esecuzione posticipata di query, vedere [Esecuzione di query](https://msdn.microsoft.com/library/bb738633.aspx).

A questo punto è possibile implementare la `SearchIndex` visualizzazione che visualizzerà il modulo all'utente. Fare clic con il pulsante destro del mouse all'interno del `SearchIndex` metodo, quindi scegliere **Aggiungi visualizzazione**. Nella finestra di dialogo **Aggiungi visualizzazione** specificare che si intende passare un `Movie` oggetto al modello di visualizzazione come classe del modello. Nell'elenco **modello di impalcatura** scegliere **elenco**, quindi fare clic su **Aggiungi**.

![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image5.png)

Quando si fa clic sul pulsante **Aggiungi** , viene creato il modello di visualizzazione *Views\Movies\SearchIndex.cshtml* . Poiché **l'elenco è stato selezionato nell'** elenco dei modelli di **impalcatura** , Visual Studio ha generato automaticamente (con impalcatura) alcuni markup predefiniti nella visualizzazione. L'impalcatura ha creato un form HTML. Ha esaminato la `Movie` classe e creato il codice per eseguire il rendering `<label>` degli elementi per ogni proprietà della classe. Nell'elenco seguente viene illustrata la creazione della vista generata:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cshtml)]

Eseguire l'applicazione e passare a */Movies/SearchIndex*. Accodare una stringa di query, ad esempio `?searchString=ghost`, all'URL. Vengono visualizzati i film filtrati.

![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image6.png)

Se si modifica la firma del `SearchIndex` metodo in modo che disponga di un parametro denominato `id` , il `id` parametro corrisponderà al `{id}` segnaposto per le route predefinite impostate nel file *Global. asax* .

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample13.txt)]

Il metodo originale ha un `SearchIndex` aspetto simile al seguente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cs)]

Il `SearchIndex` metodo modificato avrà un aspetto simile al seguente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cs?highlight=1,3)]

È ora possibile passare il titolo della ricerca come dati di route (un segmento di URL), anziché come valore della stringa di query.

![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image7.png)

Tuttavia, non è possibile supporre che gli utenti modifichino l'URL ogni volta che desiderano cercare un film. A questo punto si aggiungerà un'interfaccia utente che consente di filtrare i film. Se è stata modificata la firma del `SearchIndex` metodo per testare come passare il parametro ID associato alla route, modificarlo di nuovo in modo che il `SearchIndex` Metodo accetti un parametro di stringa denominato `searchString` :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

Aprire il file *Views\Movies\SearchIndex.cshtml* e, subito dopo `@Html.ActionLink("Create New", "Create")` , aggiungere quanto segue:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml?highlight=2)]

Nell'esempio seguente viene illustrata una parte del file *Views\Movies\SearchIndex.cshtml* con il markup di filtro aggiunto.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cshtml?highlight=12-15)]

L' `Html.BeginForm` Helper crea un tag di apertura `<form>` . L' `Html.BeginForm` helper fa in modo che il form invii a se stesso quando l'utente invia il modulo facendo clic sul pulsante **filtro** .

Eseguire l'applicazione e provare a cercare un film.

![](examining-the-edit-methods-and-edit-view/_static/image8.png)

Nessun `HttpPost` Overload del `SearchIndex` metodo. Non è necessario, perché il metodo non modifica lo stato dell'applicazione, limitando a filtrare i dati.

È possibile aggiungere il metodo `HttpPost SearchIndex` seguente. In tal caso, l'invoker dell'azione corrisponderebbe al `HttpPost SearchIndex` metodo e il `HttpPost SearchIndex` Metodo verrebbe eseguito come illustrato nell'immagine seguente.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image9.png)

Tuttavia, anche se si aggiunge questa versione `HttpPost` del metodo `SearchIndex`, esiste una limitazione sul modo sulla relativa implementazione. Si supponga che si desideri usare come segnalibro una ricerca specifica o inviare un collegamento agli amici su cui possono fare clic per visualizzare lo stesso elenco filtrato di film. Si noti che l'URL per la richiesta HTTP POST è lo stesso dell'URL per la richiesta GET (localhost: xxxxx/Movies/SearchIndex), non sono presenti informazioni di ricerca nell'URL stesso. A questo punto, le informazioni sulla stringa di ricerca vengono inviate al server come valore del campo del modulo. Ciò significa che non è possibile acquisire le informazioni di ricerca nel segnalibro o inviarle agli amici in un URL.

La soluzione consiste nell'usare un overload di `BeginForm` che specifichi che la richiesta post deve aggiungere le informazioni di ricerca all'URL e che deve essere indirizzata alla versione HttpGet del `SearchIndex` metodo. Sostituire il `BeginForm` metodo senza parametri esistente con quanto segue:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cshtml)]

![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

A questo punto, quando si invia una ricerca, l'URL contiene una stringa di query di ricerca. La ricerca passerà anche al metodo di azione `HttpGet SearchIndex`, anche se si dispone di un metodo `HttpPost SearchIndex`.

![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image11.png)

## <a name="adding-search-by-genre"></a>Aggiunta della ricerca per genere

Se è stata aggiunta la `HttpPost` versione del `SearchIndex` metodo, eliminarla ora.

Si aggiungerà quindi una funzionalità che consente agli utenti di cercare i film in base al genere. Sostituire il metodo `SearchIndex` con il codice seguente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cs)]

Questa versione del `SearchIndex` metodo accetta un parametro aggiuntivo, ovvero `movieGenre` . Le prime righe di codice creano un `List` oggetto per mantenere i generi di film dal database.

Il codice seguente è una query LINQ che recupera tutti i generi dal database.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample22.cs)]

Il codice usa il `AddRange` metodo della raccolta generica `List` per aggiungere tutti i generi distinti all'elenco. (Senza il `Distinct` modificatore, verranno aggiunti generi duplicati, ad esempio, la commedia verrebbe aggiunta due volte nell'esempio). Il codice archivia quindi l'elenco dei generi nell' `ViewBag` oggetto.

Il codice seguente illustra come controllare il `movieGenre` parametro. Se non è vuoto, il codice vincola ulteriormente la query dei film per limitare i film selezionati al genere specificato.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample23.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Aggiunta di markup alla visualizzazione SearchIndex per supportare la ricerca per genere

Aggiungere un `Html.DropDownList` helper al file *Views\Movies\SearchIndex.cshtml* , immediatamente prima dell' `TextBox` Helper. Il markup completato è illustrato di seguito:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample24.cshtml?highlight=4)]

Eseguire l'applicazione e passare a */Movies/SearchIndex*. Provare a eseguire una ricerca in base al genere, al nome del film e a entrambi i criteri.

![](examining-the-edit-methods-and-edit-view/_static/image12.png)

In questa sezione sono stati esaminati i metodi di azione CRUD e le visualizzazioni generate dal Framework. Sono stati creati un metodo e una visualizzazione di azione di ricerca che consentono agli utenti di eseguire ricerche in base al titolo e al genere. Nella sezione successiva verrà illustrato come aggiungere una proprietà al `Movie` modello e come aggiungere un inizializzatore che creerà automaticamente un database di prova.

> [!div class="step-by-step"]
> [Precedente](accessing-your-models-data-from-a-controller.md) 
>  [Avanti](adding-a-new-field-to-the-movie-model-and-table.md)
