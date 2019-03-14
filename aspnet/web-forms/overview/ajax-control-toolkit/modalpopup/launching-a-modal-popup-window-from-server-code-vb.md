---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Avvio di una finestra Popup modale dal codice Server (VB) | Microsoft Docs
author: wenz
description: Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client. Tuttavia alcuni scenari richiedono che t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 79c7281032d8b36d0e4f4b62dcbbad2ab5ebbb77
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57063158"
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a>Avvio di una finestra popup modale dal codice server (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)

> Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client. Alcuni scenari richiedono tuttavia che l'apertura della finestra popup modale è attivato sul lato server.


## <a name="overview"></a>Panoramica

Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client. Alcuni scenari richiedono tuttavia che l'apertura della finestra popup modale è attivato sul lato server.

## <a name="steps"></a>Passaggi

Prima di tutto, per illustrare come funziona il controllo ModalPopup è necessario un controllo pulsante di ASP.NET di web. Aggiungere il pulsante di annullamento all'interno di &lt;form&gt; elemento in una nuova pagina:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

Quindi, è necessario il markup per la finestra popup che si desidera creare. Come definire un `<asp:Panel>` controllano e assicurarsi che include un controllo pulsante. Il controllo ModalPopup offre le funzionalità per rendere tali un pulsante per chiudere la finestra popup; in caso contrario, non vi è alcun approccio facile per lasciare che scompaiono.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

Successivamente, aggiungere il controllo ModalPopup di ASP.NET AJAX Toolkit alla pagina. Impostare le proprietà per il pulsante che consente di caricare il controllo pulsante che lo rende non vengono più visualizzati e l'ID della finestra popup effettivo.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

Come con tutte le pagine web basate su ASP.NET AJAX; la gestione di Script deve caricare le librerie JavaScript necessarie per i browser di destinazione diverso:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

Eseguire l'esempio nel browser. Quando si fa clic sul pulsante, viene visualizzata la finestra popup modale. Per ottenere lo stesso effetto tramite codice lato server, è necessario un nuovo pulsante:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

Come può notare, un clic sul pulsante genera un postback ed esegue il `ServerButton_Click()` metodo nel server. In questo metodo, una funzione JavaScript denominata `launchModal()` viene eseguita per essere precisi, la funzione JavaScript verrà eseguita dopo la pagina è stata caricata:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

Il compito di `launchModal()` consiste nel visualizzare il controllo ModalPopup. Il `launchModal()` funzione viene eseguita una volta caricata la pagina HTML completata. In quel momento, tuttavia, il framework ASP.NET AJAX non è stato caricato completamente ancora. Pertanto, il `launchModal()` funzione limita a impostare una variabile che il controllo ModalPopup deve essere visualizzato in un secondo momento:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

Il `pageLoad()` funzione JavaScript è una funzione speciale che viene eseguita una volta che ASP.NET AJAX è stato caricato completamente. Di conseguenza è aggiungere codice a questa funzione per visualizzare il controllo ModalPopup, ma solo se `launchModal()` è stato chiamato prima di:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

Il `$find()` funzione esegue la ricerca di un elemento denominato nella pagina e previsto l'ID lato server come un parametro. Pertanto `$find("mpe")` restituisce la rappresentazione di client del controllo ModalPopup; relativo `show()` metodo consente la finestra popup visualizzata.


[![Viene visualizzata la finestra popup modale quando viene fatto clic su uno dei pulsanti](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)

Viene visualizzata la finestra popup modale quando viene fatto clic su uno dei pulsanti ([fare clic per visualizzare l'immagine con dimensioni normali](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](positioning-a-modalpopup-cs.md)
> [Successivo](using-modalpopup-with-a-repeater-control-vb.md)
