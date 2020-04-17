---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Creazione di un nuovo ASP.NET di un progetto MVC Documenti Microsoft
author: rick-anderson
description: Passaggio 1 viene illustrato come inserire la struttura dell'applicazione NerdDinner di base sul posto.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 240c8a04cec3c07f434182775d1c519aab029761
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541481"
---
# <a name="create-a-new-aspnet-mvc-project"></a>Creare un nuovo progetto ASP.NET MVC

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 1 di un'esercitazione gratuita [sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come compilare un'applicazione Web piccola ma completa usando ASP.NET MVC 1.
> 
> Passaggio 1 viene illustrato come inserire la struttura dell'applicazione NerdDinner di base sul posto.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [ALL'archivio musicale MVC.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-1-file-gtnew-project"></a>Passaggio 1 di NerdDinner: File-&gt;Nuovo progetto

Inizieremo l'applicazione NerdDinner selezionando la voce di menu **File-&gt;Nuovo progetto** all'interno di Visual Studio 2008 o la versione gratuita di Visual Web Developer 2008 Express.

Verrà eseguito la finestra di dialogo "Nuovo progetto". Per creare una nuova applicazione MVC ASP.NET, si selezionerà il nodo "Web" sul lato sinistro della finestra di dialogo e quindi scegliere il modello di progetto "ASP.NET MVC Web Application" a destra:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Importante: Assicurarsi di aver scaricato e installato ASP.NET MVC, altrimenti non verrà visualizzato nella finestra di dialogo Nuovo progetto. È possibile utilizzare V2 dell'Installazione [guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) se non è ancora stato&gt;installato (ASP.NET MVC è disponibile all'interno della sezione "Piattaforma Web- Framework e runtime").*

Chiameremo il nuovo progetto che creeremo "NerdDinner" e quindi fare clic sul pulsante "ok" per crearlo.

Quando si fa clic su "ok" Visual Studio verrà visualizzata una finestra di dialogo aggiuntiva che richiede di creare facoltativamente un progetto di unit test per la nuova applicazione pure. Questo progetto di unit test consente di creare test automatizzati che verificano la funzionalità e il comportamento dell'applicazione (qualcosa verrà illustrato come fare più avanti in questa esercitazione).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

L'elenco a discesa "Framework di test" nella finestra di dialogo precedente viene popolato con tutti i modelli di progetto di unit test MVC disponibili ASP.NET installati nel computer. Le versioni possono essere scaricate per NUnit, MBUnit e XUnit. È supportato anche il framework di unit test di Visual Studio incorporato.

*Nota: Visual Studio Unit Test Framework è disponibile solo con Visual Studio 2008 Professional e versioni successive. Se si utilizza VS 2008 Standard Edition o Visual Web Developer 2008 Express, sarà necessario scaricare e installare le estensioni NUnit, MBUnit o XUnit per ASP.NET MVC affinché questa finestra di dialogo venga visualizzata. La finestra di dialogo non verrà visualizzata se non sono installati framework di test.*

Useremo il nome predefinito "NerdDinner.Tests" per il progetto di test che creiamo e useremo l'opzione del framework "Visual Studio Unit Test". Quando si fa clic sul pulsante "ok" Visual Studio creerà una soluzione per noi con due progetti in esso - uno per la nostra applicazione web e uno per i nostri unit test:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Esame della struttura di directory NerdDinner

Quando si crea una nuova applicazione MVC ASP.NET con Visual Studio, aggiunge automaticamente un numero di file e directory al progetto:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

ASP.NET progetti MVC per impostazione predefinita dispongono di sei directory di primo livello:The Project projects by default have six top-level directories:

| **Directory** | **Scopo** |
| --- | --- |
| **/Controller** | Dove si inserisceno le classi Controller che gestiscono le richieste di URL |
| **/Modelli** | Dove si inserisceno classi che rappresentano e manipolano i dati |
| **/Viste** | Dove inserire i file di modello dell'interfaccia utente responsabili del rendering dell'output |
| **/Script** | Dove inserire i file e gli script della libreria JavaScript (.js) |
| **/Contenuto** | Dove si inseriscono i file CSS e di immagine e altri contenuti non dinamici/non JavaScript |
| **/Dati\_app** | Posizione in cui si archiviano i file di dati che si desidera leggere/scrivere. |

ASP.NET MVC non richiede questa struttura. Infatti, gli sviluppatori che lavorano su applicazioni di grandi dimensioni in genere partizionano l'applicazione tra più progetti per renderla più gestibile (ad esempio: le classi del modello di dati spesso passano in un progetto di libreria di classi separato dall'applicazione Web). La struttura di progetto predefinita, tuttavia, fornisce una convenzione di directory predefinita che è possibile utilizzare per mantenere puliti i problemi dell'applicazione.

Quando si espande la directory /Controllers scopriremo che Visual Studio ha aggiunto due classi controller – HomeController e AccountController – per impostazione predefinita al progetto:When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Quando espandiamo la directory /Views, troveremo tre sottodirectory – /Home, /Account e /Shared – così come diversi file di modello al loro interno sono stati aggiunti al progetto per impostazione predefinita:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Quando espandiamo le directory /Content e /Scripts, troviamo un file Site.css che viene utilizzato per applicare uno stile a tutto il codice HTML nel sito, nonché librerie JavaScript che possono abilitare ASP.NET supporto AJAX e jQuery all'interno dell'applicazione:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Quando espandiamo il progetto NerdDinner.Tests, verranno individuate due classi che contengono unit test per le classi controller:When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Questi file predefiniti aggiunti da Visual Studio ci forniscono una struttura di base per un'applicazione funzionante - completo di home page, informazioni sulla pagina, pagine di login/logout/registrazione dell'account e una pagina di errore non gestita (tutti wired-up e funzionanti fuori dalla scatola).

### <a name="running-the-nerddinner-application"></a>Esecuzione dell'applicazione NerdDinnerRunning the NerdDinner Application

È possibile eseguire il progetto scegliendo le voci di menu Debug- Avvia debug o Debug- Avvia senza eseguire debug:We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Verrà avviata la ASP.NET server Web incorporato fornito con Visual Studio ed eseguire l'applicazione seguente:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Di seguito è riportata la home page del nuovo progetto (URL: "/") quando viene eseguito:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Facendo clic sulla scheda "Informazioni" viene visualizzata una pagina di informazioni (URL: "/Home/Informazioni):Clicking the "About" tab ive to a about page (URL: "/Home/About"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Facendo clic sul collegamento "Log On" in alto a destra si accede a una pagina di accesso (URL: "/Account/LogOn")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Se non abbiamo un account di login possiamo fare clic sul link di registrazione (URL: "/Account/Registra") per crearne uno:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

Il codice per implementare la funzionalità home, about e logout/log è stato aggiunto per impostazione predefinita quando abbiamo creato il nuovo progetto. Lo useremo come punto di partenza della nostra applicazione.

### <a name="testing-the-nerddinner-application"></a>Test dell'applicazione NerdDinnerTesting the NerdDinner Application

Se si usa la Professional Edition o la versione successiva di Visual Studio 2008, è possibile usare il supporto IDE di unit test incorporato all'interno di Visual Studio per testare il progetto:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Scegliendo una delle opzioni precedenti si aprirà il riquadro "Risultati test" all'interno dell'IDE e ci fornirà lo stato di superamento/fallimento sui 27 unit test inclusi nel nostro nuovo progetto che coprono la funzionalità incorporata:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Più avanti in questa esercitazione parleremo di ulteriori informazioni sui test automatizzati e aggiungeremo ulteriori unit test che coprono la funzionalità dell'applicazione implementata.

### <a name="next-step"></a>passaggio successivo

Ora abbiamo una struttura di applicazione di base in atto. Creiamo ora [un database per archiviare i dati dell'applicazione.](create-a-database.md)

> [!div class="step-by-step"]
> [Successivo](introducing-the-nerddinner-tutorial.md)
> [precedente](create-a-database.md)
