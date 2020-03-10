---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Contenimento in OData V4 con l'API Web 2,2 | Microsoft Docs
author: rick-anderson
description: Tradizionalmente, è possibile accedere a un'entità solo se è stata incapsulata all'interno di un set di entità. Tuttavia OData V4 fornisce due opzioni aggiuntive, singleton e con...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 50050e40c4c42bf6d769d077c27864ee6417d4db
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525123"
---
# <a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="3ed9a-104">Contenimento in OData V4 con l'API Web 2,2</span><span class="sxs-lookup"><span data-stu-id="3ed9a-104">Containment in OData v4 Using Web API 2.2</span></span>

<span data-ttu-id="3ed9a-105">di Jinfu Tan</span><span class="sxs-lookup"><span data-stu-id="3ed9a-105">by Jinfu Tan</span></span>

> <span data-ttu-id="3ed9a-106">Tradizionalmente, è possibile accedere a un'entità solo se è stata incapsulata all'interno di un set di entità.</span><span class="sxs-lookup"><span data-stu-id="3ed9a-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="3ed9a-107">Tuttavia OData V4 fornisce due opzioni aggiuntive, singleton e contenimento, entrambe supportate da WebAPI 2,2.</span><span class="sxs-lookup"><span data-stu-id="3ed9a-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>

<span data-ttu-id="3ed9a-108">Questo argomento illustra come definire un contenuto in un endpoint OData in WebApi 2,2.</span><span class="sxs-lookup"><span data-stu-id="3ed9a-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="3ed9a-109">Per altre informazioni sul contenimento, vedere la pagina relativa [all'indipendenza con OData V4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span><span class="sxs-lookup"><span data-stu-id="3ed9a-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="3ed9a-110">Per creare un endpoint OData V4 nell'API Web, vedere [creare un endpoint OData V4 usando API Web ASP.NET 2,2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="3ed9a-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="3ed9a-111">In primo luogo, verrà creato un modello di dominio di contenimento nel servizio OData, usando questo modello di dati:</span><span class="sxs-lookup"><span data-stu-id="3ed9a-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![Modello di dati](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="3ed9a-113">Un account contiene molti PaymentInstruments (PI), ma non viene definito un set di entità per un PI.</span><span class="sxs-lookup"><span data-stu-id="3ed9a-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="3ed9a-114">Al contrario, è possibile accedere al PIs solo tramite un account.</span><span class="sxs-lookup"><span data-stu-id="3ed9a-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="3ed9a-115">È possibile scaricare la soluzione usata in questo argomento da [codeplex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span><span class="sxs-lookup"><span data-stu-id="3ed9a-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="3ed9a-116">Definizione del modello di dati</span><span class="sxs-lookup"><span data-stu-id="3ed9a-116">Defining the data model</span></span>

1. <span data-ttu-id="3ed9a-117">Definire i tipi CLR.</span><span class="sxs-lookup"><span data-stu-id="3ed9a-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="3ed9a-118">L'attributo `Contained` viene usato per le proprietà di navigazione di contenimento.</span><span class="sxs-lookup"><span data-stu-id="3ed9a-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="3ed9a-119">Generare il modello EDM basato sui tipi CLR.</span><span class="sxs-lookup"><span data-stu-id="3ed9a-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="3ed9a-120">Il `ODataConventionModelBuilder` gestirà la compilazione del modello EDM se l'attributo `Contained` viene aggiunto alla proprietà di navigazione corrispondente.</span><span class="sxs-lookup"><span data-stu-id="3ed9a-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="3ed9a-121">Se la proprietà è un tipo di raccolta, verrà creata anche una funzione `GetCount(string NameContains)`.</span><span class="sxs-lookup"><span data-stu-id="3ed9a-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="3ed9a-122">I metadati generati appariranno come segue:</span><span class="sxs-lookup"><span data-stu-id="3ed9a-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="3ed9a-123">L'attributo `ContainsTarget` indica che la proprietà di navigazione è un contenimento.</span><span class="sxs-lookup"><span data-stu-id="3ed9a-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="3ed9a-124">Definire il controller del set di entità contenitore</span><span class="sxs-lookup"><span data-stu-id="3ed9a-124">Define the containing entity set controller</span></span>

<span data-ttu-id="3ed9a-125">Le entità contenute non hanno il proprio controller; l'azione è definita nel controller del set di entità contenitore.</span><span class="sxs-lookup"><span data-stu-id="3ed9a-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="3ed9a-126">In questo esempio è presente un AccountsController, ma nessun PaymentInstrumentsController.</span><span class="sxs-lookup"><span data-stu-id="3ed9a-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="3ed9a-127">Se il percorso OData è di 4 o più segmenti, funziona solo il routing degli attributi, ad esempio `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` nel controller precedente.</span><span class="sxs-lookup"><span data-stu-id="3ed9a-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="3ed9a-128">In caso contrario, sia l'attributo che il routing convenzionale funzionano: ad esempio, `GetPayInPIs(int key)` corrisponde a `GET ~/Accounts(1)/PayinPIs`.</span><span class="sxs-lookup"><span data-stu-id="3ed9a-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="3ed9a-129">*Grazie a Leo Hu per il contenuto originale di questo articolo.*</span><span class="sxs-lookup"><span data-stu-id="3ed9a-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
