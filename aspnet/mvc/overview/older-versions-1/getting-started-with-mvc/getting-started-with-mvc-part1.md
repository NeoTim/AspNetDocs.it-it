---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introduzione ad ASP.NET MVC | Microsoft Docs
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: dcc2e703829cfa0b77575870feff451fd0738f56
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59416493"
---
# <a name="intro-to-aspnet-mvc"></a><span data-ttu-id="40f0e-104">Introduzione ad ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="40f0e-104">Intro to ASP.NET MVC</span></span>

<span data-ttu-id="40f0e-105">da [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="40f0e-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="40f0e-106">Una versione aggiornata se è disponibile in questa esercitazione [Ecco](../../getting-started/introduction/getting-started.md) utilizzando [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span><span class="sxs-lookup"><span data-stu-id="40f0e-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span></span> <span data-ttu-id="40f0e-107">La nuova esercitazione Usa ASP.NET MVC 5, che offre numerosi miglioramenti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="40f0e-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
>
>
> <span data-ttu-id="40f0e-108">Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="40f0e-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="40f0e-109">Si creerà una semplice applicazione web che legge e scrive da un database.</span><span class="sxs-lookup"><span data-stu-id="40f0e-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="40f0e-110">Visitare il [centro di formazione di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.</span><span class="sxs-lookup"><span data-stu-id="40f0e-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="40f0e-111">Creare la prima applicazione Web MVC ASP.NET utilizzando [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="40f0e-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="40f0e-112">Ci assicureremo una piccola applicazione elenco di film che verrà creato e l'elenco di film invia i tuoi commenti.</span><span class="sxs-lookup"><span data-stu-id="40f0e-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="40f0e-113">Scopo dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="40f0e-113">What You'll Build</span></span>

<span data-ttu-id="40f0e-114">Ecco due schermate dell'applicazione che verrà compilata.</span><span class="sxs-lookup"><span data-stu-id="40f0e-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="40f0e-115">Si avrà una semplice tabella di film con varie colonne.</span><span class="sxs-lookup"><span data-stu-id="40f0e-115">You will have a simple table of movies with various columns.</span></span>

[![M<span data-ttu-id="40f0e-116">ovie List - Windows Internet Explorer (12)]</span><span class="sxs-lookup"><span data-stu-id="40f0e-116">ovie List - Windows Internet Explorer (12)]</span></span>(getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

<span data-ttu-id="40f0e-117">E sarà necessario un modulo creare in modo che possiamo aggiungere all'elenco di film.</span><span class="sxs-lookup"><span data-stu-id="40f0e-117">And you'll have a Create Form so we can add movies to the list.</span></span>

[![C<span data-ttu-id="40f0e-118">Crea un film - Windows Internet Explorer (2)]</span><span class="sxs-lookup"><span data-stu-id="40f0e-118">reate a Movie - Windows Internet Explorer (2)]</span></span>(getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="40f0e-119">Competenze</span><span class="sxs-lookup"><span data-stu-id="40f0e-119">Skills You'll Learn</span></span>

<span data-ttu-id="40f0e-120">Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web ASP.NET MVC con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="40f0e-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="40f0e-121">Si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="40f0e-121">You'll learn:</span></span>

- <span data-ttu-id="40f0e-122">Come creare un nuovo progetto ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="40f0e-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="40f0e-123">Come creare un nuovo Database con SQL Server</span><span class="sxs-lookup"><span data-stu-id="40f0e-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="40f0e-124">Come creare visualizzazioni e controller MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="40f0e-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="40f0e-125">Come recuperare e visualizzare i dati</span><span class="sxs-lookup"><span data-stu-id="40f0e-125">How to retrieve and display data</span></span>
- <span data-ttu-id="40f0e-126">Come modificare i dati e abilitare la convalida dei dati</span><span class="sxs-lookup"><span data-stu-id="40f0e-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="40f0e-127">Come aggiornare lo schema del database</span><span class="sxs-lookup"><span data-stu-id="40f0e-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="40f0e-128">Introduzione</span><span class="sxs-lookup"><span data-stu-id="40f0e-128">Get Started</span></span>

<span data-ttu-id="40f0e-129">Iniziare eseguendo Visual Web Developer 2010 Express (che chiamerò "VWD" nel) e selezionare Nuovo progetto dalla schermata Start.</span><span class="sxs-lookup"><span data-stu-id="40f0e-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="40f0e-130">Visual Web Developer è un IDE, o ambiente di sviluppo integrato.</span><span class="sxs-lookup"><span data-stu-id="40f0e-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="40f0e-131">Come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni.</span><span class="sxs-lookup"><span data-stu-id="40f0e-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="40f0e-132">È presente una barra degli strumenti nella parte superiore che mostra le diverse opzioni disponibili si, nonché i menu potrebbe anche avere utilizzato per selezionare File | Nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="40f0e-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

[![M<span data-ttu-id="40f0e-133">icrosoft Visual Web Developer 2010 Express]</span><span class="sxs-lookup"><span data-stu-id="40f0e-133">icrosoft Visual Web Developer 2010 Express]</span></span>(getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="40f0e-134">Creazione della prima applicazione</span><span class="sxs-lookup"><span data-stu-id="40f0e-134">Creating Your First Application</span></span>

<span data-ttu-id="40f0e-135">È possibile creare applicazioni con Visual Basic o Visual c#.</span><span class="sxs-lookup"><span data-stu-id="40f0e-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="40f0e-136">Per il momento, selezionare Visual c# a sinistra, quindi selezionare "Applicazione Web ASP.NET MVC 2".</span><span class="sxs-lookup"><span data-stu-id="40f0e-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="40f0e-137">Denominare il progetto "Movies" e fare clic su OK.</span><span class="sxs-lookup"><span data-stu-id="40f0e-137">Name your project "Movies" and click OK.</span></span>

[![N<span data-ttu-id="40f0e-138">uovo progetto]</span><span class="sxs-lookup"><span data-stu-id="40f0e-138">ew Project]</span></span>(getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

<span data-ttu-id="40f0e-139">Sul lato destro è di Esplora soluzioni con tutti i file e cartelle all'interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="40f0e-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="40f0e-140">Finestra al centro di big data è possibile modificare il codice e trascorrono la maggior parte del tempo.</span><span class="sxs-lookup"><span data-stu-id="40f0e-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="40f0e-141">Visual Studio usato un modello predefinito per il progetto ASP.NET MVC che appena creato, in modo che sia subito un'applicazione funzionante non esegue alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="40f0e-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="40f0e-142">Si tratta di una semplice "Hello World!</span><span class="sxs-lookup"><span data-stu-id="40f0e-142">This is a simple "Hello World!</span></span> <span data-ttu-id="40f0e-143">progetto che è un buon punto di partenza per la nostra applicazione.</span><span class="sxs-lookup"><span data-stu-id="40f0e-143">project, and it is a good place to start for our application.</span></span>

[![M<span data-ttu-id="40f0e-144">icrosoft Visual Web Developer 2010 Express]</span><span class="sxs-lookup"><span data-stu-id="40f0e-144">icrosoft Visual Web Developer 2010 Express]</span></span>(getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

<span data-ttu-id="40f0e-145">Selezionare il pulsante "play" alla barra degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="40f0e-145">Select the "play" button to the toolbar.</span></span>

![Avvia debug](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="40f0e-147">È una freccia verde rivolta verso destra che verrà compilato il programma e avviare l'applicazione in un web browser.</span><span class="sxs-lookup"><span data-stu-id="40f0e-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

*<span data-ttu-id="40f0e-148">NOTA: È invece possibile premere F5 sulla tastiera o selezionare Debug -&gt;Avvia debug dal menu "Debug".</span><span class="sxs-lookup"><span data-stu-id="40f0e-148">NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.</span></span>*

<span data-ttu-id="40f0e-149">In questo modo Visual Web Developer avviare un server web di sviluppo ed eseguire l'applicazione web (non esistono alcuna configurazione o passaggi manuali necessari per abilitare questa opzione).</span><span class="sxs-lookup"><span data-stu-id="40f0e-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="40f0e-150">Quindi verrà avviato un browser e configurarlo per Sfoglia home page dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="40f0e-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="40f0e-151">Notare che la barra degli indirizzi del browser viene visualizzato "localhost" e non un valore come example.com sotto.</span><span class="sxs-lookup"><span data-stu-id="40f0e-151">Notice below that the address bar of the browser says "localhost", and not something like example.com.</span></span> <span data-ttu-id="40f0e-152">Ciò avviene perché localhost fa sempre riferimento al proprio computer locale - che in questo caso è in esecuzione l'applicazione, che abbiamo appena creato.</span><span class="sxs-lookup"><span data-stu-id="40f0e-152">That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

[![H<span data-ttu-id="40f0e-153">orte pagina]</span><span class="sxs-lookup"><span data-stu-id="40f0e-153">ome Page]</span></span>(getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

<span data-ttu-id="40f0e-154">Impostazione predefinita questo modello predefinito include è due pagine alla visita e una pagina di accesso basic.</span><span class="sxs-lookup"><span data-stu-id="40f0e-154">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="40f0e-155">È possibile modificare il comportamento dell'applicazione e scopri un po' su ASP.NET MVC nel processo.</span><span class="sxs-lookup"><span data-stu-id="40f0e-155">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="40f0e-156">Chiudere il browser e consente di modificare il codice.</span><span class="sxs-lookup"><span data-stu-id="40f0e-156">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="40f0e-157">Successivo</span><span class="sxs-lookup"><span data-stu-id="40f0e-157">Next</span></span>](getting-started-with-mvc-part2.md)
