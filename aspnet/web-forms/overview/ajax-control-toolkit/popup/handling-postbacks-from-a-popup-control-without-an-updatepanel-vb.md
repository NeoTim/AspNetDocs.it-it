---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Gestione dei postback da un controllo popup senza UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Il dispositivo Extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare un popup quando viene attivato qualsiasi altro controllo. Quando si verifica un postback in su...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: aaecf77c1d25f2c99ef4e9948d79fc01b837169b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611716"
---
# <a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a><span data-ttu-id="f5041-104">Gestione dei postback da un controllo Popup senza UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="f5041-104">Handling Postbacks from A Popup Control Without an UpdatePanel (VB)</span></span>

<span data-ttu-id="f5041-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f5041-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f5041-106">[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f5041-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span></span>

> <span data-ttu-id="f5041-107">Il dispositivo Extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare un popup quando viene attivato qualsiasi altro controllo.</span><span class="sxs-lookup"><span data-stu-id="f5041-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="f5041-108">Quando si verifica un postback in un pannello di questo tipo e nella pagina sono presenti diversi pannelli, è difficile determinare su quale pannello è stato fatto clic.</span><span class="sxs-lookup"><span data-stu-id="f5041-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="overview"></a><span data-ttu-id="f5041-109">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="f5041-109">Overview</span></span>

<span data-ttu-id="f5041-110">Il dispositivo Extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare un popup quando viene attivato qualsiasi altro controllo.</span><span class="sxs-lookup"><span data-stu-id="f5041-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="f5041-111">Quando si verifica un postback in un pannello di questo tipo e nella pagina sono presenti diversi pannelli, è difficile determinare su quale pannello è stato fatto clic.</span><span class="sxs-lookup"><span data-stu-id="f5041-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="f5041-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="f5041-112">Steps</span></span>

<span data-ttu-id="f5041-113">Quando si usa un `PopupControl` con un postback, ma senza avere un `UpdatePanel` nella pagina, il Toolkit di controllo non offre un modo per determinare quale elemento client ha attivato il popup che a sua volta ha causato il postback.</span><span class="sxs-lookup"><span data-stu-id="f5041-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="f5041-114">Un piccolo espediente fornisce tuttavia una soluzione alternativa per questo scenario.</span><span class="sxs-lookup"><span data-stu-id="f5041-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="f5041-115">Innanzitutto, di seguito è illustrata la configurazione di base: due caselle di testo che attivano entrambe lo stesso popup, un calendario.</span><span class="sxs-lookup"><span data-stu-id="f5041-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="f5041-116">Due `PopupControlExtenders` riunire caselle di testo e popup.</span><span class="sxs-lookup"><span data-stu-id="f5041-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="f5041-117">L'idea di base consiste nell'aggiungere un campo del form nascosto nel &lt;`form`elemento &gt; che include la casella di testo che ha avviato il popup:</span><span class="sxs-lookup"><span data-stu-id="f5041-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="f5041-118">Quando la pagina viene caricata, il codice JavaScript aggiunge un gestore eventi a entrambe le caselle di testo: ogni volta che l'utente fa clic su una casella di testo, il nome viene scritto nel campo del form nascosto:</span><span class="sxs-lookup"><span data-stu-id="f5041-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

<span data-ttu-id="f5041-119">Nel codice sul lato server è necessario leggere il valore del campo nascosto.</span><span class="sxs-lookup"><span data-stu-id="f5041-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="f5041-120">Poiché i campi dei moduli nascosti sono semplici da modificare, è necessario un approccio all'elenco di elementi consentiti per convalidare il valore nascosto.</span><span class="sxs-lookup"><span data-stu-id="f5041-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="f5041-121">Una volta identificata la casella di testo corretta, viene scritta la data dal calendario.</span><span class="sxs-lookup"><span data-stu-id="f5041-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]

<span data-ttu-id="f5041-122">[![il calendario viene visualizzato quando l'utente fa clic nella casella di testo](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f5041-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="f5041-123">Il calendario viene visualizzato quando l'utente fa clic nella casella[di testo (fare clic per visualizzare l'immagine con dimensioni complete](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f5041-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span></span>

<span data-ttu-id="f5041-124">[![facendo clic su una data la inserisce nella casella di testo](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="f5041-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="f5041-125">Se si fa clic su una data, la si inserisce nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni complete](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f5041-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f5041-126">Precedente</span><span class="sxs-lookup"><span data-stu-id="f5041-126">Previous</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
