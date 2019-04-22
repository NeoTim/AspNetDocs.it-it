---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Data Binding del controllo Slider (c#) | Microsoft Docs
author: wenz
description: Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata usando il mouse. È possibile associare la posizione corrente...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 3f966869c106416886dfa4e9e3c2cf67ee5fe337
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59385813"
---
# <a name="databinding-the-slider-control-c"></a>Data binding del controllo Slider (C#)

da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)

> Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata usando il mouse. È possibile associare la posizione corrente del dispositivo di scorrimento su un altro controllo ASP.NET.


## <a name="overview"></a>Panoramica

Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata usando il mouse. È possibile associare la posizione corrente del dispositivo di scorrimento su un altro controllo ASP.NET.

## <a name="steps"></a>Passaggi

Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

Successivamente, aggiungere due `TextBox` controlli alla pagina. Uno verrà trasformato in un dispositivo di scorrimento grafico e un altro manterrà la posizione del dispositivo di scorrimento.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

Il passaggio successivo è già il passaggio finale. Il `SliderExtender` controllo da ASP.NET AJAX Control Toolkit rende un dispositivo di scorrimento dalla casella di testo prima e seconda casella di testo vengono aggiornati automaticamente quando il dispositivo di scorrimento posizionare le modifiche. In modo che funzioni correttamente, il `SliderExtender`del `TargetControlID` attributo deve essere impostato sull'ID della prima casella di testo; il `BoundControlID` attributo deve essere impostato sull'ID della seconda casella di testo.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

Come può notare nel browser, il data binding funziona in entrambe le direzioni: digitando un nuovo valore nella casella di testo Aggiorna posizione del dispositivo di scorrimento. Se si apporta la seconda casella di testo di sola lettura, è possibile aggiungere un livello di protezione basso per il campo di testo in modo che sia più difficile per all'utente di aggiornare manualmente il valore in tale posizione.


[![Dispositivo di scorrimento e casella di testo sono sincronizzate](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)

Dispositivo di scorrimento e casella di testo vengono sincronizzati ([fare clic per visualizzare l'immagine con dimensioni normali](databinding-the-slider-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](using-the-slider-control-with-auto-postback-cs.md)
> [Successivo](using-the-slider-control-with-auto-postback-vb.md)
