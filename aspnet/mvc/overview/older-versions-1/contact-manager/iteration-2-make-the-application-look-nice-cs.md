---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: "Iterazione #2 – Rendere l'applicazione un aspetto gradesia (C ) Documenti Microsoft"
author: rick-anderson
description: In questa iterazione viene migliorato l'aspetto dell'applicazione modificando la pagina master della visualizzazione MVC ASP.NET predefinita e il foglio di stile CSS.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: ee1d7c92524f6cbdb0f2d7facf85b629e0d91318
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542443"
---
# <a name="iteration-2--make-the-application-look-nice-c"></a>Iterazione 2 - Migliorare l'aspetto dell'applicazione (C#)

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il codice](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> In questa iterazione viene migliorato l'aspetto dell'applicazione modificando la pagina master della visualizzazione MVC ASP.NET predefinita e il foglio di stile CSS.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Compilazione di una gestione dei contatti ASP.NET'applicazione MVC (c ')

In questa serie di esercitazioni viene compilata un'intera applicazione di gestione dei contatti dall'inizio alla fine. L'applicazione Contact Manager consente di memorizzare le informazioni di contatto - nomi, numeri di telefono e indirizzi e-mail - per un elenco di persone.

Costruiamo l'applicazione su più iterazioni. Ad ogni iterazione, miglioriamo gradualmente l'applicazione. L'obiettivo di questo approccio a più iterazioni è consentire di comprendere il motivo di ogni modifica.

- Iterazione #1- Crea l'applicazione. Nella prima iterazione, creiamo il Contact Manager nel modo più semplice possibile. Aggiungiamo il supporto per le operazioni di base del database: crea, leggi, aggiorna ed elimina (CRUD).

- Iterazione #2 - Rendere l'applicazione un aspetto gradel. In questa iterazione viene migliorato l'aspetto dell'applicazione modificando la pagina master della visualizzazione MVC ASP.NET predefinita e il foglio di stile CSS.

- #3 iterazione - Aggiungere la convalida del form. Nella terza iterazione, aggiungiamo la convalida di base del form. Impediamo alle persone di inviare un modulo senza compilare i campi modulo obbligatori. Convalidiamo anche gli indirizzi e-mail e i numeri di telefono.

- Iterazione #4 - Rendere l'applicazione liberamente accoppiato. In questa quarta iterazione, sfruttiamo diversi modelli di progettazione software per semplificare la manutenzione e la modifica dell'applicazione Contact Manager. Ad esempio, viene eseguito il refactoring dell'applicazione per usare il modello Repository e il modello di inserimento delle dipendenze.

- #5 di iterazione - Creare unit test. Nella quinta iterazione, seminiamo la nostra applicazione più facile da gestire e modificare aggiungendo unit test. Ci prendiamo gioco delle classi del modello di dati e creiamo unit test per i controller e la logica di convalida.

- #6 di iterazione - Utilizzare lo sviluppo basato su test. In questa sesta iterazione, aggiungiamo nuove funzionalità all'applicazione scrivendo prima gli unit test e scrivendo il codice sugli unit test. In questa iterazione vengono aggiunti gruppi di contatti.

- Iterazione #7- Aggiungere funzionalità Ajax.Iteration #7 - Add Ajax functionality. Nella settima iterazione, miglioriamo la velocità di risposta e le prestazioni dell'applicazione aggiungendo il supporto per Ajax.

## <a name="this-iteration"></a>Questa iterazione

L'obiettivo di questa iterazione è migliorare l'aspetto dell'applicazione Contact Manager. Attualmente, contact Manager utilizza il valore predefinito ASP.NET pagina master visualizzazione MVC e foglio di stile CSS (vedere Figura 1). Questi non sembrano male, ma non voglio che il Contact Manager a guardare proprio come ogni altro ASP.NET mvc sito Web. Voglio sostituire questi file con file personalizzati.

[![Finestra di dialogo relativa al nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**Figura 01**: l'aspetto predefinito di un'applicazione MVC ASP.NET[(Fare clic per visualizzare l'immagine](iteration-2-make-the-application-look-nice-cs/_static/image2.png)a dimensioni intere)

In questa iterazione, vengono illustrati due approcci per migliorare la progettazione visiva dell'applicazione. In primo luogo, vi mostrerò come sfruttare il ASP.NET di MVC progettazione per scaricare un modello di progettazione MVC ASP.NET gratuito. La raccolta Progettazione MVC ASP.NET consente di creare un'applicazione Web professionale senza eseguire alcuna operazione.

Ho deciso di non utilizzare un modello dalla raccolta di progettazione MVC ASP.NET per l'applicazione Contact Manager. Invece, ho avuto un design personalizzato creato da uno studio di design professionale. Nella seconda parte di questa esercitazione spiego come ho lavorato con una società di progettazione professionale per creare la progettazione finale ASP.NET MVC.

## <a name="the-aspnet-mvc-design-gallery"></a>Raccolta ASP.NET mvc

Il ASP.NET MVC Design Gallery è una risorsa gratuita fornita da Microsoft. Il ASP.NET raccolta MVC si trova all'indirizzo seguente:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

Il ASP.NET MVC Design Gallery ospita una raccolta di progetti di siti Web gratuiti creati in modo specifico per l'utilizzo in un progetto MVC ASP.NET. I disegni e modelli vengono caricati dai membri della community. I visitatori della Galleria possono votare per i loro disegni preferiti (vedi Figura 2).

[![Finestra di dialogo relativa al nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**Figura 02**: il ASP.NET raccolta di progettazione MVC ([Fare clic per visualizzare l'immagine](iteration-2-make-the-application-look-nice-cs/_static/image4.png)a dimensioni intere )

Mentre scrivo questo tutorial, il design più popolare nella galleria è un design chiamato ottobre da David Hauser. È possibile utilizzare questa progettazione per un progetto MVC ASP.NET completando i passaggi seguenti:You can use this design for an ASP.NET MVC project by completing the following steps:

1. Fare clic sul pulsante **Download** per scaricare il file October.zip nel computer.
2. Fare clic con il pulsante destro del mouse sul file October.zip scaricato e scegliere il pulsante **Sblocca** (vedere la figura 3).
3. Decomprimere il file in una cartella denominata ottobre.
4. Selezionare tutti i file dalla cartella DesignTemplate contenuta nella cartella Ottobre, fare clic con il pulsante destro del mouse sui file e selezionare l'opzione di menu **Copia**.
5. Fare clic con il pulsante destro del mouse sul nodo del progetto ContactManager nella finestra Esplora soluzioni di Visual Studio e selezionare l'opzione di menu **Incolla** (vedere la Figura 4).
6. Selezionare l'opzione di menu di Visual Studio **Modifica, Trova e sostituisci, Sostituisci rapidamente** e *sostituisci [MyProjectName]* con *ContactManager* (vedere Figura 5).

[![Finestra di dialogo relativa al nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**Figura 03**: Sblocco di un file scaricato dal Web ([Fare clic per visualizzare l'immagine](iteration-2-make-the-application-look-nice-cs/_static/image6.png)a dimensioni intere )

[![Finestra di dialogo relativa al nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**Figura 04**: sovrascrittura di file in Esplora soluzioni ([Fare clic per visualizzare l'immagine a dimensioni intere](iteration-2-make-the-application-look-nice-cs/_static/image8.png))

[![Finestra di dialogo relativa al nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**Figura 05**: Sostituzione di [ProjectName] con ContactManager ([Fare clic per visualizzare l'immagine a dimensioni intere)](iteration-2-make-the-application-look-nice-cs/_static/image10.png)

Dopo aver completato questi passaggi, l'applicazione Web utilizzerà la nuova progettazione. La pagina in Figura 6 illustra l'aspetto dell'applicazione Contact Manager con la progettazione di ottobre.

[![Finestra di dialogo relativa al nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**Figura 06**: ContactManager con il modello di ottobre ([Fare clic per visualizzare l'immagine](iteration-2-make-the-application-look-nice-cs/_static/image12.png)a dimensioni intere )

## <a name="creating-a-custom-aspnet-mvc-design"></a>Creazione di una progettazione MVC ASP.NET personalizzata

Il ASP.NET MVC Design Gallery dispone di una buona selezione di diversi stili di progettazione. La raccolta offre un modo indolore per personalizzare l'aspetto delle applicazioni MVC ASP.NET. E, naturalmente, la Galleria ha il grande vantaggio di essere completamente gratuito.

Tuttavia, potrebbe essere necessario creare un design completamente unico per il tuo sito web. In tal caso, ha senso lavorare con una società di progettazione di siti web. Ho deciso di adottare questo approccio per la progettazione per l'applicazione Contact Manager.

Ho riempito il Contact Manager da Iteration #1 e ho inviato il progetto alla società di progettazione. Non erano proprietari di Visual Studio (vergogna su di loro!), ma che non ha presentato un problema. Sono stati in grado di scaricare Microsoft [https://www.asp.net](https://www.asp.net) Visual Web Developer gratuitamente dal sito Web e aprire l'applicazione Contact Manager in Visual Web Developer. In un paio di giorni, avevano prodotto il disegno in Figura 7.

[![Finestra di dialogo relativa al nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**Figura 07**: il ASP.NET Progettazione di MVC Contact Manager ([Fare clic per visualizzare l'immagine](iteration-2-make-the-application-look-nice-cs/_static/image14.png)a dimensioni intere )

Il nuovo progetto consisteva in due file principali: un nuovo file di foglio di stile CSS e un nuovo file di pagina master di visualizzazione. Una pagina master di visualizzazione contiene il layout e il contenuto condiviso per le visualizzazioni in un'applicazione MVC ASP.NET. Ad esempio, la pagina master di visualizzazione include l'intestazione, le schede di spostamento e il piè di pagina visualizzati nella Figura 7. Ho sovrascritto la pagina master della visualizzazione Site.Master esistente nella cartella Views/Shared con il nuovo file Site.Master della società di progettazione,

L'azienda di progettazione ha inoltre creato un nuovo foglio di stile CSS e un set di immagini. Ho inserito questi nuovi file nella cartella Content e ho sovrascritto il file Site.css esistente. È necessario inserire tutto il contenuto statico nella cartella Content.

Si noti che il nuovo progetto per Contact Manager include immagini per la modifica e l'eliminazione dei contatti. Accanto a ogni contatto nella tabella HTML dei contatti viene visualizzata un'immagine Modifica ed Elimina.

Originariamente, questi collegamenti che sono stati renderizzati con il codice HTML. Helper ActionLink() come questo:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

Il metodo Html.ActionLink() non supporta le immagini (il codice HTML del metodo codifica il testo del collegamento per motivi di sicurezza). Pertanto, ho sostituito le chiamate a Html.ActionLink() con chiamate a Url.Action() in questo modo:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

Il metodo Html.ActionLink() esegue il rendering di un intero collegamento ipertestuale HTML. Il metodo Url.Action(), d'altra parte, esegue &lt;&gt; il rendering solo dell'URL senza il tag a.

Si noti, inoltre, che il nuovo design include schede selezionate e non selezionate. Ad esempio, nella figura 8, la scheda **Crea nuovo contatto** è selezionata e la scheda Contatti **personali** non è selezionata.

[![Finestra di dialogo relativa al nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**Figura 08**: Schede selezionate e non selezionate ([Fare clic per visualizzare l'immagine](iteration-2-make-the-application-look-nice-cs/_static/image16.png)a dimensioni intere)

Per supportare il rendering di schede selezionate e non selezionate, ho creato un helper HTML personalizzato denominato MenuItemHelper.To support rendering both selected and unselected tabs, I created a custom HTML helper named the MenuItemHelper. Questo metodo helper &lt;esegue&gt; il &lt;rendering di un&gt; tag li o di un tag li class, ovvero "selected" a seconda che il controller e l'azione correnti corrispondano al controller e al nome dell'azione passati all'helper. Il codice per il MenuItemHelper è contenuto nel listato 1.The code for the MenuItemHelper is contained in Listing 1.

**Listato 1 - Helpers-MenuItemHelper.csListing 1 - Helpers-MenuItemHelper.cs**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

Il MenuItemHelper utilizza il TagBuilder classe &lt;&gt; internamente per compilare il tag html li. La classe TagBuilder è una classe di utilità molto utile che è possibile utilizzare ogni volta che è necessario creare un nuovo tag HTML. Include metodi per l'aggiunta di attributi, l'aggiunta di classi CSS, la generazione di ID e la modifica del tag s HTML interno.

## <a name="summary"></a>Riepilogo

In questa iterazione, è stata migliorata la progettazione visiva dell'applicazione ASP.NET MVC. In primo luogo, è stato introdotto al ASP.NET MVC Design Gallery. È stato illustrato come scaricare modelli di progettazione gratuiti dalla raccolta di progettazione MVC ASP.NET che è possibile utilizzare nelle applicazioni MVC ASP.NET.

Successivamente, è stato illustrato come creare un progetto personalizzato modificando il file dei fogli di stile CSS predefiniti e il file della pagina di visualizzazione master. Per supportare il nuovo design, abbiamo dovuto apportare alcune modifiche minori alla nostra applicazione Contact Manager. Ad esempio, è stato aggiunto un nuovo helper HTML denominato MenuItemHelper che visualizza le schede selezionate e deselezionate.

Nella prossima iterazione, affrontiamo il tema molto importante della convalida. Aggiungiamo il codice di convalida all'applicazione in modo che un utente non possa creare un nuovo contatto senza fornire i valori richiesti, ad esempio un nome e cognome di una persona.

> [!div class="step-by-step"]
> [Successivo](iteration-1-create-the-application-cs.md)
> [precedente](iteration-3-add-form-validation-cs.md)
