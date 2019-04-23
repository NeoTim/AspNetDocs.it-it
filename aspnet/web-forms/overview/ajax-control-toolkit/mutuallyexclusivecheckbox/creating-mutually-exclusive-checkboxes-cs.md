---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Creazione di caselle di controllo che si escludono a vicenda (c#) | Microsoft Docs
author: wenz
description: "Quando è possibile selezionare solo uno di un set di opzioni, pulsanti di opzione in genere vengono usati. C'è però uno svantaggio: Una volta un pulsante di opzione in un gruppo selezionato,..."
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 01d6d2988278d3d371d93b23bbdf089d83900405
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59397851"
---
# <a name="creating-mutually-exclusive-checkboxes-c"></a><span data-ttu-id="4e7ac-104">Creazione di caselle di controllo che si escludono a vicenda (C#)</span><span class="sxs-lookup"><span data-stu-id="4e7ac-104">Creating Mutually Exclusive Checkboxes (C#)</span></span>

<span data-ttu-id="4e7ac-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4e7ac-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4e7ac-106">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4e7ac-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span></span>

> <span data-ttu-id="4e7ac-107">Quando è possibile selezionare solo uno di un set di opzioni, pulsanti di opzione in genere vengono usati.</span><span class="sxs-lookup"><span data-stu-id="4e7ac-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="4e7ac-108">C'è però uno svantaggio: Dopo aver selezionato un pulsante di opzione in un gruppo, non è possibile deselezionare tutti i pulsanti di opzione.</span><span class="sxs-lookup"><span data-stu-id="4e7ac-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="4e7ac-109">Le caselle di controllo può essere deselezionate in qualsiasi momento, tuttavia non si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="4e7ac-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="4e7ac-110">Questa esercitazione offre il meglio di entrambi gli approcci: caselle di controllo che si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="4e7ac-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="4e7ac-111">Panoramica</span><span class="sxs-lookup"><span data-stu-id="4e7ac-111">Overview</span></span>

<span data-ttu-id="4e7ac-112">Quando è possibile selezionare solo uno di un set di opzioni, pulsanti di opzione in genere vengono usati.</span><span class="sxs-lookup"><span data-stu-id="4e7ac-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="4e7ac-113">C'è però uno svantaggio: Dopo aver selezionato un pulsante di opzione in un gruppo, non è possibile deselezionare tutti i pulsanti di opzione.</span><span class="sxs-lookup"><span data-stu-id="4e7ac-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="4e7ac-114">Le caselle di controllo può essere deselezionate in qualsiasi momento, tuttavia non si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="4e7ac-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="4e7ac-115">Questa esercitazione offre il meglio di entrambi gli approcci: caselle di controllo che si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="4e7ac-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="4e7ac-116">Passaggi</span><span class="sxs-lookup"><span data-stu-id="4e7ac-116">Steps</span></span>

<span data-ttu-id="4e7ac-117">ASP.NET AJAX Control Toolkit contiene il dispositivo extender MutuallyExclusiveCheckBox.</span><span class="sxs-lookup"><span data-stu-id="4e7ac-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="4e7ac-118">Ciò consente ai programmatori di assegnare qualsiasi casella di controllo a un nome di gruppo (`Key` attributo).</span><span class="sxs-lookup"><span data-stu-id="4e7ac-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="4e7ac-119">Da tutte le caselle di controllo nello stesso gruppo, è possibile selezionare solo uno alla volta.</span><span class="sxs-lookup"><span data-stu-id="4e7ac-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="4e7ac-120">Iniziamo con l'inserimento di due caselle di controllo in una nuova pagina ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4e7ac-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="4e7ac-121">Possono esserci più, ma due di essi sono sufficienti per illustrare il principio:</span><span class="sxs-lookup"><span data-stu-id="4e7ac-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

<span data-ttu-id="4e7ac-122">Per entrambe le caselle di controllo, è necessario inserire un controllo MutuallyExclusiveCheckBoxExtender nella pagina.</span><span class="sxs-lookup"><span data-stu-id="4e7ac-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="4e7ac-123">Entrambi gli attributi chiave devono avere lo stesso valore, proprio come il valore degli attributi degli elementi di pulsante di opzione HTML devono essere identici per indicare il gruppo che a cui appartengono.</span><span class="sxs-lookup"><span data-stu-id="4e7ac-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="4e7ac-124">La proprietà TargetControlID dell'extender punta all'ID della casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="4e7ac-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

<span data-ttu-id="4e7ac-125">Infine, includere ASP.NET AJAX `ScriptManager` necessaria per tutti gli elementi di ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="4e7ac-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

<span data-ttu-id="4e7ac-126">Salvare ed eseguire la pagina: È possibile selezionare e deselezionare caselle di controllo, tuttavia in nessuna circostanza entrambe le caselle di controllo controllare.</span><span class="sxs-lookup"><span data-stu-id="4e7ac-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="4e7ac-127">[![Solo una casella di controllo può essere controllato in un momento](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4e7ac-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span></span>

<span data-ttu-id="4e7ac-128">Solo una casella di controllo può essere controllato in un momento ([fare clic per visualizzare l'immagine con dimensioni normali](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4e7ac-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4e7ac-129">avanti</span><span class="sxs-lookup"><span data-stu-id="4e7ac-129">Next</span></span>](creating-mutually-exclusive-checkboxes-vb.md)
