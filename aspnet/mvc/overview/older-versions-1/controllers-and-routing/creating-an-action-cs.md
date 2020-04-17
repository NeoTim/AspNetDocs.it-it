---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Creazione di un'azione (C ) Documenti Microsoft
author: rick-anderson
description: Informazioni su come aggiungere una nuova azione a un controller MVC ASP.NET. Informazioni sui requisiti per un metodo come azione.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 1c1edfbab41afa58c7cb2c0d306995e9fe491c90
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542274"
---
# <a name="creating-an-action-c"></a><span data-ttu-id="ddb6d-104">Creazione di un'azione (C#)</span><span class="sxs-lookup"><span data-stu-id="ddb6d-104">Creating an Action (C#)</span></span>

<span data-ttu-id="ddb6d-105">da parte [di Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ddb6d-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="ddb6d-106">Informazioni su come aggiungere una nuova azione a un controller MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="ddb6d-107">Informazioni sui requisiti per un metodo come azione.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-107">Learn about the requirements for a method to be an action.</span></span>

<span data-ttu-id="ddb6d-108">L'obiettivo di questa esercitazione è spiegare come è possibile creare una nuova azione del controller.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="ddb6d-109">Informazioni sui requisiti di un metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="ddb6d-110">Si apprenderà inoltre come impedire l'esposizione di un metodo come azione.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="ddb6d-111">Aggiunta di un'azione a un controllerAdding an Action to a Controller</span><span class="sxs-lookup"><span data-stu-id="ddb6d-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="ddb6d-112">Aggiungere una nuova azione a un controller aggiungendo un nuovo metodo al controller.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="ddb6d-113">Ad esempio, il controller nel listato 1 contiene un'azione denominata Index() e un'azione denominata SayHello().</span><span class="sxs-lookup"><span data-stu-id="ddb6d-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="ddb6d-114">Entrambi i metodi vengono esposti come azioni.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="ddb6d-115">**Listato 1 - Controller-HomeController.csListing 1 - Controllers-HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="ddb6d-115">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

<span data-ttu-id="ddb6d-116">Per essere esposto all'universo come azione, un metodo deve soddisfare determinati requisiti:</span><span class="sxs-lookup"><span data-stu-id="ddb6d-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="ddb6d-117">Il metodo deve essere pubblico.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-117">The method must be public.</span></span>
- <span data-ttu-id="ddb6d-118">Il metodo non può essere un metodo statico.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="ddb6d-119">Il metodo non può essere un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="ddb6d-120">Il metodo non può essere un costruttore, getter o setter.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="ddb6d-121">Il metodo non può avere tipi generici aperti.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="ddb6d-122">Il metodo non è un metodo della classe base del controller.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="ddb6d-123">Il metodo non può contenere parametri **ref** o **out.**</span><span class="sxs-lookup"><span data-stu-id="ddb6d-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="ddb6d-124">Si noti che non esistono restrizioni sul tipo restituito di un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="ddb6d-125">Un'azione del controller può restituire una stringa, un DateTime, un'istanza della classe Random o void.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="ddb6d-126">Il framework di MVC ASP.NET convertirà qualsiasi tipo restituito che non è un risultato di azione in una stringa ed eseguirà il rendering della stringa nel browser.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="ddb6d-127">Quando si aggiunge un metodo che non viola questi requisiti a un controller, il metodo viene esposto come azione del controller.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="ddb6d-128">Fate attenzione qui.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-128">Be careful here.</span></span> <span data-ttu-id="ddb6d-129">A controller action can be invoked by anyone connected to the Internet.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="ddb6d-130">Non creare, ad esempio, un'azione del controller DeleteMyWebsite().</span><span class="sxs-lookup"><span data-stu-id="ddb6d-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="ddb6d-131">Impedire che un metodo pubblico venga richiamatoPreventing a Public Method From Being Invoked</span><span class="sxs-lookup"><span data-stu-id="ddb6d-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="ddb6d-132">Se è necessario creare un metodo pubblico in una classe controller e non si desidera esporre il metodo come azione del controller, è possibile impedire che il metodo venga richiamato utilizzando l'attributo [NonAction].</span><span class="sxs-lookup"><span data-stu-id="ddb6d-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the [NonAction] attribute.</span></span> <span data-ttu-id="ddb6d-133">Ad esempio, il controller nel listato 2 contiene un metodo pubblico denominato CompanySecrets() decorato con l'attributo [NonAction].</span><span class="sxs-lookup"><span data-stu-id="ddb6d-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the [NonAction] attribute.</span></span>

<span data-ttu-id="ddb6d-134">**Listato 2 - Controllers-WorkController.cs**</span><span class="sxs-lookup"><span data-stu-id="ddb6d-134">**Listing 2 - Controllers\WorkController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

<span data-ttu-id="ddb6d-135">Se si tenta di richiamare l'azione del controller CompanySecrets() digitando /Work/CompanySecrets nella barra degli indirizzi del browser, verrà visualizzato il messaggio di errore nella Figura 1.</span><span class="sxs-lookup"><span data-stu-id="ddb6d-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>

<span data-ttu-id="ddb6d-136">[![Richiamo di un metodo NonAction](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ddb6d-136">[![Invoking a NonAction method](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span></span>

<span data-ttu-id="ddb6d-137">**Figura 01**: richiamare un metodo NonAction[(Fare clic per visualizzare l'immagine a dimensioni intere)](creating-an-action-cs/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="ddb6d-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-cs/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ddb6d-138">[Successivo](creating-a-controller-cs.md)
> [precedente](asp-net-mvc-routing-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ddb6d-138">[Previous](creating-a-controller-cs.md)
[Next](asp-net-mvc-routing-overview-vb.md)</span></span>
