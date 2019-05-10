---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: Esecuzione di diverse animazioni contemporaneamente (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Consente di eseguire severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: 02eb078a4fb8fddbbac18e4631214f354388cc2f
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65133407"
---
# <a name="executing-several-animations-at-the-same-time-vb"></a><span data-ttu-id="a1d55-104">Esecuzione di diverse animazioni contemporaneamente (VB)</span><span class="sxs-lookup"><span data-stu-id="a1d55-104">Executing Several Animations at The Same Time (VB)</span></span>

<span data-ttu-id="a1d55-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a1d55-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a1d55-106">[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a1d55-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span></span>

> <span data-ttu-id="a1d55-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="a1d55-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a1d55-108">Consente l'esecuzione di diverse animazioni in parallelo.</span><span class="sxs-lookup"><span data-stu-id="a1d55-108">It allows to run several animations in a parallel fashion.</span></span>

## <a name="overview"></a><span data-ttu-id="a1d55-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="a1d55-109">Overview</span></span>

<span data-ttu-id="a1d55-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="a1d55-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a1d55-111">Consente l'esecuzione di diverse animazioni in parallelo.</span><span class="sxs-lookup"><span data-stu-id="a1d55-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="a1d55-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="a1d55-112">Steps</span></span>

<span data-ttu-id="a1d55-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="a1d55-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

<span data-ttu-id="a1d55-114">Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="a1d55-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

<span data-ttu-id="a1d55-115">Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="a1d55-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

<span data-ttu-id="a1d55-116">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="a1d55-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

<span data-ttu-id="a1d55-117">All'interno di `<Animations>` nodo, usare `<OnLoad>` per eseguire le animazioni dopo la pagina è stata caricata completamente.</span><span class="sxs-lookup"><span data-stu-id="a1d55-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="a1d55-118">In generale, `<OnLoad>` accetta solo un'animazione.</span><span class="sxs-lookup"><span data-stu-id="a1d55-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="a1d55-119">Il framework di animazione consente di eseguire il join di diverse animazioni in uno solo tramite il `<Parallel>` elemento.</span><span class="sxs-lookup"><span data-stu-id="a1d55-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="a1d55-120">Tutte le animazioni in `<Parallel>` vengono eseguiti nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="a1d55-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="a1d55-121">Di seguito è riportato l'un markup possibili per il `AnimationExtender` controllo, dissolvenza in uscita e ridimensionare il pannello allo stesso tempo:</span><span class="sxs-lookup"><span data-stu-id="a1d55-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

<span data-ttu-id="a1d55-122">E infatti: quando si esegue questo script, il pannello viene visualizzato, quindi deve essere ridimensionato (più di triplicare la larghezza e dimezzamento dell'altezza) e dissolve nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="a1d55-122">And indeed: when you run this script, the panel is displayed, then resizes (more than tripling its width and halving its height) and fades out at the same time.</span></span>

<span data-ttu-id="a1d55-123">[![Il pannello è dissolvenza in uscita e il ridimensionamento (incluso il relativo contenuto, grazie al motore di rendering del browser)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a1d55-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span></span>

<span data-ttu-id="a1d55-124">Il pannello è dissolvenza in uscita e il ridimensionamento (incluso il relativo contenuto, grazie al motore di rendering del browser) ([fare clic per visualizzare l'immagine con dimensioni normali](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a1d55-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a1d55-125">[Precedente](adding-animation-to-a-control-vb.md)
> [Successivo](executing-several-animations-after-each-other-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a1d55-125">[Previous](adding-animation-to-a-control-vb.md)
[Next](executing-several-animations-after-each-other-vb.md)</span></span>
