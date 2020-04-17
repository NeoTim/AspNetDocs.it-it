---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Creazione di percorsi personalizzati (C Documenti Microsoft
author: rick-anderson
description: Informazioni su come aggiungere route personalizzate a un'applicazione MVC ASP.NET. In questa esercitazione viene illustrato come modificare la tabella di route predefinita nel file Global.asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: b66ccc5e0cd4f6d7e5884394c2b7555b76382d3d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542755"
---
# <a name="creating-custom-routes-c"></a>Creazione di route personalizzate (C#)

da parte [di Microsoft](https://github.com/microsoft)

> Informazioni su come aggiungere route personalizzate a un'applicazione MVC ASP.NET. In questa esercitazione viene illustrato come modificare la tabella di route predefinita nel file Global.asax.

In questa esercitazione viene illustrato come aggiungere una route personalizzata a un'applicazione MVC ASP.NET. Viene illustrato come modificare la tabella di route predefinita nel file Global.asax con una route personalizzata.

Per molte applicazioni MVC ASP.NET semplici, la tabella di route predefinita funzionerà correttamente. Tuttavia, è possibile che si disponga di esigenze di routing specifiche. In tal caso, è possibile creare una route personalizzata.

Si supponga, ad esempio, che si sta creando un'applicazione blog. È possibile gestire le richieste in arrivo simili alle seguente:You might want to handle incoming requests that look like this:

/Archivio/12-25-2009

Quando un utente immette questa richiesta, si desidera restituire il post di blog che corrisponde alla data 25/12/2009. Per gestire questo tipo di richiesta, è necessario creare una route personalizzata.

Il file Global.asax nel listato 1 contiene una nuova route personalizzata, denominata Blog, che gestisce le richieste simili a /Archive/*data di immissione*.

**Listato 1 - Global.asax (con percorso personalizzato)**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

L'ordine delle route aggiunte alla tabella di route è importante. Il nostro nuovo percorso Blog personalizzato viene aggiunto prima del percorso predefinito esistente. Se hai invertito l'ordine, verrà sempre chiamata la route predefinita anziché la route personalizzata.

La route del blog personalizzato corrisponde a qualsiasi richiesta che inizia con /Archive/. Quindi, corrisponde a tutti i seguenti URL:

- /Archivio/12-25-2009

- /Archivio/10-6-2004

- /Archivio/mela

La route personalizzata esegue il mapping della richiesta in ingresso a un controller denominato Archive e richiama l'azione Entry(). Quando viene chiamato il metodo Entry(), la data di immissione viene passata come parametro denominato entryDate.

È possibile utilizzare il percorso personalizzato Blog con il controller nel listato 2.You can use the Blog custom route with the controller in Listing 2.

**Listato 2 - ArchiveController.cs**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

Si noti che il metodo Entry() nel listato 2 accetta un parametro di tipo DateTime. Il framework MVC è abbastanza intelligente per convertire automaticamente la data di immissione dall'URL in un valore DateTime. Se il parametro di data di ingresso dall'URL non può essere convertito in un DateTime, viene generato un errore (vedere Figura 1).

**Figura 1 - Errore durante la conversione del parametro**

[![Finestra di dialogo relativa al nuovo progetto](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)

**Figura 01:** Errore durante la conversione del parametro ([Fare clic per visualizzare l'immagine](creating-custom-routes-cs/_static/image2.png)a dimensioni intere)

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione era dimostrare come è possibile creare un percorso personalizzato. Si è appreso come aggiungere una route personalizzata alla tabella di route nel file Global.asax che rappresenta le voci di blog. È stato illustrato come eseguire il mapping delle richieste per le voci di blog a un controller denominato ArchiveController e un'azione controller denominata Entry().

> [!div class="step-by-step"]
> [Successivo](aspnet-mvc-controllers-overview-cs.md)
> [precedente](creating-a-route-constraint-cs.md)
