---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Creazione di un vincolo di routeC#personalizzato () | Microsoft Docs
author: StephenWalther
description: Stephen Walther illustra come è possibile creare un vincolo di route personalizzato. Viene implementato un semplice vincolo personalizzato che impedisce la corrispondenza di una route con...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 98d5839e3d2623665770ccc5689c28f9eb29c8f6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601451"
---
# <a name="creating-a-custom-route-constraint-c"></a><span data-ttu-id="b8c30-104">Creazione di un vincolo di route personalizzato (C#)</span><span class="sxs-lookup"><span data-stu-id="b8c30-104">Creating a Custom Route Constraint (C#)</span></span>

<span data-ttu-id="b8c30-105">di [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="b8c30-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="b8c30-106">Stephen Walther illustra come è possibile creare un vincolo di route personalizzato.</span><span class="sxs-lookup"><span data-stu-id="b8c30-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="b8c30-107">Viene implementato un semplice vincolo personalizzato che impedisce la corrispondenza di una route quando viene effettuata una richiesta del browser da un computer remoto.</span><span class="sxs-lookup"><span data-stu-id="b8c30-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>

<span data-ttu-id="b8c30-108">L'obiettivo di questa esercitazione è illustrare come è possibile creare un vincolo di route personalizzato.</span><span class="sxs-lookup"><span data-stu-id="b8c30-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="b8c30-109">Un vincolo di route personalizzato consente di impedire la corrispondenza di una route, a meno che non venga stabilita una corrispondenza per alcune condizioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="b8c30-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="b8c30-110">In questa esercitazione viene creato un vincolo di route localhost.</span><span class="sxs-lookup"><span data-stu-id="b8c30-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="b8c30-111">Il vincolo di route localhost corrisponde solo alle richieste effettuate dal computer locale.</span><span class="sxs-lookup"><span data-stu-id="b8c30-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="b8c30-112">Le richieste remote da Internet non corrispondono.</span><span class="sxs-lookup"><span data-stu-id="b8c30-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="b8c30-113">Implementare un vincolo di route personalizzato implementando l'interfaccia IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="b8c30-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="b8c30-114">Si tratta di un'interfaccia estremamente semplice che descrive un solo metodo:</span><span class="sxs-lookup"><span data-stu-id="b8c30-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="b8c30-115">Il metodo restituisce un valore booleano.</span><span class="sxs-lookup"><span data-stu-id="b8c30-115">The method returns a Boolean value.</span></span> <span data-ttu-id="b8c30-116">Se si restituisce false, la route associata al vincolo non corrisponderà alla richiesta del browser.</span><span class="sxs-lookup"><span data-stu-id="b8c30-116">If you return false, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="b8c30-117">Il vincolo localhost è contenuto nel listato 1.</span><span class="sxs-lookup"><span data-stu-id="b8c30-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="b8c30-118">**Listato 1-LocalhostConstraint.cs**</span><span class="sxs-lookup"><span data-stu-id="b8c30-118">**Listing 1 - LocalhostConstraint.cs**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="b8c30-119">Il vincolo nel listato 1 sfrutta la proprietà setlocale esposta dalla classe HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="b8c30-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="b8c30-120">Questa proprietà restituisce true quando l'indirizzo IP della richiesta è 127.0.0.1 o quando l'indirizzo IP della richiesta è uguale all'indirizzo IP del server.</span><span class="sxs-lookup"><span data-stu-id="b8c30-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="b8c30-121">Si usa un vincolo personalizzato all'interno di una route definita nel file Global. asax.</span><span class="sxs-lookup"><span data-stu-id="b8c30-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="b8c30-122">Il file Global. asax nel listato 2 usa il vincolo localhost per impedire a chiunque di richiedere una pagina di amministrazione, a meno che non effettui la richiesta dal server locale.</span><span class="sxs-lookup"><span data-stu-id="b8c30-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="b8c30-123">Ad esempio, una richiesta di/Admin/DeleteAll avrà esito negativo se eseguita da un server remoto.</span><span class="sxs-lookup"><span data-stu-id="b8c30-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="b8c30-124">**Listato 2-Global. asax**</span><span class="sxs-lookup"><span data-stu-id="b8c30-124">**Listing 2 - Global.asax**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="b8c30-125">Il vincolo localhost viene usato nella definizione della route di amministrazione.</span><span class="sxs-lookup"><span data-stu-id="b8c30-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="b8c30-126">Questa route non verrà confrontata con una richiesta del browser remoto.</span><span class="sxs-lookup"><span data-stu-id="b8c30-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="b8c30-127">Tenere presente, tuttavia, che altre route definite in Global. asax potrebbero corrispondere alla stessa richiesta.</span><span class="sxs-lookup"><span data-stu-id="b8c30-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="b8c30-128">È importante comprendere che un vincolo impedisce a una determinata route di abbinare una richiesta e non tutte le route definite nel file Global. asax.</span><span class="sxs-lookup"><span data-stu-id="b8c30-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="b8c30-129">Si noti che la route predefinita è stata impostata come commento dal file Global. asax nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="b8c30-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="b8c30-130">Se si include la route predefinita, la route predefinita corrisponderà alle richieste per il controller di amministrazione.</span><span class="sxs-lookup"><span data-stu-id="b8c30-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="b8c30-131">In tal caso, gli utenti remoti possono comunque richiamare le azioni del controller di amministrazione anche se le richieste non corrispondono alla route di amministrazione.</span><span class="sxs-lookup"><span data-stu-id="b8c30-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b8c30-132">[Precedente](creating-a-route-constraint-cs.md)
> [Successivo](asp-net-mvc-controller-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b8c30-132">[Previous](creating-a-route-constraint-cs.md)
[Next](asp-net-mvc-controller-overview-vb.md)</span></span>
