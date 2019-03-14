---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Creazione di un'azione (c#) | Microsoft Docs
author: microsoft
description: Informazioni su come aggiungere una nuova azione a un controller MVC ASP.NET. Informazioni sui requisiti per un metodo su cui eseguire un'azione.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 243248ee30c6a2db7f102f7743d0393d4a6a9d24
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57056128"
---
<a name="creating-an-action-c"></a><span data-ttu-id="5cb40-104">Creazione di un'azione (C#)</span><span class="sxs-lookup"><span data-stu-id="5cb40-104">Creating an Action (C#)</span></span>
====================
<span data-ttu-id="5cb40-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="5cb40-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="5cb40-106">Informazioni su come aggiungere una nuova azione a un controller MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5cb40-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="5cb40-107">Informazioni sui requisiti per un metodo su cui eseguire un'azione.</span><span class="sxs-lookup"><span data-stu-id="5cb40-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="5cb40-108">L'obiettivo di questa esercitazione è illustrare come è possibile creare una nuova azione del controller.</span><span class="sxs-lookup"><span data-stu-id="5cb40-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="5cb40-109">Informazioni sui requisiti di un metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="5cb40-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="5cb40-110">Anche informazioni su come impedire che un metodo viene esposta come un'azione.</span><span class="sxs-lookup"><span data-stu-id="5cb40-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="5cb40-111">Aggiunta di un'azione a un Controller</span><span class="sxs-lookup"><span data-stu-id="5cb40-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="5cb40-112">Aggiungere una nuova azione a un controller aggiungendo un nuovo metodo al controller.</span><span class="sxs-lookup"><span data-stu-id="5cb40-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="5cb40-113">Ad esempio, il controller nel listato 1 contiene un'azione denominata index () e un'azione denominata sayHello ().</span><span class="sxs-lookup"><span data-stu-id="5cb40-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="5cb40-114">Entrambi i metodi vengono esposti come azioni.</span><span class="sxs-lookup"><span data-stu-id="5cb40-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="5cb40-115">**Listato 1 - controllers\homecontroller.cs.**</span><span class="sxs-lookup"><span data-stu-id="5cb40-115">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

<span data-ttu-id="5cb40-116">Per poter essere esposto di Universe (universo) come un'azione, un metodo deve soddisfare determinati requisiti:</span><span class="sxs-lookup"><span data-stu-id="5cb40-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="5cb40-117">Il metodo deve essere pubblico.</span><span class="sxs-lookup"><span data-stu-id="5cb40-117">The method must be public.</span></span>
- <span data-ttu-id="5cb40-118">Il metodo non può essere un metodo statico.</span><span class="sxs-lookup"><span data-stu-id="5cb40-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="5cb40-119">Il metodo non può essere un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="5cb40-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="5cb40-120">Il metodo non può essere un costruttore, getter o setter.</span><span class="sxs-lookup"><span data-stu-id="5cb40-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="5cb40-121">Il metodo non può avere tipi generici aperti.</span><span class="sxs-lookup"><span data-stu-id="5cb40-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="5cb40-122">Il metodo non è un metodo della classe di base di controller.</span><span class="sxs-lookup"><span data-stu-id="5cb40-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="5cb40-123">Il metodo non può contenere **ref** oppure **out** parametri.</span><span class="sxs-lookup"><span data-stu-id="5cb40-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="5cb40-124">Si noti che non sono previste restrizioni sul tipo restituito di un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="5cb40-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="5cb40-125">Un'azione del controller può restituire una stringa, un valore DateTime, un'istanza della classe Random, o void.</span><span class="sxs-lookup"><span data-stu-id="5cb40-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="5cb40-126">Il framework ASP.NET MVC convertirà qualsiasi tipo restituito non è il risultato di un'azione in una stringa e la stringa al browser di eseguire il rendering.</span><span class="sxs-lookup"><span data-stu-id="5cb40-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="5cb40-127">Quando si aggiunge un metodo che non violano tali requisiti per un controller, il metodo viene esposta come un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="5cb40-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="5cb40-128">Prestare attenzione qui.</span><span class="sxs-lookup"><span data-stu-id="5cb40-128">Be careful here.</span></span> <span data-ttu-id="5cb40-129">Un'azione del controller può essere richiamata da qualsiasi utente connesso a Internet.</span><span class="sxs-lookup"><span data-stu-id="5cb40-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="5cb40-130">Non, ad esempio, creare un'azione del controller DeleteMyWebsite().</span><span class="sxs-lookup"><span data-stu-id="5cb40-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="5cb40-131">Impedisce che un metodo pubblico da richiamare</span><span class="sxs-lookup"><span data-stu-id="5cb40-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="5cb40-132">Se è necessario creare un metodo pubblico in una classe controller e non si vuole esporre il metodo come un'azione del controller è possibile impedire il metodo da cui viene richiamato usando l'attributo [NonAction].</span><span class="sxs-lookup"><span data-stu-id="5cb40-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the [NonAction] attribute.</span></span> <span data-ttu-id="5cb40-133">Ad esempio, il controller nel listato 2 contiene un metodo pubblico denominato CompanySecrets() decorata con l'attributo [NonAction].</span><span class="sxs-lookup"><span data-stu-id="5cb40-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the [NonAction] attribute.</span></span>

<span data-ttu-id="5cb40-134">**Listato 2 - Controllers\WorkController.cs**</span><span class="sxs-lookup"><span data-stu-id="5cb40-134">**Listing 2 - Controllers\WorkController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

<span data-ttu-id="5cb40-135">Se si prova a richiamare l'azione del controller CompanySecrets() digitando /Work/CompanySecrets nella barra degli indirizzi del browser si riceverà il messaggio di errore nella figura 1.</span><span class="sxs-lookup"><span data-stu-id="5cb40-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


<span data-ttu-id="5cb40-136">[![Richiama un metodo NonAction](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5cb40-136">[![Invoking a NonAction method](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span></span>

<span data-ttu-id="5cb40-137">**Figura 01**: Richiama un metodo NonAction ([fare clic per visualizzare l'immagine con dimensioni normali](creating-an-action-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="5cb40-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-cs/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5cb40-138">[Precedente](creating-a-controller-cs.md)
> [Successivo](asp-net-mvc-routing-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="5cb40-138">[Previous](creating-a-controller-cs.md)
[Next](asp-net-mvc-routing-overview-vb.md)</span></span>
