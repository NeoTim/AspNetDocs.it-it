---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Riutilizzo dell'interfaccia utente tramite pagine Master e visualizzazioni parziali | Microsoft Docs
author: microsoft
description: Passaggio 7 esamina i metodi che è possibile applicare il principio DRY entro i modelli di vista per eliminare la duplicazione del codice, usando modelli di visualizzazione parziale e le pagine master.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: 0da32e6ac38f10df6e581517989b3b1fd2f2328c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57055998"
---
<a name="re-use-ui-using-master-pages-and-partials"></a><span data-ttu-id="c21f5-103">Riutilizzare l'interfaccia utente tramite pagine master e visualizzazioni parziali</span><span class="sxs-lookup"><span data-stu-id="c21f5-103">Re-use UI Using Master Pages and Partials</span></span>
====================
<span data-ttu-id="c21f5-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c21f5-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="c21f5-105">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="c21f5-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="c21f5-106">Si tratta di passaggio 7 di una liberazione [esercitazione sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-dettaglio come compilare una piccola, ma completa, applicazione web con ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="c21f5-106">This is step 7 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="c21f5-107">Passaggio 7 vengono esaminati modi che sevice del "principio DRY" all'interno dei nostri modelli di vista per eliminare la duplicazione del codice, usando modelli di visualizzazione parziale e le pagine master.</span><span class="sxs-lookup"><span data-stu-id="c21f5-107">Step 7 looks at ways we can apply the "DRY Principle" within our view templates to eliminate code duplication, using partial view templates and master pages.</span></span>
> 
> <span data-ttu-id="c21f5-108">Se si usa ASP.NET MVC 3, è consigliabile seguire le [Guida introduttiva con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oppure [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="c21f5-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-7-partials-and-master-pages"></a><span data-ttu-id="c21f5-109">NerdDinner passaggio 7: Righe parzialmente eseguite e pagine Master</span><span class="sxs-lookup"><span data-stu-id="c21f5-109">NerdDinner Step 7: Partials and Master Pages</span></span>

<span data-ttu-id="c21f5-110">Una delle filosofie di progettazione che ASP.NET MVC implementa è il principio di "Eseguire operazioni non ' t Repeat Yourself" (noto come "Prova").</span><span class="sxs-lookup"><span data-stu-id="c21f5-110">One of the design philosophies ASP.NET MVC embraces is the "Do Not Repeat Yourself" principle (commonly referred to as "DRY").</span></span> <span data-ttu-id="c21f5-111">Una progettazione DRY consente di eliminare la duplicazione del codice e per la logica, che in definitiva rende più veloce per compilare e maggiore facile di gestione di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="c21f5-111">A DRY design helps eliminate the duplication of code and logic, which ultimately makes applications faster to build and easier to maintain.</span></span>

<span data-ttu-id="c21f5-112">Abbiamo già visto il principio DRY applicato in molti gli scenari di NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="c21f5-112">We've already seen the DRY principle applied in several of our NerdDinner scenarios.</span></span> <span data-ttu-id="c21f5-113">Alcuni esempi: entro il livello del modello, in modo da essere applicato a entrambe le modifica e creare scenari nel nostro controller; viene implementata la logica di convalida Utilizziamo nuovamente il modello di vista "NotFound" tra i metodi di azione di modifica, dettagli e l'eliminazione; si sta usando un modello di denominazione convenzione - con i modelli di visualizzazione, che elimina la necessità di specificare in modo esplicito il nome quando si chiama il metodo helper View(); e viene nuovamente Usa la classe DinnerFormViewModel per entrambe le modifica e creare scenari di azione.</span><span class="sxs-lookup"><span data-stu-id="c21f5-113">A few examples: our validation logic is implemented within our model layer, which enables it to be enforced across both edit and create scenarios in our controller; we are re-using the "NotFound" view template across the Edit, Details and Delete action methods; we are using a convention- naming pattern with our view templates, which eliminates the need to explicitly specify the name when we call the View() helper method; and we are re-using the DinnerFormViewModel class for both Edit and Create action scenarios.</span></span>

<span data-ttu-id="c21f5-114">Verranno ora esaminate modi che sevice del "principio DRY" all'interno dei nostri modelli di vista per eliminare la duplicazione del codice sono anche.</span><span class="sxs-lookup"><span data-stu-id="c21f5-114">Let's now look at ways we can apply the "DRY Principle" within our view templates to eliminate code duplication there as well.</span></span>

### <a name="re-visiting-our-edit-and-create-view-templates"></a><span data-ttu-id="c21f5-115">Riesaminare la modifica e creazione di modelli di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="c21f5-115">Re-visiting our Edit and Create View Templates</span></span>

<span data-ttu-id="c21f5-116">Utilizziamo attualmente due modelli di visualizzazione diverse: "Edit" e "Tornarci": per visualizzare il form Dinner dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="c21f5-116">Currently we are using two different view templates – "Edit.aspx" and "Create.aspx" – to display our Dinner form UI.</span></span> <span data-ttu-id="c21f5-117">Un confronto visivo rapido di essi viene evidenziata la similitudine sono.</span><span class="sxs-lookup"><span data-stu-id="c21f5-117">A quick visual comparison of them highlights how similar they are.</span></span> <span data-ttu-id="c21f5-118">Ecco come appare il modulo di creazione:</span><span class="sxs-lookup"><span data-stu-id="c21f5-118">Below is what the create form looks like:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

<span data-ttu-id="c21f5-119">Ed ecco come appare il modulo "Modifica":</span><span class="sxs-lookup"><span data-stu-id="c21f5-119">And here is what our "Edit" form looks like:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

<span data-ttu-id="c21f5-120">Non molto una differenza significativa è presente?</span><span class="sxs-lookup"><span data-stu-id="c21f5-120">Not much of a difference is there?</span></span> <span data-ttu-id="c21f5-121">Diversi dal testo del titolo e l'intestazione, i controlli di layout e l'input del form sono identici.</span><span class="sxs-lookup"><span data-stu-id="c21f5-121">Other than the title and header text, the form layout and input controls are identical.</span></span>

<span data-ttu-id="c21f5-122">Se si apre il "Edit" e "Tornarci" modelli di vista, che si noterà che contengono codice di controllo di layout e l'input identici form.</span><span class="sxs-lookup"><span data-stu-id="c21f5-122">If we open up the "Edit.aspx" and "Create.aspx" view templates we'll find that they contain identical form layout and input control code.</span></span> <span data-ttu-id="c21f5-123">Questa duplicazione significa che si finisce la necessità di apportare modifiche due volte ogni volta che si introducono o modificare una proprietà di Dinner nuovo - che non è valida.</span><span class="sxs-lookup"><span data-stu-id="c21f5-123">This duplication means we end up having to make changes twice anytime we introduce or change a new Dinner property - which is not good.</span></span>

### <a name="using-partial-view-templates"></a><span data-ttu-id="c21f5-124">Usando i modelli di visualizzazione parziale</span><span class="sxs-lookup"><span data-stu-id="c21f5-124">Using Partial View Templates</span></span>

<span data-ttu-id="c21f5-125">ASP.NET MVC supporta la possibilità di definire i modelli di "visualizzazione parziale" che possono essere usati per incapsulare la logica per il rendering di visualizzazione per una parte di una pagina secondaria.</span><span class="sxs-lookup"><span data-stu-id="c21f5-125">ASP.NET MVC supports the ability to define "partial view" templates that can be used to encapsulate view rendering logic for a sub-portion of a page.</span></span> <span data-ttu-id="c21f5-126">"Righe parzialmente eseguite" forniscono un modo utile per definire la logica di visualizzazione per il rendering di una volta e quindi riutilizzare in più posizioni in un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c21f5-126">"Partials" provide a useful way to define view rendering logic once, and then re-use it in multiple places across an application.</span></span>

<span data-ttu-id="c21f5-127">Per "DRY-up" nostro Edit. aspx e tornarci la duplicazione del modello di visualizzazione, è possibile creare un modello di visualizzazione parziale denominato "DinnerForm.ascx" che incapsula il layout del form e comune a entrambi gli elementi di input.</span><span class="sxs-lookup"><span data-stu-id="c21f5-127">To help "DRY-up" our Edit.aspx and Create.aspx view template duplication, we can create a partial view template named "DinnerForm.ascx" that encapsulates the form layout and input elements common to both.</span></span> <span data-ttu-id="c21f5-128">È possibile eseguire questa operazione facendo clic sul nostro/viste o Dinners directory e scegliendo la "Add -&gt;Visualizza" comando di menu:</span><span class="sxs-lookup"><span data-stu-id="c21f5-128">We'll do this by right-clicking on our /Views/Dinners directory and choosing the "Add-&gt;View" menu command:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

<span data-ttu-id="c21f5-129">Verrà visualizzata la finestra di dialogo "Aggiungi visualizzazione".</span><span class="sxs-lookup"><span data-stu-id="c21f5-129">This will display the "Add View" dialog.</span></span> <span data-ttu-id="c21f5-130">La nuova vista si desidera creare "DinnerForm", selezionare la casella di controllo "Crea una visualizzazione parziale" all'interno della finestra di dialogo e indicare che viene passata è una classe DinnerFormViewModel denominiamo:</span><span class="sxs-lookup"><span data-stu-id="c21f5-130">We'll name the new view we want to create "DinnerForm", select the "Create a partial view" checkbox within the dialog, and indicate that we will pass it a DinnerFormViewModel class:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

<span data-ttu-id="c21f5-131">Quando si fa clic sul pulsante "Aggiungi", Visual Studio creerà un nuovo modello di visualizzazione "DinnerForm.ascx" per noi all'interno della directory "\Views\Dinners".</span><span class="sxs-lookup"><span data-stu-id="c21f5-131">When we click the "Add" button, Visual Studio will create a new "DinnerForm.ascx" view template for us within the "\Views\Dinners" directory.</span></span>

<span data-ttu-id="c21f5-132">È possibile quindi copiare e incollare il layout del form duplicati / input di codice di controllo dai nostri modelli di vista Edit.aspx/ tornarci in nostro nuovo modello di visualizzazione parziale "DinnerForm.ascx":</span><span class="sxs-lookup"><span data-stu-id="c21f5-132">We can then copy/paste the duplicate form layout / input control code from our Edit.aspx/ Create.aspx view templates into our new "DinnerForm.ascx" partial view template:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

<span data-ttu-id="c21f5-133">È quindi possibile aggiornare i modelli di visualizzazione creazione e modifica per chiamare il modello parziale DinnerForm ed eliminare la duplicazione di form.</span><span class="sxs-lookup"><span data-stu-id="c21f5-133">We can then update our Edit and Create view templates to call the DinnerForm partial template and eliminate the form duplication.</span></span> <span data-ttu-id="c21f5-134">È possibile eseguire questa operazione Html.RenderPartial("DinnerForm") chiamante entro i nostri modelli di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="c21f5-134">We can do this by calling Html.RenderPartial("DinnerForm") within our view templates:</span></span>

##### <a name="createaspx"></a><span data-ttu-id="c21f5-135">Create.aspx</span><span class="sxs-lookup"><span data-stu-id="c21f5-135">Create.aspx</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a><span data-ttu-id="c21f5-136">Edit.aspx</span><span class="sxs-lookup"><span data-stu-id="c21f5-136">Edit.aspx</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

<span data-ttu-id="c21f5-137">È possibile qualificare in modo esplicito il percorso del modello parziale desiderata quando si chiama Html.RenderPartial (ad esempio: ~ Views/Dinners/DinnerForm.ascx ").</span><span class="sxs-lookup"><span data-stu-id="c21f5-137">You can explicitly qualify the path of the partial template you want when calling Html.RenderPartial (for example: ~Views/Dinners/DinnerForm.ascx").</span></span> <span data-ttu-id="c21f5-138">Nel codice precedente, tuttavia, stiamo sfruttando il modello di denominazione basata sulle convenzioni all'interno di ASP.NET MVC e specificare solo "DinnerForm" come nome di eseguire il rendering parziale.</span><span class="sxs-lookup"><span data-stu-id="c21f5-138">In our code above, though, we are taking advantage of the convention-based naming pattern within ASP.NET MVC, and just specifying "DinnerForm" as the name of the partial to render.</span></span> <span data-ttu-id="c21f5-139">Quando si esegue questa operazione, ASP.NET MVC cercherà prima nella directory delle visualizzazioni basato sulle convenzioni (DinnersController sarebbe/viste/Dinners).</span><span class="sxs-lookup"><span data-stu-id="c21f5-139">When we do this ASP.NET MVC will look first in the convention-based views directory (for DinnersController this would be /Views/Dinners).</span></span> <span data-ttu-id="c21f5-140">Se non viene trovato il modello parziale vi quindi cercherà lo nella directory /Views/Shared.</span><span class="sxs-lookup"><span data-stu-id="c21f5-140">If it doesn't find the partial template there it will then look for it in the /Views/Shared directory.</span></span>

<span data-ttu-id="c21f5-141">Quando viene chiamato Html.RenderPartial() utilizzando solo il nome della visualizzazione parziale, ASP.NET MVC passerà alla visualizzazione parziale gli oggetti di un dizionario ViewData e modello stesso utilizzati dal modello di vista del chiamante.</span><span class="sxs-lookup"><span data-stu-id="c21f5-141">When Html.RenderPartial() is called with just the name of the partial view, ASP.NET MVC will pass to the partial view the same Model and ViewData dictionary objects used by the calling view template.</span></span> <span data-ttu-id="c21f5-142">In alternativa, esistono versioni di overload di Html.RenderPartial() che consentono di passare un oggetto modello alternativo e/o il dizionario ViewData per la visualizzazione parziale da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="c21f5-142">Alternatively, there are overloaded versions of Html.RenderPartial() that enable you to pass an alternate Model object and/or ViewData dictionary for the partial view to use.</span></span> <span data-ttu-id="c21f5-143">Ciò è utile per scenari in cui si desidera solo passare un sottoinsieme di modello/ViewModel completo.</span><span class="sxs-lookup"><span data-stu-id="c21f5-143">This is useful for scenarios where you only want to pass a subset of the full Model/ViewModel.</span></span>

| <span data-ttu-id="c21f5-144">**Sul lato dell'argomento: Il motivo per cui &lt;% %&gt; invece di &lt;% = %&gt;?**</span><span class="sxs-lookup"><span data-stu-id="c21f5-144">**Side Topic: Why &lt;% %&gt; instead of &lt;%= %&gt;?**</span></span> |
| --- |
| <span data-ttu-id="c21f5-145">Uno degli aspetti meno evidenti si potrebbe aver notato con il codice precedente è che si sta usando un &lt;% %&gt; block invece di un &lt;% = %&gt; bloccare quando si chiama Html.RenderPartial().</span><span class="sxs-lookup"><span data-stu-id="c21f5-145">One of the subtle things you might have noticed with the code above is that we are using a &lt;% %&gt; block instead of a &lt;%= %&gt; block when calling Html.RenderPartial().</span></span> <span data-ttu-id="c21f5-146">&lt;% = %&gt; blocchi in ASP.NET indicano che uno sviluppatore desidera eseguire il rendering di un valore specificato (ad esempio: &lt;% = "Hello" %&gt; consentirà di visualizzare "Hello").</span><span class="sxs-lookup"><span data-stu-id="c21f5-146">&lt;%= %&gt; blocks in ASP.NET indicate that a developer wants to render a specified value (for example: &lt;%= "Hello" %&gt; would render "Hello").</span></span> <span data-ttu-id="c21f5-147">&lt;% %&gt; blocchi indicano invece che lo sviluppatore desidera eseguire il codice e che qualsiasi output sottoposto a rendering all'interno di essi deve essere eseguita in modo esplicito (ad esempio: &lt;Response.Write("Hello") %&gt;.</span><span class="sxs-lookup"><span data-stu-id="c21f5-147">&lt;% %&gt; blocks instead indicate that the developer wants to execute code, and that any rendered output within them must be done explicitly (for example: &lt;% Response.Write("Hello") %&gt;.</span></span> <span data-ttu-id="c21f5-148">Il motivo si sta usando un &lt;% %&gt; blocco con il nostro codice Html.RenderPartial sopra è perché il metodo Html.RenderPartial() non restituisce una stringa e restituisce invece il contenuto direttamente con il modello di vista chiamante del flusso di output.</span><span class="sxs-lookup"><span data-stu-id="c21f5-148">The reason we are using a &lt;% %&gt; block with our Html.RenderPartial code above is because the Html.RenderPartial() method doesn't return a string, and instead outputs the content directly to the calling view template's output stream.</span></span> <span data-ttu-id="c21f5-149">Ciò avviene per motivi di efficienza di prestazioni e in tal modo evita la necessità di creare un oggetto stringa temporaneo (potenzialmente molto grande).</span><span class="sxs-lookup"><span data-stu-id="c21f5-149">It does this for performance efficiency reasons, and by doing so it avoids the need to create a (potentially very large) temporary string object.</span></span> <span data-ttu-id="c21f5-150">Questo riduce l'utilizzo di memoria e migliorare la velocità effettiva complessiva dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c21f5-150">This reduces memory usage and improves overall application throughput.</span></span> <span data-ttu-id="c21f5-151">Un errore comune quando si utilizza Html.RenderPartial() è omesso di aggiungere un punto e virgola alla fine della chiamata quando è all'interno di un &lt;% %&gt; blocco.</span><span class="sxs-lookup"><span data-stu-id="c21f5-151">One common mistake when using Html.RenderPartial() is to forget to add a semi-colon at the end of the call when it is within a &lt;% %&gt; block.</span></span> <span data-ttu-id="c21f5-152">Ad esempio, questo codice causa un errore del compilatore: &lt;% Html.RenderPartial("DinnerForm")&gt; è invece necessario scrivere: &lt;Html.RenderPartial("DinnerForm") %; %&gt; infatti &lt;% %&gt; blocchi sono istruzioni di codice indipendente e quando si usa C# istruzioni di codice necessario terminare con un punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="c21f5-152">For example, this code will cause a compiler error: &lt;% Html.RenderPartial("DinnerForm") %&gt; You instead need to write: &lt;% Html.RenderPartial("DinnerForm"); %&gt; This is because &lt;% %&gt; blocks are self-contained code statements, and when using C# code statements need to be terminated with a semi-colon.</span></span> |

### <a name="using-partial-view-templates-to-clarify-code"></a><span data-ttu-id="c21f5-153">Usando i modelli di visualizzazione parziale per chiarire codice</span><span class="sxs-lookup"><span data-stu-id="c21f5-153">Using Partial View Templates to Clarify Code</span></span>

<span data-ttu-id="c21f5-154">È stato creato il modello di visualizzazione parziale "DinnerForm" per evitare di duplicare la logica di visualizzazione per il rendering in più posizioni.</span><span class="sxs-lookup"><span data-stu-id="c21f5-154">We created the "DinnerForm" partial view template to avoid duplicating view rendering logic in multiple places.</span></span> <span data-ttu-id="c21f5-155">Questo è il motivo più comune per creare modelli di visualizzazione parziale.</span><span class="sxs-lookup"><span data-stu-id="c21f5-155">This is the most common reason to create partial view templates.</span></span>

<span data-ttu-id="c21f5-156">In alcuni casi è comunque opportuno per creare le visualizzazioni parziali, anche quando vengono chiamati solo in un'unica posizione.</span><span class="sxs-lookup"><span data-stu-id="c21f5-156">Sometimes it still makes sense to create partial views even when they are only being called in a single place.</span></span> <span data-ttu-id="c21f5-157">I modelli di vista molto complicata spesso possono diventare molto più semplici da leggere quando la logica di visualizzazione per il rendering verrà estratti e partizionata in uno o più anche denominata templates parziale.</span><span class="sxs-lookup"><span data-stu-id="c21f5-157">Very complicated view templates can often become much easier to read when their view rendering logic is extracted and partitioned into one or more well named partial templates.</span></span>

<span data-ttu-id="c21f5-158">Ad esempio, si consideri il seguente frammento di codice dal file Site. master nel nostro progetto (che esamineremo a breve).</span><span class="sxs-lookup"><span data-stu-id="c21f5-158">For example, consider the below code-snippet from the Site.master file in our project (which we will be looking at shortly).</span></span> <span data-ttu-id="c21f5-159">Il codice è relativamente semplice da leggere, in parte perché la logica per la visualizzazione di un'account di accesso/disconnessione collegamento nella parte superiore destra della schermata viene incapsulata all'interno di "LogOnUserControl" parziale:</span><span class="sxs-lookup"><span data-stu-id="c21f5-159">The code is relatively straight-forward to read – partly because the logic to display a login/logout link at the top right of the screen is encapsulated within the "LogOnUserControl" partial:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

<span data-ttu-id="c21f5-160">Ogni volta che ci si ritroverà a confusione cercando di capire il markup html/codice all'interno di un modello di vista, prendere in considerazione se non sarebbe più chiaro se alcune di esse è stato estratto e sottoposta a refactoring in visualizzazioni parziali ben identificabili.</span><span class="sxs-lookup"><span data-stu-id="c21f5-160">Whenever you find yourself getting confused trying to understand the html/code markup within a view-template, consider whether it wouldn't be clearer if some of it was extracted and refactored into well-named partial views.</span></span>

### <a name="master-pages"></a><span data-ttu-id="c21f5-161">Pagine master</span><span class="sxs-lookup"><span data-stu-id="c21f5-161">Master Pages</span></span>

<span data-ttu-id="c21f5-162">Oltre a supportare le visualizzazioni parziali, ASP.NET MVC supporta inoltre la possibilità di creare modelli "pagina master" che possono essere usati per definire il layout comuni e html principale di un sito.</span><span class="sxs-lookup"><span data-stu-id="c21f5-162">In addition to supporting partial views, ASP.NET MVC also supports the ability to create "master page" templates that can be used to define the common layout and top-level html of a site.</span></span> <span data-ttu-id="c21f5-163">I controlli possono quindi essere aggiunti alla pagina master per identificare aree sostituibili che possono essere sottoposto a override o "compilate" viste segnaposto di contenuto.</span><span class="sxs-lookup"><span data-stu-id="c21f5-163">Content placeholder controls can then be added to the master page to identify replaceable regions that can be overridden or "filled in" by views.</span></span> <span data-ttu-id="c21f5-164">Ciò fornisce un modo molto efficace (e DRY) per applicare un layout comune in un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c21f5-164">This provides a very effective (and DRY) way to apply a common layout across an application.</span></span>

<span data-ttu-id="c21f5-165">Per impostazione predefinita, i nuovi progetti ASP.NET MVC hanno un modello di pagina master a essi aggiunti automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c21f5-165">By default, new ASP.NET MVC projects have a master page template automatically added to them.</span></span> <span data-ttu-id="c21f5-166">Questa pagina master è denominata "Site" e si trova all'interno della cartella \Views\Shared\:</span><span class="sxs-lookup"><span data-stu-id="c21f5-166">This master page is named "Site.master" and lives within the \Views\Shared\ folder:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

<span data-ttu-id="c21f5-167">Il file Site. master predefinito è la seguente.</span><span class="sxs-lookup"><span data-stu-id="c21f5-167">The default Site.master file looks like below.</span></span> <span data-ttu-id="c21f5-168">Definisce il codice html esterno del sito, oltre a un menu per la navigazione nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="c21f5-168">It defines the outer html of the site, along with a menu for navigation at the top.</span></span> <span data-ttu-id="c21f5-169">Contiene due controlli sostituibili segnaposto di contenuto: uno per il titolo e l'altro per la quale il principale contenuto di una pagina deve essere sostituito:</span><span class="sxs-lookup"><span data-stu-id="c21f5-169">It contains two replaceable content placeholder controls – one for the title, and the other for where the primary content of a page should be replaced:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

<span data-ttu-id="c21f5-170">Tutti i modelli di vista, che abbiamo creato per la nostra applicazione NerdDinner ("List", "Dettagli", "Modifica", "Crea", "NotFound", e così via) sono basati su questo modello Site. master.</span><span class="sxs-lookup"><span data-stu-id="c21f5-170">All of the view templates we've created for our NerdDinner application ("List", "Details", "Edit", "Create", "NotFound", etc) have been based on this Site.master template.</span></span> <span data-ttu-id="c21f5-171">Questa caratteristica è indicata tramite l'attributo "MasterPageFile" che è stato aggiunto per impostazione predefinita nella parte superiore &lt;% @ % pagina&gt; direttiva quando abbiamo creato viste utilizzando la finestra di dialogo "Aggiungi visualizzazione":</span><span class="sxs-lookup"><span data-stu-id="c21f5-171">This is indicated via the "MasterPageFile" attribute that was added by default to the top &lt;% @ Page %&gt; directive when we created our views using the "Add View" dialog:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

<span data-ttu-id="c21f5-172">Ciò significa che è possibile modificare il contenuto Site. master, e hanno le modifiche automaticamente essere applicate e usate quando si esegue il rendering di uno dei nostri modelli di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c21f5-172">What this means is that we can change the Site.master content, and have the changes automatically be applied and used when we render any of our view templates.</span></span>

<span data-ttu-id="c21f5-173">Aggiorniamo la sezione di intestazione del nostro Site. master, in modo che l'intestazione dell'applicazione è "NerdDinner" anziché "My MVC Application".</span><span class="sxs-lookup"><span data-stu-id="c21f5-173">Let's update our Site.master's header section so that the header of our application is "NerdDinner" instead of "My MVC Application".</span></span> <span data-ttu-id="c21f5-174">È possibile anche aggiornare il menu di navigazione in modo che nella prima scheda è "Trovare un Dinner" (gestito dal metodo di azione Index () di HomeController) e aggiungiamo una nuova scheda denominata "Host a Dinner" (gestito dal metodo di azione del DinnersController create ()):</span><span class="sxs-lookup"><span data-stu-id="c21f5-174">Let's also update our navigation menu so that the first tab is "Find a Dinner" (handled by the HomeController's Index() action method), and let's add a new tab called "Host a Dinner" (handled by the DinnersController's Create() action method):</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

<span data-ttu-id="c21f5-175">Quando si salva il file Site. master e aggiornare il browser si vedrà l'intestazione modifiche vengano visualizzate in tutte le visualizzazioni all'interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c21f5-175">When we save the Site.master file and refresh our browser we'll see our header changes show up across all views within our application.</span></span> <span data-ttu-id="c21f5-176">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c21f5-176">For example:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

<span data-ttu-id="c21f5-177">E con il */Dinners/modifica / [id]* URL:</span><span class="sxs-lookup"><span data-stu-id="c21f5-177">And with the */Dinners/Edit/[id]* URL:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a><span data-ttu-id="c21f5-178">Passo successivo</span><span class="sxs-lookup"><span data-stu-id="c21f5-178">Next Step</span></span>

<span data-ttu-id="c21f5-179">Righe parzialmente eseguite e le pagine master forniscono opzioni molto flessibili che consentono di organizzare normalmente le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="c21f5-179">Partials and master pages provide very flexible options that enable you to cleanly organize views.</span></span> <span data-ttu-id="c21f5-180">Troverai che consentono di evitare la duplicazione di visualizzazione del contenuto / codice e rendere più semplice da leggere e gestire i modelli di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c21f5-180">You'll find that they help you avoid duplicating view content/ code, and make your view templates easier to read and maintain.</span></span>

<span data-ttu-id="c21f5-181">È possibile a questo punto rivedere lo scenario di elenco che è stato creato in precedenza e abilitare il supporto di paging scalabile.</span><span class="sxs-lookup"><span data-stu-id="c21f5-181">Let's now revisit the listing scenario we built earlier and enable scalable paging support.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c21f5-182">[Precedente](use-viewdata-and-implement-viewmodel-classes.md)
> [Successivo](implement-efficient-data-paging.md)</span><span class="sxs-lookup"><span data-stu-id="c21f5-182">[Previous](use-viewdata-and-implement-viewmodel-classes.md)
[Next](implement-efficient-data-paging.md)</span></span>