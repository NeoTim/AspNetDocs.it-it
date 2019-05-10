---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: Uso di TextBoxWatermark con i controlli di convalida (VB) | Microsoft Docs
author: wenz
description: Il controllo di TextBoxWatermark in AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella. Quando un utente fa clic nella casella, lo posso...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 63363a8e25a351c23f89186ef3d9c44355ba3c44
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65132484"
---
# <a name="using-textboxwatermark-with-validation-controls-vb"></a><span data-ttu-id="1591b-104">Uso di TextBoxWatermark con i controlli di convalida (VB)</span><span class="sxs-lookup"><span data-stu-id="1591b-104">Using TextBoxWatermark With Validation Controls (VB)</span></span>

<span data-ttu-id="1591b-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1591b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1591b-106">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="1591b-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span></span>

> <span data-ttu-id="1591b-107">Il controllo di TextBoxWatermark in AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella.</span><span class="sxs-lookup"><span data-stu-id="1591b-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="1591b-108">Quando un utente fa clic nella casella, viene svuotato.</span><span class="sxs-lookup"><span data-stu-id="1591b-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="1591b-109">Se l'utente lascia la casella senza dover immettere testo, viene nuovamente visualizzato il testo precompilato.</span><span class="sxs-lookup"><span data-stu-id="1591b-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="1591b-110">Ciò siano in conflitto con i controlli di convalida ASP.NET nella stessa pagina, ma è possibile risolvere questi problemi.</span><span class="sxs-lookup"><span data-stu-id="1591b-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="overview"></a><span data-ttu-id="1591b-111">Panoramica</span><span class="sxs-lookup"><span data-stu-id="1591b-111">Overview</span></span>

<span data-ttu-id="1591b-112">Il `TextBoxWatermark` controllo in AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella.</span><span class="sxs-lookup"><span data-stu-id="1591b-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="1591b-113">Quando un utente fa clic nella casella, viene svuotato.</span><span class="sxs-lookup"><span data-stu-id="1591b-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="1591b-114">Se l'utente lascia la casella senza dover immettere testo, viene nuovamente visualizzato il testo precompilato.</span><span class="sxs-lookup"><span data-stu-id="1591b-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="1591b-115">Ciò siano in conflitto con i controlli di convalida ASP.NET nella stessa pagina, ma è possibile risolvere questi problemi.</span><span class="sxs-lookup"><span data-stu-id="1591b-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="1591b-116">Passaggi</span><span class="sxs-lookup"><span data-stu-id="1591b-116">Steps</span></span>

<span data-ttu-id="1591b-117">La configurazione di base dell'esempio è il seguente: una `TextBox` controllo è transmux usando un `TextBoxWatermarkExtender` controllo.</span><span class="sxs-lookup"><span data-stu-id="1591b-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="1591b-118">Un pulsante Attiva un postback e verrà successivamente utilizzato per attivare i controlli di convalida della pagina.</span><span class="sxs-lookup"><span data-stu-id="1591b-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="1591b-119">Inoltre, un `ScriptManager` controllo necessario per inizializzare ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="1591b-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="1591b-120">Aggiungere ora un `RequiredFieldValidator` controllo che controlla se vi sia il testo nel campo quando il form viene inviato.</span><span class="sxs-lookup"><span data-stu-id="1591b-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="1591b-121">Il `InitialValue` proprietà del validator deve essere impostata sullo stesso valore che viene utilizzata la `TextBoxWatermarkExtender` controllo: Quando viene inviato il modulo, il valore di una casella di testo non modificato è il valore del limite all'interno di esso:</span><span class="sxs-lookup"><span data-stu-id="1591b-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="1591b-122">Tuttavia si è verificato un problema con questo approccio: Se il client viene disabilitato JavaScript, il campo di testo non è precompilato con il testo della filigrana, pertanto il `RequiredFieldValidator` non genererà un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="1591b-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="1591b-123">Pertanto un secondo `RequiredFieldValidator` è necessario il controllo che verifica la presenza di una casella di testo vuoto (omettendo il `InitialValue` attributo).</span><span class="sxs-lookup"><span data-stu-id="1591b-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="1591b-124">Perché usano entrambi i validator `Display` = `"Dynamic"`, l'utente finale non è possibile distinguere dall'aspetto visivo che le convalide due è stato attivato; al contrario, sembra che si è verificato solo uno di essi.</span><span class="sxs-lookup"><span data-stu-id="1591b-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="1591b-125">Infine, aggiungere codice lato server per il testo nel campo di output se nessun validator rilasciato un messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="1591b-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="1591b-126">[![Visualizza un errore di convalida che non vi sia alcun testo nel campo](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1591b-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="1591b-127">Visualizza un errore di convalida che non vi sia alcun testo nel campo ([fare clic per visualizzare l'immagine con dimensioni normali](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1591b-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1591b-128">Precedente</span><span class="sxs-lookup"><span data-stu-id="1591b-128">Previous</span></span>](using-textboxwatermark-in-a-formview-vb.md)
