---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Fornire supporto per l'immissione di moduli dati CRUD (Create, Read, Update, Delete) Documenti Microsoft
author: rick-anderson
description: Passaggio 5 viene illustrato come prendere la nostra classe DinnersController ulteriormente abilitando il supporto per la modifica, la creazione e l'eliminazione di cene con esso pure.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: 2b75a7eda8bce4baa25d92626639f4d904eb363a
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542625"
---
# <a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Fornire il supporto per operazioni di creazione, lettura, aggiornamento ed eliminazione sulle voci di moduli di dati

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 5 di un'esercitazione gratuita [sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come compilare un'applicazione Web piccola ma completa usando ASP.NET MVC 1.
> 
> Passaggio 5 viene illustrato come prendere la nostra classe DinnersController ulteriormente abilitando il supporto per la modifica, la creazione e l'eliminazione di cene con esso pure.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [ALL'archivio musicale MVC.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>Passaggio 5 di NerdDinner: creazione, aggiornamento ed eliminazione di scenari di moduloNerdDinner Step 5: Create, Update, Delete Form Scenarios

Abbiamo introdotto controller e visualizzazioni e illustrato come usarli per implementare un'esperienza di elenco/dettagli per le cene in loco. Il passo successivo sarà quello di prendere la nostra classe DinnersController ulteriormente e abilitare il supporto per la modifica, la creazione e l'eliminazione di cene con esso pure.

### <a name="urls-handled-by-dinnerscontroller"></a>URL gestiti da DinnersController

In precedenza sono stati aggiunti metodi di azione a DinnersController che implementano il supporto per due URL: */Dinners* e */Dinners/Details/[id]*.

| **URL** | **Verbo** | **Scopo** |
| --- | --- | --- |
| */Le cene/* | GET | Visualizzare un elenco HTML delle cene imminenti. |
| */Cena/Dettagli/[id]* | GET | Visualizzare i dettagli relativi a una cena specifica. |

A questo punto verranno aggiunti metodi di azione per implementare tre URL aggiuntivi: */Dinners/Edit/[id]*, */Dinners/Create*e */Dinners/Delete/[id]*. Questi URL consentiranno il supporto per la modifica di cene esistenti, la creazione di nuove cene e l'eliminazione di cene.

Supporteremo le interazioni dei verbi HTTP GET e HTTP POST con questi nuovi URL. Le richieste HTTP GET a questi URL visualizzeranno la visualizzazione HTML iniziale dei dati (un modulo popolato con i dati Dinner in caso di "edit", un modulo vuoto nel caso di "create" e una schermata di conferma dell'eliminazione in caso di "delete"). Le richieste HTTP POST a questi URL salveranno/aggiornano/elimineranno i dati dinner nel nostro DinnerRepository (e da lì al database).

| **URL** | **Verbo** | **Scopo** |
| --- | --- | --- |
| */Dinners/Edit/[id]* | GET | Visualizzare un modulo HTML modificabile popolato con i dati di Dinner. |
| POST | Salvare le modifiche apportate al modulo per una determinata dinner nel database. |
| */Cena/Crea* | GET | Visualizza un modulo HTML vuoto che consente agli utenti di definire nuove cene. |
| POST | Creare una nuova cena e salvarla nel database. |
| */Cena/Elimina/[id]* | GET | Visualizzare la schermata di conferma dell'eliminazione. |
| POST | Elimina la cena specificata dal database. |

### <a name="edit-support"></a>Modifica supporto

Iniziamo implementando lo scenario di "modifica".

#### <a name="the-http-get-edit-action-method"></a>Il metodo di azione edit HTTP-GETThe HTTP-GET Edit Action Method

Inizieremo implementando il comportamento HTTP "GET" del nostro metodo di azione di modifica. Questo metodo verrà richiamato quando viene richiesto l'URL */Dinners/Edit/[id].* La nostra implementazione sarà simile a:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

Il codice precedente usa il DinnerRepository per recuperare un Dinner oggetto. Viene quindi eseguito il rendering di un modello di visualizzazione utilizzando il Dinner oggetto. Poiché non è stato passato in modo esplicito un nome di modello per il metodo helper *View(),* verrà utilizzato il percorso predefinito basato sulla convenzione per risolvere il modello di visualizzazione: /Views/Dinners/Edit.aspx.

Creiamo ora questo modello di vista. Lo faremo facendo clic con il pulsante destro del mouse all'interno del metodo Edit e selezionando il comando del menu di scelta rapida "Aggiungi vista":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

All'interno della finestra di dialogo "Aggiungi visualizzazione" indicheremo che stiamo passando un Dinner oggetto al modello di visualizzazione come modello e scegliere di eseguire lo scaffolding automatico di un modello "Modifica":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio aggiungerà un nuovo file di modello di visualizzazione "Edit.aspx" per noi all'interno della directory "Views". Verrà inoltre aperto il nuovo modello di visualizzazione "Edit.aspx" all'interno dell'editor di codice, popolato con un'implementazione di scaffolding iniziale "Modifica", come indicato di seguito:It will also open up the new "Edit.aspx" view template within the code-editor – populated with an initial "Edit" scaffold implementation like below:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Apportiamo alcune modifiche allo scaffold "modifica" predefinito generato e aggiorniamo il modello di visualizzazione di modifica in modo che il contenuto sia riportato di seguito (che rimuove alcune delle proprietà che non vogliamo esporre):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Quando eseguiamo l'applicazione e richiediamo l'URL *"/Dinners/Edit/1"* vedremo la seguente pagina:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

Il markup HTML generato dalla visualizzazione è simile a quello riportato di seguito. Si tratta di HTML &lt;&gt; standard – con un elemento form che esegue un HTTP POST &lt;per l'URL&gt; */Dinners/Edit/1* quando viene premuto il pulsante "Salva" tipo di input "submit"/. Un &lt;elemento type-"text"&gt; / di input HTML è stato restituito per ogni proprietà modificabile:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Metodi helper Html.BeginForm() e Html.TextBox() Html Helper

Il modello di visualizzazione "Edit.aspx" utilizza diversi metodi "Html Helper": Html.ValidationSummary(), Html.BeginForm(), Html.TextBox() e Html.ValidationMessage(). Oltre a generare il markup HTML per noi, questi metodi helper forniscono la gestione degli errori incorporato e il supporto di convalida.

##### <a name="htmlbeginform-helper-method"></a>Metodo helper Html.BeginForm()

Il metodo helper Html.BeginForm() è &lt;&gt; ciò che restituisce l'elemento form HTML nel markup. Nel modello di visualizzazione Edit.aspx si noterà che si sta applicando un'istruzione "using" di C, quando si utilizza questo metodo. La parentesi graffa aperta indica &lt;l'inizio del contenuto del modulo&gt; e la parentesi &lt;graffa di chiusura è ciò che indica la fine dell'elemento /form:&gt;

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

In alternativa, se si trova l'approccio di istruzione "using" innaturale per uno scenario come questo, è possibile utilizzare una combinazione Html.BeginForm() e Html.EndForm() (che fa la stessa cosa):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

La chiamata a Html.BeginForm() senza parametri ne farà restituire un elemento del form che esegue un'applicazione di UN HTTP-POST all'URL della richiesta corrente. Questo è il motivo per cui la nostra visualizzazione di modifica genera * &lt;un'azione del modulo , "/Dinners/Edit/1" method , "post".&gt; * Avremmo potuto in alternativa passare parametri espliciti a Html.BeginForm() se volessimo pubblicare contenuti in un URL diverso.

##### <a name="htmltextbox-helper-method"></a>Metodo helper Html.TextBox()

La visualizzazione Edit.aspx utilizza il metodo helper &lt;Html.TextBox() per&gt; restituire gli elementi type-"text"/ di input:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

Il metodo Html.TextBox() precedente accetta un singolo parametro, che viene utilizzato &lt;per specificare sia&gt; gli attributi id/name dell'elemento di input type/"text" per l'output, sia la proprietà del modello da cui popolare il valore della casella di testo. Ad esempio, l'oggetto Dinner che è stato passato alla visualizzazione di modifica ha un valore della proprietà "Title" di ".NET Futures" e pertanto l'output della chiamata di chiamata della chiamata Html.TextBox("Title") è * &lt;l'oggetto id di input "Title" name,"Title" type "text" value ".NET Futures" /&gt;*.

In alternativa, è possibile utilizzare il primo parametro Html.TextBox() per specificare l'id/nome dell'elemento e quindi passare in modo esplicito il valore da utilizzare come secondo parametro:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Spesso si vorrà eseguire la formattazione personalizzata sul valore di output. Il metodo statico String.Format() incorporato in .NET è utile per questi scenari. Il modello di visualizzazione Edit.aspx utilizza questo per formattare il valore EventDate (che è di tipo DateTime) in modo che non mostri i secondi per l'ora:Our Edit.aspx view template is using this to format the EventDate value (which is of type DateTime) so that it doesn't show seconds for the time:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Un terzo parametro per Html.TextBox() può essere utilizzato facoltativamente per restituire attributi HTML aggiuntivi. Il frammento di codice riportato di seguito illustra come eseguire il rendering di un &lt;attributo size/30" aggiuntivo e di un attributo class-"mycssclass" sull'elemento type-"text" di&gt; input. Si noti come si sta espore il@" character because "nome dell'attributo class utilizzando una "classe" è una parola chiave riservata in C :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implementazione del metodo di azione di modifica HTTP-POSTImplementing the HTTP-POST Edit Action Method

È ora implementata la versione HTTP-GET del metodo di azione Edit. Quando un utente richiede l'URL */Dinners/Edit/1* riceve una pagina HTML simile alla seguente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Premendo il pulsante "Salva" viene visualizzato un post di modulo nell'URL &lt;&gt; */Dinners/Edit/1* e vengono inviati i valori del modulo di input HTML utilizzando il verbo HTTP POST. È ora possibile implementare il comportamento HTTP POST del metodo di azione di modifica, che gestirà il salvataggio della cena.

Inizieremo aggiungendo un metodo di azione "Edit" di overload al nostro DinnersController che ha un attributo "AcceptVerbs" che indica che gestisce gli scenari HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Quando l'attributo [AcceptVerbs] viene applicato ai metodi di azione di overload, ASP.NET MVC gestisce automaticamente l'invio di richieste al metodo di azione appropriato a seconda del verbo HTTP in ingresso. Le richieste HTTP POST agli URL */Dinners/Edit/[id]* andranno al metodo Edit precedente, mentre tutte le altre richieste di verbo HTTP agli URL */Dinners/Edit/[id]* verranno inviate al primo metodo Edit che abbiamo implementato (che non aveva un `[AcceptVerbs]` attributo).

| **Argomento collaterale: Perché differenziarsi tramite verbi HTTP?** |
| --- |
| Ci si potrebbe chiedere – perché stiamo usando un singolo URL e differenziando il suo comportamento tramite il verbo HTTP? Perché non avere solo due URL separati per gestire il caricamento e il salvataggio delle modifiche? Ad esempio: /Dinners/Edit/[id] per visualizzare il modulo iniziale e /Dinners/Save/[id] per gestire il post del modulo per salvarlo? Il rovescio della medaglia con la pubblicazione di due URL separati è che nei casi in cui pubblichiamo a /Dinners/Save/2, e quindi abbiamo bisogno di visualizzare nuovamente il modulo HTML a causa di un errore di input, l'utente finale finirà per avere l'URL /Dinners/Save/2 nella barra degli indirizzi del browser (poiché questo era l'URL in cui il modulo ha pubblicato). Se l'utente finale aggiunge ai segnalibri questa pagina rivisualizzata nell'elenco dei preferiti del browser o copia/incolla l'URL e lo invia via e-mail a un amico, finirà per salvare un URL che non funzionerà in futuro (poiché tale URL dipende dai valori dei post). Esponendo un singolo URL (ad esempio: /Dinners/Edit/[id]) e differenziando ne l'elaborazione tramite verbo HTTP, è sicuro per gli utenti finali aggiungere un segnalibro alla pagina di modifica e/o inviare l'URL ad altri. |

#### <a name="retrieving-form-post-values"></a>Recupero dei valori di postazione dei moduli

Ci sono una varietà di modi in cui possiamo accedere ai parametri del modulo pubblicato all'interno del nostro metodo HTTP POST "Edit". Un approccio semplice consiste nell'utilizzare solo il Request proprietà nella controller classe di base per accedere alla raccolta di form e recuperare direttamente i valori registrati:One simple approach is to just use the Request property on the Controller base class to access the form collection and retrieve the posted values directly:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

L'approccio precedente è un po' dettagliato, tuttavia, soprattutto una volta aggiunta la logica di gestione degli errori.

Un approccio migliore per questo scenario consiste nell'utilizzare il metodo helper *UpdateModel()* incorporato nella classe di base Controller. Supporta l'aggiornamento delle proprietà di un oggetto che lo passiamo utilizzando i parametri del modulo in ingresso. Utilizza la reflection per determinare i nomi delle proprietà nell'oggetto e quindi converte e assegna automaticamente i valori in base ai valori di input inviati dal client.

Potremmo usare il metodo UpdateModel() per semplificare la nostra azione di modifica HTTP-POST usando questo codice:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Ora possiamo visitare l'URL */Dinners/Edit/1* e modificare il titolo della nostra Cena:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Quando si fa clic sul pulsante "Salva", verrà eseguito un post del modulo per l'azione Modifica e i valori aggiornati verranno mantenuti nel database. Verrà quindi reindirizzato all'URL dei dettagli per la cena (che visualizzerà i valori appena salvati):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Gestione degli errori di modificaHandling Edit Errors

L'attuale implementazione HTTP-POST funziona correttamente, tranne quando si verificano errori.

Quando un utente commette un errore durante la modifica di un modulo, è necessario assicurarsi che il modulo venga visualizzato nuovamente con un messaggio di errore informativo che li guida a risolverlo. Sono inclusi i casi in cui un utente finale invia un input non corretto (ad esempio: una stringa di data in formato non corretto), nonché i casi in cui il formato di input è valido ma si è verificata una violazione della regola business. Quando si verificano errori, il modulo deve conservare i dati di input immessi in origine dall'utente in modo che non debbano ricaricare manualmente le modifiche. Questo processo deve essere ripetuto tutte le volte che è necessario fino al completamento del modulo.

ASP.NET MVC include alcune funzionalità predefinite che semplificano la gestione degli errori e la visualizzazione dei moduli. Per vedere queste funzionalità in azione è possibile aggiornare il metodo di azione Edit con il codice seguente:To see these features in action let's update our Edit action method with the following code:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

Il codice precedente è simile all'implementazione precedente, ad eccezione del fatto che ora stiamo eseguendo il wrapping di un blocco di gestione degli errori try/catch intorno al nostro lavoro. Se si verifica un'eccezione quando si chiama UpdateModel(), o quando si tenta di salvare il DinnerRepository (che verrà generato un'eccezione se l'oggetto Dinner che si sta tentando di salvare non è valido a causa di una violazione della regola all'interno del modello), verrà eseguito il blocco di gestione degli errori catch. All'interno di esso si esegue un ciclo su tutte le violazioni delle regole che esistono nel Dinner oggetto e aggiungerli a un ModelState oggetto (che discuteremo a breve). Viene quindi visualizzata nuovamente la visualizzazione.

Per vedere questo lavoro è possibile rieseguire l'applicazione, modificare un Dinnere modificarlo per avere un titolo vuoto, un EventDate di "BOGUS", e utilizzare un numero di telefono del Regno Unito con un valore country di USA. Quando premiamo il pulsante "Salva" il nostro metodo HTTP POST Edit non sarà in grado di salvare la cena (perché ci sono errori) e visualizzerà nuovamente il modulo:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

La nostra applicazione ha un'esperienza di errore decente. Gli elementi di testo con l'input non valido vengono evidenziati in rosso e i messaggi di errore di convalida vengono visualizzati all'utente finale su di essi. Il modulo conserva anche i dati di input immessi dall'utente in origine, in modo che non debbano riempire nulla.

Come, si potrebbe chiedere, è successo? In che modo le caselle di testo Title, EventDate e ContactPhone si sono evidenziate in rosso e hanno saputo restituire i valori utente immessi in origine? E come sono stati visualizzati i messaggi di errore nella lista in alto? La buona notizia è che questo non si è verificato per magia - piuttosto è stato perché abbiamo usato alcune delle funzionalità integrate ASP.NET MVC che rendono la convalida dell'input e gli scenari di gestione degli errori facile.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Informazioni sui metodi di supporto HTML ModelState e sui metodi di supporto HTML di convalidaUnderstanding ModelState and the Validation HTML Helper Methods

Le classi controller dispongono di una raccolta di proprietà "ModelState" che consente di indicare che esistono errori con un oggetto modello passato a una visualizzazione. Le voci di errore all'interno della raccolta ModelState identificano il nome della proprietà del modello con il problema (ad esempio: "Title", "EventDate" o "ContactPhone" e consentono di specificare un messaggio di errore descrittivo (ad esempio: "Title is required").

Il metodo helper *UpdateModel()* popola automaticamente l'insieme ModelState quando rileva errori durante il tentativo di assegnare valori di modulo alle proprietà nell'oggetto modello. Ad esempio, il Dinner dell'oggetto EventDate proprietà è di tipo DateTime.For example, our Dinner object's EventDate property is of type DateTime. Quando il metodo UpdateModel() non è stato in grado di assegnargli il valore stringa "BOGUS" nello scenario precedente, il metodo UpdateModel() ha aggiunto una voce all'insieme ModelState che indica che si è verificato un errore di assegnazione con tale proprietà.

Gli sviluppatori possono anche scrivere codice per aggiungere in modo esplicito le voci di errore nella raccolta ModelState come stiamo facendo di seguito all'interno del nostro blocco di gestione degli errori "catch", che popola la raccolta ModelState con voci basate sulle violazioni delle regole attive nell'oggetto Dinner:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Integrazione dell'helper Html con ModelState

I metodi helper HTML, come Html.TextBox(), controllano l'insieme ModelState durante il rendering dell'output. Se esiste un errore per l'elemento, esegue il rendering del valore immesso dall'utente e di una classe di errore CSS.

Ad esempio, nella nostra vista "Modifica" stiamo usando il metodo helper Html.TextBox() per eseguire il rendering dell'oggetto EventDate dell'oggetto Dinner:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Quando è stato eseguito il rendering della visualizzazione nello scenario di errore, il metodo Html.TextBox() ha controllato l'insieme ModelState per verificare se si sono verificati errori associati alla proprietà "EventDate" dell'oggetto Dinner. Quando ha determinato che si è verificato un errore, ha eseguito il rendering dell'input dell'utente inviato ("BOGUS") come valore e ha aggiunto una classe di errore css al tipo di &lt;input "textbox"/markup&gt; generato:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

È possibile personalizzare l'aspetto della classe di errore css in modo da avere l'aspetto desiderato. La classe di errore CSS predefinita, ovvero "input-validation-error", è definita nel foglio di stile *"content"site.css* e presenta il seguente aspetto:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Questa regola CSS è ciò che ha causato i nostri elementi di input non validi da evidenziare come di seguito:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Metodo helper Html.ValidationMessage()

Il metodo helper Html.ValidationMessage() può essere utilizzato per generare il messaggio di errore ModelState associato a una particolare proprietà del modello:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Il codice di cui sopra restituisce: * &lt;span&gt; class "field-validation-error" Il valore 'BOGUS' non è valido&lt;/span&gt;*

Il metodo helper Html.ValidationMessage() supporta anche un secondo parametro che consente agli sviluppatori di eseguire l'override del messaggio di testo di errore visualizzato:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Il codice precedente restituisce: * &lt;span&gt;\*&lt;class, "field-validation-error" /span&gt; * invece del testo dell'errore predefinito quando è presente un errore per la proprietà EventDate.

##### <a name="htmlvalidationsummary-helper-method"></a>Metodo helper Html.ValidationSummary()

Il metodo helper Html.ValidationSummary() può essere utilizzato per eseguire &lt;il&gt;&lt;rendering&gt;&lt;di&gt; un messaggio di errore di riepilogo, accompagnato da un elenco ul li/ /ul di tutti i messaggi di errore dettagliati nell'insieme ModelState:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Il metodo helper Html.ValidationSummary() accetta un parametro di stringa facoltativo, che definisce un messaggio di errore di riepilogo da visualizzare sopra l'elenco di errori dettagliati:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Facoltativamente, è possibile utilizzare CSS per eseguire l'override dell'aspetto dell'elenco errori.

#### <a name="using-a-addruleviolations-helper-method"></a>Utilizzo di un metodo helper AddRuleViolationsUsing a AddRuleViolations Helper Method

La nostra implementazione iniziale di HTTP-POST Edit ha usato un'istruzione foreach all'interno del blocco catch per eseguire un ciclo sulle violazioni delle regole dell'oggetto Dinner e aggiungerle all'insieme ModelState del controller:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Possiamo rendere questo codice un po 'più pulito aggiungendo una classe "ControllerHelpers" per il Progetto NerdDinner e implementare un metodo di estensione "AddRuleViolations" all'interno di esso che aggiunge un metodo di supporto alla classe ModelStateDictionary ASP.NET MVC. Questo metodo di estensione può incapsulare la logica necessaria per popolare il ModelStateDictionary con un elenco di errori RuleViolation:This extension method can encapsulate the logic necessary to populate the ModelStateDictionary with a list of RuleViolation errors:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

È quindi possibile aggiornare il metodo di azione HTTP-POST Edit per usare questo metodo di estensione per popolare la raccolta ModelState con le violazioni delle regole di Dinner.We can then update our HTTP-POST Edit action method to use this extension method to populate the ModelState collection with our Dinner Rule Violations.

#### <a name="complete-edit-action-method-implementations"></a>Implementazioni complete di Edit Action Method

Il codice seguente implementa tutta la logica del controller necessaria per lo scenario di modifica:The code below implements all of the controller logic necessary for our Edit scenario:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

La cosa bella della nostra implementazione di modifica è che né la classe Controller né il modello di visualizzazione deve sapere nulla circa la convalida specifica o le regole di business applicate dal modello Dinner.The nice thing about our Edit implementation is that neither our Controller class nor our View template has to know anything about the specific validation or business rules being enforced by our Dinner model. Possiamo aggiungere altre regole al nostro modello in futuro e non è necessario apportare modifiche al codice al controller o alla visualizzazione per poter essere supportate. Questo ci offre la flessibilità necessaria per evolvere facilmente i requisiti dell'applicazione in futuro con un minimo di modifiche al codice.

### <a name="create-support"></a>Crea supporto

Abbiamo terminato l'implementazione del comportamento "Edit" della nostra classe DinnersController. Passiamo ora all'implementazione del supporto "Crea" su di esso, che consentirà agli utenti di aggiungere nuove cene.

#### <a name="the-http-get-create-action-method"></a>Il metodo di azione di creazione HTTP-GETThe HTTP-GET Create Action Method

Inizieremo implementando il comportamento HTTP "GET" del nostro metodo di azione create. Questo metodo verrà chiamato quando un utente visita l'URL */Dinners/Create.* La nostra implementazione si presenta come:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

Il codice precedente crea un nuovo oggetto Dinner e assegna la relativa proprietà EventDate in modo che sia una settimana in futuro. Viene quindi eseguito il rendering di un View basato sul nuovo Dinner oggetto. Poiché non è stato passato in modo esplicito un nome al metodo helper *View(),* verrà utilizzato il percorso predefinito basato sulla convenzione per risolvere il modello di visualizzazione: /Views/Dinners/Create.aspx.

Creiamo ora questo modello di vista. Possiamo farlo facendo clic con il pulsante destro del mouse all'interno del metodo di azione Crea e selezionando il comando del menu di scelta rapida "Aggiungi visualizzazione". All'interno della finestra di dialogo "Aggiungi visualizzazione" indicheremo che stiamo passando un Dinner oggetto al modello di visualizzazione e scegliere di auto-scaffolding un modello "Crea":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio salverà una nuova visualizzazione "Create.aspx" basata su scaffolding nella directory "Views" e aprirlo all'interno dell'IDE:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Apportiamo alcune modifiche al file di scaffold "create" predefinito che è stato generato per noi e modifichiamolo in modo che sia simile al seguente:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

E ora quando eseguiamo l'applicazione e accedere all'URL *"/Dinners/Create"* all'interno del browser eseguirà il rendering dell'interfaccia utente come di seguito dall'implementazione Crea azione:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementazione del metodo di azione di creazione HTTP-POSTImplementing the HTTP-POST Create Action Method

Abbiamo implementato la versione HTTP-GET del nostro metodo di azione Create. Quando un utente fa clic sul pulsante "Salva" esegue un post di modulo &lt;all'URL */Dinners/Create* e invia i valori del modulo di input&gt; HTML utilizzando il verbo HTTP POST.

È ora possibile implementare il comportamento HTTP POST del metodo di azione di creazione. Inizieremo aggiungendo un metodo di azione "Create" di overload al nostro DinnersController che ha un attributo "AcceptVerbs" che indica che gestisce gli scenari HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Ci sono una varietà di modi in cui possiamo accedere ai parametri del modulo pubblicato all'interno del nostro metodo HTTP-POST abilitato "Crea".

Un approccio consiste nel creare un nuovo oggetto Dinner e quindi utilizzare il metodo helper *UpdateModel()* (come abbiamo fatto con l'azione Edit) per popolarlo con i valori del modulo pubblicato. Possiamo quindi aggiungerlo al nostro DinnerRepository, renderlo persistente al database e reindirizzare l'utente alla nostra azione Dettagli per mostrare la cena appena creata utilizzando il codice seguente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

In alternativa, possiamo usare un approccio in cui abbiamo il nostro metodo di azione Create() prendere un Dinner oggetto come parametro del metodo. ASP.NET MVC creerà automaticamente un'istanza di un nuovo oggetto Dinner, ne popolerà le proprietà utilizzando gli input del modulo e lo passerà al metodo di azione:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Il metodo di azione precedente verifica che l'oggetto Dinner sia stato popolato correttamente con i valori post del form controllando la proprietà ModelState.IsValid. Verrà restituito false se sono presenti problemi di conversione di input (ad esempio: una stringa di "BOGUS" per il EventDate proprietà) e se si verificano problemi il metodo di azione visualizza nuovamente il form.

Se i valori di input sono validi, il metodo di azione tenta di aggiungere e salvare il nuovo Dinner per il DinnerRepository. Esegue il wrapping di questo lavoro all'interno di un blocco try/catch e visualizza nuovamente il form in caso di violazioni delle regole di business (che causerebbero la generazione di un'eccezione dal metodo dinnerRepository.Save()).

Per visualizzare questo comportamento di gestione degli errori in azione, è possibile richiedere l'opzione */Dinners/Create* URL e immettere i dettagli relativi a una nuova cena. L'input o i valori non corretti faranno sì che il modulo di creazione venga visualizzato nuovamente con gli errori evidenziati come di seguito:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Si noti come il modulo Crea rispetti esattamente le stesse regole di convalida e di business del modulo di modifica. Ciò è dovuto al fatto che le regole di convalida e business sono state definite nel modello e non sono incorporate nell'interfaccia utente o nel controller dell'applicazione. Ciò significa che in seguito è possibile modificare/evolvere le regole di convalida o di business in un'unica posizione e applicarle in tutta l'applicazione. Non sarà necessario modificare alcun codice all'interno dei nostri metodi di azione Edit o Create per rispettare automaticamente eventuali nuove regole o modifiche a quelle esistenti.

Quando si fissano i valori di input e fare clic sul pulsante "Salva" nuovamente, la nostra aggiunta al DinnerRepository avrà esito positivo, e una nuova cena verrà aggiunto al database. Verremo quindi reindirizzati all'URL */Dinners/Details/[id]* – dove ci verranno presentati i dettagli sulla cena appena creata:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Elimina supporto

È ora possibile aggiungere il supporto "Delete" per il nostro DinnersController.Let's now add "Delete" support to our DinnersController.

#### <a name="the-http-get-delete-action-method"></a>Il metodo di azione di eliminazione HTTP-GETThe HTTP-GET Delete Action Method

Inizieremo implementando il comportamento HTTP GET del metodo di azione delete. Questo metodo verrà chiamato quando qualcuno visita l'URL */Dinners/Delete/[id].* Di seguito è riportata l'implementazione:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

Il metodo di azione tenta di recuperare la cena da eliminare. Se il Dinner esiste, esegue il rendering di una visualizzazione basata sull'oggetto Dinner. Se l'oggetto non esiste (o è già stato eliminato) restituisce una visualizzazione che esegue il rendering del modello di visualizzazione "NotFound" creato in precedenza per il metodo di azione "Dettagli".

È possibile creare il modello di visualizzazione "Elimina" facendo clic con il pulsante destro del mouse sul metodo di azione Elimina e selezionando il comando del menu di scelta rapida "Aggiungi visualizzazione". All'interno della finestra di dialogo "Aggiungi visualizzazione" indicheremo che stiamo passando un Dinner oggetto al modello di visualizzazione come modello e scegliere di creare un modello vuoto:All the "Add View" dialog we'll indicate that we are passing a Dinner object to our view template as its model, and choose to create an empty template:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio aggiungerà un nuovo file di modello di visualizzazione "Delete.aspx" per noi all'interno della nostra directory "Views". Aggiungeremo codice HTML e codice al modello per implementare una schermata di conferma dell'eliminazione come di seguito:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

Il codice precedente visualizza il titolo della cena da &lt;&gt; eliminare e restituisce un elemento del modulo che esegue un POST all'URL /Dinners/Delete/[id] se l'utente finale fa clic sul pulsante "Elimina" al suo interno.

Quando si esegue l'applicazione e si accede all'URL *"/Dinners/Delete/[id]"* per un oggetto Dinner valido, viene eseguito il rendering dell'interfaccia utente come di seguito:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Argomento collaterale: Perché stiamo facendo un POST?** |
| --- |
| Potresti chiederti: perché abbiamo fatto il &lt;&gt; nostro sforzo di creare un modulo all'interno della nostra schermata di conferma Elimina? Perché non utilizzare semplicemente un collegamento ipertestuale standard per il collegamento a un metodo di azione che esegue l'operazione di eliminazione effettiva? Il motivo è che vogliamo stare attenti a guardarci da web-crawler e motori di ricerca che scoprono i nostri URL e causano inavvertitamente l'eliminazione dei dati quando seguono i link. Gli URL basati su HTTP-GET sono considerati "sicuri" per l'accesso/ricerca per indicizzazione e si suppone che non seguano quelli HTTP-POST. Una buona regola consiste nell'assicurarsi di inserire sempre operazioni distruttive o di modifica dei dati dietro le richieste HTTP-POST. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implementazione del metodo di azione di eliminazione HTTP-POSTImplementing the HTTP-POST Delete Action Method

Ora abbiamo la versione HTTP-GET del nostro metodo di azione Delete implementato che visualizza una schermata di conferma di eliminazione. Quando un utente finale fa clic sul pulsante "Elimina" eseguirà un post di modulo per l'URL */Dinners/Dinner/[id].*

È ora possibile implementare il comportamento HTTP "POST" del metodo di azione di eliminazione utilizzando il codice seguente:Let's now implement the HTTP "POST" behavior of the delete action method using the code below:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

La versione HTTP-POST del metodo di azione Delete tenta di recuperare l'oggetto dinner da eliminare. Se non riesce a trovarlo (perché è già stato eliminato) esegue il rendering del modello "NotFound". Se trova la cena, la elimina dal DinnerRepository. Viene quindi eseguito il rendering di un modello "Eliminato".

Per implementare il modello "Eliminato" fare clic con il pulsante destro del mouse nel metodo di azione e scegliere il menu di scelta rapida "Aggiungi visualizzazione". Chiameremo la nostra vista "Eliminato" e faremo in modo che sia un modello vuoto (e non prendere un oggetto modello fortemente tipizzato). Aggiungeremo quindi alcuni contenuti HTML:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

E ora quando eseguiamo la nostra applicazione e accedere all'URL *"/Dinners/Delete/[id]"* per un oggetto Dinner valido, verrà visualizzata la schermata di conferma dell'eliminazione della cena come di seguito:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Quando si fa clic sul pulsante "Elimina" eseguirà un HTTP-POST per l'URL */Dinners/Delete/[id],* che eliminerà la cena dal nostro database, e visualizzerà il nostro modello di vista "Eliminato":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Sicurezza dell'associazione al modello

Sono stati illustrati due modi diversi per usare le funzionalità di associazione di modelli incorporate di MVC di ASP.NET MVC. Il primo utilizzando il UpdateModel() metodo per aggiornare le proprietà su un oggetto modello esistente e il secondo utilizzando ASP.NET supporto di MVC per il passaggio di oggetti del modello come parametri del metodo di azione. Entrambe queste tecniche sono molto potenti ed estremamente utili.

Questo potere comporta anche la responsabilità. È importante essere sempre paranoici sulla sicurezza quando si accetta qualsiasi input dell'utente, e questo è vero anche quando si associano oggetti per formare l'input. Si dovrebbe fare attenzione a codificare sempre in HTML tutti i valori immessi dall'utente per evitare attacchi injection HTML e JavaScript e fare attenzione agli attacchi SQL injection (nota: stiamo usando LINQ to SQL per la nostra applicazione, che codifica automaticamente i parametri per prevenire questi tipi di attacchi). Non dovresti mai fare affidamento sulla sola convalida lato client e utilizzare sempre la convalida lato server per proteggerti dagli hacker che tentano di inviarti valori fasulli.

Un elemento di sicurezza aggiuntivo per assicurarsi di pensare quando si utilizzano le funzionalità di associazione di ASP.NET MVC è l'ambito degli oggetti che si stanno associando. In particolare, si desidera assicurarsi di comprendere le implicazioni di sicurezza delle proprietà che si consente di essere associati e assicurarsi di consentire solo quelle proprietà che in realtà devono essere aggiornabili da un utente finale da aggiornare.

Per impostazione predefinita, il metodo UpdateModel() tenterà di aggiornare tutte le proprietà dell'oggetto modello che corrispondono ai valori dei parametri del form in ingresso. Analogamente, gli oggetti passati come parametri del metodo di azione anche per impostazione predefinita possono avere tutte le proprietà impostate tramite i parametri del modulo.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Blocco dell'associazione in base all'utilizzo

È possibile bloccare i criteri di associazione in base all'utilizzo fornendo un "elenco di inclusione" esplicito di proprietà che possono essere aggiornate. Questa operazione può essere eseguita passando un parametro di matrice di stringhe aggiuntivo al metodo UpdateModel() come indicato di seguito:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Gli oggetti passati come parametri del metodo di azione supportano anche un attributo [Bind] che consente di specificare un "elenco di inclusione" delle proprietà consentite come di seguito:Objects passed as action method parameters also support a [Bind] attribute that enables an "include list" of allowed properties to be specified like below:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Blocco dell'associazione in base al tipo

È anche possibile bloccare le regole di associazione in base al tipo. In questo modo è possibile specificare le regole di associazione una sola volta e quindi applicarle in tutti gli scenari (inclusi gli scenari di parametro UpdateModel e metodo di azione) in tutti i controller e metodi di azione.

È possibile personalizzare le regole di associazione per tipo aggiungendo un [Bind] attributo a un tipo o registrandolo all'interno del file Global.asax dell'applicazione (utile per gli scenari in cui non si è proprietari del tipo). È quindi possibile utilizzare le proprietà Include ed Exclude dell'attributo Bind per controllare quali proprietà sono associabili per la classe o l'interfaccia specifica.

Useremo questa tecnica per la classe Dinner nell'applicazione NerdDinner e aggiungeremo un attributo [Bind] che limita l'elenco di proprietà associabili a quanto segue:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Si noti che non è consentito la raccolta RSVP da modificare tramite binding, né è possibile consentire il DinnerID o HostedBy proprietà da impostare tramite binding. Per motivi di sicurezza, invece, manipoleremo queste proprietà particolari usando codice esplicito all'interno dei nostri metodi di azione.

### <a name="crud-wrap-up"></a>Wrapping CRUD

ASP.NET MVC include una serie di funzionalità incorporate che consentono di implementare scenari di inserimento di form. Abbiamo usato una varietà di queste funzionalità per fornire il supporto dell'interfaccia utente CRUD in cima al nostro DinnerRepository.

Stiamo usando un approccio incentrato sul modello per implementare la nostra applicazione. Ciò significa che tutta la logica di convalida e regola business è definita all'interno del livello del modello e non all'interno dei controller o delle visualizzazioni. Né la classe Controller né i modelli di visualizzazione conoscono le regole di business specifiche applicate dalla classe modello Dinner.

In questo modo l'architettura dell'applicazione verrà mantenuta pulita e sarà più semplice da testare. È possibile aggiungere ulteriori regole di business al layer del modello in futuro e *non è necessario apportare modifiche* al codice per il Controller o la visualizzazione per poter essere supportate. Questo ci fornirà una grande agilità per evolvere e cambiare la nostra applicazione in futuro.

Il nostro DinnersController ora abilita gli elenchi/dettagli della cena, nonché il supporto per la creazione, la modifica e l'eliminazione. Il codice completo per la classe è disponibile di seguito:The complete code for the class can be found below:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>passaggio successivo

Ora abbiamo CRUD di base (Creazione, Lettura, Aggiornamento ed Elimina) implementare all'interno della nostra classe DinnersController.We now have basic CRUD (Create, Read, Update and Delete) support implement within our DinnersController class.

Esaminiamo ora come è possibile usare le classi ViewData e ViewModel per abilitare un'interfaccia utente ancora più ricca nei form.

> [!div class="step-by-step"]
> [Successivo](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [precedente](use-viewdata-and-implement-viewmodel-classes.md)
