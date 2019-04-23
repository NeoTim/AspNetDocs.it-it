---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Creazione di route personalizzate (c#) | Microsoft Docs
author: microsoft
description: Informazioni su come aggiungere route personalizzati a un'applicazione ASP.NET MVC. In questa esercitazione descrive come modificare la tabella di route predefinito nel file Global. asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 7b7324c9e0518697c0978b96b0123cb44133722b
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59418937"
---
# <a name="creating-custom-routes-c"></a><span data-ttu-id="544cf-104">Creazione di route personalizzate (C#)</span><span class="sxs-lookup"><span data-stu-id="544cf-104">Creating Custom Routes (C#)</span></span>

<span data-ttu-id="544cf-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="544cf-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="544cf-106">Informazioni su come aggiungere route personalizzati a un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="544cf-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="544cf-107">In questa esercitazione descrive come modificare la tabella di route predefinito nel file Global. asax.</span><span class="sxs-lookup"><span data-stu-id="544cf-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>


<span data-ttu-id="544cf-108">In questa esercitazione descrive come aggiungere una route personalizzata a un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="544cf-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="544cf-109">Descrive come modificare la tabella di route predefinito nel file Global. asax con una route personalizzata.</span><span class="sxs-lookup"><span data-stu-id="544cf-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="544cf-110">Per molte applicazioni MVC ASP.NET semplice, la tabella di route predefinite funzioneranno senza problemi.</span><span class="sxs-lookup"><span data-stu-id="544cf-110">For many simple ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="544cf-111">Tuttavia, è possibile scoprire che si sono specializzati esigenze di routine.</span><span class="sxs-lookup"><span data-stu-id="544cf-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="544cf-112">In tal caso, è possibile creare una route personalizzata.</span><span class="sxs-lookup"><span data-stu-id="544cf-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="544cf-113">Si supponga, ad esempio, che si compila un'applicazione blog.</span><span class="sxs-lookup"><span data-stu-id="544cf-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="544cf-114">Si potrebbe voler gestire le richieste in ingresso con un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="544cf-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="544cf-115">/Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="544cf-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="544cf-116">Quando un utente immette la richiesta, si desidera restituire la voce del blog che corrisponde alla data 12/25/2009.</span><span class="sxs-lookup"><span data-stu-id="544cf-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="544cf-117">Per gestire questo tipo di richiesta, è necessario creare una route personalizzata.</span><span class="sxs-lookup"><span data-stu-id="544cf-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="544cf-118">Il file Global. asax nel listato 1 contiene una nuova route personalizzata, denominata Blog, che gestisce le richieste che hanno l'aspetto /Archive/*data voce*.</span><span class="sxs-lookup"><span data-stu-id="544cf-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="544cf-119">**Listato 1 - Global. asax (con route personalizzate)**</span><span class="sxs-lookup"><span data-stu-id="544cf-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

<span data-ttu-id="544cf-120">L'ordine delle route che aggiungono alla tabella di route è importante.</span><span class="sxs-lookup"><span data-stu-id="544cf-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="544cf-121">La nuova route Blog personalizzata viene aggiunto prima route esistente predefinita.</span><span class="sxs-lookup"><span data-stu-id="544cf-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="544cf-122">Se è invertito l'ordine, quindi verrà chiamata la route predefinita sempre anziché route personalizzata.</span><span class="sxs-lookup"><span data-stu-id="544cf-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="544cf-123">La route di Blog personalizzata corrisponde a qualsiasi richiesta che inizia con/archiviazione /.</span><span class="sxs-lookup"><span data-stu-id="544cf-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="544cf-124">Pertanto, corrisponde a tutti gli URL seguenti:</span><span class="sxs-lookup"><span data-stu-id="544cf-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="544cf-125">/Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="544cf-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="544cf-126">/ Archiviazione/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="544cf-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="544cf-127">/ Archiviazione/apple</span><span class="sxs-lookup"><span data-stu-id="544cf-127">/Archive/apple</span></span>

<span data-ttu-id="544cf-128">Route personalizzata esegue il mapping della richiesta in ingresso a un controller denominato archivio e richiama l'azione Entry().</span><span class="sxs-lookup"><span data-stu-id="544cf-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="544cf-129">Quando viene chiamato il metodo Entry(), la data di inizio viene passata come un parametro denominato entryDate.</span><span class="sxs-lookup"><span data-stu-id="544cf-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="544cf-130">È possibile usare la route personalizzata Blog con il controller nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="544cf-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="544cf-131">**Listato 2 - ArchiveController.cs**</span><span class="sxs-lookup"><span data-stu-id="544cf-131">**Listing 2 - ArchiveController.cs**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

<span data-ttu-id="544cf-132">Si noti che il metodo Entry() nel listato 2 accetta un parametro di tipo DateTime.</span><span class="sxs-lookup"><span data-stu-id="544cf-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="544cf-133">Il framework MVC è in grado di convertire automaticamente la data di inizio dall'URL in un valore DateTime.</span><span class="sxs-lookup"><span data-stu-id="544cf-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="544cf-134">Se il parametro della data voce dall'URL non può essere convertito in un oggetto DateTime, viene generato un errore (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="544cf-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="544cf-135">**Figura 1. errore di conversione del parametro**</span><span class="sxs-lookup"><span data-stu-id="544cf-135">**Figure 1 - Error from converting parameter**</span></span>


<span data-ttu-id="544cf-136">[![La finestra di dialogo Nuovo progetto](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="544cf-136">[![The New Project dialog box](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span></span>

<span data-ttu-id="544cf-137">**Figura 01**: Errore di conversione del parametro ([fare clic per visualizzare l'immagine con dimensioni normali](creating-custom-routes-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="544cf-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-cs/_static/image2.png))</span></span>


## <a name="summary"></a><span data-ttu-id="544cf-138">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="544cf-138">Summary</span></span>

<span data-ttu-id="544cf-139">L'obiettivo di questa esercitazione è stata per illustrare come è possibile creare una route personalizzata.</span><span class="sxs-lookup"><span data-stu-id="544cf-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="544cf-140">Si è appreso come aggiungere una route personalizzata alla tabella di route nel file Global. asax che rappresenta le voci di blog.</span><span class="sxs-lookup"><span data-stu-id="544cf-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="544cf-141">È stato illustrato come eseguire il mapping di richieste per le voci di blog a un controller denominato ArchiveController e un'azione del controller denominato Entry().</span><span class="sxs-lookup"><span data-stu-id="544cf-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="544cf-142">[Precedente](aspnet-mvc-controllers-overview-cs.md)
> [Successivo](creating-a-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="544cf-142">[Previous](aspnet-mvc-controllers-overview-cs.md)
[Next](creating-a-route-constraint-cs.md)</span></span>
