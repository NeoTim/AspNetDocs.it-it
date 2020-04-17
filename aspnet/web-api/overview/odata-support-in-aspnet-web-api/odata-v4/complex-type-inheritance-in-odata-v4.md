---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Ereditarietà dei tipi complessi in OData v4 con ASP.NETAPI Web Documenti Microsoft
author: rick-anderson
description: In base alla specifica OData v4, un tipo complesso può ereditare da un altro tipo complesso. (Un tipo complesso è un tipo strutturato senza una chiave.) API Web...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: f433cf625c7d6ff4922d8c4a9954682fc0f1cc33
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543600"
---
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="a2722-104">Ereditarietà dei tipi complessi in OData v4 con ASP.NET API WebComplex Type Inheritance in OData v4 with ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="a2722-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="a2722-105">da parte [di Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a2722-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a2722-106">In base alla [specifica](http://www.odata.org/documentation/odata-version-4-0/)OData v4 , un tipo complesso può ereditare da un altro tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="a2722-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="a2722-107">(Un tipo *complesso* è un tipo strutturato senza una chiave.) L'API Web OData 5.3 supporta l'ereditarietà dei tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="a2722-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="a2722-108">In questo argomento viene illustrato come compilare un modello EDM (Entity Data Model) con tipi di ereditarietà complessi.</span><span class="sxs-lookup"><span data-stu-id="a2722-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="a2722-109">Per il codice sorgente completo, vedere [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="a2722-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a2722-110">Versioni software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="a2722-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a2722-111">API Web OData 5.3</span><span class="sxs-lookup"><span data-stu-id="a2722-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="a2722-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="a2722-112">OData v4</span></span>

## <a name="model-hierarchy"></a><span data-ttu-id="a2722-113">Gerarchia dei modelli</span><span class="sxs-lookup"><span data-stu-id="a2722-113">Model Hierarchy</span></span>

<span data-ttu-id="a2722-114">Per illustrare l'ereditarietà dei tipi complessi, verrà utilizzata la gerarchia di classi seguente.</span><span class="sxs-lookup"><span data-stu-id="a2722-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="a2722-115">`Shape`è un tipo complesso astratto.</span><span class="sxs-lookup"><span data-stu-id="a2722-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="a2722-116">`Rectangle`, `Triangle`e `Circle` sono tipi `Shape`complessi `RoundRectangle` derivati `Rectangle`da e deriva da .</span><span class="sxs-lookup"><span data-stu-id="a2722-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="a2722-117">`Window`è un tipo di `Shape` entità e contiene un'istanza.</span><span class="sxs-lookup"><span data-stu-id="a2722-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="a2722-118">Di seguito sono riportate le classi CLR che definiscono questi tipi.</span><span class="sxs-lookup"><span data-stu-id="a2722-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="a2722-119">Creare il modello EDMBuild the EDM Model</span><span class="sxs-lookup"><span data-stu-id="a2722-119">Build the EDM Model</span></span>

<span data-ttu-id="a2722-120">Per creare l'EDM, è possibile utilizzare **ODataConventionModelBuilder**, che deduce le relazioni di ereditarietà dai tipi CLR.</span><span class="sxs-lookup"><span data-stu-id="a2722-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="a2722-121">È anche possibile compilare l'EDM in modo esplicito, utilizzando **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="a2722-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="a2722-122">Questa operazione richiede più codice, ma offre un maggiore controllo sull'EDM.</span><span class="sxs-lookup"><span data-stu-id="a2722-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="a2722-123">Questi due esempi creano lo stesso schema EDM.</span><span class="sxs-lookup"><span data-stu-id="a2722-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="a2722-124">Documento metadati</span><span class="sxs-lookup"><span data-stu-id="a2722-124">Metadata Document</span></span>

<span data-ttu-id="a2722-125">Ecco il documento di metadati OData, che mostra l'ereditarietà dei tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="a2722-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="a2722-126">Dal documento di metadati, è possibile vedere che:</span><span class="sxs-lookup"><span data-stu-id="a2722-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="a2722-127">Il `Shape` tipo complesso è astratto.</span><span class="sxs-lookup"><span data-stu-id="a2722-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="a2722-128">I `Rectangle` `Triangle`tipi `Circle` , e complex `Shape`hanno il tipo di base .</span><span class="sxs-lookup"><span data-stu-id="a2722-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="a2722-129">Il `RoundRectangle` tipo ha `Rectangle`il tipo di base .</span><span class="sxs-lookup"><span data-stu-id="a2722-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="a2722-130">Casting di tipi complessi</span><span class="sxs-lookup"><span data-stu-id="a2722-130">Casting Complex Types</span></span>

<span data-ttu-id="a2722-131">Il cast su tipi complessi è ora supportato.</span><span class="sxs-lookup"><span data-stu-id="a2722-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="a2722-132">Ad esempio, la query `Shape` seguente `Rectangle`esegue il cast di a a .</span><span class="sxs-lookup"><span data-stu-id="a2722-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="a2722-133">Ecco il payload della risposta:Here's the response payload:</span><span class="sxs-lookup"><span data-stu-id="a2722-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
