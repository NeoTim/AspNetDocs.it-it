---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Modifica delle proprietà di DropShadow dal codice Client (VB) | Microsoft Docs
author: wenz
description: Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura. Proprietà di questo tipo di estensione possono essere modificate anche tramite client JavaScript...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: c9b0946568063b9e5cf1454bd7a57c43304c3543
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59390311"
---
# <a name="manipulating-dropshadow-properties-from-client-code-vb"></a>Modifica delle proprietà di DropShadow dal codice client (VB)

da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)

> Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura. Proprietà di questo tipo di estensione possono essere modificate anche con il codice JavaScript client.


## <a name="overview"></a>Panoramica

Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura. Proprietà di questo tipo di estensione possono essere modificate anche con il codice JavaScript client.

## <a name="steps"></a>Passaggi

Il codice inizia con un pannello che contiene alcune righe di testo:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

La classe CSS associata offre il pannello di un colore di sfondo interessante:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

Il `DropShadowExtender` viene aggiunto per estendere il pannello con un effetto di ombreggiatura, impostato su 50% di opacità:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

Quindi, ASP.NET AJAX `ScriptManager` controllo Abilita il Toolkit di controllo lavorare:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

Un altro pannello contiene due collegamenti di JavaScript per l'impostazione di opacità dell'ombreggiatura: il collegamento meno ridurre l'opacità dell'ombreggiatura, aumenta il collegamento di più.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

La funzione JavaScript `changeOpacity()` quindi necessario trovare prima il `DropShadowExtender` controllo nella pagina. ASP.NET AJAX definisce il `$find()` metodo per esattamente tale attività. Successivamente, il `get_Opacity()` che consente di recuperare l'opacità corrente, il `set_Opacity()` lo imposta metodo. Il codice JavaScript inserisce quindi il valore di opacità nel `<label>` elemento:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


[![Topacità he viene modificato sul lato client](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)

L'opacità viene modificato sul lato client ([fare clic per visualizzare l'immagine con dimensioni normali](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](adjusting-the-z-index-of-a-dropshadow-vb.md)
