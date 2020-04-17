---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: Visualizzazione dei dati in un grafico con ASP.NET pagine Web (Razor) Documenti Microsoft
author: rick-anderson
description: Questo capitolo spiega come visualizzare i dati in un grafico. Nei capitoli precedenti è stato illustrato come visualizzare i dati manualmente e in una griglia. Questo capitolo spiega...
ms.author: riande
ms.date: 05/22/2012
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: ad55d4898ee39e0239ef8b0bd5eea6387af3c816
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542885"
---
# <a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>Visualizzazione di dati in un grafico con ASP.NET pagine Web (Razor)

da parte [di Microsoft](https://github.com/microsoft)

> In questo articolo viene illustrato come utilizzare un grafico per visualizzare i dati `Chart` in un sito Web di pagine Web ASP.NET (Razor) utilizzando l'helper.
> 
> **Cosa imparerai**:
> 
> - Come visualizzare i dati in un grafico.
> - Come applicare uno stile ai grafici utilizzando i temi incorporati.
> - Come salvare i grafici e come memorizzarli nella cache per migliorare le prestazioni.
> 
> Di seguito sono riportate le funzionalità di programmazione ASP.NET introdotte nell'articolo:
> 
> - L'aiutante. `Chart`
> 
> > [!NOTE]
> > Le informazioni contenute in questo articolo si applicano a ASP.NET pagine Web 1.0 e pagine Web 2.

<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>L'helper del grafico

Quando si desidera visualizzare i dati in `Chart` forma grafica, è possibile utilizzare l'helper. L'helper `Chart` può eseguire il rendering di un'immagine che visualizza i dati in diversi tipi di grafico. Supporta molte opzioni per la formattazione e l'etichettatura. L'helper `Chart` può eseguire il rendering di più di 30 tipi di grafici, inclusi tutti i tipi di grafici che potrebbero avere familiarità con Microsoft Excel o altri strumenti &#8212; grafici ad area, grafici a barre, istogrammi, grafici a linee e grafici a torta, insieme a grafici più specializzati come i grafici azionari.

| **Grafico ad area** Descrizione: Immagine del tipo di grafico ad area ![](7-displaying-data-in-a-chart/_static/image1.jpg) | **Grafico a barre** ![Descrizione: Immagine del tipo di grafico a barre](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Istogramma** ![Descrizione: Immagine del tipo di istogramma](7-displaying-data-in-a-chart/_static/image3.jpg) | **Grafico a linee** ![Descrizione: Immagine del tipo di grafico a linee](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **Grafico a torta** ![Descrizione: Immagine del tipo di grafico a torta](7-displaying-data-in-a-chart/_static/image5.jpg) | **Stock chart** ![Descrizione: Immagine del tipo di grafico azionario](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Elementi del grafico

I grafici mostrano dati ed elementi aggiuntivi come legende, assi, serie e così via. L'immagine seguente mostra molti degli elementi del grafico `Chart` che è possibile personalizzare quando si utilizza l'helper. In questo articolo viene illustrato come impostare alcuni (non tutti) di questi elementi.

![Descrizione: Immagine che mostra gli elementi del grafico](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Creazione di un grafico dai datiCreating a Chart from Data

I dati visualizzati in un grafico possono provengono da una matrice, dai risultati restituiti da un database o dai dati in un file XML.

### <a name="using-an-array"></a>Utilizzo di un array

Come illustrato in [Introduzione alla programmazione di pagine Web ASP.NET utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890), una matrice consente di archiviare un insieme di elementi simili in un'unica variabile. È possibile utilizzare le matrici per contenere i dati che si desidera includere nel grafico.

In questa procedura viene illustrato come creare un grafico dai dati nelle matrici, utilizzando il tipo di grafico predefinito. Viene inoltre illustrato come visualizzare il grafico all'interno della pagina.

1. Creare un nuovo file denominato *ChartArrayBasic.cshtml*.
2. Sostituire il contenuto esistente con quanto segue: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    Il codice crea innanzitutto un nuovo grafico e ne imposta la larghezza e l'altezza. Specificare il titolo del `AddTitle` grafico utilizzando il metodo . Per aggiungere dati, `AddSeries` utilizzare il metodo . In questo esempio vengono `name` `xValue`utilizzati `yValues` i `AddSeries` parametri , e del metodo . Il `name` parametro viene visualizzato nella legenda del grafico. Il `xValue` parametro contiene una matrice di dati visualizzata lungo l'asse orizzontale del grafico. Il `yValues` parametro contiene una matrice di dati utilizzata per tracciare i punti verticali del grafico.

    Il `Write` metodo esegue effettivamente il rendering del grafico. In questo caso, poiché non è stato `Chart` specificato un tipo di grafico, l'helper esegue il rendering del grafico predefinito, ovvero un istogramma.
3. Eseguire la pagina nel browser. Il browser visualizza il grafico. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Utilizzo di una query di database per i dati del graficoUsing a Database Query for Chart Data

Se le informazioni che si desidera creare in un grafico si trova in un database, è possibile eseguire una query di database e quindi utilizzare i dati dei risultati per creare il grafico. In questa procedura viene illustrato come leggere e visualizzare i dati del database creato nell'articolo [Introduzione all'utilizzo di un database in siti di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893).

1. Aggiungere una cartella *\_App Data* alla radice del sito Web se la cartella non esiste già.
2. Nella cartella *App\_Data* aggiungere il file di database denominato *SmallBakery.sdf* descritto in Introduzione [all'utilizzo](https://go.microsoft.com/fwlink/?LinkId=202893)di un database in ASP.NET siti di pagine Web .
3. Creare un nuovo file denominato *ChartDataQuery.cshtml*.
4. Sostituire il contenuto esistente con quanto segue:   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    Il codice apre innanzitutto il database SmallBakery `db`e lo assegna a una variabile denominata . Questa variabile `Database` rappresenta un oggetto che può essere utilizzato per leggere e scrivere nel database. Successivamente, il codice esegue una query SQL per ottenere il nome e il prezzo di ogni prodotto. Il codice crea un nuovo grafico e vi passa la `DataBindTable` query di database chiamando il metodo del grafico. Questo metodo accetta due `dataSource` parametri: il parametro è `xField` per i dati della query e il parametro consente di impostare la colonna di dati utilizzata per l'asse x del grafico.

    In alternativa all'utilizzo del `DataBindTable` metodo `AddSeries` , è `Chart` possibile utilizzare il metodo dell'helper. Il `AddSeries` metodo consente `xValue` di `yValues` impostare i parametri e . Ad esempio, invece `DataBindTable` di usare il metodo in questo modo:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    È possibile `AddSeries` utilizzare il metodo in questo modo:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Entrambi eseguono il rendering degli stessi risultati. Il `AddSeries` metodo è più flessibile perché è possibile specificare `DataBindTable` il tipo di grafico e i dati in modo più esplicito, ma il metodo è più facile da usare se non è necessaria la flessibilità aggiuntiva.
5. Eseguire la pagina in un browser. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>Utilizzo di dati XML

La terza opzione per la creazione di grafici consiste nell'utilizzare un file XML come dati per il grafico. Ciò richiede che il file XML disponga anche di un file di schema (file*con estensione xsd)* che descrive la struttura XML. In questa procedura viene illustrato come leggere i dati da un file XML.

1. Nella *cartella\_App Data* creare un nuovo file XML denominato *data.xml*.
2. Sostituire il codice XML esistente con il codice seguente, ovvero alcuni dati XML relativi ai dipendenti di una società fittizia. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. Nella *cartella\_App Data* creare un nuovo file XML denominato *data.xsd*. Si noti che l'estensione questa volta è *.xsd*.)
4. Sostituire il codice XML esistente con quanto segue: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. Nella radice del sito Web creare un nuovo file denominato *ChartDataXML.cshtml*.
6. Sostituire il contenuto esistente con quanto segue: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    Il codice crea `DataSet` innanzitutto un oggetto. Questo oggetto viene utilizzato per gestire i dati letti dal file XML e organizzarlo in base alle informazioni contenute nel file di schema. (Si noti che la parte `using SystemData`superiore del codice include l'istruzione . Questa operazione è necessaria per poter `DataSet` lavorare con l'oggetto. Per altre informazioni, vedere [ &quot;Utilizzo&quot; ](#SB_UsingStatements) di istruzioni e nomi completi più avanti in questo articolo.)

    Successivamente, il codice `DataView` crea un oggetto basato sul set di dati. La visualizzazione dati fornisce un oggetto che il grafico può associare a &#8212; ovvero leggere e tracciare. Il grafico si associa ai dati utilizzando `AddSeries` il metodo , come illustrato in `xValue` precedenza durante la creazione di un grafico dei dati della matrice, ad eccezione del fatto che questa volta i parametri e `yValues` vengono impostati sull'oggetto `DataView` .

    In questo esempio viene inoltre illustrato come specificare un particolare tipo di grafico. Quando i dati vengono `AddSeries` aggiunti `chartType` nel metodo, il parametro viene impostato anche per visualizzare un grafico a torta.
7. Eseguire la pagina in un browser. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>Istruzioni "Using" e nomi completi
> 
> Il .NET Framework che ASP.NET pagine Web con sintassi Razor è basato su è costituito da molte migliaia di componenti (classi). Per renderlo gestibile per l'utilizzo con tutte queste classi, sono organizzati in *spazi dei nomi*, che sono un po 'come le librerie. Ad esempio, `System.Web` lo spazio dei nomi contiene `System.Xml` classi che supportano la comunicazione browser/server, `System.Data` lo spazio dei nomi contiene classi utilizzate per creare e leggere file XML e lo spazio dei nomi contiene classi che consentono di utilizzare i dati.
> 
> Per accedere a qualsiasi classe specificata in .NET Framework, il codice deve conoscere non solo il nome della classe, ma anche lo spazio dei nomi in cui si trova la classe. Ad esempio, per utilizzare `Chart` l'helper, il `System.Web.Helpers.Chart` codice deve trovare la`System.Web.Helpers`classe , che`Chart`combina lo spazio dei nomi ( ) con il nome della classe ( ). Questo è noto come il nome *completo* della classe &#8212; la sua posizione completa e non ambigua all'interno della vastità di .NET Framework. Nel codice, questo sarebbe simile al seguente:In code, this would look like the following:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> Tuttavia, è ingombrante (e soggetto a errori) dover utilizzare questi nomi lunghi e completi ogni volta che si desidera fare riferimento a una classe o un helper. Pertanto, per semplificare l'utilizzo dei nomi di classe, è possibile *importare* gli spazi dei nomi che ti interessano, che in genere è solo una manciata tra i molti spazi dei nomi in .NET Framework. Se è stato importato uno spazio dei nomi,`Chart`è possibile utilizzare solo`System.Web.Helpers.Chart`un nome di classe ( ) anziché il nome completo ( ). Quando il codice viene eseguito e rileva un nome di classe, può cercare solo gli spazi dei nomi importati per trovare tale classe.
> 
> Quando si utilizza ASP.NET pagine Web con sintassi Razor per creare pagine Web, `WebPage` in genere si utilizza ogni volta lo stesso set di classi, tra cui la classe, i vari helper e così via. Per risparmiare il lavoro di importazione degli spazi dei nomi pertinenti ogni volta che si crea un sito Web, ASP.NET è configurato in modo da importare automaticamente un set di spazi dei nomi principali per ogni sito Web. Ecco perché non hai avuto a che fare con gli spazi dei nomi o l'importazione fino ad ora; tutte le classi con cui hai lavorato si trovano negli spazi dei nomi che sono già stati importati per l'utente.
> 
> Tuttavia, a volte è necessario lavorare con una classe che non si trovi in uno spazio dei nomi importato automaticamente. In tal caso, è possibile utilizzare il nome completo di tale classe oppure importare manualmente lo spazio dei nomi che contiene la classe. Per importare uno spazio `using` dei`import` nomi, utilizzare l'istruzione ( in Visual Basic), come illustrato in un esempio precedente dell'articolo.
> 
> Ad esempio, `DataSet` la classe `System.Data` si trova nello spazio dei nomi . Lo `System.Data` spazio dei nomi non è automaticamente disponibile per ASP.NET pagine Razor.The namespace is not automatically available to ASP.NET Razor pages. Pertanto, per `DataSet` utilizzare la classe utilizzando il nome completo, è possibile utilizzare codice simile al seguente:Therefore, to work with the class using its fully qualified name, you can use code like this:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Se è necessario `DataSet` utilizzare ripetutamente la classe è possibile importare uno spazio dei nomi come questo e quindi utilizzare solo il nome della classe nel codice:If you have to use the class repeatedly you can import a namespace like this and then use just the class name in code:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> È possibile `using` aggiungere istruzioni per qualsiasi altro spazio dei nomi .NET Framework a cui si desidera fare riferimento. Tuttavia, come indicato, non sarà necessario eseguire questa operazione spesso, perché la maggior parte delle classi che si lavorerà con sono in spazi dei nomi che vengono importati automaticamente da ASP.NET per l'utilizzo nelle pagine *.cshtml* e *.vbhtml.*

<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Visualizzazione di grafici all'interno di una pagina Web

Negli esempi visti finora, si crea un grafico e quindi il rendering del grafico viene eseguito direttamente nel browser come elemento grafico. In molti casi, tuttavia, si desidera visualizzare un grafico come parte di una pagina, non solo da solo nel browser. Per farlo richiede un processo in due fasi. Il primo passaggio consiste nel creare una pagina che genera il grafico, come già visto.

Il secondo passaggio consiste nel visualizzare l'immagine risultante in un'altra pagina. Per visualizzare l'immagine, `<img>` si utilizza un elemento HTML, nello stesso modo in cui si visualizza qualsiasi immagine. Tuttavia, anziché fare riferimento a un file `<img>` con estensione *jpg* o *png,* l'elemento fa riferimento al file con estensione *cshtml* che contiene l'helper `Chart` che crea il grafico. Quando viene eseguita `<img>` la pagina di `Chart` visualizzazione, l'elemento ottiene l'output dell'helper ed esegue il rendering del grafico.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Creare un file denominato *ShowChart.cshtml*.
2. Sostituire il contenuto esistente con quanto segue: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    Il codice `<img>` utilizza l'elemento per visualizzare il grafico creato in precedenza nel file *ChartArrayBasic.cshtml.*
3. Eseguire la pagina Web in un browser. Il file *ShowChart.cshtml* visualizza l'immagine del grafico in base al codice contenuto nel file *ChartArrayBasic.cshtml.*

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Applicazione di stili a un grafico

L'helper `Chart` supporta un numero elevato di opzioni che consentono di personalizzare l'aspetto del grafico. È possibile impostare colori, tipi di carattere, bordi e così via. Un modo semplice per personalizzare l'aspetto di un grafico consiste nell'utilizzare un *tema*. I temi sono raccolte di informazioni che specificano come eseguire il rendering di un grafico utilizzando caratteri, colori, etichette, tavolozze ed effetti. Si noti che lo stile di un grafico non indica il tipo di grafico.

Nella tabella seguente sono elencati i temi incorporati.

| Tema | Descrizione |
| --- | --- |
| `Vanilla` | Visualizza le colonne rosse su uno sfondo bianco. |
| `Blue` | Visualizza colonne blu su uno sfondo sfumato blu. |
| `Green` | Visualizza le colonne blu su uno sfondo sfumato verde. |
| `Yellow` | Visualizza le colonne arancioni su uno sfondo sfumato giallo. |
| `Vanilla3D` | Visualizza le colonne rosse 3D su uno sfondo bianco. |

È possibile specificare il tema da utilizzare quando si crea un nuovo grafico.

1. Creare un nuovo file denominato *ChartStyleGreen.cshtml*.
2. Sostituire il contenuto esistente nella pagina con quanto segue:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Questo codice è lo stesso dell'esempio precedente che utilizza `theme` il database `Chart` per i dati, ma aggiunge il parametro quando crea l'oggetto. Di seguito viene illustrato il codice modificato:The following shows the changed code:

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Eseguire la pagina in un browser. Vengono visualizzati gli stessi dati di prima, ma il grafico sembra più lucido: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Salvataggio di un grafico

Quando si `Chart` utilizza l'helper come si è visto finora in questo articolo, l'helper ricrea il grafico da zero ogni volta che viene richiamato. Se necessario, il codice per il grafico esegue nuovamente una query sul database o rilegge il file XML per ottenere i dati. In alcuni casi, questa operazione può essere un'operazione complessa, ad esempio se il database su cui si sta eseguendo la query è di grandi dimensioni o se il file XML contiene molti dati. Anche se il grafico non include molti dati, il processo di creazione dinamica di un'immagine occupa risorse del server e se molte persone richiedono la pagina o le pagine che visualizzano il grafico, potrebbe esserci un impatto sulle prestazioni del tuo sito web.

Per ridurre il potenziale impatto sulle prestazioni della creazione di un grafico, è possibile creare un grafico la prima volta che è necessario e quindi salvarlo. Quando il grafico è necessario di nuovo, anziché rigenerarlo, è sufficiente recuperare la versione salvata ed eseguirne il rendering.

È possibile salvare un grafico nei modi seguenti:You can save a chart in these ways:

- Memorizzare il grafico nella memoria del computer (sul server).
- Salvare il grafico come file di immagine.
- Salvare il grafico come file XML. Questa opzione consente di modificare il grafico prima di salvarlo.

### <a name="caching-a-chart"></a>Memorizzazione nella cache di un graficoCaching a Chart

Dopo aver creato un grafico, è possibile memorizzarlo nella cache. La memorizzazione nella cache di un grafico significa che non deve essere ricreato se deve essere visualizzato nuovamente. Quando si salva un grafico nella cache, si assegna una chiave che deve essere univoca per tale grafico.

I grafici salvati nella cache potrebbero essere rimossi se la memoria del server è insufficiente. Inoltre, la cache viene cancellata se l'applicazione viene riavviata per qualsiasi motivo. Pertanto, il modo standard per lavorare con un grafico memorizzato nella cache consiste nel controllare sempre prima se è disponibile nella cache e, in caso contrario, quindi crearlo o ricrearlo.

1. Nella directory principale del sito Web creare un file denominato *ShowCachedChart.cshtml*.
2. Sostituire il contenuto esistente con quanto segue: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    Il `<img>` tag `src` include un attributo che punta al file *ChartSaveToCache.cshtml* e passa una chiave alla pagina come stringa di query. La chiave contiene &quot;il&quot;valore myChartKey . Il file *ChartSaveToCache.cshtml* contiene l'helper `Chart` che crea il grafico. Questa pagina verrà creata in un attimo.

    Alla fine della pagina è presente un collegamento a una pagina denominata *ClearCache.cshtml*. Questa è una pagina che creerai anche a breve. È necessario il *ClearCache.cshtml* solo per testare la memorizzazione nella cache per questo esempio, non è un collegamento o una pagina che normalmente si include quando si lavora con grafici memorizzati nella cache.
3. Nella directory principale del sito Web creare un nuovo file denominato *ChartSaveToCache.cshtml*.
4. Sostituire il contenuto esistente con quanto segue:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    Il codice controlla innanzitutto se è stato passato qualcosa come valore della chiave nella stringa di query. In tal caso, il codice tenta di leggere `GetFromCache` un grafico dalla cache chiamando il metodo e passandogli la chiave . Se si scopre che non c'è nulla nella cache sotto quella chiave (che sarebbe accaduto la prima volta che viene richiesto il grafico), il codice crea il grafico come al solito. Al termine del grafico, il codice lo salva `SaveToCache`nella cache chiamando . Tale metodo richiede una chiave (in modo che il grafico può essere richiesto in un secondo momento) e la quantità di tempo che il grafico deve essere salvato nella cache. L'ora esatta in cui un grafico deve memorizzare nella cache dipende dalla frequenza con cui si è pensato che i dati che rappresentassero potrebbero cambiare. Il `SaveToCache` metodo richiede `slidingExpiration` anche un parametro &#8212; se è impostato su true, il contatore di timeout viene reimpostato ogni volta che si accede al grafico. In questo caso, in effetti significa che la voce della cache del grafico scade 2 minuti dopo l'ultima volta che un utente ha effettuato l'accesso al grafico. L'alternativa alla scadenza variabile è la scadenza assoluta, il che significa che la voce della cache scadrebbe esattamente 2 minuti dopo essere stata inserita nella cache, indipendentemente dalla frequenza con cui è stato effettuato l'accesso.

    Infine, il codice `WriteFromCache` utilizza il metodo per recuperare ed eseguire il rendering del grafico dalla cache. Si noti che `if` questo metodo è esterno al blocco che controlla la cache, perché otterrà il grafico dalla cache se il grafico era lì per iniziare o doveva essere generato e salvato nella cache.

    Si noti che `AddTitle` nell'esempio, il metodo include un timestamp. Aggiunge la data e l'ora &#8212; `DateTime.Now` &#8212; correnti al titolo.
5. Creare una nuova pagina denominata *ClearCache.cshtml e sostituirne* il contenuto con quanto segue:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    In questa `WebCache` pagina viene utilizzato l'helper per rimuovere il grafico memorizzato nella cache in *ChartSaveToCache.cshtml*. Come notato in precedenza, normalmente non è necessario avere una pagina come questa. Si sta creando qui solo per rendere più facile testare la memorizzazione nella cache.
6. Eseguire la pagina Web *ShowCachedChart.cshtml* in un browser. Nella pagina viene visualizzata l'immagine del grafico in base al codice contenuto nel file *ChartSaveToCache.cshtml.* Prendere nota di ciò che il timestamp dice nel titolo del grafico. 

    ![Descrizione: Immagine del grafico di base con timestamp nel titolo del grafico](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Chiudere il browser.
8. Eseguire nuovamente il *file ShowCachedChart.cshtml.* Si noti che il timestamp è lo stesso di prima, che indica che il grafico non è stato rigenerato, ma è stato invece letto dalla cache.
9. In *ShowCachedChart.cshtml*fare clic sul collegamento **Cancella cache.** Verrà visualizzato *ClearCache.cshtml*, che segnala che la cache è stata cancellata.
10. Fare clic sul collegamento **Torna a ShowCachedChart.cshtml** oppure rieseguire *ShowCachedChart.cshtml* da WebMatrix. Si noti che questa volta il timestamp è stato modificato, perché la cache è stata cancellata. Pertanto, il codice doveva rigenerare il grafico e inserirlo nuovamente nella cache.

### <a name="saving-a-chart-as-an-image-file"></a>Salvataggio di un grafico come file di immagine

È inoltre possibile salvare un grafico come file di immagine (ad esempio, come file *con estensione jpg)* sul server. È quindi possibile utilizzare il file di immagine come si farebbe con qualsiasi immagine. Il vantaggio è che il file viene memorizzato anziché salvato in una cache temporanea. È possibile salvare una nuova immagine del grafico in momenti diversi (ad esempio, ogni ora) e quindi tenere un record permanente delle modifiche che si verificano nel tempo. Si noti che è necessario assicurarsi che l'applicazione web disponga dell'autorizzazione per salvare un file nella cartella sul server in cui si desidera inserire il file di immagine.

1. Nella directory principale del sito Web, creare una cartella denominata * \_ChartFiles* se non esiste già.
2. Nella directory principale del sito Web creare un nuovo file denominato *ChartSave.cshtml*.
3. Sostituire il contenuto esistente con quanto segue:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    Il codice verifica innanzitutto *.jpg* se il file `File.Exists` jpg esiste chiamando il metodo . Se il file non esiste, il `Chart` codice crea una nuova da una matrice. Questa volta, il `Save` codice chiama `path` il metodo e passa il parametro per specificare il percorso del file e il nome del file in cui salvare il grafico. Nel corpo della pagina, `<img>` un elemento utilizza il percorso per puntare al file *jpg* da visualizzare.
4. Eseguire il file *ChartSave.cshtml.*
5. Tornare a WebMatrix. Si noti che un file di immagine denominato *chart01.jpg* è stato salvato nella * \_cartella ChartFiles.*

### <a name="saving-a-chart-as-an-xml-file"></a>Salvataggio di un grafico come file XML

Infine, è possibile salvare un grafico come file XML sul server. Un vantaggio dell'utilizzo di questo metodo rispetto alla memorizzazione nella cache del grafico o il salvataggio del grafico in un file è che è possibile modificare il codice XML prima di visualizzare il grafico, se lo si desidera. L'applicazione deve disporre delle autorizzazioni di lettura/scrittura per la cartella sul server in cui si desidera inserire il file di immagine.

1. Nella directory principale del sito Web creare un nuovo file denominato *ChartSaveXml.cshtml*.
2. Sostituire il contenuto esistente con quanto segue:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Questo codice è simile al codice visualizzato in precedenza per l'archiviazione di un grafico nella cache, ad eccezione del fatto che utilizza un file XML. Il codice verifica innanzitutto se il file `File.Exists` XML esiste chiamando il metodo . Se il file esiste, il `Chart` codice crea un nuovo `themePath` oggetto e passa il nome del file come parametro. In questo modo il grafico viene creato in base a qualsiasi elemento presente nel file XML. Se il file XML non esiste già, il codice crea `SaveXml` un grafico come normale e quindi chiama per salvarlo. Il rendering del `Write` grafico viene eseguito utilizzando il metodo , come si è visto in precedenza.

    Come per la pagina che mostrava la memorizzazione nella cache, questo codice include un timestamp nel titolo del grafico.
3. Creare una nuova pagina denominata ChartDisplayXMLChart.cshtml e aggiungervi il markup seguente:Create a new page named *ChartDisplayXMLChart.cshtml* and add the following markup to it: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Eseguire la pagina *ChartDisplayXMLChart.cshtml.* Viene visualizzato il grafico. Prendere nota del timestamp nel titolo del grafico.
5. Chiudere il browser.
6. In WebMatrix fare clic con il **Refresh** * \_* pulsante destro del mouse sulla cartella ChartFiles , scegliere Aggiorna , quindi aprire la cartella. Il file *XMLChart.xml* in questa `Chart` cartella è stato creato dall'helper. 

    ![Descrizione: la cartella _ChartFiles che mostra il file XMLChart.xml creato dall'helper Chart.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Eseguire nuovamente la pagina *ChartDisplayXMLChart.cshtml.* Il grafico mostra lo stesso timestamp della prima esecuzione della pagina. Questo perché il grafico viene generato dal codice XML salvato in precedenza.
8. In WebMatrix aprire * \_* la cartella ChartFiles ed eliminare il file *XMLChart.xml.*
9. Eseguire nuovamente la pagina *ChartDisplayXMLChart.cshtml.* Questa volta, il timestamp viene `Chart` aggiornato, perché l'helper ha dovuto ricreare il file XML. Se lo si * \_* desidera, controllare la cartella ChartFiles e notare che il file XML è tornato.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione all'utilizzo di un database in siti di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893)
- [Utilizzo della memorizzazione nella cache in ASP.NET siti di pagine Web per migliorare le prestazioniUsing Caching in ASP.NET Web Pages Sites to Improve Performance](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Classe Chart](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) (ASP.NET riferimento all'API Web Pages su MSDN)
