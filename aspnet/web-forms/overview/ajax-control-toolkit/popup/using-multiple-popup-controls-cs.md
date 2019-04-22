---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: Uso di più controlli Popup (c#) | Microsoft Docs
author: wenz
description: Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. È anche possibile usare m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 2d13fbfdb8d2fe66c5ff036060b9289017f79d14
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59421537"
---
# <a name="using-multiple-popup-controls-c"></a>Uso di più controlli Popup (C#)

da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)

> Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. È anche possibile usare più di un controllo popup in un'unica pagina.


## <a name="overview"></a>Panoramica

Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. È anche possibile usare più di un controllo popup in un'unica pagina.

## <a name="steps"></a>Passaggi

Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

Successivamente, aggiungere un pannello che funge da finestra popup. Nello scenario corrente, il pannello contiene un `Calendar` controllo. Per evitare la pagina viene aggiornata causata dai postback del calendario, il pannello viene inserito all'interno di un `UpdatePanel` controllo:

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

La pagina contiene inoltre due caselle di testo. Per ogni casella di testo, il calendario popup verranno visualizzati dopo aver attivata la casella di testo.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

A questo punto estendere ciascuna delle due caselle di testo con un `PopupControlExtender`. Il `TargetControlID` attributo fornisce l'ID del controllo associato all'oggetto extender. Il `PopupControlID` attributo contiene l'ID del pannello popup. In questo caso, entrambe le estensioni di visualizzare il pannello stesso, ma sono anche possibili, diversi pannelli.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

Ora ogni volta che si fa clic all'interno di un campo di testo, viene visualizzato un calendario sotto il campo, è possibile selezionare una data. (Ricevendo la data selezionata nelle caselle di testo verrà trattato in un'esercitazione diversa.)


[![Quando l'utente fa clic nella casella di testo viene visualizzato il calendario](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)

Il calendario viene visualizzato quando l'utente fa clic nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](using-multiple-popup-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
