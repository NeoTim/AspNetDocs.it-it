---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
title: Master/dettagli filtro usando un controllo DropDownList (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione si noterà come visualizzare i record master in un controllo DropDownList e i dettagli dell'elemento di elenco selezionato in un controllo GridView.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: ea44717e-ab2e-46cd-a692-e4a9c0de194c
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 5db5e30cac21bad0591f4476a1b1156b50117536
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59382264"
---
# <a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Applicazione di filtri al report master o di dettaglio usando un controllo DropDownList (VB)

da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_7_VB.exe) o [Scarica il PDF](master-detail-filtering-with-a-dropdownlist-vb/_static/datatutorial07vb1.pdf)

> In questa esercitazione si noterà come visualizzare i record master in un controllo DropDownList e i dettagli dell'elemento di elenco selezionato in un controllo GridView.


## <a name="introduction"></a>Introduzione

Un tipo comune di report è il *master-details report*, in cui il report inizia con la visualizzazione di alcuni set di record "master". L'utente può quindi il drill-down in uno dei record master, in tal modo la visualizzazione "dei dettagli." di record master Master-Details report è la scelta ideale per la visualizzazione delle relazioni uno-a-molti, ad esempio un report che mostra tutte le categorie e quindi consentire a un utente di selezionare una determinata categoria e visualizzare i prodotti associati. Report master/dettaglio risultano inoltre utili per la visualizzazione di informazioni dettagliate dalle tabelle particolarmente "wide" (ovvero quelle che presenta un elevato numero di colonne). Ad esempio, il livello di "master" di un rapporto master/dettagli potrebbe mostrare solo il prodotto nome e il prezzo unitario dei prodotti nel database e il drill-down in un determinato prodotto mostrerebbe i campi aggiuntivi del prodotto (categoria, fornitore, la quantità per unità, e così via).

Esistono molti modi con cui può essere implementato un rapporto master/dettaglio. Su questo e successive tre esercitazioni esamineremo una varietà di report master o di dettaglio. In questa esercitazione verrà illustrato come visualizzare i record master in un [controllo DropDownList](https://msdn.microsoft.com/library/dtx91y0z.aspx) e i dettagli dell'elemento di elenco selezionato in un controllo GridView. In particolare, report master o di dettaglio dell'esercitazione Elenca informazioni category e product.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Passaggio 1: Visualizzare le categorie in un controllo DropDownList

Report master o di dettaglio elencherà le categorie presenti in un elenco a discesa con i prodotti dell'elemento elenco selezionato visualizzati più in basso nella pagina in un controllo GridView. La prima attività prima di Stati Uniti, è quindi disporre le categorie visualizzate in un controllo DropDownList. Aprire il `FilterByDropDownList.aspx` nella pagina la `Filtering` cartella, trascinare un controllo DropDownList dalla casella degli strumenti nella finestra di progettazione della pagina e impostare relativo `ID` proprietà `Categories`. Fare quindi clic sul collegamento dallo smart tag del controllo DropDownList Scegli origine dati. Verrà visualizzata la configurazione guidata origine dati.


[![Sspecificare l'origine dati del controllo DropDownList](master-detail-filtering-with-a-dropdownlist-vb/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image1.png)

**Figura 1**: Specificare l'origine dati del controllo DropDownList ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-vb/_static/image3.png))


Scegliere di aggiungere un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource` che richiama la `CategoriesBLL` della classe `GetCategories()` (metodo).


[![Aun nuovo CategoriesDataSource denominato di ObjectDataSource gg](master-detail-filtering-with-a-dropdownlist-vb/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image4.png)

**Figura 2**: Aggiungere un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-vb/_static/image6.png))


[![Cimpostare come usare la classe CategoriesBLL](master-detail-filtering-with-a-dropdownlist-vb/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image7.png)

**Figura 3**: Scegliere di usare la `CategoriesBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-vb/_static/image9.png))


[![Cconfigurare ObjectDataSource per utilizzare il metodo GetCategories()](master-detail-filtering-with-a-dropdownlist-vb/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image10.png)

**Figura 4**: Configurare ObjectDataSource per usare la `GetCategories()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-vb/_static/image12.png))


Dopo la configurazione di ObjectDataSource è ancora necessario specificare quale campo dell'origine dati deve essere visualizzato in DropDownList e che uno deve essere associato come valore per l'elemento dell'elenco. Dispone il `CategoryName` campo, come la visualizzazione e `CategoryID` come valore per ogni elemento dell'elenco.


[![Hla visualizzazione del controllo DropDownList CategoryName Field AVE e CategoryID Usa come valore](master-detail-filtering-with-a-dropdownlist-vb/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image13.png)

**Figura 5**: La visualizzazione DropDownList il `CategoryName` campo e l'utilizzo `CategoryID` come valore ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-vb/_static/image15.png))


A questo punto si dispone di un controllo DropDownList popolata con i record dal `Categories` tabella (tutto eseguita in circa sei secondi). Figura 6 mostra lo stato di avanzamento fino ad ora, quando viene visualizzato tramite un browser.


[![A Elenco a discesa Elenca le categorie corrente](master-detail-filtering-with-a-dropdownlist-vb/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image16.png)

**Figura 6**: Un elenco a discesa Elenca le categorie correnti ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-vb/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>Passaggio 2: Aggiunta di GridView prodotti

L'ultimo passaggio nel report master o di dettaglio è elencare i prodotti associati alla categoria selezionata. A tale scopo, aggiungere un controllo GridView alla pagina e creare un nuovo oggetto ObjectDataSource denominato `productsDataSource`. Disporre le `productsDataSource` controllo selezionare i relativi dati dal `ProductsBLL` della classe `GetProductsByCategoryID(categoryID)` (metodo).


[![SScegliere il metodo GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-vb/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image19.png)

**Figura 7**: Selezionare il `GetProductsByCategoryID(categoryID)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-vb/_static/image21.png))


Dopo aver scelto il metodo, la procedura guidata ObjectDataSource richiede il valore per il metodo *`categoryID`* parametro. Per utilizzare il valore dell'oggetto selezionato `categories` DropDownList elemento impostato l'origine del parametro al controllo e il ControlID a `Categories`.


[![Sil parametro sul valore di DropDownList categorie categoryID et](master-detail-filtering-with-a-dropdownlist-vb/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image22.png)

**Figura 8**: Impostare il *`categoryID`* parametro per il valore del `Categories` DropDownList ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-vb/_static/image24.png))


Si consiglia di estrarre lo stato di avanzamento in un browser. Durante la prima visita la pagina, i prodotti appartengano alla categoria selezionata (bevande) vengono visualizzati (come illustrato nella figura 9), ma la modifica DropDownList non aggiorna i dati. Infatti, deve verificarsi un postback per il controllo GridView per l'aggiornamento. A tale scopo sono disponibili due opzioni (nessuno dei quali è necessario scrivere alcun codice):

- **Impostare le categorie DropDownList**[proprietà AutoPostBack](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**su True.** (È possibile farlo selezionando l'opzione Abilita AutoPostBack nello smart tag del controllo DropDownList.) Ogni volta che il controllo DropDownList selezionata verrà attivato un postback elemento è stato modificato dall'utente. Pertanto, quando l'utente seleziona una nuova categoria da DropDownList negativi a un postback e GridView verrà aggiornato con i prodotti per la categoria appena selezionato. (Questo è l'approccio che ho utilizzato in questa esercitazione).
- **Aggiungere un controllo Web sul pulsante accanto a DropDownList.** Impostare il `Text` proprietà per l'aggiornamento o qualcosa di simile. Con questo approccio, l'utente dovrà selezionare una nuova categoria e quindi fare clic sul pulsante. Fare clic sul pulsante verrà possono causare un postback e aggiorna il GridView per elencare i prodotti della categoria selezionata.

Le figure da 9 e 10 illustrano il rapporto master/dettaglio in azione.


[![Wuando prima visita la pagina, i prodotti di bevande sono visualizzate](master-detail-filtering-with-a-dropdownlist-vb/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image25.png)

**Figura 9**: Durante la prima visita la pagina, vengono visualizzati i prodotti di bevande ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-vb/_static/image27.png))


[![Sdesignare un nuovo prodotto (producono) genera automaticamente un PostBack, l'aggiornamento di GridView](master-detail-filtering-with-a-dropdownlist-vb/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image28.png)

**Figura 10**: Se si seleziona automaticamente un nuovo prodotto (produzione), un PostBack, l'aggiornamento di GridView ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-vb/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>Aggiunta di un elemento di elenco ", scegliere una categoria"

Quando si visita prima la `FilterByDropDownList.aspx` pagina le categorie primo elemento di elenco del DropDownList (bevande) è selezionata per impostazione predefinita, che mostra i prodotti delle bibite nel GridView. Piuttosto che mostra i prodotti della categoria prima, che si può decidere di invece avere un elemento del controllo DropDownList selezionato che trovo una regola, ad esempio, "-- seleziona una categoria,".

Per aggiungere un nuovo elemento elenco per il controllo DropDownList, passare alla finestra proprietà e fare clic sui puntini di sospensione di `Items` proprietà. Aggiungere un nuovo elemento elenco con il `Text` ", scegliere una categoria," e il `Value` `-1`.


[![Agg--scegliere una categoria: elemento di elenco](master-detail-filtering-with-a-dropdownlist-vb/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image31.png)

**Figura 11**: Aggiungere un: scegliere una categoria: elemento di elenco ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-vb/_static/image33.png))


In alternativa, è possibile aggiungere l'elemento dell'elenco, aggiungere il markup seguente per il controllo DropDownList:


[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample1.aspx)]

Inoltre, è necessario impostare il controllo DropDownList `AppendDataBoundItems` su True perché quando le categorie vengono associate a DropDownList da ObjectDataSource queste sovrascriveranno eventuali voci di elenco aggiunte manualmente se `AppendDataBoundItems` non è True.


![Impostare la proprietà AppendDataBoundItems su True](master-detail-filtering-with-a-dropdownlist-vb/_static/image34.png)

**Figura 12**: Impostare il `AppendDataBoundItems` proprietà su True


Dopo tali modifiche, prima di tutto gli utenti in visita la pagina è selezionata l'opzione "-- seleziona una categoria," e prodotti non vengono visualizzata.


[![On vengono visualizzati i prodotti di nessun carico pagina iniziale](master-detail-filtering-with-a-dropdownlist-vb/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image35.png)

**Figura 13**: Con i prodotti di nessun carico pagina iniziale vengono visualizzati ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-vb/_static/image37.png))


Il motivo prodotti non vengono visualizzata quando perché l'elemento di elenco ", scegliere una categoria," è selezionato è il valore è `-1` e non sono presenti prodotti nel database con un `CategoryID` di `-1`. Se si tratta del comportamento desiderato, quindi a questo punto è terminato. Se, tuttavia, si desidera visualizzare *tutti i* delle categorie di quando è selezionata la voce di elenco ", scegliere una categoria,", tornare al `ProductsBLL` classe e personalizzare il `GetProductsByCategoryID(categoryID)` metodo in modo che si richiama il `GetProducts()` metodo se il valore passato in *`categoryID`* parametro è minore di zero:


[!code-vb[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample2.vb)]

La tecnica usata in questo esempio è simile all'approccio viene utilizzati per visualizzare tutti i fornitori nel [parametri dichiarativi](../basic-reporting/declarative-parameters-cs.md) tutorial, anche se per questo esempio verrà usato un valore di `-1` per indicare che devono essere tutti i record recuperato in contrapposizione a `Nothing`. Infatti il *`categoryID`* parametri del `GetProductsByCategoryID(categoryID)` metodo prevede come valore intero passato, mentre nell'esercitazione parametri dichiarativi è stavamo passando un parametro di input di stringa.

Figura 14 viene illustrata una cattura di schermata della `FilterByDropDownList.aspx` quando è selezionata l'opzione "-- seleziona una categoria,". In questo caso, tutti i prodotti vengono visualizzati per impostazione predefinita e l'utente può limitare la visualizzazione scegliendo una categoria specifica.


[![All dei prodotti sono ora elencati per impostazione predefinita](master-detail-filtering-with-a-dropdownlist-vb/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image38.png)

**Figura 14**: Tutti i prodotti sono ora elencati per impostazione predefinita ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-vb/_static/image40.png))


## <a name="summary"></a>Riepilogo

Quando si visualizzano dati correlati in modo gerarchico, è spesso utile per presentare i dati usando i report master o di dettaglio, da cui l'utente può iniziare ad analizzare i dati nella parte superiore della gerarchia e il drill-down nei dettagli. In questa esercitazione abbiamo esaminato la creazione di un report master o di dettaglio che mostri i prodotti della categoria selezionata. Questa operazione è stata eseguita usando un controllo DropDownList per l'elenco di categorie e un controllo GridView per i prodotti appartenenti alla categoria selezionata.

Nel [prossima esercitazione](master-detail-filtering-with-two-dropdownlists-vb.md) Daremo il passaggio di un'interfaccia DropDownList ulteriormente, usando due controlli DropDownList.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
> [Successivo](master-detail-filtering-with-two-dropdownlists-vb.md)
