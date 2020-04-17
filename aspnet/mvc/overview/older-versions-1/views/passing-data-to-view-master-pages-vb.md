---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: Passaggio di dati per visualizzare le pagine master (VB)Passing Data to View Master Pages (VB) Documenti Microsoft
author: rick-anderson
description: L'obiettivo di questa esercitazione è spiegare come è possibile passare i dati da un controller a una pagina master di visualizzazione. Esaminiamo due strategie per il passaggio di dati a una vista m...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: ef65541a5f2297414f923e2f8108a14de67531e5
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542469"
---
# <a name="passing-data-to-view-master-pages-vb"></a>Passaggio di dati a pagine master di visualizzazione (VB)

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> L'obiettivo di questa esercitazione è spiegare come è possibile passare i dati da un controller a una pagina master di visualizzazione. Esaminiamo due strategie per il passaggio di dati a una pagina master di visualizzazione. In primo luogo, discutiamo di una soluzione semplice che si traduce in un'applicazione che è difficile da gestire. Successivamente, esaminiamo una soluzione molto migliore che richiede un po 'più di lavoro iniziale, ma si traduce in un'applicazione molto più gestibile.

## <a name="passing-data-to-view-master-pages"></a>Passaggio di dati per visualizzare le pagine masterPassing Data to View Master Pages

L'obiettivo di questa esercitazione è spiegare come è possibile passare i dati da un controller a una pagina master di visualizzazione. Esaminiamo due strategie per il passaggio di dati a una pagina master di visualizzazione. In primo luogo, discutiamo di una soluzione semplice che si traduce in un'applicazione che è difficile da gestire. Successivamente, esaminiamo una soluzione molto migliore che richiede un po 'più di lavoro iniziale, ma si traduce in un'applicazione molto più gestibile.

### <a name="the-problem"></a>Problema

Si supponga che si sta creando un'applicazione di database di film e si desidera visualizzare l'elenco delle categorie di film in ogni pagina dell'applicazione (vedere Figura 1). Si supponga, inoltre, che l'elenco delle categorie di film viene memorizzato in una tabella di database. In tal caso, sarebbe opportuno recuperare le categorie dal database ed eseguire il rendering dell'elenco delle categorie di film all'interno di una pagina master di visualizzazione.

[![Visualizzazione delle categorie di film in una pagina master di visualizzazione](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)

**Figura 01**: Visualizzazione delle categorie di film in una pagina master di visualizzazione ([Fare clic per visualizzare l'immagine](passing-data-to-view-master-pages-vb/_static/image3.png)a dimensioni intere )

Ecco il problema. Come si recupera l'elenco delle categorie di film nella pagina master? Si è tentati di chiamare direttamente i metodi delle classi del modello nella pagina master. In altre parole, si è tentati di includere il codice per il recupero dei dati dal database direttamente nella pagina master. Tuttavia, ignorando i controller MVC per accedere al database violerebbe la netta separazione dei problemi che è uno dei vantaggi principali della creazione di un'applicazione MVC.

In un'applicazione MVC, si desidera che tutte le interazioni tra le visualizzazioni MVC e il modello MVC vengano gestite dai controller MVC. Questa separazione dei problemi si traduce in un'applicazione più gestibile, adattabile e tesa.

In un'applicazione MVC, tutti i dati passati a una visualizzazione, inclusa una pagina master di visualizzazione, devono essere passati a una visualizzazione da un'azione del controller. Inoltre, i dati devono essere passati sfruttando i dati di visualizzazione. Nella parte restante di questa esercitazione, esaminare due metodi di passaggio dei dati di visualizzazione a una pagina master di visualizzazione.

### <a name="the-simple-solution"></a>La soluzione semplice

Iniziamo con la soluzione più semplice per passare i dati di visualizzazione da un controller a una pagina master di visualizzazione. La soluzione più semplice consiste nel passare i dati di visualizzazione per la pagina master in ogni azione del controller.

Si consideri il controller nel listato 1.Consider the controller in Listing 1. Espone due azioni `Index()` denominate e . `Details()` Il `Index()` metodo di azione restituisce tutti i filmati nella tabella di database Movies. Il `Details()` metodo di azione restituisce tutti i filmati in una particolare categoria di filmato.

**Lista torto 1 –`Controllers\HomeController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

Si noti `Index()` che `Details()` entrambe le azioni e aggiungono due elementi per visualizzare i dati. L'azione `Index()` aggiunge due tasti: categorie e film. La chiave categories rappresenta l'elenco delle categorie di film visualizzati dalla pagina master di visualizzazione. Il tasto filmati rappresenta l'elenco dei filmati visualizzati dalla pagina di visualizzazione dell'indice.

L'azione `Details()` aggiunge anche due tasti denominati categorie e film. La chiave categories, ancora una volta, rappresenta l'elenco delle categorie di film visualizzati dalla pagina master di visualizzazione. Il tasto movies rappresenta l'elenco dei filmati in una particolare categoria visualizzata dalla pagina di visualizzazione Dettagli (vedere Figura 2).

[![Visualizzazione Dettagli](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)

**Figura 02**: Visualizzazione Dettagli ([Fare clic per visualizzare l'immagine a dimensioni intere)](passing-data-to-view-master-pages-vb/_static/image6.png)

La vista Indice è contenuta nel listato 2. Scorre semplicemente l'elenco dei filmati rappresentati dall'elemento movies nei dati di visualizzazione.

**Listato 2 –`Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

La pagina master di visualizzazione è contenuta nel listato 3. La pagina master di visualizzazione scorre ed esegue il rendering di tutte le categorie di film rappresentate dall'elemento categories dai dati di visualizzazione.

**Lista 3 –`Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

Tutti i dati vengono passati alla visualizzazione e alla pagina master della visualizzazione tramite i dati della visualizzazione. Questo è il modo corretto per passare i dati alla pagina master.

Allora, cosa c'è di sbagliato in questa soluzione? Il problema è che questa soluzione viola il principio DRY (Don't Repeat Yourself). Ogni azione del controller deve aggiungere lo stesso elenco di categorie di film per visualizzare i dati. La presenza di codice duplicato nell'applicazione rende l'applicazione molto più difficile da gestire, adattare e modificare.

### <a name="the-good-solution"></a>La buona soluzione

In questa sezione viene esaminata una soluzione alternativa e migliore per passare i dati da un'azione del controller a una pagina master di visualizzazione. Invece di aggiungere le categorie di film per la pagina master in ogni azione del controller, aggiungiamo le categorie di film ai dati di visualizzazione una sola volta. Tutti i dati di visualizzazione utilizzati dalla pagina master di visualizzazione vengono aggiunti in un controller dell'applicazione.

La classe ApplicationController è contenuta nel listato 4.

La classe ApplicationController è contenuta nel listato 4.

**Lista 4 –`Controllers\ApplicationController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

Ci sono tre cose che si dovrebbe notare circa il controller dell'applicazione nel listato 4. In primo luogo, si noti che la classe eredita dalla classe System.Web.Mvc.Controller di base. Il controller dell'applicazione è una classe controller.

In secondo luogo, si noti che la classe controller dell'applicazione è una classe MustInherit.Second, notice that the Application controller class is a MustInherit class. Una classe MustInherit è una classe che deve essere implementata da una classe concreta. Poiché il controller dell'applicazione è una classe MustInherit, non è possibile richiamare direttamente i metodi definiti nella classe. Se si tenta di richiamare direttamente la classe Application, verrà visualizzato un messaggio di errore Risorsa non trovata.

In terzo luogo, si noti che il controller dell'applicazione contiene un costruttore che aggiunge l'elenco di categorie di film per visualizzare i dati. Ogni classe controller che eredita dal controller dell'applicazione chiama automaticamente il costruttore del controller dell'applicazione. Ogni volta che si chiama qualsiasi azione su qualsiasi controller che eredita dal controller dell'applicazione, le categorie di film vengono incluse automaticamente nei dati di visualizzazione.

Il controller Movies nel listato 5 eredita dal controller dell'applicazione.

**Lista 5 –`Controllers\MoviesController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

Il controller Movies, proprio come il controller Home descritto nella `Index()` sezione `Details()`precedente, espone due metodi di azione denominati e . Si noti che l'elenco delle categorie di film visualizzati dalla `Index()` `Details()` pagina master di visualizzazione non viene aggiunto per visualizzare i dati nel metodo o . Poiché il controller Film eredita dal controller dell'applicazione, l'elenco delle categorie di film viene aggiunto per visualizzare automaticamente i dati.

Si noti che questa soluzione per l'aggiunta di dati di visualizzazione per una pagina master di visualizzazione non viola il principio DRY (Don't Repeat Yourself). Il codice per l'aggiunta dell'elenco di categorie di film per visualizzare i dati è contenuto in una sola posizione: il costruttore per il controller dell'applicazione.

### <a name="summary"></a>Riepilogo

In questa esercitazione sono stati illustrati due approcci per passare i dati di visualizzazione da un controller a una pagina master di visualizzazione. In primo luogo, abbiamo esaminato un approccio semplice, ma difficile da mantenere. Nella prima sezione, è stato illustrato come è possibile aggiungere i dati di visualizzazione per una pagina master di visualizzazione in ogni azione del controller nell'applicazione. Abbiamo concluso che questo è stato un cattivo approccio perché viola il principio DRY (Don't Repeat Yourself).

Successivamente, è stata esaminata una strategia molto migliore per l'aggiunta di dati richiesti da una pagina master di visualizzazione per visualizzare i dati. Anziché aggiungere i dati di visualizzazione in ogni azione del controller, i dati di visualizzazione sono stati aggiunti una sola volta all'interno di un controller dell'applicazione. In questo modo, è possibile evitare codice duplicato quando si passano dati a una pagina master di visualizzazione in un'applicazione MVC ASP.NET.

> [!div class="step-by-step"]
> [Indietro](creating-page-layouts-with-view-master-pages-vb.md)
