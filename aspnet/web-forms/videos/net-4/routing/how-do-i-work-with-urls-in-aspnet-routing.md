---
uid: web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
title: 'Procedura: Usare gli URL nel Routing ASP.NET? | Microsoft Docs'
author: rick-anderson
description: In questo video Chris Pels illustra come specificare gli URL in un sito web che utilizza il routing di ASP.NET. In primo luogo, viene creato un sito web e il routing viene definito nel GL....
ms.author: riande
ms.date: 10/15/2010
ms.assetid: 08f9d0a7-cfa0-4914-a672-8a64295d7ba8
msc.legacyurl: /web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
msc.type: video
ms.openlocfilehash: eb1060fd9cc9469dc2b1d2e918823316c36840cb
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59404819"
---
# <a name="how-do-i-work-with-urls-in-aspnet-routing"></a><span data-ttu-id="f0de4-105">Procedura: Usare gli URL nel Routing ASP.NET?</span><span class="sxs-lookup"><span data-stu-id="f0de4-105">How Do I: Work with URLs in ASP.NET Routing?</span></span>

<span data-ttu-id="f0de4-106">da [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="f0de4-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="f0de4-107">In questo video Chris Pels illustra come specificare gli URL in un sito web che utilizza il routing di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f0de4-107">In this video Chris Pels shows how to specify URLs in a web site that utilizes ASP.NET routing.</span></span> <span data-ttu-id="f0de4-108">In primo luogo, viene creato un sito web e il routing viene definito nella classe di applicazione globale (o asax).</span><span class="sxs-lookup"><span data-stu-id="f0de4-108">First, a web site is created and routing is defined in the Global Application Class (.asax).</span></span> <span data-ttu-id="f0de4-109">Successivamente, viene creata una pagina web di esempio e un URL basato su una route definita viene aggiunto alla pagina utilizzando la "hardcoded" approccio standard, ad esempio, "~/Stats/Visitors".</span><span class="sxs-lookup"><span data-stu-id="f0de4-109">Next, a sample web page is created and a URL based upon a defined route is added to the page using the standard "hard coded" approach, e.g., "~/Stats/Visitors".</span></span> <span data-ttu-id="f0de4-110">Un altro collegamento viene quindi aggiunto alla pagina che genera lo stesso URL in modo dinamico nel markup utilizzando il metodo RouteValue che accetta il nome della route e i parametri.</span><span class="sxs-lookup"><span data-stu-id="f0de4-110">Another link is then added to the page which dynamically generates the same URL in markup using the RouteValue method which accepts the route name and parameters.</span></span> <span data-ttu-id="f0de4-111">Lo stesso URL viene quindi implementato utilizzando codice anziché markup direttamente nella pagina.</span><span class="sxs-lookup"><span data-stu-id="f0de4-111">The same URL is then implemented using code rather than markup directly in the page.</span></span> <span data-ttu-id="f0de4-112">Route originale e la posizione della pagina fisica vengono quindi modificati, risultante non è più nel collegamento hardcoded funziona mentre entrambi generati dinamicamente collegamenti funzione correttamente.</span><span class="sxs-lookup"><span data-stu-id="f0de4-112">The original route and physical page location are then changed, resulting in the hard coded link no longer working whereas both dynamically generated links function properly.</span></span> <span data-ttu-id="f0de4-113">Infine, il valore di collegamenti generati dinamicamente viene quindi illustrato.</span><span class="sxs-lookup"><span data-stu-id="f0de4-113">Finally, the value of dynamically generated links is then discussed.</span></span>

[<span data-ttu-id="f0de4-114">&#9654;Guarda il video (20 minuti)</span><span class="sxs-lookup"><span data-stu-id="f0de4-114">&#9654; Watch video (20 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-work-with-urls-in-aspnet-routing)

> [!div class="step-by-step"]
> [<span data-ttu-id="f0de4-115">Precedente</span><span class="sxs-lookup"><span data-stu-id="f0de4-115">Previous</span></span>](how-do-i-use-routing-with-aspnet-web-forms.md)
