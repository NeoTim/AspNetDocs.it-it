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
# <a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="3bed6-103">Creare un nuovo progetto ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="3bed6-103">Create a New ASP.NET MVC Project</span></span>

<span data-ttu-id="3bed6-104">da parte [di Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="3bed6-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="3bed6-105">Scarica il PDF</span><span class="sxs-lookup"><span data-stu-id="3bed6-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="3bed6-106">Questo è il passaggio 1 di un'esercitazione gratuita [sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come compilare un'applicazione Web piccola ma completa usando ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="3bed6-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="3bed6-107">Passaggio 1 viene illustrato come inserire la struttura dell'applicazione NerdDinner di base sul posto.</span><span class="sxs-lookup"><span data-stu-id="3bed6-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="3bed6-108">Se si utilizza ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [ALL'archivio musicale MVC.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="3bed6-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="3bed6-109">Passaggio 1 di NerdDinner: File-&gt;Nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="3bed6-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="3bed6-110">Inizieremo l'applicazione NerdDinner selezionando la voce di menu **File-&gt;Nuovo progetto** all'interno di Visual Studio 2008 o la versione gratuita di Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="3bed6-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="3bed6-111">Verrà eseguito la finestra di dialogo "Nuovo progetto".</span><span class="sxs-lookup"><span data-stu-id="3bed6-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="3bed6-112">Per creare una nuova applicazione MVC ASP.NET, si selezionerà il nodo "Web" sul lato sinistro della finestra di dialogo e quindi scegliere il modello di progetto "ASP.NET MVC Web Application" a destra:</span><span class="sxs-lookup"><span data-stu-id="3bed6-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="3bed6-113">*Importante: Assicurarsi di aver scaricato e installato ASP.NET MVC, altrimenti non verrà visualizzato nella finestra di dialogo Nuovo progetto. È possibile utilizzare V2 dell'Installazione [guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) se non è ancora stato&gt;installato (ASP.NET MVC è disponibile all'interno della sezione "Piattaforma Web- Framework e runtime").*</span><span class="sxs-lookup"><span data-stu-id="3bed6-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="3bed6-114">Chiameremo il nuovo progetto che creeremo "NerdDinner" e quindi fare clic sul pulsante "ok" per crearlo.</span><span class="sxs-lookup"><span data-stu-id="3bed6-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="3bed6-115">Quando si fa clic su "ok" Visual Studio verrà visualizzata una finestra di dialogo aggiuntiva che richiede di creare facoltativamente un progetto di unit test per la nuova applicazione pure.</span><span class="sxs-lookup"><span data-stu-id="3bed6-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="3bed6-116">Questo progetto di unit test consente di creare test automatizzati che verificano la funzionalità e il comportamento dell'applicazione (qualcosa verrà illustrato come fare più avanti in questa esercitazione).</span><span class="sxs-lookup"><span data-stu-id="3bed6-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="3bed6-117">L'elenco a discesa "Framework di test" nella finestra di dialogo precedente viene popolato con tutti i modelli di progetto di unit test MVC disponibili ASP.NET installati nel computer.</span><span class="sxs-lookup"><span data-stu-id="3bed6-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="3bed6-118">Le versioni possono essere scaricate per NUnit, MBUnit e XUnit.</span><span class="sxs-lookup"><span data-stu-id="3bed6-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="3bed6-119">È supportato anche il framework di unit test di Visual Studio incorporato.</span><span class="sxs-lookup"><span data-stu-id="3bed6-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="3bed6-120">*Nota: Visual Studio Unit Test Framework è disponibile solo con Visual Studio 2008 Professional e versioni successive. Se si utilizza VS 2008 Standard Edition o Visual Web Developer 2008 Express, sarà necessario scaricare e installare le estensioni NUnit, MBUnit o XUnit per ASP.NET MVC affinché questa finestra di dialogo venga visualizzata. La finestra di dialogo non verrà visualizzata se non sono installati framework di test.*</span><span class="sxs-lookup"><span data-stu-id="3bed6-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="3bed6-121">Useremo il nome predefinito "NerdDinner.Tests" per il progetto di test che creiamo e useremo l'opzione del framework "Visual Studio Unit Test".</span><span class="sxs-lookup"><span data-stu-id="3bed6-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="3bed6-122">Quando si fa clic sul pulsante "ok" Visual Studio creerà una soluzione per noi con due progetti in esso - uno per la nostra applicazione web e uno per i nostri unit test:</span><span class="sxs-lookup"><span data-stu-id="3bed6-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="3bed6-123">Esame della struttura di directory NerdDinner</span><span class="sxs-lookup"><span data-stu-id="3bed6-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="3bed6-124">Quando si crea una nuova applicazione MVC ASP.NET con Visual Studio, aggiunge automaticamente un numero di file e directory al progetto:</span><span class="sxs-lookup"><span data-stu-id="3bed6-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="3bed6-125">ASP.NET progetti MVC per impostazione predefinita dispongono di sei directory di primo livello:The Project projects by default have six top-level directories:</span><span class="sxs-lookup"><span data-stu-id="3bed6-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="3bed6-126">**Directory**</span><span class="sxs-lookup"><span data-stu-id="3bed6-126">**Directory**</span></span> | <span data-ttu-id="3bed6-127">**Scopo**</span><span class="sxs-lookup"><span data-stu-id="3bed6-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="3bed6-128">**/Controller**</span><span class="sxs-lookup"><span data-stu-id="3bed6-128">**/Controllers**</span></span> | <span data-ttu-id="3bed6-129">Dove si inserisceno le classi Controller che gestiscono le richieste di URL</span><span class="sxs-lookup"><span data-stu-id="3bed6-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="3bed6-130">**/Modelli**</span><span class="sxs-lookup"><span data-stu-id="3bed6-130">**/Models**</span></span> | <span data-ttu-id="3bed6-131">Dove si inserisceno classi che rappresentano e manipolano i dati</span><span class="sxs-lookup"><span data-stu-id="3bed6-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="3bed6-132">**/Viste**</span><span class="sxs-lookup"><span data-stu-id="3bed6-132">**/Views**</span></span> | <span data-ttu-id="3bed6-133">Dove inserire i file di modello dell'interfaccia utente responsabili del rendering dell'output</span><span class="sxs-lookup"><span data-stu-id="3bed6-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="3bed6-134">**/Script**</span><span class="sxs-lookup"><span data-stu-id="3bed6-134">**/Scripts**</span></span> | <span data-ttu-id="3bed6-135">Dove inserire i file e gli script della libreria JavaScript (.js)</span><span class="sxs-lookup"><span data-stu-id="3bed6-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="3bed6-136">**/Contenuto**</span><span class="sxs-lookup"><span data-stu-id="3bed6-136">**/Content**</span></span> | <span data-ttu-id="3bed6-137">Dove si inseriscono i file CSS e di immagine e altri contenuti non dinamici/non JavaScript</span><span class="sxs-lookup"><span data-stu-id="3bed6-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="3bed6-138">**/Dati\_app**</span><span class="sxs-lookup"><span data-stu-id="3bed6-138">**/App\_Data**</span></span> | <span data-ttu-id="3bed6-139">Posizione in cui si archiviano i file di dati che si desidera leggere/scrivere.</span><span class="sxs-lookup"><span data-stu-id="3bed6-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="3bed6-140">ASP.NET MVC non richiede questa struttura.</span><span class="sxs-lookup"><span data-stu-id="3bed6-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="3bed6-141">Infatti, gli sviluppatori che lavorano su applicazioni di grandi dimensioni in genere partizionano l'applicazione tra più progetti per renderla più gestibile (ad esempio: le classi del modello di dati spesso passano in un progetto di libreria di classi separato dall'applicazione Web).</span><span class="sxs-lookup"><span data-stu-id="3bed6-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="3bed6-142">La struttura di progetto predefinita, tuttavia, fornisce una convenzione di directory predefinita che è possibile utilizzare per mantenere puliti i problemi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3bed6-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="3bed6-143">Quando si espande la directory /Controllers scopriremo che Visual Studio ha aggiunto due classi controller – HomeController e AccountController – per impostazione predefinita al progetto:When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span><span class="sxs-lookup"><span data-stu-id="3bed6-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="3bed6-144">Quando espandiamo la directory /Views, troveremo tre sottodirectory – /Home, /Account e /Shared – così come diversi file di modello al loro interno sono stati aggiunti al progetto per impostazione predefinita:</span><span class="sxs-lookup"><span data-stu-id="3bed6-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="3bed6-145">Quando espandiamo le directory /Content e /Scripts, troviamo un file Site.css che viene utilizzato per applicare uno stile a tutto il codice HTML nel sito, nonché librerie JavaScript che possono abilitare ASP.NET supporto AJAX e jQuery all'interno dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="3bed6-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="3bed6-146">Quando espandiamo il progetto NerdDinner.Tests, verranno individuate due classi che contengono unit test per le classi controller:When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span><span class="sxs-lookup"><span data-stu-id="3bed6-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="3bed6-147">Questi file predefiniti aggiunti da Visual Studio ci forniscono una struttura di base per un'applicazione funzionante - completo di home page, informazioni sulla pagina, pagine di login/logout/registrazione dell'account e una pagina di errore non gestita (tutti wired-up e funzionanti fuori dalla scatola).</span><span class="sxs-lookup"><span data-stu-id="3bed6-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="3bed6-148">Esecuzione dell'applicazione NerdDinnerRunning the NerdDinner Application</span><span class="sxs-lookup"><span data-stu-id="3bed6-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="3bed6-149">È possibile eseguire il progetto scegliendo le voci di menu Debug- Avvia debug o Debug- Avvia senza eseguire debug:We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span><span class="sxs-lookup"><span data-stu-id="3bed6-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="3bed6-150">Verrà avviata la ASP.NET server Web incorporato fornito con Visual Studio ed eseguire l'applicazione seguente:</span><span class="sxs-lookup"><span data-stu-id="3bed6-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="3bed6-151">Di seguito è riportata la home page del nuovo progetto (URL: "/") quando viene eseguito:</span><span class="sxs-lookup"><span data-stu-id="3bed6-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="3bed6-152">Facendo clic sulla scheda "Informazioni" viene visualizzata una pagina di informazioni (URL: "/Home/Informazioni):Clicking the "About" tab ive to a about page (URL: "/Home/About"):</span><span class="sxs-lookup"><span data-stu-id="3bed6-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="3bed6-153">Facendo clic sul collegamento "Log On" in alto a destra si accede a una pagina di accesso (URL: "/Account/LogOn")</span><span class="sxs-lookup"><span data-stu-id="3bed6-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="3bed6-154">Se non abbiamo un account di login possiamo fare clic sul link di registrazione (URL: "/Account/Registra") per crearne uno:</span><span class="sxs-lookup"><span data-stu-id="3bed6-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="3bed6-155">Il codice per implementare la funzionalità home, about e logout/log è stato aggiunto per impostazione predefinita quando abbiamo creato il nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="3bed6-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="3bed6-156">Lo useremo come punto di partenza della nostra applicazione.</span><span class="sxs-lookup"><span data-stu-id="3bed6-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="3bed6-157">Test dell'applicazione NerdDinnerTesting the NerdDinner Application</span><span class="sxs-lookup"><span data-stu-id="3bed6-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="3bed6-158">Se si usa la Professional Edition o la versione successiva di Visual Studio 2008, è possibile usare il supporto IDE di unit test incorporato all'interno di Visual Studio per testare il progetto:</span><span class="sxs-lookup"><span data-stu-id="3bed6-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="3bed6-159">Scegliendo una delle opzioni precedenti si aprirà il riquadro "Risultati test" all'interno dell'IDE e ci fornirà lo stato di superamento/fallimento sui 27 unit test inclusi nel nostro nuovo progetto che coprono la funzionalità incorporata:</span><span class="sxs-lookup"><span data-stu-id="3bed6-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="3bed6-160">Più avanti in questa esercitazione parleremo di ulteriori informazioni sui test automatizzati e aggiungeremo ulteriori unit test che coprono la funzionalità dell'applicazione implementata.</span><span class="sxs-lookup"><span data-stu-id="3bed6-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="3bed6-161">passaggio successivo</span><span class="sxs-lookup"><span data-stu-id="3bed6-161">Next Step</span></span>

<span data-ttu-id="3bed6-162">Ora abbiamo una struttura di applicazione di base in atto.</span><span class="sxs-lookup"><span data-stu-id="3bed6-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="3bed6-163">Creiamo ora [un database per archiviare i dati dell'applicazione.](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="3bed6-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3bed6-164">[Successivo](introducing-the-nerddinner-tutorial.md)
> [precedente](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="3bed6-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
