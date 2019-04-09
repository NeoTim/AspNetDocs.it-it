---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: Esecuzione di diverse animazioni contemporaneamente (c#) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Consente di eseguire severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: 0d9c566a301c8b64e33e67b0e9415a5955b5436e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59388218"
---
# <a name="executing-several-animations-at-the-same-time-c"></a><span data-ttu-id="8b6d0-104">Esecuzione di diverse animazioni contemporaneamente (c#)</span><span class="sxs-lookup"><span data-stu-id="8b6d0-104">Executing Several Animations at The Same Time (C#)</span></span>

<span data-ttu-id="8b6d0-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8b6d0-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8b6d0-106">[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="8b6d0-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span></span>

> <span data-ttu-id="8b6d0-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="8b6d0-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8b6d0-108">Consente l'esecuzione di diverse animazioni in parallelo.</span><span class="sxs-lookup"><span data-stu-id="8b6d0-108">It allows to run several animations in a parallel fashion.</span></span>


## <a name="overview"></a><span data-ttu-id="8b6d0-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="8b6d0-109">Overview</span></span>

<span data-ttu-id="8b6d0-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="8b6d0-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8b6d0-111">Consente l'esecuzione di diverse animazioni in parallelo.</span><span class="sxs-lookup"><span data-stu-id="8b6d0-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="8b6d0-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="8b6d0-112">Steps</span></span>

<span data-ttu-id="8b6d0-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="8b6d0-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

<span data-ttu-id="8b6d0-114">Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="8b6d0-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

<span data-ttu-id="8b6d0-115">Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="8b6d0-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

<span data-ttu-id="8b6d0-116">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="8b6d0-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

<span data-ttu-id="8b6d0-117">All'interno di `<Animations>` nodo, usare `<OnLoad>` per eseguire le animazioni dopo la pagina è stata caricata completamente.</span><span class="sxs-lookup"><span data-stu-id="8b6d0-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="8b6d0-118">In generale, `<OnLoad>` accetta solo un'animazione.</span><span class="sxs-lookup"><span data-stu-id="8b6d0-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="8b6d0-119">Il framework di animazione consente di eseguire il join di diverse animazioni in uno solo tramite il `<Parallel>` elemento.</span><span class="sxs-lookup"><span data-stu-id="8b6d0-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="8b6d0-120">Tutte le animazioni in `<Parallel>` vengono eseguiti nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="8b6d0-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="8b6d0-121">Di seguito è riportato l'un markup possibili per il `AnimationExtender` controllo, dissolvenza in uscita e ridimensionare il pannello allo stesso tempo:</span><span class="sxs-lookup"><span data-stu-id="8b6d0-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

<span data-ttu-id="8b6d0-122">E infatti: quando si esegue questo script, il pannello viene visualizzato, quindi deve essere ridimensionato (più di triplicare la larghezza e dimezzamento dell'altezza) e dissolve nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="8b6d0-122">And indeed: when you run this script, the panel is displayed, then resizes (more than tripling its width and halving its height) and fades out at the same time.</span></span>


[![T<span data-ttu-id="8b6d0-123">Pannello he è dissolvenza in uscita e il ridimensionamento (incluso il relativo contenuto, grazie al motore di rendering del browser)]</span><span class="sxs-lookup"><span data-stu-id="8b6d0-123">he panel is fading out and resizing (including its content, thanks to the browser's rendering engine)]</span></span>(executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)

<span data-ttu-id="8b6d0-124">Il pannello è dissolvenza in uscita e il ridimensionamento (incluso il relativo contenuto, grazie al motore di rendering del browser) ([fare clic per visualizzare l'immagine con dimensioni normali](executing-several-animations-at-the-same-time-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8b6d0-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8b6d0-125">[Precedente](adding-animation-to-a-control-cs.md)
> [Successivo](executing-several-animations-after-each-other-cs.md)</span><span class="sxs-lookup"><span data-stu-id="8b6d0-125">[Previous](adding-animation-to-a-control-cs.md)
[Next](executing-several-animations-after-each-other-cs.md)</span></span>
