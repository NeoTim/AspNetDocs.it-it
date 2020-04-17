---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: Utilizzo dei controlli AJAX Control Toolkit e delle estensioni di controllo (VB) Documenti Microsoft
author: rick-anderson
description: Informazioni su come aggiungere controlli ED estensioni AJAX Control Toolkit alle pagine ASP.NET.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: 129d6841bc4db62ecbe5f26c4830d1ce2f199bff
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540012"
---
# <a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a>Uso di controlli e controlli Extender di AJAX Control Toolkit (VB)

da parte [di Microsoft](https://github.com/microsoft)

> Informazioni su come aggiungere controlli ED estensioni AJAX Control Toolkit alle pagine ASP.NET.

AJAX Control Toolkit contiene un set di controlli ed estensioni di controllo. In questa breve esercitazione viene illustrato come aggiungere controlli ed estensioni di controllo a una pagina ASP.NET.

> [!NOTE] 
> 
> Per istruzioni sull'installazione di AJAX Control Toolkit e sull'aggiunta di AJAX Control Toolkit alla casella degli strumenti di Visual Studio/Visual Web Developer, vedere l'esercitazione [Introduzione a AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).

## <a name="using-ajax-control-toolkit-controls"></a>Utilizzo dei controlli AJAX Control Toolkit

Un controllo AJAX Control Toolkit funziona come un normale controllo ASP.NET. È possibile trascinare il controllo dalla casella degli strumenti in una pagina ASP.NET. È possibile aggiungere il controllo alla pagina in visualizzazione Progettazione o Origine.

Esiste un requisito speciale quando si utilizzano i controlli di AJAX Control Toolkit. La pagina deve contenere un controllo ScriptManager.The page must contain a ScriptManager control. Il controllo ScriptManager è responsabile dell'inclusione di tutto il codice JavaScript necessario richiesto dai controlli AJAX Control Toolkit.

Ad esempio, la scheda AJAX Control Toolkit include un controllo denominato Editor controllo. Questo controllo visualizza un editor HTML avanzato. Per aggiungere il controllo Editor a una pagina, attenersi alla seguente procedura:

1. Creare una nuova pagina ASP.NET denominata ShowEditor.aspxCreate a new ASP.NET page named ShowEditor.aspx
2. Selezionare il controllo ScriptManager sotto la scheda Estensioni AJAX nella casella degli strumenti e trascinarlo nella pagina.
3. Selezionare il controllo Editor dal sotto la scheda AJAX Control Toolkit nella casella degli strumenti e trascinare il controllo nella pagina (vedere Figura 1). La finestra di progettazione dovrebbe essere simile Figura 2.The Designer should look like Figure 2.
4. Eseguire il sito Web selezionando l'opzione di menu **Debug, Avvia debug** o premendo il tasto F5.
5. Verrà visualizzata la pagina in Figura 3.

[![Selezione del controllo Editor HTML](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)

**Figura 01**: Selezione del controllo Editor HTML ([Fare clic per visualizzare l'immagine](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png)a dimensioni intere)

[![Finestra di progettazione di Visual Studio con ScriptManager e controllo Modifica](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)

**Figura 02:** Finestra di progettazione di Visual Studio con ScriptManager e controllo Modifica[(Fare clic per visualizzare l'immagine a dimensioni intere)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png)

[![Pagina DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)

**Figura 03**: La pagina DisplayEditor.aspx[(Fare clic per visualizzare l'immagine](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png)a dimensioni intere)

## <a name="using-ajax-control-toolkit-control-extenders"></a>Utilizzo di estensioni di controllo AJAX Control ToolkitUsing AJAX Control Toolkit Control Extenders

AJAX Control Toolkit contiene anche estensioni di controllo. Come suggerisce il nome, un'estensione di controllo estende la funzionalità di un controllo esistente. Ad esempio, l'estensione del controllo ConfirmButton estende il controllo Button standard ASP.NET. L'extender modifica il Button controllo s comportamento in modo che il Button visualizza una finestra di dialogo di conferma quando si fa clic su di esso.

Un'estensione di controllo, proprio come un controllo AJAX Control Toolkit, richiede un controllo ScriptManager.A control extender, just like an AJAX Control Toolkit control control, requires a ScriptManager control. È necessario aggiungere un controllo ScriptManager a una pagina prima di iniziare a utilizzare gli extender di controllo nella pagina.

Attenersi alla seguente procedura per utilizzare l'estensione di controllo ConfirmButton:Follow these steps to use the ConfirmButton control extender:

1. Creare una nuova pagina ASP.NET denominata ShowConfirmButton.aspxCreate a new ASP.NET page named ShowConfirmButton.aspx
2. Aggiungere un controllo ScriptManager alla pagina trascinandolo nella pagina da sotto la scheda Estensioni AJAX.
3. Aggiungere un controllo Button standard alla pagina trascinando il pulsante dal campo della scheda Standard nella casella degli strumenti nell'area di progettazione.
4. Fare clic sull'opzione Aggiungi attività **Extender** (vedere la Figura 4).
5. Nella finestra di dialogo Scegli extender, selezionare ConfirmButtonExtender (vedere La figura 5) e fare clic su OK pulsante.
6. Selezionare il controllo Button nella finestra di\_progettazione ed espandere il nodo Extender, Button1 ConfirmButtonExtender nella finestra Proprietà (vedere la Figura 6). Assegnare il valore *Really?* alla proprietà ConfirmText.
7. Eseguire la pagina selezionando l'opzione di menu **Debug, Avvia debug** o premere il tasto F5.

[![Opzione Aggiungi attività Extender](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)

**Figura 04**: L'opzione Aggiungi attività Extender ([Fare clic per visualizzare l'immagine a dimensioni intere)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png)

[![Selezione dell'estensione del controllo ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)

**Figura 05**: Selezione dell'extender del controllo ConfirmButton ([Fare clic per visualizzare l'immagine](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png)a dimensioni intere)

[![Impostazione di una proprietà ConfirmButtonSetting a ConfirmButton property](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)

**Figura 06**: Impostazione di una proprietà ConfirmButton[(Fare clic per visualizzare l'immagine a dimensioni intere)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png)

Quando la pagina si apre, dovrebbe essere visualizzato un pulsante. Quando si fa clic sul pulsante, viene visualizzata la finestra di dialogo di conferma in Figura 7.

[![Visualizzazione della finestra di dialogo di conferma](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)

**Figura 07**: Visualizzazione della finestra di dialogo di conferma[(Fare clic per visualizzare l'immagine a dimensioni intere)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png)

Si noti che in genere non si trascina un'estensione di controllo in una pagina. Utilizzare invece l'opzione Aggiungi attività **Extender** per aggiungere un'estensione a un controllo già aggiunto a una pagina. Si noti inoltre che è possibile impostare le proprietà dell'estensione del controllo aprendo la finestra delle proprietà per il controllo da estendere.

Un singolo controllo ASP.NET può essere esteso da più estensioni di controllo. Nella finestra delle proprietà del controllo da estendere verranno elencate tutte le estensioni di controllo associate al controllo.

> [!div class="step-by-step"]
> [Successivo](get-started-with-the-ajax-control-toolkit-vb.md)
> [precedente](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)
