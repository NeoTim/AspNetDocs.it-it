---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Lab pratico: Strumenti Web di Visual Studio 2013 | Microsoft Docs'
author: rick-anderson
description: Visual Studio è un ambiente di sviluppo straordinaria per. Windows basata su rete e i progetti web. Include un editor di testo potente che può essere utilizzato con facilità per...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 2fb987dd9b26ad9f0e8a88fd881bde4505ec4148
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65115891"
---
# <a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="23db7-104">Lab pratico: Strumenti Web di Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="23db7-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>

<span data-ttu-id="23db7-105">da [Camp Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="23db7-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="23db7-106">Download Web Camp Kit di formazione</span><span class="sxs-lookup"><span data-stu-id="23db7-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

> <span data-ttu-id="23db7-107">Visual Studio è un ambiente di sviluppo straordinaria per. Windows basata su rete e i progetti web.</span><span class="sxs-lookup"><span data-stu-id="23db7-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="23db7-108">Include un editor di testo potente che può essere facilmente utilizzato per modificare i file autonomi senza un progetto.</span><span class="sxs-lookup"><span data-stu-id="23db7-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="23db7-109">Visual Studio gestisce un albero di analisi complete durante la modifica di ogni file.</span><span class="sxs-lookup"><span data-stu-id="23db7-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="23db7-110">In questo modo Visual Studio per fornire durante l'esecuzione molto più veloce e più piacevole l'esperienza di sviluppo senza precedenti il completamento automatico e le azioni in base al documento.</span><span class="sxs-lookup"><span data-stu-id="23db7-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="23db7-111">Queste funzionalità sono particolarmente efficace in documenti HTML e CSS.</span><span class="sxs-lookup"><span data-stu-id="23db7-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="23db7-112">Tutta questa capacità è disponibile anche per le estensioni, rendendo più semplice estendere l'editor con nuove potenti funzionalità in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="23db7-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="23db7-113">Web Essentials è una raccolta (principalmente) miglioramenti relativi al web per Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="23db7-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="23db7-114">Include numerose nuove completamenti IntelliSense (soprattutto per CSS), nuove funzionalità di collegamento del Browser, automatic file JSHint per JavaScript, nuovi avvisi per HTML e CSS e molte altre funzionalità che sono essenziali per lo sviluppo web moderno.</span><span class="sxs-lookup"><span data-stu-id="23db7-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="23db7-115">Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile all'indirizzo [ https://aka.ms/webcamps-training-kit ](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="23db7-115">All sample code and snippets are included in the Web Camps Training Kit, available at [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span></span>

<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="23db7-116">Panoramica</span><span class="sxs-lookup"><span data-stu-id="23db7-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="23db7-117">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="23db7-117">Objectives</span></span>

<span data-ttu-id="23db7-118">In questo laboratorio pratico, si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="23db7-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="23db7-119">Usare nuove funzionalità dell'editor HTML incluse in Web Essentials, ad esempio i frammenti di codice HTML5 avanzati e codifica Zen</span><span class="sxs-lookup"><span data-stu-id="23db7-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="23db7-120">Usare nuove funzionalità dell'editor CSS incluse in Web Essentials, ad esempio la selezione dei colori e una descrizione comando di matrice di Browser</span><span class="sxs-lookup"><span data-stu-id="23db7-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="23db7-121">Usare nuove funzionalità dell'editor JavaScript inclusa in Web Essentials, ad esempio l'estrazione di File e IntelliSense per tutti gli elementi HTML</span><span class="sxs-lookup"><span data-stu-id="23db7-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="23db7-122">Scambiare i dati tra browser e Visual Studio usando il collegamento del Browser</span><span class="sxs-lookup"><span data-stu-id="23db7-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="23db7-123">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="23db7-123">Prerequisites</span></span>

<span data-ttu-id="23db7-124">Di seguito è necessario per completare questo laboratorio pratico:</span><span class="sxs-lookup"><span data-stu-id="23db7-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="23db7-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) o versione successiva</span><span class="sxs-lookup"><span data-stu-id="23db7-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="23db7-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="23db7-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="23db7-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="23db7-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="23db7-128">Configurazione</span><span class="sxs-lookup"><span data-stu-id="23db7-128">Setup</span></span>

<span data-ttu-id="23db7-129">Per eseguire gli esercizi in questo laboratorio pratico, è necessario configurare prima di tutto l'ambiente.</span><span class="sxs-lookup"><span data-stu-id="23db7-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="23db7-130">Aprire una finestra di Windows Explorer e passare a del lab **origine** cartella.</span><span class="sxs-lookup"><span data-stu-id="23db7-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="23db7-131">Fare doppio clic su **Setup. cmd** e selezionare **Esegui come amministratore** per avviare il processo di installazione che la configurazione dell'ambiente e installare i frammenti di codice di Visual Studio per questo ambiente lab.</span><span class="sxs-lookup"><span data-stu-id="23db7-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="23db7-132">Se viene visualizzata la finestra di dialogo controllo dell'Account utente, confermare l'azione per continuare.</span><span class="sxs-lookup"><span data-stu-id="23db7-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="23db7-133">Assicurarsi che tutte le dipendenze per questo ambiente lab che sono stati verificati prima di eseguire il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="23db7-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="23db7-134">Usando i frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="23db7-134">Using the Code Snippets</span></span>

<span data-ttu-id="23db7-135">In tutto il documento di laboratorio, verrà invitati a inserire blocchi di codice.</span><span class="sxs-lookup"><span data-stu-id="23db7-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="23db7-136">Per praticità, la maggior parte di questo codice viene fornito come Visual Studio frammenti di codice che è possibile accedere da all'interno di Visual Studio 2013, per evitare che sia necessario aggiungerla manualmente.</span><span class="sxs-lookup"><span data-stu-id="23db7-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="23db7-137">Ogni esercizio è accompagnata da una soluzione inizia che si trova nel **iniziare** cartella dell'esercizio che consente di seguire ogni esercizio indipendentemente dagli altri.</span><span class="sxs-lookup"><span data-stu-id="23db7-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="23db7-138">Tenere presente che i frammenti di codice aggiunti durante un esercizio non sono presenti queste soluzioni di avvio e potrebbero non funzionare fino a quando non si have completato l'esercizio.</span><span class="sxs-lookup"><span data-stu-id="23db7-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="23db7-139">All'interno del codice sorgente per un esercizio, si noterà anche un **End** cartella che contiene una soluzione di Visual Studio con il codice che scaturisce da completare i passaggi nell'esercizio corrispondente.</span><span class="sxs-lookup"><span data-stu-id="23db7-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="23db7-140">È possibile usare queste soluzioni come prassi consigliata se ti serve assistenza aggiuntiva durante l'esecuzione in questo laboratorio pratico.</span><span class="sxs-lookup"><span data-stu-id="23db7-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>

---

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="23db7-141">Esercizi</span><span class="sxs-lookup"><span data-stu-id="23db7-141">Exercises</span></span>

<span data-ttu-id="23db7-142">Questo laboratorio pratico include gli esercizi seguenti:</span><span class="sxs-lookup"><span data-stu-id="23db7-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="23db7-143">Uso di Browser Link e Web Essentials</span><span class="sxs-lookup"><span data-stu-id="23db7-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="23db7-144">Sfruttando i vantaggi di IntelliSense e frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="23db7-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="23db7-145">Al primo avvio di Visual Studio, è necessario selezionare una delle raccolte di impostazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="23db7-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="23db7-146">Ogni raccolta predefinita è pensata per associare un particolare stile di sviluppo e determina i layout delle finestre, il comportamento dell'editor, frammenti di codice IntelliSense e opzioni della finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="23db7-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="23db7-147">Le procedure descritte in questa esercitazione vengono descritte le azioni necessarie per eseguire una determinata attività in Visual Studio quando si usa la **delle impostazioni di sviluppo generale** raccolta.</span><span class="sxs-lookup"><span data-stu-id="23db7-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="23db7-148">Se si sceglie una raccolta di impostazioni diverse per l'ambiente di sviluppo, possono essere presenti differenze nei passaggi che è necessario tenere conto.</span><span class="sxs-lookup"><span data-stu-id="23db7-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="23db7-149">Esercizio 1: Uso di Browser Link e Web Essentials</span><span class="sxs-lookup"><span data-stu-id="23db7-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="23db7-150">**Web Essentials** è un'estensione di Visual Studio che aggiunge numerose funzionalità utili per lo sviluppo web moderno, principalmente incentrato sul rendere l'esperienza di sviluppo web molto più veloce e più piacevole.</span><span class="sxs-lookup"><span data-stu-id="23db7-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="23db7-151">È possibile installare Web Essentials dalla raccolta di estensioni in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="23db7-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="23db7-152">**Collegamento del browser** è una nuova funzionalità inclusa in Visual Studio 2013 che fornisce un canale tra l'IDE di Visual Studio e qualsiasi browser aperto per lo scambio di dati tra l'applicazione web e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="23db7-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="23db7-153">Web Essentials estende il collegamento del Browser con gli strumenti per modificare il modello a oggetti DOM e gli stili CSS delle pagine web direttamente dal browser.</span><span class="sxs-lookup"><span data-stu-id="23db7-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="23db7-154">In questo esercizio, verrà illustrato alcune delle funzionalità supportate da **Web Essentials** e **Browser Link** per migliorare una pagina semplice quiz.</span><span class="sxs-lookup"><span data-stu-id="23db7-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="23db7-155">Attività 1: esecuzione del progetto in più browser</span><span class="sxs-lookup"><span data-stu-id="23db7-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="23db7-156">In questa attività si configurerà l'applicazione web per l'esecuzione in più browser in una sola volta, questo è utile per i test tra browser.</span><span class="sxs-lookup"><span data-stu-id="23db7-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="23db7-157">Aprire **Microsoft Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="23db7-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="23db7-158">Nel **File** dal menu **Open | Progetto/soluzione...**  e passare a **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** nel **origine** cartella del lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span><span class="sxs-lookup"><span data-stu-id="23db7-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="23db7-159">Selezionare **Begin.sln** e fare clic su **Open**.</span><span class="sxs-lookup"><span data-stu-id="23db7-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="23db7-160">Sulla barra degli strumenti di Visual Studio, espandere il menu di scelta del browser e selezionare **Esplora con...** .</span><span class="sxs-lookup"><span data-stu-id="23db7-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="23db7-161">![Esplora con opzione di menu](visual-studio-2013-web-tools/_static/image1.png "Sfoglia con... nel menu browser")</span><span class="sxs-lookup"><span data-stu-id="23db7-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="23db7-162">*Esplora con opzione di menu*</span><span class="sxs-lookup"><span data-stu-id="23db7-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="23db7-163">Nel **Esplora con** finestra di dialogo, selezionare **Google Chrome** e **Internet Explorer** tenendo premuto il **CTRL** della chiave e fare clic su  **Imposta come predefinito**.</span><span class="sxs-lookup"><span data-stu-id="23db7-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="23db7-164">![Esplorare la finestra di dialogo](visual-studio-2013-web-tools/_static/image2.png "Sfoglia con la finestra di dialogo")</span><span class="sxs-lookup"><span data-stu-id="23db7-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="23db7-165">*Selezione di più browser predefiniti*</span><span class="sxs-lookup"><span data-stu-id="23db7-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="23db7-166">Google Chrome sia Internet Explorer deve essere visualizzata come browser predefinito.</span><span class="sxs-lookup"><span data-stu-id="23db7-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="23db7-167">Fare clic su **annullare** per chiudere la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="23db7-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="23db7-168">![Google Chrome e Internet Explorer come browser predefiniti](visual-studio-2013-web-tools/_static/image3.png "Internet Explorer e Google Chrome browser predefiniti")</span><span class="sxs-lookup"><span data-stu-id="23db7-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="23db7-169">*Google Chrome e Internet Explorer come browser predefiniti*</span><span class="sxs-lookup"><span data-stu-id="23db7-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="23db7-170">Dopo aver configurato il browser predefinito, il **più browser** opzione è selezionata nel menu di scelta del browser.</span><span class="sxs-lookup"><span data-stu-id="23db7-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="23db7-171">![Più browser](visual-studio-2013-web-tools/_static/image4.png "più browser")</span><span class="sxs-lookup"><span data-stu-id="23db7-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="23db7-172">Premere **CTRL** + **F5** per eseguire l'applicazione senza eseguire il debug.</span><span class="sxs-lookup"><span data-stu-id="23db7-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="23db7-173">Quando entrambe le finestre del browser aprono, posizionati uno sopra l'altro per poterlo visualizzare gli aggiornamenti in entrambi i browser contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="23db7-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="23db7-174">Il browser dovrebbe visualizzare una pagina web con un rettangolo blu chiaro.</span><span class="sxs-lookup"><span data-stu-id="23db7-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="23db7-175">![Inserimento di un browser sopra l'altro](visual-studio-2013-web-tools/_static/image5.png "inserimento uno browser sopra l'altro")</span><span class="sxs-lookup"><span data-stu-id="23db7-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="23db7-176">*Inserimento di un browser sopra l'altro*</span><span class="sxs-lookup"><span data-stu-id="23db7-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="23db7-177">Non chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="23db7-177">Do not close the browsers.</span></span> <span data-ttu-id="23db7-178">Verranno usati nell'attività successiva.</span><span class="sxs-lookup"><span data-stu-id="23db7-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="23db7-179">Attività 2: Zen usando la codifica per creare elementi HTML</span><span class="sxs-lookup"><span data-stu-id="23db7-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="23db7-180">**Scrittura di codice Zen** è un editor di plug-in per HTML ad alta velocità, XML, XSL (o qualsiasi altro formato codice strutturato) codifica e la modifica.</span><span class="sxs-lookup"><span data-stu-id="23db7-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="23db7-181">Alla base di questo plug-in è un motore di abbreviazione potente che consente di espandere le espressioni - simile a selettori CSS - nel codice HTML.</span><span class="sxs-lookup"><span data-stu-id="23db7-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="23db7-182">Il codice Zen rappresenta un modo rapido per scrivere sintassi del selettore di stile HTML tramite un CSS.</span><span class="sxs-lookup"><span data-stu-id="23db7-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="23db7-183">In questo esercizio si userà la funzionalità di codifica Zen fornita da Web Essentials per generare i pulsanti HTML che rappresentano le opzioni della domanda.</span><span class="sxs-lookup"><span data-stu-id="23db7-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="23db7-184">Tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="23db7-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="23db7-185">Aprire il **index. cshtml** file che si trova nel **viste** | **Home** cartella.</span><span class="sxs-lookup"><span data-stu-id="23db7-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="23db7-186">Sostituire il **&lt;!-TODO: aggiungere qui, le opzioni&gt;** commento con il codice seguente e premere **scheda**.</span><span class="sxs-lookup"><span data-stu-id="23db7-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="23db7-187">Il codice deve essere espanso in formato HTML.</span><span class="sxs-lookup"><span data-stu-id="23db7-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="23db7-188">![HTML espanso](visual-studio-2013-web-tools/_static/image6.png "espanso HTML")</span><span class="sxs-lookup"><span data-stu-id="23db7-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="23db7-189">*HTML espanso*</span><span class="sxs-lookup"><span data-stu-id="23db7-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="23db7-190">Per altre informazioni sulla sintassi di codifica Zen, vedere gli argomenti seguenti [articolo](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="23db7-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="23db7-191">Scegliere il **Aggiorna browser collegati** pulsante per aggiornare entrambi i browser.</span><span class="sxs-lookup"><span data-stu-id="23db7-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="23db7-192">![Aggiorna browser collegati](visual-studio-2013-web-tools/_static/image7.png "Aggiorna browser collegati")</span><span class="sxs-lookup"><span data-stu-id="23db7-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="23db7-193">*Aggiorna browser collegati*</span><span class="sxs-lookup"><span data-stu-id="23db7-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="23db7-194">![Internet Explorer - pagina aggiornata con quattro pulsanti](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - pagina aggiornata con quattro pulsanti")</span><span class="sxs-lookup"><span data-stu-id="23db7-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="23db7-195">*Internet Explorer - pagina aggiornata con quattro pulsanti*</span><span class="sxs-lookup"><span data-stu-id="23db7-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="23db7-196">![Google Chrome - pagina aggiornata con quattro pulsanti](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - pagina aggiornata con quattro pulsanti")</span><span class="sxs-lookup"><span data-stu-id="23db7-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="23db7-197">*Google Chrome - pagina aggiornata con quattro pulsanti*</span><span class="sxs-lookup"><span data-stu-id="23db7-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="23db7-198">Tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="23db7-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="23db7-199">Si sono aggiunti i pulsanti alla pagina, ma è necessario aggiungere un modello di domanda.</span><span class="sxs-lookup"><span data-stu-id="23db7-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="23db7-200">A tale scopo, si userà una nuova funzionalità in Web Essentials chiamato **generatore di Lorem Ipsum**.</span><span class="sxs-lookup"><span data-stu-id="23db7-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="23db7-201">Individuare il **div** elemento con la **classe** attributo **front**.</span><span class="sxs-lookup"><span data-stu-id="23db7-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="23db7-202">Aggiungere il codice seguente come primo elemento figlio del **div**, quindi premere **scheda**.</span><span class="sxs-lookup"><span data-stu-id="23db7-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="23db7-203">Il codice deve essere espanso in formato HTML.</span><span class="sxs-lookup"><span data-stu-id="23db7-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="23db7-204">![Generato automaticamente di Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum generato automaticamente")</span><span class="sxs-lookup"><span data-stu-id="23db7-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="23db7-205">*Lorem Ipsum generato automaticamente*</span><span class="sxs-lookup"><span data-stu-id="23db7-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="23db7-206">Come parte della scrittura di codice Zen, è ora possibile generare codice Lorem Ipsum direttamente nell'editor HTML.</span><span class="sxs-lookup"><span data-stu-id="23db7-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="23db7-207">È sufficiente digitare **lorem** e fare clic su **scheda** e un 30 word Lorem Ipsum verrà inserito il testo.</span><span class="sxs-lookup"><span data-stu-id="23db7-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="23db7-208">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="23db7-208">E.g.</span></span> <span data-ttu-id="23db7-209">*lorem10* inserisce 10 parole Lorem Ipsum.</span><span class="sxs-lookup"><span data-stu-id="23db7-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="23db7-210">Si aggiungerà un logo nella parte superiore della domanda con un'altra nuova funzionalità in Web Essentials chiamato **generatore di Lorem Pixel**.</span><span class="sxs-lookup"><span data-stu-id="23db7-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="23db7-211">Aggiungere il codice seguente come primo elemento figlio del **div** elemento con **contenitore** come **classe** valore, quindi premere **scheda**.</span><span class="sxs-lookup"><span data-stu-id="23db7-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="23db7-212">Il codice deve espandere in formato HTML.</span><span class="sxs-lookup"><span data-stu-id="23db7-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="23db7-213">![Generato automaticamente lorem Pixel](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel generato automaticamente")</span><span class="sxs-lookup"><span data-stu-id="23db7-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="23db7-214">*Lorem Pixel generato automaticamente*</span><span class="sxs-lookup"><span data-stu-id="23db7-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="23db7-215">Come parte della scrittura di codice Zen, è anche possibile generare codice Lorem Pixel direttamente nell'editor HTML.</span><span class="sxs-lookup"><span data-stu-id="23db7-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="23db7-216">È sufficiente digitare **pix-200x200-animali** e fare clic su **della scheda** e un **img** verrà inserito il tag con un'immagine di 200x200 dell'animale.</span><span class="sxs-lookup"><span data-stu-id="23db7-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="23db7-217">Per altre informazioni, consultare [Lorem Pixel](http://www.lorempixel.com).</span><span class="sxs-lookup"><span data-stu-id="23db7-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="23db7-218">Scegliere il **Aggiorna browser collegati** pulsante per aggiornare entrambi i browser.</span><span class="sxs-lookup"><span data-stu-id="23db7-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="23db7-219">![Internet Explorer - generato automaticamente immagini e testo](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - generato automaticamente immagini e testo")</span><span class="sxs-lookup"><span data-stu-id="23db7-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="23db7-220">*Internet Explorer - generato automaticamente immagini e testo*</span><span class="sxs-lookup"><span data-stu-id="23db7-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="23db7-221">![Google Chrome - generato automaticamente immagini e testo](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - generato automaticamente immagini e testo")</span><span class="sxs-lookup"><span data-stu-id="23db7-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="23db7-222">*Google Chrome - generato automaticamente immagini e testo*</span><span class="sxs-lookup"><span data-stu-id="23db7-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="23db7-223">L'immagine viene selezionato casualmente quando si aggiunge il frammento di codice, l'immagine visualizzata nel browser può differire.</span><span class="sxs-lookup"><span data-stu-id="23db7-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="23db7-224">Non chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="23db7-224">Do not close the browsers.</span></span> <span data-ttu-id="23db7-225">Verranno usati nell'attività successiva.</span><span class="sxs-lookup"><span data-stu-id="23db7-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="23db7-226">Attività 3: aggiornamento di una proprietà di stile</span><span class="sxs-lookup"><span data-stu-id="23db7-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="23db7-227">In questa attività si userà il collegamento Browser **ispezionare modalità** funzionalità per rilevare la posizione esatta in cui viene generato l'elemento DOM specifico e quindi aggiornare la proprietà color di quell'elemento con un selettore di colore fornito dal Web Essentials.</span><span class="sxs-lookup"><span data-stu-id="23db7-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="23db7-228">Nel browser Internet Explorer, premere **CTRL** + **ALT** + **ho** per abilitare la modalità di ispezionare.</span><span class="sxs-lookup"><span data-stu-id="23db7-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="23db7-229">Spostare il puntatore sul bordo blu chiaro e fare clic su.</span><span class="sxs-lookup"><span data-stu-id="23db7-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="23db7-230">![Spostare il puntatore del mouse sul bordo blu chiaro](visual-studio-2013-web-tools/_static/image14.png "spostando il puntatore sul bordo blu chiaro")</span><span class="sxs-lookup"><span data-stu-id="23db7-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="23db7-231">*Spostare il puntatore del mouse sul bordo blu chiaro*</span><span class="sxs-lookup"><span data-stu-id="23db7-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="23db7-232">Tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="23db7-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="23db7-233">Si noti come l'elemento HTML selezionato nel browser viene anche selezionato nell'editor HTML di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="23db7-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="23db7-234">![Elemento HTML selezionato nell'editor di Visual Studio HTML](visual-studio-2013-web-tools/_static/image15.png "elemento HTML selezionato nell'editor HTML di Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="23db7-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="23db7-235">*Elemento HTML selezionato nell'editor HTML di Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="23db7-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="23db7-236">A questo punto si aggiornerà il **front** classe CSS per modificare lo stile dell'elemento selezionato.</span><span class="sxs-lookup"><span data-stu-id="23db7-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="23db7-237">A tale scopo, premere **CTRL** + **,** per aprire la **passa a** casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="23db7-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="23db7-238">Tipo di **CSS** , quindi premere **invio** per aprire il file.</span><span class="sxs-lookup"><span data-stu-id="23db7-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="23db7-239">![Apertura file Site. CSS](visual-studio-2013-web-tools/_static/image16.png "apertura del file CSS")</span><span class="sxs-lookup"><span data-stu-id="23db7-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="23db7-240">*Apertura del file CSS*</span><span class="sxs-lookup"><span data-stu-id="23db7-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="23db7-241">Premere **CTRL** + **F** e il tipo **.flip-container .front** per trovare il selettore CSS.</span><span class="sxs-lookup"><span data-stu-id="23db7-241">Press **CTRL** + **F** and type **.flip-container .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="23db7-242">Fare clic sul quadrato blu chiaro nella proprietà del bordo della classe per aprire la selezione colori.</span><span class="sxs-lookup"><span data-stu-id="23db7-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="23db7-243">![Aprire la selezione colori](visual-studio-2013-web-tools/_static/image17.png "aprendo la selezione colori")</span><span class="sxs-lookup"><span data-stu-id="23db7-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="23db7-244">*Aprire la selezione colori*</span><span class="sxs-lookup"><span data-stu-id="23db7-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="23db7-245">Espandere la selezione colori facendo clic sul pulsante freccia di espansione e selezionare un nuovo colore.</span><span class="sxs-lookup"><span data-stu-id="23db7-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="23db7-246">![Espandere la selezione colori](visual-studio-2013-web-tools/_static/image18.png "espandendo la selezione colori")</span><span class="sxs-lookup"><span data-stu-id="23db7-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="23db7-247">*Espandere la selezione colori*</span><span class="sxs-lookup"><span data-stu-id="23db7-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="23db7-248">Premere **CTRL** + **ALT** + **invio** per aggiornare i browser collegati.</span><span class="sxs-lookup"><span data-stu-id="23db7-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="23db7-249">Passare a Internet Explorer e notare come è stato modificato il colore del bordo.</span><span class="sxs-lookup"><span data-stu-id="23db7-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="23db7-250">![Internet Explorer - colore del bordo aggiornato](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - colore del bordo aggiornato")</span><span class="sxs-lookup"><span data-stu-id="23db7-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="23db7-251">*Internet Explorer - colore del bordo aggiornato*</span><span class="sxs-lookup"><span data-stu-id="23db7-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="23db7-252">Passare a Google Chrome e notare come è stato modificato il colore del bordo.</span><span class="sxs-lookup"><span data-stu-id="23db7-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="23db7-253">![Google Chrome: colore del bordo aggiornato](visual-studio-2013-web-tools/_static/image20.png "Google Chrome: colore del bordo aggiornato")</span><span class="sxs-lookup"><span data-stu-id="23db7-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="23db7-254">*Google Chrome: colore del bordo aggiornato*</span><span class="sxs-lookup"><span data-stu-id="23db7-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="23db7-255">Tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="23db7-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="23db7-256">Passare alla fine della **CSS** file e premere **CTRL** + **F** per individuare il **.btn** selettore.</span><span class="sxs-lookup"><span data-stu-id="23db7-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="23db7-257">Si noti che il **- webkit-border-radius** proprietà sottolineata in verde.</span><span class="sxs-lookup"><span data-stu-id="23db7-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="23db7-258">![proprietà - webkit-border-radius del selettore btn](visual-studio-2013-web-tools/_static/image21.png "proprietà - webkit-border-radius del selettore btn")</span><span class="sxs-lookup"><span data-stu-id="23db7-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="23db7-259">*proprietà - webkit-border-radius del selettore btn*</span><span class="sxs-lookup"><span data-stu-id="23db7-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="23db7-260">Posizionare il cursore nel **- webkit-border-radius** proprietà.</span><span class="sxs-lookup"><span data-stu-id="23db7-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="23db7-261">Una linea blu verrà visualizzato sotto la prima lettera della parola prima della proprietà.</span><span class="sxs-lookup"><span data-stu-id="23db7-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="23db7-262">Questo è il **smart tag**.</span><span class="sxs-lookup"><span data-stu-id="23db7-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="23db7-263">Premere **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="23db7-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="23db7-264">Aprire il menu di suggerimenti e fare clic su **manca una proprietà standard (raggio bordo) Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="23db7-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="23db7-265">![Componente mancante suggerimento di proprietà standard](visual-studio-2013-web-tools/_static/image22.png "componente mancante suggerimento di proprietà standard")</span><span class="sxs-lookup"><span data-stu-id="23db7-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="23db7-266">*Aggiungi mancanti suggerimento di proprietà standard*</span><span class="sxs-lookup"><span data-stu-id="23db7-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="23db7-267">Il **border-radius** proprietà viene aggiunta automaticamente alla regola CSS.</span><span class="sxs-lookup"><span data-stu-id="23db7-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="23db7-268">![Manca una proprietà standard aggiunta](visual-studio-2013-web-tools/_static/image23.png "manca una proprietà standard aggiunta")</span><span class="sxs-lookup"><span data-stu-id="23db7-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="23db7-269">*Manca una proprietà standard aggiunta*</span><span class="sxs-lookup"><span data-stu-id="23db7-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="23db7-270">Spostare il puntatore sul **border-radius** proprietà per visualizzare i **Browser matrice tooltip**.</span><span class="sxs-lookup"><span data-stu-id="23db7-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="23db7-271">Il **Browser matrice tooltip** Mostra la disponibilità della proprietà in ogni browser.</span><span class="sxs-lookup"><span data-stu-id="23db7-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="23db7-272">![Browser matrice tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrice tooltip")</span><span class="sxs-lookup"><span data-stu-id="23db7-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="23db7-273">*Descrizione comando matrice browser*</span><span class="sxs-lookup"><span data-stu-id="23db7-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="23db7-274">Si noti che il valore della **border-radius** proprietà è ancora sottolineato.</span><span class="sxs-lookup"><span data-stu-id="23db7-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="23db7-275">Spostare il puntatore sul valore per visualizzare il messaggio di avviso.</span><span class="sxs-lookup"><span data-stu-id="23db7-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="23db7-276">![Avviso di valore di proprietà Border-radius](visual-studio-2013-web-tools/_static/image25.png "avviso valore di proprietà Border-radius")</span><span class="sxs-lookup"><span data-stu-id="23db7-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="23db7-277">*Avviso di valore di proprietà Border-radius*</span><span class="sxs-lookup"><span data-stu-id="23db7-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="23db7-278">Rimuovere l'unità dei **border-radius** valore della proprietà come indicato dalla descrizione comando.</span><span class="sxs-lookup"><span data-stu-id="23db7-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="23db7-279">Come **border-radius** è la proprietà standard per la definizione di bordo arrotondato come sono angoli, è possibile rimuovere il **- webkit-border-radius** proprietà e il valore in base alla regola CSS.</span><span class="sxs-lookup"><span data-stu-id="23db7-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="23db7-280">Posizionare il cursore nel **ritorno a capo automatico** proprietà e si noti che lo smart tag viene visualizzato anche sotto.</span><span class="sxs-lookup"><span data-stu-id="23db7-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="23db7-281">Aprire il menu di scelta e fare clic su **aggiungere le specifiche di fornitore mancanti**.</span><span class="sxs-lookup"><span data-stu-id="23db7-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="23db7-282">![Aggiungere mancante fornitore specifiche suggerimento](visual-studio-2013-web-tools/_static/image26.png "aggiungere mancante fornitore specifiche suggerimento")</span><span class="sxs-lookup"><span data-stu-id="23db7-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="23db7-283">*Aggiungere mancante fornitore specifiche suggerimento*</span><span class="sxs-lookup"><span data-stu-id="23db7-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="23db7-284">Il **-ms-ritorno a capo automatico** proprietà viene aggiunta automaticamente alla regola CSS.</span><span class="sxs-lookup"><span data-stu-id="23db7-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="23db7-285">![Aggiunta la proprietà specifica fornitore](visual-studio-2013-web-tools/_static/image27.png "proprietà specifico fornitore aggiunta")</span><span class="sxs-lookup"><span data-stu-id="23db7-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="23db7-286">*Proprietà specifico fornitore aggiunta*</span><span class="sxs-lookup"><span data-stu-id="23db7-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="23db7-287">Attività 4: aggiornamento del codice HTML dal Browser</span><span class="sxs-lookup"><span data-stu-id="23db7-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="23db7-288">In questa attività si userà il collegamento Browser **modalità di progettazione** questa funzionalità per modificare l'oggetto DOM dal browser e trasferire le modifiche al file di origine HTML in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="23db7-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="23db7-289">In Google Chrome, premere **CTRL** + **ALT** + **1!d** per abilitare la modalità di progettazione.</span><span class="sxs-lookup"><span data-stu-id="23db7-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="23db7-290">Spostare il puntatore sul **Lorem Ipsum dolor sit amet** assegnare un'etichetta e fare clic su.</span><span class="sxs-lookup"><span data-stu-id="23db7-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="23db7-291">![Modifica la domanda](visual-studio-2013-web-tools/_static/image28.png "modifica la domanda")</span><span class="sxs-lookup"><span data-stu-id="23db7-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="23db7-292">*Modifica la domanda*</span><span class="sxs-lookup"><span data-stu-id="23db7-292">*Editing the question*</span></span>
3. <span data-ttu-id="23db7-293">Dovrebbe essere visualizzato un cursore.</span><span class="sxs-lookup"><span data-stu-id="23db7-293">A cursor should appear.</span></span> <span data-ttu-id="23db7-294">Sostituire il testo originale con *ciò che simile quando scrive una domanda più lungo?*, quindi premere **ESC** per uscire dalla modalità progettazione.</span><span class="sxs-lookup"><span data-stu-id="23db7-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="23db7-295">![Domanda modificato](visual-studio-2013-web-tools/_static/image29.png "domanda modificato")</span><span class="sxs-lookup"><span data-stu-id="23db7-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="23db7-296">*Domanda modificato*</span><span class="sxs-lookup"><span data-stu-id="23db7-296">*Question edited*</span></span>
4. <span data-ttu-id="23db7-297">Commutatore torna a Visual Studio e aprire **index. cshtml**, se non è già aperto.</span><span class="sxs-lookup"><span data-stu-id="23db7-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="23db7-298">Si noti che il testo interno del **&lt;p&gt;** elemento è stato aggiornato.</span><span class="sxs-lookup"><span data-stu-id="23db7-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="23db7-299">![Domanda aggiornate nella pagina HTML](visual-studio-2013-web-tools/_static/image30.png "domanda aggiornate nella pagina HTML")</span><span class="sxs-lookup"><span data-stu-id="23db7-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="23db7-300">*Domanda aggiornato nella pagina HTML*</span><span class="sxs-lookup"><span data-stu-id="23db7-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="23db7-301">Attività 5: revisione SEO avvisi correlati</span><span class="sxs-lookup"><span data-stu-id="23db7-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="23db7-302">**Ottimizzazione motore di ricerca** (SEO) è il processo di creazione di una classificazione di sito Web successiva nell'elenco dei motori di ricerca di risultati.</span><span class="sxs-lookup"><span data-stu-id="23db7-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="23db7-303">Maggiore sarà il sito classifica e in modo più coerente è elencato, i visitatori ulteriori otterrà il sito da tale motore di ricerca.</span><span class="sxs-lookup"><span data-stu-id="23db7-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="23db7-304">Web Essentials include uno strumento di analisi che esamina l'HTML, i report i problemi trovati e offre assistenza per risolverli.</span><span class="sxs-lookup"><span data-stu-id="23db7-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="23db7-305">Passare al **visualizzazione** dal menu **elenco errori** per aprire il **elenco errori** finestra.</span><span class="sxs-lookup"><span data-stu-id="23db7-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="23db7-306">![Errore nella visualizzazione elenco dal menu](visual-studio-2013-web-tools/_static/image31.png "elenco errori nel menu Visualizza")</span><span class="sxs-lookup"><span data-stu-id="23db7-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="23db7-307">*Menu elenco nella visualizzazione errore*</span><span class="sxs-lookup"><span data-stu-id="23db7-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="23db7-308">Si noti che vi sia un avviso di SEO che informa che un **&lt;meta&gt;** tag per la descrizione della pagina è manca.</span><span class="sxs-lookup"><span data-stu-id="23db7-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="23db7-309">Fare doppio clic la voce di avviso SEO per risolvere il problema.</span><span class="sxs-lookup"><span data-stu-id="23db7-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="23db7-310">![Finestra Elenco errori](visual-studio-2013-web-tools/_static/image32.png "finestra Elenco errori")</span><span class="sxs-lookup"><span data-stu-id="23db7-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="23db7-311">*Finestra Elenco errori*</span><span class="sxs-lookup"><span data-stu-id="23db7-311">*Error List window*</span></span>
3. <span data-ttu-id="23db7-312">Nel **Web Essentials** della finestra di dialogo fare clic su **Yes** inserire una descrizione &lt;meta&gt; tag.</span><span class="sxs-lookup"><span data-stu-id="23db7-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="23db7-313">![Finestra di dialogo Web Essentials](visual-studio-2013-web-tools/_static/image33.png "nella finestra di dialogo Web Essentials")</span><span class="sxs-lookup"><span data-stu-id="23db7-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="23db7-314">*Finestra di dialogo Web Essentials*</span><span class="sxs-lookup"><span data-stu-id="23db7-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="23db7-315">L'editor per  **\_layout. cshtml** apre e il **&lt;meta&gt;** tag viene aggiunto automaticamente al **head** sezione del File HTML.</span><span class="sxs-lookup"><span data-stu-id="23db7-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="23db7-316">![Tag Meta aggiunti automaticamente nella pagina layout](visual-studio-2013-web-tools/_static/image34.png "Meta tag aggiunti automaticamente nella pagina layout")</span><span class="sxs-lookup"><span data-stu-id="23db7-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="23db7-317">*Tag Meta aggiunto automaticamente a \_pagina Layout*</span><span class="sxs-lookup"><span data-stu-id="23db7-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="23db7-318">Modificare il valore della **contenuti** dell'attributo *GeekQuiz* e salvare il file.</span><span class="sxs-lookup"><span data-stu-id="23db7-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="23db7-319">Esercizio 2: Sfruttando i vantaggi di IntelliSense e frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="23db7-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="23db7-320">Web Essentials, l'editor HTML è stato esteso con funzionalità aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="23db7-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="23db7-321">In questo esercizio, si noterà alcune nuove funzionalità che sono utili durante lo sviluppo di applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="23db7-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="23db7-322">Attività 1: uso di IntelliSense nei documenti HTML</span><span class="sxs-lookup"><span data-stu-id="23db7-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="23db7-323">La prima nuova funzionalità verrà visualizzato in questa attività viene chiamata **IntelliSense dinamica**.</span><span class="sxs-lookup"><span data-stu-id="23db7-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="23db7-324">IntelliSense dinamica legge altri tag e attributi di dedurre gli ID possibili che si userà.</span><span class="sxs-lookup"><span data-stu-id="23db7-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="23db7-325">In questa attività si creerà un nuovo elemento di form HTML che contiene un'etichetta e un campo di input.</span><span class="sxs-lookup"><span data-stu-id="23db7-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="23db7-326">Aggiungerà un **per** attributo per l'etichetta per associarlo all'input, si noterà i suggerimenti di IntelliSense basati agli ID degli input nell'ambito.</span><span class="sxs-lookup"><span data-stu-id="23db7-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="23db7-327">Aprire **Visual Studio Express 2013 per il Web** e il **Begin.sln** soluzione che si trova nel **inizio/origine/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense** cartella.</span><span class="sxs-lookup"><span data-stu-id="23db7-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="23db7-328">In alternativa, è possibile continuare con la soluzione ottenuta nell'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="23db7-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="23db7-329">Nelle **Esplora soluzioni**, aprire il **index. cshtml** file che si trova nel **viste** | **Home** cartella.</span><span class="sxs-lookup"><span data-stu-id="23db7-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="23db7-330">Aggiungere il seguente formato all'interno di **&lt;sezione&gt;** elemento.</span><span class="sxs-lookup"><span data-stu-id="23db7-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="23db7-331">(Code - Snippet *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span><span class="sxs-lookup"><span data-stu-id="23db7-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="23db7-332">Il tag di input deve essere preceduto da un'etichetta con una descrizione del campo.</span><span class="sxs-lookup"><span data-stu-id="23db7-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="23db7-333">Aggiungere l'etichetta seguente prima del tag di input.</span><span class="sxs-lookup"><span data-stu-id="23db7-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="23db7-334">Il **per** attributo di un **&lt;etichetta&gt;** specifica quale elemento del form un'etichetta è associato.</span><span class="sxs-lookup"><span data-stu-id="23db7-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="23db7-335">Il valore dell'attributo deve essere uguale all'id dell'elemento correlato.</span><span class="sxs-lookup"><span data-stu-id="23db7-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="23db7-336">Aggiungere il **per** attributo il **&lt;etichetta&gt;** elemento.</span><span class="sxs-lookup"><span data-stu-id="23db7-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="23db7-337">Come illustrato nella figura seguente, il &quot;name&quot; valore viene visualizzata nella finestra di IntelliSense, in base all'id degli elementi all'interno dell'ambito stesso (che li racchiude  **&lt;form&gt;**).</span><span class="sxs-lookup"><span data-stu-id="23db7-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="23db7-338">![Che mostra l'id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "che mostra l'id in IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="23db7-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="23db7-339">*Che mostra l'id in IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="23db7-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="23db7-340">Eliminare le aggiunte di recente **&lt;form&gt;** elemento e il relativo contenuto.</span><span class="sxs-lookup"><span data-stu-id="23db7-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="23db7-341">Attività 2 - con frammenti di codice HTML</span><span class="sxs-lookup"><span data-stu-id="23db7-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="23db7-342">HTML5 introdotte più di 25 nuovi tag semantici.</span><span class="sxs-lookup"><span data-stu-id="23db7-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="23db7-343">Visual Studio ha già il supporto IntelliSense per questi tag, ma Visual Studio 2013 rendono più veloce e più semplici da scrivere markup mediante l'aggiunta di nuovi frammenti di codice.</span><span class="sxs-lookup"><span data-stu-id="23db7-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="23db7-344">Anche se questi tag non sono complicati, presentano alcune sottigliezze piccole, ad esempio aggiungendo i fallback codec corretto per il *audio* tag.</span><span class="sxs-lookup"><span data-stu-id="23db7-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="23db7-345">In questa attività, verranno visualizzati i frammenti di codice HTML per i tag audio.</span><span class="sxs-lookup"><span data-stu-id="23db7-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="23db7-346">Nel **index. cshtml** file, digitare  **&lt;aud** all'interno di **&lt;sezione&gt;** elemento, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="23db7-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="23db7-347">![Inserimento di un elemento audio](visual-studio-2013-web-tools/_static/image36.png "inserimento di un elemento audio")</span><span class="sxs-lookup"><span data-stu-id="23db7-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="23db7-348">*Inserimento di un elemento audio*</span><span class="sxs-lookup"><span data-stu-id="23db7-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="23db7-349">Premere **della scheda** due volte e notare come il codice seguente viene aggiunta nella pagina e il cursore viene posizionato sulle **src** attributo della prima origine.</span><span class="sxs-lookup"><span data-stu-id="23db7-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="23db7-350">Premendo il **scheda** chiave due volte, viene inserito il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="23db7-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="23db7-351">Il frammento di audio Mostra l'utilizzo standard delle *audio* tag, con due file di origine per il supporto migliorato.</span><span class="sxs-lookup"><span data-stu-id="23db7-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="23db7-352">Eliminare la seconda riga e aggiornare l'origine della prima riga con il collegamento seguente per la presentazione WebCampsTV Katana: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span><span class="sxs-lookup"><span data-stu-id="23db7-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="23db7-353">Seguito è riportato il codice risultante.</span><span class="sxs-lookup"><span data-stu-id="23db7-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="23db7-354">Il file di origine *KatanaProject.mp3* viene usato come esempio.</span><span class="sxs-lookup"><span data-stu-id="23db7-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="23db7-355">Se si preferisce, è possibile usare un'altra origine.</span><span class="sxs-lookup"><span data-stu-id="23db7-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="23db7-356">Premere **CTRL** + **S** per salvare il file.</span><span class="sxs-lookup"><span data-stu-id="23db7-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="23db7-357">Premere **CTRL** + **F5** per avviare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="23db7-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="23db7-358">Si noti che l'applicazione è stato aggiunto un lettore audio.</span><span class="sxs-lookup"><span data-stu-id="23db7-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="23db7-359">![Lettore audio in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "lettore Audio in Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="23db7-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="23db7-360">*Lettore audio in Internet Explorer*</span><span class="sxs-lookup"><span data-stu-id="23db7-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="23db7-361">![Lettore audio in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "lettore Audio in Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="23db7-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="23db7-362">*Lettore audio in Google Chrome*</span><span class="sxs-lookup"><span data-stu-id="23db7-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="23db7-363">Non chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="23db7-363">Do not close the browsers.</span></span> <span data-ttu-id="23db7-364">Verranno usati nell'attività successiva.</span><span class="sxs-lookup"><span data-stu-id="23db7-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="23db7-365">Attività 3: uso di IntelliSense in JavaScript documenti</span><span class="sxs-lookup"><span data-stu-id="23db7-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="23db7-366">Web Essentials 2013, i fogli di stile e pagine HTML producono un elenco di ID e i nomi di classe.</span><span class="sxs-lookup"><span data-stu-id="23db7-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="23db7-367">In questa attività si apprenderà come tali elenchi migliorano il supporto IntelliSense per JavaScript in Web Essentials 2013.</span><span class="sxs-lookup"><span data-stu-id="23db7-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="23db7-368">Nel **index. cshtml** , aggiungere il codice seguente per definire una **script** tag per il codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="23db7-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="23db7-369">Aggiungere il codice seguente all'interno di **script** tag per definire la funzione di callback pronto.</span><span class="sxs-lookup"><span data-stu-id="23db7-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="23db7-370">(Code - Snippet *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="23db7-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="23db7-371">Posizionare il cursore nel **script** tag e premere **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="23db7-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="23db7-372">Per aprire il menu di suggerimento.</span><span class="sxs-lookup"><span data-stu-id="23db7-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="23db7-373">Fare clic su **estrarre in File**.</span><span class="sxs-lookup"><span data-stu-id="23db7-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="23db7-374">![Estrarre suggerimento file JavaScript](visual-studio-2013-web-tools/_static/image39.png "JavaScript Estrai nel suggerimento di file")</span><span class="sxs-lookup"><span data-stu-id="23db7-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="23db7-375">*JavaScript Estrai nel suggerimento di file*</span><span class="sxs-lookup"><span data-stu-id="23db7-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="23db7-376">Nel **Salva con nome** finestra, seleziona la **script** cartella, nome del file **init.js** e fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="23db7-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="23db7-377">![Salva con nome finestra](visual-studio-2013-web-tools/_static/image40.png "finestra Salva con nome")</span><span class="sxs-lookup"><span data-stu-id="23db7-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="23db7-378">*Salva con nome finestra*</span><span class="sxs-lookup"><span data-stu-id="23db7-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="23db7-379">Il **init.js** file viene creato e viene spostato il contenuto dello script nel file.</span><span class="sxs-lookup"><span data-stu-id="23db7-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="23db7-380">![File Init.js creato con il contenuto incluso](visual-studio-2013-web-tools/_static/image41.png "Init.js file creato con il contenuto disponibile")</span><span class="sxs-lookup"><span data-stu-id="23db7-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="23db7-381">*File Init.js creato con il contenuto disponibile*</span><span class="sxs-lookup"><span data-stu-id="23db7-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="23db7-382">Aprire il **index. cshtml** del file e verificare che il tag di script è stato sostituito con un riferimento al **init.js** file.</span><span class="sxs-lookup"><span data-stu-id="23db7-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="23db7-383">![Riferimento html Init.js](visual-studio-2013-web-tools/_static/image42.png "riferimento html Init.js")</span><span class="sxs-lookup"><span data-stu-id="23db7-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="23db7-384">*Riferimento html Init.js*</span><span class="sxs-lookup"><span data-stu-id="23db7-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="23db7-385">Andare alla **Esplora soluzioni** e notare che le **init.js** file è stato incluso automaticamente nella soluzione.</span><span class="sxs-lookup"><span data-stu-id="23db7-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="23db7-386">![File Init.js inclusi nella soluzione](visual-studio-2013-web-tools/_static/image43.png "Init.js file inclusi nella soluzione")</span><span class="sxs-lookup"><span data-stu-id="23db7-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="23db7-387">*File Init.js inclusi nella soluzione*</span><span class="sxs-lookup"><span data-stu-id="23db7-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="23db7-388">Tornare al **init.js** file per aggiornare il **pronto** funzione di callback.</span><span class="sxs-lookup"><span data-stu-id="23db7-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="23db7-389">All'interno della definizione di funzione callback che viene passata a *pronti*, aggiungere il codice seguente per ottenere tutti gli elementi da un attributo di classe specifica.</span><span class="sxs-lookup"><span data-stu-id="23db7-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="23db7-390">Premere **CTRL** + **spazio** tra le virgolette all'interno di **getElementsByClassName** chiamata di funzione.</span><span class="sxs-lookup"><span data-stu-id="23db7-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="23db7-391">![Visualizzazione di IntelliSense per la funzione getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "IntelliSense che mostra per la funzione getElementsByClassName")</span><span class="sxs-lookup"><span data-stu-id="23db7-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="23db7-392">*Visualizzazione di IntelliSense per la funzione getElementsByClassName*</span><span class="sxs-lookup"><span data-stu-id="23db7-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="23db7-393">Si noti che IntelliSense mostra le classi definite nei fogli di stile progetto.</span><span class="sxs-lookup"><span data-stu-id="23db7-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="23db7-394">Sostituire la riga che è stato creato con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="23db7-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="23db7-395">Posizionare il cursore dopo **au** all'interno delle virgolette nel **getElementsByTagName** (funzione) e premere **CTRL** + **spazio**.</span><span class="sxs-lookup"><span data-stu-id="23db7-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="23db7-396">![Visualizzazione di IntelliSense per il metodo getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "IntelliSense che mostra il metodo getElementByTagName")</span><span class="sxs-lookup"><span data-stu-id="23db7-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="23db7-397">*Visualizzazione di IntelliSense per il metodo getElementsByTagName*</span><span class="sxs-lookup"><span data-stu-id="23db7-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="23db7-398">Selezionare **&quot;audio&quot;** dall'elenco, quindi premere **invio**.</span><span class="sxs-lookup"><span data-stu-id="23db7-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="23db7-399">Il risultato è illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="23db7-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="23db7-400">![Recupero di elementi Audio](visual-studio-2013-web-tools/_static/image46.png "il recupero degli elementi Audio")</span><span class="sxs-lookup"><span data-stu-id="23db7-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="23db7-401">*Recupero di elementi Audio*</span><span class="sxs-lookup"><span data-stu-id="23db7-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="23db7-402">In **Esplora soluzioni**, fare doppio clic sul **init.js** del file nei **script** cartella e selezionare **file JavaScript Minify** dal **Web Essentials** menu.</span><span class="sxs-lookup"><span data-stu-id="23db7-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="23db7-403">![I file JavaScript di minimizzare](visual-studio-2013-web-tools/_static/image47.png "file Minify JavaScript")</span><span class="sxs-lookup"><span data-stu-id="23db7-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="23db7-404">*Minimizzare i file JavaScript*</span><span class="sxs-lookup"><span data-stu-id="23db7-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="23db7-405">Quando viene richiesto di abilitare la riduzione automatica quando le modifiche ai file di origine fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="23db7-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="23db7-406">![Abilitazione di avviso automatico minimizzazione](visual-studio-2013-web-tools/_static/image48.png "abilitazione avviso automatico minimizzazione")</span><span class="sxs-lookup"><span data-stu-id="23db7-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="23db7-407">*Abilitazione di avviso di minimizzazione automatica*</span><span class="sxs-lookup"><span data-stu-id="23db7-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="23db7-408">Il **init.min.js** viene creato e viene aggiunto come dipendenza per il **init.js** file.</span><span class="sxs-lookup"><span data-stu-id="23db7-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="23db7-409">![File Init.min.js creato](visual-studio-2013-web-tools/_static/image49.png "file Init.min.js creato")</span><span class="sxs-lookup"><span data-stu-id="23db7-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="23db7-410">*File Init.min.js creato*</span><span class="sxs-lookup"><span data-stu-id="23db7-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="23db7-411">Aprire il **init.min.js** file e notare che il file è minimizzato.</span><span class="sxs-lookup"><span data-stu-id="23db7-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="23db7-412">![Contenuto del file Init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js contenuto del file")</span><span class="sxs-lookup"><span data-stu-id="23db7-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="23db7-413">*Contenuto del file Init.min.js*</span><span class="sxs-lookup"><span data-stu-id="23db7-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="23db7-414">Nel **init.js** , aggiungere il codice seguente sotto il **getElementsByTagName** per riprodurre tutti gli elementi audio chiamata di funzione.</span><span class="sxs-lookup"><span data-stu-id="23db7-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="23db7-415">(Code - Snippet *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="23db7-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="23db7-416">Fare clic su **CTRL** + **S** per salvare il file.</span><span class="sxs-lookup"><span data-stu-id="23db7-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="23db7-417">Poiché il file minimizzato è già aperto, si verrà visualizzata una finestra di dialogo che informa che il file è stato modificato all'esterno dell'editor di origine.</span><span class="sxs-lookup"><span data-stu-id="23db7-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="23db7-418">Scegliere **Sì**.</span><span class="sxs-lookup"><span data-stu-id="23db7-418">Click **Yes**.</span></span>

    <span data-ttu-id="23db7-419">![Avviso di Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "avviso di Microsoft Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="23db7-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="23db7-420">*Avviso di Microsoft Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="23db7-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="23db7-421">Tornare alla **init.min.js** file per verificare che il file è stato aggiornato con il nuovo codice.</span><span class="sxs-lookup"><span data-stu-id="23db7-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="23db7-422">![Il file Init.min.js aggiornato](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file aggiornato")</span><span class="sxs-lookup"><span data-stu-id="23db7-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="23db7-423">*File Init.min.js aggiornato*</span><span class="sxs-lookup"><span data-stu-id="23db7-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="23db7-424">Scegliere il **aggiornamento del collegamento Browser** pulsante.</span><span class="sxs-lookup"><span data-stu-id="23db7-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="23db7-425">Dopo che entrambi i browser vengono aggiornati i lettori audio che si è visto nell'attività precedente verranno avviata la riproduzione automatica.</span><span class="sxs-lookup"><span data-stu-id="23db7-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="23db7-426">![Lettore audio incluso nella visualizzazione](visual-studio-2013-web-tools/_static/image53.png "lettore Audio incluso nella visualizzazione")</span><span class="sxs-lookup"><span data-stu-id="23db7-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="23db7-427">*Lettore audio incluso nella visualizzazione*</span><span class="sxs-lookup"><span data-stu-id="23db7-427">*Audio player included in view*</span></span>

---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="23db7-428">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="23db7-428">Summary</span></span>

<span data-ttu-id="23db7-429">Completando questa esercitazione pratica si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="23db7-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="23db7-430">Usare nuove funzionalità dell'editor HTML incluse in Web Essentials, ad esempio i frammenti di codice HTML5 avanzati e codifica Zen</span><span class="sxs-lookup"><span data-stu-id="23db7-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="23db7-431">Usare nuove funzionalità dell'editor CSS incluse in Web Essentials, ad esempio la selezione dei colori e una descrizione comando di matrice di Browser</span><span class="sxs-lookup"><span data-stu-id="23db7-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="23db7-432">Usare nuove funzionalità dell'editor JavaScript inclusa in Web Essentials, ad esempio l'estrazione di File e IntelliSense per tutti gli elementi HTML</span><span class="sxs-lookup"><span data-stu-id="23db7-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="23db7-433">Scambiare i dati tra browser e Visual Studio usando il collegamento del Browser</span><span class="sxs-lookup"><span data-stu-id="23db7-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
