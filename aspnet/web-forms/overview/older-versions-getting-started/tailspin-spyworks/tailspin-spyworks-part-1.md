---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: Parte 1. File -> Nuovo progetto | Microsoft Docs
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 1 illustra panoramica e File/nuovo progetto.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 70d2efb789d694a0aaecc046615c7b3622079dc1
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59385358"
---
# <a name="part-1-file--new-project"></a><span data-ttu-id="5973b-104">Parte 1. File -> Nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="5973b-104">Part 1: File-> New Project</span></span>

<span data-ttu-id="5973b-105">da [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="5973b-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="5973b-106">Tailspin Spyworks viene illustrato come saliente è davvero semplice per creare applicazioni potenti e scalabili per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="5973b-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="5973b-107">Illustra come usare le nuove funzionalità in ASP.NET 4 per creare un negozio online, tra cui acquisti, estrazione e l'amministrazione.</span><span class="sxs-lookup"><span data-stu-id="5973b-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="5973b-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="5973b-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="5973b-109">Parte 1 illustra panoramica e File/nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="5973b-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="5973b-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="5973b-110">Overview</span></span>

<span data-ttu-id="5973b-111">Questa esercitazione viene fornita un'introduzione a Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5973b-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="5973b-112">Si sarà possibile avviare lentamente, in modo che l'esperienza di sviluppo di livello web per principianti è corretto.</span><span class="sxs-lookup"><span data-stu-id="5973b-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="5973b-113">L'applicazione che sarà compilata si è un semplice negozio online.</span><span class="sxs-lookup"><span data-stu-id="5973b-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="5973b-114">I visitatori possono sfogliare i prodotti per categoria:</span><span class="sxs-lookup"><span data-stu-id="5973b-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="5973b-115">È possibile visualizzare un singolo prodotto e aggiungerlo al carrello acquisti:</span><span class="sxs-lookup"><span data-stu-id="5973b-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="5973b-116">È possibile esaminarne carrello, la rimozione di tutti gli elementi che non sono più necessarie:</span><span class="sxs-lookup"><span data-stu-id="5973b-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="5973b-117">Verrà richiesto loro di procedere con l'acquisto</span><span class="sxs-lookup"><span data-stu-id="5973b-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="5973b-118">Dopo l'ordinamento, visualizzano una schermata di conferma semplice:</span><span class="sxs-lookup"><span data-stu-id="5973b-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="5973b-119">Iniziamo creando un nuovo progetto di Web Form ASP.NET in Visual Studio 2010 e verrà aggiunto in modo incrementale le funzionalità per creare un'applicazione funziona completa.</span><span class="sxs-lookup"><span data-stu-id="5973b-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="5973b-120">Lungo il percorso, si affronterà l'accesso al database, le visualizzazioni elenco e griglia, le pagine di aggiornamento dati, la convalida dei dati, utilizzando le pagine master per layout delle pagine coerente, AJAX, convalida, l'appartenenza degli utenti e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="5973b-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="5973b-121">È possibile seguire la procedura dettagliata, oppure è possibile scaricare l'applicazione completata [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="5973b-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="5973b-122">È possibile usare Visual Studio 2010 o le gratuito Visual Web Developer 2010 dal [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="5973b-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="5973b-123">Per compilare l'applicazione, è possibile usare SQL Server o gratuita SQL Server Express per ospitare il database.</span><span class="sxs-lookup"><span data-stu-id="5973b-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="5973b-124">File / nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="5973b-124">File / New Project</span></span>

<span data-ttu-id="5973b-125">Si inizierà selezionando il nuovo progetto dal menu File in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5973b-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="5973b-126">Verrà visualizzata la finestra di dialogo Nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="5973b-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="5973b-127">Scegliamo Visual c# / modelli Web Raggruppa in base a sinistra e quindi scegliere il modello "Applicazione Web ASP.NET" nella colonna centrale.</span><span class="sxs-lookup"><span data-stu-id="5973b-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="5973b-128">Denominare il progetto TailspinSpyworks e premere il pulsante OK.</span><span class="sxs-lookup"><span data-stu-id="5973b-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="5973b-129">Verrà creato il progetto.</span><span class="sxs-lookup"><span data-stu-id="5973b-129">This will create our project.</span></span> <span data-ttu-id="5973b-130">Diamo un'occhiata le cartelle in cui sono inclusi nell'applicazione in Esplora soluzioni sul lato destro.</span><span class="sxs-lookup"><span data-stu-id="5973b-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="5973b-131">La soluzione vuota non è completamente vuota, che aggiunge una struttura di cartelle di base:</span><span class="sxs-lookup"><span data-stu-id="5973b-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="5973b-132">Notare le convenzioni implementate dal modello di progetto predefiniti di ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="5973b-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="5973b-133">La cartella "Account" implementa un'interfaccia utente di base per ASP. Sottosistema di appartenenza di NET.</span><span class="sxs-lookup"><span data-stu-id="5973b-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="5973b-134">La cartella "Scripts" funge da archivio per i file JavaScript lato client e i file con estensione js jQuery core vengono resi disponibili per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="5973b-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="5973b-135">La cartella "Stili" viene usata per organizzare gli oggetti visivi di sito web (fogli di stile CSS)</span><span class="sxs-lookup"><span data-stu-id="5973b-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="5973b-136">Quando si preme F5 per eseguire l'applicazione e il rendering della pagina default. aspx viene visualizzato quanto segue.</span><span class="sxs-lookup"><span data-stu-id="5973b-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="5973b-137">Il primo miglioramento dell'applicazione sarà quello di sostituire il file Style. CSS del modello Web Form predefinito con i file di immagine associata che verranno eseguito il rendering di asthetics visual che intendiamo per la nostra applicazione Tailspin Spyworks e classi CSS.</span><span class="sxs-lookup"><span data-stu-id="5973b-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="5973b-138">Al termine dell'operazione la pagina default. aspx viene eseguito il rendering come segue.</span><span class="sxs-lookup"><span data-stu-id="5973b-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="5973b-139">Si noti che i collegamenti di immagine nella parte superiore destra della pagina e le voci di menu che sono stati aggiunti alla pagina master.</span><span class="sxs-lookup"><span data-stu-id="5973b-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="5973b-140">Solo i collegamenti "Accedi" e "Account" puntano alle pagine esistenti (generate dal modello predefinito) e il resto delle pagine che verrà implementato poiché noi creiamo la nostra applicazione.</span><span class="sxs-lookup"><span data-stu-id="5973b-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="5973b-141">Eseguiremo, inoltre, per spostare la pagina Master per la directory di stili.</span><span class="sxs-lookup"><span data-stu-id="5973b-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="5973b-142">Anche se questa è solo una preferenza può avere operazioni un po' più semplice se si decide di apportare in futuro "skinable" la nostra applicazione.</span><span class="sxs-lookup"><span data-stu-id="5973b-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="5973b-143">Dopo questa operazione che è necessario modificare la pagina master i riferimenti in tutti i file con estensione aspx generato per l'impostazione predefinita le pagine Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5973b-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5973b-144">Successivo</span><span class="sxs-lookup"><span data-stu-id="5973b-144">Next</span></span>](tailspin-spyworks-part-2.md)
