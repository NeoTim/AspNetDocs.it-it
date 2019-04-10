---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animazione in base a una condizione (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Se un'animazione è...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 78140f56184e88fb4dbe29f234aebd5732b69ad9
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59418872"
---
# <a name="animation-depending-on-a-condition-vb"></a><span data-ttu-id="3de73-104">Animazione in base a una condizione (VB)</span><span class="sxs-lookup"><span data-stu-id="3de73-104">Animation Depending On a Condition (VB)</span></span>

<span data-ttu-id="3de73-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3de73-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3de73-106">[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="3de73-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span></span>

> <span data-ttu-id="3de73-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="3de73-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3de73-108">Se viene eseguita un'animazione o non può dipendere anche una condizione sotto forma di codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3de73-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="3de73-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="3de73-109">Overview</span></span>

<span data-ttu-id="3de73-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="3de73-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3de73-111">Se viene eseguita un'animazione o non può dipendere anche una condizione sotto forma di codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3de73-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="3de73-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="3de73-112">Steps</span></span>

<span data-ttu-id="3de73-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="3de73-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

<span data-ttu-id="3de73-114">Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="3de73-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

<span data-ttu-id="3de73-115">Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="3de73-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

<span data-ttu-id="3de73-116">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria</span><span class="sxs-lookup"><span data-stu-id="3de73-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory</span></span> `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

<span data-ttu-id="3de73-117">All'interno di `<Animations>` nodo, usare `<OnLoad>` per eseguire le animazioni dopo la pagina è stata caricata completamente.</span><span class="sxs-lookup"><span data-stu-id="3de73-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="3de73-118">Invece di uno delle animazioni, regolare il `<Condition>` entra in gioco l'elemento.</span><span class="sxs-lookup"><span data-stu-id="3de73-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="3de73-119">Il codice JavaScript fornito come valore del `ConditionScript` attributo viene eseguito in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="3de73-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="3de73-120">Se restituisce true, l'animazione viene eseguita, in caso contrario non.</span><span class="sxs-lookup"><span data-stu-id="3de73-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="3de73-121">Il markup seguente fornisce due animazioni, ognuna di esse in esecuzione al 50% dei case al momento casuale.</span><span class="sxs-lookup"><span data-stu-id="3de73-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="3de73-122">Poiché può esistere solo un'animazione all'interno `<OnLoad>`, i due `<Condition>` animazioni vengono unite mediante la `<Sequence>` elemento:</span><span class="sxs-lookup"><span data-stu-id="3de73-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

<span data-ttu-id="3de73-123">Si noti che il segno di minore (`<`) nel `ConditionScript` attributo deve essere sottoposto a escape ().</span><span class="sxs-lookup"><span data-stu-id="3de73-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="3de73-124">Quando si esegue questo script, nessuna animazione viene eseguito, o uno dei due viene, o eseguire entrambe.</span><span class="sxs-lookup"><span data-stu-id="3de73-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>


[![T<span data-ttu-id="3de73-125">Pannello he è dissolvenza in uscita senza alcun ridimensionamento, in modo che il secondo viene eseguito animazione, il primo elemento non è stata]</span><span class="sxs-lookup"><span data-stu-id="3de73-125">he panel is fading out without resizing, so the second animation runs, the first one didn't]</span></span>(animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

<span data-ttu-id="3de73-126">Il pannello è dissolvenza in uscita senza alcun ridimensionamento, in modo che non ha esecuzioni animazione secondo, il primo elemento ([fare clic per visualizzare l'immagine con dimensioni normali](animation-depending-on-a-condition-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3de73-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3de73-127">[Precedente](executing-several-animations-after-each-other-vb.md)
> [Successivo](picking-one-animation-out-of-a-list-vb.md)</span><span class="sxs-lookup"><span data-stu-id="3de73-127">[Previous](executing-several-animations-after-each-other-vb.md)
[Next](picking-one-animation-out-of-a-list-vb.md)</span></span>
