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
# <a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="9a0e3-103">Usare AJAX per distribuire aggiornamenti dinamici</span><span class="sxs-lookup"><span data-stu-id="9a0e3-103">Use AJAX to Deliver Dynamic Updates</span></span>

<span data-ttu-id="9a0e3-104">da parte [di Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="9a0e3-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="9a0e3-105">Scarica il PDF</span><span class="sxs-lookup"><span data-stu-id="9a0e3-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="9a0e3-106">Questo è il passaggio 10 di un'esercitazione gratuita [sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come compilare un'applicazione Web piccola ma completa usando ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="9a0e3-107">Passaggio 10 implementa il supporto per gli utenti connessi a RSVP il loro interesse a partecipare a una cena, utilizzando un approccio basato su Ajax integrato all'interno della pagina dei dettagli della cena.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="9a0e3-108">Se si utilizza ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [ALL'archivio musicale MVC.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="9a0e3-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="9a0e3-109">Passaggio 10 di NerdDinner: Abilitazione AJAX RSVP accettaNerdDinner Step 10: AJAX Enabling RSSpPs Accepts</span><span class="sxs-lookup"><span data-stu-id="9a0e3-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="9a0e3-110">Ora implementiamo il supporto per gli utenti connessi a RSVP il loro interesse a partecipare a una cena.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="9a0e3-111">Abiliteremo questo utilizzando un approccio basato su AJAX integrato all'interno della pagina dei dettagli della cena.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="9a0e3-112">Indicare se l'utente è RSVP'd</span><span class="sxs-lookup"><span data-stu-id="9a0e3-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="9a0e3-113">Gli utenti possono visitare l'URL */Dinners/Details/[id*] per visualizzare i dettagli relativi a una determinata cena:</span><span class="sxs-lookup"><span data-stu-id="9a0e3-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="9a0e3-114">Il metodo di azione Details() viene implementato in questo modo:</span><span class="sxs-lookup"><span data-stu-id="9a0e3-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="9a0e3-115">Il primo passaggio per implementare il supporto RSVP consisterà nell'aggiungere un metodo helper "IsUserRegistered(username)" all'oggetto Dinner (all'interno dell'Dinner.cs classe parziale che abbiamo creato in precedenza).</span><span class="sxs-lookup"><span data-stu-id="9a0e3-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="9a0e3-116">Questo metodo helper restituisce true o false a seconda che l'utente sia attualmente RSVP'd per la cena:</span><span class="sxs-lookup"><span data-stu-id="9a0e3-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="9a0e3-117">È quindi possibile aggiungere il codice seguente al modello di visualizzazione Details.aspx per visualizzare un messaggio appropriato che indica se l'utente è registrato o meno per l'evento:We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span><span class="sxs-lookup"><span data-stu-id="9a0e3-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="9a0e3-118">E ora quando un utente visita una cena per cui è registrato vedrà questo messaggio:</span><span class="sxs-lookup"><span data-stu-id="9a0e3-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="9a0e3-119">E quando visitano una cena non sono registrati per vedranno il seguente messaggio:</span><span class="sxs-lookup"><span data-stu-id="9a0e3-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="9a0e3-120">Implementazione del metodo di azione RegisterImplementing the Register Action Method</span><span class="sxs-lookup"><span data-stu-id="9a0e3-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="9a0e3-121">Aggiungiamo ora la funzionalità necessaria per consentire agli utenti di RSVP per una cena dalla pagina dei dettagli.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="9a0e3-122">Per implementare questo, creeremo una nuova classe "RSVPController" facendo clic con il&gt;pulsante destro del mouse sulla directory Controllers e scegliendo il comando di menu Aggiungi Controller.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="9a0e3-123">Implementeremo un metodo di azione "Register" all'interno della nuova classe RSVPController che accetta un id per una cena come argomento, recupera l'oggetto Dinner appropriato, controlla se l'utente connesso è attualmente presente nell'elenco degli utenti che si sono registrati per esso e se non aggiunge un oggetto RSVP per loro:</span><span class="sxs-lookup"><span data-stu-id="9a0e3-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="9a0e3-124">Si noti sopra come stiamo restituendo una semplice stringa come output del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="9a0e3-125">Potremmo incorporare questo messaggio all'interno di un modello di visualizzazione, ma dato che è così piccolo useremo solo il metodo helper Content() nella classe base del controller e restituiremo un messaggio stringa come sopra.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="9a0e3-126">Chiamata del metodo di azione RSVPForEvent tramite AJAXCalling the RSVPForEvent Action Method using AJAX</span><span class="sxs-lookup"><span data-stu-id="9a0e3-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="9a0e3-127">Useremo AJAX per richiamare il metodo di azione Register dalla visualizzazione Dettagli.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="9a0e3-128">Implementare questo è abbastanza facile.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="9a0e3-129">Per prima cosa aggiungeremo due riferimenti alla libreria di script:</span><span class="sxs-lookup"><span data-stu-id="9a0e3-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="9a0e3-130">La prima libreria fa riferimento alla libreria di script sul lato client AJAX di base ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="9a0e3-131">Questo file ha una dimensione di circa 24 k (compresso) e contiene funzionalità AJAX lato client di base.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="9a0e3-132">La seconda libreria contiene funzioni di utilità che si integrano con ASP.NET metodi helper AJAX incorporati di MVC (che useremo a breve).</span><span class="sxs-lookup"><span data-stu-id="9a0e3-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="9a0e3-133">È quindi possibile aggiornare il codice del modello di visualizzazione aggiunto in precedenza in modo che anziché inviare un messaggio "Non si è registrati per questo evento", viene invece eseguito il rendering di un collegamento che quando premuto esegue una chiamata AJAX che richiama il metodo di azione RSVPForEvent sul controller RSVP e RSVP l'utente:</span><span class="sxs-lookup"><span data-stu-id="9a0e3-133">We can then update the view template code we added earlier so that instead of outputting a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="9a0e3-134">Il metodo helper Ajax.ActionLink() utilizzato in precedenza è incorporato ASP.NET MVC ed è simile al metodo helper Html.ActionLink(), ad eccezione del fatto che, anziché eseguire una navigazione standard, viene eseguita una chiamata AJAX al metodo di azione quando si fa clic sul collegamento.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="9a0e3-135">Sopra stiamo chiamando il metodo di azione "Register" sul controller "RSVP" e passando il DinnerID come parametro "id" ad esso.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="9a0e3-136">Il parametro Finale AjaxOptions che stiamo passando indica che vogliamo prendere il &lt;contenuto&gt; restituito dal metodo di azione e aggiornare l'elemento div HTML nella pagina il cui id è "rsvpmsg".</span><span class="sxs-lookup"><span data-stu-id="9a0e3-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="9a0e3-137">E ora, quando un utente accede a una cena per cui non è ancora registrato, visualizzerà un collegamento a RSVP:</span><span class="sxs-lookup"><span data-stu-id="9a0e3-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="9a0e3-138">Se fanno clic sul collegamento "RSVP per questo evento" effettueranno una chiamata AJAX al metodo di azione Register sul controller RSVP e, al termine, verrà visualizzato un messaggio aggiornato come di seguito:</span><span class="sxs-lookup"><span data-stu-id="9a0e3-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="9a0e3-139">La larghezza di banda di rete e il traffico coinvolti quando si effettua questa chiamata AJAX è davvero leggero.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="9a0e3-140">Quando l'utente fa clic sul collegamento "RSVP per questo evento", viene effettuata una piccola richiesta di rete HTTP POST all'URL */Dinners/Register/1* che si presenta come di seguito in transito:</span><span class="sxs-lookup"><span data-stu-id="9a0e3-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="9a0e3-141">E la risposta dal nostro metodo di azione Register è semplicemente:</span><span class="sxs-lookup"><span data-stu-id="9a0e3-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="9a0e3-142">Questa chiamata leggera è veloce e funzionerà anche su una rete lenta.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="9a0e3-143">Aggiunta di un'animazione jQueryAdding a jQuery Animation</span><span class="sxs-lookup"><span data-stu-id="9a0e3-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="9a0e3-144">La funzionalità AJAX che abbiamo implementato funziona bene e velocemente.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="9a0e3-145">A volte può accadere così velocemente, però, che un utente potrebbe non notare che il collegamento RSVP è stato sostituito con nuovo testo.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="9a0e3-146">Per rendere il risultato un po 'più ovvio possiamo aggiungere una semplice animazione per attirare l'attenzione sul messaggio di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="9a0e3-147">Il modello di progetto MVC di ASP.NET predefinito include jQuery – una libreria JavaScript open source eccellente (e molto popolare) che è supportata anche da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="9a0e3-148">jQuery fornisce una serie di funzionalità, tra cui una bella selezione HTML DOM e libreria di effetti.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="9a0e3-149">Per usare jQuery aggiungeremo prima un riferimento di script ad esso.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="9a0e3-150">Poiché useremo jQuery all'interno di una varietà di posizioni all'interno del nostro sito, aggiungeremo il riferimento allo script all'interno del nostro file di pagina master Site.master in modo che tutte le pagine possano usarlo.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="9a0e3-151">*Suggerimento: assicurarsi di aver installato l'hotfix JavaScript intellisense per VS 2008 SP1 che consente il supporto più completo intellisense per i file JavaScript (incluso jQuery). È possibile scaricarlo da:http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="9a0e3-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="9a0e3-152">Il codice scritto utilizzando JQuery utilizza spesso un metodo JavaScript globale di tipo "()" che recupera uno o più elementi HTML utilizzando un selettore CSS.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="9a0e3-153">Ad esempio, *l'opzione "("#rsvpmsg")* seleziona qualsiasi elemento HTML con id di rsvpmsg, mentre *.(".qualcosa")* seleziona tutti gli elementi con il nome della classe CSS "something".</span><span class="sxs-lookup"><span data-stu-id="9a0e3-153">For example, *$("#rsvpmsg")* selects any HTML element with the id of rsvpmsg, while *$(".something")* would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="9a0e3-154">È inoltre possibile scrivere query più avanzate, ad esempio "restituire tutti i pulsanti di opzione selezionati" utilizzando una query del selettore, ad esempio: *.@type@checked*</span><span class="sxs-lookup"><span data-stu-id="9a0e3-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: *$("input[@type=radio][@checked]")*.</span></span>

<span data-ttu-id="9a0e3-155">Una volta selezionati gli elementi, è possibile chiamare i metodi su di essi per eseguire un'azione, ad esempio nasconderli: *#rsvpmsg .*</span><span class="sxs-lookup"><span data-stu-id="9a0e3-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="9a0e3-156">Per il nostro scenario RSVP, definiremo una semplice funzione JavaScript denominata "AnimateRSVPMessage"&gt; che seleziona il div "rsvpmsg" &lt;e anima le dimensioni del contenuto di testo.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="9a0e3-157">Il codice seguente avvia il testo piccolo e quindi fa sì che aumenti in un intervallo di tempo di 400 millisecondi:The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span><span class="sxs-lookup"><span data-stu-id="9a0e3-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="9a0e3-158">È quindi possibile collegare questa funzione JavaScript da chiamare dopo il completamento della chiamata AJAX passando il relativo nome al metodo helper Ajax.ActionLink() (tramite la proprietà dell'evento AjaxOptions "OnSuccess"):</span><span class="sxs-lookup"><span data-stu-id="9a0e3-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="9a0e3-159">E ora quando si fa clic sul collegamento "RSVP per questo evento" e la nostra chiamata AJAX viene completata correttamente, il messaggio di contenuto inviato verrà animato e grande sarà grande:</span><span class="sxs-lookup"><span data-stu-id="9a0e3-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="9a0e3-160">Oltre a fornire un evento "OnSuccess", l'oggetto AjaxOptions espone gli eventi OnBegin, OnFailure e OnComplete che è possibile gestire (insieme a un'ampia gamma di altre proprietà e opzioni utili).</span><span class="sxs-lookup"><span data-stu-id="9a0e3-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="9a0e3-161">Pulizia - Effettuare il refactoring di una visualizzazione parziale RSVPCleanup - Refactor out a RSVP Partial View</span><span class="sxs-lookup"><span data-stu-id="9a0e3-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="9a0e3-162">Il nostro modello di visualizzazione dei dettagli sta iniziando a diventare un po 'lungo, che lo straordinario renderà un po 'più difficile da capire.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="9a0e3-163">Per migliorare la leggibilità del codice, finiamo con la creazione di una visualizzazione parziale, RSVPStatus.ascx, che incapsula tutto il codice di visualizzazione RSVP per la pagina dei dettagli.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="9a0e3-164">A tale scopo, è possibile fare clic con il pulsante&gt;destro del mouse sulla cartella "Views" e quindi scegliere il comando di menu Aggiungi- Visualizza.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="9a0e3-165">Avremo prendere un Dinner oggetto come il suo fortemente tipizzato ViewModel.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="9a0e3-166">È quindi possibile copiare/incollare il contenuto RSVP dalla visualizzazione Details.aspx in esso.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="9a0e3-167">Una volta fatto questo, creiamo anche un'altra visualizzazione parziale, EditAndDeleteLinks.ascx, che incapsula il codice della visualizzazione di collegamento Edit ed Delete.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="9a0e3-168">Avremo anche prendere un Dinner oggetto come il suo ViewModel fortemente tipizzato e copiare/incollare la logica di modifica e eliminazione dalla visualizzazione Details.aspx in esso.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="9a0e3-169">Il nostro modello di visualizzazione dei dettagli può quindi includere solo due chiamate al metodo Html.RenderPartial() nella parte inferiore:</span><span class="sxs-lookup"><span data-stu-id="9a0e3-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="9a0e3-170">In questo modo il codice più pulito da leggere e gestire.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="9a0e3-171">passaggio successivo</span><span class="sxs-lookup"><span data-stu-id="9a0e3-171">Next Step</span></span>

<span data-ttu-id="9a0e3-172">Esaminiamo ora come possiamo usare ulteriormente AJAX e aggiungere il supporto di mapping interattivo alla nostra applicazione.</span><span class="sxs-lookup"><span data-stu-id="9a0e3-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9a0e3-173">[Successivo](secure-applications-using-authentication-and-authorization.md)
> [precedente](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="9a0e3-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
