---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: Uso del Postback automatico con CascadingDropDown (c#) | Microsoft Docs
author: wenz
description: Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichi associati i valori in anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c84951691935d9976f961f65f96fa70633ecbce
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57061328"
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a><span data-ttu-id="3d7b4-103">Uso del postback automatico con CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="3d7b4-103">Using Auto-Postback with CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="3d7b4-104">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3d7b4-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3d7b4-105">[Scaricare il codice](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="3d7b4-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span></span>

> <span data-ttu-id="3d7b4-106">Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList Carica valori in un altro controllo DropDownList associati.</span><span class="sxs-lookup"><span data-stu-id="3d7b4-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="3d7b4-107">Tuttavia quando si usa il controllo CascadingDropDown, ASP. Funzionalità di un postback automatico del controllo DropDownList di NET non funziona, poiché in modo asincrono il caricamento dei dati nell'elenco genera un postback (non necessario) a se stesso.</span><span class="sxs-lookup"><span data-stu-id="3d7b4-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="3d7b4-108">Con oltre a codice JavaScript, è possibile evitare questo effetto.</span><span class="sxs-lookup"><span data-stu-id="3d7b4-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="3d7b4-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="3d7b4-109">Overview</span></span>

<span data-ttu-id="3d7b4-110">Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList Carica valori in un altro controllo DropDownList associati.</span><span class="sxs-lookup"><span data-stu-id="3d7b4-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="3d7b4-111">(Ad esempio, un unico elenco fornisce un elenco di stati degli Stati Uniti e il successivo elenco viene riempito con città più importanti di tale stato.) Tuttavia quando si usa il controllo CascadingDropDown, ASP. Funzionalità di un postback automatico del controllo DropDownList di NET non funziona, poiché in modo asincrono il caricamento dei dati nell'elenco genera un postback (non necessario) a se stesso.</span><span class="sxs-lookup"><span data-stu-id="3d7b4-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="3d7b4-112">Con oltre a codice JavaScript, è possibile evitare questo effetto.</span><span class="sxs-lookup"><span data-stu-id="3d7b4-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="3d7b4-113">Passaggi</span><span class="sxs-lookup"><span data-stu-id="3d7b4-113">Steps</span></span>

<span data-ttu-id="3d7b4-114">Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la &lt; `form` &gt; elemento):</span><span class="sxs-lookup"><span data-stu-id="3d7b4-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="3d7b4-115">Quindi, è necessario un controllo DropDownList:</span><span class="sxs-lookup"><span data-stu-id="3d7b4-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="3d7b4-116">Per questo elenco, viene aggiunto un controllo extender CascadingDropDown, che fornisce informazioni di URL e il metodo di servizio web:</span><span class="sxs-lookup"><span data-stu-id="3d7b4-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="3d7b4-117">Il dispositivo extender CascadingDropDown chiama quindi in modo asincrono un servizio web con la firma del metodo seguente:</span><span class="sxs-lookup"><span data-stu-id="3d7b4-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="3d7b4-118">Il metodo restituisce una matrice di tipo valore CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="3d7b4-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="3d7b4-119">Il costruttore del tipo prevede innanzitutto la didascalia della voce di elenco e quindi il valore (HTML `value` attributo).</span><span class="sxs-lookup"><span data-stu-id="3d7b4-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="3d7b4-120">Il caricamento della pagina nel browser riempirà l'elenco a discesa con i fornitori di tre, il secondo viene preselezionato.</span><span class="sxs-lookup"><span data-stu-id="3d7b4-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="3d7b4-121">Inoltre, ASP.NET definisce il `__doPostBack()` metodo JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3d7b4-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="3d7b4-122">Dopo la pagina è stata caricata, questa chiamata JavaScript viene aggiunto all'elenco a discesa, ma solo se esistono elementi in esso.</span><span class="sxs-lookup"><span data-stu-id="3d7b4-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="3d7b4-123">Se non sono presenti elementi nell'elenco, il Toolkit di controllo sta caricando, in modo che il codice JavaScript Usa un timeout e tenta nuovamente di mezzo secondo.</span><span class="sxs-lookup"><span data-stu-id="3d7b4-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

<span data-ttu-id="3d7b4-124">In questo modo, un postback viene eseguito solo quando vi sono effettivamente gli elementi nell'elenco e l'utente seleziona una voce.</span><span class="sxs-lookup"><span data-stu-id="3d7b4-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="3d7b4-125">[![Se si seleziona un elemento di elenco, un postback](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3d7b4-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="3d7b4-126">Se si seleziona un elemento di elenco, un postback ([fare clic per visualizzare l'immagine con dimensioni normali](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3d7b4-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3d7b4-127">[Precedente](presetting-list-entries-with-cascadingdropdown-cs.md)
> [Successivo](filling-a-list-using-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="3d7b4-127">[Previous](presetting-list-entries-with-cascadingdropdown-cs.md)
[Next](filling-a-list-using-cascadingdropdown-vb.md)</span></span>