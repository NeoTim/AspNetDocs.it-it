---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Popolamento dinamico di un controllo tramite codice JavaScript (c#) | Microsoft Docs
author: wenz
description: Il controllo di DynamicPopulate di ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e inserisce il valore risultante in un controllo di destinazione in t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: a6b433f187495b8dcd874bcab8ddc607e6de61c9
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59422525"
---
# <a name="dynamically-populating-a-control-using-javascript-code-c"></a>Popolamento dinamico di un controllo tramite codice JavaScript (C#)

da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> Il controllo di DynamicPopulate di ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e inserisce il valore risultante in un controllo di destinazione nella pagina senza un aggiornamento della pagina. È anche possibile attivare il popolamento usando codice JavaScript lato client personalizzato.


## <a name="overview"></a>Panoramica

Il `DynamicPopulate` chiama un servizio web (o un metodo di pagina) e inserisce il valore risultante in un controllo di destinazione nella pagina senza un aggiornamento della pagina di controllo in ASP.NET AJAX Control Toolkit. È anche possibile attivare il popolamento usando codice JavaScript lato client personalizzato.

## <a name="steps"></a>Passaggi

Prima di tutto, è necessario un servizio Web ASP.NET che implementa il metodo che verrà chiamata dal `DynamicPopulateExtender` controllo. Il servizio web implementa il metodo `getDate()` che prevede un argomento di tipo string, denominato `contextKey`, poiché il `DynamicPopulate` controllo Invia un'informazione rapida con ogni chiamata al servizio web. Ecco il codice (file `DynamicPopulate.cs.asmx`) che recupera la data corrente in uno dei tre formati:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

Nel passaggio successivo, creare un nuovo sito ASP.NET e iniziare con il controllo ScriptManager di AJAX ASP.NET:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

Quindi, aggiungere un controllo etichetta (ad esempio usando il controllo HTML lo stesso nome, o `<asp:Label />` controllo web) che in un secondo momento verrà visualizzato il risultato della chiamata al servizio web.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

Successivamente, includere un `DynamicPopulateExtender` controllano e forniscono informazioni sui servizi web, controllo di destinazione, ma non il nome del controllo che attiva la popolazione questa verrà eseguita in un secondo momento usando JavaScript personalizzato.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

A questo punto alla parte JavaScript. Il `$find()` funzione, definito dalla libreria ASP.NET AJAX, restituisce un riferimento a oggetti lato server di ASP.NET AJAX Control Toolkit, ad esempio `DynamicPopulateExtender`. Nel file corrente `$find("dpe")` restituisce un riferimento a quella `DynamicPopulateExtender` controllo nella pagina. Espone un metodo denominato `populate()` che attiva il processo di compilazione dinamica. Il `populate()` metodo richiede un argomento: la chiave di contesto che verrà utilizzata come argomento per il `getDate()` metodo web. Quindi, ad esempio, `$find("dpe").populate("format1")` potrebbe popolare l'etichetta con la data corrente nel formato mese-giorno-anno.

Per rendere un po' più flessibile il codice di esempio, l'utente può ora scegliere tra diversi formati di Data. Per ognuno di essi, viene visualizzato un pulsante di opzione. Una volta l'utente fa clic su un pulsante di opzione, il codice JavaScript popola in modo dinamico l'etichetta con il formato della data selezionata. Ecco i pulsanti di opzione:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

Si noti che nel contesto di un pulsante di opzione, l'espressione JavaScript `this.value` fa riferimento al valore del pulsante corrente, che è esattamente le stesse informazioni il `getDate()` metodo utilizzabili.


[![A Fare clic sul pulsante Recupera la data dal server, nel formato specificato](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

Un clic sul pulsante Recupera la data dal server, nel formato specificato ([fare clic per visualizzare l'immagine con dimensioni normali](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](dynamically-populating-a-control-cs.md)
> [Successivo](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
