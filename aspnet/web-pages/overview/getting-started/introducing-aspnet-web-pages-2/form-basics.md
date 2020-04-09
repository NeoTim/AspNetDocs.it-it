---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: Introduzione ASP.NET pagine Web - Nozioni di base sui moduli HTML Documenti Microsoft
author: Rick-Anderson
description: In questa esercitazione vengono illustrate le nozioni di base su come creare un modulo di input e su come gestire l'input dell'utente quando si utilizza ASP.NET pagine Web (Razor). E ora che...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: f57661077ec3bb13f3d4ec41b130bda4d2fb9070
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676328"
---
# <a name="introducing-aspnet-web-pages---html-form-basics"></a>Introduzione a pagine Web ASP.NET - Nozioni di base sui moduli HTML

 di [Tom FitzMacken](https://github.com/tfitzmac)

> In questa esercitazione vengono illustrate le nozioni di base su come creare un modulo di input e su come gestire l'input dell'utente quando si utilizza ASP.NET pagine Web (Razor). E ora che hai un database, userai le tue abilità di modulo per consentire agli utenti di trovare film specifici nel database. Si presuppone che la serie sia stata completata [tramite Introduzione alla visualizzazione dei dati mediante ASP.NET pagine Web](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> Contenuto dell'esercitazione:
> 
> - Come creare un modulo utilizzando elementi HTML standard.
> - Come leggere l'input dell'utente in un modulo.
> - Come creare una query SQL che ottiene in modo selettivo i dati utilizzando un termine di ricerca fornito dall'utente.
> - Come avere campi nella pagina "ricordare" ciò che l'utente ha inserito.
>   
> 
> Caratteristiche/tecnologie discusse:
> 
> - Oggetto `Request`.
> - Clausola `Where` SQL.

## <a name="what-youll-build"></a>Scopo dell'esercitazione

Nell'esercitazione precedente è stato creato un database, vi `WebGrid` sono stati aggiunti dati e quindi è stato usato l'helper per visualizzare i dati. In questa esercitazione si aggiungerà una casella di ricerca che consente di trovare filmati di un genere specifico o il cui titolo contiene qualsiasi parola immessa. (Ad esempio, sarete in grado di trovare tutti i film il cui genere è "Azione" o il cui titolo contiene "Harry" o "Avventura.")

Quando hai finito con questo tutorial, avrai una pagina come questa:

![Pagina Film con ricerca di generi e titoli](form-basics/_static/image1.png)

La parte elenco della pagina è la &mdash; stessa dell'ultima esercitazione di una griglia. La differenza sarà che la griglia mostrerà solo i film che hai cercato.

## <a name="about-html-forms"></a>Informazioni sui moduli HTML

(Se hai esperienza con la creazione di moduli `GET` `POST`HTML e con la differenza tra e , puoi saltare questa sezione.)

Un modulo include &mdash; caselle di testo, pulsanti, pulsanti di opzione, caselle di controllo, elenchi a discesa e così via per un modulo. Gli utenti compilano questi controlli o effettuano selezioni e quindi inviano il modulo facendo clic su un pulsante.

La sintassi HTML di base di un modulo è illustrata da questo esempio:

[!code-html[Main](form-basics/samples/sample1.html)]

Quando questo markup viene eseguito in una pagina, crea un modulo semplice simile a questa illustrazione:When this markup runs in a page, it creates a simple form that looks like this illustration:

![Modulo HTML di base come visualizzato nel browser](form-basics/_static/image2.png)

L'elemento `<form>` racchiude gli elementi HTML da inviare. (Un errore facile da fare è quello di aggiungere elementi `<form>` alla pagina, ma poi dimenticare di metterli all'interno di un elemento. In tal caso, non viene presentato nulla.) L'attributo `method` indica al browser come inviare l'input dell'utente. Impostare questa `post` opzione su se si sta eseguendo `get` un aggiornamento sul server o su se si stanno solo recuperando dati dal server.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **Sicurezza dei verbi GET, POST e HTTP**
> 
> HTTP, il protocollo che i browser e i server utilizzano per lo scambio di informazioni, è notevolmente semplice nelle sue operazioni di base. I browser utilizzano solo pochi verbi per effettuare richieste ai server. Quando si scrive codice per il Web, è utile comprendere questi verbi e come il browser e il server li utilizzano. Di lontano i verbi più comunemente utilizzati sono i seguenti:
> 
> - `GET`. Il browser utilizza questo verbo per recuperare qualcosa dal server. Ad esempio, quando si digita un URL nel `GET` browser, il browser esegue un'operazione per richiedere la pagina desiderata. Se la pagina include elementi `GET` grafici, il browser esegue operazioni aggiuntive per ottenere le immagini. Se `GET` l'operazione deve passare informazioni al server, le informazioni vengono passate come parte dell'URL nella stringa di query.
> - `POST`. Il browser `POST` invia una richiesta per inviare i dati da aggiungere o modificare sul server. Ad esempio, `POST` il verbo viene utilizzato per creare record in un database o modificare quelli esistenti. La maggior parte delle volte, quando si compila un modulo `POST` e si fa clic sul pulsante Invia, il browser esegue un'operazione. In `POST` un'operazione, i dati passati al server si trovano nel corpo della pagina.
> 
> Una distinzione importante tra questi `GET` verbi è che un'operazione non dovrebbe cambiare nulla sul server `GET` - o per metterlo in modo leggermente più astratto, un'operazione non comporta una modifica dello stato sul server. È possibile `GET` eseguire un'operazione sulle stesse risorse tutte le volte che si desidera e tali risorse non cambiano. (Un'operazione `GET` è spesso detta "sicura", o per usare un termine tecnico, è *idempotente*.) Al contrario, naturalmente, una `POST` richiesta cambia qualcosa sul server ogni volta che si esegue l'operazione.
> 
> Due esempi aiuteranno a illustrare questa distinzione. Quando si esegue una ricerca utilizzando un motore come Bing o Google, si compila un modulo costituito da una casella di testo e quindi si fa clic sul pulsante di ricerca. Il browser `GET` esegue un'operazione, con il valore immesso nella casella passato come parte dell'URL. L'utilizzo di un'operazione `GET` per questo tipo di modulo va bene, perché un'operazione di ricerca non modifica le risorse sul server, ma solo recupera le informazioni.
> 
> Ora considera il processo di ordinare qualcosa online. Compilare i dettagli dell'ordine e quindi fare clic sul pulsante Invia. Questa operazione sarà `POST` una richiesta, perché l'operazione comporterà modifiche sul server, ad esempio un nuovo record di ordine, una modifica delle informazioni dell'account e forse molte altre modifiche. A `GET` differenza dell'operazione, `POST` non è possibile ripetere la richiesta: se lo è stato, ogni volta che la richiesta viene inviata nuovamente, si genererà un nuovo ordine sul server. (In casi come questo, i siti web spesso ti avvisano di non fare clic su un pulsante di invio più di una volta, o disabiliteranno il pulsante di invio in modo da non inviare nuovamente il modulo accidentalmente.)
> 
> Nel corso di questa esercitazione, si `GET` userà `POST` sia un'operazione che un'operazione per lavorare con i moduli HTML. Spiegheremo in ogni caso perché il verbo che usi è quello appropriato.
> 
> Per ulteriori informazioni sui verbi HTTP, vedere l'articolo [sulle definizioni](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) dei metodi nel sito W3C.

La maggior parte `<input>` degli elementi di input dell'utente sono elementi HTML. Hanno un `<input type="type" name="name">,` aspetto simile al punto in cui *il tipo* indica il tipo di controllo di input dell'utente desiderato. Questi elementi sono quelli comuni:

- Casella di testo:`<input type="text">`
- Casella di controllo:`<input type="check">`
- Pulsante di opzione:`<input type="radio">`
- Pulsante:`<input type="button">`
- Pulsante Invia:`<input type="submit">`

È inoltre possibile `<textarea>` utilizzare l'elemento per creare `<select>` una casella di testo su più righe e l'elemento per creare un elenco a discesa o un elenco scorrevole. Per ulteriori informazioni sugli elementi modulo HTML, vedere [Moduli HTML e input](http://www.w3schools.com/html/html_forms.asp) nel sito W3Schools.

L'attributo `name` è molto importante, perché il nome è come si otterrà il valore dell'elemento in un secondo momento, come si vedrà a breve.

La parte interessante è ciò che tu, lo sviluppatore della pagina, fai con l'input dell'utente. Non esiste alcun comportamento incorporato associato a questi elementi. Invece, è necessario ottenere i valori che l'utente ha inserito o selezionato ed eseguire un'operazione con loro. Questo è quello che imparerai in questo tutorial.

> [!TIP] 
> 
> **HTML5 e moduli di input**
> 
> Come forse saprai, HTML è in transizione e la versione più recente (HTML5) include il supporto per modi più intuitivi per gli utenti di inserire informazioni. Ad esempio, in HTML5, puoi dire alla pagina che vuoi che l'utente immetta una data. Il browser può quindi visualizzare automaticamente un calendario anziché richiedere all'utente di immettere una data manualmente. Tuttavia, HTML5 è nuovo e non è ancora supportato in tutti i browser.
> 
> ASP.NET pagine Web supporta l'input HTML5 nella misura in cui il browser dell'utente supporta. Per un'idea dei nuovi `<input>` attributi per l'elemento in HTML5, consultate Tipo di [input &lt;&gt; HTML Attribute](http://www.w3schools.com/html/html_form_input_types.asp) nel sito W3Schools.

## <a name="creating-the-form"></a>Creazione del form

In WebMatrix, nell'area di lavoro **File,** aprire la pagina *Movies.cshtml.In* WebMatrix, in the Files workspace, open the Movies.cshtml page.

Dopo il `</h1>` tag di `<div>` chiusura `grid.GetHtml` e prima del tag di apertura della chiamata, aggiungere il markup seguente:

[!code-html[Main](form-basics/samples/sample2.html)]

Questo markup crea un modulo con `searchGenre` una casella di testo denominata e un pulsante di invio. La casella di testo e il `<form>` pulsante di invio sono racchiusi in un elemento il cui `method` attributo è impostato su `get`. (Ricorda che se non si inserisce la casella `<form>` di testo e il pulsante di invio all'interno di un elemento, non verrà inviato nulla quando si fa clic sul pulsante.) Si utilizza `GET` il verbo qui perché si sta creando un modulo che non apporta alcuna modifica sul server - si traduce solo in una ricerca. (Nell'esercitazione precedente, `post` è stato utilizzato un metodo, che è il modo in cui si inviano le modifiche al server. Vedrai che nel prossimo tutorial di nuovo.)

Eseguire la pagina. Anche se non è stato definito alcun comportamento per il modulo, è possibile visualizzarne l'aspetto:

![Pagina Film con casella di ricerca per Genere](form-basics/_static/image3.png)

Immettere un valore nella casella di testo, ad esempio "Commedia". Quindi fare clic su **Cerca genere**.

Prendere nota dell'URL della pagina. Poiché si `<form>` imposta `method` l'attributo dell'elemento su `get`, il valore immesso fa ora parte della stringa di query nell'URL, in questo modo:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Lettura dei valori dei moduli

La pagina contiene già del codice che ottiene i dati del database e visualizza i risultati in una griglia. A questo punto è necessario aggiungere del codice che legge il valore della casella di testo in modo da poter eseguire una query SQL che include il termine di ricerca.

Poiché si imposta il `get`metodo del form su , è possibile leggere il valore immesso nella casella di testo utilizzando codice simile al seguente:

`var searchTerm = Request.QueryString["searchGenre"];`

L'oggetto `Request.QueryString` `QueryString` (la `Request` proprietà dell'oggetto) include i valori degli `GET` elementi inviati come parte dell'operazione. La `Request.QueryString` proprietà contiene una *raccolta* (un elenco) dei valori inviati nel modulo. Per ottenere un singolo valore, specificare il nome dell'elemento desiderato. Ecco perché è necessario disporre `name` di `<input>` un`searchTerm`attributo sull'elemento ( ) che crea la casella di testo. (Per ulteriori `Request` informazioni sull'oggetto, vedere la [barra laterale](#BKMK_TheRequestObject) più avanti.)

È abbastanza semplice leggere il valore della casella di testo. Ma se l'utente non ha inserito nulla nella casella di testo, ma ha fatto clic su **Cerca** comunque, è possibile ignorare tale clic, dal momento che non c'è nulla da cercare.

Il codice seguente è un esempio che illustra come implementare queste condizioni. (Non è necessario aggiungere questo codice ancora; lo farai in un momento.)

[!code-csharp[Main](form-basics/samples/sample3.cs)]

Il test si interrompe in questo modo:

- Ottenere il `Request.QueryString["searchGenre"]`valore di , ovvero il `<input>` valore `searchGenre`immesso nell'elemento denominato .
- Scopri se è vuoto utilizzando `IsEmpty` il metodo . Questo metodo è il modo standard per determinare se un elemento (ad esempio, un elemento form) contiene un valore. Ma in realtà, ti importa solo se *non* è vuoto, quindi ...
- Aggiungere `!` l'operatore `IsEmpty` prima del test. (L'operatore `!` significa NOT logico).

In parole povere, l'intera `if` condizione si traduce nel seguente: Se *l'elemento searchGenre del modulo non è vuoto, allora ...*

Questo blocco imposta la fase per la creazione di una query che utilizza il termine di ricerca. Questa operazione viene illustrata nella sezione seguente.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **Oggetto Request**
> 
> L'oggetto `Request` contiene tutte le informazioni che il browser invia all'applicazione quando viene richiesta o inviata una pagina. Questo oggetto include tutte le informazioni fornite dall'utente, ad esempio i valori delle caselle di testo o un file da caricare. Include anche tutti i tipi di informazioni aggiuntive, come i cookie, i valori nella stringa di query URL (se presente), il percorso del file della pagina in esecuzione, il tipo di browser che l'utente sta utilizzando, l'elenco delle lingue impostate nel browser e molto altro ancora.
> 
> L'oggetto `Request` è una *raccolta* (elenco) di valori. Si ottiene un singolo valore dalla raccolta specificandone il nome:
> 
> `var someValue = Request["name"];`
> 
> L'oggetto `Request` espone effettivamente diversi sottoinsiemi. Ad esempio:
> 
> - `Request.Form`fornisce i valori degli `<form>` elementi all'interno `POST` dell'elemento inviato se la richiesta è una richiesta.
> - `Request.QueryString`fornisce solo i valori nella stringa di query dell'URL. In un URL `http://mysite/myapp/page?searchGenre=action&page=2`come `?searchGenre=action&page=2` , la sezione dell'URL è la stringa di query.
> - `Request.Cookies`la raccolta consente di accedere ai cookie che il browser ha inviato.
> 
> Per ottenere un valore che si sa è `Request["name"]`nel modulo inviato, è possibile utilizzare . In alternativa, è possibile utilizzare `Request.Form["name"]` le `POST` versioni `Request.QueryString["name"]` più `GET` specifiche (per le richieste) o (per le richieste). Naturalmente, *nome* è il nome dell'elemento da ottenere.
> 
> Il nome dell'elemento che si desidera ottenere deve essere univoco all'interno della raccolta in uso. Ecco perché l'oggetto `Request` fornisce i `Request.Form` `Request.QueryString`sottoinsiemi come e . Si supponga che la pagina `userName` contenga un `userName`elemento form denominato e *che contenga anche* un cookie denominato . Se si `Request["userName"]`ottiene , è ambiguo se si desidera il valore del modulo o il cookie. Tuttavia, se `Request.Form["userName"]` `Request.Cookie["userName"]`si ottiene o , si sta essendo esplicito su quale valore ottenere.
> 
> È buona norma essere specifici e utilizzare `Request` il sottoinsieme di che `Request.Form` `Request.QueryString`ti interessa, come o . Per le pagine semplici che si sta creando in questo tutorial, probabilmente non fa alcuna differenza. Tuttavia, quando si creano pagine più `Request.Form` `Request.QueryString` complesse, utilizzando la versione esplicita o può aiutare a evitare problemi che possono verificarsi quando la pagina contiene un modulo (o più moduli), cookie, valori di stringa di query e così via.

## <a name="creating-a-query-by-using-a-search-term"></a>Creazione di una query tramite un termine di ricerca

Ora che si conosce come ottenere il termine di ricerca immesso dall'utente, è possibile creare una query che lo utilizza. Tenere presente che per ottenere tutti gli elementi del filmato dal database, si sta utilizzando una query SQL che assomiglia a questa istruzione:

`SELECT * FROM Movies`

Per ottenere solo determinati filmati, è necessario `Where` utilizzare una query che include una clausola. Questa clausola consente di impostare una condizione sulle righe restituite dalla query. Ad esempio:

`SELECT * FROM Movies WHERE Genre = 'Action'`

Il formato `WHERE column = value`di base è . È possibile utilizzare diversi `=`operatori `>` oltre a `<` , come `<>` (maggiore di), `<=` (minore di), (non uguale a), (minore o uguale a), ecc., a seconda di ciò che si sta cercando.

Nel caso in cui vi state chiedendo, &mdash; `SELECT` le istruzioni `Select` SQL `select`non sono case sensitive è lo stesso come (o anche ). Tuttavia, gli utenti spesso sfruttano `SELECT` in `WHERE`maiuscolo le parole chiave in un'istruzione SQL, ad esempio e , per semplificarne la lettura.

### <a name="passing-the-search-term-as-a-parameter"></a>Passaggio del termine di ricerca come parametroPassing the search term as a parameter

La ricerca di un genere`WHERE Genre = 'Action'`specifico è abbastanza facile ( ), ma si desidera essere in grado di cercare qualsiasi genere che l'utente immette. A tale scopo, creare come query SQL che include un segnaposto per il valore da cercare. Sarà simile a questo comando:

`SELECT * FROM Movies WHERE Genre = @0`

Il segnaposto `@` è il carattere seguito da zero. Come si può intuire, una query può contenere `@1` `@2`più segnaposto e sarebbero denominati `@0`, , , e così via.

Per impostare la query ed effettivamente passarla il valore, utilizzare il codice simile al seguente:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Questo codice è simile a quello già fatto per visualizzare i dati nella griglia. Le uniche differenze sono:

- La query contiene`WHERE Genre = @0"`un segnaposto ( ).
- La query viene inserita`selectCommand`in una variabile ( ); in precedenza, la query `db.Query` è stata passata direttamente al metodo.
- Quando si `db.Query` chiama il metodo, si passano sia la query che il valore da utilizzare per il segnaposto. Se la query contiene più segnaposto, è necessario passarli tutti come valori separati al metodo.

Se si mettono insieme tutti questi elementi, si ottiene il codice seguente:If you put all these elements together, you get the following code:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Importante!** L'utilizzo di `@0`segnaposto (ad esempio ) per passare valori a un comando SQL è *estremamente importante* per la sicurezza. Il modo in cui lo vedi qui, con segnaposto per i dati variabili, è l'unico modo in cui dovresti costruire i comandi SQL.
> 
> Non costruire mai un'istruzione SQL mettendo insieme (concatenando) il testo letterale e i valori ottenuti dall'utente. Concatenando l'input dell'utente in un'istruzione SQL apre il sito a un *attacco SQL injection* in cui un utente malintenzionato invia valori alla pagina che violano il database. (È possibile leggere di più nell'articolo [SQL Injection](https://msdn.microsoft.com/library/ms161953.aspx) il sito Web MSDN.)

## <a name="updating-the-movies-page-with-search-code"></a>Aggiornamento della pagina Film con il codice di ricerca

A questo punto è possibile aggiornare il codice nel file *Movies.cshtml.* Per iniziare, sostituisci il codice nel blocco di codice nella parte superiore della pagina con questo codice:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

La differenza qui è che hai inserito la query nella `selectCommand` `db.Query` variabile, che si passerà in seguito. L'inserimento dell'istruzione SQL in una variabile consente di modificare l'istruzione, che è ciò che si farà per eseguire la ricerca.

Hai anche rimosso queste due righe, che verrà rimesso in seguito:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Non si desidera eseguire ancora la query, `db.Query`ovvero chiamare , e non `WebGrid` si desidera inizializzare l'helper. Si farà queste cose dopo aver capito quale istruzione SQL deve essere eseguita.

Dopo questo blocco riscritto, è possibile aggiungere la nuova logica per la gestione della ricerca. Il codice completato sarà simile al seguente. Aggiornare il codice nella pagina in modo che corrisponda a questo esempio:Update the code in your page so it matches this example:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

La pagina ora funziona in questo modo. Ogni volta che la pagina viene eseguita, il codice apre il database e la `selectCommand` variabile viene impostata sull'istruzione SQL che ottiene tutti i record dalla `Movies` tabella. Il codice inizializza `searchTerm` anche la variabile.

Tuttavia, se la richiesta corrente `searchGenre` include un `selectCommand` valore per l'elemento, il codice `Where` imposta una query diversa, ovvero una che include la clausola per cercare un genere. Si imposta `searchTerm` anche a tutto ciò che è stato passato per la casella di ricerca (che potrebbe essere nulla).

Indipendentemente dall'istruzione SQL `selectCommand`in , `db.Query` il codice chiama per eseguire `searchTerm`la query, passando tutto ciò che si trova in . Se non c'è niente in `searchTerm`, non importa, perché in questo caso `selectCommand` non c'è nessun parametro per passare il valore a comunque.

Infine, il codice `WebGrid` inizializza l'helper usando i risultati della query, proprio come prima.

Si può vedere che inserendo l'istruzione SQL e il termine di ricerca in variabili, è stata aggiunta flessibilità al codice. Come si vedrà più avanti in questa esercitazione, è possibile usare questo framework di base e continuare ad aggiungere logica per diversi tipi di ricerche.

## <a name="testing-the-search-by-genre-feature"></a>Test della funzionalità Search-by-Genre

In WebMatrix eseguire la pagina *Movies.cshtml.* Viene visualizzata la pagina con la casella di testo per genere.

Inserisci un genere che hai inserito per uno dei record di test, quindi fai clic su **Cerca**. Questa volta vedi un elenco dei solo film che corrispondono a quel genere:

![Elenco della pagina dei film dopo la ricerca del genere 'Comedies'](form-basics/_static/image4.png)

Inserisci un genere diverso e cerca di nuovo. Provare a immettere il genere utilizzando tutte le lettere minuscole o tutte maiuscole in modo da vedere che la ricerca non fa distinzione tra maiuscole e minuscole.

## <a name="remembering-what-the-user-entered"></a>"Ricordare" ciò che l'utente ha inserito

Potresti aver notato che dopo aver inserito un genere e cliccato **Search Genre**, hai visto un elenco per quel genere. Tuttavia, la casella &mdash; di testo di ricerca era vuota, in altre parole, non ricordava ciò che avevi inserito.

È importante comprendere perché si verifica questo comportamento. Quando si invia una pagina, il browser invia una richiesta al server Web. Quando ASP.NET ottiene la richiesta, crea una nuova istanza della pagina, esegue il codice in essa contenuto e quindi esegue nuovamente il rendering della pagina nel browser. In effetti, però, la pagina non sa che stavi solo lavorando con una versione precedente di se stessa. Tutto quello che sa è che ha ottenuto una richiesta che aveva alcuni dati del modulo in esso.

Ogni volta che &mdash; richiedi una pagina, sia per &mdash; la prima volta che inviandola, riceverai una nuova pagina. Il server Web non ha memoria dell'ultima richiesta. Né ASP.NET, e nemmeno il browser. L'unica connessione tra queste istanze separate della pagina è qualsiasi dato trasmesso tra di essi. Se si invia una pagina, ad esempio, la nuova istanza della pagina può ottenere i dati del modulo inviati dall'istanza precedente. (Un altro modo per passare i dati tra le pagine consiste nell'utilizzare i cookie.)

Un modo formale per descrivere questa situazione consiste nel dire che le pagine web sono *senza stato.* I server Web e le pagine stesse e gli elementi nella pagina non mantengono alcuna informazione sullo stato precedente di una pagina. Il web è stato progettato in questo modo perché mantenere lo stato per le singole richieste esaurirebbe rapidamente le risorse dei server web, che spesso gestiscono migliaia, forse anche centinaia di migliaia, di richieste al secondo.

Ecco perché la casella di testo era vuota. Dopo aver inviato la pagina, ASP.NET creata una nuova istanza della pagina ed è stata eseguita attraverso il codice e il markup. Non c'era nulla in quel codice che ASP.NET detto di inserire un valore nella casella di testo. Quindi ASP.NET non ha fatto nulla, e la casella di testo è stato eseguito il rendering senza un valore in esso.

C'è in realtà un modo semplice per aggirare questo problema. Il genere immesso nella *is* casella di testo &mdash; è disponibile `Request.QueryString["searchGenre"]`nel codice in .

Aggiornare il markup per la `value` casella di `searchTerm`testo in modo che l'attributo ottenga il relativo valore da , come nell'esempio seguente:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

In questa pagina, potresti `value` anche impostare l'attributo sulla `searchTerm` variabile, poiché tale variabile contiene anche il genere che hai inserito. Ma utilizzando `Request` l'oggetto `value` per impostare l'attributo come illustrato di seguito è il modo standard per eseguire questa attività. (Supponendo che si desidera &mdash; anche eseguire questa operazione in alcune situazioni, si potrebbe desiderare di eseguire il rendering della pagina *senza* valori nei campi. Tutto dipende da cosa sta succedendo con la tua app.)

> [!NOTE]
> Non è possibile "ricordare" il valore di una casella di testo utilizzata per le password. Sarebbe un buco di sicurezza per consentire alle persone di compilare un campo password utilizzando il codice.

Eseguire nuovamente la pagina, immettere un genere e fare clic su **Cerca genere**. Questa volta non solo vedi i risultati della ricerca, ma la casella di testo ricorda ciò che hai inserito l'ultima volta:

![Pagina che mostra che la casella di testo ha 'ricordato' la voce precedente](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Ricerca di qualsiasi parola nel titolo

È ora possibile cercare qualsiasi genere, ma si potrebbe anche desiderare di cercare un titolo. È difficile ottenere un titolo esattamente corretto quando si esegue la ricerca, quindi è possibile cercare una parola che appare in qualsiasi punto all'interno di un titolo. A tale scopo in SQL, utilizzare l'operatore `LIKE` e la sintassi come il seguente:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Questo comando ottiene tutti i film i cui titoli contengono "avventura". Quando si `LIKE` utilizza l'operatore, `%` si include il carattere jolly come parte del termine di ricerca. La `LIKE 'adventure%'` ricerca significa "iniziare con 'avventura'". (Tecnicamente, significa "La stringa 'avventura' seguita da qualsiasi cosa.") Allo stesso modo, `LIKE '%adventure'` il termine di ricerca significa "qualsiasi cosa seguita dalla stringa 'avventura'", che è un altro modo per dire "termina con 'avventura'".

Il termine `LIKE '%adventure%'` di ricerca significa quindi "con 'avventura' in qualsiasi punto del titolo". (Tecnicamente, "qualsiasi cosa nel titolo, seguita da 'avventura', seguita da qualsiasi cosa.")

All'interno dell'elemento, `<form>` aggiungere il `</div>` seguente markup proprio sotto il `</form>` tag di chiusura per la ricerca di genere (appena prima dell'elemento di chiusura):

[!code-html[Main](form-basics/samples/sample10.html)]

Il codice per gestire questa ricerca è simile al codice per la `LIKE` ricerca di generi, ad eccezione del fatto che è necessario assemblare la ricerca. All'interno del blocco di codice nella `if` parte superiore `if` della pagina, aggiungi questo blocco subito dopo il blocco per la ricerca di generi:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Questo codice usa la stessa logica vista in `LIKE` precedenza, ad`%`eccezione del fatto che la ricerca utilizza un operatore e il codice inserisce " " prima e dopo il termine di ricerca.

Si noti come è stato facile aggiungere un'altra ricerca alla pagina. Tutto quello che dovevi fare era:

- Creare `if` un blocco che teda per verificare se la casella di ricerca pertinente ha un valore.
- Impostare `selectCommand` la variabile su una nuova istruzione SQL.
- Impostare `searchTerm` la variabile sul valore da passare alla query.

Ecco il blocco di codice completo, che contiene la nuova logica per una ricerca del titolo:Here's the complete code block, which contains the new logic for a title search:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Ecco un riepilogo delle operazioni eseguite da questo codice:

- Le `searchTerm` variabili `selectCommand` e vengono inizializzate nella parte superiore. Si imposterà queste variabili sul termine di ricerca appropriato (se presente) e sul comando SQL appropriato in base alle operazioni eseguite dall'utente nella pagina. La ricerca predefinita è il semplice caso di ottenere tutti i film dal database.
- Nei test `searchGenre` per `searchTitle`e , `searchTerm` il codice imposta il valore che si desidera cercare. Tali blocchi di `selectCommand` codice vengono inoltre impostati su un comando SQL appropriato per tale ricerca.
- Il `db.Query` metodo viene richiamato una sola volta, utilizzando qualsiasi comando SQL è in `selectedCommand` e qualsiasi valore è in `searchTerm`. Se non è presente alcun termine di ricerca (nessun genere e nessuna parola titolo), il valore di `searchTerm` è una stringa vuota. Tuttavia, questo non importa, perché in questo caso la query non richiede un parametro.

## <a name="testing-the-title-search-feature"></a>Test della funzionalità di ricerca del titolo

Ora puoi testare la tua pagina di ricerca completata. Eseguire *Movies.cshtml*.

Immettere un genere e fare clic su **Cerca genere**. La griglia visualizza filmati di quel genere, come prima.

Immettere una parola titolo e fare clic su **Cerca titolo**. Nella griglia vengono visualizzati i filmati con tale parola nel titolo.

![Elenco della pagina dei film dopo aver cercato 'The' nel titolo](form-basics/_static/image6.png)

Lasciare vuote entrambe le caselle di testo e fare clic su uno dei pulsanti. La griglia visualizza tutti i filmati.

## <a name="combining-the-queries"></a>Combinazione delle query

Potresti notare che le ricerche che puoi eseguire sono esclusive. Non è possibile cercare il titolo e il genere contemporaneamente, anche se entrambe le caselle di ricerca con. Ad esempio, non è possibile cercare tutti i film d'azione il cui titolo contiene "Avventura". Poiché la pagina è codificata ora, se si immettono valori sia per il genere che per il titolo, la ricerca del titolo ha la precedenza. Per creare una ricerca che combina le condizioni, è necessario creare una query SQL con sintassi simile alla seguente:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

E si avrebbe dovuto eseguire la query utilizzando un'istruzione simile alla seguente (approssimativamente parlando):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

La creazione di logica per consentire molte permutazioni dei criteri di ricerca può coinvolgere un po', come si può vedere. Pertanto, ci fermeremo qui.

## <a name="coming-up-next"></a>Prossimamente

Nell'esercitazione successiva verrà creata una pagina che usa un modulo per consentire agli utenti di aggiungere filmati al database.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Elenco completo per la pagina del filmato (aggiornato con la ricerca)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web ASP.NET utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Clausola SQL WHERE](http://www.w3schools.com/sql/sql_where.asp) nel sito W3Schools
- [Articolo sulle definizioni](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) dei metodi nel sito W3C

> [!div class="step-by-step"]
> [Successivo](displaying-data.md)
> [precedente](entering-data.md)
