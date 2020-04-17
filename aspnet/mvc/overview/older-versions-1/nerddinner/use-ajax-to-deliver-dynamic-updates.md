---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Utilizzo di AJAX per il recapito di aggiornamenti dinamici Documenti Microsoft
author: rick-anderson
description: Passaggio 10 implementa il supporto per gli utenti connessi a RSVP il loro interesse a partecipare a una cena, utilizzando un approccio basato su Ajax integrato nei dettagli della cena...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 13680b91edc665852fd4af56e4fc79551e6de15e
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541247"
---
# <a name="use-ajax-to-deliver-dynamic-updates"></a>Usare AJAX per distribuire aggiornamenti dinamici

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 10 di un'esercitazione gratuita [sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come compilare un'applicazione Web piccola ma completa usando ASP.NET MVC 1.
> 
> Passaggio 10 implementa il supporto per gli utenti connessi a RSVP il loro interesse a partecipare a una cena, utilizzando un approccio basato su Ajax integrato all'interno della pagina dei dettagli della cena.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [ALL'archivio musicale MVC.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>Passaggio 10 di NerdDinner: Abilitazione AJAX RSVP accettaNerdDinner Step 10: AJAX Enabling RSSpPs Accepts

Ora implementiamo il supporto per gli utenti connessi a RSVP il loro interesse a partecipare a una cena. Abiliteremo questo utilizzando un approccio basato su AJAX integrato all'interno della pagina dei dettagli della cena.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Indicare se l'utente è RSVP'd

Gli utenti possono visitare l'URL */Dinners/Details/[id*] per visualizzare i dettagli relativi a una determinata cena:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

Il metodo di azione Details() viene implementato in questo modo:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Il primo passaggio per implementare il supporto RSVP consisterà nell'aggiungere un metodo helper "IsUserRegistered(username)" all'oggetto Dinner (all'interno dell'Dinner.cs classe parziale che abbiamo creato in precedenza). Questo metodo helper restituisce true o false a seconda che l'utente sia attualmente RSVP'd per la cena:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

È quindi possibile aggiungere il codice seguente al modello di visualizzazione Details.aspx per visualizzare un messaggio appropriato che indica se l'utente è registrato o meno per l'evento:We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

E ora quando un utente visita una cena per cui è registrato vedrà questo messaggio:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

E quando visitano una cena non sono registrati per vedranno il seguente messaggio:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implementazione del metodo di azione RegisterImplementing the Register Action Method

Aggiungiamo ora la funzionalità necessaria per consentire agli utenti di RSVP per una cena dalla pagina dei dettagli.

Per implementare questo, creeremo una nuova classe "RSVPController" facendo clic con il&gt;pulsante destro del mouse sulla directory Controllers e scegliendo il comando di menu Aggiungi Controller.

Implementeremo un metodo di azione "Register" all'interno della nuova classe RSVPController che accetta un id per una cena come argomento, recupera l'oggetto Dinner appropriato, controlla se l'utente connesso è attualmente presente nell'elenco degli utenti che si sono registrati per esso e se non aggiunge un oggetto RSVP per loro:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Si noti sopra come stiamo restituendo una semplice stringa come output del metodo di azione. Potremmo incorporare questo messaggio all'interno di un modello di visualizzazione, ma dato che è così piccolo useremo solo il metodo helper Content() nella classe base del controller e restituiremo un messaggio stringa come sopra.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Chiamata del metodo di azione RSVPForEvent tramite AJAXCalling the RSVPForEvent Action Method using AJAX

Useremo AJAX per richiamare il metodo di azione Register dalla visualizzazione Dettagli. Implementare questo è abbastanza facile. Per prima cosa aggiungeremo due riferimenti alla libreria di script:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

La prima libreria fa riferimento alla libreria di script sul lato client AJAX di base ASP.NET. Questo file ha una dimensione di circa 24 k (compresso) e contiene funzionalità AJAX lato client di base. La seconda libreria contiene funzioni di utilità che si integrano con ASP.NET metodi helper AJAX incorporati di MVC (che useremo a breve).

È quindi possibile aggiornare il codice del modello di visualizzazione aggiunto in precedenza in modo che anziché inviare un messaggio "Non si è registrati per questo evento", viene invece eseguito il rendering di un collegamento che quando premuto esegue una chiamata AJAX che richiama il metodo di azione RSVPForEvent sul controller RSVP e RSVP l'utente:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

Il metodo helper Ajax.ActionLink() utilizzato in precedenza è incorporato ASP.NET MVC ed è simile al metodo helper Html.ActionLink(), ad eccezione del fatto che, anziché eseguire una navigazione standard, viene eseguita una chiamata AJAX al metodo di azione quando si fa clic sul collegamento. Sopra stiamo chiamando il metodo di azione "Register" sul controller "RSVP" e passando il DinnerID come parametro "id" ad esso. Il parametro Finale AjaxOptions che stiamo passando indica che vogliamo prendere il &lt;contenuto&gt; restituito dal metodo di azione e aggiornare l'elemento div HTML nella pagina il cui id è "rsvpmsg".

E ora, quando un utente accede a una cena per cui non è ancora registrato, visualizzerà un collegamento a RSVP:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Se fanno clic sul collegamento "RSVP per questo evento" effettueranno una chiamata AJAX al metodo di azione Register sul controller RSVP e, al termine, verrà visualizzato un messaggio aggiornato come di seguito:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

La larghezza di banda di rete e il traffico coinvolti quando si effettua questa chiamata AJAX è davvero leggero. Quando l'utente fa clic sul collegamento "RSVP per questo evento", viene effettuata una piccola richiesta di rete HTTP POST all'URL */Dinners/Register/1* che si presenta come di seguito in transito:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

E la risposta dal nostro metodo di azione Register è semplicemente:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Questa chiamata leggera è veloce e funzionerà anche su una rete lenta.

### <a name="adding-a-jquery-animation"></a>Aggiunta di un'animazione jQueryAdding a jQuery Animation

La funzionalità AJAX che abbiamo implementato funziona bene e velocemente. A volte può accadere così velocemente, però, che un utente potrebbe non notare che il collegamento RSVP è stato sostituito con nuovo testo. Per rendere il risultato un po 'più ovvio possiamo aggiungere una semplice animazione per attirare l'attenzione sul messaggio di aggiornamento.

Il modello di progetto MVC di ASP.NET predefinito include jQuery – una libreria JavaScript open source eccellente (e molto popolare) che è supportata anche da Microsoft. jQuery fornisce una serie di funzionalità, tra cui una bella selezione HTML DOM e libreria di effetti.

Per usare jQuery aggiungeremo prima un riferimento di script ad esso. Poiché useremo jQuery all'interno di una varietà di posizioni all'interno del nostro sito, aggiungeremo il riferimento allo script all'interno del nostro file di pagina master Site.master in modo che tutte le pagine possano usarlo.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Suggerimento: assicurarsi di aver installato l'hotfix JavaScript intellisense per VS 2008 SP1 che consente il supporto più completo intellisense per i file JavaScript (incluso jQuery). È possibile scaricarlo da:http://tinyurl.com/vs2008javascripthotfix*

Il codice scritto utilizzando JQuery utilizza spesso un metodo JavaScript globale di tipo "()" che recupera uno o più elementi HTML utilizzando un selettore CSS. Ad esempio, *l'opzione "("#rsvpmsg")* seleziona qualsiasi elemento HTML con id di rsvpmsg, mentre *.(".qualcosa")* seleziona tutti gli elementi con il nome della classe CSS "something". È inoltre possibile scrivere query più avanzate, ad esempio "restituire tutti i pulsanti di opzione selezionati" utilizzando una query del selettore, ad esempio: *.@type@checked*

Una volta selezionati gli elementi, è possibile chiamare i metodi su di essi per eseguire un'azione, ad esempio nasconderli: *#rsvpmsg .*

Per il nostro scenario RSVP, definiremo una semplice funzione JavaScript denominata "AnimateRSVPMessage"&gt; che seleziona il div "rsvpmsg" &lt;e anima le dimensioni del contenuto di testo. Il codice seguente avvia il testo piccolo e quindi fa sì che aumenti in un intervallo di tempo di 400 millisecondi:The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

È quindi possibile collegare questa funzione JavaScript da chiamare dopo il completamento della chiamata AJAX passando il relativo nome al metodo helper Ajax.ActionLink() (tramite la proprietà dell'evento AjaxOptions "OnSuccess"):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

E ora quando si fa clic sul collegamento "RSVP per questo evento" e la nostra chiamata AJAX viene completata correttamente, il messaggio di contenuto inviato verrà animato e grande sarà grande:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Oltre a fornire un evento "OnSuccess", l'oggetto AjaxOptions espone gli eventi OnBegin, OnFailure e OnComplete che è possibile gestire (insieme a un'ampia gamma di altre proprietà e opzioni utili).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Pulizia - Effettuare il refactoring di una visualizzazione parziale RSVPCleanup - Refactor out a RSVP Partial View

Il nostro modello di visualizzazione dei dettagli sta iniziando a diventare un po 'lungo, che lo straordinario renderà un po 'più difficile da capire. Per migliorare la leggibilità del codice, finiamo con la creazione di una visualizzazione parziale, RSVPStatus.ascx, che incapsula tutto il codice di visualizzazione RSVP per la pagina dei dettagli.

A tale scopo, è possibile fare clic con il pulsante&gt;destro del mouse sulla cartella "Views" e quindi scegliere il comando di menu Aggiungi- Visualizza. Avremo prendere un Dinner oggetto come il suo fortemente tipizzato ViewModel. È quindi possibile copiare/incollare il contenuto RSVP dalla visualizzazione Details.aspx in esso.

Una volta fatto questo, creiamo anche un'altra visualizzazione parziale, EditAndDeleteLinks.ascx, che incapsula il codice della visualizzazione di collegamento Edit ed Delete. Avremo anche prendere un Dinner oggetto come il suo ViewModel fortemente tipizzato e copiare/incollare la logica di modifica e eliminazione dalla visualizzazione Details.aspx in esso.

Il nostro modello di visualizzazione dei dettagli può quindi includere solo due chiamate al metodo Html.RenderPartial() nella parte inferiore:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

In questo modo il codice più pulito da leggere e gestire.

### <a name="next-step"></a>passaggio successivo

Esaminiamo ora come possiamo usare ulteriormente AJAX e aggiungere il supporto di mapping interattivo alla nostra applicazione.

> [!div class="step-by-step"]
> [Successivo](secure-applications-using-authentication-and-authorization.md)
> [precedente](use-ajax-to-implement-mapping-scenarios.md)
