---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: Utilizzo dell'estensione vb (ColorPicker Control Extender) Documenti Microsoft
author: rick-anderson
description: ColorPicker è un'estensione AJAX ASP.NET che fornisce funzionalità di selezione del colore lato client con interfaccia utente in un controllo popup. Può essere collegato a qualsiasi ASP.NET...
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: c373102665ac17020ca8d5f14d3488a874bb68de
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542846"
---
# <a name="using-the-colorpicker-control-extender-vb"></a>Utilizzo dell'estensione del controllo ColorPicker (VB)Using the ColorPicker Control Extender (VB)

da parte [di Microsoft](https://github.com/microsoft)

> ColorPicker è un'estensione AJAX ASP.NET che fornisce funzionalità di selezione del colore lato client con interfaccia utente in un controllo popup. Può essere associato a qualsiasi ASP.NET controllo TextBox. esso.

L'obiettivo di questa esercitazione è spiegare come è possibile usare l'estensione del controllo ColorPicker di AJAX Control Toolkit. L'extender di controllo ColorPicker visualizza una finestra di dialogo popup che consente di selezionare un colore. Il ColorPicker è utile ogni volta che si desidera fornire un'interfaccia utente intuitiva per un utente di scegliere un colore.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Estensione di un controllo TextBox con il controllo ColorPicker ExtenderExtending a TextBox Control with the ColorPicker Control Extender

Si supponga, ad esempio, che si desidera creare un sito Web che consenta ai visitatori di creare biglietti da visita personalizzati. I visitatori possono inserire il testo di un biglietto da visita e scegliere il colore. La pagina ASP.NET nel listato 1 contiene due controlli TextBox denominati txtCardText e txtCardColor. Quando si invia il modulo, vengono visualizzati i valori selezionati (vedere la figura 1).

[![Modulo semplice per la creazione di un biglietto da visita](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

**Figura 01**: Modulo semplice per la creazione di un biglietto da visita ([Fare clic per visualizzare l'immagine](using-the-colorpicker-control-extender-vb/_static/image2.png)a dimensioni intere)

**Listato 1 - CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

Il modulo nel listato 1 funziona, ma non fornisce un'esperienza utente eccezionale. L'utente deve digitare un colore nella casella di testo. Se l'utente desidera un colore specializzato - per esempio, solo la giusta tonalità di verde pisone - allora l'utente deve capire il codice di colore HTML senza alcun aiuto.

È possibile utilizzare l'extender del controllo ColorPicker per creare un'esperienza utente migliore. Il ColorPicker visualizza una finestra di dialogo di colore quando si sposta lo stato attivo su un TextBox controllo (vedere Figura 2).

[![Estensione del controllo ColorPickerThe ColorPicker Control Extender](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

**Figura 02**: Estensione del controllo ColorPicker ([Fare clic per visualizzare l'immagine a dimensioni intere](using-the-colorpicker-control-extender-vb/_static/image4.png))

È necessario completare due passaggi per utilizzare l'estensione del controllo ColorPicker con il form nel listato 1:

1. Aggiungere un controllo ScriptManager alla pagina
2. Aggiungere l'estensione del controllo ColorPicker alla pagina

Prima di poter utilizzare ColorPicker, è necessario aggiungere un ScriptManager alla pagina. Un buon posto per aggiungere scriptManager è proprio &lt;&gt; sotto il tag di apertura del modulo lato server. È possibile trascinare ScriptManager nella pagina dalla casella degli strumenti (lo ScriptManager si trova nella scheda Estensioni AJAX). In alternativa, è possibile digitare il seguente tag nella vista Origine sotto il tag del modulo sul lato server di apertura:

&lt;asp:ScriptManager ID : "ScriptManager1" runat "server" /&gt;

Il modo più semplice per aggiungere l'estensione del controllo ColorPicker alla pagina è in visualizzazione Progettazione. Se si passa il mouse sopra il txtCardColor casella di testo, viene visualizzata un'opzione di attività intelligente consente di aggiungere un'estensione (vedere Figura 3). Se si seleziona questa opzione, viene visualizzata la procedura guidata Extender (vedere la Figura 4).

[![Aggiunta di un'estensione](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

**Figura 03**: Aggiunta di un'estensione ([Fare clic per visualizzare l'immagine a dimensioni intere](using-the-colorpicker-control-extender-vb/_static/image6.png))

[![Selezione di un'estensione di controllo con la procedura guidata Extender](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

**Figura 04**: Selezione di un'estensione di controllo con la procedura guidata Extender ([Fare clic per visualizzare l'immagine a dimensioni intere](using-the-colorpicker-control-extender-vb/_static/image8.png))

È possibile selezionare l'estensione ColorPicker per estendere il controllo txtCardColor TextBox con l'extender ColorPicker. Fare clic su OK per chiudere la finestra di dialogo.

Dopo aver apportato queste modifiche, l'origine della pagina sarà simile al listato 2.After you make these changes, the source for the page looks like Listing 2.

**Listato 2 - CreateCard.aspx (con ColorPicker)**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

Si noti che la pagina contiene ora un ColorPickerExtender controllo che viene visualizzato direttamente sotto il txtCardColor TextBox controllo. Il ColorPickerExtender controllo estende il txtCardColor controllo in modo che venga visualizzata una finestra di dialogo di selezione colori.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Utilizzo di un pulsante per avviare la finestra di dialogo Selezione colori

L'estensione ColorPicker supporta le seguenti proprietà:

- PopupButtonId - L'ID di un pulsante nella pagina che fa sì che la finestra di dialogo di selezione colori da visualizzare.
- PopupPosition - La posizione, relativa al controllo di destinazione, della finestra di dialogo di selezione colori. I valori possibili sono Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right e Left (il valore predefinito è BottomLeft).
- SampleControlId: ID di un controllo che visualizza il colore selezionato.
- SelectedColor - Il colore iniziale selezionato da ColorPicker.

È possibile utilizzare queste proprietà per personalizzare la modalità di visualizzazione della finestra di dialogo di selezione colori e la modalità di visualizzazione del colore selezionato. La pagina nel listato 3 illustra come è possibile utilizzare diverse di queste proprietà.

**Listato 3 - CreateCardButton.aspxListing 3 - CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

La pagina nel listato 3 include un pulsante Seleziona colore (vedere Figura 5). Quando si fa clic su questo pulsante, la finestra di dialogo di selezione colori viene visualizzata sopra il TextBox.When you click this button, the color picker dialog appears above the TextBox. Se si seleziona un colore dalla finestra di dialogo, il colore selezionato viene visualizzato come colore di sfondo del controllo Etichetta lblSample.

Il ColorPicker PopupButtonID proprietà viene utilizzata per associare il Pick Color pulsante per il ColorPicker extender. Quando si specifica un valore per il PopupButtonID proprietà, la finestra di dialogo di selezione colori non viene più visualizzata quando il controllo di destinazione ha lo stato attivo. È necessario fare clic sul pulsante per visualizzare la finestra di dialogo.

The SampleControlID property is used to associate a control that displays the selected color with the ColorPicker. Il Controllo Colore consente di modificare il colore di sfondo di questo controllo in base al colore attualmente selezionato.

[![Visualizzazione della finestra di dialogo di selezione colori con un pulsante](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

**Figura 05**: Visualizzazione della finestra di dialogo di selezione colori con un pulsante ([Fare clic per visualizzare l'immagine a dimensioni intere)](using-the-colorpicker-control-extender-vb/_static/image10.png)

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come utilizzare l'estensione del controllo ColorPicker per visualizzare una finestra di dialogo di selezione colori popup. In primo luogo, è stato esaminato come è possibile visualizzare la finestra di dialogo quando lo stato attivo viene spostato su un TextBox controllo. Successivamente, si è appreso come creare un pulsante che visualizza la finestra di dialogo di selezione colori quando si fa clic sul pulsante.

> [!div class="step-by-step"]
> [Indietro](using-the-colorpicker-control-extender-cs.md)
