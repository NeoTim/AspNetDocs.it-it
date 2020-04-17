---
uid: web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programmazione di pagine Web ASP.NET (Razor) tramite Visual Studio Documenti Microsoft
author: Rick-Anderson
description: In questa appendice viene illustrato come utilizzare Visual Studio 2010 o Visual Web Developer 2010 Express per programmare ASP.NET pagine Web con la sintassi Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: e586a116fc48a797bdd40befad5851a548282ce6
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542898"
---
# <a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="75cec-103">Programmazione di pagine Web ASP.NET (Razor) tramite Visual Studio</span><span class="sxs-lookup"><span data-stu-id="75cec-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>

<span data-ttu-id="75cec-104"> di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="75cec-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="75cec-105">In questo articolo viene illustrato come utilizzare Visual Studio o Visual Web Developer Express per programmare ASP.NET siti Web (Razor).</span><span class="sxs-lookup"><span data-stu-id="75cec-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
>
> <span data-ttu-id="75cec-106">Contenuto dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="75cec-106">What you'll learn</span></span>
>
> - <span data-ttu-id="75cec-107">Elementi che è necessario installare (semmai) per lavorare con ASP.NET pagine Web nella versione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75cec-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="75cec-108">Come aggiungere il supporto per ASP.NET pagine Web a Visual Web Developer 2010 Express.</span><span class="sxs-lookup"><span data-stu-id="75cec-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="75cec-109">Come usare le funzionalità di Visual Studio per lavorare con ASP.NET pagine Razor, tra cui IntelliSense e il debugger.</span><span class="sxs-lookup"><span data-stu-id="75cec-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="75cec-110">Versioni software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="75cec-110">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="75cec-111">Pagine Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="75cec-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="75cec-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="75cec-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="75cec-113">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="75cec-113">WebMatrix 3</span></span>
>
>
> <span data-ttu-id="75cec-114">Questa esercitazione funziona anche con ASP.NET pagine Web 2, Visual Studio 2012, Visual Studio 2010 e WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="75cec-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>

<span data-ttu-id="75cec-115">È possibile programmare ASP.NET pagine Web con la sintassi Razor utilizzando WebMatrix o molti altri editor di codice.</span><span class="sxs-lookup"><span data-stu-id="75cec-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="75cec-116">È inoltre possibile utilizzare Microsoft Visual Studio, un ambiente di sviluppo integrato (IDE) completo che fornisce un potente set di strumenti per la creazione di molti tipi di applicazioni (non solo siti Web).</span><span class="sxs-lookup"><span data-stu-id="75cec-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="75cec-117">Per utilizzare ASP.NET pagine Razor, è possibile utilizzare [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span><span class="sxs-lookup"><span data-stu-id="75cec-117">To work with ASP.NET Razor pages, you can use [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span>

<span data-ttu-id="75cec-118">Due funzionalità particolarmente utili che Visual Studio fornisce per la programmazione con ASP.NET pagine Web Razor sono:</span><span class="sxs-lookup"><span data-stu-id="75cec-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="75cec-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="75cec-119">*IntelliSense*.</span></span> <span data-ttu-id="75cec-120">La funzionalità IntelliSense incorporata in Visual Studio è più completa di IntelliSense in WebMatrix.The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="75cec-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="75cec-121">*Debugger*.</span><span class="sxs-lookup"><span data-stu-id="75cec-121">*Debugger*.</span></span> <span data-ttu-id="75cec-122">Il debugger consente di risolvere i problemi del codice arrestando un programma durante l'esecuzione, esaminando le variabili ed eseguendo il codice riga per riga.</span><span class="sxs-lookup"><span data-stu-id="75cec-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="75cec-123">Utilizzo di Visual Studio con versioni diverse di pagine Web di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="75cec-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="75cec-124">Per sviluppare app Web ASP.NET in Visual Studio 2017, installare il carico di lavoro **di sviluppo Web e di ASP.NET.**</span><span class="sxs-lookup"><span data-stu-id="75cec-124">To develop ASP.NET web apps in Visual Studio 2017, install the **ASP.NET and web development** workload.</span></span>

<span data-ttu-id="75cec-125">Visual Studio 2012 e Visual Studio 2013 includono il supporto per le pagine Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="75cec-125">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="75cec-126">I pacchetti necessari per supportare ASP.NET pagine Web vengono installati quando si installa Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75cec-126">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="75cec-127">Visual Studio 2010 non include il supporto per impostazione predefinita per ASP.NET pagine Web.</span><span class="sxs-lookup"><span data-stu-id="75cec-127">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="75cec-128">Per utilizzare ASP.NET pagine Web con Visual Studio 2010, è necessario installare il pacchetto MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="75cec-128">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="75cec-129">Per ottenere ASP.NET pagine Web 2, si installa ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="75cec-129">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="75cec-130">Nella tabella seguente viene riepilogato il supporto per ASP.NET pagine Web in diverse versioni di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75cec-130">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="75cec-131">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="75cec-131">Visual Studio 2010</span></span> | <span data-ttu-id="75cec-132">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="75cec-132">Visual Studio 2012</span></span> | <span data-ttu-id="75cec-133">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="75cec-133">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="75cec-134">**Pagine Web ASP.NET 2**</span><span class="sxs-lookup"><span data-stu-id="75cec-134">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="75cec-135">Installare ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="75cec-135">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="75cec-136">(Incluso)</span><span class="sxs-lookup"><span data-stu-id="75cec-136">(Included)</span></span> | <span data-ttu-id="75cec-137">(Incluso)</span><span class="sxs-lookup"><span data-stu-id="75cec-137">(Included)</span></span> |
| <span data-ttu-id="75cec-138">**Pagine Web ASP.NET 3**</span><span class="sxs-lookup"><span data-stu-id="75cec-138">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="75cec-139">Aggiornamento a pagine Web ASP.NET 3 tramite NuGet</span><span class="sxs-lookup"><span data-stu-id="75cec-139">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="75cec-140">(Incluso)</span><span class="sxs-lookup"><span data-stu-id="75cec-140">(Included)</span></span> |

<span data-ttu-id="75cec-141">Per utilizzare Visual Studio 2010, vedere [Installazione del supporto per ASP.NET pagine Web in Visual Studio 2010](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="75cec-141">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="75cec-142">Avvio di Visual Studio da WebMatrix</span><span class="sxs-lookup"><span data-stu-id="75cec-142">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="75cec-143">Se è stato avviato un progetto in WebMatrix e si desidera passare a Visual Studio, WebMatrix fornisce un pulsante per aprire facilmente il progetto in Visual Studio.If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75cec-143">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="75cec-144">Per abilitare questo pulsante, è necessario che Visual Studio sia installato nel computer.</span><span class="sxs-lookup"><span data-stu-id="75cec-144">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="75cec-145">The following image shows the button in WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="75cec-145">The following image shows the button in WebMatrix.</span></span>

![avviare Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="75cec-147">Quando si fa clic sul pulsante, il progetto viene aperto in Visual Studio.When you click the button, the project is opened in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75cec-147">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="75cec-148">È possibile passare avanti e indietro tra WebMatrix e Visual Studio senza problemi.</span><span class="sxs-lookup"><span data-stu-id="75cec-148">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="75cec-149">Si riceverà una notifica se i file sono stati modificati nell'altro ambiente e devono essere ricaricati per ottenere le modifiche più recenti.</span><span class="sxs-lookup"><span data-stu-id="75cec-149">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="75cec-150">Creazione di ASP.NET Razor Site in Visual StudioCreating a Razor Site in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="75cec-150">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="75cec-151">Per creare un sito Web Razor ASP.NET in Visual Studio:To create an ASP.NET Razor website in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="75cec-151">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="75cec-152">Aprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75cec-152">Open Visual Studio.</span></span>
2. <span data-ttu-id="75cec-153">Scegliere Nuovo sito **Web**dal menu **File** .</span><span class="sxs-lookup"><span data-stu-id="75cec-153">In the **File** menu, click **New Web Site**.</span></span>

    ![creare un nuovo sito Web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="75cec-155">Nella finestra di dialogo **Nuovo sito Web,** selezionare il linguaggio da utilizzare (Visual C.NET o Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="75cec-155">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="75cec-156">Selezionare il modello **ASP.NET sito Web (Razor).**</span><span class="sxs-lookup"><span data-stu-id="75cec-156">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![sito rasoio](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="75cec-158">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="75cec-158">Click **OK**.</span></span>

<span data-ttu-id="75cec-159">Il nuovo progetto esiste e viene popolato con alcune pagine Web predefinite per iniziare.</span><span class="sxs-lookup"><span data-stu-id="75cec-159">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="75cec-160">Using IntelliSense</span><span class="sxs-lookup"><span data-stu-id="75cec-160">Using IntelliSense</span></span>

<span data-ttu-id="75cec-161">Dopo aver creato un sito, è possibile vedere il funzionamento di IntelliSense in Visual Studio.Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75cec-161">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="75cec-162">Nel sito Web appena creato aprire la pagina *Default.cshtml.*</span><span class="sxs-lookup"><span data-stu-id="75cec-162">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="75cec-163">Dopo `<h3>` i tag nella `@ServerInfo.` pagina, digitare (incluso il punto).</span><span class="sxs-lookup"><span data-stu-id="75cec-163">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="75cec-164">Si noti come IntelliSense `ServerInfo` visualizza i metodi disponibili per l'helper in un elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="75cec-164">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span>

    ![Intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="75cec-166">Selezionare `GetHtml` il metodo dall'elenco e quindi premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="75cec-166">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="75cec-167">IntelliSense inserisce automaticamente il metodo .</span><span class="sxs-lookup"><span data-stu-id="75cec-167">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="75cec-168">(Come con qualsiasi metodo in C `()` , è necessario aggiungere caratteri dopo il metodo.) Il codice completato `GetHtml` per il metodo è simile all'esempio seguente:The completed code for the method looks like the following example:</span><span class="sxs-lookup"><span data-stu-id="75cec-168">(As with any method in C#, you must add `()` characters after the method.) The completed code for the `GetHtml` method looks like the following example:</span></span>

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="75cec-169">Premete Ctrl e F5 per eseguire la pagina.</span><span class="sxs-lookup"><span data-stu-id="75cec-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="75cec-170">Questo è l'aspetto della pagina quando viene visualizzata in un browser:</span><span class="sxs-lookup"><span data-stu-id="75cec-170">This is what the page looks like when displayed in a browser:</span></span>

    ![pagina predefinita nel browser](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="75cec-172">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="75cec-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="75cec-173">Utilizzo del debugger</span><span class="sxs-lookup"><span data-stu-id="75cec-173">Using the Debugger</span></span>

1. <span data-ttu-id="75cec-174">Nella parte superiore della pagina *Default.cshtml,* dopo `Page.Title`la riga che inizia con , aggiungere la seguente riga di codice:</span><span class="sxs-lookup"><span data-stu-id="75cec-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span>

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="75cec-175">Nel margine grigio dell'editor a sinistra del codice fare clic accanto a questa nuova riga per aggiungere un *punto di interruzione*.</span><span class="sxs-lookup"><span data-stu-id="75cec-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="75cec-176">Un punto di interruzione è un marcatore che indica al debugger di interrompere l'esecuzione del programma in quel punto in modo da poter vedere cosa sta succedendo.</span><span class="sxs-lookup"><span data-stu-id="75cec-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![impostare il punto di interruzione](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="75cec-178">Rimuovere la chiamata `ServerInfo.GetHtml` al metodo e aggiungere `@myTime` una chiamata alla variabile al suo posto.</span><span class="sxs-lookup"><span data-stu-id="75cec-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="75cec-179">Questa chiamata visualizza il valore di ora corrente restituito dalla nuova riga di codice.</span><span class="sxs-lookup"><span data-stu-id="75cec-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="75cec-180">Premere F5 per eseguire la pagina nel debugger.</span><span class="sxs-lookup"><span data-stu-id="75cec-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="75cec-181">La pagina si interrompe in corrispondenza del punto di interruzione impostato.</span><span class="sxs-lookup"><span data-stu-id="75cec-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="75cec-182">L'immagine seguente mostra l'aspetto della pagina nell'editor con il punto di interruzione (in giallo).</span><span class="sxs-lookup"><span data-stu-id="75cec-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span>

    ![punto di interruzione di debug](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="75cec-184">Nella barra degli strumenti Debug, fare clic sul pulsante **Esegui istruzione** (o premere F11) per eseguire la riga di codice successiva.</span><span class="sxs-lookup"><span data-stu-id="75cec-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="75cec-185">Ogni volta che si fa clic su questo pulsante, si passa l'esecuzione alla riga di codice successiva.</span><span class="sxs-lookup"><span data-stu-id="75cec-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Pulsante Entra in](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="75cec-187">Esaminare il `myTime` valore della variabile posizionando il puntatore del mouse su di essa o controllando i valori **visualizzati** nelle finestre Variabili locali e **Stack di chiamate.**</span><span class="sxs-lookup"><span data-stu-id="75cec-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="75cec-188">In Visual Studio viene visualizzato il valore della variabile.</span><span class="sxs-lookup"><span data-stu-id="75cec-188">Visual Studio display the value of the variable.</span></span>

    ![Mostra valore temporale](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="75cec-190">Al termine dell'esame della variabile e dell'esecuzione del codice, premere F5 per continuare l'esecuzione della pagina senza fermarsi a ogni riga.</span><span class="sxs-lookup"><span data-stu-id="75cec-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="75cec-191">Al termine dell'esecuzione di un'istruzione alla volta in tutto il codice, il browser visualizza la pagina.</span><span class="sxs-lookup"><span data-stu-id="75cec-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="75cec-192">Per ulteriori informazioni sul debugger e su come eseguire il debug del codice in Visual Studio, vedere [Procedura dettagliata: debug di pagine Web in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="75cec-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="75cec-193">Utilizzo di Razor in ASP.NET progetti MVC con Visual StudioUsing Razor in ASP.NET MVC projects with Visual Studio</span><span class="sxs-lookup"><span data-stu-id="75cec-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="75cec-194">La sintassi Razor viene utilizzata ampiamente anche nei progetti MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="75cec-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="75cec-195">MVC è un modo potente e basato su modelli per creare siti Web dinamici.</span><span class="sxs-lookup"><span data-stu-id="75cec-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="75cec-196">Se il sito ASP.NET pagine Web diventa difficile da gestire, è consigliabile convertirlo in un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="75cec-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="75cec-197">Per un esempio di creazione di un'applicazione MVC, vedere [Introduzione all'ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="75cec-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="75cec-198">Installazione del supporto per ASP.NET pagine Web in Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="75cec-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="75cec-199">In questa sezione viene illustrato come installare Visual Web Developer Express 2010 e gli strumenti di ASP.NET pagine Web per Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75cec-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="75cec-200">Se non si dispone già dell'Installazione guidata piattaforma Web, scaricarla dal seguente URL:</span><span class="sxs-lookup"><span data-stu-id="75cec-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="75cec-201">Eseguire l'Installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="75cec-201">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="75cec-202">Fare clic sulla scheda **Prodotti.**</span><span class="sxs-lookup"><span data-stu-id="75cec-202">Click the **Products** tab.</span></span>

    ![Scheda Prodotti WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="75cec-204">Cercare **ASP.NET MVC 4** (per ASP.NET pagine Web 2) e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="75cec-204">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="75cec-205">Questi prodotti includono strumenti di Visual Studio per la creazione di siti Web ASP.NET Razor.These products include Visual Studio tools for building ASP.NET Razor websites.</span><span class="sxs-lookup"><span data-stu-id="75cec-205">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Opzioni di installazione di WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="75cec-207">Fare clic su **Installa** per completare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="75cec-207">Click **Install** to complete the installation.</span></span>
