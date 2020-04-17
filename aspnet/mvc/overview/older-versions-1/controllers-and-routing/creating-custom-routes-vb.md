---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Creazione di percorsi personalizzati (VB) Documenti Microsoft
author: rick-anderson
description: Informazioni su come aggiungere route personalizzate a un'applicazione MVC ASP.NET. In questa esercitazione viene illustrato come modificare la tabella di route predefinita nel file Global.asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: fd12307f685fa064832bb4198df06df2c3f2d3be
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542742"
---
# <a name="creating-custom-routes-vb"></a><span data-ttu-id="a16ea-104">Creazione di route personalizzate (VB)</span><span class="sxs-lookup"><span data-stu-id="a16ea-104">Creating Custom Routes (VB)</span></span>

<span data-ttu-id="a16ea-105">da parte [di Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a16ea-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a16ea-106">Informazioni su come aggiungere route personalizzate a un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a16ea-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="a16ea-107">In questa esercitazione viene illustrato come modificare la tabella di route predefinita nel file Global.asax.</span><span class="sxs-lookup"><span data-stu-id="a16ea-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>

<span data-ttu-id="a16ea-108">In questa esercitazione viene illustrato come aggiungere una route personalizzata a un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a16ea-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="a16ea-109">Viene illustrato come modificare la tabella di route predefinita nel file Global.asax con una route personalizzata.</span><span class="sxs-lookup"><span data-stu-id="a16ea-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="a16ea-110">Nelle applicazioni MVC ASP.NET, la tabella di route predefinita funzionerà correttamente.</span><span class="sxs-lookup"><span data-stu-id="a16ea-110">In ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="a16ea-111">Tuttavia, è possibile che si disponga di esigenze di routing specifiche.</span><span class="sxs-lookup"><span data-stu-id="a16ea-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="a16ea-112">In tal caso, è possibile creare una route personalizzata.</span><span class="sxs-lookup"><span data-stu-id="a16ea-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="a16ea-113">Si supponga, ad esempio, che si sta creando un'applicazione blog.</span><span class="sxs-lookup"><span data-stu-id="a16ea-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="a16ea-114">È possibile gestire le richieste in arrivo simili alle seguente:You might want to handle incoming requests that look like this:</span><span class="sxs-lookup"><span data-stu-id="a16ea-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="a16ea-115">/Archivio/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="a16ea-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="a16ea-116">Quando un utente immette questa richiesta, si desidera restituire il post di blog che corrisponde alla data 25/12/2009.</span><span class="sxs-lookup"><span data-stu-id="a16ea-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="a16ea-117">Per gestire questo tipo di richiesta, è necessario creare una route personalizzata.</span><span class="sxs-lookup"><span data-stu-id="a16ea-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="a16ea-118">Il file Global.asax nel listato 1 contiene una nuova route personalizzata, denominata Blog, che gestisce le richieste simili a /Archive/*data di immissione*.</span><span class="sxs-lookup"><span data-stu-id="a16ea-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="a16ea-119">**Listato 1 - Global.asax (con percorso personalizzato)**</span><span class="sxs-lookup"><span data-stu-id="a16ea-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

<span data-ttu-id="a16ea-120">L'ordine delle route aggiunte alla tabella di route è importante.</span><span class="sxs-lookup"><span data-stu-id="a16ea-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="a16ea-121">Il nostro nuovo percorso Blog personalizzato viene aggiunto prima del percorso predefinito esistente.</span><span class="sxs-lookup"><span data-stu-id="a16ea-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="a16ea-122">Se hai invertito l'ordine, verrà sempre chiamata la route predefinita anziché la route personalizzata.</span><span class="sxs-lookup"><span data-stu-id="a16ea-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="a16ea-123">La route del blog personalizzato corrisponde a qualsiasi richiesta che inizia con /Archive/.</span><span class="sxs-lookup"><span data-stu-id="a16ea-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="a16ea-124">Quindi, corrisponde a tutti i seguenti URL:</span><span class="sxs-lookup"><span data-stu-id="a16ea-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="a16ea-125">/Archivio/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="a16ea-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="a16ea-126">/Archivio/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="a16ea-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="a16ea-127">/Archivio/mela</span><span class="sxs-lookup"><span data-stu-id="a16ea-127">/Archive/apple</span></span>

<span data-ttu-id="a16ea-128">La route personalizzata esegue il mapping della richiesta in ingresso a un controller denominato Archive e richiama l'azione Entry().</span><span class="sxs-lookup"><span data-stu-id="a16ea-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="a16ea-129">Quando viene chiamato il metodo Entry(), la data di immissione viene passata come parametro denominato entryDate.</span><span class="sxs-lookup"><span data-stu-id="a16ea-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="a16ea-130">È possibile utilizzare il percorso personalizzato Blog con il controller nel listato 2.You can use the Blog custom route with the controller in Listing 2.</span><span class="sxs-lookup"><span data-stu-id="a16ea-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="a16ea-131">**Listato 2 - ArchiveController.vb**</span><span class="sxs-lookup"><span data-stu-id="a16ea-131">**Listing 2 - ArchiveController.vb**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

<span data-ttu-id="a16ea-132">Si noti che il metodo Entry() nel listato 2 accetta un parametro di tipo DateTime.</span><span class="sxs-lookup"><span data-stu-id="a16ea-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="a16ea-133">Il framework MVC è abbastanza intelligente per convertire automaticamente la data di immissione dall'URL in un valore DateTime.</span><span class="sxs-lookup"><span data-stu-id="a16ea-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="a16ea-134">Se il parametro di data di ingresso dall'URL non può essere convertito in un DateTime, viene generato un errore (vedere Figura 1).</span><span class="sxs-lookup"><span data-stu-id="a16ea-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="a16ea-135">**Figura 1 - Errore durante la conversione del parametro**</span><span class="sxs-lookup"><span data-stu-id="a16ea-135">**Figure 1 - Error from converting parameter**</span></span>

<span data-ttu-id="a16ea-136">[![Finestra di dialogo relativa al nuovo progetto](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a16ea-136">[![The New Project dialog box](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span></span>

<span data-ttu-id="a16ea-137">**Figura 01:** Errore durante la conversione del parametro ([Fare clic per visualizzare l'immagine](creating-custom-routes-vb/_static/image2.png)a dimensioni intere)</span><span class="sxs-lookup"><span data-stu-id="a16ea-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-vb/_static/image2.png))</span></span>

## <a name="summary"></a><span data-ttu-id="a16ea-138">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="a16ea-138">Summary</span></span>

<span data-ttu-id="a16ea-139">L'obiettivo di questa esercitazione era dimostrare come è possibile creare un percorso personalizzato.</span><span class="sxs-lookup"><span data-stu-id="a16ea-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="a16ea-140">Si è appreso come aggiungere una route personalizzata alla tabella di route nel file Global.asax che rappresenta le voci di blog.</span><span class="sxs-lookup"><span data-stu-id="a16ea-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="a16ea-141">È stato illustrato come eseguire il mapping delle richieste per le voci di blog a un controller denominato ArchiveController e un'azione controller denominata Entry().</span><span class="sxs-lookup"><span data-stu-id="a16ea-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a16ea-142">[Successivo](asp-net-mvc-controller-overview-vb.md)
> [precedente](creating-a-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a16ea-142">[Previous](asp-net-mvc-controller-overview-vb.md)
[Next](creating-a-route-constraint-vb.md)</span></span>
