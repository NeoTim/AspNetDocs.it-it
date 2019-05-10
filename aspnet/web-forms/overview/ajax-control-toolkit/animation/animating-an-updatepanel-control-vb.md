---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animazione di un controllo UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Per il contenuto di un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: b44dfd284ac1ed94e92bd52f4ca426a36bf86825
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65130751"
---
# <a name="animating-an-updatepanel-control-vb"></a><span data-ttu-id="f94f9-104">Animazione di un controllo UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="f94f9-104">Animating an UpdatePanel Control (VB)</span></span>

<span data-ttu-id="f94f9-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f94f9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f94f9-106">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f94f9-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span></span>

> <span data-ttu-id="f94f9-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="f94f9-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f94f9-108">Per il contenuto di un UpdatePanel, esiste già una speciale estensione che si basa principalmente sul framework di animazione: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="f94f9-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="f94f9-109">Questa esercitazione illustra come configurare un'animazione di questo tipo per un controllo UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="f94f9-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>

## <a name="overview"></a><span data-ttu-id="f94f9-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="f94f9-110">Overview</span></span>

<span data-ttu-id="f94f9-111">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="f94f9-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f94f9-112">Per il contenuto di un `UpdatePanel`, è presente una speciale estensione che si basa principalmente sul framework di animazione: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="f94f9-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="f94f9-113">Questa esercitazione illustra come configurare un'animazione di questo tipo per un `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="f94f9-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="f94f9-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="f94f9-114">Steps</span></span>

<span data-ttu-id="f94f9-115">Il primo passaggio consiste nel modo consueto per includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX viene caricata e il Toolkit di controllo possono essere usato:</span><span class="sxs-lookup"><span data-stu-id="f94f9-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

<span data-ttu-id="f94f9-116">L'animazione in questo scenario verrà applicata a un ASP.NET `Wizard` controllo web che si trova in un `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="f94f9-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="f94f9-117">Tre passaggi (arbitrari) offrono sufficienti opzioni per attivare i postback:</span><span class="sxs-lookup"><span data-stu-id="f94f9-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

<span data-ttu-id="f94f9-118">Il markup necessario per il `UpdatePanelAnimationExtender` controllo è abbastanza simile al markup utilizzato per il `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="f94f9-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="f94f9-119">Nel `TargetControlID` attributo forniamo il `ID` del `UpdatePanel` per aggiungere un'animazione; all'interno del `UpdatePanelAnimationExtender` controllo, il `<Animations>` elemento contiene markup XML per le animazioni.</span><span class="sxs-lookup"><span data-stu-id="f94f9-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="f94f9-120">Tuttavia c'è una differenza: La quantità di eventi e gestori eventi è limitata alla `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="f94f9-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="f94f9-121">Per `UpdatePanels`, solo due di esse esiste:</span><span class="sxs-lookup"><span data-stu-id="f94f9-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="f94f9-122">`<OnUpdated>` Quando è stato aggiornato UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="f94f9-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="f94f9-123">`<OnUpdating>` Avvio aggiornamento di UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="f94f9-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="f94f9-124">In questo scenario, il contenuto di nuovo il `UpdatePanel` (dopo il postback) devono dissolvenza in entrata.</span><span class="sxs-lookup"><span data-stu-id="f94f9-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="f94f9-125">Questo è il markup necessario per che:</span><span class="sxs-lookup"><span data-stu-id="f94f9-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

<span data-ttu-id="f94f9-126">Ora ogni volta che si verifica un postback all'interno di UpdatePanel, il nuovo contenuto del Pannello di dissolvenza in entrata senza problemi.</span><span class="sxs-lookup"><span data-stu-id="f94f9-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>

<span data-ttu-id="f94f9-127">[![Il passaggio successivo della procedura guidata è la dissolvenza in entrata](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f94f9-127">[![The next wizard step is fading in](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="f94f9-128">Il passaggio successivo della procedura guidata è la dissolvenza in entrata ([fare clic per visualizzare l'immagine con dimensioni normali](animating-an-updatepanel-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f94f9-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f94f9-129">[Precedente](changing-an-animation-using-client-side-code-vb.md)
> [Successivo](dynamically-controlling-updatepanel-animations-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f94f9-129">[Previous](changing-an-animation-using-client-side-code-vb.md)
[Next](dynamically-controlling-updatepanel-animations-vb.md)</span></span>
