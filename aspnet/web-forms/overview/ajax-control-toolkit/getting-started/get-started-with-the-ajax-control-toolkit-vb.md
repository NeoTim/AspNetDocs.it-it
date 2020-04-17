---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: Introduzione a AJAX Control Toolkit (VB) Documenti Microsoft
author: rick-anderson
description: Scopri tutto quello che devi sapere per iniziare a usare AJAX Control Toolkit.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: 6af5e293a35ce3e3ba35a0f7abfd2d92d538c84c
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543561"
---
# <a name="get-started-with-the-ajax-control-toolkit-vb"></a>Introduzione ad AJAX Control Toolkit (VB)

da parte [di Microsoft](https://github.com/microsoft)

> Scopri tutto quello che devi sapere per iniziare a usare AJAX Control Toolkit.

AJAX Control Toolkit contiene più di 30 controlli liberi che è possibile utilizzare nelle applicazioni ASP.NET. In questa esercitazione viene illustrato come scaricare AJAX Control Toolkit e aggiungere i controlli del toolkit alla casella degli strumenti di Visual Studio/Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Download di AJAX Control Toolkit

[L'AJAX Control Toolkit](http://devexpress.com/act) è un progetto open source sviluppato dai membri della comunità ASP.NET e dal team di ASP.NET.

[![Download di AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)

**Figura 01**: Download di AJAX Control Toolkit ([Fare clic per visualizzare l'immagine a dimensioni intere](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))

Dopo aver scaricato il file, è necessario sbloccarlo. Fare clic con il pulsante destro del mouse sul file, selezionare Proprietà e fare clic sul pulsante **Sblocca** (vedere la figura 2).

[![Sblocco del file zip di AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)

**Figura 02**: Sblocco del file zip ajax Control Toolkit[(Fare clic per visualizzare l'immagine a dimensioni intere)](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png)

Dopo aver sbloccato il file, è possibile decomprimere il file: fare clic con il pulsante destro del mouse sul file e selezionare l'opzione di menu **Estrai tutto.** A questo punto, siamo pronti per aggiungere il toolkit alla casella degli strumenti di Visual Studio/Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Aggiunta di AJAX Control Toolkit alla casella degli strumenti

Il modo più semplice per utilizzare AJAX Control Toolkit consiste nell'aggiungere il toolkit alla casella degli strumenti di Visual Studio/Visual Web Developer (vedere Figura 3). In questo modo, è sufficiente trascinare un controllo toolkit in una pagina quando si desidera utilizzarlo.

[![AJAX Control Toolkit viene visualizzato nella casella degli strumenti](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)

**Figura 03:** AJAX Control Toolkit viene visualizzato nella casella degli[strumenti(Fare clic per visualizzare l'immagine](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png)a dimensioni intere)

In primo luogo, è necessario aggiungere una scheda AJAX Control Toolkit alla casella degli strumenti. Seguire questa procedura.

1. Creare un nuovo ASP.NET sito Web selezionando l'opzione di menu File, Nuovo sito Web. Fare doppio clic su Default.aspx nella finestra Esplora soluzioni per aprire il file nell'editor.
2. Fare clic con il pulsante destro del mouse sulla Casella degli strumenti sotto la scheda Generale e selezionare l'opzione di menu **Aggiungi scheda** (vedere la figura 4).
3. Immettere una nuova scheda denominata AJAX Control Toolkit.

[![Aggiunta di una nuova scheda](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)

**Figura 04**: Aggiunta di una nuova scheda([Fare clic per visualizzare l'immagine a dimensioni intere)](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png)

Successivamente, è necessario aggiungere i controlli AJAX Control Toolkit alla nuova scheda.

- Fare clic con il pulsante destro del mouse sotto la scheda AJAX Control Toolkit e selezionare l'opzione di menu **Choose Items (vedere la Figura 5)**.
- Passare al percorso in cui è stato decompresso L'AJAX Control Toolkit e selezionare l'assembly AjaxControlToolkit.dll.

[![Scegliere gli elementi da aggiungere alla casella degli strumenti](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)

**Figura 05**: Scegliere gli elementi da aggiungere alla casella degli strumenti([Fare clic per visualizzare l'immagine](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png)a dimensioni intere)

Dopo aver completato questi passaggi, tutti i controlli del toolkit verranno visualizzati nella casella degli strumenti.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Aggiornamento a una nuova versione del toolkit

Se si utilizza una versione precedente del Toolkit e ora è necessario passare a una versione successiva, ecco i passaggi consigliati:

- Binaries - Elimina la versione precedente dell'assembly AjaxControlToolkit.dll dalla cartella Bin del sito Web.
- Elementi della casella degli strumenti: eliminare la scheda AJAX Control Toolkit e seguire i passaggi precedenti per ricreare la scheda con la nuova versione dell'assembly AjaxControlToolkit.dll.

> [!div class="step-by-step"]
> [Successivo](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
> [precedente](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
