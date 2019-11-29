---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Creazione di caselle di controllo che siC#escludono a vicenda () | Microsoft Docs
author: wenz
description: 'Quando è possibile selezionare solo un set di opzioni, i pulsanti di opzione vengono in genere utilizzati. Si è verificato un svantaggio, tuttavia: una volta selezionato un pulsante di opzione in un gruppo,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: ddc154601752cc856f00dd4f3207952ab7e0e3e0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606531"
---
# <a name="creating-mutually-exclusive-checkboxes-c"></a><span data-ttu-id="bbf6e-104">Creazione di caselle di controllo che si escludono a vicenda (C#)</span><span class="sxs-lookup"><span data-stu-id="bbf6e-104">Creating Mutually Exclusive Checkboxes (C#)</span></span>

<span data-ttu-id="bbf6e-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bbf6e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bbf6e-106">[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="bbf6e-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span></span>

> <span data-ttu-id="bbf6e-107">Quando è possibile selezionare solo un set di opzioni, i pulsanti di opzione vengono in genere utilizzati.</span><span class="sxs-lookup"><span data-stu-id="bbf6e-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="bbf6e-108">Si è verificato un svantaggio, tuttavia: una volta selezionato un pulsante di opzione in un gruppo, non è possibile deselezionare tutti i pulsanti di opzione.</span><span class="sxs-lookup"><span data-stu-id="bbf6e-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="bbf6e-109">Le caselle di controllo possono essere deselezionate in qualsiasi momento, ma non si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="bbf6e-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="bbf6e-110">Questa esercitazione offre il meglio di entrambi gli approcci: caselle di controllo che si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="bbf6e-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="overview"></a><span data-ttu-id="bbf6e-111">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="bbf6e-111">Overview</span></span>

<span data-ttu-id="bbf6e-112">Quando è possibile selezionare solo un set di opzioni, i pulsanti di opzione vengono in genere utilizzati.</span><span class="sxs-lookup"><span data-stu-id="bbf6e-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="bbf6e-113">Si è verificato un svantaggio, tuttavia: una volta selezionato un pulsante di opzione in un gruppo, non è possibile deselezionare tutti i pulsanti di opzione.</span><span class="sxs-lookup"><span data-stu-id="bbf6e-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="bbf6e-114">Le caselle di controllo possono essere deselezionate in qualsiasi momento, ma non si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="bbf6e-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="bbf6e-115">Questa esercitazione offre il meglio di entrambi gli approcci: caselle di controllo che si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="bbf6e-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="bbf6e-116">Passaggi</span><span class="sxs-lookup"><span data-stu-id="bbf6e-116">Steps</span></span>

<span data-ttu-id="bbf6e-117">ASP.NET AJAX Control Toolkit contiene l'estensione MutuallyExclusiveCheckBox.</span><span class="sxs-lookup"><span data-stu-id="bbf6e-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="bbf6e-118">Consente ai programmatori di assegnare qualsiasi casella di controllo a un nome di gruppo (attributo`Key`).</span><span class="sxs-lookup"><span data-stu-id="bbf6e-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="bbf6e-119">Da tutte le caselle di controllo all'interno dello stesso gruppo, è possibile selezionare una sola volta.</span><span class="sxs-lookup"><span data-stu-id="bbf6e-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="bbf6e-120">Iniziamo inserendo due caselle di controllo in una nuova pagina ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bbf6e-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="bbf6e-121">Ci possono essere più, ma due di essi sono sufficienti per illustrare il principio:</span><span class="sxs-lookup"><span data-stu-id="bbf6e-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

<span data-ttu-id="bbf6e-122">Per entrambe le caselle di controllo, è necessario inserire un controllo MutuallyExclusiveCheckBoxExtender nella pagina.</span><span class="sxs-lookup"><span data-stu-id="bbf6e-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="bbf6e-123">Entrambi gli attributi chiave devono avere lo stesso valore, così come gli attributi valore degli elementi del pulsante di opzione HTML devono essere identici per indicare il gruppo a cui appartengono.</span><span class="sxs-lookup"><span data-stu-id="bbf6e-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="bbf6e-124">La proprietà TargetControlID dell'oggetto Extender punta all'ID della casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="bbf6e-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

<span data-ttu-id="bbf6e-125">Includere infine il `ScriptManager` AJAX di ASP.NET, richiesto da tutti gli elementi di ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="bbf6e-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

<span data-ttu-id="bbf6e-126">Salvare ed eseguire la pagina: è possibile selezionare e deselezionare entrambe le caselle di controllo, ma non è possibile selezionare entrambe le caselle di controllo.</span><span class="sxs-lookup"><span data-stu-id="bbf6e-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>

<span data-ttu-id="bbf6e-127">[![è possibile selezionare una sola casella di controllo alla volta](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bbf6e-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span></span>

<span data-ttu-id="bbf6e-128">È possibile controllare una sola casella di controllo alla volta ([fare clic per visualizzare l'immagine con dimensioni complete](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bbf6e-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bbf6e-129">avanti</span><span class="sxs-lookup"><span data-stu-id="bbf6e-129">Next</span></span>](creating-mutually-exclusive-checkboxes-vb.md)
