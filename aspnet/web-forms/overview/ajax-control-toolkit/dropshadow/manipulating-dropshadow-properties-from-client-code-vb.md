---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Modifica delle proprietà di DropShadow dal codice Client (VB) | Microsoft Docs
author: wenz
description: Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura. Proprietà di questo tipo di estensione possono essere modificate anche tramite client JavaScript...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: d8b8330174f6f49e96c42a0e15eeaf934bf24f87
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048368"
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="97b50-104">Modifica delle proprietà di DropShadow dal codice client (VB)</span><span class="sxs-lookup"><span data-stu-id="97b50-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>
====================
<span data-ttu-id="97b50-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="97b50-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="97b50-106">[Scaricare il codice](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="97b50-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="97b50-107">Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="97b50-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="97b50-108">Proprietà di questo tipo di estensione possono essere modificate anche con il codice JavaScript client.</span><span class="sxs-lookup"><span data-stu-id="97b50-108">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="97b50-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="97b50-109">Overview</span></span>

<span data-ttu-id="97b50-110">Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="97b50-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="97b50-111">Proprietà di questo tipo di estensione possono essere modificate anche con il codice JavaScript client.</span><span class="sxs-lookup"><span data-stu-id="97b50-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="97b50-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="97b50-112">Steps</span></span>

<span data-ttu-id="97b50-113">Il codice inizia con un pannello che contiene alcune righe di testo:</span><span class="sxs-lookup"><span data-stu-id="97b50-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="97b50-114">La classe CSS associata offre il pannello di un colore di sfondo interessante:</span><span class="sxs-lookup"><span data-stu-id="97b50-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="97b50-115">Il `DropShadowExtender` viene aggiunto per estendere il pannello con un effetto di ombreggiatura, impostato su 50% di opacità:</span><span class="sxs-lookup"><span data-stu-id="97b50-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="97b50-116">Quindi, ASP.NET AJAX `ScriptManager` controllo Abilita il Toolkit di controllo lavorare:</span><span class="sxs-lookup"><span data-stu-id="97b50-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="97b50-117">Un altro pannello contiene due collegamenti di JavaScript per l'impostazione di opacità dell'ombreggiatura: il collegamento meno ridurre l'opacità dell'ombreggiatura, aumenta il collegamento di più.</span><span class="sxs-lookup"><span data-stu-id="97b50-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="97b50-118">La funzione JavaScript `changeOpacity()` quindi necessario trovare prima il `DropShadowExtender` controllo nella pagina.</span><span class="sxs-lookup"><span data-stu-id="97b50-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="97b50-119">ASP.NET AJAX definisce il `$find()` metodo per esattamente tale attività.</span><span class="sxs-lookup"><span data-stu-id="97b50-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="97b50-120">Successivamente, il `get_Opacity()` che consente di recuperare l'opacità corrente, il `set_Opacity()` lo imposta metodo.</span><span class="sxs-lookup"><span data-stu-id="97b50-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="97b50-121">Il codice JavaScript inserisce quindi il valore di opacità nel `<label>` elemento:</span><span class="sxs-lookup"><span data-stu-id="97b50-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


<span data-ttu-id="97b50-122">[![L'opacità viene modificato sul lato client](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="97b50-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="97b50-123">L'opacità viene modificato sul lato client ([fare clic per visualizzare l'immagine con dimensioni normali](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="97b50-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="97b50-124">Precedente</span><span class="sxs-lookup"><span data-stu-id="97b50-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
