---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
title: Uso di HoverMenu con un controllo Repeater (c#) | Microsoft Docs
author: wenz
description: 'Il controllo di HoverMenu in AJAX Control Toolkit fornisce un effetto popup semplice: Quando il puntatore del mouse viene posizionato su un elemento, viene visualizzata un finestra popup a dalla specifica...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e7700e7b-edc3-4183-a713-70e507cc7490
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7f64e90eb2f8f87e2f382cb7897793e7071d305d
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59384500"
---
# <a name="using-hovermenu-with-a-repeater-control-c"></a>Uso di HoverMenu con un controllo Repeater (C#)

da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)

> Il controllo di HoverMenu in AJAX Control Toolkit fornisce un effetto popup semplice: Quando il puntatore del mouse viene posizionato su un elemento, viene visualizzata un finestra popup in una posizione specificata. È anche possibile usare questo controllo all'interno di un controllo repeater.


## <a name="overview"></a>Panoramica

Il `HoverMenu` controllo in AJAX Control Toolkit fornisce un effetto popup semplice: Quando il puntatore del mouse viene posizionato su un elemento, viene visualizzata un finestra popup in una posizione specificata. È anche possibile usare questo controllo all'interno di un controllo repeater.

## <a name="steps"></a>Passaggi

Prima di tutto, è richiesta un'origine dati. Questo esempio Usa il database AdventureWorks e Microsoft SQL Server 2005 Express Edition. Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa l'edizione express) ed è anche disponibile come download separato in [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Il database AdventureWorks è parte di SQL Server 2005 Samples and Sample Databases (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Il modo più semplice per configurare il database è di utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database.

In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è anche l'impostazione predefinita. Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.

Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample1.aspx)]

Quindi, aggiungere un'origine dati alla pagina. Per usare una quantità limitata di dati, selezionare solo le prime cinque voci della tabella Vendor del database AdventureWorks. Se si usa l'Assistente per Visual Studio per creare l'origine dati, è importante che un bug nella versione corrente non prefisso il nome della tabella (`Vendor`) con `Purchasing`. Il markup seguente mostra la sintassi corretta:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample2.aspx)]

Successivamente, aggiungere un pannello che funge da finestra popup modale:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample3.aspx)]

A questo punto, il `HoverMenuExtender` entra in gioco. In modo che ogni elemento nell'origine dati Ottiene il proprio controllo popup, il programma di estensione deve essere inserita all'interno di repeater `<ItemTemplate>` sezione. Questo è il markup:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample4.aspx)]

A questo punto ogni elemento nell'origine dati consente di visualizzare una finestra popup a destra (`PopupPosition` attributo) dopo un ritardo di 50 millisecondi (`PopDelay` attributo).


[![Viene visualizzato il menu di passaggio del mouse accanto a ogni elemento nel repeater](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)

Viene visualizzato il menu di passaggio del mouse accanto a ogni elemento nel repeater ([fare clic per visualizzare l'immagine con dimensioni normali](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](using-hovermenu-with-a-repeater-control-vb.md)
