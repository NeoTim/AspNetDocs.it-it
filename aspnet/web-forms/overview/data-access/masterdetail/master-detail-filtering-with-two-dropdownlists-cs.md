---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
title: Master/dettagli filtro due controlli DropDownList (c#) | Microsoft Docs
author: rick-anderson
description: Questa esercitazione si espande la relazione master-Details per aggiungere un terzo livello, usando due controlli DropDownList per selezionare i recor desiderato padre e padre del padre...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: ac4b0d77-4816-4ded-afd0-88dab667aedd
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
msc.type: authoredcontent
ms.openlocfilehash: 03d0cd7e835b5526af60a21679260f849714c37e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59421290"
---
# <a name="masterdetail-filtering-with-two-dropdownlists-c"></a>Applicazione di filtri al report master o di dettaglio usando due controlli DropDownList (C#)

da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_8_CS.exe) o [Scarica il PDF](master-detail-filtering-with-two-dropdownlists-cs/_static/datatutorial08cs1.pdf)

> Questa esercitazione si espande la relazione master-Details per aggiungere un terzo livello, usando due controlli DropDownList per selezionare i record padre e padre del padre desiderati.


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](master-detail-filtering-with-a-dropdownlist-cs.md) abbiamo esaminato il modo visualizzare un report semplice master/dettaglio mediante un singolo controllo DropDownList popolato con le categorie e un controllo GridView che mostra i prodotti appartenenti alla categoria selezionata. Questo modello di report funziona bene quando si visualizzano i record che hanno una relazione uno-a-molti e possono essere facilmente esteso per lavorare con gli scenari che includono più relazioni uno-a-molti. Ad esempio, un sistema di immissione ordini avrebbe tabelle che corrispondono ai clienti, ordini e righe d'ordine. Un determinato cliente può disporre di più ordini con tutti gli ordini costituito da più elementi. Questo tipo di dati può essere presentato all'utente con due controlli DropDownList e un controllo GridView. Il primo controllo DropDownList avrebbe una voce di elenco per ogni cliente nel database con il secondo uno contenuto in fase di ordini effettuati dal cliente selezionato. Un controllo GridView elencherà le voci dell'ordine selezionato.

Mentre il database Northwind includere le informazioni di dettagli cliente/ordine canonici nel relativo `Customers`, `Orders`, e `Order Details` tabelle, queste tabelle non vengono rilevate nell'architettura. Ciononostante, è comunque possibile descrivere usando due controlli DropDownList dipendenti. Il primo controllo DropDownList elencherà le categorie e il secondo i prodotti appartenenti alla categoria selezionata. Un controllo DetailsView quindi elencherà i dettagli del prodotto selezionato.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Passaggio 1: Creazione e popolamento DropDownList categorie

Il nostro primo obiettivo consiste nell'aggiungere il controllo DropDownList che elenca le categorie. Questi passaggi sono stati esaminati in dettaglio nell'esercitazione precedente, ma sono riepilogati di seguito per motivi di completezza.

Aprire il `MasterDetailsDetails.aspx` nella pagina la `Filtering` cartella, aggiungere un controllo DropDownList alla pagina, imposta relativo `ID` proprietà `Categories`e quindi fare clic sul collegamento Configura origine dati nel relativo smart tag. Scegliere la configurazione guidata origine dati per aggiungere una nuova origine dati.


[![Auna nuova origine dati per il controllo DropDownList gg](master-detail-filtering-with-two-dropdownlists-cs/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image1.png)

**Figura 1**: Aggiungere una nuova origine dati per il controllo DropDownList ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image3.png))


La nuova origine dati deve, naturalmente, essere ObjectDataSource. Denominare il nuovo oggetto ObjectDataSource `CategoriesDataSource` , che verrà richiamare il `CategoriesBLL` dell'oggetto `GetCategories()` (metodo).


[![Cimpostare come usare la classe CategoriesBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image4.png)

**Figura 2**: Scegliere di usare la `CategoriesBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image6.png))


[![Cconfigurare ObjectDataSource per utilizzare il metodo GetCategories()](master-detail-filtering-with-two-dropdownlists-cs/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image7.png)

**Figura 3**: Configurare ObjectDataSource per usare la `GetCategories()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image9.png))


Dopo la configurazione di ObjectDataSource è comunque necessario specificare quale campo dell'origine dati deve essere visualizzato nei `Categories` DropDownList e quale deve essere configurato come il valore dell'elemento di elenco. Impostare il `CategoryName` campo, come la visualizzazione e `CategoryID` come valore per ogni elemento dell'elenco.


[![Hla visualizzazione del controllo DropDownList CategoryName Field AVE e CategoryID Usa come valore](master-detail-filtering-with-two-dropdownlists-cs/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image10.png)

**Figura 4**: La visualizzazione DropDownList il `CategoryName` campo e l'utilizzo `CategoryID` come valore ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image12.png))


A questo punto si dispone di un controllo DropDownList (`Categories`) che viene popolato con i record dal `Categories` tabella. Quando l'utente sceglie una nuova categoria da DropDownList sarebbe auspicabile un postback per aggiornare il prodotto DropDownList che andremo a creare nel passaggio 2. Pertanto, selezionare l'opzione Abilita AutoPostBack dal `categories` tag intelligente del controllo DropDownList.


[![EAbilita un postback automatico per il controllo DropDownList categorie](master-detail-filtering-with-two-dropdownlists-cs/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image13.png)

**Figura 5**: Povolit vlastnost AutoPostBack per il `Categories` DropDownList ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Passaggio 2: Visualizzazione dei prodotti della categoria selezionata in un secondo DropDownList

Con la `Categories` DropDownList completato, il passaggio successivo consiste nella visualizzazione di un controllo DropDownList dei prodotti appartenenti alla categoria selezionata. A tale scopo, aggiungere un altro controllo DropDownList alla pagina denominata `ProductsByCategory`. Come con le `Categories` DropDownList, creare un nuovo oggetto ObjectDataSource per il `ProductsByCategory` DropDownList denominato `ProductsByCategoryDataSource`.


[![Auna nuova origine dati per ProductsByCategory DropDownList gg](master-detail-filtering-with-two-dropdownlists-cs/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image16.png)

**Figura 6**: Aggiungere una nuova origine dati per il `ProductsByCategory` DropDownList ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image18.png))


[![CCrea un nuovo ProductsByCategoryDataSource denominato di ObjectDataSource](master-detail-filtering-with-two-dropdownlists-cs/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image19.png)

**Figura 7**: Creare un nuovo oggetto ObjectDataSource denominato `ProductsByCategoryDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image21.png))


Poiché il `ProductsByCategory` DropDownList esigenze per visualizzare solo i prodotti appartenenti alla categoria selezionata, avere ObjectDataSource richiama il `GetProductsByCategoryID(categoryID)` metodo dal `ProductsBLL` oggetto.


[![Cimpostare come usare la classe ProductsBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image22.png)

**Figura 8**: Scegliere di usare la `ProductsBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image24.png))


[![Cconfigurare ObjectDataSource per utilizzare il metodo GetProductsByCategoryID(categoryID)](master-detail-filtering-with-two-dropdownlists-cs/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image25.png)

**Figura 9**: Configurare ObjectDataSource per usare la `GetProductsByCategoryID(categoryID)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image27.png))


Nel passaggio finale della procedura guidata è necessario specificare il valore della *`categoryID`* parametro. Assegnare questo parametro per l'elemento selezionato dal `Categories` DropDownList.


[![Pil valore del parametro da DropDownList categorie categoryID ull](master-detail-filtering-with-two-dropdownlists-cs/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image28.png)

**Figura 10**: Eseguire il pull il *`categoryID`* valore del parametro dal `Categories` DropDownList ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image30.png))


Con ObjectDataSource configurato, resta che per specificare quali campi dell'origine dati vengono usati per la visualizzazione e il valore degli elementi del controllo DropDownList. Visualizzare il `ProductName` campo e usare il `ProductID` campo come valore.


[![Sspecificare i campi di origine dati utilizzata per il controllo DropDownList degli ListItems proprietà Text e Value](master-detail-filtering-with-two-dropdownlists-cs/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image31.png)

**Figura 11**: Specificare i campi di origine dati utilizzata per il controllo DropDownList `ListItem` s' `Text` e `Value` delle proprietà ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image33.png))


Con ObjectDataSource e `ProductsByCategory` DropDownList configurare nostra pagina verranno visualizzati due controlli DropDownList: il primo elencherà tutte le categorie mentre il secondo elenca i prodotti appartenenti alla categoria selezionata. Quando l'utente seleziona una nuova categoria dal primo DropDownList, negativi a un postback e verrà essere riassociati DropDownList secondo, che mostra i prodotti appartenenti alla categoria appena selezionata. Figure 12 e 13 mostra `MasterDetailsDetails.aspx` in azione quando viene visualizzato tramite un browser.


[![Wuando prima visitando la pagina, la categoria di bevande è selezionata](master-detail-filtering-with-two-dropdownlists-cs/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image34.png)

**Figura 12**: Durante la prima visita la pagina, la categoria di bevande è selezionata ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image36.png))


[![Choosing una categoria diversa consente di visualizzare di prodotti le nuove categorie](master-detail-filtering-with-two-dropdownlists-cs/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image37.png)

**Figura 13**: Scelta di un diverso Visualizza categoria di prodotti le nuove categorie ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image39.png))


Attualmente le `productsByCategory` DropDownList, quando è cambiato, viene *non* possono causare un postback. Tuttavia, è consigliabile un postback si verificano solo quando si aggiunge un controllo DetailsView per visualizzare i dettagli del prodotto selezionato (passaggio 3). Di conseguenza, verificare la casella di controllo Abilita AutoPostBack il `productsByCategory` tag intelligente del controllo DropDownList.


[![EAbilita la funzionalità di un postback automatico per il controllo DropDownList productsByCategory](master-detail-filtering-with-two-dropdownlists-cs/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image40.png)

**Figura 14**: Abilitare la funzionalità di un postback automatico per il `productsByCategory` DropDownList ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Passaggio 3: Uso di un controllo DetailsView per visualizzare i dettagli per il prodotto selezionato

Il passaggio finale consiste per visualizzare i dettagli per il prodotto selezionato in un controllo DetailsView. Per eseguire questa operazione, aggiungere un controllo DetailsView alla pagina, impostare relativi `ID` proprietà `ProductDetails`e creare un nuovo oggetto ObjectDataSource per essa. Configurare ObjectDataSource per estrarre i dati dal `ProductsBLL` della classe `GetProductByProductID(productID)` metodo tramite valore selezionato del `ProductsByCategory` DropDownList per il valore della *`productID`* parametro.


[![Cimpostare come usare la classe ProductsBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image43.png)

**Figura 15**: Scegliere di usare la `ProductsBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image45.png))


[![Cconfigurare ObjectDataSource per utilizzare il metodo GetProductByProductID(productID)](master-detail-filtering-with-two-dropdownlists-cs/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image46.png)

**Figura 16**: Configurare ObjectDataSource per usare la `GetProductByProductID(productID)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image48.png))


[![Pil valore del parametro da ProductsByCategory DropDownList productID ull](master-detail-filtering-with-two-dropdownlists-cs/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image49.png)

**Figura 17**: Eseguire il pull il *`productID`* valore del parametro dal `ProductsByCategory` DropDownList ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image51.png))


È possibile scegliere di visualizzare i campi disponibili in DetailsView. Ho scelto di rimuovere il `ProductID`, `SupplierID`, e `CategoryID` campi e riordinare e formattati i campi rimanenti. Inoltre, sono deselezionate out DetailsView `Height` e `Width` proprietà, che consente il controllo DetailsView espandere la larghezza necessaria più adatti alla visualizzazione dei dati anziché è vincolato a una dimensione specificata. Il markup completo viene visualizzato di seguito:


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample1.aspx)]

Si consiglia di provare il `MasterDetailsDetails.aspx` pagina in un browser. A prima vista potrebbe sembrare che funzionino nel modo desiderato, ma si verifica un problema complesso. Quando si sceglie una nuova categoria la `ProductsByCategory` DropDownList viene aggiornato per includere tali prodotti per la categoria selezionata, ma il `ProductDetails` DetailsView ha continuato a visualizzare le informazioni sul prodotto precedenti. DetailsView viene aggiornata quando si sceglie un prodotto diverso per la categoria selezionata. Inoltre, se esegue il test sufficientemente attentamente, si noterà che se si sceglie continuamente nuove categorie (ad esempio la scelta delle bibite dal `Categories` DropDownList, quindi condimenti, quindi dolciumi) ogni altra selezione della categoria fa sì che il `ProductDetails`DetailsView da aggiornare.

Per facilitare la concretizzare la questo problema, esaminiamo un esempio specifico. Durante la prima visita la pagina sia selezionata la categoria Beverages e i prodotti correlati vengono caricati nel `ProductsByCategory` DropDownList. Chai è il prodotto selezionato e i relativi dettagli vengono visualizzati nei `ProductDetails` DetailsView, come illustrato nella figura 18.


[![TDettagli he selezionate del prodotto vengono visualizzati in un controllo DetailsView](master-detail-filtering-with-two-dropdownlists-cs/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image52.png)

**Figura 18**: I dettagli del prodotto selezionato vengono visualizzati in un controllo DetailsView ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image54.png))


Se si modifica la selezione delle categorie da bibite a condimenti, si verifica un postback e `ProductsByCategory` DropDownList viene aggiornato di conseguenza, ma DetailsView vengono ancora visualizzati i dettagli per Chai compare.


[![The dettagli del prodotto selezionata precedentemente vengono ancora visualizzate](master-detail-filtering-with-two-dropdownlists-cs/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image55.png)

**Figura 19**: Dettagli precedentemente selezionata del prodotto sono ancora visualizzati ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image57.png))


Selezione di un nuovo prodotto nell'elenco viene aggiornato DetailsView come previsto. Se si seleziona una categoria di nuovo dopo aver modificato il prodotto, non aggiorna il DetailsView nuovamente. Tuttavia, se invece un nuovo prodotto è stata selezionata una nuova categoria, è necessario riavviare DetailsView. Che cosa nel mondo succede?

Si tratta di un problema di temporizzazione nel ciclo di vita della pagina. Ogni volta che viene richiesta una pagina che procede attraverso una serie di passaggi come il rendering. In uno di questi passaggi ObjectDataSource controlla verificare se uno qualsiasi dei relativi `SelectParameters` valori sono stati modificati. Se, pertanto, il controllo Web per dati associato a ObjectDataSource riconosce che sono necessarie per aggiornare la visualizzazione. Ad esempio, quando una nuova categoria viene selezionata, il `ProductsByCategoryDataSource` ObjectDataSource rileva che sono stati modificati i valori del parametro e il `ProductsByCategory` DropDownList rebinds stesso, il recupero di prodotti per la categoria selezionata.

Il problema che si verifica in questa situazione è che si verifica il punto di vita che consentono di controllare gli ObjectDataSource per i parametri modificati *prima di* la riassociazione dei dati associati controlli Web. Pertanto, quando si seleziona una nuova categoria di `ProductsByCategoryDataSource` ObjectDataSource rileva una modifica nel valore del relativo parametro. Utilizzato da ObjectDataSource i `ProductDetails` DetailsView, tuttavia, non notare eventuali modifiche perché il `ProductsByCategory` DropDownList deve ancora essere riassociati. Più avanti nel ciclo di vita di `ProductsByCategory` DropDownList riassocia di ObjectDataSource, acquisisce i prodotti per la categoria appena selezionato. Mentre il `ProductsByCategory` valore di DropDownList è cambiato, la `ProductDetails` DetailsView ObjectDataSource ha già eseguito il controllo di valori di parametro; pertanto, DetailsView visualizza i risultati precedenti. Questa interazione è illustrata nella figura 20.


[![TObjectDataSource verifica he ProductsByCategory DropDownList valore modifiche dopo il dettagli prodotto di DetailsView la presenza di modifiche](master-detail-filtering-with-two-dropdownlists-cs/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image58.png)

**Figura 20**: Il `ProductsByCategory` DropDownList valore modifiche dopo il `ProductDetails` ObjectDataSource verifica DetailsView la presenza di modifiche ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image60.png))


Per risolvere il problema, dobbiamo associare di nuovo in modo esplicito il `ProductDetails` DetailsView dopo il `ProductsByCategory` DropDownList è stato associato. È possibile eseguire questa operazione chiamando il `ProductDetails` DetailsView `DataBind()` metodo quando la `ProductsByCategory` del controllo DropDownList `DataBound` viene generato l'evento. Aggiungere il codice del gestore eventi seguente per il `MasterDetailsDetails.aspx` classe code-behind della pagina (vedere il "[a livello di codice impostando i valori dei parametri di ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)" per una discussione su come aggiungere un gestore eventi):


[!code-csharp[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample2.cs)]

Dopo questa chiamata esplicita per il `ProductDetails` DetailsView `DataBind()` metodo è stato aggiunto, l'esercitazione funziona come previsto. Figura 21 evidenzia come questo modificato risolto la causa del problema precedente.


[![TProductDetails DetailsView è viene generato l'evento associato a dati in modo esplicito aggiornati quando il ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image61.png)

**Figura 21**: Il `ProductDetails` DetailsView è esplicitamente aggiornati quando il `ProductsByCategory` del controllo DropDownList `DataBound` viene generato l'evento ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-two-dropdownlists-cs/_static/image63.png))


## <a name="summary"></a>Riepilogo

Il controllo DropDownList costituisce un elemento dell'interfaccia utente ideale per i report master o di dettaglio in cui è presente una relazione uno-a-molti tra i record master e di dettaglio. Nell'esercitazione precedente abbiamo visto come utilizzare un controllo DropDownList singolo per filtrare i prodotti visualizzati in base alla categoria selezionata. In questa esercitazione viene sostituito il controllo GridView di prodotti con un controllo DropDownList e usato un controllo DetailsView per visualizzare i dettagli del prodotto selezionato. I concetti illustrati in questa esercitazione possono essere facilmente esteso ai modelli di dati che coinvolgono più relazioni uno-a-molti, ad esempio i clienti, ordini e gli elementi di ordine. In generale, è sempre possibile aggiungere un controllo DropDownList per ognuna delle entità "uno" nelle relazioni uno-a-molti.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Hilton Giesenow, revisore per questa esercitazione. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-detail-filtering-with-a-dropdownlist-cs.md)
> [Successivo](master-detail-filtering-across-two-pages-cs.md)
