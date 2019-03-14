---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Creazione di controllo su/giù numerico con un back-end del servizio Web (VB) | Microsoft Docs
author: wenz
description: Anziché consentire a un utente di digitare un valore in una casella di controllo, un controllo (che è presente in Windows e altri sistemi operativi) su/giù numerico potrebbe rivelarsi man mano c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: bf69b3e6932df528e8ccd2348ffa6f13132f99f8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029488"
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a>Creazione di un controllo su/giù numerico con un back-end del servizio Web (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)

> Anziché consentire a un utente di digitare un valore in una casella di controllo, un valore numerico su/giù controllo (che è presente in Windows e altri sistemi operativi) potrebbe rivelarsi man mano familiarità. Per impostazione predefinita, il controllo NumericUpDown sempre aumenta o diminuisce di un valore di 1, ma un servizio web dimostra una maggiore flessibilità.


## <a name="overview"></a>Panoramica

Anziché consentire a un utente di digitare un valore in una casella di controllo, un valore numerico su/giù controllo (che è presente in Windows e altri sistemi operativi) potrebbe rivelarsi man mano familiarità. Per impostazione predefinita, il `NumericUpDown` controllo sempre aumenta o diminuisce di un valore di 1, ma un servizio web dimostra una maggiore flessibilità.

## <a name="steps"></a>Passaggi

ASP.NET AJAX Control Toolkit contiene il `NumericUpDown` extender che aggiunge automaticamente due pulsanti a una casella di testo: Uno per aumentare il valore, uno per ridurlo. Tuttavia il controllo supporta anche una chiamata al servizio web (o chiamata al metodo pagina). Ogni volta che il viene selezionato verso l'alto o verso il basso sul pulsante, il codice JavaScript codice si connette al server web ed esegue un metodo non esiste. La firma del metodo è quello riportato di seguito:

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

Il `current` argomento è il valore corrente nella casella di testo; la `tag` attributo sia i dati di contesto aggiuntive che è possibile impostare come proprietà del `NumericUpDown` extender (ma non obbligatorio).

Per questo esempio, il controllo su/giù numerico consente solo valori che sono potenze di due: 1, 2, 4, 8, 16, 32, 64 e così via. Di conseguenza, il metodo eseguito quando l'utente vuole aumentare il valore necessario raddoppiare il valore precedente. l'altro metodo deve dividere valore per due. Quindi, ecco il servizio web completo:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

Infine, creare una nuova pagina ASP.NET. Come di consueto, è necessario un `ScriptManager` (controllo), una `TextBox` controllo e un `NumericUpDownExtender` controllo. Nel secondo caso, è necessario fornire le informazioni sul servizio web:

- `ServiceDownMethod` nome di scorrimento metodo web o pagina (metodo)
- `ServiceDownPath` percorso del servizio web con il metodo di servizio verso il basso. omettere se si usa un metodo di pagina
- `ServiceUpMethod` nome della freccia metodo web o metodo di pagina
- `ServiceUpPath` percorso del servizio web con il metodo del servizio backup; omettere se si usa un metodo di pagina

Ecco il markup completo per la pagina:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

Se si esegue la pagina, si noti come il valore nella casella di testo raddoppia sempre quando si fa clic sul pulsante superiore e viene dimezzato quando si fa clic sul pulsante inferiore.


[![Vengono visualizzati solo i numeri che sono una potenza di 2](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)

Vengono visualizzati solo i numeri che sono una potenza di 2 ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
