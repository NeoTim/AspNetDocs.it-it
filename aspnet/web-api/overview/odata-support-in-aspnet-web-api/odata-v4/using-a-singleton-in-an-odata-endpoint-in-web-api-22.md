---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Creare un singleton in OData V4 usando l'API Web 2,2 | Microsoft Docs
author: rick-anderson
description: Questo argomento illustra come definire un singleton in un endpoint OData nell'API Web 2,2.
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 218449c18759b306e425c55f8e7b573d837b4658
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622087"
---
# <a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="619a9-103">Creare un singleton in OData V4 usando l'API Web 2,2</span><span class="sxs-lookup"><span data-stu-id="619a9-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>

<span data-ttu-id="619a9-104">di Zoe Luo</span><span class="sxs-lookup"><span data-stu-id="619a9-104">by Zoe Luo</span></span>

> <span data-ttu-id="619a9-105">Tradizionalmente, è possibile accedere a un'entità solo se è stata incapsulata all'interno di un set di entità.</span><span class="sxs-lookup"><span data-stu-id="619a9-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="619a9-106">Tuttavia OData V4 fornisce due opzioni aggiuntive, singleton e contenimento, entrambe supportate da WebAPI 2,2.</span><span class="sxs-lookup"><span data-stu-id="619a9-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>

<span data-ttu-id="619a9-107">Questo articolo illustra come definire un singleton in un endpoint OData nell'API Web 2,2.</span><span class="sxs-lookup"><span data-stu-id="619a9-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="619a9-108">Per informazioni su un singleton e su come sfruttarlo, vedere [utilizzo di un singleton per definire un'entità speciale](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span><span class="sxs-lookup"><span data-stu-id="619a9-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="619a9-109">Per creare un endpoint OData V4 nell'API Web, vedere [creare un endpoint OData V4 usando API Web ASP.NET 2,2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="619a9-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="619a9-110">Verrà creato un singleton nel progetto API Web usando il modello di dati seguente:</span><span class="sxs-lookup"><span data-stu-id="619a9-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![Modello dati](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="619a9-112">Un singleton denominato `Umbrella` verrà definito in base al tipo `Company`e verrà definito un set di entità denominato `Employees` in base al tipo `Employee`.</span><span class="sxs-lookup"><span data-stu-id="619a9-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="619a9-113">La soluzione usata in questa esercitazione può essere scaricata da [codeplex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span><span class="sxs-lookup"><span data-stu-id="619a9-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="619a9-114">Definizione del modello di dati</span><span class="sxs-lookup"><span data-stu-id="619a9-114">Define the data model</span></span>

1. <span data-ttu-id="619a9-115">Definire i tipi CLR.</span><span class="sxs-lookup"><span data-stu-id="619a9-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="619a9-116">Generare il modello EDM basato sui tipi CLR.</span><span class="sxs-lookup"><span data-stu-id="619a9-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="619a9-117">`builder.Singleton<Company>("Umbrella")` indica al generatore di modelli di creare un singleton denominato `Umbrella` nel modello EDM.</span><span class="sxs-lookup"><span data-stu-id="619a9-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="619a9-118">I metadati generati appariranno come segue:</span><span class="sxs-lookup"><span data-stu-id="619a9-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="619a9-119">Dai metadati è possibile vedere che la proprietà di navigazione `Company` nel set di entità `Employees` è associata al `Umbrella`singleton.</span><span class="sxs-lookup"><span data-stu-id="619a9-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="619a9-120">L'associazione viene eseguita automaticamente da `ODataConventionModelBuilder`, perché solo `Umbrella` dispone del tipo di `Company`.</span><span class="sxs-lookup"><span data-stu-id="619a9-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="619a9-121">In caso di ambiguità nel modello, è possibile utilizzare `HasSingletonBinding` per associare in modo esplicito una proprietà di navigazione a un singleton; `HasSingletonBinding` ha lo stesso effetto dell'uso dell'attributo `Singleton` nella definizione del tipo CLR:</span><span class="sxs-lookup"><span data-stu-id="619a9-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="619a9-122">Definire il controller singleton</span><span class="sxs-lookup"><span data-stu-id="619a9-122">Define the singleton controller</span></span>

<span data-ttu-id="619a9-123">Come il controller EntitySet, il controller singleton eredita da `ODataController`e il nome del controller singleton deve essere `[singletonName]Controller`.</span><span class="sxs-lookup"><span data-stu-id="619a9-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="619a9-124">Per gestire diversi tipi di richieste, è necessario che le azioni siano predefinite nel controller.</span><span class="sxs-lookup"><span data-stu-id="619a9-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="619a9-125">Il **routing degli attributi** è abilitato per impostazione predefinita in WebAPI 2,2.</span><span class="sxs-lookup"><span data-stu-id="619a9-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="619a9-126">Ad esempio, per definire un'azione per gestire l'esecuzione di query `Revenue` da `Company` tramite il routing degli attributi, usare quanto segue:</span><span class="sxs-lookup"><span data-stu-id="619a9-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="619a9-127">Se non si è disposti a definire gli attributi per ogni azione, è sufficiente definire le azioni che seguono le [convenzioni di routing OData](../odata-routing-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="619a9-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="619a9-128">Poiché una chiave non è necessaria per l'esecuzione di query su un singleton, le azioni definite nel controller Singleton sono leggermente diverse dalle azioni definite nel controller EntitySet.</span><span class="sxs-lookup"><span data-stu-id="619a9-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="619a9-129">Per riferimento, le firme del metodo per ogni definizione di azione nel controller Singleton sono elencate di seguito.</span><span class="sxs-lookup"><span data-stu-id="619a9-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="619a9-130">Fondamentalmente, questo è tutto ciò che è necessario fare sul lato del servizio.</span><span class="sxs-lookup"><span data-stu-id="619a9-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="619a9-131">Il [progetto di esempio](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contiene tutto il codice per la soluzione e il client OData che Mostra come usare il singleton.</span><span class="sxs-lookup"><span data-stu-id="619a9-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="619a9-132">Il client viene compilato seguendo la procedura descritta in [creare un'app client OData V4](create-an-odata-v4-client-app.md).</span><span class="sxs-lookup"><span data-stu-id="619a9-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="619a9-133">.</span><span class="sxs-lookup"><span data-stu-id="619a9-133">.</span></span> 

<span data-ttu-id="619a9-134">*Grazie a Leo Hu per il contenuto originale di questo articolo.*</span><span class="sxs-lookup"><span data-stu-id="619a9-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
