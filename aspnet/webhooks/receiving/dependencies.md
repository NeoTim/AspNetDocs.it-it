---
uid: webhooks/receiving/dependencies
title: dipendenze del ricevitore di WebHooks ASP.NET Documenti Microsoft
author: rick-anderson
description: Dipendenze del ricevitore e inserimento delle dipendenze in ASP.NET WebHook.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: b50442b3d95512bc0db7583b93de3bbef2d4bb4a
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675203"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="6015f-103">Dipendenze del ricevitore di ASP.NET WebHooks</span><span class="sxs-lookup"><span data-stu-id="6015f-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="6015f-104">Microsoft ASP.NET WebHooks è progettato tenendo presente l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="6015f-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="6015f-105">La maggior parte delle dipendenze nel sistema può essere sostituita con implementazioni alternative usando un motore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="6015f-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="6015f-106">Vedere [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) per un elenco delle dipendenze del ricevitore.</span><span class="sxs-lookup"><span data-stu-id="6015f-106">See [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="6015f-107">Se non è stata registrata alcuna dipendenza, viene utilizzata un'implementazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6015f-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="6015f-108">Vedere [ReceiverServices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) per un elenco delle implementazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="6015f-108">See [ReceiverServices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
