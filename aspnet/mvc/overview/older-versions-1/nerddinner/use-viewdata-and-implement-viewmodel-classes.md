---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Utilizzare ViewData e implementare le classi ViewModel . Documenti Microsoft
author: rick-anderson
description: Passaggio 6 viene illustrato come abilitare il supporto per scenari di modifica dei moduli più avanzati e vengono illustrati anche due approcci che possono essere utilizzati per passare i dati dai controller alle visualizzazioni:...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 7fa2af2a55d12bbe11b29dff594823a1e5ea0152
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541104"
---
# <a name="use-viewdata-and-implement-viewmodel-classes"></a>Usare ViewData e implementare classi ViewModel

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 6 di un'esercitazione gratuita [sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come compilare un'applicazione Web piccola ma completa usando ASP.NET MVC 1.
> 
> Passaggio 6 viene illustrato come abilitare il supporto per scenari di modifica di moduli più avanzati e vengono inoltre illustrati due approcci che possono essere utilizzati per passare i dati dai controller alle visualizzazioni: ViewData e ViewModel.Step 6 shows how enable support for richer form editing scenarios, and also discusses two approaches that can be used to pass data from controllers to views: ViewData and ViewModel.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [ALL'archivio musicale MVC.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>Passaggio 6 di NerdDinnerNerdDinner Step 6: ViewData and ViewModel

Abbiamo illustrato una serie di scenari di post del modulo e discusso come implementare il supporto di creazione, aggiornamento ed eliminazione (CRUD). Ora prenderemo la nostra implementazione DinnersController ulteriormente e abilitare il supporto per scenari di modifica dei form più ricchi. In questo modo verranno illustrati due approcci che possono essere utilizzati per passare i dati dai controller alle visualizzazioni: ViewData e ViewModel.While doing this we'll discuss two approaches that can be used to pass data from controllers to views: ViewData and ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Passaggio di dati dai controller ai modelli di visualizzazionePassing Data from Controllers to View-Templates

Una delle caratteristiche di definizione del modello MVC è la rigorosa "separazione dei problemi" che consente di applicare tra i diversi componenti di un'applicazione. Modelli, controller e viste hanno ciascuno ruoli e responsabilità ben definiti e comunicano tra loro in modi ben definiti. Ciò consente di promuovere la testabilità e il riutilizzo del codice.

Quando una classe Controller decide di eseguire il rendering di una risposta HTML a un client, è responsabile del passaggio esplicito al modello di visualizzazione di tutti i dati necessari per eseguire il rendering della risposta. I modelli di visualizzazione non devono mai eseguire il recupero dei dati o la logica dell'applicazione e devono invece limitarsi a disporre solo del codice di rendering che viene determinato dal modello o dai dati passati dal controller.

Al momento i dati del modello passati dalla nostra classe DinnersController ai nostri modelli di visualizzazione sono semplici e semplici – un elenco di oggetti Dinner nel caso di Index() e un singolo oggetto Dinner nel caso di Details(), Edit(), Create() e Delete(). Man mano che aggiungiamo altre funzionalità dell'interfaccia utente all'applicazione, spesso è necessario passare più di questi dati per eseguire il rendering delle risposte HTML all'interno dei modelli di visualizzazione. Ad esempio, potremmo voler cambiare il campo "Paese" all'interno delle nostre visualizzazioni Modifica e Crea da casella di testo HTML a elenco a discesa. Anziché impostare come hardcoded l'elenco a discesa dei nomi dei paesi nel modello di visualizzazione, è possibile generarlo da un elenco di paesi supportati popolati in modo dinamico. Avremo bisogno di un modo per passare sia l'oggetto Dinner *che* l'elenco dei paesi supportati dal controller ai nostri modelli di visualizzazione.

Diamo un'occhiata a due modi in cui possiamo raggiungere questo obiettivo.

### <a name="using-the-viewdata-dictionary"></a>Utilizzo del dizionario ViewDataUsing the ViewData Dictionary

La classe di base Controller espone una proprietà del dizionario "ViewData" che può essere utilizzata per passare elementi di dati aggiuntivi dai controller alle visualizzazioni.

Ad esempio, per supportare lo scenario in cui vogliamo modificare la casella di testo "Paese" all'interno della nostra visualizzazione di modifica da una casella di testo HTML in un elenco a discesa, possiamo aggiornare il nostro metodo di azione Edit() per passare (oltre a un oggetto Dinner) un oggetto SelectList che può essere utilizzato come modello di un elenco a discesa di paesi.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

Il costruttore di SelectList precedente è accettare un elenco di contee con cui popolare l'elenco a discesa, nonché il valore attualmente selezionato.

È quindi possibile aggiornare il modello di visualizzazione Edit.aspx per utilizzare il metodo helper Html.DropDownList() anziché il metodo helper Html.TextBox() usato in precedenza:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

Il metodo helper Html.DropDownList() precedente accetta due parametri. Il primo è il nome dell'elemento modulo HTML da restituire. Il secondo è il modello "SelectList" passato tramite il dizionario ViewData. Si sta usando la parola chiave "as" di Cper eseguire il cast del tipo all'interno del dizionario come SelectList.

E ora quando eseguiamo la nostra applicazione e accedere l'URL */Dinners/Edit/1* all'interno del nostro browser vedremo che la nostra interfaccia utente di modifica è stata aggiornata per visualizzare un elenco a discesa di paesi invece di una casella di testo:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Poiché viene eseguito anche il rendering del modello di visualizzazione Edit dal metodo HTTP-POST Edit (negli scenari in cui si verificano errori), è consigliabile assicurarsi di aggiornare anche questo metodo per aggiungere il SelectList a ViewData quando viene eseguito il rendering del modello di visualizzazione in scenari di errore:Because we also render the Edit view template from the HTTP-POST Edit method (in scenarios when errors occur), we'll want to make sure that we also update this method to add the SelectList to ViewData when the view template is rendered in error scenarios:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

E ora il nostro scenario di modifica DinnersController supporta un controllo DropDownList.

### <a name="using-a-viewmodel-pattern"></a>Utilizzo di un modello ViewModelUsing a ViewModel Pattern

L'approccio del dizionario ViewData ha il vantaggio di essere abbastanza veloce e facile da implementare. Ad alcuni sviluppatori non piace usare dizionari basati su stringa, tuttavia, poiché gli errori di battitura possono causare errori che non verranno rilevati in fase di compilazione. Il dizionario ViewData non tipizzato richiede anche l'utilizzo dell'operatore "as" o il cast quando si usa un linguaggio fortemente tipizzato come C , in un modello di visualizzazione.

Un approccio alternativo che è possibile usare è spesso indicato come il modello "ViewModel". Quando si usa questo modello vengono create classi fortemente tipizzate ottimizzate per gli scenari di visualizzazione specifici e che espongono proprietà per i valori dinamici/contenuti necessari per i modelli di visualizzazione. Le classi controller possono quindi popolare e passare queste classi ottimizzate per la visualizzazione al modello di visualizzazione da usare. In questo modo è possibile abilitare l'indipendenza dai tipi, il controllo in fase di compilazione e l'utilizzo di Intellisense dell'editor all'interno dei modelli di visualizzazione.

Ad esempio, per abilitare gli scenari di modifica del form cena è possibile creare una classe "DinnerFormViewModel" come di seguito che espone due proprietà fortemente tipizzate: un Dinner oggetto e il SelectList modello necessario per popolare l'elenco a discesa paesi:For example, to enable dinner form editing scenarios we can create a "DinnerFormViewModel" class like below that exposes two strongly-typed properties: a Dinner object, and the SelectList model needed to populate the countries dropdownlist:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

È quindi possibile aggiornare il metodo di azione Edit() per creare DinnerFormViewModel usando l'oggetto Dinner che recuperiamo dal repository e quindi passarlo al modello di visualizzazione:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Aggiorneremo quindi il modello di visualizzazione in modo che si aspetti un oggetto "DinnerFormViewModel" anziché un oggetto "Dinner" modificando l'attributo "inherits" nella parte superiore della pagina edit.aspx in questo modo:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Una volta fatto questo, il punto di vista della proprietà "Model" all'interno del modello di visualizzazione verrà aggiornato per riflettere il modello a oggetti del tipo DinnerFormViewModel che stiamo passando:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

È quindi possibile aggiornare il codice di visualizzazione per lavorare fuori di esso. Si noti come non stiamo modificando i nomi degli elementi di input che stiamo creando (gli elementi del modulo saranno ancora denominati "Title", "Country") – ma stiamo aggiornando i metodi html Helper per recuperare i valori utilizzando la classe DinnerFormViewModel:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Aggiorneremo anche il nostro metodo Edit post per usare la classe DinnerFormViewModel durante il rendering degli errori:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Possiamo anche aggiornare i nostri metodi di azione Create() per riutilizzare la stessa classe *DinnerFormViewModel* per abilitare i paesi DropDownList anche all'interno di questi. Di seguito è riportata l'implementazione HTTP-GET:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Di seguito è riportata l'implementazione del metodo HTTP-POST Create:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

E ora entrambe le schermate Modifica e Crea supportano gli elenchi a discesa per la selezione del paese.

### <a name="custom-shaped-viewmodel-classes"></a>Classi ViewModel a forma personalizzataCustom-shaped ViewModel classes

Nello scenario precedente, la nostra classe DinnerFormViewModel espone direttamente l'oggetto modello Dinner come proprietà, insieme a una proprietà del modello SelectList di supporto. Questo approccio funziona correttamente per gli scenari in cui l'interfaccia utente HTML che si desidera creare all'interno del modello di visualizzazione corrisponde relativamente ai nostri oggetti del modello di dominio.

Per gli scenari in cui questo non è il caso, un'opzione che è possibile utilizzare è quella di creare una classe ViewModel a forma personalizzata il cui modello a oggetti è più ottimizzato per l'utilizzo da parte della visualizzazione e che potrebbe apparire completamente diverso dall'oggetto modello di dominio sottostante. Ad esempio, potrebbe potenzialmente esporre nomi di proprietà e/o proprietà di aggregazione diverse raccolte da più oggetti modello.

Le classi ViewModel di forma personalizzata possono essere usate sia per passare i dati dai controller alle visualizzazioni per il rendering, sia per gestire i dati del modulo sottoposti a postback al metodo di azione di un controller. Per questo scenario successivo, è possibile fare in modo che il metodo di azione aggiorni un oggetto ViewModel con i dati inviati dal modulo e quindi utilizzi l'istanza ViewModel per eseguire il mapping o recuperare un oggetto modello di dominio effettivo.

Le classi ViewModel a forma personalizzata possono fornire una grande flessibilità e sono qualcosa da analizzare ogni volta che si trova il codice di rendering all'interno dei modelli di visualizzazione o il codice di registrazione dei moduli all'interno dei metodi di azione iniziando a diventare troppo complicato. Questo è spesso un segno che i modelli di dominio non corrispondono in modo pulito all'interfaccia utente che si sta generando e che una classe ViewModel a forma personalizzata intermedia può essere utile.

### <a name="next-step"></a>passaggio successivo

Esaminiamo ora come è possibile usare i partial e le pagine master per riutilizzare e condividere l'interfaccia utente nell'applicazione.

> [!div class="step-by-step"]
> [Successivo](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [precedente](re-use-ui-using-master-pages-and-partials.md)
