---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Uso di più controlli popup (VB) | Microsoft Docs
author: wenz
description: Il dispositivo Extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare un popup quando viene attivato qualsiasi altro controllo. È anche possibile usare m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: e1f4ff64e9fdf48ea63b75c97acd53a64b5ab5ce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611618"
---
# <a name="using-multiple-popup-controls-vb"></a><span data-ttu-id="3735a-104">Uso di più controlli Popup (VB)</span><span class="sxs-lookup"><span data-stu-id="3735a-104">Using Multiple Popup Controls (VB)</span></span>

<span data-ttu-id="3735a-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3735a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3735a-106">[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="3735a-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span></span>

> <span data-ttu-id="3735a-107">Il dispositivo Extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare un popup quando viene attivato qualsiasi altro controllo.</span><span class="sxs-lookup"><span data-stu-id="3735a-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="3735a-108">È anche possibile usare più di un controllo popup in una sola pagina.</span><span class="sxs-lookup"><span data-stu-id="3735a-108">It is also possible to use more than one popup control on one page.</span></span>

## <a name="overview"></a><span data-ttu-id="3735a-109">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="3735a-109">Overview</span></span>

<span data-ttu-id="3735a-110">Il dispositivo Extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare un popup quando viene attivato qualsiasi altro controllo.</span><span class="sxs-lookup"><span data-stu-id="3735a-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="3735a-111">È anche possibile usare più di un controllo popup in una sola pagina.</span><span class="sxs-lookup"><span data-stu-id="3735a-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="3735a-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="3735a-112">Steps</span></span>

<span data-ttu-id="3735a-113">Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="3735a-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="3735a-114">Aggiungere quindi un pannello che funge da popup.</span><span class="sxs-lookup"><span data-stu-id="3735a-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="3735a-115">Nello scenario corrente, il pannello contiene un controllo `Calendar`.</span><span class="sxs-lookup"><span data-stu-id="3735a-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="3735a-116">Per evitare gli aggiornamenti della pagina causati dai postback del calendario, il pannello viene inserito in un controllo `UpdatePanel`:</span><span class="sxs-lookup"><span data-stu-id="3735a-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="3735a-117">La pagina contiene anche due caselle di testo.</span><span class="sxs-lookup"><span data-stu-id="3735a-117">The page also contains two text boxes.</span></span> <span data-ttu-id="3735a-118">Per ogni casella di testo, il popup del calendario verrà visualizzato dopo l'attivazione della casella di testo.</span><span class="sxs-lookup"><span data-stu-id="3735a-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="3735a-119">Estendere ora ognuna delle due caselle di testo con un `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="3735a-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="3735a-120">L'attributo `TargetControlID` fornisce l'ID del controllo associato al dispositivo Extender.</span><span class="sxs-lookup"><span data-stu-id="3735a-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="3735a-121">L'attributo `PopupControlID` contiene l'ID del pannello popup.</span><span class="sxs-lookup"><span data-stu-id="3735a-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="3735a-122">In questo caso, entrambi i dispositivi Extender mostrano lo stesso pannello, ma sono anche disponibili pannelli diversi.</span><span class="sxs-lookup"><span data-stu-id="3735a-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="3735a-123">A questo punto, quando si fa clic all'interno di un campo di testo, viene visualizzato un calendario sotto il campo, che consente di selezionare una data.</span><span class="sxs-lookup"><span data-stu-id="3735a-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="3735a-124">(Il recupero della data selezionata nelle caselle di testo sarà trattato in un'esercitazione diversa).</span><span class="sxs-lookup"><span data-stu-id="3735a-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>

<span data-ttu-id="3735a-125">[![il calendario viene visualizzato quando l'utente fa clic nella casella di testo](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3735a-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="3735a-126">Il calendario viene visualizzato quando l'utente fa clic nella casella[di testo (fare clic per visualizzare l'immagine con dimensioni complete](using-multiple-popup-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3735a-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3735a-127">[Precedente](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Successivo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="3735a-127">[Previous](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span></span>
