---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
title: Filtro master/dettagli in due pagine (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà implementato questo modello utilizzando un controllo GridView per elencare i fornitori nel database. Ogni riga Supplier in GridView conterrà un...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 361d6a44-3f1f-4daf-85df-d4c2b8bf065d
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 0e30d47a565c3b6cb9f647d54d47c10a418762f4
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2020
ms.locfileid: "89044974"
---
# <a name="masterdetail-filtering-across-two-pages-vb"></a>Applicazione di filtri a report master o di dettaglio su due pagine (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_9_VB.exe) o [scaricare il file PDF](master-detail-filtering-across-two-pages-vb/_static/datatutorial09vb1.pdf)

> In questa esercitazione verrà implementato questo modello utilizzando un controllo GridView per elencare i fornitori nel database. Ogni riga Supplier in GridView conterrà un collegamento Visualizza prodotti che, quando selezionato, consente di portare l'utente in una pagina separata in cui sono elencati i prodotti per il fornitore selezionato.

## <a name="introduction"></a>Introduzione

Nelle due esercitazioni precedenti è stato illustrato come visualizzare i report master/dettaglio in una singola pagina Web utilizzando i controlli DropDownList per visualizzare i record "Master" e [GridView](master-detail-filtering-with-a-dropdownlist-vb.md) o [DetailsView](master-detail-filtering-with-two-dropdownlists-vb.md) per visualizzare i "dettagli". Un altro modello comune usato per i report master/dettagli consiste nel disporre dei record master in una pagina Web e dei dettagli visualizzati in un altro. Un sito Web del forum, come [i forum ASP.NET](https://forums.asp.net/), è un ottimo esempio di questo modello in pratica. I forum ASP.NET sono costituiti da un'ampia gamma di forum Introduzione, Web Form, controlli per la presentazione dei dati e così via. Ogni forum è costituito da molti thread e ogni thread è costituito da un numero di post. Nella Home page dei forum di ASP.NET sono elencati i forum. Quando si fa clic su un forum si fa clic su una `ShowForum.aspx` pagina, in cui sono elencati i thread del forum. Analogamente, se si fa clic su un thread `ShowPost.aspx` , viene visualizzato il post per il thread su cui è stato fatto clic.

In questa esercitazione verrà implementato questo modello utilizzando un controllo GridView per elencare i fornitori nel database. Ogni riga Supplier in GridView conterrà un collegamento Visualizza prodotti che, quando selezionato, consente di portare l'utente in una pagina separata in cui sono elencati i prodotti per il fornitore selezionato.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Passaggio 1: aggiunta `SupplierListMaster.aspx` `ProductsForSupplierDetails.aspx` di pagine e alla `Filtering` cartella

Quando si definisce il layout di pagina nella terza esercitazione, sono state aggiunte alcune pagine "Starter" nelle `BasicReporting` `Filtering` cartelle, e `CustomFormatting` . Tuttavia, non è stata aggiunta alcuna pagina iniziale per questa esercitazione, quindi è necessario aggiungere due nuove pagine alla `Filtering` cartella: `SupplierListMaster.aspx` e `ProductsForSupplierDetails.aspx` . `SupplierListMaster.aspx` elenca i record "Master" (fornitori), mentre visualizzerà `ProductsForSupplierDetails.aspx` i prodotti per il fornitore selezionato.

Quando si creano queste due nuove pagine, è necessario associarle alla `Site.master` pagina master.

![Aggiungere le pagine SupplierListMaster. aspx e ProductsForSupplierDetails. aspx alla cartella Filtering](master-detail-filtering-across-two-pages-vb/_static/image1.png)

**Figura 1**: aggiungere le `SupplierListMaster.aspx` `ProductsForSupplierDetails.aspx` pagine e alla `Filtering` cartella

Inoltre, quando si aggiungono nuove pagine al progetto, assicurarsi di aggiornare il file della mappa del sito, `Web.sitemap` , di conseguenza. Per questa esercitazione è sufficiente aggiungere la `SupplierListMaster.aspx` pagina alla mappa del sito usando il contenuto XML seguente come figlio dell'elemento Filtering Reports `<siteMapNode>` :

[!code-xml[Main](master-detail-filtering-across-two-pages-vb/samples/sample1.xml)]

> [!NOTE]
> È possibile automatizzare il processo di aggiornamento del file della mappa del sito quando si aggiungono nuove pagine ASP.NET con la macro della mappa del [sito](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx)Visual Studio gratuita di [Scott Allen](http://odetocode.com/Blogs/scott/).

## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Passaggio 2: visualizzazione dell'elenco Supplier in`SupplierListMaster.aspx`

Con le `SupplierListMaster.aspx` `ProductsForSupplierDetails.aspx` pagine e create, il passaggio successivo consiste nel creare il GridView dei fornitori in `SupplierListMaster.aspx` . Aggiungere un controllo GridView alla pagina e associarlo a un nuovo ObjectDataSource. Questo ObjectDataSource deve usare il `SuppliersBLL` metodo della classe `GetSuppliers()` per restituire tutti i fornitori.

[![Selezionare la classe SuppliersBLL](master-detail-filtering-across-two-pages-vb/_static/image3.png)](master-detail-filtering-across-two-pages-vb/_static/image2.png)

**Figura 2**: selezionare la `SuppliersBLL` classe ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image4.png))

[![Configurare ObjectDataSource per l'utilizzo del metodo getsuppliers ()](master-detail-filtering-across-two-pages-vb/_static/image6.png)](master-detail-filtering-across-two-pages-vb/_static/image5.png)

**Figura 3**: configurare ObjectDataSource per l'uso del `GetSuppliers()` Metodo ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image7.png))

È necessario includere un collegamento denominato Visualizza prodotti in ogni riga GridView che, quando si fa clic, consente all'utente di `ProductsForSupplierDetails.aspx` passare il valore della riga selezionata `SupplierID` attraverso la QueryString. Se, ad esempio, l'utente fa clic sul collegamento Visualizza prodotti per il fornitore di Tokyo Traders (che ha un `SupplierID` valore pari a 4), dovrà essere inviato a `ProductsForSupplierDetails.aspx?SupplierID=4` .

A tale scopo, aggiungere un [HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) a GridView, che aggiunge un collegamento ipertestuale a ogni riga di GridView. Per iniziare, fare clic sul collegamento Modifica colonne dallo smart tag di GridView. Selezionare quindi HyperLinkField nell'elenco in alto a sinistra e fare clic su Aggiungi per includere il HyperLinkField nell'elenco dei campi di GridView.

[![Aggiungere un HyperLinkField a GridView](master-detail-filtering-across-two-pages-vb/_static/image9.png)](master-detail-filtering-across-two-pages-vb/_static/image8.png)

**Figura 4**: aggiungere un HyperLinkField al controllo GridView ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image10.png))

HyperLinkField può essere configurato in modo da usare lo stesso valore di testo o URL per il collegamento in ogni riga GridView oppure può basare tali valori sui valori dei dati associati a ogni riga specifica. Per specificare un valore statico in tutte le righe, usare le `Text` proprietà o di HyperLinkField `NavigateUrl` . Poiché si vuole che il testo del collegamento sia lo stesso per tutte le righe, impostare la `Text` Proprietà HyperLinkField per visualizzare i prodotti.

[![Impostare la proprietà Text di HyperLinkField per visualizzare i prodotti](master-detail-filtering-across-two-pages-vb/_static/image12.png)](master-detail-filtering-across-two-pages-vb/_static/image11.png)

**Figura 5**: impostare la `Text` proprietà di HyperLinkField per visualizzare i prodotti ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image13.png))

Per impostare i valori di testo o URL in modo che siano basati sui dati sottostanti associati alla riga GridView, specificare i campi dati da cui devono essere estratti i valori di testo o URL nelle `DataTextField` `DataNavigateUrlFields` proprietà o. `DataTextField` può essere impostato solo su un singolo campo di dati; `DataNavigateUrlFields`, tuttavia, può essere impostato su un elenco di campi dati delimitati da virgole. Spesso è necessario basare il testo o l'URL su una combinazione del valore del campo dati della riga corrente e di un markup statico. In questa esercitazione, ad esempio, si vuole che l'URL dei collegamenti di HyperLinkField sia `ProductsForSupplierDetails.aspx?SupplierID=supplierID` , dove *`supplierID`* è il valore row's di ogni GridView `SupplierID` . Si noti che in questo caso sono necessari sia i valori statici che quelli guidati dai dati: la `ProductsForSupplierDetails.aspx?SupplierID=` parte dell'URL del collegamento è statica, mentre la *`supplierID`* parte è basata sui dati perché il relativo valore è il valore di ogni riga `SupplierID` .

Per indicare una combinazione di valori statici e basati sui dati, utilizzare le `DataTextFormatString` `DataNavigateUrlFormatString` proprietà e. In queste proprietà immettere il markup statico in base alle esigenze e quindi usare il marcatore in `{0}` cui si vuole che venga visualizzato il valore del campo specificato nelle `DataTextField` `DataNavigateUrlFields` proprietà o. Se `DataNavigateUrlFields` per la proprietà sono stati specificati più campi `{0}` , utilizzare la posizione in cui si desidera inserire il valore del primo campo, `{1}` per il secondo valore del campo e così via.

Applicando questo oggetto all'esercitazione, è necessario impostare la `DataNavigateUrlFields` proprietà su `SupplierID` , dal momento che si tratta del campo dati il cui valore è necessario personalizzare in base alle singole righe e della `DataNavigateUrlFormatString` proprietà su `ProductsForSupplierDetails.aspx?SupplierID={0}` .

[![Configurare HyperLinkField in modo da includere l'URL del collegamento appropriato in base al SupplierID](master-detail-filtering-across-two-pages-vb/_static/image15.png)](master-detail-filtering-across-two-pages-vb/_static/image14.png)

**Figura 6**: configurare HyperLinkField per includere l'URL del collegamento appropriato in base al `SupplierID` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image16.png))

Dopo aver aggiunto il HyperLinkField, è possibile personalizzare e riordinare i campi di GridView. Il markup seguente mostra GridView dopo aver apportato alcune personalizzazioni a livello di campo secondarie.

[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample2.aspx)]

È possibile visualizzare la `SupplierListMaster.aspx` pagina tramite un browser. Come illustrato nella figura 7, nella pagina sono attualmente elencati tutti i fornitori, incluso un collegamento Visualizza prodotti. Facendo clic sul collegamento Visualizza prodotti verrà visualizzata la pagina relativa al `ProductsForSupplierDetails.aspx` passaggio del fornitore `SupplierID` nella QueryString.

[![Ogni riga Supplier contiene un collegamento Visualizza prodotti](master-detail-filtering-across-two-pages-vb/_static/image18.png)](master-detail-filtering-across-two-pages-vb/_static/image17.png)

**Figura 7**: ogni riga Supplier contiene un collegamento View Products ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image19.png))

## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Passaggio 3: elenco dei prodotti del fornitore in`ProductsForSupplierDetails.aspx`

A questo punto la `SupplierListMaster.aspx` pagina Invia gli utenti a `ProductsForSupplierDetails.aspx` , passando il fornitore selezionato `SupplierID` nella QueryString. Il passaggio finale dell'esercitazione consiste nel visualizzare i prodotti in un GridView in la cui proprietà è `ProductsForSupplierDetails.aspx` `SupplierID` uguale a `SupplierID` passata attraverso la QueryString. A tale scopo, aggiungere un controllo GridView alla `ProductsForSupplierDetails.aspx` pagina usando un nuovo controllo ObjectDataSource denominato `ProductsBySupplierDataSource` che richiama il `GetProductsBySupplierID(supplierID)` Metodo dalla `ProductsBLL` classe.

[![Aggiungere un nuovo ObjectDataSource denominato ProductsBySupplierDataSource](master-detail-filtering-across-two-pages-vb/_static/image21.png)](master-detail-filtering-across-two-pages-vb/_static/image20.png)

**Figura 8**: aggiungere un nuovo ObjectDataSource denominato `ProductsBySupplierDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image22.png))

[![Selezionare la classe ProductsBLL](master-detail-filtering-across-two-pages-vb/_static/image24.png)](master-detail-filtering-across-two-pages-vb/_static/image23.png)

**Figura 9**: selezionare la `ProductsBLL` classe ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image25.png))

[![Fare in modo che ObjectDataSource chiami il metodo GetProductsBySupplierID (supplierID)](master-detail-filtering-across-two-pages-vb/_static/image27.png)](master-detail-filtering-across-two-pages-vb/_static/image26.png)

**Figura 10**: fare in modo che ObjectDataSource richiami il `GetProductsBySupplierID(supplierID)` Metodo ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image28.png))

Il passaggio finale della procedura guidata Configura origine dati richiede di specificare l'origine del parametro del `GetProductsBySupplierID(supplierID)` metodo *`supplierID`* . Per usare il valore QueryString, impostare l'origine del parametro su QueryString e immettere il nome del valore QueryString da usare nella casella di testo QueryStringField ( `SupplierID` ).

[![Popolare il valore del parametro supplierID dal valore QueryString SupplierID](master-detail-filtering-across-two-pages-vb/_static/image30.png)](master-detail-filtering-across-two-pages-vb/_static/image29.png)

**Figura 11**: popolare il *`supplierID`* valore del parametro dal `SupplierID` valore QueryString ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image31.png))

Questo è tutto ciò che occorre fare. La figura 12 Mostra la `ProductsForSupplierDetails.aspx` pagina quando viene visitata facendo clic sul collegamento Tokyo Traders da `SupplierListMaster.aspx` .

[![Vengono visualizzati i prodotti forniti da Tokyo Traders](master-detail-filtering-across-two-pages-vb/_static/image33.png)](master-detail-filtering-across-two-pages-vb/_static/image32.png)

**Figura 12**: vengono visualizzati i prodotti forniti da Tokyo Traders ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image34.png))

## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Visualizzazione delle informazioni sul fornitore in`ProductsForSupplierDetails.aspx`

Come illustrato nella figura 12, nella `ProductsForSupplierDetails.aspx` pagina sono elencati semplicemente i prodotti forniti dall' `SupplierID` oggetto specificato nella QueryString. Un utente inviato direttamente a questa pagina, tuttavia, non è in grado di sapere che la figura 12 Mostra i prodotti di Tokyo Traders. Per risolvere questo problema, è possibile visualizzare anche le informazioni sui fornitori in questa pagina.

Per iniziare, aggiungere un controllo FormView sopra i prodotti GridView. Creare un nuovo controllo ObjectDataSource denominato `SuppliersDataSource` che richiama il `SuppliersBLL` metodo della classe `GetSupplierBySupplierID(supplierID)` .

[![Selezionare la classe SuppliersBLL](master-detail-filtering-across-two-pages-vb/_static/image36.png)](master-detail-filtering-across-two-pages-vb/_static/image35.png)

**Figura 13**: selezionare la `SuppliersBLL` classe ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image37.png))

[![Fare in modo che ObjectDataSource chiami il metodo GetSupplierBySupplierID (supplierID)](master-detail-filtering-across-two-pages-vb/_static/image39.png)](master-detail-filtering-across-two-pages-vb/_static/image38.png)

**Figura 14**: fare in modo che ObjectDataSource richiami il `GetSupplierBySupplierID(supplierID)` Metodo ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image40.png))

Come con, fare in modo che `ProductsBySupplierDataSource` al *`supplierID`* parametro venga assegnato il valore del `SupplierID` valore QueryString.

[![Popolare il valore del parametro supplierID dal valore QueryString SupplierID](master-detail-filtering-across-two-pages-vb/_static/image42.png)](master-detail-filtering-across-two-pages-vb/_static/image41.png)

**Figura 15**: popolare il *`supplierID`* valore del parametro dal `SupplierID` valore QueryString ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image43.png))

Quando si associa FormView a ObjectDataSource nella visualizzazione progettazione, Visual Studio creerà automaticamente i controlli di FormView `ItemTemplate` , `InsertItemTemplate` e `EditItemTemplate` con i controlli Web Label e TextBox per ogni campo dati restituito da ObjectDataSource. Poiché è sufficiente visualizzare le informazioni sui fornitori, è possibile rimuovere `InsertItemTemplate` e `EditItemTemplate` . Successivamente, modificare ItemTemplate in modo che visualizzi il nome della società del fornitore in un `<h3>` elemento e l'indirizzo, la città, il paese e il numero di telefono sotto il nome della società. In alternativa, è possibile impostare manualmente il FormView `DataSourceID` e creare il `ItemTemplate` markup, come è stato riportato nell'esercitazione "[visualizzazione dei dati con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)".

Una volta modificate, il markup dichiarativo di FormView dovrebbe avere un aspetto simile al seguente:

[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample3.aspx)]

Nella figura 16 viene mostrata una schermata della `ProductsForSupplierDetails.aspx` pagina dopo che sono state incluse le informazioni sul fornitore descritte in precedenza.

[![L'elenco dei prodotti include un riepilogo sul fornitore](master-detail-filtering-across-two-pages-vb/_static/image45.png)](master-detail-filtering-across-two-pages-vb/_static/image44.png)

**Figura 16**: l'elenco dei prodotti include un riepilogo sul fornitore ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image46.png))

## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Applicazione dei tocchi finali per l' `ProductsForSupplierDetails.aspx` interfaccia utente

Per migliorare l'esperienza utente per questo report, è necessario apportare alcune aggiunte alla `ProductsForSupplierDetails.aspx` pagina. Attualmente, l'unico modo in cui un utente può tornare dalla `ProductsForSupplierDetails.aspx` pagina all'elenco dei fornitori è fare clic sul pulsante indietro del browser. Si aggiungerà un controllo collegamento ipertestuale alla pagina a cui si collega il collegamento `ProductsForSupplierDetails.aspx` , in modo da `SupplierListMaster.aspx` consentire all'utente di tornare all'elenco principale.

[![Aggiungere un controllo collegamento ipertestuale per riportare l'utente a SupplierListMaster. aspx](master-detail-filtering-across-two-pages-vb/_static/image48.png)](master-detail-filtering-across-two-pages-vb/_static/image47.png)

**Figura 17**: aggiungere un controllo collegamento ipertestuale per riportare l'utente a `SupplierListMaster.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image49.png))

Se l'utente fa clic sul collegamento Visualizza prodotti per un fornitore che non dispone di alcun prodotto, `ProductsBySupplierDataSource` ObjectDataSource in `ProductsForSupplierDetails.aspx` non restituirà alcun risultato. Il GridView associato a ObjectDataSource non eseguirà il rendering di alcun markup risultante in un'area vuota della pagina nel browser dell'utente. Per comunicare più chiaramente all'utente che non sono presenti prodotti associati al fornitore selezionato, è possibile impostare la `EmptyDataText` proprietà di GridView sul messaggio che si desidera visualizzare quando si verifica una situazione di questo tipo. Questa proprietà è stata impostata su "non sono presenti prodotti forniti da questo fornitore".

Per impostazione predefinita, tutti i fornitori nel database Northwind forniscono almeno un prodotto. Tuttavia, per questa esercitazione è stata modificata manualmente la `Products` tabella, in modo che il fornitore escargos nouveaux non sia più associato ad alcun prodotto. Nella figura 18 viene illustrata la pagina dei dettagli per le esNouveauxe dopo che questa modifica è stata apportata.

[![Gli utenti sono informati che il fornitore non fornisce alcun prodotto](master-detail-filtering-across-two-pages-vb/_static/image51.png)](master-detail-filtering-across-two-pages-vb/_static/image50.png)

**Figura 18**: gli utenti sono informati che il fornitore non fornisce alcun prodotto ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image52.png))

## <a name="summary"></a>Riepilogo

Mentre i report master/dettagli possono visualizzare sia i record master che i record di dettaglio in una singola pagina, in molti siti Web vengono separati tra due pagine Web. In questa esercitazione è stato illustrato come implementare un report master/dettagli con i fornitori elencati in GridView nella pagina Web "Master" e i prodotti associati elencati nella pagina "dettagli". Ogni riga Supplier nella pagina Web master contiene un collegamento alla pagina dei dettagli passata lungo il valore della riga `SupplierID` . Tali collegamenti specifici per la riga possono essere facilmente aggiunti usando il HyperLinkField di GridView.

Nella pagina dei dettagli il recupero dei prodotti per il fornitore specificato è stato eseguito richiamando il `ProductsBLL` metodo della classe `GetProductsBySupplierID(supplierID)` . Il *`supplierID`* valore del parametro è stato specificato in modo dichiarativo utilizzando QueryString come origine del parametro. È stato inoltre illustrato come visualizzare i dettagli del fornitore nella pagina dei dettagli utilizzando un FormView.

L' [esercitazione successiva](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) è quella finale nei report master/dettagli. Si esaminerà come visualizzare un elenco di prodotti in un controllo GridView in cui ogni riga dispone di un pulsante Seleziona. Facendo clic sul pulsante Seleziona, i dettagli del prodotto vengono visualizzati in un controllo DetailsView nella stessa pagina.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). E può essere raggiunto all'indirizzo [ mitchell@4GuysFromRolla.com .](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile all'indirizzo [http://ScottOnWriting.NET](http://ScottOnWriting.NET) .

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione è stato Hilton Giesenow. Sei interessato a esaminare i miei prossimi articoli MSDN? In caso affermativo, rilasciare una riga in [ mitchell@4GuysFromRolla.com .](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-detail-filtering-with-two-dropdownlists-vb.md) 
>  [Avanti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md)
