---
uid: mvc/overview/getting-started/introduction/adding-search
title: Cerca | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/17/2019
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: be4e4d13e574b0fcb77d2d0fb8c6f58041b1ece2
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2020
ms.locfileid: "89044874"
---
# <a name="search"></a>Ricerca

[!INCLUDE [consider RP](~/includes/razor.md)]

## <a name="adding-a-search-method-and-search-view"></a>Aggiunta di un metodo di ricerca e di una visualizzazione di ricerca

In questa sezione si aggiungerà la funzionalità di ricerca al `Index` metodo di azione che consente di cercare i film in base al genere o al nome.

## <a name="prerequisites"></a>Prerequisiti

Per trovare la corrispondenza con le schermate di questa sezione, è necessario eseguire l'applicazione (F5) e aggiungere i film seguenti al database.

| Titolo | Data di rilascio | Genre | Prezzo |
| ----- | ------------ | ----- | ----- |
| Ghostbusters | 6/8/1984 | Film commedia | 6,99 |
| Ghostbusters II | 6/16/1989 | Film commedia | 6,99 |
| Pianeta delle scimmie | 3/27/1986 | Azione | 5,99 |

## <a name="updating-the-index-form"></a>Aggiornamento del modulo di indice

Per iniziare, aggiornare il `Index` metodo di azione alla `MoviesController` classe esistente. Ecco il codice:

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

La prima riga del `Index` metodo crea la seguente query [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) per selezionare i film:

[!code-csharp[Main](adding-search/samples/sample2.cs)]

La query è definita a questo punto, ma non è ancora stata eseguita sul database.

Se il `searchString` parametro contiene una stringa, la query Movies viene modificata per filtrare in base al valore della stringa di ricerca, usando il codice seguente:

[!code-csharp[Main](adding-search/samples/sample3.cs)]

Il codice `s => s.Title` precedente è un'[espressione lambda](https://msdn.microsoft.com/library/bb397687.aspx). Le espressioni lambda vengono usate nelle query [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) basate sul metodo come argomenti per i metodi degli operatori di query standard, ad esempio il metodo [where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) usato nel codice riportato in precedenza. Le query LINQ non vengono eseguite quando vengono definite o quando vengono modificate chiamando un metodo come `Where` o `OrderBy` . Al contrario, l'esecuzione della query viene posticipata, il che significa che la valutazione di un'espressione viene posticipata finché il relativo valore realizzato non viene effettivamente iterato o [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) viene chiamato il metodo. Nell' `Search` esempio, la query viene eseguita nella vista *index. cshtml* . Per altre informazioni sull'esecuzione posticipata di query, vedere [Esecuzione di query](https://msdn.microsoft.com/library/bb738633.aspx).

> [!NOTE]
> Il metodo [Contains](https://msdn.microsoft.com/library/bb155125.aspx) viene eseguito sul database, non sul codice c# precedente. Nel database [contiene](https://msdn.microsoft.com/library/bb155125.aspx) mapping a [SQL like](https://msdn.microsoft.com/library/ms179859.aspx), che non fa distinzione tra maiuscole e minuscole.

A questo punto è possibile aggiornare la `Index` vista che visualizzerà il modulo all'utente.

Eseguire l'applicazione e passare a */movies/index*. Accodare una stringa di query, ad esempio `?searchString=ghost`, all'URL. Vengono visualizzati i film filtrati.

![SearchQryStr](adding-search/_static/image1.png)

Se si modifica la firma del `Index` metodo in modo che disponga di un parametro denominato `id` , il `id` parametro corrisponderà al `{id}` segnaposto per le route predefinite impostate nel file * \_ Start\RouteConfig.cs dell'app* .

[!code-json[Main](adding-search/samples/sample4.txt)]

Il metodo originale ha un `Index` aspetto simile al seguente:

[!code-csharp[Main](adding-search/samples/sample5.cs)]

Il `Index` metodo modificato avrà un aspetto simile al seguente:

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

È ora possibile passare il titolo della ricerca come dati di route (un segmento di URL), anziché come valore della stringa di query.

![](adding-search/_static/image2.png)

Tuttavia, non è possibile supporre che gli utenti modifichino l'URL ogni volta che desiderano cercare un film. A questo punto si aggiungerà l'interfaccia utente per filtrare i film. Se è stata modificata la firma del `Index` metodo per testare come passare il parametro ID associato alla route, modificarlo di nuovo in modo che il `Index` Metodo accetti un parametro di stringa denominato `searchString` :

[!code-csharp[Main](adding-search/samples/sample7.cs)]

Aprire il file *Views\Movies\Index.cshtml* e, subito dopo `@Html.ActionLink("Create New", "Create")` , aggiungere il markup del modulo evidenziato di seguito:

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

L' `Html.BeginForm` Helper crea un tag di apertura `<form>` . L' `Html.BeginForm` helper fa in modo che il form invii a se stesso quando l'utente invia il modulo facendo clic sul pulsante **filtro** .

Visual Studio 2013 offre un miglioramento significativo durante la visualizzazione e la modifica dei file di visualizzazione. Quando si esegue l'applicazione con un file di visualizzazione aperto, Visual Studio 2013 richiama il metodo di azione del controller corretto per visualizzare la visualizzazione.

![](adding-search/_static/image3.png)

Con la visualizzazione dell'indice aperta in Visual Studio (come illustrato nell'immagine precedente), toccare CTR F5 o F5 per eseguire l'applicazione, quindi provare a cercare un film.

![](adding-search/_static/image4.png)

Nessun `HttpPost` Overload del `Index` metodo. Non è necessario, perché il metodo non modifica lo stato dell'applicazione, limitando a filtrare i dati.

È possibile aggiungere il metodo `HttpPost Index` seguente. In tal caso, l'invoker dell'azione corrisponderebbe al `HttpPost Index` metodo e il `HttpPost Index` Metodo verrebbe eseguito come illustrato nell'immagine seguente.

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

Tuttavia, anche se si aggiunge questa versione `HttpPost` del metodo `Index`, esiste una limitazione sul modo sulla relativa implementazione. Si supponga che si desideri usare come segnalibro una ricerca specifica o inviare un collegamento agli amici su cui possono fare clic per visualizzare lo stesso elenco filtrato di film. Si noti che l'URL della richiesta HTTP POST è lo stesso dell'URL per la richiesta GET (localhost: xxxxx/movies/index). non sono presenti informazioni di ricerca nell'URL stesso. A questo punto, le informazioni sulla stringa di ricerca vengono inviate al server come valore del campo del modulo. Ciò significa che non è possibile acquisire le informazioni di ricerca nel segnalibro o inviarle agli amici in un URL.

La soluzione consiste nell'usare un overload di `BeginForm` che specifichi che la richiesta post deve aggiungere le informazioni di ricerca all'URL e che deve essere indirizzata alla `HttpGet` versione del `Index` metodo. Sostituire il `BeginForm` metodo senza parametri esistente con il markup seguente:

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

A questo punto, quando si invia una ricerca, l'URL contiene una stringa di query di ricerca. La ricerca passerà anche al metodo di azione `HttpGet Index`, anche se si dispone di un metodo `HttpPost Index`.

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>Aggiunta della ricerca per genere

Se è stata aggiunta la `HttpPost` versione del `Index` metodo, eliminarla ora.

Si aggiungerà quindi una funzionalità che consente agli utenti di cercare i film in base al genere. Sostituire il metodo `Index` con il codice seguente:

[!code-csharp[Main](adding-search/samples/sample11.cs)]

Questa versione del `Index` metodo accetta un parametro aggiuntivo, ovvero `movieGenre` . Le prime righe di codice creano un `List` oggetto per mantenere i generi di film dal database.

Il codice seguente è una query LINQ che recupera tutti i generi dal database.

[!code-csharp[Main](adding-search/samples/sample12.cs)]

Il codice usa il `AddRange` metodo della raccolta generica `List` per aggiungere tutti i generi distinti all'elenco. (Senza il `Distinct` modificatore, verranno aggiunti generi duplicati, ad esempio, la commedia verrebbe aggiunta due volte nell'esempio). Il codice archivia quindi l'elenco dei generi nell' `ViewBag.MovieGenre` oggetto. L'archiviazione dei dati di categoria, ad esempio i generi di film, come oggetto [Select](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) list in a `ViewBag` , quindi l'accesso ai dati della categoria in una casella di riepilogo a discesa è un approccio tipico per le applicazioni MVC.

Il codice seguente illustra come controllare il `movieGenre` parametro. Se non è vuoto, il codice vincola ulteriormente la query dei film per limitare i film selezionati al genere specificato.

[!code-csharp[Main](adding-search/samples/sample13.cs)]

Come indicato in precedenza, la query non viene eseguita nel database fino a quando non viene eseguita l'iterazione dell'elenco di film (che si verifica nella visualizzazione, dopo la `Index` restituzione del metodo di azione).

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>Aggiunta di markup alla visualizzazione index per supportare la ricerca per genere

Aggiungere un `Html.DropDownList` helper al file *Views\Movies\Index.cshtml* , immediatamente prima dell' `TextBox` Helper. Il markup completato è illustrato di seguito:

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

Nel codice seguente:

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

Il parametro "MovieGenre" fornisce la chiave per l' `DropDownList` Helper per trovare un oggetto `IEnumerable<SelectListItem>` in `ViewBag` . `ViewBag`È stato popolato nel metodo di azione:

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

Il parametro "All" fornisce un'etichetta di opzione. Se si esamina tale scelta nel browser, si noterà che il relativo attributo "value" è vuoto. Poiché il controller filtra solo `if` la stringa non è `null` o vuoto, l'invio di un valore vuoto per `movieGenre` Mostra tutti i generi.

È anche possibile impostare un'opzione per la selezione predefinita. Se si vuole "Comedy" come opzione predefinita, modificare il codice nel controller come segue:

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

Eseguire l'applicazione e passare a */movies/index*. Provare a eseguire una ricerca in base al genere, al nome del film e a entrambi i criteri.

![](adding-search/_static/image8.png)

In questa sezione sono stati creati un metodo e una visualizzazione di azione di ricerca che consentono agli utenti di eseguire ricerche in base al titolo e al genere. Nella sezione successiva verrà illustrato come aggiungere una proprietà al `Movie` modello e come aggiungere un inizializzatore che creerà automaticamente un database di prova.

> [!div class="step-by-step"]
> [Precedente](examining-the-edit-methods-and-edit-view.md) 
>  [Avanti](adding-a-new-field.md)
