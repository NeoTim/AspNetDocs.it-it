---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: Creazione di un'estensione di controllo VB (Controllo Kit di Controllo AJAX Personalizzato) Documenti Microsoft
author: rick-anderson
description: Gli Extender personalizzati consentono di personalizzare ed estendere le funzionalità dei controlli ASP.NET senza dover creare nuove classi.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: adce8e23ccf0a3364ee85534e98f8ea67ae4718d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543678"
---
# <a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a>Creazione di un controllo Extender di AJAX Control Toolkit personalizzato (VB)

da parte [di Microsoft](https://github.com/microsoft)

> Gli Extender personalizzati consentono di personalizzare ed estendere le funzionalità dei controlli ASP.NET senza dover creare nuove classi.

In questa esercitazione si apprenderà come creare un'estensione di controllo AJAX Control Toolkit personalizzata. Creiamo un nuovo Extender semplice ma utile che modifica lo stato di un Button da disabilitato a abilitato quando si digita testo in un TextBox.We create a simple, but useful, new extender that changes the state of a Button from disabled to enabled when you type text into a TextBox. Dopo aver letto questa esercitazione, sarà possibile estendere il ASP.NET AJAX Toolkit con le proprie estensioni di controllo.

È possibile creare estensioni di controllo personalizzate utilizzando Visual Studio o Visual Web Developer (assicurarsi di disporre della versione più recente di Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Panoramica di DisabledButton Extender

Il nuovo extender di controllo è denominato l'extender DisabledButton.Our new control extender is named the DisabledButton extender. Questo extender avrà tre proprietà:

- TargetControlID - Il TextBox che estende il controllo.
- TargetButtonIID - Il pulsante disabilitato o abilitato.
- DisabledText - Il testo inizialmente visualizzato nel pulsante. Quando si inizia a digitare, il Button visualizza il valore del Button Text proprietà.

Associare l'estensione DisabledButton a un TextBox e Button controllo. Prima di digitare qualsiasi testo, il Button è disabilitato e il TextBox e Button simile al seguente:

[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

([Fare clic per visualizzare l'immagine a dimensioni intere](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))

Dopo aver iniziato a digitare testo, il Button è abilitato e il TextBox e Button simile al seguente:

[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

([Fare clic per visualizzare l'immagine a dimensioni intere](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))

Per creare l'estensione di controllo, è necessario creare i tre file seguenti:To create our control extender, we need to create the following three files:

- DisabledButtonExtender.vb: questo file è la classe del controllo lato server che gestirà la creazione dell'estensione e consentirà di impostare le proprietà in fase di progettazione. Definisce inoltre le proprietà che possono essere impostate sull'extender. Queste proprietà sono accessibili tramite codice e in fase di progettazione e corrispondono alle proprietà definite nel file DisableButtonBehavior.js.These properties are accessible via code and at design time and match properties defined in the DisableButtonBehavior.js file.
- DisabledButtonBehavior.js -- Questo file è dove si aggiungerà tutta la logica di script client.
- DisabledButtonDesigner.vb: questa classe abilita la funzionalità in fase di progettazione. Questa classe è necessaria se si desidera che l'estensione del controllo funzioni correttamente con la finestra di progettazione di Visual Studio/Visual Web Developer.

Pertanto, un'estensione di controllo è costituita da un controllo lato server, un comportamento lato client e una classe della finestra di progettazione sul lato server. Si apprenderà come creare tutti e tre questi file nelle sezioni seguenti.

## <a name="creating-the-custom-extender-website-and-project"></a>Creazione del sito Web e del progetto Extender personalizzati

Il primo passaggio consiste nel creare un progetto di libreria di classi e un sito Web in Visual Studio/Visual Web Developer. Creeremo l'estensione personalizzata nel progetto della libreria di classi e testeremo l'estensione personalizzata nel sito Web.

Iniziamo con il sito web. Per creare il sito Web, attenersi alla seguente procedura:

1. Selezionare l'opzione di menu **File, Nuovo sito Web**.
2. Selezionare il **modello ASP.NET sito Web.**
3. Assegnare al nuovo sito Web il nome *Website1*.
4. Fare clic sul pulsante **OK**.

Successivamente, è necessario creare il progetto di libreria di classi che conterrà il codice per l'estensione del controllo:Next, we need to create the class library project that will contain the code for the control extender:

1. Selezionare l'opzione di menu **File, Aggiungi, Nuovo progetto**.
2. Selezionare il modello **Libreria di classi.**
3. Assegnare alla nuova libreria di classi il nome **CustomExtenders**.
4. Fare clic sul pulsante **OK**.

Dopo aver completato questi passaggi, la finestra Esplora soluzioni dovrebbe essere simile Figura 1.After you complete these steps, your Solution Explorer window should look like Figure 1.

[![Soluzione con sito Web e progetto di libreria di classi](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)

**Figura 01**: Soluzione con sito Web e progetto di libreria di classi[(Fare clic per visualizzare l'immagine](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png)a dimensioni intere)

Successivamente, è necessario aggiungere tutti i riferimenti all'assembly necessari al progetto di libreria di classi:Next, you need to add all of the necessary assembly references to the class library project:

1. Fare clic con il pulsante destro del mouse sul progetto CustomExtenders e selezionare l'opzione di menu **Aggiungi riferimento**.
2. Selezionare la scheda .NET.
3. Aggiungere riferimenti agli assembly riportati di seguito:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Selezionare la scheda Sfoglia.
5. Aggiungere un riferimento all'assembly AjaxControlToolkit.dll. Questo assembly si trova nella cartella in cui è stato scaricato AJAX Control Toolkit.

È possibile verificare di aver aggiunto tutti i riferimenti corretti facendo clic con il pulsante destro del mouse sul progetto, selezionando Proprietà e facendo clic sulla scheda Riferimenti (vedere la Figura 2).

[![Cartella Riferimenti con i riferimenti necessari](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)

**Figura 02**: cartella Riferimenti con i riferimenti necessari ([Fare clic per visualizzare l'immagine](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png)a dimensioni intere)

## <a name="creating-the-custom-control-extender"></a>Creazione dell'estensione del controllo personalizzatoCreating the Custom Control Extender

Ora che abbiamo la nostra libreria di classi, possiamo iniziare a costruire il nostro controllo Extender. Let s iniziare con le ossa nude di una classe di controllo Extender personalizzato (vedere il listato 1).

**Listato 1 - MyCustomExtender.vbListing 1 - MyCustomExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

Esistono diverse cose che si nota sulla classe di estensione del controllo nel listato 1.There are several things that you note about the control extender class in Listing 1. In primo luogo, si noti che la classe eredita dalla base ExtenderControlBase classe. Tutti i controlli estensione AJAX Control Toolkit derivano da questa classe di base. Ad esempio, la classe base include il TargetID proprietà che è una proprietà obbligatoria di ogni estensione di controllo.

Successivamente, si noti che la classe include i due attributi seguenti relativi allo script client:

- WebResource: fa sì che un file venga incluso come risorsa incorporata in un assembly.
- ClientScriptResource - Fa sì che una risorsa script venga recuperata da un assembly.

L'attributo WebResource viene utilizzato per incorporare il file JavaScript MyControlBehavior.js nell'assembly quando viene compilato l'extender personalizzato. L'attributo ClientScriptResource viene utilizzato per recuperare lo script MyControlBehavior.js dall'assembly quando l'estensione personalizzata viene utilizzata in una pagina Web.

Affinché gli attributi WebResource e ClientScriptResource funzionino, è necessario compilare il file JavaScript come risorsa incorporata. Selezionare il file nella finestra Esplora soluzioni, aprire la finestra delle proprietà e assegnare il valore *Risorsa incorporata* alla proprietà Operazione di **compilazione.**

Si noti che l'estensione di controllo include anche un attributo TargetControlType.Notice that the control extender also includes a TargetControlType attribute. Questo attributo viene utilizzato per specificare il tipo di controllo esteso dall'estensione del controllo. Nel caso del listato 1, l'estensione del controllo viene utilizzata per estendere un controllo TextBox.

Infine, si noti che l'estensione personalizzata include una proprietà denominata MyProperty.Finally, notice that the custom extender includes a property named MyProperty. La proprietà è contrassegnata con l'attributo ExtenderControlProperty. I metodi GetPropertyValue() e SetPropertyValue() vengono utilizzati per passare il valore della proprietà dall'estensione del controllo lato server al comportamento lato client.

Let s andare avanti e implementare il codice per il nostro DisabledButton Extender. Il codice per questo extender è disponibile nel listato 2.The code for this extender can be found in Listing 2.

**Listato 2 - DisabledButtonExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

L'estensione DisabledButton nel listato 2 dispone di due proprietà denominate TargetButtonID e DisabledText.The DisabledButton extender in Listing 2 has two properties named TargetButtonID and DisabledText. Il IDReferenceProperty applicato per il TargetButtonID proprietà impedisce di assegnare qualsiasi elemento diverso dall'ID di un Button controllo a questa proprietà.

Gli attributi WebResource e ClientScriptResource associano un comportamento lato client che si trova in un file denominato DisabledButtonBehavior.js a questo Extender. Discuteremo di questo file JavaScript nella sezione successiva.

## <a name="creating-the-custom-extender-behavior"></a>Creazione del comportamento di estensione personalizzatoCreating the Custom Extender Behavior

Il componente lato client di un'estensione di controllo è denominato comportamento. La logica effettiva per la disabilitazione e l'abilitazione di Button è contenuta nel comportamento DisabledButton. Il codice JavaScript per il comportamento è incluso nel listato 3.

**Listato 3 - DisabledButton.jsListing 3 - DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

Il file JavaScript nel listato 3 contiene una classe lato client denominata DisabledButtonBehavior.The JavaScript file in Listing 3 contains a client-side class named DisabledButtonBehavior. Questa classe, come il suo gemello sul lato server, include due\_proprietà denominate\_TargetButtonID e\_DisabledText\_a cui è possibile accedere utilizzando get TargetButtonID/set TargetButtonID e get DisabledText/set DisabledText.

Il metodo initialize() associa un gestore eventi keyup all'elemento di destinazione per il comportamento. Ogni volta che si digita una lettera nel TextBox associato a questo comportamento, viene eseguito il gestore keyup. Il gestore keyup abilita o disabilita il Button a seconda se il TextBox associato al comportamento contiene testo.

Tenere presente che è necessario compilare il file JavaScript nel listato 3 come risorsa incorporata. Selezionare il file nella finestra Esplora soluzioni, aprire la finestra delle proprietà e assegnare il valore *Risorsa incorporata* alla proprietà Operazione di **compilazione** (vedere la Figura 3). Questa opzione è disponibile sia in Visual Studio che in Visual Web Developer.

[![Aggiunta di un file JavaScript come risorsa incorporata](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)

**Figura 03**: Aggiunta di un file JavaScript come risorsa incorporata([Fare clic per visualizzare l'immagine](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png)a dimensioni intere )

## <a name="creating-the-custom-extender-designer"></a>Creazione della finestra di progettazione Extender personalizzataCreating the Custom Extender Designer

C'è un'ultima classe che dobbiamo creare per completare il nostro extender. È necessario creare la classe di progettazione nel listato 4.We need to create the designer class in Listing 4. Questa classe è necessaria per fare in modo che l'estensione si comporti correttamente con la finestra di progettazione di Visual Studio/Visual Web Developer.

**Listato 4 - DisabledButtonDesigner.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

Associare la finestra di progettazione nel listato 4 con il DisabledButton estensione con il Designer attributo. È necessario applicare l'attributo Designer alla classe DisabledButtonExtender in questo modo:You need to apply the Designer attribute to the DisabledButtonExtender class like this:

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a>Utilizzo dell'Extender personalizzato

Ora che abbiamo finito di creare l'estensione di controllo DisabledButton, è il momento di utilizzarlo nel nostro ASP.NET web. In primo luogo, è necessario aggiungere l'extender personalizzato alla casella degli strumenti. A tale scopo, seguire questa procedura:

1. Aprire una pagina ASP.NET facendo doppio clic sulla pagina nella finestra Esplora soluzioni.
2. Fare clic con il pulsante destro del mouse sulla casella degli strumenti e selezionare l'opzione di menu **Scegli elementi**.
3. Nella finestra di dialogo Scegli elementi della Casella degli strumenti passare all'assembly CustomExtenders.dll.
4. Fare clic sul pulsante **OK** per chiudere la finestra di dialogo.

Dopo aver completato questi passaggi, l'estensione di controllo DisabledButton deve essere visualizzato nella casella degli strumenti (vedere Figura 4).

[![DisabledButton nella casella degli strumenti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)

**Figura 04**: DisabledButton nella casella degli strumenti([Fare clic per visualizzare l'immagine a dimensioni intere](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))

Successivamente, è necessario creare una nuova pagina ASP.NET. A tale scopo, seguire questa procedura:

1. Creare una nuova pagina di ASP.NET denominata ShowDisabledButton.aspx.Create a new ASP.NET page named ShowDisabledButton.aspx.
2. Trascinare un ScriptManager nella pagina.
3. Trascinare un controllo TextBox nella pagina.
4. Trascinare un Button controllo nella pagina.
5. Nella finestra Proprietà modificare la proprietà ID pulsante sul valore <em>btnSave</em> e la proprietà Text sul valore *\*Save*.

È stata creata una pagina con un controllo standard ASP.NET TextBox e Button.We created a page with a standard textBox and Button control.

Successivamente, è necessario estendere il controllo TextBox con l'estensione DisabledButton:Next, we need to extend the TextBox control with the DisabledButton extender:

1. Selezionare l'opzione Aggiungi attività **Extender** per aprire la finestra di dialogo Creazione guidata estensione (vedere la Figura 5). Si noti che la finestra di dialogo include l'estensione DisabledButton personalizzata.
2. Selezionare l'extender DisabledButton e fare clic sul pulsante **OK.**

[![Finestra di dialogo Creazione guidata Extender](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)

**Figura 05**: Finestra di dialogo Creazione guidata Extender[(Fare clic per visualizzare l'immagine a dimensioni intere)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png)

Infine, è possibile impostare le proprietà del DisabledButton estensione. È possibile modificare le proprietà dell'estensione DisabledButton modificando le proprietà del controllo TextBox:

1. Selezionare il TextBox nella finestra di progettazione.
2. Nella finestra Proprietà espandere il nodo Extender (vedere la figura 6).
3. Assegnare il valore *Save* alla proprietà DisabledText e il valore *btnSave* alla proprietà TargetButtonID.

[![Impostazione delle proprietà dell'extender](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)

**Figura 06**: Impostazione delle proprietà dell'extender([Fare clic per visualizzare l'immagine a dimensioni intere)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png)

Quando si esegue la pagina (premendo F5), il Button controllo è inizialmente disabilitato. Non appena si inizia a immettere testo nella casella di testo, il Button controllo è abilitato (vedere Figura 7).

[![L'estensione DisabledButton in azioneThe DisabledButton extender in action](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)

**Figura 07**: L'estensione DisabledButton in azione([Fare clic per visualizzare l'immagine](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png)a dimensioni intere )

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione era spiegare come è possibile estendere AJAX Control Toolkit con controlli extender personalizzati. In questa esercitazione è stato creato un semplice extender di controllo DisabledButton.In this tutorial, we created a simple DisabledButton control extender. Questo extender è stato implementato creando una classe DisabledButtonExtender, un comportamento JavaScript DisabledButtonBehavior e una classe DisabledButtonDesigner.We implemented this extender by creating a DisabledButtonExtender class, a DisabledButtonBehavior JavaScript behavior, and a DisabledButtonDesigner class. Si segue un set di passaggi simile ogni volta che si crea un'estensione di controllo personalizzato.

> [!div class="step-by-step"]
> [Indietro](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
