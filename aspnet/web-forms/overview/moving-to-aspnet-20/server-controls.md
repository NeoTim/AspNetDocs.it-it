---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Controlli server - Documenti Microsoft
author: rick-anderson
description: ASP.NET 2.0 migliora i controlli server in molti modi. In questo modulo verranno illustrate alcune delle modifiche dell'architettura apportate ASP.NET 2.0 e Visual Studio 200...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: 7109f10e87abfadf1e7e08795cf9d3d6bf5df122
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543743"
---
# <a name="server-controls"></a>Controlli server

da parte [di Microsoft](https://github.com/microsoft)

> ASP.NET 2.0 migliora i controlli server in molti modi. In questo modulo verranno illustrate alcune delle modifiche dell'architettura apportate al modo in cui ASP.NET 2.0 e Visual Studio 2005 si occupa dei controlli server.

ASP.NET 2.0 migliora i controlli server in molti modi. In questo modulo verranno illustrate alcune delle modifiche dell'architettura apportate al modo in cui ASP.NET 2.0 e Visual Studio 2005 si occupa dei controlli server.

## <a name="view-state"></a>Stato di visualizzazione

La modifica principale dello stato di visualizzazione in ASP.NET 2.0 è una drastica riduzione delle dimensioni. Si consideri una pagina con solo un controllo Calendar. Ecco lo stato di visualizzazione in ASP.NET 1.1.

[!code-css[Main](server-controls/samples/sample1.css)]

Ora ecco lo stato di visualizzazione su una pagina identica in ASP.NET 2.0.

[!code-css[Main](server-controls/samples/sample2.css)]

Si tratta di una modifica piuttosto significativa e considerando che lo stato di visualizzazione viene portato avanti e indietro in rete, questa modifica può offrire agli sviluppatori un aumento significativo delle prestazioni. La riduzione delle dimensioni dello stato di visualizzazione è in gran parte dovuta al modo in cui lo gestiamo internamente. Tenere presente che lo stato di visualizzazione è una stringa codificata Base64.Remember that view state is a Base64 encoded string. Per comprendere meglio la modifica dello stato di visualizzazione in ASP.NET 2.0, diamo un'occhiata ai valori decodificati dagli esempi precedenti.

Ecco lo stato di visualizzazione 1.1 decodificato:

[!code-css[Main](server-controls/samples/sample3.css)]

Questo può sembrare un po 'incomprensibile, ma c'è uno schema qui. Nella ASP.NET 1.x, abbiamo usato singoli caratteri per identificare &lt; &gt; i tipi di dati e i valori delimitati utilizzando i caratteri. The "t" in the view state sample above represents a Triplet. Il Triplet contiene una coppia di ArrayLists (la "l" rappresenta un ArrayList.) Uno di questi ArrayLists contiene un Int32 ("i") con un valore di 1 e l'altro contiene un altro Triplet. Il Triplet contiene una coppia di ArrayLists e così via. La cosa importante da ricordare è che usiamo Triplette che contengono coppie, identifichiamo i tipi di dati tramite una lettera e usiamo i &lt; caratteri e &gt; come delimitatori.

In ASP.NET 2.0, lo stato di visualizzazione decodificato ha un aspetto leggermente diverso.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Si dovrebbe notare un enorme cambiamento nell'aspetto dello stato di visualizzazione decodificato. Questo cambiamento ha diverse basi architettoniche. Lo stato di visualizzazione in ASP.NET 1.x utilizzato il LosFormatter per serializzare i dati. In 2.0 viene utilizzata la nuova classe ObjectStateFormatter.In 2.0, we use the new ObjectStateFormatter class. Questa classe è stata progettata specificamente per facilitare la serializzazione e la deserializzazione dello stato di visualizzazione e dello stato del controllo. (Lo stato di controllo verrà trattato nella sezione successiva.) La modifica del metodo con cui vengono eseguite la serializzazione e la deserializzazione offre molti vantaggi. Uno dei più drammatici è il fatto che a differenza di LosFormatter che utilizza un TextWriter, il ObjectStateFormatter utilizza un BinaryWriter. Ciò consente a ASP.NET 2.0 per archiviare lo stato di visualizzazione una serie di byte anziché stringhe. Prendiamo, ad esempio, un numero intero. In ASP.NET 1.1, un numero intero richiesto 4 byte di stato di visualizzazione. In ASP.NET 2.0, lo stesso numero intero richiede solo 1 byte. Sono stati apportati altri miglioramenti per ridurre la quantità di stato di visualizzazione archiviato. I valori DateTime, ad esempio, vengono ora archiviati utilizzando un TickCount anziché una stringa.

Come se tutto ciò non bastasse, è stata prestata particolare attenzione al fatto che uno dei maggiori consumer dello stato di visualizzazione in 1.x era il DataGrid e controlli simili. Uno svantaggio importante dei controlli, ad esempio il DataGrid in cui si occupa dello stato di visualizzazione è che spesso contiene grandi quantità di informazioni ripetute. In ASP.NET 1.x, tali informazioni ripetute sono state semplicemente archiviate più e più volte con conseguente uno stato di visualizzazione gonfio. In ASP.NET 2.0, utilizziamo la nuova classe IndexedString per archiviare tali dati. Se una stringa si ripete, è sufficiente archiviare il token per IndexedString e l'indice all'interno di una tabella in esecuzione di IndexedString oggetti.

## <a name="control-state"></a>Stato di controllo

Uno dei principali lamentele che gli sviluppatori avevano con lo stato di visualizzazione è stata la dimensione che ha aggiunto al payload HTTP. Come accennato in precedenza, uno dei maggiori consumer dello stato di visualizzazione è il controllo DataGrid.As previously mentioned, one of the greatest consumers of view state is the DataGrid control. Per evitare l'enorme quantità di stato di visualizzazione generato da un DataGrid, molti sviluppatori semplicemente disabilitato lo stato di visualizzazione per il controllo. Sfortunatamente, quella soluzione non è sempre stata buona. Lo stato di visualizzazione in ASP.NET 1.x contiene non solo i dati necessari per la corretta funzionalità del controllo. Contiene inoltre informazioni sullo stato dell'interfaccia utente del controllo. Ciò significa che se si desidera consentire l'impaginazione in un DataGrid, è necessario abilitare lo stato di visualizzazione anche se non sono necessarie tutte le informazioni dell'interfaccia utente contenute nello stato di visualizzazione. È uno scenario tutto o niente.

In ASP.NET 2.0, lo stato di controllo risolve il problema in modo chiaro tramite l'introduzione dello stato di controllo. Lo stato del controllo contiene i dati assolutamente necessari per la corretta funzionalità di un controllo. A differenza dello stato di visualizzazione, lo stato del controllo non può essere disabilitato. Pertanto, è importante che i dati archiviati nello stato di controllo siano controllati con attenzione.

> [!NOTE]
> Lo stato del \_ \_controllo viene mantenuto insieme allo stato di visualizzazione nel campo del form nascosto VIEWSTATE.

Questo video è una procedura dettagliata dello stato di visualizzazione e dello stato del controllo.

![](server-controls/_static/image1.png)

[Apri video a schermo intero](server-controls/_static/state1.wmv)

Affinché un controllo server di leggere e scrivere nello stato del controllo, è necessario eseguire tre passaggi.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Passaggio 1: Chiamare il metodo RegisterRequiresControlStateStep 1: Call the RegisterRequiresControlState Method

Il RegisterRequiresControlState metodo ASP.NET che un controllo deve mantenere lo stato del controllo. Accetta un argomento di tipo Control che è il controllo che viene registrato.

È importante notare che la registrazione non persiste da richiesta a richiesta. Pertanto, questo metodo deve essere chiamato a ogni richiesta se un controllo deve mantenere lo stato del controllo. È consigliabile chiamare il metodo in OnInit.It is recommended that the method be called in OnInit.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Passaggio 2: Eseguire l'override di SaveControlStateStep 2: Override SaveControlState

Il SaveControlState metodo salva le modifiche di stato del controllo per un controllo dall'ultimo postback. Restituisce un oggetto che rappresenta lo stato del controllo.

## <a name="step-3-override-loadcontrolstate"></a>Passaggio 3: Eseguire l'override di LoadControlStateStep 3: Override LoadControlState

Il metodo LoadControlState carica lo stato salvato in un controllo. Il metodo accetta un argomento di tipo Object che contiene lo stato salvato per il controllo.

## <a name="full-xhtml-compliance"></a>Conformità XHTML completa

Qualsiasi sviluppatore Web conosce l'importanza degli standard nelle applicazioni Web. Per mantenere un ambiente di sviluppo basato su standard, ASP.NET 2.0 è completamente compatibile con XHTML. Pertanto, il rendering di tutti i tag viene eseguito in base agli standard XHTML nei browser che supportano HTML 4.0 o versione successiva.

La definizione DOCTYPE in ASP.NET 1.1 era la seguente:

[!code-html[Main](server-controls/samples/sample6.html)]

In ASP.NET 2.0, la definizione DOCTYPE predefinita è la seguente:

[!code-html[Main](server-controls/samples/sample7.html)]

Se lo si desidera, è possibile modificare la conformità XHTML predefinita tramite il nodo xhtmlConformance nel file di configurazione. Ad esempio, il nodo seguente nel file web.config modificherà la conformità XHTML in XHTML 1.0 Strict:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Se lo si desidera, è anche possibile configurare ASP.NET per l'utilizzo della configurazione legacy utilizzata in ASP.NET 1.x come segue:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Rendering adattivo tramite adattatoriAdaptive Rendering Using Adapters

In ASP.NET 1.x, il &lt;file di&gt; configurazione contiene una sezione browserCaps che ha popolato un Oggetto HttpBrowserCapabilities. Questo oggetto ha consentito a uno sviluppatore di determinare quale dispositivo sta effettuando una particolare richiesta ed eseguire il rendering del codice in modo appropriato. In ASP.NET 2.0, il modello è stato migliorato e ora utilizza la nuova classe ControlAdapter.In ASP.NET 2.0, the model has improved and now uses the new ControlAdapter class. La classe ControlAdapter esegue l'override degli eventi nel ciclo di vita del controllo e controlla il rendering dei controlli in base alle funzionalità dell'agente utente. Le funzionalità di un agente utente specifico sono definite da un file di definizione del browser (un file con estensione browser) memorizzato nel file c: \* \*nella \*cartella Browsers \*.

> [!NOTE]
> La classe ControlAdapter è una classe astratta.

Analogamente &lt;alla&gt; sezione browserCaps in 1.x, il file di definizione del browser utilizza un'espressione regolare per analizzare la stringa agente utente per identificare il browser richiedente. Definisce particolari funzionalità per l'agente utente. Il ControlAdapter esegue il rendering del controllo tramite il Render metodo. Pertanto, se si esegue l'override di Render metodo, non è necessario chiamare Render sulla classe di base. Questa operazione può causare il rendering due volte, una volta per l'adattatore e una volta per il controllo stesso.

## <a name="developing-a-custom-adapter"></a>Sviluppo di un adattatore personalizzatoDeveloping a Custom Adapter

È possibile sviluppare il proprio adattatore personalizzato ereditando da ControlAdapter.You can develop your own custom adapter by inheriting from ControlAdapter. Inoltre, è possibile ereditare dalla classe astratta PageAdapter nei casi in cui è necessario un adattatore per una pagina. Il mapping dei controlli all'adattatore &lt;personalizzato&gt; viene eseguito tramite l'elemento controlAdapters nel file di definizione del browser. Ad esempio, il seguente codice XML da un file di definizione del browser esegue il mapping del controllo Menu alla classe MenuAdapter:

[!code-html[Main](server-controls/samples/sample10.html)]

Utilizzando questo modello, diventa abbastanza facile per uno sviluppatore di controlli di destinazione di un particolare dispositivo o browser. È anche abbastanza semplice per uno sviluppatore avere il controllo completo sul rendering delle pagine su ogni dispositivo.

## <a name="per-device-rendering"></a>Rendering per dispositivo

Le proprietà del controllo server in ASP.NET 2.0 possono essere specificate per dispositivo usando un prefisso specifico del browser. Ad esempio, il codice seguente modificherà il testo di un'etichetta a seconda del dispositivo utilizzato per esplorare la pagina.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Quando la pagina contenente questa etichetta viene visualizzata da Internet Explorer, l'etichetta visualizzerà il testo "Si sta esplorando da Internet Explorer". Quando la pagina viene sfogliata da Firefox, l'etichetta visualizzerà il testo "Stai navigando da Firefox". Quando la pagina viene sfogliata da qualsiasi altro dispositivo, verrà visualizzato il testo "Si sta navigando da un dispositivo sconosciuto". Qualsiasi proprietà può essere specificata utilizzando questa sintassi speciale.

## <a name="setting-focus"></a>Impostazione della messa a fuoco

ASP.NET 1.x gli sviluppatori spesso chiesto come impostare lo stato attivo iniziale su un particolare controllo. Ad esempio, in una pagina di accesso, è utile fare in modo che la casella di testo ID utente otti lo stato attivo quando la pagina viene caricata per la prima volta. In ASP.NET 1.x, questa operazione richiedeva la scrittura di alcuni script sul lato client. Anche se uno script di questo tipo è un'attività semplice, non è più necessario in ASP.NET 2.0 grazie al metodo SetFocus. Il SetFocus metodo accetta un argomento che indica il controllo che deve ricevere lo stato attivo. Questo argomento può essere l'ID client del controllo come stringa o il nome del controllo Server come oggetto Control. Ad esempio, per impostare lo stato attivo iniziale su un controllo TextBox denominato\_txtUserID al primo caricamento della pagina, aggiungere il codice seguente a Caricamento pagina:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

- o

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 utilizza il gestore Webresource.axd (illustrato in precedenza) per eseguire il rendering di una funzione lato client che imposta lo stato attivo. Il nome della funzione lato\_client è WebForm AutoFocus come illustrato di seguito:

[!code-html[Main](server-controls/samples/sample14.html)]

In alternativa, è possibile utilizzare il metodo Focus per un controllo per impostare lo stato attivo iniziale su tale controllo. Il Focus metodo deriva dal Control classe ed è disponibile per tutti i controlli ASP.NET 2.0. È anche possibile impostare lo stato attivo su un particolare controllo quando si verifica un errore di convalida. Che verrà trattato in un modulo successivo.

## <a name="new-server-controls-in-aspnet-20"></a>Nuovi controlli server in ASP.NET 2.0

Di seguito sono riportati i nuovi controlli server in ASP.NET 2.0. Andremo più in dettaglio su alcuni di loro nei moduli successivi.

## <a name="imagemap-control"></a>ImageMap (controllo)

Il controllo ImageMap consente di aggiungere aree sensibili a un'immagine in grado di avviare un postback o passare a un URL. Sono disponibili tre tipi di hotspot; CircleHotSpot, RectangleHotSpot e PolygonHotSpot. Gli hotspot vengono aggiunti tramite un editor di raccolta in Visual Studio o a livello di codice nel codice. Non è disponibile alcuna interfaccia utente per disegnare aree sensibili su un'immagine. Le coordinate e le dimensioni o il raggio dell'hotspot devono essere specificati in modo dichiarativo. Non esiste inoltre alcuna rappresentazione visiva di un'area sensibile nella finestra di progettazione. Se un'area sensibile è configurata per passare a un URL, l'URL viene specificato tramite la proprietà NavigateUrl dell'area sensibile. Nel caso di un'area sensibile di postback, la proprietà PostBackValue consente di passare una stringa nel postback che può essere recuperata nel codice lato server.

![HotSpot Collection Editor in Visual Studio](server-controls/_static/image1.jpg)

**Figura 1:** Editor dell'insieme HotSpot in Visual Studio

## <a name="bulletedlist-control"></a>Controllo BulletedList

Il BulletedList controllo è un elenco puntato che può essere facilmente associato a dati. L'elenco può essere ordinato (numerato) o non ordinato tramite il BulletStyle proprietà. Ogni elemento nell'elenco è rappresentato da un ListItem oggetto.

![Controllo BulletedList in Visual Studio](server-controls/_static/image1.gif)

**Figura 2:** controllo BulletedList in Visual StudioFigure 2: BulletedList Control in Visual Studio

## <a name="hiddenfield-control"></a>Controllo HiddenField

Il controllo HiddenField aggiunge un campo modulo nascosto alla pagina, il cui valore è disponibile nel codice lato server. Il valore di un campo modulo nascosto è generalmente previsto per rimanere invariato tra i postback. Tuttavia, è possibile che un utente malintenzionato modifichi il valore prima del postback. In questo caso, il HiddenField controllo genererà il ValueChanged evento. Se si dispone di informazioni riservate nel HiddenField controllo e si desidera assicurarsi che rimanga invariato, è necessario gestire il ValueChanged evento nel codice.

## <a name="fileupload-control"></a>Controllo FileUpload

Il controllo FileUpload in ASP.NET 2.0 consente di caricare file su un server Web tramite una pagina ASP.NET. Questo controllo è molto simile alla classe ASP.NET 1.x HtmlInputFile con alcune eccezioni. In ASP.NET 1.x, è consigliabile che il PostedFile proprietà deve essere controllato per null per determinare se si dispone di un file valido. Il controllo FileUpload in ASP.NET 2.0 aggiunge una nuova proprietà HasFile che è possibile utilizzare per lo stesso scopo ed è un po' più efficiente.

Il PostedFile proprietà è ancora disponibile per l'accesso a un HttpPostedFile oggetto, ma alcune delle funzionalità del HttpPostedFile è ora disponibile intrinsecamente con il FileUpload controllo. Ad esempio, per salvare un file caricato in ASP.NET 1.x, chiamare il SaveAs metodo il HttpPostedFile oggetto. Utilizzando il controllo FileUpload in ASP.NET 2.0, è necessario chiamare il SaveAs metodo sul FileUpload stesso.

Un'altra modifica significativa nel comportamento 2.0 (e probabilmente la modifica più significativa) è che non è più necessario caricare un intero file caricato in memoria prima di salvarlo. In 1.x, qualsiasi file caricato viene salvato interamente in memoria prima di essere scritto su disco. Questa architettura impedisce il caricamento di file di grandi dimensioni.

In ASP.NET 2.0, l'attributo requestLengthDiskThreshold dell'elemento httpRuntime consente di configurare il numero di Kilobyte mantenuti in un buffer in memoria prima di essere scritti su disco.

**IMPORTANTE**: la documentazione MSDN (e la documentazione altrove) specifica che questo valore è in byte (non Kilobyte) e che il valore predefinito è 256. Il valore è effettivamente specificato in Kilobyte e il valore predefinito è 80. Avendo un valore predefinito di 80K, ci assicuriamo che il buffer non finisca nell'heap oggetti grandi.

## <a name="wizard-control"></a>Controllo Wizard

È abbastanza comune incontrare ASP.NET sviluppatori alle prese con il tentativo di raccogliere informazioni in una serie di "pagine" utilizzando pannelli o trasferendoda da una pagina all'altra. Il più delle volte, lo sforzo è frustrante e richiede tempo. Il nuovo controllo Wizard risolve i problemi consentendo passaggi lineari e non lineari in un'interfaccia guidata con cui gli utenti hanno familiarità. Il controllo Wizard presenta i form di input in una serie di passaggi. Ogni passaggio è di un particolare tipo specificato dal StepType proprietà del controllo. I tipi di passaggio disponibili sono i seguenti:

| **Tipo di passo** | **Spiegazione** |
| --- | --- |
| Auto | La procedura guidata determina automaticamente il tipo di passaggio in base alla posizione all'interno della gerarchia dei passaggi. |
| Inizia | Il primo passo, spesso usato per presentare una dichiarazione introduttiva. |
| Passaggio | Un passo normale. |
| Finish | Il passaggio finale, in genere utilizzato per presentare un pulsante per completare la procedura guidata. |
| Operazione completata | Presenta un messaggio che comunica l'esito positivo o negativo. |

> [!NOTE]
> Il controllo Wizard tiene traccia del proprio stato utilizzando ASP.NET stato del controllo. Pertanto, il EnableViewState proprietà può essere impostata su false senza alcun danno.

Questo video è una procedura dettagliata del controllo Wizard.This video is a walkthrough of the Wizard control.

![](server-controls/_static/image2.png)

[Apri video a schermo intero](server-controls/_static/wizard1.wmv)

## <a name="localize-control"></a>Localizzare il controlloLocalize Control

Il controllo Localize è simile a un controllo Literal.The Localize control is similar to a Literal control. Tuttavia, il Localize controllo dispone di un **Mode** proprietà che controlla come viene eseguito il rendering del markup che viene aggiunto ad esso. La proprietà Mode supporta i valori seguenti:The Mode property supports the following values:

| **Modalità** | **Spiegazione** |
| --- | --- |
| Trasformare | Il markup viene trasformato in base al protocollo del browser che effettua la richiesta. |
| Passthrough | Il rendering del markup viene eseguito così com'è. |
| Codificare | Il markup aggiunto al controllo viene codificato utilizzando HtmlEncode. |

## <a name="multiview-and-view-controls"></a>Controlli MultiView e View

Il MultiView controllo funge da contenitore per View controlli e il View controllo funge da contenitore (molto simile a un Panel controllo) per altri controlli. Ogni visualizzazione in un MultiView controllo è rappresentato da un singolo View controllo. Il primo controllo View nel MultiView è la visualizzazione 0, il secondo è la visualizzazione 1 e così via. È possibile passare da una visualizzazione all'altra specificando ActiveViewIndex del controllo MultiView.

## <a name="substitution-control"></a>Controllo della sostituzione

Il controllo Sostituzione viene utilizzato insieme alla memorizzazione nella cache di ASP.NET. Nei casi in cui si desidera sfruttare la memorizzazione nella cache, ma si dispone di parti di una pagina che devono essere aggiornate a ogni richiesta (in altre parole, parti di una pagina che sono esenti dalla memorizzazione nella cache), il componente di sostituzione fornisce un'ottima soluzione. Il controllo non esegue effettivamente il rendering di alcun output da solo. Al contrario, è associato a un metodo nel codice lato server. Quando viene richiesta la pagina, viene chiamato il metodo e viene eseguito il rendering del markup restituito al posto del controllo di sostituzione.

Il metodo a cui è associato il Substitution controllo viene specificato tramite il **MethodName** proprietà. Tale metodo deve soddisfare i seguenti criteri:

- Deve essere un metodo statico (condiviso in VB).
- Accetta un parametro di tipo HttpContext.
- Restituisce una stringa che rappresenta il markup che deve sostituire il controllo nella pagina.

Il Substitution controllo non è in grado di modificare qualsiasi altro controllo nella pagina, ma ha accesso all'oggetto corrente HttpContext tramite il relativo parametro.

## <a name="gridview-control"></a>Controllo GridView

Il controllo GridView è la sostituzione del controllo DataGrid. Questo controllo verrà trattato in modo più dettagliato in un modulo successivo.

## <a name="detailsview-control"></a>Controllo DetailsView

Il controllo DetailsView consente di visualizzare un singolo record da un'origine dati e di modificarlo o eliminarlo. Viene trattato in modo più dettagliato in un modulo successivo.

## <a name="formview-control"></a>Controllo FormView

Il FormView controllo viene utilizzato per visualizzare un singolo record da un'origine dati in un'interfaccia configurabile. Viene trattato in modo più dettagliato in un modulo successivo.

## <a name="accessdatasource-control"></a>Controllo AccessDataSource

Il controllo AccessDataSource viene utilizzato per associare dati a un database di Access. Viene trattato in modo più dettagliato in un modulo successivo.

## <a name="objectdatasource-control"></a>Controllo ObjectDataSource

Il ObjectDataSource controllo viene utilizzato per supportare un'architettura a tre livelli in modo che i controlli possono essere associati a dati a un oggetto business di livello intermedio anziché un modello a due livelli in cui i controlli sono associati direttamente all'origine dati. Sarà discusso in modo più dettagliato in un modulo successivo.

## <a name="xmldatasource-control"></a>Controllo XmlDataSource

Il xmlDataSource controllo viene utilizzato per l'associazione dati a un'origine dati XML. Viene trattato in modo più dettagliato in un modulo successivo.

## <a name="sitemapdatasource-control"></a>Controllo SiteMapDataSource

Il controllo SiteMapDataSource fornisce l'associazione dati per i controlli di spostamento nel sito in base a una mappa del sito. Sarà discusso in modo più dettagliato in un modulo successivo.

## <a name="sitemappath-control"></a>Controllo SiteMapPath

Il controllo SiteMapPath visualizza una serie di collegamenti di spostamento comunemente denominati breadcrumb. Viene trattato in modo più dettagliato in un modulo successivo.

## <a name="menu-control"></a>Controllo Menu

Il controllo Menu visualizza i menu dinamici utilizzando DHTML. Viene trattato in modo più dettagliato in un modulo successivo.

## <a name="treeview-control"></a>Controllo TreeView

Il controllo TreeView viene utilizzato per visualizzare una visualizzazione ad albero gerarchica dei dati. Viene trattato in modo più dettagliato in un modulo successivo.

## <a name="login-control"></a>Controllo di accesso

Il controllo Login fornisce un meccanismo per accedere a un sito Web. Viene trattato in modo più dettagliato in un modulo successivo.

## <a name="loginview-control"></a>Controllo LoginView

Il controllo LoginView consente la visualizzazione di diversi modelli in base allo stato di accesso dell'utente. Viene trattato in modo più dettagliato in un modulo successivo.

## <a name="passwordrecovery-control"></a>Controllo PasswordRecovery

Il controllo PasswordRecovery viene utilizzato per recuperare le password dimenticate dagli utenti di un'applicazione ASP.NET. Viene trattato in modo più dettagliato in un modulo successivo.

## <a name="loginstatus"></a>LoginStatus

Il controllo LoginStatus visualizza lo stato di accesso di un utente. Viene trattato in modo più dettagliato in un modulo successivo.

## <a name="loginname"></a>LoginName

Il controllo LoginName visualizza il nome utente di un utente dopo l'accesso a un'applicazione ASP.NET. Viene trattato in modo più dettagliato in un modulo successivo.

## <a name="createuserwizard"></a>Createuserwizard

Il CreateUserWizard è una procedura guidata configurabile che offre agli utenti la possibilità di creare un account di appartenenza ASP.NET da utilizzare in un'applicazione ASP.NET. Viene trattato in modo più dettagliato in un modulo successivo.

## <a name="changepassword"></a>ChangePassword

Il controllo ChangePassword consente agli utenti di modificare la password per un'applicazione ASP.NET. Viene trattato in modo più dettagliato in un modulo successivo.

## <a name="various-webparts"></a>Varie WebParts

ASP.NET 2.0 viene fornito con varie web part. Questi verranno trattati in dettaglio in un modulo successivo.
