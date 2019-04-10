---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Uso di postback con ReorderList (VB) | Microsoft Docs
author: wenz
description: Il controllo ReorderList in AJAX Control Toolkit fornisce un elenco che possa essere riordinato in base all'utente tramite trascinamento della selezione. Ogni volta che l'elenco viene riordinato, un ordine di acquisto...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 22c7eecb841ff67196d21e6efeeda63a3456c5cb
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59409083"
---
# <a name="using-postbacks-with-reorderlist-vb"></a><span data-ttu-id="33ae0-104">Uso di postback con ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="33ae0-104">Using Postbacks with ReorderList (VB)</span></span>

<span data-ttu-id="33ae0-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="33ae0-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="33ae0-106">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="33ae0-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span></span>

> <span data-ttu-id="33ae0-107">Il controllo ReorderList in AJAX Control Toolkit fornisce un elenco che possa essere riordinato in base all'utente tramite trascinamento della selezione.</span><span class="sxs-lookup"><span data-stu-id="33ae0-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="33ae0-108">Ogni volta che l'elenco viene riordinato, un postback informa il server della modifica.</span><span class="sxs-lookup"><span data-stu-id="33ae0-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="33ae0-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="33ae0-109">Overview</span></span>

<span data-ttu-id="33ae0-110">Il `ReorderList` controllo in AJAX Control Toolkit fornisce un elenco che possa essere riordinato in base all'utente tramite trascinamento della selezione.</span><span class="sxs-lookup"><span data-stu-id="33ae0-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="33ae0-111">Ogni volta che l'elenco viene riordinato, un postback informa il server della modifica.</span><span class="sxs-lookup"><span data-stu-id="33ae0-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="33ae0-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="33ae0-112">Steps</span></span>

<span data-ttu-id="33ae0-113">Esistono varie fonti di dati possibili per il `ReorderList` controllo.</span><span class="sxs-lookup"><span data-stu-id="33ae0-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="33ae0-114">Uno consiste nell'usare un `XmlDataSource` controllo:</span><span class="sxs-lookup"><span data-stu-id="33ae0-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="33ae0-115">Per associare il codice XML per un `ReorderList` devono essere impostati controllo e abilitare i postback, gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="33ae0-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- `DataSourceID`<span data-ttu-id="33ae0-116">: L'ID dell'origine dati</span><span class="sxs-lookup"><span data-stu-id="33ae0-116">: The ID of the data source</span></span>
- `SortOrderField`<span data-ttu-id="33ae0-117">: La proprietà da ordinare</span><span class="sxs-lookup"><span data-stu-id="33ae0-117">: The property to sort by</span></span>
- `AllowReorder`<span data-ttu-id="33ae0-118">: Se si desidera consentire all'utente di riordinare gli elementi dell'elenco</span><span class="sxs-lookup"><span data-stu-id="33ae0-118">: Whether to allow the user to reorder the list elements</span></span>
- `PostBackOnReorder`<span data-ttu-id="33ae0-119">: Se si desidera creare un postback ogni volta che l'elenco viene ridisposto</span><span class="sxs-lookup"><span data-stu-id="33ae0-119">: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="33ae0-120">Ecco il markup per il controllo appropriato:</span><span class="sxs-lookup"><span data-stu-id="33ae0-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="33ae0-121">All'interno di `ReorderList` controllo, i dati specifici dell'origine dati può essere associato usando la `Eval()` metodo:</span><span class="sxs-lookup"><span data-stu-id="33ae0-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

<span data-ttu-id="33ae0-122">In una posizione arbitraria nella pagina, un'etichetta conterrà le informazioni quando si è verificato il riordinamento ultimo:</span><span class="sxs-lookup"><span data-stu-id="33ae0-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="33ae0-123">Questa etichetta viene riempita con il testo nel codice lato server, la gestione del postback:</span><span class="sxs-lookup"><span data-stu-id="33ae0-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

<span data-ttu-id="33ae0-124">Infine, in modo da attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito nella pagina:</span><span class="sxs-lookup"><span data-stu-id="33ae0-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


[![E<span data-ttu-id="33ae0-125">riordinamento ACH attiva un postback]</span><span class="sxs-lookup"><span data-stu-id="33ae0-125">ach reordering triggers a postback]</span></span>(using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)

<span data-ttu-id="33ae0-126">Ogni riordinamento attiva un postback ([fare clic per visualizzare l'immagine con dimensioni normali](using-postbacks-with-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="33ae0-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="33ae0-127">[Precedente](drag-and-drop-via-reorderlist-cs.md)
> [Successivo](drag-and-drop-via-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="33ae0-127">[Previous](drag-and-drop-via-reorderlist-cs.md)
[Next](drag-and-drop-via-reorderlist-vb.md)</span></span>
