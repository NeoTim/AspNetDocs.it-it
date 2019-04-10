---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
title: Trascinamento della selezione tramite ReorderList (c#) | Microsoft Docs
author: wenz
description: Il controllo ReorderList in AJAX Control Toolkit fornisce un elenco che possa essere riordinato in base all'utente tramite trascinamento della selezione. L'ordine di elenco è...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6350ee8e-11d6-4aff-b51c-942878014835
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 988aa9252cfd93067888734006e6003347f1fb5e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59414751"
---
# <a name="drag-and-drop-via-reorderlist-c"></a><span data-ttu-id="312bd-104">Trascinamento della selezione tramite ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="312bd-104">Drag and Drop via ReorderList (C#)</span></span>

<span data-ttu-id="312bd-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="312bd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="312bd-106">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="312bd-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span></span>

> <span data-ttu-id="312bd-107">Il controllo ReorderList in AJAX Control Toolkit fornisce un elenco che possa essere riordinato in base all'utente tramite trascinamento della selezione.</span><span class="sxs-lookup"><span data-stu-id="312bd-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="312bd-108">L'ordine corrente dell'elenco dovrà essere resi persistenti nel server.</span><span class="sxs-lookup"><span data-stu-id="312bd-108">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="312bd-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="312bd-109">Overview</span></span>

<span data-ttu-id="312bd-110">Il `ReorderList` controllo in AJAX Control Toolkit fornisce un elenco che possa essere riordinato in base all'utente tramite trascinamento della selezione.</span><span class="sxs-lookup"><span data-stu-id="312bd-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="312bd-111">L'ordine corrente dell'elenco dovrà essere resi persistenti nel server.</span><span class="sxs-lookup"><span data-stu-id="312bd-111">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="312bd-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="312bd-112">Steps</span></span>

<span data-ttu-id="312bd-113">Il `ReorderList` controllo supporta il data binding da un database all'elenco.</span><span class="sxs-lookup"><span data-stu-id="312bd-113">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="312bd-114">Soprattutto, supporta anche la scrittura delle modifiche all'ordine l'elemento di elenco nuovamente all'archivio dati.</span><span class="sxs-lookup"><span data-stu-id="312bd-114">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="312bd-115">In questo esempio Usa Microsoft SQL Server 2005 Express Edition come archivio dati.</span><span class="sxs-lookup"><span data-stu-id="312bd-115">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="312bd-116">Il database è una parte facoltativa (e gratuita) di un'installazione di Visual Studio, compresa la express edition.</span><span class="sxs-lookup"><span data-stu-id="312bd-116">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="312bd-117">È anche disponibile come download separato sotto [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="312bd-117">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="312bd-118">In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è anche l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="312bd-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="312bd-119">Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="312bd-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="312bd-120">Il modo più semplice per configurare il database è di utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="312bd-120">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="312bd-121">Connettersi al server, fare doppio clic su `Databases` e creare un nuovo database (pulsante destro del mouse e scegliere `New Database`) denominato `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="312bd-121">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="312bd-122">In questo database, creare una nuova tabella denominata `AJAX` con le quattro colonne seguenti:</span><span class="sxs-lookup"><span data-stu-id="312bd-122">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- `id` <span data-ttu-id="312bd-123">(integer chiave, primario, identità, non NULL)</span><span class="sxs-lookup"><span data-stu-id="312bd-123">(primary key, integer, identity, not NULL)</span></span>
- `char` <span data-ttu-id="312bd-124">(char(1), NULL)</span><span class="sxs-lookup"><span data-stu-id="312bd-124">(char(1), NULL)</span></span>
- `description` <span data-ttu-id="312bd-125">(varchar(50), NULL)</span><span class="sxs-lookup"><span data-stu-id="312bd-125">(varchar(50), NULL)</span></span>
- `position` <span data-ttu-id="312bd-126">(int, valore NULL)</span><span class="sxs-lookup"><span data-stu-id="312bd-126">(int, NULL)</span></span>


[![T<span data-ttu-id="312bd-127">layout he della tabella AJAX]</span><span class="sxs-lookup"><span data-stu-id="312bd-127">he layout of the AJAX table]</span></span>(drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)

<span data-ttu-id="312bd-128">Il layout della tabella AJAX ([fare clic per visualizzare l'immagine con dimensioni normali](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="312bd-128">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span></span>


<span data-ttu-id="312bd-129">Successivamente, compilare la tabella con un paio di valori.</span><span class="sxs-lookup"><span data-stu-id="312bd-129">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="312bd-130">Si noti che il `position` colonna contiene l'ordinamento degli elementi.</span><span class="sxs-lookup"><span data-stu-id="312bd-130">Note that the `position` column holds the sort order of the elements.</span></span>


[![T<span data-ttu-id="312bd-131">data iniziale he della tabella di AJAX]</span><span class="sxs-lookup"><span data-stu-id="312bd-131">he initial data in the AJAX table]</span></span>(drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)

<span data-ttu-id="312bd-132">I dati iniziali della tabella di AJAX ([fare clic per visualizzare l'immagine con dimensioni normali](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="312bd-132">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span></span>


<span data-ttu-id="312bd-133">Il passaggio successivo è necessario per generare un `SqlDataSource` controllo per comunicare con il nuovo database e la relativa tabella.</span><span class="sxs-lookup"><span data-stu-id="312bd-133">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="312bd-134">L'origine dati deve supportare le `SELECT` e `UPDATE` comandi SQL.</span><span class="sxs-lookup"><span data-stu-id="312bd-134">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="312bd-135">Quando viene modificata in seguito l'ordine degli elementi dell'elenco, il `ReorderList` controllo Invia automaticamente i due valori dell'origine dati `Update` comando: la nuova posizione e l'ID dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="312bd-135">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="312bd-136">Pertanto, l'origine dati esigenze un `<UpdateParameters>` sezione per questi due valori:</span><span class="sxs-lookup"><span data-stu-id="312bd-136">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="312bd-137">Il `ReorderList` controllo deve impostare gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="312bd-137">The `ReorderList` control needs to set the following attributes:</span></span>

- `AllowReorder`<span data-ttu-id="312bd-138">: Indica se gli elementi dell'elenco possono essere riorganizzati</span><span class="sxs-lookup"><span data-stu-id="312bd-138">: Whether the list items may be rearranged</span></span>
- `DataSourceID`<span data-ttu-id="312bd-139">: L'ID dell'origine dati</span><span class="sxs-lookup"><span data-stu-id="312bd-139">: The ID of the data source</span></span>
- `DataKeyField`<span data-ttu-id="312bd-140">: Il nome della colonna chiave primaria nell'origine dati</span><span class="sxs-lookup"><span data-stu-id="312bd-140">: The name of the primary key column in the data source</span></span>
- `SortOrderField`<span data-ttu-id="312bd-141">: La colonna di origine dati che fornisce l'ordinamento per gli elementi dell'elenco</span><span class="sxs-lookup"><span data-stu-id="312bd-141">: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="312bd-142">Nel `<DragHandleTemplate>` e `<ItemTemplate>` sezioni, il layout dell'elenco è possibile ottimizzare.</span><span class="sxs-lookup"><span data-stu-id="312bd-142">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="312bd-143">Inoltre, l'associazione dati è possibile utilizzando il `Eval()` metodo, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="312bd-143">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="312bd-144">Le informazioni di stile CSS riportato di seguito (indicate nel `<DragHandleTemplate>` sezione del `ReorderList` controllo) garantisce che il puntatore del mouse cambia in modo appropriato quando posiziona il mouse sul quadratino di trascinamento:</span><span class="sxs-lookup"><span data-stu-id="312bd-144">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

<span data-ttu-id="312bd-145">Infine, un `ScriptManager` controllo Inizializza ASP.NET AJAX per la pagina:</span><span class="sxs-lookup"><span data-stu-id="312bd-145">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="312bd-146">Eseguire questo esempio nel browser e ridisporre un po' gli elementi dell'elenco.</span><span class="sxs-lookup"><span data-stu-id="312bd-146">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="312bd-147">Quindi, ricaricare la pagina e/o esaminare il database.</span><span class="sxs-lookup"><span data-stu-id="312bd-147">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="312bd-148">Le posizioni modificate sono state mantenute e si riflettono anche in base ai valori di `position` colonna nel database e che tutto senza alcun codice, con un semplice utilizzo dei markup.</span><span class="sxs-lookup"><span data-stu-id="312bd-148">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


[![T<span data-ttu-id="312bd-149">ha dati in modifiche del database secondo l'ordine dell'elemento elenco nuovo]</span><span class="sxs-lookup"><span data-stu-id="312bd-149">he data in the database changes according to the new list item order]</span></span>(drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)

<span data-ttu-id="312bd-150">I dati delle modifiche del database in base al nuovo elenco di elementi ordine ([fare clic per visualizzare l'immagine con dimensioni normali](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="312bd-150">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="312bd-151">[Precedente](using-postbacks-with-reorderlist-cs.md)
> [Successivo](using-postbacks-with-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="312bd-151">[Previous](using-postbacks-with-reorderlist-cs.md)
[Next](using-postbacks-with-reorderlist-vb.md)</span></span>
