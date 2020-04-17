---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Controlli con associazione a dati Documenti Microsoft
author: rick-anderson
description: La maggior parte delle applicazioni ASP.NET si basa su un certo grado di presentazione dei dati da un'origine dati back-end. I controlli con associazione a dati sono stati una parte fondamentale dell'interazione con w...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: 941e2ed15b3da28991e7b06cbab570eb1b5b8899
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543080"
---
# <a name="data-bound-controls"></a>Controlli associati a dati

da parte [di Microsoft](https://github.com/microsoft)

> La maggior parte delle applicazioni ASP.NET si basa su un certo grado di presentazione dei dati da un'origine dati back-end. I controlli con associazione a dati sono stati una parte fondamentale dell'interazione con i dati nelle applicazioni Web dinamiche. ASP.NET 2.0 introduce alcuni miglioramenti sostanziali ai controlli con associazione a dati, tra cui una nuova classe BaseDataBoundControl e una sintassi dichiarativa.

La maggior parte delle applicazioni ASP.NET si basa su un certo grado di presentazione dei dati da un'origine dati back-end. I controlli con associazione a dati sono stati una parte fondamentale dell'interazione con i dati nelle applicazioni Web dinamiche. ASP.NET 2.0 introduce alcuni miglioramenti sostanziali ai controlli con associazione a dati, tra cui una nuova classe BaseDataBoundControl e una sintassi dichiarativa.

Il BaseDataBoundControl funge da classe di base per il DataBoundControl classe e il HierarchicalDataBoundControl classe. In this module, we will discuss the following classes that derive from DataBoundControl:

- AdRotator
- Controlli elenco
- GridView
- Formview
- Detailsview

Verranno inoltre illustrate le classi seguenti che derivano dalla classe HierarchicalDataBoundControl:We will also discuss the following classes that derive from HierarchicalDataBoundControl class:

- TreeView
- Menu
- Sitemappath

## <a name="databoundcontrol-class"></a>Classe DataBoundControl

La classe DataBoundControl è una classe astratta (contrassegnata MustInherit in VB) utilizzata per interagire con dati tabulari o di tipo elenco. I controlli seguenti sono alcuni dei controlli che derivano da DataBoundControl.

## <a name="adrotator"></a>AdRotator

Il controllo AdRotator consente di visualizzare un banner grafico in una pagina Web collegata a un URL specifico. L'elemento grafico visualizzato viene ruotato utilizzando le proprietà del controllo. La frequenza di visualizzazione di un determinato annuncio in una pagina può essere configurata utilizzando la proprietà **Impressions** e gli annunci possono essere filtrati utilizzando il filtro basato su parole chiave.

I controlli AdRotator usano un file XML o una tabella in un database per i dati. Gli attributi seguenti vengono utilizzati nei file XML per configurare il controllo AdRotator.

### <a name="imageurl"></a>ImageUrl
L'URL di un'immagine da visualizzare per l'annuncio.

### <a name="navigateurl"></a>Navigateurl
L'URL a cui deve essere indirizzato l'utente quando si fa clic sull'annuncio. Questo dovrebbe essere codificato IN URL.

### <a name="alternatetext"></a>Alternatetext
Testo alternativo visualizzato in una descrizione comando e letto dalle utilità per la lettura dello schermo. Viene inoltre visualizzato quando l'immagine specificata da ImageUrl non è disponibile.

### <a name="keyword"></a>Parola chiave
Definisce una parola chiave che può essere utilizzata quando si utilizza il filtro per parole chiave. Se specificato, verranno visualizzati solo gli annunci con una parola chiave corrispondente al filtro per parole chiave.

### <a name="impressions"></a>Impressioni
Un numero di ponderazione che determina la frequenza con cui è probabile che un determinato annuncio venga visualizzato. È relativo all'impressione di altri annunci nello stesso file. Il valore massimo delle impressioni collettive per tutti gli annunci in un file XML è 2.048.000.000 1.

### <a name="height"></a>Altezza:
Altezza dell'annuncio in pixel.

### <a name="width"></a>Larghezza
Larghezza dell'annuncio in pixel.

> [!NOTE]
> Gli attributi Height e Width sostituiscono l'altezza e la larghezza del controllo AdRotator stesso.

Un tipico file XML potrebbe essere simile al seguente:A typical XML file might look like the following:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

Nell'esempio precedente, l'annuncio per Contoso ha il doppio delle probabilità di essere visualizzato come annuncio per il sito Web ASP.NET a causa del valore dell'attributo Impressions.

Per visualizzare gli annunci dal file XML precedente, aggiungere un controllo AdRotator a una pagina e impostare la proprietà **AdvertisementFile** in modo che punti al file XML come illustrato di seguito:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Se si sceglie di utilizzare una tabella di database come origine dati per il controllo AdRotator, sarà innanzitutto necessario impostare un database utilizzando lo schema seguente:

| **Nome colonna** | **Tipo di dati** | **Descrizione** |
| --- | --- | --- |
| ID | INT | Chiave primaria. Questa colonna può avere qualsiasi nome. |
| ImageUrl | nvarchar(*lunghezza*) | URL relativo o assoluto dell'immagine da visualizzare per l'annuncio. |
| Navigateurl | nvarchar(*lunghezza*) | URL di destinazione per l'annuncio. Se non fornisci un valore, l'annuncio non è un collegamento ipertestuale. |
| Alternatetext | nvarchar(*lunghezza*) | Testo visualizzato se non è possibile trovare l'immagine. In alcuni browser, il testo viene visualizzato come descrizione comando. Il testo alternativo viene utilizzato anche per l'accessibilità in modo che gli utenti che non possono vedere l'elemento grafico possano sentire la sua descrizione leggere ad alta voce. |
| Parola chiave | nvarchar(*lunghezza*) | Una categoria per l'annuncio in cui la pagina può filtrare. |
| Impressioni | int(4) | Un numero che indica la probabilità di visualizzazione dell'annuncio. Maggiore è il numero, più spesso viene visualizzato l'annuncio. Il totale di tutti i valori delle impressioni nel file XML non può superare 2.048.000.000 - 1. |
| Larghezza | int(4) | Larghezza dell'immagine in pixel. |
| Altezza: | int(4) | Altezza dell'immagine in pixel. |

Nei casi in cui si dispone già di un database con uno schema diverso, è possibile utilizzare le proprietà **AlternateTextField**, **ImageUrlField**e **NavigateUrlField** per eseguire il mapping degli attributi AdRotator al database esistente. Per visualizzare i dati dal database nel controllo AdRotator, aggiungere un controllo origine dati alla pagina, configurare la stringa di connessione per il controllo origine dati in modo che punti al database e impostare la proprietà **DataSourceID** del controllo AdRotator sull'ID del controllo origine dati. Nei casi in cui è necessario configurare gli annunci AdRotator a livello di codice, usa l'evento AdCreated. L'evento AdCreated accetta due parametri; uno un oggetto e l'altro un'istanza di AdCreatedEventArgs. Il AdCreatedEventArgs è un riferimento all'annuncio che viene creato.

Il frammento di codice seguente imposta ImageUrl, NavigateUrl e AlternateText per un annuncio a livello di codice:The following code snippet sets the ImageUrl, NavigateUrl, and AlternateText for an ad programmatically:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Controlli elenco

I controlli elenco includono ListBox, DropDownList, CheckBoxList, RadioButtonList e BulletedList. Ognuno di questi controlli può essere associato a dati a un'origine dati. Utilizzano un campo nell'origine dati come testo visualizzato e possono facoltativamente utilizzare un secondo campo come valore di un elemento. Gli elementi possono anche essere aggiunti staticamente in fase di progettazione ed è possibile combinare elementi statici ed elementi dinamici aggiunti da un'origine dati.

Per associare dati a un controllo elenco, aggiungere un controllo origine dati alla pagina. Specificare un comando SELECT per il controllo origine dati, quindi impostare la proprietà DataSourceID del controllo elenco sull'ID del controllo origine dati. Utilizzare il **DataTextField** e **DataValueField** proprietà per definire il testo visualizzato e il valore per il controllo. Inoltre, è possibile utilizzare il **DataTextFormatString** proprietà per controllare l'aspetto del testo visualizzato come segue:

| **Espressione** | **Descrizione** |
| --- | --- |
| Prezzo:{0:C} | Per i dati numerici/decimali. Visualizza il valore letterale "Prezzo:" seguito da numeri in formato valuta. Il formato della valuta dipende dalle impostazioni cultura specificate nell'attributo culture nella direttiva **Page** o nel file Web.config. |
| {0:D4} | Per i dati integer. Non può essere utilizzato con i numeri decimali. I numeri interi vengono visualizzati in un campo con spaziatura zero di quattro caratteri di larghezza. |
| {0:N2}% | Per i dati numerici. Visualizza il numero con precisione di 2 cifre decimali seguito dal valore letterale "%". |
| {0:000.0} | Per i dati numerici/decimali. I numeri vengono arrotondati a una cifra decimale. I numeri composti da meno di tre cifre sono riempiti con degli zero. |
| {0:D} | Per i dati di data/ora. Visualizza il formato data estesa ("giovedì, 06 agosto 1996"). Il formato della data dipende dalle impostazioni cultura della pagina o del file Web.config. |
| {0:d} | Per i dati di data/ora. Visualizza il formato di data breve ("31/12/99"). |
| {0:yy-MM-dd} | Per i dati di data/ora. Visualizza la data nel formato numerico anno-mese-giorno (96-08-06) |

## <a name="gridview"></a>GridView

Il controllo GridView consente la visualizzazione e la modifica di dati tabulari tramite un approccio dichiarativo ed è il successore del controllo DataGrid. Nel controllo GridView sono disponibili le funzionalità seguenti.

- Associazione a controlli origine dati, ad esempio SqlDataSource.Binding to data source controls, such as SqlDataSource.
- Funzionalità di ordinamento integrate.
- Funzionalità integrate di aggiornamento ed eliminazione.
- Funzionalità di paging incorporate.
- Funzionalità di selezione delle righe incorporate.
- Accesso a livello di codice al modello a oggetti GridView per impostare dinamicamente le proprietà, gestire gli eventi e così via.
- Più campi chiave.
- Più campi dati per le colonne collegamento ipertestuale.
- Aspetto personalizzabile attraverso temi e stili.

**Campi colonna**

Ogni colonna nel controllo GridView è rappresentata da un DataControlField oggetto. Per impostazione predefinita, la proprietà AutoGenerateColumns è impostata su **true**, che crea un oggetto AutoGeneratedField per ogni campo nell'origine dati. Viene quindi eseguito il rendering di ogni campo come colonna nel controllo GridView nell'ordine in cui ogni campo viene visualizzato nell'origine dati. È inoltre possibile controllare manualmente quali campi colonna vengono visualizzati nel controllo **GridView** impostando la proprietà **AutoGenerateColumns** su **false** e quindi definendo la propria raccolta di campi colonna. Tipi di campo colonna diversi determinano il comportamento delle colonne nel controllo.

Nella tabella seguente sono elencati i diversi tipi di campo colonna che è possibile utilizzare.

| **Tipo di campo colonna** | **Descrizione** |
| --- | --- |
| BoundField | Visualizza il valore di un campo in un'origine dati. Si tratta del tipo di colonna predefinito del controllo GridView. |
| ButtonField | Visualizza un pulsante di comando per ogni elemento nel controllo GridView. In questo modo è possibile creare una colonna di controlli pulsante personalizzati, ad esempio il Aggiungi o il pulsante Rimuovi. |
| CheckBoxField | Visualizza una casella di controllo per ogni elemento nel controllo GridView. Questo tipo di campo colonna viene comunemente utilizzato per visualizzare campi con un valore booleano. |
| CommandField | Visualizza i pulsanti di comando predefiniti per eseguire operazioni di selezione, modifica o eliminazione. |
| HyperLinkField | Visualizza il valore di un campo in un'origine dati come collegamento ipertestuale. Questo tipo di campo colonna consente di associare un secondo campo all'URL del collegamento ipertestuale. |
| ImageField | Visualizza un'immagine per ogni elemento nel controllo GridView. |
| Templatefield | Visualizza il contenuto definito dall'utente per ogni elemento nel controllo GridView in base a un modello specificato. Questo tipo di campo colonna consente di creare un campo colonna personalizzato. |

Per definire una raccolta di campi colonna in modo dichiarativo, aggiungere innanzitutto i tag ** &lt;Columns&gt; ** di apertura e chiusura tra i tag di apertura e chiusura del controllo GridView. Elencare quindi i campi colonna che si desidera includere tra i tag ** &lt;Columns&gt; ** di apertura e di chiusura. Le colonne specificate vengono aggiunte alla raccolta Columns nell'ordine indicato. Il Columns raccolta archivia tutti i campi colonna nel controllo e consente di gestire a livello di codice i campi colonna nel controllo GridView.The **Columns** collection stores all the column fields in the control and allows you to programmatically manage the column fields in the GridView control.

I campi colonna dichiarati in modo esplicito possono essere visualizzati in combinazione con i campi colonna generati automaticamente. Quando vengono utilizzati entrambi, i campi colonna dichiarati in modo esplicito vengono visualizzati per primi, seguiti dai campi colonna generati automaticamente.

## <a name="binding-to-data"></a>Associazione ai dati

Il controllo GridView può essere associato a un controllo origine dati (ad esempio **SqlDataSource**, **ObjectDataSource**e così via), nonché a qualsiasi origine dati che implementa l'interfaccia System.Collections.IEnumerable , ad esempio System.Data.DataView, System.Collections.ArrayList o System.Collections.Hashtable . Utilizzare uno dei metodi seguenti per associare il controllo GridView al tipo di origine dati appropriato:

- Per eseguire l'associazione a un controllo origine dati, impostare la proprietà DataSourceID del controllo GridView sul valore ID del controllo origine dati. Il controllo GridView viene associato automaticamente al controllo origine dati specificato e può sfruttare le funzionalità del controllo origine dati per eseguire funzionalità di ordinamento, aggiornamento, eliminazione e paging. Questo è il metodo preferito da associare ai dati.
- Per eseguire l'associazione a un'origine dati che implementa l'interfaccia System.Collections.IEnumerable , impostare a livello di codice la proprietà DataSource del controllo GridView sull'origine dati, quindi chiamare il metodo DataBind. Quando si utilizza questo metodo, il controllo GridView non fornisce funzionalità incorporate di ordinamento, aggiornamento, eliminazione e paging. È necessario fornire questa funzionalità manualmente.

## <a name="data-operations"></a>Operazioni dati

Il controllo GridView fornisce molte funzionalità incorporate che consentono all'utente di ordinare, aggiornare, eliminare, selezionare e scorrere gli elementi nel controllo. Quando il controllo GridView è associato a un controllo origine dati, il controllo GridView può sfruttare le funzionalità del controllo origine dati e fornire funzionalità di ordinamento, aggiornamento ed eliminazione automatiche.

> [!NOTE]
> Il controllo GridView può fornire supporto per l'ordinamento, l'aggiornamento e l'eliminazione con altri tipi di origini dati; Tuttavia, è necessario fornire un gestore eventi appropriato con l'implementazione per queste operazioni.

L'ordinamento consente all'utente di ordinare gli elementi nel controllo GridView rispetto a una colonna specifica facendo clic sull'intestazione della colonna. Per abilitare l'ordinamento, impostare la proprietà AllowSorting su **true**.

Le funzionalità di aggiornamento, eliminazione e selezione automatiche sono abilitate quando si fa clic su un pulsante in un campo colonna **ButtonField** o **TemplateField,** con il nome di comando "Edit", "Delete" e "Select", rispettivamente. Il controllo GridView può aggiungere automaticamente un campo colonna **CommandField** con un pulsante Modifica, Elimina o Seleziona se la proprietà AutoGenerateEditButton, AutoGenerateDeleteButton o AutoGenerateSelectButton è impostata rispettivamente su **true**.

> [!NOTE]
> L'inserimento di record nell'origine dati non è supportato direttamente dal controllo GridView. Tuttavia, è possibile inserire record utilizzando il controllo GridView in combinazione con il Controllo DetailsView o FormView.

Anziché visualizzare tutti i record nell'origine dati contemporaneamente, il controllo GridView può suddividere automaticamente i record in pagine. Per abilitare il paging, impostare la proprietà AllowPaging su **true**.

## <a name="customizing-the-user-interface"></a>Personalizzazione dell'interfaccia utente

È possibile personalizzare l'aspetto del controllo GridView impostando le proprietà di stile per le diverse parti del controllo. Nella tabella seguente sono elencate le diverse proprietà di stile.

| **Proprietà Style** | **Descrizione** |
| --- | --- |
| AlternatingRowStyle | Impostazioni di stile per le righe di dati alternate nel controllo GridView. Quando questa proprietà è impostata, le righe di dati vengono visualizzate alternando le impostazioni RowStyle e **AlternatingRowStyle.When** this property is set, the data rows are displayed alternad between the RowStyle settings and the AlternatingRowStyle settings. |
| ModificaStileRiga | Impostazioni di stile per la riga in corso di modifica nel controllo GridView. |
| EmptyDataRowStyle | Impostazioni di stile per la riga di dati vuota visualizzata nel controllo GridView quando l'origine dati non contiene record. |
| Footerstyle | Impostazioni di stile per la riga del piè di pagina del controllo GridView. |
| Headerstyle | Impostazioni di stile per la riga di intestazione del controllo GridView. |
| Pagerstyle | Impostazioni di stile per la riga del pager del controllo GridView. |
| Rowstyle | Impostazioni di stile per le righe di dati nel controllo GridView. Quando viene impostata anche la proprietà **AlternatingRowStyle,** le righe di dati vengono visualizzate alternando le impostazioni **RowStyle** e **AlternatingRowStyle.** |
| SelectedRowStyle (StileRiga)SelectedRowStyle | Impostazioni di stile per la riga selezionata nel controllo GridView. |

È inoltre possibile visualizzare o nascondere diverse parti del controllo. Nella tabella seguente sono elencate le proprietà che controllano quali parti vengono visualizzate o nascoste.

| **Proprietà** | **Descrizione** |
| --- | --- |
| ShowPiè di pagina | Mostra o nasconde la sezione del piè di pagina del controllo GridView. |
| Showheader | Mostra o nasconde la sezione dell'intestazione del controllo GridView. |

### <a name="events"></a>Events

Il controllo GridView fornisce diversi eventi che è possibile programmare. In questo modo è possibile eseguire una routine personalizzata ogni volta che si verifica un evento. Nella tabella seguente sono elencati gli eventi supportati dal controllo GridView.

| **Evento** | **Descrizione** |
| --- | --- |
| Pageindexchanged | Si verifica quando si fa clic su uno dei pulsanti di spostamento, ma dopo il controllo GridView gestisce l'operazione di spostamento. Questo evento viene in genere utilizzato quando è necessario eseguire un'attività dopo che l'utente passa a una pagina diversa nel controllo. |
| Pageindexchanging | Si verifica quando si fa clic su uno dei pulsanti di spostamento, ma prima che il controllo GridView gestisca l'operazione di spostamento. Questo evento viene spesso utilizzato per annullare l'operazione di paging. |
| RowCancelingModifica | Si verifica quando si fa clic sul pulsante Annulla di una riga, ma prima che il controllo GridView esca dalla modalità di modifica. Questo evento viene spesso utilizzato per interrompere l'operazione di annullamento. |
| Rowcommand | Si verifica quando si fa clic su un pulsante nel controllo GridView. Questo evento viene spesso utilizzato per eseguire un'attività quando si fa clic su un pulsante nel controllo. |
| Rowcreated | Si verifica quando viene creata una nuova riga nel controllo GridView. Questo evento viene spesso utilizzato per modificare il contenuto di una riga quando viene creata la riga. |
| RowDataBound | Si verifica quando una riga di dati è associata ai dati nel controllo GridView. Questo evento viene spesso utilizzato per modificare il contenuto di una riga quando la riga è associata a dati. |
| Rowdeleted | Si verifica quando si fa clic sul pulsante Elimina di una riga, ma dopo che il controllo GridView elimina il record dall'origine dati. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di eliminazione. |
| RigaEliminazione | Si verifica quando si fa clic sul pulsante Elimina di una riga, ma prima che il controllo GridView elimini il record dall'origine dati. Questo evento viene spesso utilizzato per annullare l'operazione di eliminazione. |
| Modifica delle righe | Si verifica quando si fa clic sul pulsante Modifica di una riga, ma prima che il controllo GridView passi alla modalità di modifica. Questo evento viene spesso utilizzato per annullare l'operazione di modifica. |
| Rowupdated | Si verifica quando si fa clic sul pulsante Aggiorna di una riga, ma dopo che il controllo GridView aggiorna la riga. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di aggiornamento. |
| Rowupdating | Si verifica quando si fa clic sul pulsante Aggiorna di una riga, ma prima che il controllo GridView aggiorni la riga. Questo evento viene spesso utilizzato per annullare l'operazione di aggiornamento. |
| SelectedIndexChanged | Si verifica quando si fa clic sul pulsante Seleziona di una riga, ma dopo il controllo GridView gestisce l'operazione di selezione. Questo evento viene spesso utilizzato per eseguire un'attività dopo la selezione di una riga nel controllo. |
| SelectedIndexModifica | Si verifica quando si fa clic sul pulsante Seleziona di una riga, ma prima che il controllo GridView gestisca l'operazione di selezione. Questo evento viene spesso utilizzato per annullare l'operazione di selezione. |
| Ordinati | Si verifica quando viene fatto clic sul collegamento ipertestuale per ordinare una colonna, ma dopo il controllo GridView gestisce l'operazione di ordinamento. Questo evento viene in genere utilizzato per eseguire un'attività dopo che l'utente fa clic su un collegamento ipertestuale per ordinare una colonna. |
| Ordinamento | Si verifica quando viene fatto clic sul collegamento ipertestuale per ordinare una colonna, ma prima che il controllo GridView gestisca l'operazione di ordinamento. Questo evento viene spesso utilizzato per annullare l'operazione di ordinamento o per eseguire una routine di ordinamento personalizzata. |

## <a name="formview"></a>Formview

Il FormView controllo viene utilizzato per visualizzare un singolo record da un'origine dati. È simile al controllo DetailsView, ad eccezione del fatto che visualizza i modelli definiti dall'utente anziché i campi riga. La creazione di modelli personalizzati offre una maggiore flessibilità nel controllo della modalità di visualizzazione dei dati. Il controllo FormView supporta le funzionalità seguenti:

- Associazione a controlli origine dati, ad esempio SqlDataSource e ObjectDataSource.Binding to data source controls, such as SqlDataSource and ObjectDataSource.
- Funzionalità di inserimento integrate.
- Funzionalità integrate di aggiornamento ed eliminazione.
- Funzionalità di paging incorporate.
- Accesso a livello di codice al modello a oggetti FormView per impostare dinamicamente le proprietà, gestire gli eventi e così via.
- Aspetto personalizzabile tramite modelli, temi e stili definiti dall'utente.

## <a name="templates"></a>Modelli

Per il FormView controllo per visualizzare il contenuto, è necessario creare modelli per le diverse parti del controllo. La maggior parte dei modelli sono facoltativi; Tuttavia, è necessario creare un modello per la modalità in cui è configurato il controllo. Ad esempio, un controllo FormView che supporta l'inserimento di record deve avere un modello di elemento di inserimento definito. Nella tabella seguente sono elencati i diversi modelli che è possibile creare.

| **Tipo di modello** | **Descrizione** |
| --- | --- |
| Edititemtemplate | Definisce il contenuto per la riga di dati quando il FormView controllo è in modalità di modifica. Questo modello contiene in genere i controlli di input e i pulsanti di comando con cui l'utente può modificare un record esistente. |
| Emptydatatemplate | Definisce il contenuto per la riga di dati vuota visualizzata quando il FormView controllo è associato a un'origine dati che non contiene record. Questo modello include in genere contenuto per avvisare l'utente che l'origine dati non contiene record. |
| Footertemplate | Definisce il contenuto per la riga del piè di pagina. Questo modello contiene in genere qualsiasi contenuto aggiuntivo che si desidera visualizzare nella riga del piè di pagina. In alternativa, è possibile specificare semplicemente il testo da visualizzare nella riga del piè di pagina impostando il FooterText proprietà. |
| Headertemplate | Definisce il contenuto per la riga di intestazione. Questo modello contiene in genere qualsiasi contenuto aggiuntivo che si desidera visualizzare nella riga di intestazione. In alternativa, è possibile specificare semplicemente il testo da visualizzare nella riga di intestazione impostando il HeaderText proprietà. |
| ItemTemplate | Definisce il contenuto per la riga di dati quando il FormView controllo è in modalità di sola lettura. Questo modello include in genere contenuto per visualizzare i valori di un record esistente. |
| Insertitemtemplate | Definisce il contenuto per la riga di dati quando il FormView controllo è in modalità di inserimento. Questo modello contiene in genere i controlli di input e i pulsanti di comando con cui l'utente può aggiungere un nuovo record. |
| PagerTemplate | Definisce il contenuto per la riga del pager visualizzata quando la funzionalità di spostamento è abilitata (quando la proprietà AllowPaging è impostata su **true**). Questo modello contiene in genere i controlli con cui l'utente può passare a un altro record. |

I controlli di input nel modello di elemento di modifica e nel modello di elemento di modifica possono essere associati ai campi di un'origine dati usando un'espressione di associazione bidirezionale. In questo modo il Controllo FormView estrarre automaticamente i valori del controllo di input per un'operazione di aggiornamento o inserimento. Le espressioni di associazione bidirezionale consentono inoltre ai controlli di input in un modello di elemento di modifica di visualizzare automaticamente i valori dei campi originali.

### <a name="binding-to-data"></a>Associazione ai dati

Il controllo FormView può essere associato a un controllo origine dati (ad esempio **SqlDataSource**, AccessDataSource, **ObjectDataSource** e così via) o a qualsiasi origine dati che implementa l'interfaccia System.Collections.IEnumerable , ad esempio System.Data.DataView, System.Collections.ArrayList e System.Collections.Hashtable . Utilizzare uno dei metodi seguenti per associare il controllo FormView al tipo di origine dati appropriato:

- Per eseguire l'associazione a un controllo origine dati, impostare la proprietà DataSourceID del controllo FormView sul valore ID del controllo origine dati. Il formView controllo viene associato automaticamente al controllo origine dati specificato e può sfruttare le funzionalità del controllo origine dati per eseguire l'inserimento, aggiornamento, eliminazione e paging funzionalità. Questo è il metodo preferito da associare ai dati.
- Per eseguire l'associazione a un'origine dati che implementa l'interfaccia **System.Collections.IEnumerable** , impostare a livello di codice la proprietà DataSource del controllo FormView sull'origine dati, quindi chiamare il metodo DataBind. Quando si utilizza questo metodo, il FormView controllo non fornisce funzionalità incorporate di inserimento, aggiornamento, eliminazione e paging. È necessario fornire questa funzionalità utilizzando l'evento appropriato.

## <a name="data-operations"></a>Operazioni dati

Il FormView controllo fornisce molte funzionalità incorporate che consentono all'utente di aggiornare, eliminare, inserire e scorrere gli elementi nel controllo. Quando il controllo FormView è associato a un controllo origine dati, il controllo FormView può sfruttare le funzionalità del controllo origine dati e fornire funzionalità automatiche di aggiornamento, eliminazione, inserimento e paging. Il form .View controllo può fornire il supporto per le operazioni di aggiornamento, eliminazione, inserimento e paging con altri tipi di origini dati; Tuttavia, è necessario fornire un gestore eventi appropriato con l'implementazione per queste operazioni.

Poiché il formView controllo utilizza modelli, non fornisce un modo per generare automaticamente i pulsanti di comando per eseguire l'aggiornamento, eliminazione o inserimento di operazioni. È necessario includere manualmente questi pulsanti di comando nel modello appropriato. Il FormView controllo riconosce alcuni pulsanti il cui **CommandName** proprietà impostate su valori specifici. Nella tabella seguente sono elencati i pulsanti di comando riconosciuti dal controllo FormView.

| **Pulsante** | **Valore commandname** | **Descrizione** |
| --- | --- | --- |
| Annulla | "Annulla" | Utilizzato nelle operazioni di aggiornamento o inserimento per annullare l'operazione e per eliminare i valori immessi dall'utente. Il Controllo FormView torna quindi alla modalità specificata dalla proprietà DefaultMode. |
| Delete | "Delete" | Utilizzato nelle operazioni di eliminazione per eliminare il record visualizzato dall'origine dati. Genera gli eventi ItemDeleting e ItemDeleted. |
| Modifica | "Modifica" | Utilizzato nelle operazioni di aggiornamento per mettere il controllo FormView in modalità di modifica. Il contenuto specificato nel **EditItemTemplate** proprietà viene visualizzata per la riga di dati. |
| Insert | "Inserisci" | Utilizzato nelle operazioni di inserimento per tentare di inserire un nuovo record nell'origine dati utilizzando i valori forniti dall'utente. Genera gli eventi ItemInserting e ItemInserted. |
| Nuovo | "Nuovo" | Utilizzato nelle operazioni di inserimento per mettere il controllo FormView in modalità di inserimento. Il contenuto specificato nel **InsertItemTemplate** proprietà viene visualizzata per la riga di dati. |
| Pagina | "Pagina" | Utilizzato nelle operazioni di paging per rappresentare un pulsante nella riga del pager che esegue il paging. Per specificare l'operazione di spostamento, impostare la proprietà **CommandArgument** del pulsante su "Next", "Prev", "First", "Last" o l'indice della pagina a cui spostarsi. Genera gli eventi PageIndexChanging e PageIndexChanged. |
| Aggiornamento | "Aggiorna" | Utilizzato nelle operazioni di aggiornamento per tentare di aggiornare il record visualizzato nell'origine dati con i valori forniti dall'utente. Genera gli eventi ItemUpdating e ItemUpdated. |

A differenza del pulsante Elimina (che elimina immediatamente il record visualizzato), quando si fa clic sul pulsante Modifica o Nuovo, il controllo FormView passa rispettivamente in modalità di modifica o inserimento. In modalità di modifica, il contenuto della proprietà **EditItemTemplate** viene visualizzato per l'elemento di dati corrente. In genere, il modello di elemento di modifica viene definito in modo che il pulsante Modifica viene sostituito con un aggiornamento e un Annulla pulsante. Anche i controlli di input appropriati per il tipo di dati del campo, ad esempio un controllo TextBox o CheckBox, vengono in genere visualizzati con il valore di un campo che l'utente può modificare. Se si fa clic sul pulsante Aggiorna, il record viene aggiornato nell'origine dati, mentre facendo clic sul pulsante Annulla vengono annullate le modifiche.

Analogamente, il contenuto contenuto nel **InsertItemTemplate** proprietà viene visualizzata per l'elemento di dati quando il controllo è in modalità di inserimento. Il modello di elemento di inserimento viene in genere definito in modo che il nuovo pulsante viene sostituito con un Insert e un Annulla pulsante e controlli di input vuoti vengono visualizzati per l'utente di immettere i valori per il nuovo record. Se si fa clic sul pulsante Inserisci, il record viene inserito nell'origine dati, mentre facendo clic sul pulsante Annulla vengono annullate le modifiche.

Il FormView controllo fornisce una funzionalità di spostamento, che consente all'utente di passare ad altri record nell'origine dati. Se abilitata, una riga di spostamento viene visualizzata nel controllo FormView che contiene i controlli di spostamento tra le pagine. Per abilitare il paging, impostare la proprietà **AllowPaging** su **true**. È possibile personalizzare la riga del pager impostando le proprietà degli oggetti contenuti nel PagerStyle e il PagerSettings proprietà. Anziché utilizzare l'interfaccia utente della riga del pager incorporata, è possibile creare un'interfaccia utente personalizzata usando la proprietà **PagerTemplate.Instead** of using the built-in pager row UI, you can create your own UI by using the PagerTemplate property.

## <a name="customizing-the-user-interface"></a>Personalizzazione dell'interfaccia utente

È possibile personalizzare l'aspetto del controllo FormView impostando le proprietà di stile per le diverse parti del controllo. Nella tabella seguente sono elencate le diverse proprietà di stile.

| **Proprietà Style** | **Descrizione** |
| --- | --- |
| ModificaStileRiga | Impostazioni di stile per la riga di dati quando il controllo FormView è in modalità di modifica. |
| EmptyDataRowStyle | Impostazioni di stile per la riga di dati vuota visualizzata nel controllo FormView quando l'origine dati non contiene record. |
| Footerstyle | Impostazioni di stile per la riga del piè di pagina del controllo FormView. |
| Headerstyle | Impostazioni di stile per la riga di intestazione del controllo FormView. |
| InsertRowStyle (Stile InsertA) | Impostazioni di stile per la riga di dati quando il controllo FormView è in modalità di inserimento. |
| Pagerstyle | Impostazioni di stile per la riga del pager visualizzata nel controllo FormView quando la funzionalità di spostamento è abilitata. |
| Rowstyle | Impostazioni di stile per la riga di dati quando il controllo FormView è in modalità di sola lettura. |

## <a name="events"></a>Events

Il FormView controllo fornisce diversi eventi che è possibile programmare. In questo modo è possibile eseguire una routine personalizzata ogni volta che si verifica un evento. Nella tabella seguente sono elencati gli eventi supportati dal controllo FormView .

| **Evento** | **Descrizione** |
| --- | --- |
| Itemcommand | Si verifica quando si fa clic su un pulsante all'interno di un controllo FormView. Questo evento viene spesso utilizzato per eseguire un'attività quando si fa clic su un pulsante nel controllo. |
| Itemcreated | Si verifica dopo la creazione di tutti gli oggetti FormViewRow nel controllo FormView. Questo evento viene spesso utilizzato per modificare i valori di un record prima che venga visualizzato. |
| Itemdeleted | Si verifica quando si fa clic su un pulsante Elimina (un pulsante con la relativa proprietà **CommandName** impostata su "Delete"), ma dopo che il controllo FormView elimina il record dall'origine dati. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di eliminazione. |
| ItemDeleting (Eliminazione di elementi) | Si verifica quando si fa clic su un pulsante Elimina, ma prima che il controllo FormView elimini il record dall'origine dati. Questo evento viene spesso utilizzato per annullare l'operazione di eliminazione. |
| Iteminserted | Si verifica quando si fa clic su un pulsante Inserisci (un pulsante con la relativa proprietà **CommandName** impostata su "Insert"), ma dopo che il controllo FormView ha inserito il record. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di inserimento. |
| Iteminserting | Si verifica quando si fa clic su un pulsante Inserisci, ma prima che il controllo FormView inserisca il record. Questo evento viene spesso utilizzato per annullare l'operazione di inserimento. |
| Itemupdated | Si verifica quando viene fatto clic su un pulsante Aggiorna (un pulsante con la relativa proprietà **CommandName** impostata su "Update"), ma dopo che il controllo FormView aggiorna la riga. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di aggiornamento. |
| Itemupdating | Si verifica quando si fa clic su un pulsante Aggiorna, ma prima che il controllo FormView aggiorni il record. Questo evento viene spesso utilizzato per annullare l'operazione di aggiornamento. |
| ModeChanged (Modalità modificata) | Si verifica dopo che il controllo FormView ha modificato le modalità (per modificare, inserire o modalità di sola lettura). Questo evento viene spesso utilizzato per eseguire un'attività quando il FormView controllo cambia modalità. |
| Modechanging | Si verifica prima che il FormView controllo cambia modalità (per modificare, inserire o modalità di sola lettura). Questo evento viene spesso utilizzato per annullare una modifica della modalità. |
| Pageindexchanged | Si verifica quando si fa clic su uno dei pulsanti di spostamento, ma dopo il FormView controllo gestisce l'operazione di spostamento. Questo evento viene in genere utilizzato quando è necessario eseguire un'attività dopo che l'utente passa a un record diverso nel controllo. |
| Pageindexchanging | Si verifica quando si fa clic su uno dei pulsanti di spostamento, ma prima che il FormView controllo gestisce l'operazione di spostamento. Questo evento viene spesso utilizzato per annullare l'operazione di paging. |

## <a name="detailsview"></a>Detailsview

Il Controllo DetailsView viene utilizzato per visualizzare un singolo record da un'origine dati in una tabella, in cui ogni campo del record viene visualizzato in una riga della tabella. Può essere utilizzato in combinazione con un controllo GridView per scenari master-dettagli. Il controllo DetailsView supporta le funzionalità seguenti:The DetailsView control supports the following features:

- Associazione a controlli origine dati, ad esempio SqlDataSource.Binding to data source controls, such as SqlDataSource.
- Funzionalità di inserimento integrate.
- Funzionalità integrate di aggiornamento ed eliminazione.
- Funzionalità di paging incorporate.
- Accesso a livello di codice al modello a oggetti DetailsView per impostare dinamicamente le proprietà, gestire gli eventi e così via.
- Aspetto personalizzabile attraverso temi e stili.

## <a name="row-fields"></a>Campi riga

Ogni riga di dati nel controllo DetailsView viene creata dichiarando un controllo campo. Tipi di campo riga diversi determinano il comportamento delle righe nel controllo. I controlli campo derivano da DataControlField.Field controls derive from DataControlField. Nella tabella seguente sono elencati i diversi tipi di campo riga che è possibile utilizzare.

| **Tipo di campo colonna** | **Descrizione** |
| --- | --- |
| BoundField | Visualizza il valore di un campo in un'origine dati come testo. |
| ButtonField | Visualizza un pulsante di comando nel controllo DetailsView. In questo modo è possibile visualizzare una riga con un controllo pulsante personalizzato, ad esempio un pulsante Aggiungi o Rimuovi.This allows you to display a row with a custom button control, such as an Add or a Remove button. |
| CheckBoxField | Visualizza una casella di controllo nel controllo DetailsView. Questo tipo di campo riga viene comunemente utilizzato per visualizzare campi con un valore booleano. |
| CommandField | Visualizza i pulsanti di comando incorporati per eseguire operazioni di modifica, inserimento o eliminazione nel controllo DetailsView. |
| HyperLinkField | Visualizza il valore di un campo in un'origine dati come collegamento ipertestuale. Questo tipo di campo riga consente di associare un secondo campo all'URL del collegamento ipertestuale. |
| ImageField | Visualizza un'immagine nel controllo DetailsView. |
| Templatefield | Visualizza il contenuto definito dall'utente per una riga nel controllo DetailsView in base a un modello specificato. Questo tipo di campo riga consente di creare un campo riga personalizzato. |

Per impostazione predefinita, la proprietà AutoGenerateRows è impostata su **true**, che genera automaticamente un oggetto campo riga associato per ogni campo di un tipo associabile nell'origine dati. I tipi associabili validi sono String, DateTime, Decimal, Guid e il set di tipi primitivi. Ogni campo viene quindi visualizzato in una riga come testo, nell'ordine in cui ogni campo viene visualizzato nell'origine dati.

La generazione automatica delle righe fornisce un modo semplice e veloce per visualizzare tutti i campi del record. Tuttavia, per utilizzare le funzionalità avanzate del controllo DetailsView è necessario dichiarare in modo esplicito i campi riga da includere nel controllo DetailsView. Per dichiarare i campi riga, impostare innanzitutto la proprietà **AutoGenerateRows** su **false**. Successivamente, aggiungere i tag ** &lt;Fields&gt; ** di apertura e chiusura tra i tag di apertura e chiusura del controllo DetailsView. Infine, elencare i campi riga che si desidera includere tra i tag Campi di apertura e ** &lt;di&gt; ** chiusura. I campi riga specificati vengono aggiunti all'insieme Fields nell'ordine indicato. Il **Fields** insieme consente di gestire a livello di codice i campi riga nel DetailsView controllo.

> [!NOTE]
> I campi riga generati automaticamente non vengono aggiunti all'insieme Fields.

## <a name="binding-to-data"></a>Associazione ai dati

Il controllo DetailsView può essere associato a un controllo origine dati, ad esempio **SqlDataSource** o AccessDataSource, o a qualsiasi origine dati che implementa l'interfaccia System.Collections.IEnumerable, ad esempio System.Data.DataView, System.Collections.ArrayList e System.Collections.Hashtable.

Utilizzare uno dei metodi seguenti per associare il controllo DetailsView al tipo di origine dati appropriato:

- Per eseguire l'associazione a un controllo origine dati, impostare la proprietà DataSourceID del controllo DetailsView sul valore ID del controllo origine dati. Il controllo DetailsView viene associato automaticamente al controllo origine dati specificato. Questo è il metodo preferito da associare ai dati.
- Per eseguire l'associazione a un'origine dati che implementa l'interfaccia **System.Collections.IEnumerable** , impostare a livello di codice la proprietà DataSource del controllo DetailsView sull'origine dati, quindi chiamare il metodo DataBind.

## <a name="security"></a>Sicurezza

Questo controllo può essere utilizzato per visualizzare l'input dell'utente, che potrebbe includere script client dannosi. Controllare tutte le informazioni inviate da un client per lo script eseguibile, istruzioni SQL o altro codice prima di visualizzarlo nell'applicazione. ASP.NET fornisce una funzionalità di convalida delle richieste di input per bloccare script e HTML nell'input dell'utente.

## <a name="data-operations"></a>Operazioni dati

Il controllo DetailsView fornisce funzionalità incorporate che consentono all'utente di aggiornare, eliminare, inserire e scorrere gli elementi nel controllo. Quando il controllo DetailsView è associato a un controllo origine dati, il controllo DetailsView può sfruttare le funzionalità del controllo origine dati e fornire funzionalità automatiche di aggiornamento, eliminazione, inserimento e paging.

Il controllo DetailsView può fornire supporto per le operazioni di aggiornamento, eliminazione, inserimento e paging con altri tipi di origini dati. Tuttavia, è necessario fornire l'implementazione per queste operazioni in un gestore eventi appropriato.

Il controllo DetailsView può aggiungere automaticamente un campo riga **CommandField** con un pulsante Edit, Delete o New impostando rispettivamente le proprietà AutoGenerateEditButton, AutoGenerateDeleteButton o AutoGenerateInsertButton su **true**. A differenza del pulsante Elimina (che elimina immediatamente il record selezionato), quando si fa clic sul pulsante Modifica o Nuovo, il controllo DetailsView passa rispettivamente in modalità di modifica o inserimento. In modalità di modifica, il pulsante Modifica viene sostituito con un pulsante Aggiorna e un pulsante Annulla. I controlli di input appropriati per il tipo di dati del campo, ad esempio un controllo TextBox o CheckBox, vengono visualizzati con il valore di un campo che l'utente può modificare. Se si fa clic sul pulsante Aggiorna, il record viene aggiornato nell'origine dati, mentre facendo clic sul pulsante Annulla vengono annullate le modifiche. Analogamente, in modalità di inserimento, il pulsante Nuovo viene sostituito con un Insert e un Cancel pulsante e controlli di input vuoti vengono visualizzati per l'utente di immettere i valori per il nuovo record.

Il DetailsView controllo fornisce una funzionalità di spostamento, che consente all'utente di passare ad altri record nell'origine dati. Quando questa opzione è abilitata, i controlli di spostamento tra le pagine vengono visualizzati in una riga del pager. Per abilitare il paging, impostare la proprietà AllowPaging su **true**. La riga del pager può essere personalizzata utilizzando le proprietà PagerStyle e PagerSettings.

## <a name="customizing-the-user-interface"></a>Personalizzazione dell'interfaccia utente

È possibile personalizzare l'aspetto del controllo DetailsView impostando le proprietà di stile per le diverse parti del controllo. Nella tabella seguente sono elencate le diverse proprietà di stile.

| **Proprietà Style** | **Descrizione** |
| --- | --- |
| AlternatingRowStyle | Impostazioni di stile per le righe di dati alternate nel controllo DetailsView. Quando questa proprietà è impostata, le righe di dati vengono visualizzate alternando le impostazioni RowStyle e **AlternatingRowStyle.When** this property is set, the data rows are displayed alternad between the RowStyle settings and the AlternatingRowStyle settings. |
| CommandRowStyle | Impostazioni di stile per la riga contenente i pulsanti di comando incorporati nel controllo DetailsView. |
| ModificaStileRiga | Impostazioni di stile per le righe di dati quando il controllo DetailsView è in modalità di modifica. |
| EmptyDataRowStyle | Impostazioni di stile per la riga di dati vuota visualizzata nel controllo DetailsView quando l'origine dati non contiene record. |
| Footerstyle | Impostazioni di stile per la riga del piè di pagina del controllo DetailsView. |
| Headerstyle | Impostazioni di stile per la riga di intestazione del controllo DetailsView. |
| InsertRowStyle (Stile InsertA) | Impostazioni di stile per le righe di dati quando il controllo DetailsView è in modalità di inserimento. |
| Pagerstyle | Impostazioni di stile per la riga del pager del controllo DetailsView. |
| Rowstyle | Impostazioni di stile per le righe di dati nel controllo DetailsView. Quando viene impostata anche la proprietà **AlternatingRowStyle,** le righe di dati vengono visualizzate alternando le impostazioni **RowStyle** e **AlternatingRowStyle.** |
| FieldHeaderStyle (StileIntestazione) | Impostazioni di stile per la colonna di intestazione del controllo DetailsView. |

## <a name="events"></a>Events

Il controllo DetailsView fornisce diversi eventi che è possibile programmare. In questo modo è possibile eseguire una routine personalizzata ogni volta che si verifica un evento. Nella tabella seguente sono elencati gli eventi supportati dal controllo DetailsView. Il controllo DetailsView eredita inoltre questi eventi dalle relative classi di base: DataBinding, DataBound, Disposed, Init, Load, PreRender e Render.

| **Evento** | **Descrizione** |
| --- | --- |
| Itemcommand | Si verifica quando si fa clic su un pulsante nel controllo DetailsView. |
| Itemcreated | Si verifica dopo la creazione di tutti gli oggetti DetailsViewRow nel controllo DetailsView. Questo evento viene spesso utilizzato per modificare i valori di un record prima che venga visualizzato. |
| Itemdeleted | Si verifica quando si fa clic su un pulsante Elimina, ma dopo che il controllo DetailsView elimina il record dall'origine dati. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di eliminazione. |
| ItemDeleting (Eliminazione di elementi) | Si verifica quando si fa clic su un pulsante Elimina, ma prima che il controllo DetailsView elimini il record dall'origine dati. Questo evento viene spesso utilizzato per annullare l'operazione di eliminazione. |
| Iteminserted | Si verifica quando si fa clic su un pulsante Inserisci, ma dopo che il controllo DetailsView ha inserito il record. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di inserimento. |
| Iteminserting | Si verifica quando si fa clic su un pulsante Inserisci, ma prima che il controllo DetailsView inserisca il record. Questo evento viene spesso utilizzato per annullare l'operazione di inserimento. |
| Itemupdated | Si verifica quando si fa clic su un pulsante Aggiorna, ma dopo che il controllo DetailsView aggiorna la riga. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di aggiornamento. |
| Itemupdating | Si verifica quando si fa clic su un pulsante Aggiorna, ma prima che il controllo DetailsView aggiorni il record. Questo evento viene spesso utilizzato per annullare l'operazione di aggiornamento. |
| ModeChanged (Modalità modificata) | Si verifica dopo che il controllo DetailsView cambia modalità (modifica, inserimento o modalità di sola lettura). Questo evento viene spesso utilizzato per eseguire un'attività quando il controllo DetailsView cambia modalità. |
| Modechanging | Si verifica prima che il controllo DetailsView modifichi le modalità (modifica, inserimento o modalità di sola lettura). Questo evento viene spesso utilizzato per annullare una modifica della modalità. |
| Pageindexchanged | Si verifica quando si fa clic su uno dei pulsanti di spostamento, ma dopo il Controllo DetailsView gestisce l'operazione di spostamento. Questo evento viene in genere utilizzato quando è necessario eseguire un'attività dopo che l'utente passa a un record diverso nel controllo. |
| Pageindexchanging | Si verifica quando si fa clic su uno dei pulsanti di spostamento, ma prima che il controllo DetailsView gestisca l'operazione di spostamento. Questo evento viene spesso utilizzato per annullare l'operazione di paging. |

## <a name="the-menu-control"></a>Il controllo Menu

Il controllo Menu in ASP.NET 2.0 è progettato per essere un sistema di navigazione completo. Può essere associato facilmente a dati a origini dati gerarchiche, ad esempio SiteMapDataSource.It can be databound easily to hierarchical data sources such as the SiteMapDataSource.

Oggetto Menu controlli struttura può essere definita in modo dichiarativo o dinamico ed è costituito da un singolo nodo radice e un numero qualsiasi di sottonodi. Il codice seguente definisce in modo dichiarativo un menu per il Menu controllo.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

Nell'esempio precedente, il Home.aspx nodo è il nodo radice. Tutti gli altri nodi sono nidificati all'interno del nodo radice a vari livelli.

Esistono due tipi di menu che il Menu controllo può eseguire il rendering; menu statici e menu dinamici. I menu statici sono costituiti da voci di menu sempre visibili. I menu dinamici sono costituiti da voci di menu che sono visibili solo quando l'utente passa su di esse con il mouse. I clienti possono spesso confondere i menu statici con i menu definiti in modo dichiarativo e i menu dinamici con menu associati a dati in fase di esecuzione. Infatti, i menu dinamici e statici non sono correlati al metodo della popolazione. I termini *statici* e *dinamici* si riferiscono solo a se il menu viene visualizzato in modo statico per impostazione predefinita o solo quando l'utente esegue un'azione.

Il **StaticDisplayLevels** proprietà viene utilizzata per configurare quanti livelli del menu sono statici e pertanto visualizzati per impostazione predefinita. Nell'esempio precedente, l'impostazione della proprietà **StaticDisplayLevels** su un valore pari a 2 causerebbe la visualizzazione statica del menu del nodo Home, del nodo Musica e del nodo Movies. Tutti gli altri nodi verrebbero visualizzati dinamicamente quando l'utente passa il mouse sul nodo padre.

La proprietà **MaximumDynamicDisplayLevels** consente di configurare il numero massimo di livelli dinamici che il menu è in grado di visualizzare. Tutti i menu dinamici a un livello superiore al valore specificato dalla proprietà **MaximumDynamicDisplayLevels** vengono eliminati.

> [!NOTE]
> È quasi certo che si verifichino situazioni in cui i menu non sembrano eseguire il rendering a causa della proprietà MaximumDynamicDisplayLevels. In questi casi, assicurarsi che la proprietà sia impostata sufficientemente per consentire la visualizzazione dei menu dei clienti.

## <a name="data-binding-the-menu-control"></a>Associazione dati del controllo MenuData Binding the Menu Control

The Menu control can be bound to any hierarchical data source such as the SiteMapDataSource or the XMLDataSource. Il SiteMapDataSource è il metodo più comunemente utilizzato per l'associazione dati a un Menu controllo perché si nutre del file Web.sitemap e il relativo schema fornisce un'API nota per il Menu controllo. Nell'elenco riportato di seguito viene illustrato un semplice file Web.sitemap.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Si noti che è presente un solo elemento siteMapNode radice, in questo caso l'elemento Home. Diversi attributi possono essere configurati per ogni siteMapNode.Several attributes can be configured for each siteMapNode. Gli attributi più comunemente utilizzati sono:

- **url** Specifica l'URL da visualizzare quando un utente fa clic sulla voce di menu. Se questo attributo non è presente, il nodo eseguirà semplicemente il postback quando si fa clic su di questo pulsante.
- **titolo** Specifica il testo visualizzato nella voce di menu.
- **descrizione** Utilizzato come documentazione per il nodo. Viene visualizzata anche come descrizione comando quando il mouse viene posizionato sul nodo.
- **file siteMap** Consente sitemap nidificate. Questo attributo deve puntare a un file sitemap ASP.NET ben formato.
- **ruoli** Consente di controllare l'aspetto di un nodo mediante ASP.NET taglio di sicurezza.

Si noti che mentre questi attributi sono tutti facoltativi, il comportamento del menu potrebbe non essere quello previsto se non vengono specificati. Ad esempio, se viene specificato l'attributo *url* ma l'attributo *description* non lo è, il nodo non sarà visibile e non sarà possibile passare all'URL specificato.

## <a name="controlling-a-menus-operation"></a>Controllo di un'operazione di menu

Esistono diverse proprietà che influiscono sul funzionamento di un controllo Menu ASP.NET; la proprietà **Orientation,** la proprietà **DisappearAfter,** la proprietà **StaticItemFormatString** e la proprietà **StaticPopoutImageUrl** sono solo alcune di queste.

- **L'orientamento** può essere impostato su *orizzontale* o *verticale* e controlla se le voci di menu statico sono disposte orizzontalmente in una riga o verticalmente e impilate l'una sull'altra. Questa proprietà non influisce sui menu dinamici.
- La proprietà **DisappearAfter** configura per quanto tempo un menu dinamico deve rimanere visibile dopo che il mouse è stato allontanato da esso. Il valore è specificato in millisecondi e il valore predefinito è 500.The value is specified in milliseconds and defaults to 500. Se si imposta questa proprietà su un valore -1, il menu non scomparirà mai automaticamente. In tal caso, il menu scomparirà solo quando l'utente fa clic all'esterno del menu.
- Il **StaticItemFormatString** proprietà semplifica la gestione di verbi coerenti nel sistema di menu. Quando si specifica *{0}* questa proprietà, è necessario immettere al posto della descrizione visualizzata nell'origine dati. Ad esempio, per fare in modo che la voce di menu dell'esercizio {0} 1 dica Visita la nostra pagina Prodotti e così via, è necessario specificare Visita la nostra pagina per StaticItemFormatString. In fase di esecuzione, {0} ASP.NET sostituirà qualsiasi occorrenza di con la descrizione corretta per la voce di menu.
- La proprietà **StaticPopoutImageUrl** specifica l'immagine utilizzata per indicare che un determinato nodo di menu dispone di nodi figlio a cui è possibile accedere passando il puntatore del mouse su di esso. I menu dinamici continueranno a utilizzare l'immagine predefinita.

## <a name="templated-menu-controls"></a>Controlli del menu basato su modelli

Il Menu controllo è un controllo basato su modelli e consente due diversi ItemTemplates; StaticItemTemplate e DynamicItemTemplate. Utilizzando questi modelli, è possibile aggiungere facilmente controlli server o controlli utente ai menu.

Per modificare i modelli in Visual Studio .NET, fare clic sul pulsante Smart tag dal menu e scegliere Modifica modelli. È quindi possibile scegliere tra la modifica di StaticItemTemplate o DynamicItemTemplate.You can then choose between editing the StaticItemTemplate or the DynamicItemTemplate.

Tutti i controlli aggiunti a StaticItemTemplate verranno visualizzati nel menu statico al caricamento della pagina. Tutti i controlli aggiunti a DynamicItemTemplate verranno visualizzati in tutti i menu a comparsa.

## <a name="menu-events"></a>Eventi di menu

Il Menu controllo dispone di due eventi che sono univoci per esso; il **MenuItemClicked** e il **MenuItemDatabound** evento.

Il MenuItemClicked evento viene generato quando si fa clic su una voce di menu. Il MenuItemDatabound evento viene generato quando una voce di menu è associata a dati. Il **MenuEventArgs** che viene passato al gestore eventi fornisce l'accesso alla voce di menu tramite il Item proprietà.

## <a name="controlling-a-menus-appearance"></a>Controllo dell'aspetto di un menu

È inoltre possibile modificare l'aspetto di un controllo menu utilizzando uno o più dei numerosi stili disponibili per formattare i menu. Tra questi si trovano **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**e **DynamicHoverStyle**. Queste proprietà vengono configurate utilizzando una stringa di stile HTML standard. Ad esempio, quanto segue influirà sullo stile dei menu dinamici.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Se si utilizza uno degli stili al passaggio &lt;&gt; del mouse, sarà necessario aggiungere un elemento head alla pagina con l'elemento *runat* impostato su *server*.

I controlli menu supportano anche l'uso di temi ASP.NET 2.0.

## <a name="the-treeview-control"></a>Il controllo TreeView

Il controllo TreeView visualizza i dati in una struttura ad albero. As with the Menu control, it can be easily data bound to any hierarchical data source such as the SiteMapDataSource.

La prima domanda che i clienti potrebbero porre sul controllo TreeView in ASP.NET 2.0 è se è correlato o meno al WebControl di TreeView IE disponibile per ASP.NET 1.x. Non lo è. Il ASP.NET 2.0 controllo TreeView è stato scritto da zero e offre miglioramenti significativi rispetto a WebControl di IE TreeView che era precedentemente disponibile.

Non entrerò in dettaglio su come associare un controllo TreeView a una mappa del sito in quanto viene eseguita esattamente nello stesso modo del controllo Menu. Tuttavia, il controllo TreeView presenta alcune differenze distinte nel modo in cui opera.

Per impostazione predefinita, un controllo TreeView viene visualizzato completamente espanso. Per modificare il livello di espansione al caricamento iniziale, modificare la proprietà **ExpandDepth** del controllo. Ciò è particolarmente importante nei casi in cui il controllo TreeView è associato a dati durante l'espansione di nodi specifici.

## <a name="databinding-the-treeview-control"></a>DataBinding del controllo TreeView

A differenza del menu controllo, il controllo TreeView si presta bene alla gestione di grandi quantità di dati. Pertanto, oltre all'associazione dati a un SiteMapDataSource o XMLDataSource, il TreeView è spesso associato a dati a un DataSet o altri dati relazionali. Nei casi in cui il controllo TreeView è associato a grandi quantità di dati, è consigliabile eseguire l'associazione solo ai dati effettivamente visibili nel controllo. È quindi possibile eseguire l'associazione dati a dati aggiuntivi quando i nodi TreeView vengono espansi.

In questi casi, la proprietà **PopulateOnDemand** di TreeView deve essere impostata su *true*. Sarà quindi necessario fornire un'implementazione per il **TreeNodePopulate** metodo.

## <a name="data-binding-without-postback"></a>Associazione dati senza postBackData Binding Without PostBack

Si noti che quando si espande un nodo nell'esempio precedente per la prima volta, la pagina esegue il postback e viene aggiornata. Che non è un problema in questo esempio, ma si può immaginare che potrebbe essere in un ambiente di produzione con una grande quantità di dati. Uno scenario migliore sarebbe quello in cui il controllo TreeView sarebbe ancora in modo dinamico popolare i relativi nodi, ma senza un postback al server.

Impostando il **PopulateNodesFromClient** e **PopulateOnDemand** proprietà true, il ASP.NET controllo TreeView popolerà dinamicamente i nodi senza un postback. Quando il nodo padre viene espanso, viene effettuata una richiesta XMLHttp dal client e viene generato l'evento OnTreeNodePopulate. Il server risponde con un'isola di dati XML che viene quindi utilizzata per associare i nodi figlio.

ASP.NET crea dinamicamente il codice client che implementa questa funzionalità. I &lt;&gt; tag di script che contengono lo script vengono generati che puntano a un file AXD. Ad esempio, nell'elenco riportato di seguito vengono illustrati i collegamenti di script per il codice di script che genera la richiesta XMLHttp.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Se si sfoglia il file AXD sopra nel browser e lo si apre, verrà visualizzato il codice che implementa la richiesta XMLHttp. Questo metodo impedisce ai clienti di modificare il file di script.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Controllo del funzionamento del controllo TreeView

Il TreeView controllo dispone di diverse proprietà che influiscono sul funzionamento del controllo. Le proprietà più ovvie sono **ShowCheckBoxes**, **ShowExpandCollapse**e **ShowLines**.

La proprietà **ShowCheckBoxes** determina se i nodi visualizzano o meno una casella di controllo durante il rendering. I valori validi per questa proprietà sono **None**, **Root**, **Parent**, **Leaf**e **All**. Questi influiscono sul controllo TreeView come segue:

| **Valore proprietà** | **Effetto** |
| --- | --- |
| nessuno | Le caselle di controllo non vengono visualizzate su alcun nodo. Si tratta dell'impostazione predefinita. |
| Radice | Una casella di controllo viene visualizzata solo sul nodo radice. |
| Parent | Una casella di controllo viene visualizzata solo sui nodi che dispongono di nodi figlio. Tali nodi figlio possono essere nodi padre o nodi foglia. |
| Foglia | Una casella di controllo viene visualizzata solo nei nodi che non dispongono di nodi figlio. |
| Tutti | Viene visualizzata una casella di controllo su tutti i nodi. |

Quando vengono utilizzate le caselle di controllo, il **CheckedNodes** proprietà restituirà una raccolta di TreeView nodi che vengono controllati al postback.

La proprietà **ShowExpandCollapse** controlla l'aspetto dell'immagine di espansione/compressione accanto ai nodi radice e padre. Se questa proprietà è impostata su **false**, il rendering dei nodi TreeView viene eseguito come collegamento ipertestuale e vengono espansi/compressi facendo clic sul collegamento.

La proprietà **ShowLines** controlla se vengono visualizzate o meno linee che collegano i nodi padre ai nodi figlio. Quando **false** (impostazione predefinita), non viene visualizzata alcuna riga. Se **true**, il controllo TreeView utilizzerà le immagini delle righe nella cartella specificata dalla proprietà **LineImagesFolder** .

Per personalizzare l'aspetto delle linee di TreeView, Visual Studio .NET 2005 include uno strumento Progettazione linee. È possibile accedere a questo strumento utilizzando il pulsante Smart Tag nel controllo TreeView come indicato di seguito.

![](data-bound-controls/_static/image1.jpg)

**Figura 1**

Quando si seleziona l'opzione di menu **Personalizza immagini linea,** verrà avviato lo strumento Line Designer che consente di configurare l'aspetto delle linee TreeView.

## <a name="treeview-events"></a>TreeView Eventi

Il controllo TreeView dispone dei seguenti eventi univoci:

- SelectedNodeChanged Si verifica quando viene selezionato un nodo in base al **SelectAction** proprietà.
- TreeNodeCheckChanged Si verifica quando viene modificato lo stato delle caselle di controllo di un nodo.
- TreeNodeExpanded si verifica quando un nodo viene espanso in base al **SelectAction** proprietà.
- TreeNodeCollapsed Si verifica quando un nodo viene compresso.
- TreeNodeDataBound Si verifica quando un nodo è associato a dati.
- TreeNodePopulate Si verifica quando un nodo viene popolato.

La proprietà **SelectAction** consente di configurare l'evento generato quando viene selezionato un nodo. La proprietà SelectAction fornisce le azioni seguenti:The SelectAction property provides the following actions:

- TreeNodeSelectAction.Expand Genera TreeNodeExpanded quando il nodo è selezionato.
- TreeNodeSelectAction.None Non genera alcun evento quando il nodo è selezionato.
- TreeNodeSelectAction.Select Genera l'evento SelectedNodeChanged quando viene selezionato il nodo.
- TreeNodeSelectAction.SelectExpand Genera l'evento SelectedNodeChanged e l'evento TreeNodeExpanded quando il nodo viene selezionato.

## <a name="controlling-appearance-with-styles"></a>Controllo dell'aspetto con gli stili

Il controllo TreeView fornisce molte proprietà per controllare l'aspetto del controllo con gli stili. Sono disponibili le proprietà seguenti.

| **Nome proprietà** | **Controlli** |
| --- | --- |
| HoverNodeStyle (Stile HoverNode) | Controlla lo stile dei nodi quando il mouse viene posizionato su di essi. |
| LeafNodeStyle | Controlla lo stile dei nodi foglia. |
| Nodestyle | Controlla lo stile per tutti i nodi. Gli stili di nodo specifici (ad esempio LeafNodeStyle) eseguono l'override di questo stile. |
| Oggetto ParentNodeStyle | Controlla lo stile per tutti i nodi padre. |
| Rootnodestyle | Controlla lo stile per il nodo radice. |
| Selectednodestyle | Controlla lo stile per il nodo selezionato. |

Ognuna di queste proprietà è di sola lettura. Tuttavia, ognuno restituirà un **TreeNodeStyle** oggetto e le proprietà di tale oggetto possono essere modificate utilizzando il formato *di proprietà-sottoproprietà.* Ad esempio, per impostare la proprietà **ForeColor** di **SelectedNodeStyle**, è necessario utilizzare la sintassi seguente:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Si noti che il tag precedente non è chiuso. Questo perché quando si utilizza la sintassi dichiarativa illustrata di seguito, è necessario includere i nodi TreeViews anche nel codice HTML.

Le proprietà di stile possono essere specificate anche nel codice utilizzando il formato *property.subproperty.* Ad esempio, per impostare la proprietà **ForeColor** di **RootNodeStyle** nel codice, è necessario utilizzare la sintassi seguente:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Per un elenco completo delle diverse proprietà di stile, vedere la documentazione MSDN sul TreeNodeStyle oggetto.

## <a name="the-sitemappath-control"></a>Il controllo SiteMapPathThe SiteMapPath Control

Il controllo SiteMapPath fornisce un controllo di spostamento bread per gli sviluppatori di ASP.NET. Come gli altri controlli di spostamento, può essere facilmente associato a origini dati gerarchiche, ad esempio SiteMapDataSource o XmlDataSource.As the other navigation controls, it can be easily data bound to hierarchical data sources such as the SiteMapDataSource or XmlDataSource.

Un controllo SiteMapPath è costituito da oggetti SiteMapNodeItem. Esistono tre tipi di nodi; il nodo Root, i nodi padre e il nodo Current. Il nodo radice è il nodo nella parte superiore della struttura gerarchica. Il nodo corrente rappresenta la pagina corrente. Tutti gli altri nodi sono nodi padre.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Controllo del funzionamento del controllo SiteMapPath

Le proprietà che controllano il funzionamento del controllo SiteMapPath sono le seguenti:

| **Proprietà** | **Descrizione della proprietà** |
| --- | --- |
| ParentLevelsVisualizzato | Controlla il numero di nodi padre visualizzati. Il valore predefinito è -1 che non impone alcuna restrizione sul numero di nodi padre visualizzati. |
| PathDirection (Direzione percorso) | Controlla la direzione di SiteMapPath. I valori validi sono RootToCurrent (impostazione predefinita) e CurrentToRoot.Valid values are RootToCurrent (default) and CurrentToRoot. |
| Pathseparator | Oggetto String che controlla il carattere che separa i nodi in un SiteMapPath controllo. Il valore predefinito è :. |
| RenderCurrentNodeAsLink (Informazioni in base al gruppo dei problemi del documento d' | Valore booleano che controlla se viene eseguito il rendering del nodo corrente come collegamento. Il valore predefinito è False. |
| Skiplinktext | Assiste con l'accessibilità quando la pagina viene visualizzata dalle utilità per la lettura dello schermo. Questa proprietà consente alle utilità per la lettura dello schermo di ignorare il controllo SiteMapPath.This property allows screen reader to skip the SiteMapPath control. Per disabilitare questa funzionalità, impostare la proprietà su String.Empty. |

## <a name="templated-sitemappath-controls"></a>Controlli SiteMapPath basati su modelli

Il SiteMapControl è un controllo basato su modelli e, di conseguenza, è possibile definire diversi modelli da utilizzare nella visualizzazione del controllo. Per modificare i modelli in un controllo SiteMapPath, fare clic sul pulsante Smart Tag nel controllo e scegliere Modifica modelli dal menu. Verrà visualizzato il menu SiteMapTasks come illustrato di seguito in cui è possibile scegliere tra i diversi modelli disponibili.

![](data-bound-controls/_static/image2.jpg)

**Figura 2**

Il **modello NodeTemplate** fa riferimento a qualsiasi nodo in SiteMapPath.The NodeTemplate template refers to any node in the SiteMapPath. Se il nodo è un nodo radice o il nodo corrente e un **RootNodeTemplate** o **CurrentNodeTemplate** è configurato, il NodeTemplate viene sottoposto a override.

## <a name="sitemappath-events"></a>SiteMapPath Eventi

Il SiteMapPath controllo dispone di due eventi che non derivano dal Control classe; l'evento **ItemCreated** e l'evento **ItemDataBound.** Il ItemCreated evento viene generato quando viene creato un SiteMapPath elemento. ItemDataBound viene generato quando il DataBind metodo viene chiamato durante l'associazione dati di un SiteMapPath nodo. Oggetto **SiteMapNodeItemEventArgs** oggetto fornisce l'accesso al SiteMapNodeItem specifico tramite il Item proprietà.

## <a name="controlling-appearance-with-styles"></a>Controllo dell'aspetto con gli stili

Gli stili seguenti sono disponibili per la formattazione di un SiteMapPath controllo.

| **Nome proprietà** | **Controlli** |
| --- | --- |
| CurrentNodeStyle (Stile NodoCorrente) | Controlla lo stile del testo per il nodo corrente. |
| Rootnodestyle | Controlla lo stile del testo per il nodo radice. |
| Nodestyle | Controlla lo stile del testo per tutti i nodi presupponendo che un CurrentNodeStyle o RootNodeStyle non applicabile. |

Il NodeStyle proprietà viene sottoposta a override da CurrentNodeStyle o RootNodeStyle. Ognuna di queste proprietà è di sola lettura e restituisce un **Style** oggetto. Per modificare l'aspetto di un nodo utilizzando una di queste proprietà, è necessario impostare le proprietà del Style oggetto restituito. Ad esempio, il codice seguente modifica la proprietà forecolor del nodo corrente.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

La proprietà può essere applicata anche a livello di codice come indicato di seguito:The property can also be applied programmatically as follows:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Se viene applicato un modello, lo stile non verrà applicato.

## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Laboratorio 1: Configurazione di un controllo menu ASP.NET

1. Creare un nuovo sito Web.
2. Aggiungere un file di mappa del sito selezionando File, Nuovo, File e scegliendo Mappa del sito dall'elenco dei modelli di file.
3. Aprire la mappa del sito (Web.sitemap per impostazione predefinita) e modificarla in modo che abbia l'aspetto dell'elenco seguente. Le pagine a cui si sta collegando nel file di mappa del sito non esistono realmente, ma questo non sarà un problema per questo esercizio.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Aprire il modulo Web predefinito in visualizzazione Struttura.
5. Dalla sezione Navigazione della Casella degli strumenti aggiungere un nuovo controllo Menu alla pagina.
6. Dalla sezione Dati della Casella degli strumenti, aggiungere un nuovo SiteMapDataSource. SiteMapDataSource utilizzerà automaticamente il file Web.sitemap nel sito. Il file Web.sitemap *deve* trovarsi nella cartella radice del sito.
7. Fare clic sul controllo Menu, quindi fare clic sul pulsante Smart Tag per visualizzare la finestra di dialogo Attività menu.
8. Nell'elenco a discesa Scegli origine dati selezionare SiteMapDataSource1.In the Choose Data Source dropdown, select SiteMapDataSource1.
9. Fare clic sul collegamento Formattazione automatica e scegliere un formato per il menu.
10. Nel riquadro Proprietà impostare la proprietà **StaticDisplayLevels** su 2. Il controllo Menu dovrebbe ora visualizzare il nodo Home, Products e Services nella finestra di progettazione.
11. Sfogliare la pagina nel browser per utilizzare il menu. Poiché le pagine aggiunte alla mappa del sito non esistono effettivamente, verrà visualizzato un errore quando si tenta di individuarle.

Provare a modificare il StaticDisplayLevels e MaximumDynamicDisplayLevels proprietà e vedere come influenzano il modo in cui viene eseguito il rendering del menu.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Laboratorio 2: Associazione dinamica di un controllo TreeView

In questo esercizio si presuppone che SQL Server sia in esecuzione localmente e che il database Northwind sia presente nell'istanza di SQL Server. Se queste condizioni non vengono soddisfatte, modificare la stringa di connessione nell'esempio. Si noti che potrebbe anche essere necessario specificare l'autenticazione di SQL Server anziché una connessione trusted.

1. Creare un nuovo sito Web.
2. Passare alla visualizzazione Codice per Default.aspx e sostituire tutto il codice con il codice riportato di seguito. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Salvare la pagina come treeview.aspx.
4. Sfoglia la pagina.
5. Quando la pagina viene visualizzata per la prima volta, visualizzare l'origine della pagina nel browser. Si noti che solo i nodi visibili sono stati inviati al client.
6. Fare clic sul segno più accanto a qualsiasi nodo.
7. Visualizzare nuovamente l'origine nella pagina. Si noti che i nodi appena visualizzati sono ora presenti.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Lab 3: Visualizzazione dettagli e modifica dei dati utilizzando un controllo GridView e DetailsView

1. Creare un nuovo sito Web.
2. Aggiungere un nuovo file web.config al sito Web.
3. Aggiungere una stringa di connessione al file web.config come illustrato di seguito:Add a connection string to the web.config file as shown below: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Potrebbe essere necessario modificare la stringa di connessione in base all'ambiente.
4. Salvare e chiudere il file Web.config.
5. Aprire Default.aspx e aggiungere un nuovo controllo SqlDataSource.Open Default.aspx and add a new SqlDataSource control.
6. Modificare l'ID del controllo SqlDataSource in **Products**.
7. Nel menu **Attività SqlDataSource** fare clic su **Configura origine dati**.
8. Selezionare **Northwind** nell'elenco a discesa della connessione e fare clic su Avanti.
9. Selezionare **Prodotti** dall'elenco a discesa **Nome** e selezionare le caselle di controllo **IDProdotto**, **NomeProdotto**, **PrezzoUnitario**e **UnitàInStock,** come illustrato di seguito. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Fare clic su **Avanti**.
11. Fare clic su **Fine**.
12. Passare alla visualizzazione Origine ed esaminare il codice generato. Si notino i controlli **SelectCommand**, **DeleteCommand**, **InsertCommand**e **UpdateCommand** aggiunti al controllo SqlDataSource . Si noti inoltre i parametri che sono stati aggiunti.
13. Passare alla visualizzazione Progettazione e aggiungere un nuovo controllo GridView alla pagina.
14. Selezionare **Prodotti** dall'elenco a discesa **Scegli origine dati.**
15. Selezionare **Abilita paging** e **Abilita selezione** come illustrato di seguito. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Fare clic sul collegamento **Modifica colonne** e verificare che l'opzione Genera **automaticamente campi** sia selezionata.
17. Fare clic su **OK**.
18. Con il controllo GridView selezionato, fare clic sul pulsante accanto alla proprietà **DataKeyNames** nel riquadro Proprietà.
19. Selezionare **ProductID** dall'elenco Campi **&gt;** dati **disponibili** e fare clic sul pulsante per aggiungerlo.
20. Fare clic su OK.
21. Aggiungere un nuovo controllo SqlDataSource alla pagina.
22. Modificare l'ID del controllo SqlDataSource in **Details**.
23. Scegliere **Configura origine dati**dal menu Attività SqlDataSource .
24. Scegliere **Northwind** dall'elenco a discesa e fare clic su **Avanti**.
25. Selezionare <strong>Prodotti</strong> dall'elenco <strong> \<a discesa <strong>Nome</strong> e selezionare la casella di controllo /strong> nella casella di riepilogo <strong>Colonne.</strong>
26. Fare clic sul pulsante **WHERE.**
27. Selezionare ProductID dall'elenco a discesa **Colonna.Select** **ProductID** from the Column dropdown.
28. Selezionare **=** nell'elenco a discesa Operatore.Select in the Operator dropdown.
29. Selezionare Controllo nell'elenco a discesa **Origine.Select Control** from the **Source** dropdown.
30. Selezionare **GridView1** dall'elenco a discesa **ID controllo.**
31. Fare clic sul pulsante **Aggiungi** per aggiungere la clausola WHERE.
32. Fare clic su **OK**.
33. Fare clic sul pulsante **Avanzate** e selezionare la casella di controllo **Genera istruzioni INSERT, UPDATE e DELETE.**
34. Fare clic su **OK**.
35. Fare clic su **Avanti** e quindi su **Fine**.
36. Aggiungere un controllo DetailsView alla pagina.
37. Nell'elenco a discesa **Scegli origine dati** scegliere **Dettagli**.
38. Selezionare la casella di controllo **Abilita modifica** come illustrato di seguito. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Salvare la pagina ed esplorare Default.aspx.
40. Fare clic sul collegamento **Seleziona** accanto a record diversi per visualizzare automaticamente l'aggiornamento di DetailsView.
41. Fare clic sul collegamento **Modifica** nel controllo DetailsView.
42. Apportare una modifica al record e fare clic su **Aggiorna**.
