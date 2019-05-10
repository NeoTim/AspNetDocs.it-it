---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Data Binding del controllo Slider (c#) | Microsoft Docs
author: wenz
description: Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata usando il mouse. È possibile associare la posizione corrente...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 9613b96612c2799c17bd5633083bb439913b4dc9
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65124854"
---
# <a name="databinding-the-slider-control-c"></a><span data-ttu-id="fbd72-104">Data binding del controllo Slider (C#)</span><span class="sxs-lookup"><span data-stu-id="fbd72-104">Databinding the Slider Control (C#)</span></span>

<span data-ttu-id="fbd72-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fbd72-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fbd72-106">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="fbd72-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span></span>

> <span data-ttu-id="fbd72-107">Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata usando il mouse.</span><span class="sxs-lookup"><span data-stu-id="fbd72-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="fbd72-108">È possibile associare la posizione corrente del dispositivo di scorrimento su un altro controllo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fbd72-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="overview"></a><span data-ttu-id="fbd72-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="fbd72-109">Overview</span></span>

<span data-ttu-id="fbd72-110">Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata usando il mouse.</span><span class="sxs-lookup"><span data-stu-id="fbd72-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="fbd72-111">È possibile associare la posizione corrente del dispositivo di scorrimento su un altro controllo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fbd72-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="fbd72-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="fbd72-112">Steps</span></span>

<span data-ttu-id="fbd72-113">Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="fbd72-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

<span data-ttu-id="fbd72-114">Successivamente, aggiungere due `TextBox` controlli alla pagina.</span><span class="sxs-lookup"><span data-stu-id="fbd72-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="fbd72-115">Uno verrà trasformato in un dispositivo di scorrimento grafico e un altro manterrà la posizione del dispositivo di scorrimento.</span><span class="sxs-lookup"><span data-stu-id="fbd72-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

<span data-ttu-id="fbd72-116">Il passaggio successivo è già il passaggio finale.</span><span class="sxs-lookup"><span data-stu-id="fbd72-116">The next step is already the final step.</span></span> <span data-ttu-id="fbd72-117">Il `SliderExtender` controllo da ASP.NET AJAX Control Toolkit rende un dispositivo di scorrimento dalla casella di testo prima e seconda casella di testo vengono aggiornati automaticamente quando il dispositivo di scorrimento posizionare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="fbd72-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="fbd72-118">In modo che funzioni correttamente, il `SliderExtender`del `TargetControlID` attributo deve essere impostato sull'ID della prima casella di testo; il `BoundControlID` attributo deve essere impostato sull'ID della seconda casella di testo.</span><span class="sxs-lookup"><span data-stu-id="fbd72-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

<span data-ttu-id="fbd72-119">Come può notare nel browser, il data binding funziona in entrambe le direzioni: digitando un nuovo valore nella casella di testo Aggiorna posizione del dispositivo di scorrimento.</span><span class="sxs-lookup"><span data-stu-id="fbd72-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="fbd72-120">Se si apporta la seconda casella di testo di sola lettura, è possibile aggiungere un livello di protezione basso per il campo di testo in modo che sia più difficile per all'utente di aggiornare manualmente il valore in tale posizione.</span><span class="sxs-lookup"><span data-stu-id="fbd72-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>

<span data-ttu-id="fbd72-121">[![Dispositivo di scorrimento e casella di testo sono sincronizzate](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fbd72-121">[![Slider and text box are in sync](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="fbd72-122">Dispositivo di scorrimento e casella di testo vengono sincronizzati ([fare clic per visualizzare l'immagine con dimensioni normali](databinding-the-slider-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fbd72-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fbd72-123">[Precedente](using-the-slider-control-with-auto-postback-cs.md)
> [Successivo](using-the-slider-control-with-auto-postback-vb.md)</span><span class="sxs-lookup"><span data-stu-id="fbd72-123">[Previous](using-the-slider-control-with-auto-postback-cs.md)
[Next](using-the-slider-control-with-auto-postback-vb.md)</span></span>
