---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Disabilitazione delle azioni durante l'animazione (c#) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Supporta anche azioni...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 4315f06ea1599bacb93a23a3759610e19754cfba
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57044348"
---
<a name="disabling-actions-during-animation-c"></a>Disabilitazione delle azioni durante l'animazione (C#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Supporta anche azioni, ad esempio un clic del mouse. Tuttavia quando un clic del mouse viene avviata un'animazione, è consigliabile disabilitare i clic del mouse durante l'animazione.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Supporta anche azioni, ad esempio un clic del mouse. Tuttavia quando un clic del mouse viene avviata un'animazione, è consigliabile disabilitare i clic del mouse durante l'animazione.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

Verrà applicata l'animazione a un pulsante HTML simile al seguente:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

Si noti che un controllo HTML viene utilizzato invece di un controllo Web poiché non vogliamo pulsante per creare un postback; deve avviare l'animazione lato client per noi.

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

All'interno di `<Animations>` nodo, `<OnClick>` è l'elemento a destra per gestire i clic del mouse. Tuttavia, potrebbe essere fatto clic sul pulsante durante l'animazione, nonché. Il `<EnableAction>` può gestire di tale elemento. Impostazione `Enabled="false"` disabilita il pulsante come parte dell'animazione. Poiché si sta usando diverse singole animazioni (disabilita il pulsante e le animazioni effettive), il `<Parallel>` elemento è obbligatorio per associare le animazioni insieme in una singole. Ecco il markup completo per `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

È anche possibile abilitare nuovamente al pulsante dopo l'animazione, utilizzando l'elemento XML seguente alla fine dell'elenco:

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

Tuttavia in questo scenario specifico questo sarebbe inutile perché il pulsante dissolve e non è visibile alla fine dell'animazione.


[![Il pulsante è disabilitato, non appena viene eseguita l'animazione](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)

Il pulsante è disabilitato, non appena viene eseguita l'animazione ([fare clic per visualizzare l'immagine con dimensioni normali](disabling-actions-during-animation-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](animating-in-response-to-user-interaction-cs.md)
> [Successivo](triggering-an-animation-in-another-control-cs.md)
