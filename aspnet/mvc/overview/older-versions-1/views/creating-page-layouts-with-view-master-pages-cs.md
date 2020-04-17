---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
title: Creazione di layout di pagina con le pagine master di visualizzazione (c'è) Documenti Microsoft
author: rick-anderson
description: In questa esercitazione si apprenderà come creare un layout di pagina comune per più pagine nell'applicazione sfruttando le pagine master di visualizzazione. È possibile utilizzare un...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: dff54fcb-68b1-4488-89a2-ca97532d6a4c
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: d313e017d7061ae03e8dea79e611d0cf3838297d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542521"
---
# <a name="creating-page-layouts-with-view-master-pages-c"></a>Creazione di layout di pagina con le pagine master di visualizzazione (C#)

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_CS.pdf)

> In questa esercitazione si apprenderà come creare un layout di pagina comune per più pagine nell'applicazione sfruttando le pagine master di visualizzazione. È possibile utilizzare una pagina master di visualizzazione, ad esempio, per definire un layout di pagina a due colonne e utilizzare il layout a due colonne per tutte le pagine nell'applicazione Web.

## <a name="creating-page-layouts-with-view-master-pages"></a>Creazione di layout di pagina con le pagine master di visualizzazione

In questa esercitazione si apprenderà come creare un layout di pagina comune per più pagine nell'applicazione sfruttando le pagine master di visualizzazione. È possibile utilizzare una pagina master di visualizzazione, ad esempio, per definire un layout di pagina a due colonne e utilizzare il layout a due colonne per tutte le pagine nell'applicazione Web.

È inoltre possibile sfruttare le pagine master di visualizzazione per condividere contenuto comune tra più pagine nell'applicazione. Ad esempio, è possibile inserire il logo del sito Web, i collegamenti di navigazione e gli annunci banner in una pagina master di visualizzazione. In questo modo, ogni pagina dell'applicazione visualizzerebbe automaticamente questo contenuto.

In questa esercitazione si apprenderà come creare una nuova pagina master di visualizzazione e creare una nuova pagina di contenuto di visualizzazione basata sulla pagina master.

### <a name="creating-a-view-master-page"></a>Creazione di una pagina master di visualizzazioneCreating a View Master Page

Iniziamo creando una pagina master di visualizzazione che definisce un layout a due colonne. Per aggiungere una nuova pagina master di visualizzazione a un progetto MVC, fare clic con il pulsante destro del mouse sulla cartella Views-Shared, selezionare l'opzione di menu **Add, New Item**e selezionare il modello MVC **View Master Page** (vedere La Figura 1).

[![Aggiunta di una pagina master di visualizzazione](creating-page-layouts-with-view-master-pages-cs/_static/image2.png)](creating-page-layouts-with-view-master-pages-cs/_static/image1.png)

**Figura 01**: Aggiunta di una pagina master di visualizzazione ([Fare clic per visualizzare l'immagine a dimensioni intere](creating-page-layouts-with-view-master-pages-cs/_static/image3.png))

È possibile creare più di una pagina master di visualizzazione in un'applicazione. Ogni pagina master di visualizzazione può definire un layout di pagina diverso. Ad esempio, è possibile che si desideri che alcune pagine abbiano un layout a due colonne e altre pagine abbiano un layout a tre colonne.

Una pagina master di visualizzazione è molto simile a una visualizzazione MVC standard ASP.NET. Tuttavia, a differenza di una visualizzazione normale, una pagina master di visualizzazione contiene uno o più `<asp:ContentPlaceHolder>` tag. I `<contentplaceholder>` tag vengono utilizzati per contrassegnare le aree della pagina master che possono essere sostituite in una singola pagina di contenuto.

Ad esempio, la pagina master di visualizzazione nel listato 1 definisce un layout a due colonne. Contiene due `<contentplaceholder>` tag. Uno `<ContentPlaceHolder>` per ogni colonna.

**Lista torto 1 –`Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample1.aspx)]

Il corpo della pagina master di `<div>` visualizzazione nel listato 1 contiene due tag che corrispondono alle due colonne. La classe di colonna Foglio di `<div>` stile CSS viene applicata a entrambi i tag. Questa classe è definita nel foglio di stile dichiarato nella parte superiore della pagina master. È possibile visualizzare in anteprima come verrà eseguito il rendering della pagina master di visualizzazione passando alla visualizzazione Progettazione. Fare clic sulla scheda Progettazione nella parte inferiore sinistra dell'editor del codice sorgente (vedere la Figura 2).

[![Anteprima di una pagina master nella finestra di progettazione](creating-page-layouts-with-view-master-pages-cs/_static/image5.png)](creating-page-layouts-with-view-master-pages-cs/_static/image4.png)

**Figura 02**: Anteprima di una pagina master nella finestra di progettazione ([Fare clic per visualizzare l'immagine](creating-page-layouts-with-view-master-pages-cs/_static/image6.png)a dimensioni intere )

### <a name="creating-a-view-content-page"></a>Creazione di una pagina di contenuto di visualizzazioneCreating a View Content Page

Dopo aver creato una pagina master di visualizzazione, è possibile creare una o più pagine di contenuto di visualizzazione basate sulla pagina master di visualizzazione. Ad esempio, è possibile creare una pagina di contenuto della visualizzazione Indice per il controller Home facendo clic con il pulsante destro del mouse sulla cartella Views-Home, scegliendo **Aggiungi, Nuovo elemento**, selezionando il modello **Pagina contenuto visualizzazione MVC** , immettendo il nome Index.aspx e facendo clic sul pulsante **Aggiungi** (vedere la Figura 3).

[![Aggiunta di una pagina di contenuto di visualizzazione](creating-page-layouts-with-view-master-pages-cs/_static/image8.png)](creating-page-layouts-with-view-master-pages-cs/_static/image7.png)

**Figura 03**: Aggiunta di una pagina di contenuto di visualizzazione ([Fare clic per visualizzare l'immagine](creating-page-layouts-with-view-master-pages-cs/_static/image9.png)a dimensioni intere )

Dopo aver fatto clic sul pulsante Aggiungi, viene visualizzata una nuova finestra di dialogo che consente di selezionare una pagina master di visualizzazione da associare alla pagina di contenuto di visualizzazione (vedere Figura 4). È possibile passare alla pagina master della visualizzazione Site.master creata nella sezione precedente.

[![Selezione di una pagina master](creating-page-layouts-with-view-master-pages-cs/_static/image11.png)](creating-page-layouts-with-view-master-pages-cs/_static/image10.png)

**Figura 04**: Selezione di una pagina master ([Fare clic per visualizzare l'immagine a dimensioni intere](creating-page-layouts-with-view-master-pages-cs/_static/image12.png))

Dopo aver creato una nuova pagina di contenuto di visualizzazione basata sul Site.master pagina master, si ottiene il file nel listato 2.

**Listato 2 –`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample2.aspx)]

Si noti che `<asp:Content>` questa visualizzazione contiene `<asp:ContentPlaceHolder>` un tag che corrisponde a ciascuno dei tag nella pagina master di visualizzazione. Ogni `<asp:Content>` tag include un attributo ContentPlaceHolderID che punta alla particolare cosa `<asp:ContentPlaceHolder>` con cui esegue l'override.

Si noti, inoltre, che la pagina di visualizzazione del contenuto nel listato 2 non contiene alcuno dei normali tag HTML di apertura e chiusura. Ad esempio, non contiene i `<html>` tag `<head>` o di apertura. Tutti i normali tag di apertura e chiusura sono contenuti nella pagina master di visualizzazione.

Qualsiasi contenuto che si desidera visualizzare in una pagina `<asp:Content>` di contenuto di visualizzazione deve essere inserito all'interno di un tag. Se inserisci qualsiasi codice HTML o altro contenuto al di fuori di questi tag, si otterrà un errore quando si tenta di visualizzare la pagina.

Non è necessario sostituire `<asp:ContentPlaceHolder>` tutti i tag da una pagina master in una pagina di visualizzazione contenuto. È necessario sostituire `<asp:ContentPlaceHolder>` un tag solo quando si desidera sostituire il tag con contenuto specifico.

Ad esempio, la vista Indice modificata `<asp:Content>` nel listato 3 contiene solo due tag. Ognuno `<asp:Content>` dei tag include del testo.

**Lista 3 –`Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample3.aspx)]

Quando viene richiesta la visualizzazione nel listato 3, viene eseguito il rendering della pagina in Figura 5. Si noti che la visualizzazione esegue il rendering di una pagina con due colonne. Si noti, inoltre, che il contenuto della pagina di contenuto di visualizzazione viene unito al contenuto della pagina master di visualizzazione

[![Pagina di contenuto della visualizzazione Indice](creating-page-layouts-with-view-master-pages-cs/_static/image14.png)](creating-page-layouts-with-view-master-pages-cs/_static/image13.png)

**Figura 05**: Pagina del contenuto della visualizzazione Indice ([Fare clic per visualizzare l'immagine a dimensioni intere)](creating-page-layouts-with-view-master-pages-cs/_static/image15.png)

### <a name="modifying-view-master-page-content"></a>Modifica del contenuto della pagina master di visualizzazione

Un problema che si verifica quasi immediatamente quando si lavora con visualizzare le pagine master è il problema di modificare visualizzare il contenuto della pagina master quando vengono richieste pagine di contenuto di visualizzazione diverse. Ad esempio, si desidera che ogni pagina dell'applicazione Web abbia un titolo univoco. Tuttavia, il titolo viene dichiarato nella pagina master di visualizzazione e non nella pagina di contenuto di visualizzazione. Quindi, come si personalizza il titolo della pagina per ogni pagina di contenuto di visualizzazione?

Esistono due modi per modificare il titolo visualizzato da una pagina di contenuto di visualizzazione. In primo luogo, è possibile assegnare `<%@ page %>` un titolo di pagina per l'attributo title della direttiva dichiarata nella parte superiore di una pagina di contenuto di visualizzazione. Ad esempio, se si desidera assegnare il titolo della pagina "Super Grande sito web" alla vista Indice, è possibile includere la seguente direttiva nella parte superiore della vista Indice:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample4.aspx)]

Quando viene eseguito il rendering della vista Indice nel browser, il titolo desiderato viene visualizzato nella barra del titolo del browser:

[![Barra del titolo del browser](creating-page-layouts-with-view-master-pages-cs/_static/image17.png)](creating-page-layouts-with-view-master-pages-cs/_static/image16.png)

C'è un requisito importante che una pagina di visualizzazione master deve soddisfare affinché l'attributo title funzioni. La pagina master di `<head runat="server">` visualizzazione deve `<head>` contenere un tag anziché un tag normale per la relativa intestazione. Se `<head>` il tag non include l'attributo runat "server", il titolo non verrà visualizzato. La pagina master di `<head runat="server">` visualizzazione predefinita include il tag richiesto.

Un approccio alternativo alla modifica del contenuto della pagina master da una singola `<asp:ContentPlaceHolder>` pagina di contenuto di visualizzazione consiste nell'eseguire il wrapping dell'area che si desidera modificare in un tag. Si supponga, ad esempio, di voler modificare non solo il titolo, ma anche i meta tag, sottoposti a rendering da una pagina di visualizzazione master. La pagina di visualizzazione mastro nel listato 4 contiene un `<asp:ContentPlaceHolder>` tag all'interno del relativo `<head>` tag.

**Lista 4 –`Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample5.aspx)]

Si noti che il `<asp:ContentPlaceHolder>` tag nel listato 4 include contenuto predefinito: un titolo predefinito e meta tag predefiniti. Se non si esegue `<asp:ContentPlaceHolder>` l'override di questo tag in una singola pagina di contenuto di visualizzazione, verrà visualizzato il contenuto predefinito.

La pagina di visualizzazione del `<asp:ContentPlaceHolder>` contenuto nel listato 5 sostituisce il tag per visualizzare un titolo personalizzato e meta tag personalizzati.

**Lista 5 –`Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample6.aspx)]

### <a name="summary"></a>Riepilogo

Questa esercitazione ha fornito un'introduzione di base per visualizzare le pagine master e le pagine di contenuto. Si è appreso come creare nuove pagine master di visualizzazione e creare pagine di contenuto di visualizzazione basate su di esse. È stato inoltre esaminato come è possibile modificare il contenuto di una pagina master di visualizzazione da una determinata pagina di contenuto di visualizzazione.

> [!div class="step-by-step"]
> [Successivo](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
> [precedente](passing-data-to-view-master-pages-cs.md)
