---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Uso di TextBoxWatermark con i controlli di convalida (c#) | Microsoft Docs
author: wenz
description: Il controllo di TextBoxWatermark in AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella. Quando un utente fa clic nella casella, lo posso...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 6ed3777e72adbb1a648a6f5215820d597a13bc92
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65124499"
---
# <a name="using-textboxwatermark-with-validation-controls-c"></a><span data-ttu-id="6f756-104">Uso di TextBoxWatermark con i controlli di convalida (C#)</span><span class="sxs-lookup"><span data-stu-id="6f756-104">Using TextBoxWatermark With Validation Controls (C#)</span></span>

<span data-ttu-id="6f756-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6f756-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6f756-106">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="6f756-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span></span>

> <span data-ttu-id="6f756-107">Il controllo di TextBoxWatermark in AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella.</span><span class="sxs-lookup"><span data-stu-id="6f756-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="6f756-108">Quando un utente fa clic nella casella, viene svuotato.</span><span class="sxs-lookup"><span data-stu-id="6f756-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="6f756-109">Se l'utente lascia la casella senza dover immettere testo, viene nuovamente visualizzato il testo precompilato.</span><span class="sxs-lookup"><span data-stu-id="6f756-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="6f756-110">Ciò siano in conflitto con i controlli di convalida ASP.NET nella stessa pagina, ma è possibile risolvere questi problemi.</span><span class="sxs-lookup"><span data-stu-id="6f756-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="overview"></a><span data-ttu-id="6f756-111">Panoramica</span><span class="sxs-lookup"><span data-stu-id="6f756-111">Overview</span></span>

<span data-ttu-id="6f756-112">Il `TextBoxWatermark` controllo in AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella.</span><span class="sxs-lookup"><span data-stu-id="6f756-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="6f756-113">Quando un utente fa clic nella casella, viene svuotato.</span><span class="sxs-lookup"><span data-stu-id="6f756-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="6f756-114">Se l'utente lascia la casella senza dover immettere testo, viene nuovamente visualizzato il testo precompilato.</span><span class="sxs-lookup"><span data-stu-id="6f756-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="6f756-115">Ciò siano in conflitto con i controlli di convalida ASP.NET nella stessa pagina, ma è possibile risolvere questi problemi.</span><span class="sxs-lookup"><span data-stu-id="6f756-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="6f756-116">Passaggi</span><span class="sxs-lookup"><span data-stu-id="6f756-116">Steps</span></span>

<span data-ttu-id="6f756-117">La configurazione di base dell'esempio è il seguente: una `TextBox` controllo è transmux usando un `TextBoxWatermarkExtender` controllo.</span><span class="sxs-lookup"><span data-stu-id="6f756-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="6f756-118">Un pulsante Attiva un postback e verrà successivamente utilizzato per attivare i controlli di convalida della pagina.</span><span class="sxs-lookup"><span data-stu-id="6f756-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="6f756-119">Inoltre, un `ScriptManager` controllo necessario per inizializzare ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="6f756-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="6f756-120">Aggiungere ora un `RequiredFieldValidator` controllo che controlla se vi sia il testo nel campo quando il form viene inviato.</span><span class="sxs-lookup"><span data-stu-id="6f756-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="6f756-121">Il `InitialValue` proprietà del validator deve essere impostata sullo stesso valore che viene utilizzata la `TextBoxWatermarkExtender` controllo: Quando viene inviato il modulo, il valore di una casella di testo non modificato è il valore del limite all'interno di esso:</span><span class="sxs-lookup"><span data-stu-id="6f756-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="6f756-122">Tuttavia si è verificato un problema con questo approccio: Se il client viene disabilitato JavaScript, il campo di testo non è precompilato con il testo della filigrana, pertanto il `RequiredFieldValidator` non genererà un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="6f756-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="6f756-123">Pertanto un secondo `RequiredFieldValidator` è necessario il controllo che verifica la presenza di una casella di testo vuoto (omettendo il `InitialValue` attributo).</span><span class="sxs-lookup"><span data-stu-id="6f756-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="6f756-124">Perché usano entrambi i validator `Display` = `"Dynamic"`, l'utente finale non è possibile distinguere dall'aspetto visivo che le convalide due è stato attivato; al contrario, sembra che si è verificato solo uno di essi.</span><span class="sxs-lookup"><span data-stu-id="6f756-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="6f756-125">Infine, aggiungere codice lato server per il testo nel campo di output se nessun validator rilasciato un messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="6f756-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="6f756-126">[![Visualizza un errore di convalida che non vi sia alcun testo nel campo](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6f756-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="6f756-127">Visualizza un errore di convalida che non vi sia alcun testo nel campo ([fare clic per visualizzare l'immagine con dimensioni normali](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6f756-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6f756-128">[Precedente](using-textboxwatermark-in-a-formview-cs.md)
> [Successivo](using-textboxwatermark-in-a-formview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6f756-128">[Previous](using-textboxwatermark-in-a-formview-cs.md)
[Next](using-textboxwatermark-in-a-formview-vb.md)</span></span>
