---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Modifica dello Z Index di un controllo DropShadow (c#) | Microsoft Docs
author: wenz
description: Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura. Tuttavia questo shadow in alcuni casi è in conflitto con altri controlli, di programma...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: cc9407ba15474f58437817c9536d6040e0ea2e84
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59381444"
---
# <a name="adjusting-the-z-index-of-a-dropshadow-c"></a><span data-ttu-id="73c74-104">Modifica dello z index di un controllo DropShadow (C#)</span><span class="sxs-lookup"><span data-stu-id="73c74-104">Adjusting the Z-Index of a DropShadow (C#)</span></span>

<span data-ttu-id="73c74-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="73c74-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="73c74-106">[Scaricare il codice](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="73c74-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span></span>

> <span data-ttu-id="73c74-107">Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="73c74-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="73c74-108">Tuttavia questo shadow in alcuni casi è in conflitto con altri controlli, ad esempio il controllo di ASP.NET Menu.</span><span class="sxs-lookup"><span data-stu-id="73c74-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="73c74-109">Quando una voce di menu viene visualizzata, viene visualizzato dietro l'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="73c74-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="73c74-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="73c74-110">Overview</span></span>

<span data-ttu-id="73c74-111">Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="73c74-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="73c74-112">Tuttavia questo shadow in alcuni casi è in conflitto con altri controlli, ad esempio il controllo di ASP.NET Menu.</span><span class="sxs-lookup"><span data-stu-id="73c74-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="73c74-113">Quando una voce di menu viene visualizzata, viene visualizzato dietro l'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="73c74-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="73c74-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="73c74-114">Steps</span></span>

<span data-ttu-id="73c74-115">Il codice inizia con il pannello stesso, che contiene il testo sufficiente in modo che il pannello contiene testo sufficiente per l'effetto sia visibile:</span><span class="sxs-lookup"><span data-stu-id="73c74-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

<span data-ttu-id="73c74-116">Un altro pannello è posizionato direttamente prima il `panelShadow` pannello.</span><span class="sxs-lookup"><span data-stu-id="73c74-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="73c74-117">Contiene un menu con orientamento orizzontale, in modo che le voci di menu visualizzate in (o piuttosto: sotto) il `dropShadow` pannello):</span><span class="sxs-lookup"><span data-stu-id="73c74-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

<span data-ttu-id="73c74-118">Successivamente, il `DropShadowExtender` viene aggiunto per estendere il `panelShadow` pannello con un effetto ombreggiatura:</span><span class="sxs-lookup"><span data-stu-id="73c74-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

<span data-ttu-id="73c74-119">Infine, ASP.NET AJAX `ScriptManager` controllo Abilita il Toolkit di controllo lavorare:</span><span class="sxs-lookup"><span data-stu-id="73c74-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

<span data-ttu-id="73c74-120">Quando si esegue questo script, le voci di menu vengono visualizzate sotto il pannello.</span><span class="sxs-lookup"><span data-stu-id="73c74-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="73c74-121">Tuttavia il menu di scelta Usa la classe CSS `panel` in cui è sufficiente definire due cose come è possibile visualizzare gli elementi davanti a altro pannello:</span><span class="sxs-lookup"><span data-stu-id="73c74-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="73c74-122">Posizionamento relativo</span><span class="sxs-lookup"><span data-stu-id="73c74-122">Relative positioning</span></span>
- <span data-ttu-id="73c74-123">Un indice z positivo</span><span class="sxs-lookup"><span data-stu-id="73c74-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

<span data-ttu-id="73c74-124">Quindi, `DropShadowExtender` controllo non è in conflitto più con il controllo Menu.</span><span class="sxs-lookup"><span data-stu-id="73c74-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="73c74-125">[![Prima: La voce di menu non è visibile](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="73c74-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span></span>

<span data-ttu-id="73c74-126">Prima: La voce di menu non è visibile ([fare clic per visualizzare l'immagine con dimensioni normali](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="73c74-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span></span>


<span data-ttu-id="73c74-127">[![Dopo: Verrà visualizzata la voce di menu](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="73c74-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span></span>

<span data-ttu-id="73c74-128">Dopo: La voce di menu viene visualizzata ([fare clic per visualizzare l'immagine con dimensioni normali](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="73c74-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="73c74-129">avanti</span><span class="sxs-lookup"><span data-stu-id="73c74-129">Next</span></span>](manipulating-dropshadow-properties-from-client-code-cs.md)
