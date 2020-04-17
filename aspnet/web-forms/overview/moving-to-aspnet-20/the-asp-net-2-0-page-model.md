---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Il modello di pagina ASP.NET 2.0 Documenti Microsoft
author: rick-anderson
description: Nella ASP.NET 1.x, gli sviluppatori hanno scelto tra un modello di codice inline e un modello di codice code-behind. Code-behind potrebbe essere implementato utilizzando il Src attr...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: 6c2435a06d04209db21fb8e075f68ff0b7a9ef7e
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542859"
---
# <a name="the-aspnet-20-page-model"></a>Il modello di pagina di ASP.NET 2.0The ASP.NET 2.0 Page Model

da parte [di Microsoft](https://github.com/microsoft)

> Nella ASP.NET 1.x, gli sviluppatori hanno scelto tra un modello di codice inline e un modello di codice code-behind. Il code-behind può essere implementato utilizzando l'attributo @Page Src o CodeBehind della direttiva. In ASP.NET 2.0, gli sviluppatori hanno ancora una scelta tra codice inline e code-behind, ma sono stati apportati miglioramenti significativi al modello code-behind.

Nella ASP.NET 1.x, gli sviluppatori hanno scelto tra un modello di codice inline e un modello di codice code-behind. Il code-behind può essere implementato utilizzando l'attributo @Page Src o CodeBehind della direttiva. In ASP.NET 2.0, gli sviluppatori hanno ancora una scelta tra codice inline e code-behind, ma sono stati apportati miglioramenti significativi al modello code-behind.

## <a name="improvements-in-the-code-behind-model"></a>Miglioramenti nel modello Code-Behind

Per comprendere appieno le modifiche nel modello code-behind in ASP.NET 2.0, è meglio rivedere rapidamente il modello come esisteva nella ASP.NET 1.x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>Il modello Code-Behind in ASP.NET 1.xThe Code-Behind Model in ASP.NET 1.x

Nella ASP.NET 1.x, il modello code-behind è costituito da un file ASPX (Webform) e un file code-behind contenente codice di programmazione. I due file sono @Page stati collegati utilizzando la direttiva nel file ASPX. Ogni controllo nella pagina ASPX disponeva di una dichiarazione corrispondente nel file code-behind come variabile di istanza. Il file code-behind conteneva anche il codice per l'associazione di eventi e il codice generato necessario per la finestra di progettazione di Visual Studio.The code-behind file also contained code for event binding and generated code necessary for the Visual Studio designer. Questo modello ha funzionato abbastanza bene, ma poiché ogni elemento ASP.NET nella pagina ASPX richiedeva il codice corrispondente nel file code-behind, non vi era alcuna vera separazione di codice e contenuto. Ad esempio, se una finestra di progettazione ha aggiunto un nuovo controllo server a un file ASPX all'esterno dell'IDE di Visual Studio, l'applicazione si interromperebbe a causa dell'assenza di una dichiarazione per tale controllo nel file code-behind.

## <a name="the-code-behind-model-in-aspnet-20"></a>Il modello Code-Behind in ASP.NET 2.0

ASP.NET 2.0 migliora notevolmente questo modello. In ASP.NET 2.0, il code-behind viene implementato utilizzando le nuove *classi parziali* fornite in ASP.NET 2.0. La classe code-behind in ASP.NET 2.0 è definita come una classe parziale, ovvero contiene solo una parte della definizione della classe. La parte rimanente della definizione della classe viene generata dinamicamente da ASP.NET 2.0 utilizzando la pagina ASPX in fase di esecuzione o quando il sito Web è precompilato. Il collegamento tra il file code-behind e la pagina ASPX viene ancora stabilito utilizzando la direttiva di pagina. Tuttavia, anziché un attributo CodeBehind o Src, ASP.NET 2.0 ora utilizza l'attributo CodeFile. L'attributo Inherits viene utilizzato anche per specificare il nome della classe per la pagina.

Una tipica direttiva di pagina di tipo :

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Una definizione di classe tipica in un file code-behind ASP.NET 2.0 potrebbe essere simile alla seguente:A typical class definition in an ASP.NET 2.0 code-behind file might look like this:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> Il linguaggio Visual Basic e visual basic sono gli unici linguaggi gestiti che attualmente supportano le classi parziali. Di conseguenza, gli sviluppatori che utilizzano J , non saranno in grado di utilizzare il modello code-behind in ASP.NET 2.0.

Il nuovo modello migliora il modello code-behind perché gli sviluppatori avranno ora file di codice che contengono solo il codice che hanno creato. Fornisce inoltre una vera separazione di codice e contenuto perché non sono presenti dichiarazioni di variabili di istanza nel file code-behind.

> [!NOTE]
> Poiché la classe parziale per la pagina ASPX è dove avviene l'associazione di eventi, gli sviluppatori Visual Basic possono realizzare un leggero aumento delle prestazioni utilizzando la parola chiave Handles nel code-behind per associare gli eventi. Il linguaggio Cè non dispone di una parola chiave equivalente.

## <a name="new--page-directive-attributes"></a>Nuovi attributi della direttiva page

ASP.NET 2.0 aggiunge molti nuovi attributi per il , direttiva di pagina. Gli attributi seguenti sono nuovi in ASP.NET 2.0.

## <a name="async"></a>Async

L'attributo Async consente di configurare l'esecuzione della pagina in modo asincrono. Bene coprire le pagine asincrone più avanti in questo modulo.

## <a name="asynctimeout"></a>Asynctimeout

Specifica il timeout per le pagine asincrone. Il valore predefinito è 45 secondi.

## <a name="codefile"></a>Codefile

L'attributo CodeFile è la sostituzione dell'attributo CodeBehind in Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

Il CodeFileBaseClass attributo viene utilizzato nei casi in cui si desidera che più pagine da derivare da una singola classe di base. A causa dell'implementazione di classi parziali in ASP.NET, senza questo attributo, una classe di base che utilizza campi comuni condivisi per fare riferimento ai controlli dichiarati in una pagina ASPX non funzionerebbe correttamente perché il motore di compilazione ASP.NET Crea automaticamente nuovi membri in base ai controlli nella pagina. Pertanto, se si desidera una classe base comune per due o più pagine in ASP.NET, è necessario definire specificare la classe di base nel CodeFileBaseClass attributo e quindi derivare ogni classe di pagine da tale classe di base. L'attributo CodeFile è necessario anche quando viene utilizzato questo attributo.

## <a name="compilationmode"></a>Compilationmode

Questo attributo consente di impostare la proprietà CompilationMode della pagina ASPX. La proprietà CompilationMode è un'enumerazione contenente i valori **Always**, **Auto**e **Never**. Il valore predefinito è **Sempre**. L'impostazione **Auto** impedirà ai ASP.NET di compilare dinamicamente la pagina, se possibile. L'esclusione delle pagine dalla compilazione dinamica aumenta le prestazioni. Tuttavia, se una pagina esclusa contiene tale codice che deve essere compilato, verrà generato un errore quando la pagina viene esplorata.

## <a name="enableeventvalidation"></a>Enableeventvalidation

Questo attributo specifica se gli eventi di postback e callback vengono convalidati o meno. Quando questa opzione è abilitata, gli argomenti per gli eventi di postback o callback vengono controllati per verificare che abbiano avuto origine dal controllo server che li ha originariamente sottoposti a rendering.

## <a name="enabletheming"></a>Enabletheming

Questo attributo specifica se ASP.NET temi vengono utilizzati in una pagina. Il valore predefinito è **false**. ASP.NET temi sono trattati nel [Modulo 10.](profiles-themes-and-web-parts.md)

## <a name="linepragmas"></a>LinePragmas

Questo attributo specifica se i pragma di riga devono essere aggiunti durante la compilazione. I pragma di linea sono opzioni utilizzate dai debugger per contrassegnare sezioni specifiche del codice.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback (GestioneScrollPositionOnPostback)

Questo attributo specifica se JavaScript viene inserito nella pagina per mantenere la posizione di scorrimento tra i postback. Questo attributo è **false** per impostazione predefinita.

Quando questo **true**attributo è true &lt;&gt; , ASP.NET aggiungerà un blocco di script al postback simile al seguente:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Si noti che lo src per questo blocco di script è WebResource.axd.Note that the src for this script block is WebResource.axd. Questa risorsa non è un percorso fisico. Quando viene richiesto questo script, ASP.NET compila dinamicamente lo script.

### <a name="masterpagefile"></a>Masterpagefile

Questo attributo specifica il file della pagina master per la pagina corrente. Il percorso può essere relativo o assoluto. Le pagine master sono trattate nel [Modulo 4](master-pages.md).

## <a name="stylesheettheme"></a>Stylesheettheme

Questo attributo consente di eseguire l'override delle proprietà di aspetto dell'interfaccia utente definite da un tema ASP.NET 2.0. I temi sono trattati nel [Modulo 10.](profiles-themes-and-web-parts.md)

## <a name="theme"></a>Tema

Specifica il tema della pagina. Se non viene specificato un valore per l'attributo StyleSheetTheme, l'attributo Theme esegue l'override di tutti gli stili applicati ai controlli nella pagina.

## <a name="title"></a>Titolo

Imposta il titolo per la pagina. Il valore specificato qui &lt;verrà&gt; visualizzato nell'elemento title della pagina di cui è stato eseguito il rendering.

### <a name="viewstateencryptionmode"></a>Viewstateencryptionmode

Imposta il valore per il ViewStateEncryptionMode enumerazione. I valori disponibili sono **Sempre**, **Automatico**e **Mai**. Il valore predefinito è **Auto**. Quando questo attributo è impostato su un valore di **Auto**, viewstate viene crittografato è un controllo che lo richiede chiamando il **RegisterRequiresViewStateEncryption** metodo.

## <a name="setting-public-property-values-via-the--page-directive"></a>Impostazione dei valori delle proprietà pubbliche tramite la direttiva di pagina

Un'altra nuova funzionalità della direttiva di pagina in ASP.NET 2.0 è la possibilità di impostare il valore iniziale delle proprietà pubbliche di una classe base. Si supponga, ad esempio, di disporre di una proprietà pubblica denominata **SomeText** nella classe base e di volerlo inizializzare **su Hello** quando viene caricata una pagina. A tale scopo, è possibile impostare semplicemente il valore nella direttiva di pagina in questo modo:You can accomplish this by simply setting the value in the 'Page directive like so:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

L'attributo **SomeText** della direttiva di pagina imposta il valore iniziale della proprietà SomeText nella classe base su *Hello!*. Il video seguente è una procedura dettagliata dell'impostazione del valore iniziale di una proprietà pubblica in una classe di base utilizzando il Page direttiva.

![](the-asp-net-2-0-page-model/_static/image1.png)

[Apri video a schermo intero](the-asp-net-2-0-page-model/_static/setprop1.wmv)

## <a name="new-public-properties-of-the-page-class"></a>Nuove proprietà pubbliche della classe Page

Le seguenti proprietà pubbliche sono nuove in ASP.NET 2.0.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Restituisce il percorso relativo all'applicazione della pagina o del controllo. Ad esempio, per una http://app/folder/page.aspxpagina che si trova in , la proprietà restituisce il valore di /cartella/.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath (percorso in un confronto tra gli ambienti)

Restituisce il percorso relativo della directory virtuale della pagina o del controllo. Ad esempio, per http://app/folder/page.aspxuna pagina che si trova in , la proprietà restituisce il valore di /folder/page.aspx.

## <a name="asynctimeout"></a>Asynctimeout

Ottiene o imposta il timeout utilizzato per la gestione asincrona delle pagine. Le pagine asincrone verranno trattate più avanti in questo modulo.

## <a name="clientquerystring"></a>ClientQueryString

Proprietà di sola lettura che restituisce la stringa di query dell'URL richiesto. Questo valore è codificato in URL. È possibile utilizzare il metodo UrlDecode della classe HttpServerUtility per decodificarlo.

## <a name="clientscript"></a>ClientScript (script ClientScript)

Questa proprietà restituisce un oggetto ClientScriptManager che può essere utilizzato per gestire l'emissione di script ASP.NETs di script sul lato client. La classe ClientScriptManager viene descritta più avanti in questo modulo.

## <a name="enableeventvalidation"></a>Enableeventvalidation

Questa proprietà controlla se la convalida degli eventi è abilitata per gli eventi di postback e callback. Se abilitato, gli argomenti per gli eventi di postback o callback vengono verificati per garantire che abbiano avuto origine dal controllo server che li ha originariamente sottoposti a rendering.

## <a name="enabletheming"></a>Enabletheming

Questa proprietà ottiene o imposta un Boolean che specifica se un ASP.NET 2.0 tema si applica alla pagina.

## <a name="form"></a>Form

Questa proprietà restituisce il form HTML nella pagina ASPX come oggetto HtmlForm.

## <a name="header"></a>Intestazione

Questa proprietà restituisce un riferimento a un HtmlHead oggetto che contiene l'intestazione di pagina. È possibile utilizzare l'oggetto restituito HtmlHead oggetto per ottenere/impostare fogli di stile, Meta tag e così via.

## <a name="idseparator"></a>IdSeparator (Separatore)

Questa proprietà di sola lettura ottiene il carattere utilizzato per separare gli identificatori di controllo quando ASP.NET sta compilando un ID univoco per i controlli in una pagina. Non è progettato per l'utilizzo diretto dal codice.

## <a name="isasync"></a>IsAsync

Questa proprietà consente pagine asincrone. Le pagine asincrone vengono descritte più avanti in questo modulo.

## <a name="iscallback"></a>IsCallback

Questa proprietà di sola lettura restituisce **true** se la pagina è il risultato di una richiamata. Le richiamate vengono descritte più avanti in questo modulo.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack (informazioni in base alla finestra utente)

Questa proprietà di sola lettura restituisce **true** se la pagina fa parte di un postback tra pagine. I postback tra pagine vengono trattati più avanti in questo modulo.

## <a name="items"></a>Items

Restituisce un riferimento a un IDictionary istanza che contiene tutti gli oggetti archiviati nel contesto di pagine. È possibile aggiungere elementi a questo IDictionary oggetto e saranno disponibili per tutta la durata del contesto.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Questa proprietà controlla se ASP.NET genera JavaScript che mantiene la posizione di scorrimento delle pagine nel browser dopo un postback. (I dettagli di questa proprietà sono stati illustrati in precedenza in questo modulo.)

## <a name="master"></a>Master

Questa proprietà di sola lettura restituisce un riferimento all'istanza di MasterPage per una pagina a cui è stata applicata una pagina master.

## <a name="masterpagefile"></a>Masterpagefile

Ottiene o imposta il nome file della pagina master per la pagina. Questa proprietà può essere impostata solo nel PreInit metodo.

## <a name="maxpagestatefieldlength"></a>Maxpagestatefieldlength

Questa proprietà ottiene o imposta la lunghezza massima per lo stato delle pagine in byte. Se la proprietà è impostata su un numero positivo, lo stato di visualizzazione delle pagine verrà suddiviso in più campi nascosti in modo da non superare il numero di byte specificato. Se la proprietà è un numero negativo, lo stato di visualizzazione non verrà suddiviso in blocchi.

## <a name="pageadapter"></a>Pageadapter

Restituisce un riferimento all'oggetto PageAdapter che modifica la pagina per il browser richiedente.

## <a name="previouspage"></a>Previouspage

Restituisce un riferimento alla pagina precedente nei casi di un Server.Transfer o di un postback tra pagine.

## <a name="skinid"></a>Skinid

Specifica l'ASP.NET lo skin 2.0 da applicare alla pagina.

## <a name="stylesheettheme"></a>Stylesheettheme

Questa proprietà ottiene o imposta il foglio di stile applicato a una pagina.

## <a name="templatecontrol"></a>Templatecontrol

Restituisce un riferimento al controllo contenitore per la pagina.

## <a name="theme"></a>Tema

Ottiene o imposta il nome del ASP.NET 2.0 tema applicato alla pagina. Questo valore deve essere impostato prima del metodo PreInit.

## <a name="title"></a>Titolo

Questa proprietà ottiene o imposta il titolo per la pagina come ottenuto dall'intestazione delle pagine.

## <a name="viewstateencryptionmode"></a>Viewstateencryptionmode

Ottiene o imposta il ViewStateEncryptionMode della pagina. Vedere una descrizione dettagliata di questa proprietà più indietro in questo modulo.

## <a name="new-protected-properties-of-the-page-class"></a>Nuove proprietà protette della classe Page

Di seguito sono riportate le nuove proprietà protette della classe Page in ASP.NET 2.0.

## <a name="adapter"></a>Adattatore

Restituisce un riferimento al ControlAdapter che esegue il rendering della pagina nel dispositivo che lo ha richiesto.

## <a name="asyncmode"></a>AsyncMode

Questa proprietà indica se la pagina viene elaborata in modo asincrono. È destinato all'utilizzo da parte del runtime e non direttamente nel codice.

## <a name="clientidseparator"></a>ClientIDSeparator

Questa proprietà restituisce il carattere utilizzato come separatore durante la creazione di ID client univoci per i controlli. È destinato all'utilizzo da parte del runtime e non direttamente nel codice.

## <a name="pagestatepersister"></a>Pagestatepersister

Questa proprietà restituisce l'oggetto PageStatePersister per la pagina. Questa proprietà viene utilizzata principalmente dagli sviluppatori di controlli ASP.NET.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix (Suffisso univoco di UnFilePath)

Questa proprietà restituisce un suffisso univoco che viene aggiunto al percorso del file per i browser di memorizzazione nella cache. Il valore \_ \_predefinito è ufps e un numero a 6 cifre.

## <a name="new-public-methods-for-the-page-class"></a>Nuovi metodi pubblici per la classe Page

I metodi pubblici seguenti sono nuovi per il Page classe in ASP.NET 2.0.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Questo metodo registra i delegati del gestore eventi per l'esecuzione asincrona della pagina. Le pagine asincrone vengono descritte più avanti in questo modulo.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Applica le proprietà di un foglio di stile di pagina alla pagina.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Questo metodo è un'attività asincrona.

### <a name="getvalidators"></a>GetValidators

Restituisce una raccolta di validator per il gruppo di convalida specificato o il gruppo di convalida predefinito se non ne è stato specificato alcuno.

## <a name="registerasynctask"></a>RegisterAsyncTask

Questo metodo registra una nuova attività asincrona. Le pagine asincrone vengono trattate più avanti in questo modulo.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Questo metodo indica a ASP.NET che lo stato del controllo pagine deve essere reso persistente.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Questo metodo indica a ASP.NET che viewstate delle pagine richiede la crittografia.

## <a name="resolveclienturl"></a>ResolveClientUrl (Risolvere ClientUrl)

Restituisce un URL relativo che può essere utilizzato per le richieste client per le immagini e così via.

## <a name="setfocus"></a>SetFocus

Questo metodo imposterà lo stato attivo sul controllo specificato quando la pagina viene caricata inizialmente.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Questo metodo antuncierà la registrazione del controllo passato come non richiede più la persistenza dello stato del controllo.

## <a name="changes-to-the-page-lifecycle"></a>Modifiche al ciclo di vita della pagina

Il ciclo di vita della pagina in ASP.NET 2.0 non è cambiato drasticamente, ma esistono alcuni nuovi metodi di cui è necessario essere a conoscenza. Il ciclo di vita della pagina ASP.NET 2.0 è descritto di seguito.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (Novità in ASP.NET 2.0)

L'evento PreInit è la prima fase del ciclo di vita a cui uno sviluppatore può accedere. L'aggiunta di questo evento consente di modificare a livello di codice ASP.NET temi 2.0, pagine master, proprietà di accesso per un profilo ASP.NET 2.0 e così via. Se si è in uno stato di postback, è importante rendersi conto che Viewstate non è ancora stato applicato ai controlli a questo punto del ciclo di vita. Pertanto, se uno sviluppatore modifica una proprietà di un controllo in questa fase, verrà probabilmente sovrascritto in un secondo momento nel ciclo di vita delle pagine.

## <a name="init"></a>Init

L'evento Init non è stato modificato da ASP.NET 1.x. Questo è dove si desidera leggere o inizializzare le proprietà dei controlli nella pagina. In questa fase, le pagine mastro, i temi e così via sono già applicati alla pagina.

## <a name="initcomplete-new-in-20"></a>InitComplete (novità della versione 2.0)InitComplete (New in 2.0)

L'evento InitComplete viene chiamato alla fine della fase di inizializzazione delle pagine. A questo punto del ciclo di vita, è possibile accedere ai controlli nella pagina, ma il relativo stato non è ancora stato popolato.

## <a name="preload-new-in-20"></a>PreLoad (Novità in 2.0)

Questo evento viene chiamato dopo l'applicazione di tutti\_i dati di postback e appena prima di Caricamento pagina.

## <a name="load"></a>Caricamento

L'evento Load non è stato modificato da ASP.NET 1.x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (Novità della versione 2.0)LoadComplete (New in 2.0)

L'evento LoadComplete è l'ultimo evento nella fase di caricamento delle pagine. In questa fase, tutti i dati di postback e viewstate sono stati applicati alla pagina.

## <a name="prerender"></a>Prerender

Se si desidera che viewstate venga gestito correttamente per i controlli aggiunti alla pagina in modo dinamico, l'evento PreRender è l'ultima opportunità per aggiungerli.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (Novità della versione 2.0)PreRenderComplete (New in 2.0)

Nella fase PreRenderComplete, tutti i controlli sono stati aggiunti alla pagina e la pagina è pronta per il rendering. L'evento PreRenderComplete è l'ultimo evento generato prima del salvataggio dello stato di visualizzazione delle pagine.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (Novità della versione 2.0)SaveStateComplete (New in 2.0)

Il SaveStateComplete evento viene chiamato immediatamente dopo il salvataggio di tutti gli stati di visualizzazione della pagina e lo stato del controllo. Questo è l'ultimo evento prima che la pagina viene effettivamente eseguito il rendering nel browser.

## <a name="render"></a>Rendering

Il Render metodo non è stato modificato dopo ASP.NET 1.x. Questo è dove il HtmlTextWriter viene inizializzato e la pagina viene eseguito il rendering nel browser.

## <a name="cross-page-postback-in-aspnet-20"></a>Postback tra pagine in ASP.NET 2.0

Nella ASP.NET 1.x, i postback dovevano essere inviati alla stessa pagina. I postback tra pagine non erano consentiti. ASP.NET 2.0 aggiunge la possibilità di eseguire il postback a una pagina diversa tramite il IButtonControl interfaccia. Qualsiasi controllo che implementa la nuova interfaccia IButtonControl (Button, LinkButton e ImageButton oltre ai controlli personalizzati di terze parti) può sfruttare questa nuova funzionalità tramite l'uso dell'attributo PostBackUrl. Il codice seguente viene illustrato un Button controllo che esegue il postback a una seconda pagina.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Quando viene eseguito il postback della pagina, la pagina che avvia il postback è accessibile tramite il PreviousPage proprietà nella seconda pagina. Questa funzionalità viene implementata\_tramite la nuova funzione Lato client WebForm DoPostBackWithOptions che ASP.NET 2.0 esegue il rendering nella pagina quando un controllo esegue il postback a una pagina diversa. Questa funzione JavaScript viene fornita dal nuovo gestore WebResource.axd che genera script al client.

Il video seguente è una procedura dettagliata di un postback tra pagine.

![](the-asp-net-2-0-page-model/_static/image2.png)

[Apri video a schermo intero](the-asp-net-2-0-page-model/_static/xpage1.wmv)

## <a name="more-details-on-cross-page-postbacks"></a>Ulteriori dettagli sui postback tra pagine

### <a name="viewstate"></a>Viewstate

È possibile che si sia già chiesto cosa accade al viewstate dalla prima pagina in uno scenario di postback tra pagine. Dopo tutto, qualsiasi controllo che non implementa IPostBackDataHandler manterrà il proprio stato tramite viewstate, in modo da avere accesso alle proprietà di tale controllo nella seconda pagina di un postback tra pagine, è necessario avere accesso allo stato di visualizzazione per la pagina. ASP.NET 2.0 si occupa di questo scenario utilizzando un \_ \_nuovo campo nascosto nella seconda pagina denominata PREVIOUSPAGE. Il \_ \_campo modulo PREVIOUSPAGE contiene il riquadro di visualizzazione per la prima pagina in modo che sia possibile accedere alle proprietà di tutti i controlli nella seconda pagina.

### <a name="circumventing-findcontrol"></a>Elusione di FindControl

Nella procedura dettagliata video di un postback tra pagine, ho usato il FindControl metodo per ottenere un riferimento al TextBox controllo nella prima pagina. Questo metodo funziona bene a tale scopo, ma FindControl è costoso e richiede la scrittura di codice aggiuntivo. Fortunatamente, ASP.NET 2.0 fornisce un'alternativa a FindControl per questo scopo che funzionerà in molti scenari. La direttiva PreviousPageType consente di avere un riferimento fortemente tipizzato alla pagina precedente utilizzando l'attributo TypeName o VirtualPath. L'attributo TypeName consente di specificare il tipo della pagina precedente, mentre l'attributo VirtualPath consente di fare riferimento alla pagina precedente utilizzando un percorso virtuale. Dopo aver impostato il PreviousPageType direttiva, è quindi necessario esporre i controlli e così via a cui si desidera consentire l'accesso utilizzando le proprietà pubbliche.

## <a name="lab-1-cross-page-postback"></a>Laboratorio 1 Postback tra pagine

In questa esercitazione verrà creata un'applicazione che utilizza la nuova funzionalità di postback tra pagine di ASP.NET 2.0.

1. Aprire Visual Studio 2005 e creare un nuovo sito Web ASP.NET.
2. Aggiungere un nuovo Webform denominato page2.aspx.
3. Aprire il Default.aspx in visualizzazione Progettazione e aggiungere un Button controllo e un TextBox controllo. 

    1. Assegnare al controllo Button un ID **SubmitButton** e il controllo TextBox un ID **UserName**.
    2. Impostare la proprietà PostBackUrl del Button su page2.aspx.
4. Aprire page2.aspx nella visualizzazione Origine.
5. Aggiungere una direttiva PreviousPageType come illustrato di seguito:
6. Aggiungere il codice seguente\_al caricamento della pagina del code-behind di page2.aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Compilare il progetto scegliendo Compila dal menu Compila .
8. Aggiungere il codice seguente al code-behind per Default.aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Modificare il\_caricamento della pagina in page2.aspx nel modo seguente: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Compilare il progetto.
11. Eseguire il progetto.
12. Immettere il proprio nome nella casella di testo e fare clic sul pulsante.
13. Qual è il risultato?

## <a name="asynchronous-pages-in-aspnet-20"></a>Pagine asincrone in ASP.NET 2.0Asynchronous Pages in ASP.NET 2.0

Molti problemi di contesa in ASP.NET sono causati dalla latenza delle chiamate esterne (ad esempio le chiamate al servizio Web o al database), dalla latenza di I/O dei file e così via. Quando viene effettuata una richiesta su un'applicazione ASP.NET, ASP.NET utilizza uno dei relativi thread di lavoro per soddisfare tale richiesta. Tale richiesta è proprietaria di tale thread fino al completamento della richiesta e all'invio della risposta. ASP.NET 2.0 cerca di risolvere i problemi di latenza con questi tipi di problemi aggiungendo la possibilità di eseguire le pagine in modo asincrono. Ciò significa che un thread di lavoro può avviare la richiesta e quindi passare l'esecuzione aggiuntiva a un altro thread, tornando così rapidamente al pool di thread disponibili. Al termine dell'i/o di file, della chiamata al database e così via, viene ottenuto un nuovo thread dal pool di thread per completare la richiesta.

Il primo passaggio per l'esecuzione asincrona di una pagina consiste nell'impostare l'attributo **Async** della direttiva della pagina in questo modo:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Questo attributo indica a ASP.NET di implementare IHttpAsyncHandler per la pagina.

The next step is to call the AddOnPreRenderCompleteAsync method at a point in the lifecycle of the page prior to PreRender. Questo metodo viene in\_genere chiamato in Caricamento pagina. Il metodo AddOnPreRenderCompleteAsync accetta due parametri; un BeginEventHandler e un EndEventHandler. BeginEventHandler restituisce un IAsyncResult che viene quindi passato come parametro per il EndEventHandler.

Il video seguente è una procedura dettagliata di una richiesta di pagina asincrona.

![](the-asp-net-2-0-page-model/_static/image3.png)

[Apri video a schermo intero](the-asp-net-2-0-page-model/_static/async1.wmv)

> [!NOTE]
> Una pagina asincrona non esegue il rendering nel browser fino al completamento di EndEventHandler. Senza dubbio, ma che alcuni sviluppatori penseranno di richieste asincrone come essere simile a callback asincroni. È importante rendersi conto che non lo sono. Il vantaggio delle richieste asincrone è che il primo thread di lavoro può essere restituito al pool di thread per soddisfare le nuove richieste, riducendo in tal modo i conflitti dovuti all'associazione i/O e così via.

## <a name="script-callbacks-in-aspnet-20"></a>Callback di script in ASP.NET 2.0Script Callbacks in ASP.NET 2.0

Gli sviluppatori Web hanno sempre cercato modi per evitare lo sfarfallio associato a un callback. In ASP.NET 1.x, SmartNavigation è stato il metodo più comune per evitare lo sfarfallio, ma SmartNavigation ha causato problemi per alcuni sviluppatori a causa della complessità della sua implementazione sul client. ASP.NET 2.0 risolve questo problema con i callback di script. I callback di script utilizzano XMLHttp per effettuare richieste al server Web tramite JavaScript. La richiesta XMLHttp restituisce dati XML che possono quindi essere modificati tramite il DOM del browser. Il codice XMLHttp è nascosto all'utente dal nuovo gestore WebResource.axd.

Esistono diversi passaggi necessari per configurare un callback di script in ASP.NET 2.0.There are several steps that are necessary in order to configure a script callback in ASP.NET 2.0.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Passaggio 1: Implementare l'interfaccia ICallbackEventHandlerStep 1: Implement the ICallbackEventHandler Interface

Affinché ASP.NET riconosca la pagina come partecipante a un callback di script, è necessario implementare l'interfaccia ICallbackEventHandler. È possibile eseguire questa operazione nel file code-behind in questo modo:You can do this in your code-behind file like so:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

È anche possibile eseguire questa operazione usando la direttiva :</a0>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

In genere si usa la direttiva : Implements quando si usa il codice inline ASP.NET.

## <a name="step-2--call-getcallbackeventreference"></a>Passaggio 2: Chiamare GetCallbackEventReferenceStep 2: Call GetCallbackEventReference

Come accennato in precedenza, la chiamata XMLHttp viene incapsulata nel gestore WebResource.axd. Quando viene eseguito il rendering della pagina, ASP.NET aggiungerà una chiamata a WebForm\_DoCallback, uno script client fornito da WebResource.axd. La funzione\_DoCallback WebForm \_ \_sostituisce la funzione doPostBack per un callback. Tenere \_ \_presente che doPostBack a livello di codice invia il modulo nella pagina. In uno scenario di callback, si desidera \_ \_impedire un postback, pertanto doPostBack non sarà sufficiente.

> [!NOTE]
> \_\_doPostBack viene ancora eseguito il rendering nella pagina in uno scenario di callback di script client. Tuttavia, non viene utilizzato per il callback.

Gli argomenti per\_la funzione lato client WebForm DoCallback vengono forniti tramite la funzione\_lato server GetCallbackEventReference che normalmente verrebbe chiamata in Caricamento pagina. Una chiamata tipica a GetCallbackEventReference potrebbe essere simile alla seguente:A typical call to GetCallbackEventReference might look like this:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> In questo caso, cm è un'istanza di ClientScriptManager. La classe ClientScriptManager verrà descritta più avanti in questo modulo.

Esistono diverse versioni di overload di GetCallbackEventReference.There are several overloaded versions of GetCallbackEventReference. In questo caso, gli argomenti sono i seguenti:

`this`

Un riferimento al controllo in cui viene chiamato GetCallbackEventReference. In questo caso, è la pagina stessa.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Argomento stringa che verrà passato dal codice lato client all'evento sul lato server. In questo caso, Im passando il valore di un elenco a discesa denominato ddlCompany.

`ShowCompanyName`

Nome della funzione lato client che accetterà il valore restituito (come stringa) dall'evento di callback sul lato server. Questa funzione verrà chiamata solo quando il callback sul lato server ha esito positivo. Pertanto, per motivi di robustezza, è in genere consigliabile utilizzare la versione di overload di GetCallbackEventReference che accetta un argomento stringa aggiuntivo che specifica il nome di una funzione lato client da eseguire in caso di errore.

`null`

Stringa che rappresenta una funzione lato client avviata prima del callback al server. In questo caso, non esiste uno script di questo tipo, pertanto l'argomento è null.

`true`

Valore booleano che specifica se eseguire o meno il callback in modo asincrono.

La chiamata a\_WebForm DoCallback sul client passerà questi argomenti. Pertanto, quando viene eseguito il rendering di questa pagina nel client, tale codice sarà simile a questo:Therefore, when this page is rendered on the client, that code will look like so:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Si noti che la firma della funzione sul client è leggermente diversa. La funzione lato client passa 5 stringhe e un valore booleano. La stringa aggiuntiva (che è null nell'esempio precedente) contiene la funzione lato client che gestirà eventuali errori dal callback sul lato server.

## <a name="step-3--hook-the-client-side-control-event"></a>Passaggio 3: Agganciare l'evento di controllo lato clientStep 3: Hook the Client-Side Control Event

Si noti che il valore restituito di GetCallbackEventReference precedente è stato assegnato a una variabile stringa. Tale stringa viene utilizzata per associare un evento sul lato client per il controllo che avvia il callback. In questo esempio, il callback viene avviato da un elenco a discesa nella pagina, pertanto si desidera associare l'evento *OnChange.*

Per associare l'evento lato client, è sufficiente aggiungere un gestore al markup lato client come segue:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Tenere presente che *cbRef* è il valore restituito dalla chiamata a GetCallbackEventReference. Contiene la chiamata a\_WebForm DoCallback illustrata in precedenza.

## <a name="step-4--register-the-client-side-script"></a>Passaggio 4: Registrare lo script lato clientStep 4 : Register the Client-Side Script

Tenere presente che la chiamata a GetCallbackEventReference ha specificato che uno script sul lato client denominato **ShowCompanyName** verrà eseguito quando il callback sul lato server ha esito positivo. Tale script deve essere aggiunto alla pagina utilizzando un ClientScriptManager istanza. La classe ClientScriptManager verrà descritta più avanti in questo modulo. Lo fai così:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Passaggio 5: Chiamare i metodi dell'interfaccia ICallbackEventHandlerStep 5 : Call the Methods of the ICallbackEventHandler Interface

Il ICallbackEventHandler contiene due metodi che è necessario implementare nel codice. Sono **RaiseCallbackEvent** e **GetCallbackEvent**.

**RaiseCallbackEvent** accetta una stringa come argomento e non restituisce alcun valore. L'argomento stringa viene passato dalla chiamata\_sul lato client a WebForm DoCallback. In questo caso, tale valore è l'attributo *value* dell'elenco a discesa denominato ddlCompany. Il codice lato server deve essere inserito nel RaiseCallbackEvent metodo. Ad esempio, se il callback sta effettuando un WebRequest su una risorsa esterna, tale codice deve essere inserito in RaiseCallbackEvent.For example, if your callback is making a WebRequest against an external resource, that code should be placed in RaiseCallbackEvent.

**GetCallbackEvent** è responsabile dell'elaborazione della restituzione del callback al client. Non accetta argomenti e restituisce una stringa. La stringa restituita verrà passata come argomento alla funzione lato client, in questo caso *ShowCompanyName*.

Dopo aver completato i passaggi precedenti, si è pronti per eseguire un callback di script in ASP.NET 2.0.

![](the-asp-net-2-0-page-model/_static/image4.png)

[Apri video a schermo intero](the-asp-net-2-0-page-model/_static/callback1.wmv)

I callback di script in ASP.NET sono supportati in qualsiasi browser che supporti l'esecuzione di chiamate XMLHttp. Questo include tutti i browser moderni in uso oggi. Internet Explorer utilizza l'oggetto ActiveX XMLHttp, mentre altri browser moderni (incluso il prossimo IE 7) utilizzano un oggetto XMLHttp intrinseco. Per determinare a livello di codice se un browser supporta i callback, è possibile utilizzare la proprietà **Request.Browser.SupportCallback.To** programmatically determine if a browser supports callbacks, you can use the Request.Browser.SupportCallback property. Questa proprietà restituirà **true** se il client richiedente supporta i callback di script.

## <a name="working-with-client-script-in-aspnet-20"></a>Utilizzo di script client in ASP.NET 2.0

Gli script client in ASP.NET 2.0 vengono gestiti tramite l'utilizzo della classe ClientScriptManager. La classe ClientScriptManager tiene traccia degli script client utilizzando un tipo e un nome. In questo modo si impedisce che lo stesso script venga inserito a livello di codice in una pagina più di una volta.

> [!NOTE]
> Dopo che uno script è stato registrato correttamente in una pagina, qualsiasi tentativo successivo di registrare lo stesso script comporterà semplicemente la registrazione dello script una seconda volta. Non viene aggiunto alcuno script duplicato e non si verifica alcuna eccezione. Per evitare calcoli non necessari, esistono metodi che è possibile utilizzare per determinare se uno script è già registrato in modo da non tentare di registrarlo più di una volta.

I metodi di ClientScriptManager devono avere familiarità con tutti gli attuali sviluppatori di ASP.NET:

## <a name="registerclientscriptblock"></a>Registerclientscriptblock

Questo metodo aggiunge uno script nella parte superiore della pagina di cui è stato eseguito il rendering. Ciò è utile per l'aggiunta di funzioni che verranno chiamate in modo esplicito sul client.

Esistono due versioni di overload di questo metodo. Tre dei quattro argomenti sono comuni tra loro. ovvero:

`type (string)`

L'argomento ***di tipo*** identifica un tipo per lo script. In genere è una buona idea utilizzare il tipo di pagina (questo. GetType()) per il tipo.

`key (string)`

L'argomento ***chiave*** è una chiave definita dall'utente per lo script. Questo dovrebbe essere univoco per ogni script. Se si tenta di aggiungere uno script con la stessa chiave e lo stesso tipo di uno script già aggiunto, non verrà aggiunto.

`script (string)`

L'argomento ***script*** è una stringa contenente lo script effettivo da aggiungere. Si consiglia di utilizzare un StringBuilder per creare lo script e quindi utilizzare il metodo ToString() su StringBuilder per assegnare l'argomento ***script.***

Se si utilizza l'overload RegisterClientScriptBlock che accetta solo tre&lt;&gt; argomenti, è necessario includere elementi script ( script e &lt;/script&gt;) nello script.

È possibile scegliere di utilizzare l'overload di RegisterClientScriptBlock che accetta un quarto argomento. Il quarto argomento è un valore booleano che specifica se ASP.NET deve aggiungere automaticamente elementi di script. Se questo argomento è **true**, lo script non deve includere gli elementi di script in modo esplicito.

Utilizzare il IsClientScriptBlockRegistered metodo per determinare se uno script è già stato registrato. In questo modo è possibile evitare un tentativo di registrare nuovamente uno script già registrato.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (Novità della versione 2.0)RegisterClientScriptInclude (New in 2.0)

Il tag RegisterClientScriptInclude crea un blocco di script che si collega a un file di script esterno. Ha due overload. Uno prende una chiave e un URL. Il secondo aggiunge un terzo argomento che specifica il tipo.

Ad esempio, il codice seguente genera un blocco di script che si collega a jsfunctions.js nella radice della cartella scripts dell'applicazione:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Questo codice produce il codice seguente nella pagina di cui è stato eseguito il rendering:This code produces the following code in the rendered page:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> Il rendering del blocco di script viene eseguito nella parte inferiore della pagina.

Utilizzare il IsClientScriptIncludeRegistered metodo per determinare se uno script è già stato registrato. In questo modo è possibile evitare un tentativo di registrare nuovamente uno script.

## <a name="registerstartupscript"></a>Registerstartupscript

Il RegisterStartupScript metodo accetta gli stessi argomenti di RegisterClientScriptBlock metodo. Uno script registrato con RegisterStartupScript viene eseguito dopo il caricamento della pagina, ma prima dell'evento OnLoad sul lato client. In 1.X, gli script registrati con RegisterStartupScript&gt; sono stati inseriti appena prima del tag &lt;/form di chiusura,&gt; &lt;mentre gli script registrati con RegisterClientScriptBlock venivano inseriti immediatamente dopo il tag del modulo di apertura. In ASP.NET 2.0, entrambi vengono &lt;posizionati&gt; immediatamente prima del tag closing /form.

> [!NOTE]
> Se si registra una funzione con RegisterStartupScript, tale funzione non verrà eseguita fino a quando non viene chiamata in modo esplicito nel codice lato client.

Utilizzare il IsStartupScriptRegistered metodo per determinare se uno script è già stato registrato ed evitare un tentativo di registrare nuovamente uno script.

## <a name="other-clientscriptmanager-methods"></a>Altri metodi ClientScriptManager

Di seguito sono riportati alcuni degli altri metodi utili della classe ClientScriptManager.

|  <strong>Getcallbackeventreference</strong>   |                                                 Vedere i callback di script più indietro in questo modulo.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                Ottiene un riferimento JavaScript&lt;(javascript: call&gt;) che può essere utilizzato per eseguire il postback da un evento sul lato client.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Ottiene una stringa che può essere utilizzata per avviare un postback dal client.                                    |
|      <strong>GetWebResourceUrl</strong>       | Restituisce un URL di una risorsa incorporata in un assembly. Deve essere utilizzato insieme a <strong>RegisterClientScriptResource</strong>. |
| <strong>RegistraClientScriptResource</strong> |     Registra una risorsa Web con la pagina. Si tratta di risorse incorporate in un assembly e gestite dal nuovo gestore WebResource.axd.      |
|     <strong>RegisterHiddenField (Campo di registrazione)</strong>      |                                                 Registra un campo modulo nascosto con la pagina.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Registra il codice lato client che viene eseguito quando il modulo HTML viene inviato.                                   |
