---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Confronto tra visualizzazioni dinamiche Visualizzazioni fortemente tipizzate | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 30b84c71c86e455f15a659abf566750f1c6eea90
ms.sourcegitcommit: feb88edfb01b32f6fc9488f0f0ddb3c5b34e6ff0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/21/2020
ms.locfileid: "88702946"
---
# <a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="2316b-103">Confronto tra visualizzazioni dinamiche</span><span class="sxs-lookup"><span data-stu-id="2316b-103">Dynamic v.</span></span> <span data-ttu-id="2316b-104">e fortemente tipizzate</span><span class="sxs-lookup"><span data-stu-id="2316b-104">Strongly Typed Views</span></span>

<span data-ttu-id="2316b-105">di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2316b-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2316b-106">Esistono tre modi per passare le informazioni da un controller a una visualizzazione in ASP.NET MVC 3:</span><span class="sxs-lookup"><span data-stu-id="2316b-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="2316b-107">Come oggetto modello fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="2316b-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="2316b-108">Come tipo dinamico (tramite @model Dynamic)</span><span class="sxs-lookup"><span data-stu-id="2316b-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="2316b-109">Uso di ViewBag</span><span class="sxs-lookup"><span data-stu-id="2316b-109">Using the ViewBag</span></span>

<span data-ttu-id="2316b-110">Ho scritto una semplice applicazione di Blog MVC 3 top per confrontare e contrapporre le visualizzazioni dinamiche e fortemente tipizzate.</span><span class="sxs-lookup"><span data-stu-id="2316b-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="2316b-111">Il controller inizia con un semplice elenco di Blog:</span><span class="sxs-lookup"><span data-stu-id="2316b-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="2316b-112">Fare clic con il pulsante destro del mouse sul metodo IndexNotStonglyTyped () e aggiungere una visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="2316b-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="2316b-113">[![8475. NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2316b-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="2316b-114">Assicurarsi che la casella **Crea una visualizzazione fortemente tipizzata** non sia selezionata.</span><span class="sxs-lookup"><span data-stu-id="2316b-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="2316b-115">La visualizzazione risultante non contiene molto:</span><span class="sxs-lookup"><span data-stu-id="2316b-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="2316b-116">Poiché si sta usando una visualizzazione dinamica e non fortemente tipizzata, IntelliSense non ci aiuta.</span><span class="sxs-lookup"><span data-stu-id="2316b-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="2316b-117">Il codice completato è illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="2316b-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="2316b-118">[![6646. NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="2316b-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="2316b-119">A questo punto si aggiungerà una visualizzazione fortemente tipizzata.</span><span class="sxs-lookup"><span data-stu-id="2316b-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="2316b-120">Aggiungere il codice seguente al controller:</span><span class="sxs-lookup"><span data-stu-id="2316b-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]

<span data-ttu-id="2316b-121">Si noti che si tratta esattamente della stessa visualizzazione restituita (Blogs); chiamare come visualizzazione non fortemente tipizzata.</span><span class="sxs-lookup"><span data-stu-id="2316b-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="2316b-122">Fare clic con il pulsante destro del mouse all'interno di *StonglyTypedIndex ()* e scegliere **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="2316b-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="2316b-123">Questa volta selezionare la classe del modello di **Blog** e selezionare **Elenca** come modello di impalcatura.</span><span class="sxs-lookup"><span data-stu-id="2316b-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="2316b-124">[![5658. StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="2316b-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="2316b-125">All'interno del nuovo modello di vista si ottiene il supporto IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="2316b-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="2316b-126">[![7002. IntelliSense [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="2316b-126">[![7002.IntelliSense[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>
