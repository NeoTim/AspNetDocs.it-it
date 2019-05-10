---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: Trascinamento della selezione tramite ReorderList (VB) | Microsoft Docs
author: wenz
description: /data-access/tutorials/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 72c697bc2a2005d3ff116cf2f73d80e23bb526dd
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65124914"
---
# <a name="drag-and-drop-via-reorderlist-vb"></a>Trascinamento della selezione tramite ReorderList (VB)

da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)

> Il controllo ReorderList in AJAX Control Toolkit fornisce un elenco che possa essere riordinato in base all'utente tramite trascinamento della selezione. L'ordine corrente dell'elenco dovrà essere resi persistenti nel server.

## <a name="overview"></a>Panoramica

Il `ReorderList` controllo in AJAX Control Toolkit fornisce un elenco che possa essere riordinato in base all'utente tramite trascinamento della selezione. L'ordine corrente dell'elenco dovrà essere resi persistenti nel server.

## <a name="steps"></a>Passaggi

Il `ReorderList` controllo supporta il data binding da un database all'elenco. Soprattutto, supporta anche la scrittura delle modifiche all'ordine l'elemento di elenco nuovamente all'archivio dati.

In questo esempio Usa Microsoft SQL Server 2005 Express Edition come archivio dati. Il database è una parte facoltativa (e gratuita) di un'installazione di Visual Studio, compresa la express edition. È anche disponibile come download separato sotto [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è anche l'impostazione predefinita. Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.

Il modo più semplice per configurare il database è di utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ). Connettersi al server, fare doppio clic su `Databases` e creare un nuovo database (pulsante destro del mouse e scegliere `New Database`) denominato `Tutorials`.

In questo database, creare una nuova tabella denominata `AJAX` con le quattro colonne seguenti:

- `id` (integer chiave, primario, identità, non NULL)
- `char` (char(1), NULL)
- `description` (varchar(50), NULL)
- `position` (int, valore NULL)

[![Il layout della tabella di AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)

Il layout della tabella AJAX ([fare clic per visualizzare l'immagine con dimensioni normali](drag-and-drop-via-reorderlist-vb/_static/image3.png))

Successivamente, compilare la tabella con un paio di valori. Si noti che il `position` colonna contiene l'ordinamento degli elementi.

[![I dati iniziali della tabella di AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)

I dati iniziali della tabella di AJAX ([fare clic per visualizzare l'immagine con dimensioni normali](drag-and-drop-via-reorderlist-vb/_static/image6.png))

Il passaggio successivo è necessario per generare un `SqlDataSource` controllo per comunicare con il nuovo database e la relativa tabella. L'origine dati deve supportare le `SELECT` e `UPDATE` comandi SQL. Quando viene modificata in seguito l'ordine degli elementi dell'elenco, il `ReorderList` controllo Invia automaticamente i due valori dell'origine dati `Update` comando: la nuova posizione e l'ID dell'elemento. Pertanto, l'origine dati esigenze un `<UpdateParameters>` sezione per questi due valori:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

Il `ReorderList` controllo deve impostare gli attributi seguenti:

- `AllowReorder`: Indica se gli elementi dell'elenco possono essere riorganizzati
- `DataSourceID`: L'ID dell'origine dati
- `DataKeyField`: Il nome della colonna chiave primaria nell'origine dati
- `SortOrderField`: La colonna di origine dati che fornisce l'ordinamento per gli elementi dell'elenco

Nel `<DragHandleTemplate>` e `<ItemTemplate>` sezioni, il layout dell'elenco è possibile ottimizzare. Inoltre, l'associazione dati è possibile utilizzando il `Eval()` metodo, come illustrato di seguito:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

Le informazioni di stile CSS riportato di seguito (indicate nel `<DragHandleTemplate>` sezione del `ReorderList` controllo) garantisce che il puntatore del mouse cambia in modo appropriato quando posiziona il mouse sul quadratino di trascinamento:

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

Infine, un `ScriptManager` controllo Inizializza ASP.NET AJAX per la pagina:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

Eseguire questo esempio nel browser e ridisporre un po' gli elementi dell'elenco. Quindi, ricaricare la pagina e/o esaminare il database. Le posizioni modificate sono state mantenute e si riflettono anche in base ai valori di `position` colonna nel database e che tutto senza alcun codice, con un semplice utilizzo dei markup.

[![I dati delle modifiche del database in base al nuovo ordine di elemento di elenco](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)

I dati delle modifiche del database in base al nuovo elenco di elementi ordine ([fare clic per visualizzare l'immagine con dimensioni normali](drag-and-drop-via-reorderlist-vb/_static/image9.png))

> [!div class="step-by-step"]
> [Precedente](using-postbacks-with-reorderlist-vb.md)
