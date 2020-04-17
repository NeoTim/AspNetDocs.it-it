---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Riutilizzare l'interfaccia utente utilizzando pagine master e parziali . Documenti Microsoft
author: rick-anderson
description: Passaggio 7 esamina i modi in cui è possibile applicare il 'Principio DRY' all'interno dei modelli di visualizzazione per eliminare la duplicazione del codice, utilizzando modelli di visualizzazione parziale e pagine master.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: f381c4424a9fa0718cd234beeb01ce41bc4ca61e
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542599"
---
# <a name="re-use-ui-using-master-pages-and-partials"></a>Riutilizzare l'interfaccia utente tramite pagine master e visualizzazioni parziali

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 7 di un'esercitazione gratuita [sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come compilare un'applicazione Web piccola ma completa usando ASP.NET MVC 1.
> 
> Passaggio 7 esamina i modi in cui è possibile applicare il "principio DRY" all'interno dei modelli di visualizzazione per eliminare la duplicazione del codice, utilizzando modelli di visualizzazione parziale e pagine master.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [ALL'archivio musicale MVC.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-7-partials-and-master-pages"></a>Passaggio 7 di NerdDinner: Parziali e pagine masterNerdDinner Step 7: Partials and Master Pages

Una delle filosofie di progettazione ASP.NET MVC abbraccia è il principio "Do Not Repeat Yourself" (comunemente indicato come "DRY"). Una progettazione DRY consente di eliminare la duplicazione di codice e logica, che in ultima analisi rende le applicazioni più veloci da creare e più facili da gestire.

Abbiamo già visto il principio DRY applicato in molti dei nostri scenari NerdDinner. Alcuni esempi: la logica di convalida viene implementata all'interno del livello del modello, che ne consente l'applicazione in entrambi gli scenari di modifica e creazione nel controller. stiamo riutilizzando il modello di visualizzazione "NotFound" tra i metodi di azione Modifica, Dettagli ed Elimina; stiamo usando un modello di denominazione convenzione con i nostri modelli di visualizzazione, che elimina la necessità di specificare esplicitamente il nome quando chiamiamo il metodo helper View(); e stiamo riutilizzando la classe DinnerFormViewModel per entrambi gli scenari di azione Edit e Create.

Esaminiamo ora i modi in cui è possibile applicare il "principio DRY" all'interno dei nostri modelli di visualizzazione per eliminare la duplicazione del codice anche lì.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Rivedere i nostri modelli di modifica e creazione di viste

Attualmente si utilizzano due diversi modelli di visualizzazione, "Edit.aspx" e "Create.aspx", per visualizzare l'interfaccia utente del modulo Dinner. Un rapido confronto visivo di loro mette in evidenza quanto siano simili. Di seguito è riportato l'aspetto del modulo di creazione:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

Ed ecco come appare il nostro modulo "Modifica":

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Non c'è molta differenza? Oltre al testo del titolo e dell'intestazione, il layout del modulo e i controlli di input sono identici.

Se apriamo i modelli di visualizzazione "Edit.aspx" e "Create.aspx", scopriremo che contengono il layout del form e il codice del controllo di input identici. Questa duplicazione significa che finiamo per dover apportare modifiche due volte ogni volta che introduciamo o cambiamo una nuova proprietà Dinner - che non è buona.

### <a name="using-partial-view-templates"></a>Utilizzo di modelli di vista parziale

ASP.NET MVC supporta la possibilità di definire modelli di "visualizzazione parziale" che possono essere utilizzati per incapsulare la logica di rendering della visualizzazione per una parte secondaria di una pagina. "Parziali" forniscono un modo utile per definire la logica di rendering della visualizzazione una sola volta e quindi riutilizzarla in più posizioni in un'applicazione.

Per consentire a "DRY-up" la duplicazione del modello di visualizzazione Edit.aspx e Create.aspx, è possibile creare un modello di visualizzazione parziale denominato "DinnerForm.ascx" che incapsula il layout del modulo e gli elementi di input comuni a entrambi. Lo faremo facendo clic con il pulsante destro del mouse sulla&gt;nostra directory /Views/Dinners e scegliendo il comando di menu "Aggiungi- Visualizza":

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Verrà visualizzata la finestra di dialogo "Aggiungi vista". Il nome della nuova visualizzazione verrà creata "DinnerForm", selezionare la casella di controllo "Crea una visualizzazione parziale" all'interno della finestra di dialogo e indicare che passeremo una classe DinnerFormViewModel:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio creerà un nuovo modello di visualizzazione "DinnerForm.ascx" per noi all'interno della directory "Views".

È quindi possibile copiare/incollare il layout del modulo duplicato / il codice del controllo di input dai modelli di visualizzazione Edit.aspx/ Create.aspx nel nuovo modello di visualizzazione parziale "DinnerForm.ascx":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

È quindi possibile aggiornare i modelli di visualizzazione Di modifica e creazione per chiamare il modello parziale DinnerForm ed eliminare la duplicazione del modulo. A tale scopo, è possibile chiamare Html.RenderPartial("DinnerForm") all'interno dei modelli di visualizzazione:We can do this by calling Html.RenderPartial("DinnerForm") within our view templates:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx (informazioni in inglese)

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

È possibile qualificare in modo esplicito il percorso del modello parziale che si desidera quando si chiama Html.RenderPartial (ad esempio: "Views/Dinners/DinnerForm.ascx"). Nel codice precedente, tuttavia, stiamo sfruttando il modello di denominazione basato su convenzione all'interno di ASP.NET MVC e specificando semplicemente "DinnerForm" come nome del partial di cui eseguire il rendering. Quando si esegue questa operazione ASP.NET MVC cercherà prima nella directory di visualizzazioni basate su convenzione (per DinnersController questo sarebbe /Views/Dinners). Se non trova il modello parziale, verrà quindi cercata nella directory /Views/Shared.

Quando Html.RenderPartial() viene chiamato con il solo nome della visualizzazione parziale, ASP.NET MVC passerà alla visualizzazione parziale gli stessi oggetti dizionario Model e ViewData utilizzati dal modello di visualizzazione chiamante. In alternativa, esistono versioni di overload di Html.RenderPartial() che consentono di passare un oggetto Model alternativo e/o un dizionario ViewData per la visualizzazione parziale da utilizzare. Ciò è utile per gli scenari in cui si desidera passare solo un subset del modello/ViewModel completo.

| **Argomento &lt;laterale:&gt; Perché &lt;% invece di % ? ?&gt;** |
| --- |
| Una delle cose sottili che potreste aver notato con &lt;il&gt; codice sopra &lt;è che&gt; stiamo usando un % di blocco invece di un % % blocco quando si chiama Html.RenderPartial(). &lt;% %&gt; blocchi in ASP.NET indicano che uno sviluppatore &lt;desidera eseguire il&gt; rendering di un valore specificato (ad esempio: % : "Ciao" % eseguirebbe il rendering "Hello"). &lt;%&gt; blocchi indicano invece che lo sviluppatore desidera eseguire il codice e che &lt;qualsiasi output di cui&gt;è stato eseguito il rendering al loro interno deve essere eseguito in modo esplicito (ad esempio: % Response.Write("Hello") % . Il motivo per &lt;cui&gt; stiamo usando un blocco % con il codice Html.RenderPartial precedente è che il metodo Html.RenderPartial() non restituisce una stringa e restituisce invece il contenuto direttamente nel flusso di output del modello di visualizzazione chiamante. Questa operazione viene eseguito per motivi di efficienza delle prestazioni e in questo modo si evita la necessità di creare un oggetto stringa temporaneo (potenzialmente molto grande). Ciò riduce l'utilizzo della memoria e migliora la velocità effettiva complessiva dell'applicazione. Un errore comune quando si utilizza Html.RenderPartial() è dimenticare di aggiungere un punto &lt;e&gt; virgola alla fine della chiamata quando si trova all'interno di un% blocco. Ad esempio, questo codice causerà &lt;un errore del compilatore:&gt; % Html.RenderPartial("DinnerForm") % È invece necessario scrivere: &lt;% Html.RenderPartial("DinnerForm"); %&gt; Ciò &lt;è&gt; dovuto al fatto che % i blocchi sono istruzioni di codice indipendenti e quando si usano istruzioni di codice C . |

### <a name="using-partial-view-templates-to-clarify-code"></a>Utilizzo di modelli di vista parziale per chiarire il codiceUsing Partial View Templates to Clarify Code

È stato creato il modello di visualizzazione parziale "DinnerForm" per evitare la duplicazione della logica di rendering della visualizzazione in più posizioni. Questo è il motivo più comune per creare modelli di visualizzazione parziale.

A volte ha ancora senso creare viste parziali anche quando vengono chiamate solo in un'unica posizione. I modelli di visualizzazione molto complicati possono spesso diventare molto più facili da leggere quando la logica di rendering della visualizzazione viene estratta e partizionata in uno o più modelli parziali con nome sicuro.

Ad esempio, si consideri il frammento di codice seguente dal file Site.master nel progetto (che verrà esaminato a breve). Il codice è relativamente semplice per la lettura – in parte perché la logica per visualizzare un collegamento di login/logout nella parte superiore destra dello schermo è incapsulata all'interno del partial "LogOnUserControl":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Ogni volta che ti trovi confuso cercando di capire il markup html/code all'interno di un modello di visualizzazione, considerare se non sarebbe più chiaro se alcuni di essi è stato estratto e refactoring in visualizzazioni parziali ben denominate.

### <a name="master-pages"></a>Pagine master

Oltre a supportare le visualizzazioni parziali, ASP.NET MVC supporta anche la possibilità di creare modelli di "pagina master" che possono essere utilizzati per definire il layout comune e html di primo livello di un sito. I controlli segnaposto di contenuto possono quindi essere aggiunti alla pagina master per identificare le aree sostituibili che possono essere sostituite o "riempite" dalle visualizzazioni. Ciò fornisce un modo molto efficace (e DRY) per applicare un layout comune a un'applicazione.

Per impostazione predefinita, ai nuovi ASP.NET ai progetti MVC viene aggiunto automaticamente un modello di pagina master. Questa pagina master è denominata "Site.master" e si trova all'interno della cartella "Views" condivisa:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

Il file Site.master predefinito è simile a quello riportato di seguito. Definisce l'html esterno del sito, insieme a un menu per la navigazione nella parte superiore. Contiene due controlli segnaposto di contenuto sostituibili, uno per il titolo e l'altro per il punto in cui deve essere sostituito il contenuto principale di una pagina:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Tutti i modelli di visualizzazione che abbiamo creato per l'applicazione NerdDinner ("List", "Details", "Edit", "Create", "NotFound" e così via) sono stati basati su questo modello Site.master. Ciò è indicato tramite l'attributo "MasterPageFile" &lt;che è&gt; stato aggiunto per impostazione predefinita alla direttiva top % - % pagina quando abbiamo creato le visualizzazioni utilizzando la finestra di dialogo "Aggiungi visualizzazione":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Ciò significa che è possibile modificare il contenuto di Site.master e applicare e utilizzare automaticamente le modifiche quando viene eseguito il rendering di uno dei modelli di visualizzazione.

Aggiorniamo la sezione di intestazione di Site.master in modo che l'intestazione dell'applicazione sia "NerdDinner" anziché "My MVC Application". Aggiorniamo anche il menu di navigazione in modo che la prima scheda sia "Trova una cena" (gestita dal metodo di azione Index() di HomeController) e aggiungiamo una nuova scheda denominata "Host a Dinner" (gestita dal metodo di azione Create() di DinnersController):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Quando salviamo il file Site.master e aggiorniamo il browser vedremo le modifiche dell'intestazione visualizzate in tutte le visualizzazioni all'interno dell'applicazione. Ad esempio:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

E con l'URL */Dinners/Edit/[id]:*

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>passaggio successivo

Le pagine parziali e le pagine master offrono opzioni molto flessibili che consentono di organizzare correttamente le visualizzazioni. Troverete che aiutano a evitare la duplicazione del contenuto di visualizzazione / codice, e rendere i modelli di visualizzazione più facile da leggere e gestire.

Esaminiamo ora lo scenario di elenco creato in precedenza e abilitiamo il supporto di paging scalabile.

> [!div class="step-by-step"]
> [Successivo](use-viewdata-and-implement-viewmodel-classes.md)
> [precedente](implement-efficient-data-paging.md)
