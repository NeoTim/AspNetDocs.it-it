---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: Creazione di un Controller (c#) | Microsoft Docs
author: StephenWalther
description: In questa esercitazione, Stephen Walther spiega come aggiungere un controller a un'applicazione ASP.NET MVC.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 6e3d0bae7f07410637c2b06c500d94a02c821f5c
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65123619"
---
# <a name="creating-a-controller-c"></a><span data-ttu-id="89896-103">Creazione di un controller (C#)</span><span class="sxs-lookup"><span data-stu-id="89896-103">Creating a Controller (C#)</span></span>

<span data-ttu-id="89896-104">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="89896-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="89896-105">In questa esercitazione, Stephen Walther spiega come aggiungere un controller a un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="89896-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>

<span data-ttu-id="89896-106">L'obiettivo di questa esercitazione è illustrare come è possibile creare nuovi ASP.NET MVC controller.</span><span class="sxs-lookup"><span data-stu-id="89896-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="89896-107">Informazioni su come creare controller usando l'opzione di menu di Visual Studio aggiungere Controller e creando manualmente un file di classe.</span><span class="sxs-lookup"><span data-stu-id="89896-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="89896-108">Con l'aggiungere Controller di menu</span><span class="sxs-lookup"><span data-stu-id="89896-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="89896-109">Il modo più semplice per creare un nuovo controller consiste nella cartella Controllers nella finestra Esplora soluzioni di Visual Studio e scegliere il **Controller, Aggiungi** l'opzione di menu (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="89896-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="89896-110">Selezionando questa opzione di menu si apre la **Aggiungi Controller** finestra di dialogo (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="89896-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>

<span data-ttu-id="89896-111">[![La finestra di dialogo Nuovo progetto](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="89896-111">[![The New Project dialog box](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)</span></span>

<span data-ttu-id="89896-112">**Figura 01**: Aggiunge un nuovo controller ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-controller-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="89896-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-cs/_static/image2.png))</span></span>

<span data-ttu-id="89896-113">[![La finestra di dialogo Nuovo progetto](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="89896-113">[![The New Project dialog box](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)</span></span>

<span data-ttu-id="89896-114">**Figura 02**: La finestra di dialogo Aggiungi Controller ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-controller-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="89896-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-cs/_static/image4.png))</span></span>

<span data-ttu-id="89896-115">Si noti che la prima parte del nome del controller sia evidenziata nel **Aggiungi Controller** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="89896-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="89896-116">Ogni nome del controller deve terminare con il suffisso *Controller*.</span><span class="sxs-lookup"><span data-stu-id="89896-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="89896-117">Ad esempio, è possibile creare un controller denominato *ProductController* ma non un controller denominato *prodotto*.</span><span class="sxs-lookup"><span data-stu-id="89896-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>

<span data-ttu-id="89896-118">Se si crea un controller che non è presente il *Controller* suffisso, quindi sarà possibile richiamare il controller.</span><span class="sxs-lookup"><span data-stu-id="89896-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="89896-119">Non eseguire questa operazione, ho sprecato innumerevoli ore della vita dopo commettere questo errore.</span><span class="sxs-lookup"><span data-stu-id="89896-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>

<span data-ttu-id="89896-120">**Listato 1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="89896-120">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

<span data-ttu-id="89896-121">È sempre necessario creare i controller nella cartella controller.</span><span class="sxs-lookup"><span data-stu-id="89896-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="89896-122">In caso contrario, è possibile violare le convenzioni di ASP.NET MVC e altri sviluppatori avrà un tempo più difficile la comprensione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="89896-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="89896-123">Lo scaffolding di metodi di azione</span><span class="sxs-lookup"><span data-stu-id="89896-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="89896-124">Quando si crea un controller, è possibile generare automaticamente i metodi di azione Create, Update e dettagli (vedere la figura 3).</span><span class="sxs-lookup"><span data-stu-id="89896-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="89896-125">Se si seleziona questa opzione viene generata la classe controller nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="89896-125">If you select this option then the controller class in Listing 2 is generated.</span></span>

<span data-ttu-id="89896-126">[![La creazione automatica di metodi di azione](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="89896-126">[![Creating action methods automatically](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)</span></span>

<span data-ttu-id="89896-127">**Figura 03**: La creazione automatica di metodi di azione ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-controller-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="89896-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-cs/_static/image6.png))</span></span>

<span data-ttu-id="89896-128">**Listato 2 - Controllers\CustomerController.cs**</span><span class="sxs-lookup"><span data-stu-id="89896-128">**Listing 2 - Controllers\CustomerController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

<span data-ttu-id="89896-129">Questi metodi generati si trovano i metodi stub.</span><span class="sxs-lookup"><span data-stu-id="89896-129">These generated methods are stub methods.</span></span> <span data-ttu-id="89896-130">È necessario aggiungere la logica effettiva per la creazione, aggiornamento e che mostra i dettagli per un cliente se stessi.</span><span class="sxs-lookup"><span data-stu-id="89896-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="89896-131">Tuttavia, i metodi stub forniscono un punto di partenza utile.</span><span class="sxs-lookup"><span data-stu-id="89896-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="89896-132">Creazione di una classe Controller</span><span class="sxs-lookup"><span data-stu-id="89896-132">Creating a Controller Class</span></span>

<span data-ttu-id="89896-133">Il controller ASP.NET MVC è semplicemente una classe.</span><span class="sxs-lookup"><span data-stu-id="89896-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="89896-134">Se si preferisce, è possibile ignorare lo scaffolding di controller pratico di Visual Studio e creare manualmente una classe controller.</span><span class="sxs-lookup"><span data-stu-id="89896-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="89896-135">Attenersi ai passaggi riportati di seguito.</span><span class="sxs-lookup"><span data-stu-id="89896-135">Follow these steps:</span></span>

1. <span data-ttu-id="89896-136">Fare clic sulla cartella controller e selezionare l'opzione di menu **Aggiungi, elemento nuove** e selezionare il **classe** modello (vedere la figura 4).</span><span class="sxs-lookup"><span data-stu-id="89896-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="89896-137">Nome nuova classe PersonController.cs, quindi scegliere il **Add** pulsante.</span><span class="sxs-lookup"><span data-stu-id="89896-137">Name the new class PersonController.cs and click the **Add** button.</span></span>
3. <span data-ttu-id="89896-138">Modificare il file di classe risultante in modo che la classe eredita dalla classe di MVC di base (vedere il listato 3).</span><span class="sxs-lookup"><span data-stu-id="89896-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>

<span data-ttu-id="89896-139">[![Creazione di una nuova classe](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="89896-139">[![Creating a new class](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)</span></span>

<span data-ttu-id="89896-140">**Figura 04**: Creazione di una nuova classe ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-controller-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="89896-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-cs/_static/image8.png))</span></span>

<span data-ttu-id="89896-141">**Listato 3 - Controllers\PersonController.cs**</span><span class="sxs-lookup"><span data-stu-id="89896-141">**Listing 3 - Controllers\PersonController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

<span data-ttu-id="89896-142">Il controller nel listato 3 espone un'azione denominata index () che restituisce la stringa "Hello World!".</span><span class="sxs-lookup"><span data-stu-id="89896-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="89896-143">È possibile richiamare l'azione del controller, eseguire l'applicazione e che richiede un URL simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="89896-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE]
> 
> <span data-ttu-id="89896-144">Il Server di sviluppo ASP.NET utilizza un numero di porta casuale (ad esempio, 40071).</span><span class="sxs-lookup"><span data-stu-id="89896-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="89896-145">Quando si immette un URL per richiamare un controller, è necessario fornire il numero della porta destra.</span><span class="sxs-lookup"><span data-stu-id="89896-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="89896-146">È possibile determinare il numero di porta posizionando il puntatore del mouse sull'icona per il Server di sviluppo ASP.NET nell'Area di notifica Windows (in basso a destra dello schermo).</span><span class="sxs-lookup"><span data-stu-id="89896-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="89896-147">[Precedente](adding-dynamic-content-to-a-cached-page-cs.md)
> [Successivo](creating-an-action-cs.md)</span><span class="sxs-lookup"><span data-stu-id="89896-147">[Previous](adding-dynamic-content-to-a-cached-page-cs.md)
[Next](creating-an-action-cs.md)</span></span>
