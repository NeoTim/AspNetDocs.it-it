---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Apri i tipi in OData v4 con ASP.NETAPI Web Documenti Microsoft
author: rick-anderson
description: In OData v4, un tipo aperto è un tipo strutturato che contiene proprietà dinamiche, oltre a tutte le proprietà dichiarate nella definizione del tipo. Apri...
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: d63c96df6614896b3b67eef94a8907e528572567
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543730"
---
# <a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="67eea-104">Tipi aperti in OData v4 con API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="67eea-104">Open Types in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="67eea-105">da parte [di Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="67eea-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="67eea-106">In OData v4, un *tipo aperto* è un tipo strutturato che contiene proprietà dinamiche, oltre a tutte le proprietà dichiarate nella definizione del tipo.</span><span class="sxs-lookup"><span data-stu-id="67eea-106">In OData v4, an *open type* is a structured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="67eea-107">I tipi aperti consentono di aggiungere flessibilità ai modelli di dati.</span><span class="sxs-lookup"><span data-stu-id="67eea-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="67eea-108">In questa esercitazione viene illustrato come utilizzare i tipi aperti in ASP.NET API Web OData.This tutorial shows how to use open types in ASP.NET Web API OData.</span><span class="sxs-lookup"><span data-stu-id="67eea-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="67eea-109">In questa esercitazione si presuppone che si sappia già come creare un endpoint OData nellASP.NETAPI Web.This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="67eea-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="67eea-110">In caso contrario, iniziare leggendo [prima Create an OData v4 Endpoint.If](create-an-odata-v4-endpoint.md) not, start by reading Create an OData v4 Endpoint first.</span><span class="sxs-lookup"><span data-stu-id="67eea-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="67eea-111">Versioni software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="67eea-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="67eea-112">API Web OData 5.3</span><span class="sxs-lookup"><span data-stu-id="67eea-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="67eea-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="67eea-113">OData v4</span></span>

<span data-ttu-id="67eea-114">In primo luogo, alcuni terminologia OData:First, some OData terminology:</span><span class="sxs-lookup"><span data-stu-id="67eea-114">First, some OData terminology:</span></span>

- <span data-ttu-id="67eea-115">Tipo di entità: un tipo strutturato con una chiave.</span><span class="sxs-lookup"><span data-stu-id="67eea-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="67eea-116">Tipo complesso: un tipo strutturato senza chiave.Complex type: A structured type without a key.</span><span class="sxs-lookup"><span data-stu-id="67eea-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="67eea-117">Tipo aperto: un tipo con proprietà dinamiche.</span><span class="sxs-lookup"><span data-stu-id="67eea-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="67eea-118">Sia i tipi di entità che i tipi complessi possono essere aperti.</span><span class="sxs-lookup"><span data-stu-id="67eea-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="67eea-119">Il valore di una proprietà dinamica può essere un tipo primitivo, un tipo complesso o un tipo di enumerazione. o una raccolta di uno qualsiasi di questi tipi.</span><span class="sxs-lookup"><span data-stu-id="67eea-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="67eea-120">Per ulteriori informazioni sui tipi aperti, vedere la [specifica OData v4](http://www.odata.org/documentation/odata-version-4-0/).</span><span class="sxs-lookup"><span data-stu-id="67eea-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="67eea-121">Installare le librerie OData Web</span><span class="sxs-lookup"><span data-stu-id="67eea-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="67eea-122">Utilizzare NuGet Package Manager per installare le librerie OData dell'API Web più recenti.</span><span class="sxs-lookup"><span data-stu-id="67eea-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="67eea-123">Dalla finestra Console di Gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="67eea-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="67eea-124">Definire i tipi CLRDefine the CLR Types</span><span class="sxs-lookup"><span data-stu-id="67eea-124">Define the CLR Types</span></span>

<span data-ttu-id="67eea-125">Iniziare definendo i modelli EDM come tipi CLR.</span><span class="sxs-lookup"><span data-stu-id="67eea-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="67eea-126">Quando viene creato Entity Data Model (EDM),</span><span class="sxs-lookup"><span data-stu-id="67eea-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="67eea-127">`Category`è un tipo di enumerazione.</span><span class="sxs-lookup"><span data-stu-id="67eea-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="67eea-128">`Address`è un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="67eea-128">`Address` is a complex type.</span></span> <span data-ttu-id="67eea-129">Non dispone di una chiave, pertanto non è un tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="67eea-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="67eea-130">`Customer`è un tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="67eea-130">`Customer` is an entity type.</span></span> <span data-ttu-id="67eea-131">(Ha una chiave.)</span><span class="sxs-lookup"><span data-stu-id="67eea-131">(It has a key.)</span></span>
- <span data-ttu-id="67eea-132">`Press`è un tipo complesso aperto.</span><span class="sxs-lookup"><span data-stu-id="67eea-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="67eea-133">`Book`è un tipo di entità aperto.</span><span class="sxs-lookup"><span data-stu-id="67eea-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="67eea-134">Per creare un tipo aperto, il tipo `IDictionary<string, object>`CLR deve avere una proprietà di tipo , che contiene le proprietà dinamiche.</span><span class="sxs-lookup"><span data-stu-id="67eea-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="67eea-135">Creare il modello EDMBuild the EDM Model</span><span class="sxs-lookup"><span data-stu-id="67eea-135">Build the EDM Model</span></span>

<span data-ttu-id="67eea-136">Se si utilizza **ODataConventionModelBuilder** per `Press` creare `Book` il modello EDM e vengono aggiunti `IDictionary<string, object>` automaticamente come tipi aperti, in base alla presenza di una proprietà.</span><span class="sxs-lookup"><span data-stu-id="67eea-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="67eea-137">È anche possibile compilare l'EDM in modo esplicito, utilizzando **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="67eea-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="67eea-138">Aggiungere un controller ODataAdd an OData Controller</span><span class="sxs-lookup"><span data-stu-id="67eea-138">Add an OData Controller</span></span>

<span data-ttu-id="67eea-139">Successivamente, aggiungere un controller OData.Next, add an OData controller.</span><span class="sxs-lookup"><span data-stu-id="67eea-139">Next, add an OData controller.</span></span> <span data-ttu-id="67eea-140">Per questa esercitazione useremo un controller semplificato che supporta solo le richieste GET e POST e usa un elenco in memoria per archiviare le entità.</span><span class="sxs-lookup"><span data-stu-id="67eea-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="67eea-141">Si noti `Book` che la prima istanza non ha proprietà dinamiche.</span><span class="sxs-lookup"><span data-stu-id="67eea-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="67eea-142">La `Book` seconda istanza ha le seguenti proprietà dinamiche:</span><span class="sxs-lookup"><span data-stu-id="67eea-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="67eea-143">"Published": Tipo primitivo</span><span class="sxs-lookup"><span data-stu-id="67eea-143">"Published": Primitive type</span></span>
- <span data-ttu-id="67eea-144">"Authors": raccolta di tipi primitivi</span><span class="sxs-lookup"><span data-stu-id="67eea-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="67eea-145">"OtherCategories": raccolta di tipi di enumerazione.</span><span class="sxs-lookup"><span data-stu-id="67eea-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="67eea-146">Inoltre, `Press` la proprietà `Book` di tale istanza ha le seguenti proprietà dinamiche:</span><span class="sxs-lookup"><span data-stu-id="67eea-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="67eea-147">"Blog": tipo primitivo</span><span class="sxs-lookup"><span data-stu-id="67eea-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="67eea-148">"Indirizzo": tipo complesso</span><span class="sxs-lookup"><span data-stu-id="67eea-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="67eea-149">Interrogare i metadati</span><span class="sxs-lookup"><span data-stu-id="67eea-149">Query the Metadata</span></span>

<span data-ttu-id="67eea-150">Per ottenere il documento di metadati `~/$metadata`OData, inviare una richiesta GET a .</span><span class="sxs-lookup"><span data-stu-id="67eea-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="67eea-151">Il corpo della risposta dovrebbe essere simile al seguente:The response body should look similar to this:</span><span class="sxs-lookup"><span data-stu-id="67eea-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="67eea-152">Dal documento di metadati, è possibile vedere che:</span><span class="sxs-lookup"><span data-stu-id="67eea-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="67eea-153">Per `Book` i `Press` tipi e, `OpenType` il valore dell'attributo è true.</span><span class="sxs-lookup"><span data-stu-id="67eea-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="67eea-154">I `Customer` `Address` tipi e non dispongono di questo attributo.</span><span class="sxs-lookup"><span data-stu-id="67eea-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="67eea-155">Il `Book` tipo di entità ha tre proprietà dichiarate: ISBN, Title e Press.</span><span class="sxs-lookup"><span data-stu-id="67eea-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="67eea-156">I metadati OData `Book.Properties` non includono la proprietà della classe CLR.</span><span class="sxs-lookup"><span data-stu-id="67eea-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="67eea-157">Analogamente, `Press` il tipo complesso ha solo due proprietà dichiarate: Name e Category.</span><span class="sxs-lookup"><span data-stu-id="67eea-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="67eea-158">I metadati non `Press.DynamicProperties` includono la proprietà della classe CLR.</span><span class="sxs-lookup"><span data-stu-id="67eea-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="67eea-159">Query an Entity</span><span class="sxs-lookup"><span data-stu-id="67eea-159">Query an Entity</span></span>

<span data-ttu-id="67eea-160">Per ottenere il libro con ISBN uguale a "978-0-7356-7942-9", inviare una richiesta GET a `~/Books('978-0-7356-7942-9')`.</span><span class="sxs-lookup"><span data-stu-id="67eea-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="67eea-161">Il corpo della risposta dovrebbe essere simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="67eea-161">The response body should look similar to the following.</span></span> <span data-ttu-id="67eea-162">(Rientrato per renderlo più leggibile.)</span><span class="sxs-lookup"><span data-stu-id="67eea-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="67eea-163">Si noti che le proprietà dinamiche sono incluse inline con le proprietà dichiarate.</span><span class="sxs-lookup"><span data-stu-id="67eea-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="67eea-164">POST di un'entità</span><span class="sxs-lookup"><span data-stu-id="67eea-164">POST an Entity</span></span>

<span data-ttu-id="67eea-165">Per aggiungere un'entità Book, `~/Books`inviare una richiesta POST a .</span><span class="sxs-lookup"><span data-stu-id="67eea-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="67eea-166">Il client può impostare proprietà dinamiche nel payload della richiesta.</span><span class="sxs-lookup"><span data-stu-id="67eea-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="67eea-167">Ecco una richiesta di esempio.</span><span class="sxs-lookup"><span data-stu-id="67eea-167">Here is an example request.</span></span> <span data-ttu-id="67eea-168">Prendere nota delle proprietà "Price" e "Published".</span><span class="sxs-lookup"><span data-stu-id="67eea-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="67eea-169">Se si imposta un punto di interruzione nel metodo del controller, è possibile vedere che l'API Web ha aggiunto queste proprietà al `Properties` dizionario.</span><span class="sxs-lookup"><span data-stu-id="67eea-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="67eea-170">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="67eea-170">Additional Resources</span></span>

[<span data-ttu-id="67eea-171">Esempio di tipo open OData</span><span class="sxs-lookup"><span data-stu-id="67eea-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
