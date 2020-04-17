---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implementazione di un paging efficiente dei dati Documenti Microsoft
author: rick-anderson
description: Passo 8 mostra come aggiungere il supporto di paging al nostro / Dinners URL in modo che invece di visualizzare 1000s di cene in una sola volta, mostreremo solo 10 cene imminenti a...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: a833553fe44b62b136f7eb55c7e00eca0b0462c6
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541325"
---
# <a name="implement-efficient-data-paging"></a>Implementare una suddivisione in pagine efficiente dei dati

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 8 di un'esercitazione gratuita [sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come compilare un'applicazione Web piccola ma completa usando ASP.NET MVC 1.
> 
> Passo 8 mostra come aggiungere il supporto di paging al nostro / Dinners URL in modo che invece di visualizzare 1000s di cene in una sola volta, mostreremo solo 10 cene imminenti alla volta - e consentire agli utenti finali di pagina avanti e indietro attraverso l'intero elenco in modo SEO friendly.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [ALL'archivio musicale MVC.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-8-paging-support"></a>Passaggio 8 di NerdDinnerNerdDinner Step 8: Paging Support

Se il nostro sito ha successo, avrà migliaia di cene imminenti. Dobbiamo assicurarci che la nostra interfaccia utente sia ridimensionata per gestire tutte queste cene e consenta agli utenti di sfogliarle. Per abilitare questa funzionalità, aggiungeremo il supporto di paging all'URL */Dinners* in modo che invece di visualizzare 1000 cene contemporaneamente, visualizzeremo solo 10 cene imminenti alla volta e consentireagli agli utenti finali di scorrere avanti e indietro l'intero elenco in modo SEO friendly.

### <a name="index-action-method-recap"></a>Riassunto del metodo di azione Index()

Il metodo di azione Index() all'interno della nostra classe DinnersController è attualmente simile al seguente:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Quando viene effettuata una richiesta all'URL */Dinners,* recupera un elenco di tutte le cene imminenti e quindi esegue il rendering di un elenco di tutte le:When a request is made to the /Dinners URL, it retrieves a list of all upcoming dinners and then renders a listing of all of them out:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iqueryablelttgt"></a>Informazioni su&lt;IQueryable TUnderstanding IQueryable T&gt;

*IQueryable&lt;&gt; T* è un'interfaccia che è stata introdotta con LINQ come parte di .NET 3.5. Consente potenti scenari di "esecuzione posticipata" che è possibile sfruttare per implementare il supporto di paging.

Nel nostro DinnerRepository stiamo restituendo&lt;&gt; una sequenza IQueryable Dinner dal nostro metodo FindUpcomingDinners():

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

L'oggetto&lt;IQueryable Dinner&gt; restituito dal metodo FindUpcomingDinners() incapsula una query per recuperare gli oggetti Dinner dal database utilizzando LINQ to SQLLINQ to SQL. È importante sottolineare che la query non verrà eseguita sul database fino a quando non tentiamo di accedere/iterare i dati nella query o finché non viene chiamato il metodo ToList() su di esso. Il codice che chiama il nostro metodo FindUpcomingDinners() può scegliere di aggiungere ulteriori&lt;&gt; operazioni/filtri "concatenati" all'oggetto IQueryable Dinner prima di eseguire la query. LINQ to SQLLINQ to SQL è quindi abbastanza intelligente per eseguire la query combinata sul database quando vengono richiesti i dati.

Per implementare la logica di paging possiamo aggiornare il nostro metodo di azione Index() di DinnersController&lt;in&gt; modo che applichi ulteriori operatori "Skip" e "Take" alla sequenza IQueryable Dinner restituita prima di chiamare ToList() su di esso:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

Il codice precedente salta le prime 10 cene imminenti nel database e quindi restituisce 20 cene. LINQ to SQLLINQ to SQL è abbastanza intelligente da costruire una query SQL ottimizzata che esegue questa logica di salto nel database SQL e non nel server Web. Ciò significa che anche se abbiamo milioni di cene imminenti nel database, solo il 10 che vogliamo verrà recuperato come parte di questa richiesta (rendendolo efficiente e scalabile).

### <a name="adding-a-page-value-to-the-url"></a>Aggiunta di un valore di "pagina" all'URL

Invece di codificare a livello di codice un intervallo di pagine specifico, vorremo che i nostri URL includano un parametro "pagina" che indica quale intervallo Dinner un utente sta richiedendo.

#### <a name="using-a-querystring-value"></a>Utilizzo di un valore QuerystringUsing a Querystring value

Il codice seguente illustra come è possibile aggiornare il metodo di azione Index() per supportare un parametro querystring e abilitare URL come */Dinners?page:*

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

Il metodo di azione Index() riportato sopra include un parametro denominato "page". Il parametro viene dichiarato come un numero intero nullable (che è ciò che int? indica). Ciò significa che l'URL */Dinners?page-2* causerà il passaggio di un valore di "2" come valore del parametro. L'URL */Dinners* (senza un valore querystring) causerà un valore null da passare.

Stiamo moltiplicando il valore della pagina per la dimensione della pagina (in questo caso 10 righe) per determinare quante cene saltare. Si usa [l'operatore di "coalescing" null di C' (??)](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) che è utile quando si gestiscono tipi nullable. Il codice precedente assegna alla pagina il valore 0 se il parametro page è null.

#### <a name="using-embedded-url-values"></a>Utilizzo dei valori URL incorporati

Un'alternativa all'utilizzo di un valore querystring consiste nell'incorporare il parametro della pagina all'interno dell'URL effettivo stesso. Ad esempio: */Dinners/Page/2* o */Dinners/2*. ASP.NET MVC include un potente motore di routing degli URL che semplifica il supporto di scenari come questo.

Possiamo registrare regole di routing personalizzate che eseguono il mapping di qualsiasi URL o formato URL in ingresso a qualsiasi classe controller o metodo di azione desiderato. Tutto quello che dobbiamo fare è aprire il file Global.asax all'interno del nostro progetto:

![](implement-efficient-data-paging/_static/image2.png)

Quindi registrare una nuova regola di mapping utilizzando il metodo helper MapRoute() come la prima chiamata alle route. MapRoute() di seguito:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Sopra stiamo registrando una nuova regola di routing denominata "UpcomingDinners". Stiamo indicando che ha il formato dell'URL "Dinners/Page/'page'" – dove "page" è un valore di parametro incorporato all'interno dell'URL. Il terzo parametro del metodo MapRoute() indica che è necessario eseguire il mapping di URL che corrispondono a questo formato con il metodo di azione Index() nella classe DinnersController.

Possiamo usare esattamente lo stesso codice Index() che avevamo prima con il nostro scenario Querystring – tranne ora che il nostro parametro "page" verrà dall'URL e non dalla stringa di query:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

E ora quando eseguiamo l'applicazione e digitiamo in */Dinners* vedremo le prime 10 cene imminenti:

![](implement-efficient-data-paging/_static/image3.png)

E quando digitiamo in */Dinners/Page/1* vedremo la prossima pagina di cene:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Aggiunta dell'interfaccia utente di spostamento tra le pagine

L'ultimo passaggio per completare lo scenario di paging consiste nell'implementare l'interfaccia utente di spostamento "successivo" e "precedente" all'interno del modello di visualizzazione per consentire agli utenti di ignorare facilmente i dati della cena.

Per implementare correttamente, è necessario conoscere il numero totale di cene nel database, nonché il numero di pagine di dati che questo si traduce in. Dovremo quindi calcolare se il valore di "pagina" attualmente richiesto si trova all'inizio o alla fine dei dati e mostrare o nascondere di conseguenza l'interfaccia utente "precedente" e "successiva". Potremmo implementare questa logica all'interno del nostro metodo di azione Index(). In alternativa è possibile aggiungere una classe helper al progetto che incapsula questa logica in modo più riutilizzabile.

Di seguito è riportata una semplice classe helper&lt;"PaginatedList" che deriva dalla classe di raccolta List T&gt; incorporata in .NET Framework. Implementa una classe di raccolta riutilizzabile che può essere utilizzata per impaginare qualsiasi sequenza di dati IQueryable. Nella nostra applicazione NerdDinner avremo lavorare su&lt;IQueryable Dinner risultati, ma potrebbe altrettanto facilmente essere utilizzato contro IQueryable Product o IQueryable Customer risultati in altri scenari di applicazione:In our NerdDinner application we'll have work over IQueryable Dinner&gt; results,&lt;but it could as easily be used against IQueryable Product&gt; or IQueryable&lt;Customer&gt; results in other application scenarios:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Si noti sopra come calcola e quindi espone proprietà come "PageIndex", "PageSize", "TotalCount" e "TotalPages". Espone inoltre due proprietà di supporto "HasPreviousPage" e "HasNextPage" che indicano se la pagina di dati nella raccolta si trova all'inizio o alla fine della sequenza originale. Il codice precedente causerà l'esecuzione di due query SQL , la prima per recuperare il conteggio del numero totale di Dinner oggetti (questo non restituisce gli oggetti , anzi esegue un"SELECT COUNT" istruzione che restituisce un numero intero) e il secondo per recuperare solo le righe di dati necessarie dal database per la pagina corrente di dati.

È quindi possibile aggiornare il metodo helper DinnersController.Index() per creare una cena&lt;&gt; Di Cui un PaginatedList dal risultato DinnerRepository.FindUpcomingDinners() e passarla al modello di visualizzazione:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

È quindi possibile aggiornare il modello di visualizzazione di ,Views, Dinners, Index.aspx in&gt; &gt; modo che&lt;erediti da ViewPage&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner anziché ViewPage IEnumerable&lt;Dinner&gt;&gt;, quindi aggiungere il codice seguente nella parte inferiore del modello di visualizzazione per visualizzare o nascondere l'interfaccia utente di spostamento successiva e precedente:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Si noti sopra come stiamo usando il metodo helper Html.RouteLink() per generare i nostri collegamenti ipertestuali. Questo metodo è simile al metodo helper Html.ActionLink() che abbiamo usato in precedenza. La differenza è che stiamo generando l'URL utilizzando la regola di routing "UpcomingDinners" che abbiamo impostato all'interno del nostro file Global.asax. In questo modo verranno generati URL per il metodo di azione Index() con il formato: */Dinners/Page/'page',* dove il valore di "page" è una variabile che stiamo fornendo sopra in base all'attuale PageIndex.

E ora quando eseguiamo di nuovo la nostra applicazione vedremo 10 cene alla volta nel nostro browser:

![](implement-efficient-data-paging/_static/image5.png)

&lt; &lt; Abbiamo &lt; anche &gt; &gt; &gt; e l'interfaccia utente di navigazione nella parte inferiore della pagina che ci permette di saltare avanti e indietro sui nostri dati utilizzando gli URL accessibili al motore di ricerca:

![](implement-efficient-data-paging/_static/image6.png)

| **Argomento collaterale: Informazioni sulle&lt;implicazioni di IQueryable T&gt;** |
| --- |
| IQueryable&lt;&gt; T è una funzionalità molto potente che consente una varietà di interessanti scenari di esecuzione differita (ad esempio il paging e le query basate sulla composizione). Come con tutte le caratteristiche potenti, si vuole stare attenti con come lo si utilizza e assicurarsi che non è abusato. È importante riconoscere che la restituzione&lt;&gt; di un risultato T IQueryable dal repository consente al codice chiamante di aggiungervi metodi dell'operatore concatenati e quindi partecipare all'esecuzione finale della query. Se non si desidera fornire al codice chiamante questa&lt;funzionalità, è necessario restituire i risultati di IList T&gt; o IEnumerable&lt;T,&gt; che contengono i risultati di una query già eseguita. Per gli scenari di impaginazione, è necessario eseguire il push della logica di impaginazione dei dati effettivi nel metodo del repository chiamato. In questo scenario è possibile aggiornare il metodo FindUpcomingDinners() finder per avere una&lt; &gt; firma che ha restituito un PaginatedList: PaginatedList Dinner FindUpcomingDinners(int pageIndex, int pageSize) - oppure restituire un iList&lt;Dinner&gt;e utilizzare un parametro out "totalCount" per restituire il conteggio totale di Dinners: IList&lt;Dinner&gt; FindUpcomingDinners(int pageIndex, int pageSize, out int count) . |

### <a name="next-step"></a>passaggio successivo

Esaminiamo ora come è possibile aggiungere il supporto di autenticazione e autorizzazione all'applicazione.

> [!div class="step-by-step"]
> [Successivo](re-use-ui-using-master-pages-and-partials.md)
> [precedente](secure-applications-using-authentication-and-authorization.md)
