---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Convalida con un livello di servizio (c#) | Microsoft Docs
author: StephenWalther
description: Informazioni su come spostare la logica di convalida all'esterno di azioni del controller e a un livello di servizio separato. In questa esercitazione, Stephen Walther spiega come è...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 69ff78949589017d12a791231e38b400b49f2917
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57051838"
---
<a name="validating-with-a-service-layer-c"></a><span data-ttu-id="060dc-104">Convalida con un livello di servizio (C#)</span><span class="sxs-lookup"><span data-stu-id="060dc-104">Validating with a Service Layer (C#)</span></span>
====================
<span data-ttu-id="060dc-105">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="060dc-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="060dc-106">Informazioni su come spostare la logica di convalida all'esterno di azioni del controller e a un livello di servizio separato.</span><span class="sxs-lookup"><span data-stu-id="060dc-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="060dc-107">In questa esercitazione, Stephen Walther spiega come è possibile gestire una separazione delle problematiche sharp isolando il livello di servizio dal livello del controller.</span><span class="sxs-lookup"><span data-stu-id="060dc-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>


<span data-ttu-id="060dc-108">L'obiettivo di questa esercitazione è descrivere un metodo di esecuzione della convalida in un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="060dc-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="060dc-109">In questa esercitazione descrive come spostare la logica di convalida del controller e a un livello di servizio separato.</span><span class="sxs-lookup"><span data-stu-id="060dc-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="060dc-110">La separazione delle problematiche</span><span class="sxs-lookup"><span data-stu-id="060dc-110">Separating Concerns</span></span>

<span data-ttu-id="060dc-111">Quando si compila un'applicazione ASP.NET MVC, è consigliabile non inserire la logica di database all'interno di azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="060dc-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="060dc-112">Combinare la logica di database e controller rende più difficile da gestire nel corso del tempo l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="060dc-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="060dc-113">Si consiglia di posizionare tutta la logica di database in un livello di repository separati.</span><span class="sxs-lookup"><span data-stu-id="060dc-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="060dc-114">Listato 1, ad esempio, contiene un repository semplice denominato il ProductRepository.</span><span class="sxs-lookup"><span data-stu-id="060dc-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="060dc-115">Il repository di prodotto contiene tutto il codice di accesso di dati per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="060dc-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="060dc-116">L'elenco include anche l'interfaccia IProductRepository che implementa il repository di prodotto.</span><span class="sxs-lookup"><span data-stu-id="060dc-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="060dc-117">**Listato 1, Models\ProductRepository.cs**</span><span class="sxs-lookup"><span data-stu-id="060dc-117">**Listing 1 -- Models\ProductRepository.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

<span data-ttu-id="060dc-118">Il controller nel listato 2 Usa il livello di repository le azioni di entrambe le proprietà Index () e create ().</span><span class="sxs-lookup"><span data-stu-id="060dc-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="060dc-119">Si noti che questo controller non contiene alcuna logica di database.</span><span class="sxs-lookup"><span data-stu-id="060dc-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="060dc-120">Creazione di un livello di repository consente di mantenere una netta separazione delle problematiche.</span><span class="sxs-lookup"><span data-stu-id="060dc-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="060dc-121">I controller sono responsabili della logica di controllo di flusso dell'applicazione e il repository è responsabile della logica di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="060dc-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="060dc-122">**Listato 2 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="060dc-122">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="060dc-123">Creazione di un livello di servizio</span><span class="sxs-lookup"><span data-stu-id="060dc-123">Creating a Service Layer</span></span>

<span data-ttu-id="060dc-124">Pertanto, logica di controllo di flusso dell'applicazione appartiene in un controller e logica di accesso ai dati a cui appartiene un repository.</span><span class="sxs-lookup"><span data-stu-id="060dc-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="060dc-125">In tal caso, in cui si gestiscono la logica di convalida?</span><span class="sxs-lookup"><span data-stu-id="060dc-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="060dc-126">Una possibilità consiste nell'inserire logica di convalida in un *livello di servizio*.</span><span class="sxs-lookup"><span data-stu-id="060dc-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="060dc-127">Un livello di servizio è un ulteriore livello in un'applicazione ASP.NET MVC che consente di eseguire la comunicazione tra un controller e il livello di repository.</span><span class="sxs-lookup"><span data-stu-id="060dc-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="060dc-128">Il livello di servizio contiene la logica di business.</span><span class="sxs-lookup"><span data-stu-id="060dc-128">The service layer contains business logic.</span></span> <span data-ttu-id="060dc-129">In particolare, contiene la logica di convalida.</span><span class="sxs-lookup"><span data-stu-id="060dc-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="060dc-130">Ad esempio, il livello di servizio del prodotto nel listato 3 ha un metodo CreateProduct().</span><span class="sxs-lookup"><span data-stu-id="060dc-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="060dc-131">Il metodo CreateProduct() chiama il metodo ValidateProduct() per convalidare un nuovo prodotto prima di passare il prodotto nel repository di prodotto.</span><span class="sxs-lookup"><span data-stu-id="060dc-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="060dc-132">**Listato 3 - Models\ProductService.cs**</span><span class="sxs-lookup"><span data-stu-id="060dc-132">**Listing 3 - Models\ProductService.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

<span data-ttu-id="060dc-133">Il controller di prodotto è stato aggiornato nel listato 4 per utilizzare il livello di servizio anziché al livello del repository.</span><span class="sxs-lookup"><span data-stu-id="060dc-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="060dc-134">Il livello di servizio comunica con il livello di controller.</span><span class="sxs-lookup"><span data-stu-id="060dc-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="060dc-135">Il livello di servizio comunica con il livello di repository.</span><span class="sxs-lookup"><span data-stu-id="060dc-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="060dc-136">Ogni livello avrà responsabilità separate.</span><span class="sxs-lookup"><span data-stu-id="060dc-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="060dc-137">**Listato 4 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="060dc-137">**Listing 4 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

<span data-ttu-id="060dc-138">Si noti che il servizio di prodotto viene creato nel costruttore del controller di prodotto.</span><span class="sxs-lookup"><span data-stu-id="060dc-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="060dc-139">Quando viene creato il servizio di prodotto, dizionario di stato del modello viene passato al servizio.</span><span class="sxs-lookup"><span data-stu-id="060dc-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="060dc-140">Il servizio del prodotto Usa lo stato del modello per superare la convalida dei messaggi di errore al controller.</span><span class="sxs-lookup"><span data-stu-id="060dc-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="060dc-141">Disaccoppiamento il livello di servizio</span><span class="sxs-lookup"><span data-stu-id="060dc-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="060dc-142">Non è stato possibile isolare il livello di servizio per un aspetto e il controller.</span><span class="sxs-lookup"><span data-stu-id="060dc-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="060dc-143">Il livello di servizio e il controller di comunicazione attraverso lo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="060dc-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="060dc-144">In altre parole, il livello di servizio presenta una dipendenza su una particolare funzionalità del framework ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="060dc-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="060dc-145">Si vuole isolare il livello di servizio dal nostro livello di controller quanto più possibile.</span><span class="sxs-lookup"><span data-stu-id="060dc-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="060dc-146">In teoria, dovremmo sarà in grado di usare il livello di servizio con qualsiasi tipo di applicazione e non solo un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="060dc-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="060dc-147">Ad esempio, in futuro, potremmo voler compilare un front-end per la nostra applicazione WPF.</span><span class="sxs-lookup"><span data-stu-id="060dc-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="060dc-148">Occorre individuare un modo per rimuovere la dipendenza su ASP.NET MVC lo stato del modello dal nostro livello di servizio.</span><span class="sxs-lookup"><span data-stu-id="060dc-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="060dc-149">Nel listato 5, il livello di servizio è stato aggiornato in modo che non usi più lo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="060dc-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="060dc-150">Usa invece una classe che implementa l'interfaccia IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="060dc-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="060dc-151">**Listato 5 - Models\ProductService.cs (separato)**</span><span class="sxs-lookup"><span data-stu-id="060dc-151">**Listing 5 - Models\ProductService.cs (decoupled)**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

<span data-ttu-id="060dc-152">L'interfaccia IValidationDictionary viene definita nel listato 6.</span><span class="sxs-lookup"><span data-stu-id="060dc-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="060dc-153">Questa interfaccia semplice include un solo metodo e una singola proprietà.</span><span class="sxs-lookup"><span data-stu-id="060dc-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="060dc-154">**Listato 6 - Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="060dc-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

<span data-ttu-id="060dc-155">La classe nel listato 7, denominare la classe ModelStateWrapper, implementa l'interfaccia IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="060dc-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="060dc-156">È possibile creare istanze della classe ModelStateWrapper passando un dizionario di stato del modello al costruttore.</span><span class="sxs-lookup"><span data-stu-id="060dc-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="060dc-157">**Listato 7 - Models\ModelStateWrapper.cs**</span><span class="sxs-lookup"><span data-stu-id="060dc-157">**Listing 7 - Models\ModelStateWrapper.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

<span data-ttu-id="060dc-158">Infine, il controller aggiornato nel listato 8 Usa il ModelStateWrapper quando si crea il livello di servizio nel relativo costruttore.</span><span class="sxs-lookup"><span data-stu-id="060dc-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="060dc-159">**Listato 8 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="060dc-159">**Listing 8 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

<span data-ttu-id="060dc-160">Usando il IValidationDictionary interfaccia e la classe ModelStateWrapper ci consente di isolare completamente il livello di servizio dal nostro livello di controller.</span><span class="sxs-lookup"><span data-stu-id="060dc-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="060dc-161">Il livello di servizio non è più dipendente dallo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="060dc-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="060dc-162">È possibile passare qualsiasi classe che implementa l'interfaccia IValidationDictionary al livello di servizio.</span><span class="sxs-lookup"><span data-stu-id="060dc-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="060dc-163">Ad esempio, un'applicazione WPF può implementare l'interfaccia IValidationDictionary con una classe collection semplice.</span><span class="sxs-lookup"><span data-stu-id="060dc-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="060dc-164">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="060dc-164">Summary</span></span>

<span data-ttu-id="060dc-165">L'obiettivo di questa esercitazione è illustrare un approccio all'esecuzione della convalida in un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="060dc-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="060dc-166">In questa esercitazione è stato descritto come spostare tutta la logica di convalida del controller e a un livello di servizio separato.</span><span class="sxs-lookup"><span data-stu-id="060dc-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="060dc-167">Inoltre appreso come isolare il livello di servizio di livello il controller creando una classe ModelStateWrapper.</span><span class="sxs-lookup"><span data-stu-id="060dc-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="060dc-168">[Precedente](validating-with-the-idataerrorinfo-interface-cs.md)
> [Successivo](validation-with-the-data-annotation-validators-cs.md)</span><span class="sxs-lookup"><span data-stu-id="060dc-168">[Previous](validating-with-the-idataerrorinfo-interface-cs.md)
[Next](validation-with-the-data-annotation-validators-cs.md)</span></span>
