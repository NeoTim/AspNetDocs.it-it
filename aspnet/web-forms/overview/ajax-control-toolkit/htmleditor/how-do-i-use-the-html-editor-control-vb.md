---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
title: Come si usa il controllo editor HTML? (VB) Documenti Microsoft
author: rick-anderson
description: HTMLEditor è un ASP.NET controllo AJAX che consente di creare e modificare facilmente il contenuto HTML tramite pulsanti in una barra degli strumenti.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 32ec9321-7c8c-4b0f-8234-99acb56df6b5
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
msc.type: authoredcontent
ms.openlocfilehash: bba42b4b6ff130b209b92c1608bc37f2f574c19c
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542833"
---
# <a name="how-do-i-use-the-html-editor-control-vb"></a>Come si usa il controllo editor HTML? (VB)

da parte [di Microsoft](https://github.com/microsoft)

> HTMLEditor è un ASP.NET controllo AJAX che consente di creare e modificare facilmente il contenuto HTML tramite pulsanti in una barra degli strumenti.

L'obiettivo di questa esercitazione è fornire una panoramica del controllo Editor HTML incluso in AJAX Control Toolkit. L'editor HTML include opzioni per modificare la dimensione del carattere, selezionare un tipo di carattere, cambiare il colore di sfondo, modificare il colore di primo piano, aggiungere collegamenti, aggiungere immagini, modificare l'allineamento del testo ed eseguire operazioni di taglio, copia e incolla (vedere Figura 1).

[![L'editor HTML](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)

**Figura 01**: Editor HTML ([Fare clic per visualizzare l'immagine a dimensioni intere](how-do-i-use-the-html-editor-control-vb/_static/image2.png))

L'editor HTML consente di immettere contenuto utilizzando una modalità di progettazione oppure è possibile immettere direttamente HTML. Viene inoltre fornita l'opzione per visualizzare l'anteprima del contenuto HTML (vedere Figura 2).

[![Pulsanti Progettazione, HTML e Anteprima](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)

**Figura 02**: pulsanti Progettazione, HTML e Anteprima ([Fare clic per visualizzare l'immagine](how-do-i-use-the-html-editor-control-vb/_static/image4.png)a dimensioni intere)

In questa esercitazione viene illustrato come visualizzare l'editor HTML, come personalizzare i pulsanti della barra degli strumenti visualizzati nell'editor HTML e come evitare attacchi di cross-site scripting.

## <a name="displaying-the-html-editor"></a>Visualizzazione dell'editor HTML

Prima di poter utilizzare l'editor HTML in una pagina ASP.NET, è necessario aggiungere un controllo ScriptManager alla pagina. Il controllo ScriptManager si trova sotto la scheda Estensioni AJAX della casella degli strumenti di Visual Studio/Visual Web Developer Express.

È necessario inserire il ScriptManager controllo nella parte superiore della pagina prima di qualsiasi altro controllo nella pagina. Ad esempio, è possibile posizionarlo immediatamente &lt;sotto&gt; il tag del modulo sul lato server di apertura.

Il controllo Editor HTML si trova nella casella degli strumenti con il resto dei controlli AJAX Control Toolkit. Viene denominato controllo Editor (vedere La Figura 3).

[![Il controllo Editor HTML](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)

**Figura 03**: Il controllo Editor HTML ([Fare clic per visualizzare l'immagine a dimensioni intere)](how-do-i-use-the-html-editor-control-vb/_static/image6.png)

Dopo aver trascinato l'editor HTML in una pagina, è possibile impostarne le proprietà nella finestra delle proprietà. Ad esempio, in genere si desidera impostare il Width e Height proprietà. Il listato 1 contiene l'origine di una pagina ASP.NET che contiene un editor HTML.

**Listato 1 - SimpleEditor.aspxListing 1 - SimpleEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample1.aspx)]

La pagina nel listato 1 contiene un controllo Editor HTML, un controllo Button e un controllo Literal. Quando si fa clic sul pulsante, il contenuto dell'editor HTML viene visualizzato nel controllo Literal (vedere Figura 4).

[![Invio di un modulo con un editor HTML](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)

**Figura 04**: Invio di un modulo con un editor HTML([Fare clic per visualizzare l'immagine](how-do-i-use-the-html-editor-control-vb/_static/image8.png)a dimensioni intere)

La proprietà Html Editor Content viene utilizzata per recuperare il contenuto HTML immesso nell'editor HTML. Tieni presente che questo contenuto HTML può contenere JavaScript. Nella sezione successiva, discuteremo come è possibile prevenire gli attacchi di iniezione JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Personalizzazione della barra degli strumenti dell'editor HTML

È possibile personalizzare esattamente i pulsanti visualizzati nell'editor. Ad esempio, è possibile rimuovere la scheda HTML per impedire agli utenti di passare all'editor HTML in modalità HTML. In alternativa, è possibile rimuovere l'elenco a discesa dimensione del carattere per impedire agli utenti di creare testo eccessivamente grande in un post di messaggio del forum (vedere Figura 5).

[![Un editor HTML personalizzato](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)

**Figura 05**: Un editor HTML personalizzato ([Fare clic per visualizzare l'immagine](how-do-i-use-the-html-editor-control-vb/_static/image10.png)a dimensioni intere )

È possibile personalizzare i pulsanti della barra degli strumenti derivando un nuovo editor HTML dalla classe Editor di base. Ad esempio, l'editor personalizzato nel listato 2 contiene solo i pulsanti della barra degli strumenti per grassetto e corsivo. Tutti gli altri pulsanti della barra degli strumenti sono stati rimossi. Inoltre, la scheda HTML è stata rimossa dalla parte inferiore dell'editor (ma le schede Progettazione e Anteprima sono ancora presenti).

**Listato 2\_- Codice dell'applicazione, Editorpersonalizzato.vb**

[!code-vb[Main](how-do-i-use-the-html-editor-control-vb/samples/sample2.vb)]

È necessario aggiungere la classe nel\_listato 2 alla cartella App Code in modo che la classe verrà compilata automaticamente. Se la\_cartella App Code non esiste nel tuo sito web, puoi semplicemente aggiungere la cartella.

Dopo aver creato un editor personalizzato, è possibile aggiungerlo a una pagina ASP.NET nello stesso modo in cui si aggiunge il normale editor HTML (vedere il listato 3).

**Listato 3 - ShowCustomEditor.aspxListing 3 - ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Evitare attacchi di cross-site scripting (XSS)

Ogni volta che si accetta l'input di un utente e si rivisualizza tale input nel sito Web, è possibile aprire il sito Web agli attacchi XSS (Cross-Site Scripting). In teoria, un hacker malintenzionato potrebbe inviare codice JavaScript che viene eseguito quando l'input viene visualizzato nuovamente. Il JavaScript potrebbe essere utilizzato per rubare le password degli utenti o altre informazioni sensibili.

Normalmente, è possibile sconfiggere gli attacchi XSS con la codifica HTML qualsiasi input recuperato da un utente prima di visualizzarlo in una pagina web. Tuttavia, la codifica HTML l'output dell'editor HTML non solo codificai &lt;i tag di script,&gt; ma codifica va anche tutti i tag HTML. In altre parole, si perderebbe tutta la formattazione, ad esempio il tipo di carattere, la dimensione del carattere e il colore di sfondo.

Se si raccolgono informazioni riservate dagli utenti, ad esempio password, numeri di carta di credito e numeri di previdenza sociale, è consigliabile non visualizzare contenuti non codificati recuperati da un utente con l'editor HTML. È consigliabile utilizzare l'editor HTML solo in situazioni in cui non si sta rivisualizzando il contenuto HTML o il contenuto HTML viene inviato al sito Web da una parte attendibile.

Si supponga, ad esempio, che si sta creando un'applicazione blog. In questo caso, ha senso utilizzare l'editor HTML durante la composizione di post di blog. Tu sei l'unico che invia un post sul blog e, presumibilmente, puoi fidarti di te stesso di non inviare JavaScript dannoso. Tuttavia, non ha senso utilizzare l'editor HTML quando si consente agli utenti anonimi di pubblicare commenti. Si dovrebbe essere particolarmente attenti in situazioni in cui gli utenti inviano informazioni sensibili come le password. Potenzialmente, un utente malintenzionato potrebbe pubblicare un commento che contiene il codice JavaScript giusto per rubare una password.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stata fornita una breve panoramica del controllo Editor HTML incluso in AJAX Control Toolkit. Si è appreso come utilizzare l'editor HTML per accettare contenuto avanzato da un utente e inviare il contenuto al server. Abbiamo anche discusso come è possibile personalizzare i pulsanti della barra degli strumenti che vengono visualizzati dall'editor HTML. Infine, si è appreso come evitare attacchi di cross-site scripting quando si utilizza l'editor HTML per accettare input potenzialmente dannosi.

> [!div class="step-by-step"]
> [Indietro](how-do-i-use-the-html-editor-control-cs.md)
