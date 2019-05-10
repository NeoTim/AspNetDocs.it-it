---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Contenimento in OData v4 Using Web API 2.2 | Microsoft Docs
author: rick-anderson
description: In genere, un'entità è stato possibile accedere solo se si sono stato incapsulato all'interno di un set di entità. Ma OData v4 fornisce due opzioni aggiuntive, Singleton e Con...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 50050e40c4c42bf6d769d077c27864ee6417d4db
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65131637"
---
# <a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="ef780-104">Contenimento in OData v4 tramite l'API Web 2.2</span><span class="sxs-lookup"><span data-stu-id="ef780-104">Containment in OData v4 Using Web API 2.2</span></span>

<span data-ttu-id="ef780-105">da Jinfu Tan</span><span class="sxs-lookup"><span data-stu-id="ef780-105">by Jinfu Tan</span></span>

> <span data-ttu-id="ef780-106">In genere, un'entità è stato possibile accedere solo se si sono stato incapsulato all'interno di un set di entità.</span><span class="sxs-lookup"><span data-stu-id="ef780-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="ef780-107">Ma OData v4 fornisce due opzioni aggiuntive, Singleton e contenimento, che supporta l'API Web 2.2.</span><span class="sxs-lookup"><span data-stu-id="ef780-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>

<span data-ttu-id="ef780-108">In questo argomento viene illustrato come definire un contenimento in un endpoint OData nell'API Web 2.2.</span><span class="sxs-lookup"><span data-stu-id="ef780-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="ef780-109">Per altre informazioni sul contenuto, vedere [contenimento proviene con OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span><span class="sxs-lookup"><span data-stu-id="ef780-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="ef780-110">Per creare un endpoint OData V4 nell'API Web, vedere [creare un OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="ef780-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="ef780-111">In primo luogo, si creerà un modello di dominio di contenimento del servizio OData, utilizzando questo modello di dati:</span><span class="sxs-lookup"><span data-stu-id="ef780-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![Modello di dati](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="ef780-113">Un account contiene molti PaymentInstruments (PI), ma non definiamo una set di entità per un dispositivo PI.</span><span class="sxs-lookup"><span data-stu-id="ef780-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="ef780-114">Al contrario, PI sono accessibile solo tramite un Account.</span><span class="sxs-lookup"><span data-stu-id="ef780-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="ef780-115">È possibile scaricare la soluzione usata in questo argomento dalla [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span><span class="sxs-lookup"><span data-stu-id="ef780-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="ef780-116">La definizione del modello di dati</span><span class="sxs-lookup"><span data-stu-id="ef780-116">Defining the data model</span></span>

1. <span data-ttu-id="ef780-117">Definire i tipi CLR.</span><span class="sxs-lookup"><span data-stu-id="ef780-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="ef780-118">Il `Contained` viene usato per le proprietà di navigazione di contenimento.</span><span class="sxs-lookup"><span data-stu-id="ef780-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="ef780-119">Generare il modello EDM in base ai tipi CLR.</span><span class="sxs-lookup"><span data-stu-id="ef780-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="ef780-120">Il `ODataConventionModelBuilder` gestirà la compilazione del modello EDM, se il `Contained` attributo viene aggiunto alla proprietà di navigazione corrispondente.</span><span class="sxs-lookup"><span data-stu-id="ef780-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="ef780-121">Se la proprietà è un tipo di raccolta una `GetCount(string NameContains)` funzione verrà inoltre creata.</span><span class="sxs-lookup"><span data-stu-id="ef780-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="ef780-122">I metadati generati avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="ef780-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="ef780-123">Il `ContainsTarget` attributo indica che la proprietà di navigazione rappresenta un contenimento.</span><span class="sxs-lookup"><span data-stu-id="ef780-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="ef780-124">Definire il controller di set di entità che lo contiene</span><span class="sxs-lookup"><span data-stu-id="ef780-124">Define the containing entity set controller</span></span>

<span data-ttu-id="ef780-125">Entità indipendenti non abbiano i propri controller; l'azione è definita nel controller di set di entità che lo contiene.</span><span class="sxs-lookup"><span data-stu-id="ef780-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="ef780-126">In questo esempio, è un AccountsController, ma nessun PaymentInstrumentsController.</span><span class="sxs-lookup"><span data-stu-id="ef780-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="ef780-127">Se il percorso OData è 4 o più segmenti, solo attributo funzionamento del routing, ad esempio `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` nel controller precedente.</span><span class="sxs-lookup"><span data-stu-id="ef780-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="ef780-128">In caso contrario, attributi e routing convenzionale funziona: ad esempio, `GetPayInPIs(int key)` corrisponde a `GET ~/Accounts(1)/PayinPIs`.</span><span class="sxs-lookup"><span data-stu-id="ef780-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="ef780-129">*Grazie a Leo Hu per il contenuto originale di questo articolo.*</span><span class="sxs-lookup"><span data-stu-id="ef780-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
