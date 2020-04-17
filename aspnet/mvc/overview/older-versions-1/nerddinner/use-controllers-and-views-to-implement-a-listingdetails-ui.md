---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Utilizzo di controller e visualizzazioni per implementare un'interfaccia utente di elenco/dettagli . Documenti Microsoft
author: rick-anderson
description: Passaggio 4 viene illustrato come aggiungere un Controller all'applicazione che sfrutta il modello per fornire agli utenti un'esperienza di navigazione di elenco dati/dettagli...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 49c7dc977477a4edbfcfc68b166ae7ea3fa22f2a
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541312"
---
# <a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Usare controller e visualizzazioni per implementare un'interfaccia utente elenco/dettagli

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 4 di un'esercitazione gratuita [sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come compilare un'applicazione Web piccola ma completa usando ASP.NET MVC 1.
> 
> Passaggio 4 viene illustrato come aggiungere un Controller all'applicazione che sfrutta il modello per fornire agli utenti un elenco di dati/dettagli esperienza di navigazione per le cene sul nostro sito NerdDinner.Step 4 shows how to add a Controller to the application that takes advantage of our model to provide users with a data listing/details navigation experience for dinners on our NerdDinner site.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [ALL'archivio musicale MVC.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-4-controllers-and-views"></a>Passaggio 4 di NerdDinnerNerdDinner Step 4: Controllers and Views

Con i framework Web tradizionali (ASP classico, PHP, web Form ASP.NET e così via), gli URL in ingresso vengono in genere mappati ai file su disco. Ad esempio: una richiesta per un URL come "/Products.aspx" o "/Products.php" potrebbe essere elaborata da un file "Products.aspx" o "Products.php".

I framework MVC basati sul Web eseguono il mapping degli URL al codice server in modo leggermente diverso. Invece di mappare gli URL in ingresso ai file, eseguono invece il mapping degli URL ai metodi nelle classi. Queste classi sono chiamate "Controller" e sono responsabili dell'elaborazione delle richieste HTTP in arrivo, della gestione dell'input dell'utente, del recupero e del salvataggio dei dati e della determinazione della risposta da inviare al client (visualizzazione HTML, download di un file, reindirizzamento a un URL diverso e così via).

Ora che abbiamo creato un modello di base per l'applicazione NerdDinner, il passaggio successivo consisterà nell'aggiungere un Controller all'applicazione che ne trae vantaggio per fornire agli utenti un'esperienza di navigazione di elenco dati/dettagli per Dinners sul nostro sito.

### <a name="adding-a-dinnerscontroller-controller"></a>Aggiunta di un controller DinnersControllerAdding a DinnersController Controller

Inizieremo facendo clic con il pulsante destro del mouse sulla cartella "Controllers" all'interno del nostro progetto web, quindi selezionare il comando di menu **Aggiungi controller&gt;** (è anche possibile eseguire questo comando digitando Ctrl-M, Ctrl-C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Verrà eseguito la finestra di dialogo "Aggiungi controller":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Chiameremo il nuovo controller "DinnersController" e fare clic sul pulsante "Aggiungi". Visual Studio aggiungerà quindi un file di DinnersController.cs nella directory di controllo:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Si aprirà anche la nuova classe DinnersController all'interno dell'editor di codice.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Aggiunta di metodi di azione Index() e Details() alla classe DinnersController

Vogliamo consentire ai visitatori che utilizzano la nostra applicazione di sfogliare un elenco di cene imminenti, e consentire loro di fare clic su qualsiasi cena nella lista per vedere dettagli specifici su di esso. A tale scopo, pubblicando i seguenti URL dall'applicazione:

| **URL** | **Scopo** |
| --- | --- |
| */Le cene/* | Visualizzare un elenco HTML delle cene imminenti |
| */Cena/Dettagli/[id]* | Visualizzare i dettagli di una cena specifica indicata da un parametro "id" incorporato nell'URL, che corrisponderà all'ID della cena nel database. Ad esempio: /Dinners/Details/2 visualizzerebbe una pagina HTML con i dettagli sulla DinnerID il cui valore DinnerID è 2. |

Pubblicheremo le implementazioni iniziali di questi URL aggiungendo due "metodi di azione" pubblici alla nostra classe DinnersController come di seguito:We will publish initial implementations of these URLs by adding two public "action methods" to our DinnersController class like below:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Verrà quindi eseguita l'applicazione NerdDinner e verrà utilizzato il browser per richiamarli. Digitando l'URL *"/Dinners/"* verrà eseguita la nostra funzione *Index()* e verrà inviata la seguente risposta:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Digitando l'URL *"/Dinners/Details/2"* verrà eseguito il nostro metodo *Details()* e verrà restituita la seguente risposta:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Ci si potrebbe chiedere - come ha fatto ASP.NET MVC sapere per creare la nostra classe DinnersController e richiamare tali metodi? Per comprendere questo, diamo un rapido sguardo a come funziona il routing.

### <a name="understanding-aspnet-mvc-routing"></a>Informazioni ASP.NET routing MVC

ASP.NET MVC include un potente motore di routing degli URL che offre molta flessibilità nel controllo della modalità di mapping degli URL alle classi controller. Esso consente di personalizzare completamente come ASP.NET MVC sceglie quale classe controller per creare, quale metodo richiamare su di esso, nonché configurare diversi modi in cui le variabili possono essere analizzate automaticamente dall'URL/ Stringa di query e passate al metodo come argomenti di parametro. Offre la flessibilità necessaria per ottimizzare totalmente un sito per SEO (ottimizzazione del motore di ricerca) e pubblicare qualsiasi struttura di URL che vogliamo da un'applicazione.

Per impostazione predefinita, i nuovi progetti MVC ASP.NET vengono forniti con un set preconfigurato di regole di routing URL già registrato. In questo modo è possibile iniziare facilmente un'applicazione senza dover configurare in modo esplicito nulla. Le registrazioni delle regole di routing predefinite sono disponibili all'interno della classe "Application" dei nostri progetti, che è possibile aprire facendo doppio clic sul file "Global.asax" nella radice del progetto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Le regole di routing MVC di ASP.NET predefinite vengono registrate all'interno del metodo "RegisterRoutes" di questa classe:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

Le "vie. La chiamata al metodo MapRoute()" precedente registra una regola di routing predefinita che esegue il mapping degli URL in ingresso alle classi controller utilizzando il formato dell'URL: "/'controller'/'action'/'id'" – dove "controller" è il nome della classe controller di cui creare un'istanza, "action" è il nome di un metodo pubblico da richiamare su di esso e "id" è un parametro facoltativo incorporato nell'URL che può essere passato come argomento al metodo. Il terzo parametro passato alla chiamata al metodo "MapRoute()" è un set di valori predefiniti da utilizzare per i valori controller/azione/id nel caso in cui non siano presenti nell'URL (Controller : "Home", Action "Index", Id "").

Di seguito è riportata una tabella in cui viene illustrato come viene eseguito il mapping di un'ampia gamma di URL utilizzando la regola di route predefinita "<em>/</em>.

| **URL** | **Classe Controller** | **Metodo Azione** | **Parametri passati** |
| --- | --- | --- | --- |
| */Cena/Dettagli/2* | DinnersController | Dettagli (id) | ID 2 (in n. 2 |
| */Cena/Modifica/5* | DinnersController | Edit(id) | ID 5 (in n. 5 (informazioni in stati |
| */Cena/Crea* | DinnersController | Create() | N/D |
| */Le nozioni* | DinnersController | Indice() | N/D |
| */home* | Homecontroller | Indice() | N/D |
| */* | Homecontroller | Indice() | N/D |

Le ultime tre righe mostrano i valori predefiniti (Controller , Home, Azione , Indice, Id e ""). Poiché il metodo "Index" viene registrato come nome dell'azione predefinita, se non ne viene specificato uno, gli URL "/Dinners" e "/Home" determinano il metodo di azione Index() nelle relative classi Controller. Poiché il controller "Home" è registrato come controller predefinito, se non specificato, l'URL "/" determina la creazione dell'URL HomeController e il metodo di azione Index() su di esso.

Se non ti piacciono queste regole di routing URL predefinite, la buona notizia è che sono facili da modificare - basta modificarle all'interno del metodo RegisterRoutes sopra. Per la nostra applicazione NerdDinner, tuttavia, non stiamo andando a modificare nessuna delle regole di routing URL predefinito - invece ci limiteremo a usarli così com'è.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Utilizzo di DinnerRepository dal nostro DinnersController

Sostituiamo ora l'implementazione corrente dei metodi di azione Indice() e Dettagli() di DinnersController con implementazioni che utilizzano il modello.

Useremo la classe DinnerRepository creata in precedenza per implementare il comportamento. Inizieremo aggiungendo un'istruzione "using" che fa riferimento allo spazio dei nomi "NerdDinner.Models" e quindi dichiareremo un'istanza del nostro DinnerRepository come un campo nella nostra classe DinnerController.

Più avanti in questo capitolo introdurremo il concetto di "Iniezione di dipendenze" e mostreremo un altro modo per i nostri controller per ottenere un riferimento a un DinnerRepository che consente di migliorare unit test - ma per ora ci limiteremo a creare un'istanza del nostro DinnerRepository inline come di seguito.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Ora siamo pronti per generare una risposta HTML utilizzando i nostri oggetti del modello di dati recuperati.

### <a name="using-views-with-our-controller"></a>Utilizzo delle viste con il controllerUsing Views with our Controller

Sebbene sia possibile scrivere codice all'interno dei nostri metodi di azione per assemblare codice HTML e quindi utilizzare il metodo helper *Response.Write()* per inviarlo al client, tale approccio diventa abbastanza ingombrante rapidamente. Un approccio molto migliore è per noi di eseguire solo l'applicazione e la logica dei dati all'interno dei nostri metodi di azione DinnersController e quindi passare i dati necessari per eseguire il rendering di una risposta HTML a un modello "visualizzazione" separato che è responsabile per l'output della rappresentazione HTML di esso. Come vedremo in un attimo, un modello di "visualizzazione" è un file di testo che in genere contiene una combinazione di markup HTML e codice di rendering incorporato.

Separare la logica del controller dal rendering della visualizzazione offre diversi grandi vantaggi. In particolare consente di applicare una chiara "separazione dei problemi" tra il codice dell'applicazione e il codice di formattazione/rendering dell'interfaccia utente. In questo modo è molto più semplice eseguire unit test della logica dell'applicazione in isolamento dalla logica di rendering dell'interfaccia utente. Semplifica la modifica dei modelli di rendering dell'interfaccia utente in un secondo momento senza dover apportare modifiche al codice dell'applicazione. E può rendere più facile per sviluppatori e progettisti di collaborare insieme ai progetti.

È possibile aggiornare la nostra classe DinnersController per indicare che si desidera utilizzare un modello di visualizzazione per inviare una risposta dell'interfaccia utente HTML modificando le firme dei metodi dei due metodi di azione da avere un tipo restituito di "void" per avere invece un tipo restituito di "ActionResult". È quindi possibile chiamare il metodo helper *View()* nella classe di base Controller per restituire un oggetto "ViewResult" come di seguito:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

La firma del metodo helper *View()* che stiamo usando sopra è simile a quanto segue:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

Il primo parametro del metodo helper *View()* è il nome del file del modello di visualizzazione che si desidera utilizzare per eseguire il rendering della risposta HTML. Il secondo parametro è un oggetto modello che contiene i dati necessari al modello di visualizzazione per eseguire il rendering della risposta HTML.

All'interno del nostro metodo di azione Index() chiamiamo il metodo helper *View()* e indichiamo che vogliamo eseguire il rendering di un elenco HTML di cene utilizzando un modello di visualizzazione "Indice". Si passa il modello di visualizzazione una sequenza di Dinner oggetti per generare l'elenco da:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

All'interno del nostro metodo di azione Details() tentiamo di recuperare un oggetto Dinner utilizzando l'ID fornito all'interno dell'URL. Se viene trovato un Dinner valido, chiamiamo il metodo helper *View(),* che indica che si desidera utilizzare un modello di visualizzazione "Dettagli" per eseguire il rendering dell'oggetto Dinner recuperato. Se viene richiesta una cena non valida, viene visualizzato un messaggio di errore utile che indica che la cena non esiste utilizzando un modello di visualizzazione "NotFound" (e una versione di overload del metodo helper *View()* che accetta solo il nome del modello):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

È ora possibile implementare i modelli di visualizzazione "NotFound", "Details" e "Index".

### <a name="implementing-the-notfound-view-template"></a>Implementazione del modello di visualizzazione "NotFound"

Inizieremo implementando il modello di visualizzazione "NotFound", che visualizza un messaggio di errore descrittivo che indica che non è possibile trovare la cena richiesta.

Creeremo un nuovo modello di visualizzazione posizionando il cursore di testo all'interno di un metodo di azione del controller, quindi fare clic con il pulsante destro del mouse e scegliere il comando di menu "Aggiungi visualizzazione" (possiamo anche eseguire questo comando digitando Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Verrà visualizzato un'immagine "Aggiungi vista" come di seguito. Per impostazione predefinita, la finestra di dialogo prepoviene il nome della visualizzazione da creare in modo che corrisponda al nome del metodo di azione in cui si trovava il cursore quando è stata avviata la finestra di dialogo (in questo caso "Dettagli"). Poiché si desidera implementare prima il modello "NotFound", verrà eseguito l'override di questo nome di visualizzazione e impostarlo su invece essere "NotFound":Because we want to first implement the "NotFound" template, we'll override this view name and set it to instead be "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio creerà un nuovo modello di visualizzazione "NotFound.aspx" per noi all'interno della directory "Views" (che verrà creato anche se la directory non esiste già):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Verrà inoltre aperto il nuovo modello di visualizzazione "NotFound.aspx" all'interno dell'editor di codice:It will also open up our new "NotFound.aspx" view template within the code-editor:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Per impostazione predefinita, i modelli di visualizzazione hanno due "aree contenuto" in cui è possibile aggiungere contenuto e codice. Il primo ci permette di personalizzare il "titolo" della pagina HTML inviata indietro. Il secondo ci permette di personalizzare il "contenuto principale" della pagina HTML inviata indietro.

Per implementare il modello di visualizzazione "NotFound", aggiungeremo del contenuto di base:To implement our "NotFound" view template we'll add some basic content:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

Possiamo quindi provarlo all'interno del browser. Per fare questo chiediamo l'URL *"/Dinners/Details/9999".* Questo farà riferimento a una cena che attualmente non esiste nel database e causerà il nostro metodo di azione DinnersController.Details() per eseguire il rendering del modello di visualizzazione "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Una cosa che noterete nella schermata precedente è che il nostro modello di visualizzazione di base ha ereditato una serie di HTML che circonda il contenuto principale sullo schermo. Questo perché il nostro modello di visualizzazione utilizza un modello di "pagina master" che ci consente di applicare un layout coerente in tutte le visualizzazioni del sito. Discuteremo come le pagine master funzionano di più in una parte successiva di questa esercitazione.

### <a name="implementing-the-details-view-template"></a>Implementazione del modello di vista "Dettagli"

Ora implementiamo il modello di visualizzazione "Dettagli", che genererà codice HTML per un singolo modello Dinner.

Lo faremo posizionando il cursore di testo all'interno del metodo di azione Dettagli, quindi fare clic con il pulsante destro del mouse e scegliere il comando di menu "Aggiungi visualizzazione" (o premere Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Verrà visualizzato il riquadro di dialogo "Aggiungi visualizzazione". Manterremo il nome della vista predefinita ("Dettagli"). Si selezionerà anche la casella di controllo "Crea una visualizzazione fortemente tipizzato" nella finestra di dialogo e selezionare (utilizzando l'elenco a discesa casella combinata) il nome del tipo di modello che si sta passando dal Controller alla visualizzazione. Per questa visualizzazione si passa un oggetto Dinner (il nome completo per questo tipo è: "NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

A differenza del modello precedente, in cui si è scelto di creare una "Vista vuota", questa volta si sceglierà di "scaffold" automaticamente la vista utilizzando un modello "Dettagli". Possiamo indicare questo modificando il menu a discesa "Visualizza contenuto" nella finestra di dialogo sopra.

"Scaffolding" genererà un'implementazione iniziale del modello di visualizzazione dei dettagli in base all'oggetto Dinner che stiamo passando ad esso. Questo fornisce un modo semplice per iniziare rapidamente l'implementazione del modello di visualizzazione.

Quando si fa clic sul pulsante "Aggiungi", Visual Studio creerà un nuovo file di modello di visualizzazione "Dettagli.aspx" per noi all'interno della nostra directory "Views" Dinners:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Verrà inoltre aperto il nuovo modello di visualizzazione "Details.aspx" all'interno dell'editor di codice. Conterrà un'implementazione iniziale dello scaffolding di una visualizzazione dettagli basata su un modello Dinner.It will contain an initial scaffold implementation of a details view based on a Dinner model. Il motore di scaffolding utilizza la reflection .NET per esaminare le proprietà pubbliche esposte nella classe passata e aggiungerà il contenuto appropriato in base a ogni tipo trovato:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Possiamo richiedere l'URL *"/Dinners/Details/1"* per vedere come appare questa implementazione di scaffold "dettagli" nel browser. Utilizzando questo URL verrà visualizzata una delle cene che abbiamo aggiunto manualmente al nostro database quando l'abbiamo creato per la prima volta:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Questo ci mette in funzione rapidamente e ci fornisce un'implementazione iniziale della nostra visualizzazione Details.aspx. Possiamo quindi andare e modificarlo per personalizzare l'interfaccia utente per la nostra soddisfazione.

Quando esaminiamo il modello Details.aspx più da vicino, scopriremo che contiene HTML statico e codice di rendering incorporato. &lt;%&gt; % codice nuggets eseguire il codice &lt;quando&gt; viene eseguito il rendering del modello di visualizzazione e % % nuggets codice eseguire il codice in esso contenuto e quindi eseguire il rendering del risultato nel flusso di output del modello.

È possibile scrivere codice all'interno della visualizzazione che accede all'oggetto modello "Dinner" che è stato passato dal controller utilizzando una proprietà "Model" fortemente tipizzato. Visual Studio fornisce intellisense codice completo quando si accede a questa proprietà "Model" all'interno dell'editor:Visual Studio provides us with full code-intellisense when accessing this "Model" property within the editor:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Apportiamo alcune modifiche in modo che l'origine per il modello di visualizzazione Dettagli finale sia simile alla seguente:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Quando accediamo di nuovo all'URL *"/Dinners/Details/1"* verrà ora visualizzato come di seguito:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementazione del modello di vista "Indice"

Esaminiamo ora il modello di visualizzazione "Indice", che genererà un elenco delle cene imminenti. Per fare questo posizioneremo il nostro cursore di testo all'interno del metodo di azione Indice, quindi fare clic con il pulsante destro del mouse e scegliere il comando di menu "Aggiungi visualizzazione" (o premere Ctrl-M, Ctrl-V).

All'interno della finestra di dialogo "Aggiungi visualizzazione" manterremo il modello di vista denominato "Indice" e selezionare la casella di controllo "Crea una vista fortemente tipit". Questa volta sceglieremo di generare automaticamente un modello di visualizzazione "Elenco" e selezionare "NerdDinner.Models.Dinner" come tipo di modello passato alla visualizzazione (che poiché abbiamo indicato che stiamo creando uno scaffold "List" farà sì che la finestra di dialogo Aggiungi visualizzazione presupponga che stiamo passando una sequenza di oggetti Dinner dal nostro Controller alla vista):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio creerà un nuovo file di modello di visualizzazione "Index.aspx" per noi all'interno della nostra directory "Views". Sarà "scaffold" un'implementazione iniziale all'interno di esso che fornisce un elenco di tabella HTML delle cene passiamo alla visualizzazione.

Quando eseguiamo l'applicazione e accediamo all'URL *"/Dinners/"* renderà la nostra lista di cene in questo modo:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

La soluzione tabella di cui sopra ci dà un layout a griglia dei nostri dati Dinner - che non è proprio quello che vogliamo per il nostro consumatore di fronte Dinner elenco. È possibile aggiornare il modello di visualizzazione Index.aspx e modificarlo per elencare meno colonne di dati e utilizzare un &lt;&gt; ul elemento per eseguirne il rendering anziché una tabella utilizzando il codice seguente:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Stiamo usando la parola chiave "var" all'interno dell'istruzione foreach di cui sopra mentre ci rinosiamo su ogni cena nel nostro modello. Coloro che non hanno familiarità con la versione 3.0 di C, potrebbero pensare che l'uso di "var" significhi che l'oggetto dinner è ad associazione tardiva. Significa invece che il compilatore utilizza l'inferenza del tipo rispetto alla proprietà&lt;"Model" fortemente tipizzata (che è di tipo "IEnumerable Dinner&gt;") e compila la variabile locale "dinner" come tipo Dinner, ovvero si ottiene il controllo completo del processo di avvio e della verifica in fase di compilazione all'interno dei blocchi di codice:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Quando abbiamo colpito aggiorna sull'URL */Dinners* nel nostro browser la nostra vista aggiornata ora sembra come di seguito:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Questo è un aspetto migliore - ma non è ancora del tutto lì. L'ultimo passaggio consiste nel consentire agli utenti finali di fare clic sulle singole cene nell'elenco e visualizzare i dettagli su di esse. Implementeremo questo eseguendo il rendering di elementi di collegamento ipertestuale HTML che si collegano al metodo di azione Details sul nostro DinnersController.We'll implement this by rendering HTML hyperlink elements that link to the Details action method on our DinnersController.

Possiamo generare questi collegamenti ipertestuali all'interno della nostra vista Indice in uno dei due modi. Il primo consiste nel &lt;&gt; creare manualmente HTML a &lt;elementi&gt; come &lt;di&gt; seguito, in cui incorporiamo % blocchi all'interno di un elemento HTML:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Un approccio alternativo che possiamo usare è quello di sfruttare il metodo helper incorporato "Html.ActionLink()" all'interno di ASP.NET MVC che supporta la creazione a livello di codice la creazione di un HTML un elemento che si collega a un altro metodo di azione in un Controller:An alternative approach we can use is to take advantage of the built-in "Html.ActionLink()" helper method within ASP.NET MVC that supports programmatically creating an HTML &lt;a&gt; element that links to another action method on a Controller:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Il primo parametro del metodo helper Html.ActionLink() è il testo-collegamento da visualizzare (in questo caso il titolo della cena), il secondo parametro è il nome dell'azione Controller a cui si desidera generare il collegamento (in questo caso il metodo Details) e il terzo parametro è un insieme di parametri da inviare all'azione (implementato come tipo anonimo con nome/valori di proprietà). In questo caso si sta specificando il parametro "id" della cena a cui si desidera eseguire il collegamento e, poiché la regola di routing dell'URL predefinita in ASP.NET MVC è ""Controller"/'Action/id' il metodo helper Html.ActionLink() genererà l'output seguente:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Per la visualizzazione Index.aspx useremo l'approccio al metodo helper Html.ActionLink() e ogni cena nell'elenco verrà collegata all'URL dei dettagli appropriato:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

E ora quando abbiamo colpito il */ Dinners* URL la nostra lista di cena si presenta come di seguito:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Quando clicchiamo su una delle cene nell'elenco, navigheremo per vedere i dettagli su di esso:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Denominazione basata su convenzioni e struttura di directory di viste

ASP.NET applicazioni MVC per impostazione predefinita utilizzano una struttura di denominazione delle directory basata su convenzioni durante la risoluzione dei modelli di visualizzazione. Ciò consente agli sviluppatori di evitare di dover qualificare completamente un percorso di posizione quando si fa riferimento alle visualizzazioni dall'interno di una classe Controller.This allows developers to avoid having to fully-qualify a location path when referencing views from within a Controller class. Per impostazione predefinita ASP.NET MVC cercherà il\[file del\* modello di visualizzazione all'interno della directory di nome controller di visualizzazioni sotto l'applicazione.

Ad esempio, abbiamo lavorato sulla classe DinnersController, che fa riferimento in modo esplicito a tre modelli di visualizzazione: "Index", "Details" e "NotFound". ASP.NET MVC cercherà per impostazione predefinita queste visualizzazioni all'interno della directory *"Views" (Visualizzazioni)* sotto la directory radice dell'applicazione:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Si noti sopra come sono attualmente presenti tre classi di controller all'interno del progetto (DinnersController, HomeController e AccountController – le ultime due sono state aggiunte per impostazione predefinita quando è stato creato il progetto) e sono presenti tre sottodirectory (una per ogni controller) all'interno della directory di views.

Le viste a cui viene fatto riferimento dai controller Home e Account risolveranno automaticamente i relativi modelli di visualizzazione dalle rispettive directory *"Views"* e *"Views" e "Views" (Visualizzazioni)* e "Account". La sottodirectory *"Views"* fornisce un modo per archiviare i modelli di visualizzazione che vengono riutilizzati tra più controller all'interno dell'applicazione. Quando ASP.NET MVC tenta di risolvere un modello di visualizzazione, verrà innanzitutto controllare all'interno della directory specifica *\[del controller* di visualizzazioni e, se non riesce a trovare il modello di visualizzazione, verrà visualizzato all'interno *della* directory di visualizzazione .

Quando si tratta di assegnare un nome ai singoli modelli di visualizzazione, le indicazioni consigliate sono le indicazioni consigliate per fare in modo che il modello di visualizzazione condivida lo stesso nome del metodo di azione che ne ha causato il rendering. Ad esempio, sopra il metodo di azione "Indice" viene usata la visualizzazione "Indice" per eseguire il rendering del risultato della visualizzazione e il metodo di azione "Dettagli" utilizza la visualizzazione "Dettagli" per eseguire il rendering dei risultati. In questo modo è più semplice vedere rapidamente quale modello è associato a ogni azione.

Gli sviluppatori non è necessario specificare in modo esplicito il nome del modello di visualizzazione quando il modello di visualizzazione ha lo stesso nome del metodo di azione richiamato nel controller. È invece possibile passare solo l'oggetto modello al metodo helper "View()" (senza specificare il nome della visualizzazione) e ASP.NET MVC dedurrà automaticamente che si desidera utilizzare il modello di visualizzazione *"Views\[ControllerName]\[ActionName]* su disco per eseguirne il rendering.

Questo ci permette di pulire il nostro codice controller un po ', ed evitare di duplicare il nome due volte nel nostro codice:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

Il codice di cui sopra è tutto ciò che è necessario per implementare una bella Dinner elenco/dettagli esperienza per il sito.

#### <a name="next-step"></a>passaggio successivo

Ora abbiamo una bella esperienza di navigazione cena costruito.

È ora possibile abilitare il supporto di modifica dei moduli dati CRUD (Create, Read, Update, Delete).

> [!div class="step-by-step"]
> [Successivo](build-a-model-with-business-rule-validations.md)
> [precedente](provide-crud-create-read-update-delete-data-form-entry-support.md)
