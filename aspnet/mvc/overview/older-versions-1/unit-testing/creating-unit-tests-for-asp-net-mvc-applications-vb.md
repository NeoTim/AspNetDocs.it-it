---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: Creazione di Unit test per applicazioni ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Informazioni su come creare unit test per le azioni del controller. In questa esercitazione, Stephen Walther spiega come verificare se un'azione del controller restituisce un ParteI...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 47d42b8017837f15e0d56dfb3565257164c97bbe
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59421030"
---
# <a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a>Creazione di unit test per le applicazioni ASP.NET MVC (VB)

da [Stephen Walther](https://github.com/StephenWalther)

[Scarica il PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> Informazioni su come creare unit test per le azioni del controller. In questa esercitazione, Stephen Walther spiega come verificare se un'azione del controller restituisce una visualizzazione specifica, restituisce un set di dati specifico oppure un altro tipo di risultato dell'azione.


L'obiettivo di questa esercitazione è dimostrare come è possibile scrivere unit test per i controller in ASP.NET MVC applications. Illustreremo come creare tre tipi diversi di unit test. Descrive come testare la visualizzazione restituita da un'azione del controller, come i dati della visualizzazione restituito da un'azione del controller di test e come testare un'azione del controller si viene reindirizzati a una seconda azione del controller o meno.

## <a name="creating-the-controller-under-test"></a>Creazione del Controller sottoposta a Test

Iniziamo creando il controller che si vuole testare. Il controller, denominato il `ProductController`, è contenuta nel listato 1.

**Listato 1: `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

Il `ProductController` contiene due metodi di azione denominati `Index()` e `Details()`. Entrambi i metodi di azione restituiscono una visualizzazione. Si noti che il `Details()` azione accetta un parametro denominato ID.

## <a name="testing-the-view-returned-by-a-controller"></a>Test della visualizzazione restituita da un Controller

Si supponga che si vuole testare o meno il `ProductController` restituisce la visualizzazione a destra. È necessario assicurarsi che quando il `ProductController.Details()` azione viene richiamata, viene restituita la visualizzazione dei dettagli. La classe di test nel listato 2 contiene un unit test per testare la visualizzazione restituita dal `ProductController.Details()` azione.

**Listato 2: `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

La classe nel listato 2 include un metodo di test denominato `TestDetailsView()`. Questo metodo contiene tre righe di codice. La prima riga di codice crea una nuova istanza di `ProductController` classe. La seconda riga di codice richiama il controller `Details()` metodo di azione. Infine, l'ultima riga del codice controlla se la vista restituita dal `Details()` azione è la visualizzazione dei dettagli.

Il `ViewResult.ViewName` proprietà rappresenta il nome della visualizzazione restituita da un controller. Un avviso di big data sui test di questa proprietà. Esistono due modi che un controller può restituire una visualizzazione. Un controller può restituire in modo esplicito una visualizzazione simile al seguente:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

In alternativa, il nome della visualizzazione può essere dedotto dal nome dell'azione del controller simile al seguente:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

Questa azione del controller restituisce anche una visualizzazione denominata `Details`. Tuttavia, il nome della visualizzazione viene dedotto dal nome dell'azione. Se si desidera verificare il nome della visualizzazione, è necessario restituire in modo esplicito il nome della visualizzazione dall'azione del controller.

È possibile eseguire lo unit test nel listato 2 entrambi immettendo la combinazione di tasti **Ctrl-R, A** oppure fare clic il **eseguire tutti i test nella soluzione** pulsante (vedere la figura 1). Se il test ha esito positivo, si noterà nella finestra Risultati Test nella figura 2.


[![Rannullare tutti i test nella soluzione](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)

**Figura 01**: Eseguire tutti i test nella soluzione ([fare clic per visualizzare l'immagine con dimensioni normali](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))


[![Srrore!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)

**Figura 02**: Operazione completata ([Fare clic per visualizzare l'immagine con dimensioni normali](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>Verifica i dati della visualizzazione restituita da un Controller

Un controller MVC passa i dati a una visualizzazione usando un elemento denominato *`View Data`*. Ad esempio, si supponga che si desidera visualizzare i dettagli per un determinato prodotto quando si richiama il `ProductController Details()` azione. In tal caso, è possibile creare un'istanza di un `Product` classe (definite nel modello) e passare l'istanza per il `Details` visualizzazione, sfruttando i vantaggi di `View Data`.

Modificato `ProductController` listato 3 include una versione aggiornata `Details()` azione che restituisce un prodotto.

**Listato 3: `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

Prima di tutto, il `Details()` azione crea una nuova istanza del `Product` classe che rappresenta un computer portatile. Successivamente, l'istanza del `Product` classe viene passata come secondo parametro per il `View()` (metodo).

È possibile scrivere unit test per verificare se i dati previsti sono contenuta nella visualizzazione dei dati. Lo unit test nei test listato 4 o meno un prodotto che rappresenta un computer portatile viene restituito quando si chiama il `ProductController Details()` metodo di azione.

**Listato 4: `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

Nel listato 4, il `TestDetailsView()` metodo di verifica i dati della visualizzazione restituito richiamando il `Details()` (metodo). Il `ViewData` viene esposto come proprietà nel `ViewResult` restituito richiamando il `Details()` (metodo). Il `ViewData.Model` proprietà contiene il prodotto passato alla visualizzazione. Il test verifica semplicemente che il prodotto contenuto nei dati di visualizzazione ha il nome del computer portatile.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Test del risultato dell'azione restituito da un Controller

Un'azione del controller più complessa potrebbe restituire tipi diversi di risultati di azione a seconda dei valori dei parametri passati per l'azione del controller. Un'azione del controller può restituire un'ampia gamma di tipi di risultati delle azioni inclusi una `ViewResult`, `RedirectToRouteResult`, o `JsonResult`.

Ad esempio, modificato `Details()` azione nel listato 5 restituisce il `Details` visualizzare quando si passa l'Id del prodotto valida per l'azione. Se si passa un prodotto valido Id - Id con un valore minore di - 1, quindi si verrà reindirizzati al `Index()` azione.

**Listato 5: `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

È possibile testare il comportamento del `Details()` azione con lo unit test nel listato 6. Lo unit test nel listato 6 consente di verificare che si verrà reindirizzati al `Index` visualizzare quando viene passato un Id con il valore -1 per il `Details()` (metodo).

**Listato 6: `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

Quando si chiama il `RedirectToAction()` metodo in un'azione del controller, l'azione del controller restituisce un `RedirectToRouteResult`. I controlli di test se il `RedirectToRouteResult` reindirizzerà l'utente a un'azione del controller denominata `Index`.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato descritto come creare unit test per le azioni del controller MVC. In primo luogo, è stato descritto come verificare se la vista destra verrà restituita da un'azione del controller. Si è appreso come usare il `ViewResult.ViewName` proprietà per verificare il nome di una vista.

Successivamente, sono stati esaminati la procedura per verificare il contenuto di `View Data`. Si è appreso come controllare se è stato restituito il prodotto a destra `View Data` dopo la chiamata a un'azione del controller.

Infine, abbiamo parlato di come è possibile verificare se vengono restituiti tipi diversi di risultati dell'azione da un'azione del controller. Si è appreso come verificare se un controller restituisce un `ViewResult` o un `RedirectToRouteResult`.

> [!div class="step-by-step"]
> [Precedente](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
