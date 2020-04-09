---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
title: ASP.NET e Web Tools 2012.2 Note sulla versione di Documenti Microsoft
author: rick-anderson
description: Note sulla versione per ASP.NET e Web Tools 2012.2.
ms.author: riande
ms.date: 02/14/2013
ms.assetid: 9534e58b-1d15-4f1d-b04c-10c79b9d8227
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
msc.type: content
ms.openlocfilehash: abd6d8ce0646852a194369589cb730fc98ecb3ad
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676307"
---
# <a name="aspnet-and-web-tools-20122-release-notes"></a>Note sulla versione di ASP.NET and Web Tools 2012.2

> In questo documento viene descritto il rilascio di ASP.NET e Web Tools 2012.2. Si tratta di un aggiornamento di Visual Studio Web Tooling e ASP.NET.

- [Note di installazione](#_Installation)
- [Documentazione](#_Documentation)
- [Supporto](#_Support)
- [Requisiti software](#_Software_Requirements)
- [Nuove funzionalità di ASP.NET e Web Tools 2012.2](#_New_Features_in)

    - [Strumenti](#_Tooling)
    - [pubblicazione sul Web](#_Web_Publishing)
    - [ASP.NET modelli MVC](#_Templates)
    - [API Web ASP.NET](#_ASP.NET_Web_API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [URL brevi di ASP.NET](#_ASP.NET_Friendly_URLs)
- [Problemi noti e modifiche di rilievoKnown Issues and Breaking Changes](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Note di installazione

ASP.NET e Web Tools 2012.2 per Visual Studio 2012 possono essere installati utilizzando il programma di [installazione della piattaforma Web.](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2) Si tratta di un aggiornamento di Visual Studio 2012 o Visual Studio Express 2012 per il Web, che è necessario. Se Visual Studio non è installato, verrà installato Visual Studio Express 2012 per Web.

È inoltre possibile installare ASP.NET e Web Tools 2012.2 manualmente. È necessario disporre di Visual Studio 2012 o Visual Studio Express 2012 per il Web installato. Quindi utilizzare le seguenti istruzioni: 

1. Scaricare il programma di installazione [di ASP.NET e Web Frameworks 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) dall'Area download.
2. Quando richiesto, fare clic su Esegui. È anche possibile salvare il file per eseguirlo in un secondo momento.
3. Verificare la versione di Visual Studio che verrà aggiornata. A tale scopo, avviare Visual Studio che si desidera aggiornare. Quindi fare clic sulla voce di menu Guida.   
    ![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.jpg)
4. Se viene visualizzata &quot;la voce di menu Informazioni&quot; su Microsoft Visual Studio 2012 per Web, scaricare [Web Developer Tools 2012.2 - Visual Studio Express 2012 per Web](https://go.microsoft.com/fwlink/?LinkID=282228). In caso contrario, scaricare [Web Developer Tools 2012.2 - Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Quando richiesto, fare clic su Esegui. È anche possibile salvare il file per eseguirlo in un secondo momento.

> [!NOTE]
> ASP.NET e Web Tools 2012.2 versione non include SQL Server Data Tools. Sql Server e i database SQL di Windows Azure offrono un set più completo di strumenti di database, tra cui lo sviluppo offline supportato dal progetto, il confronto dello schema e le funzionalità avanzate di distribuzione del database. Per ulteriori informazioni o per installare [https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127)SQL Server Data Tools, visitare il sito Web all'indirizzo .

<a id="_Documentation"></a>
## <a name="documentation"></a>Documentazione

Esercitazioni e altre informazioni su ASP.NET e Web Tools 2012.2 sono disponibili sul sito Web di ASP.NET ( https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Supporto

ASP.NET e Web Tools 2012.2 è ufficialmente rilasciato e supportato. È possibile utilizzare il normale canale di supporto. È inoltre possibile pubblicare domande[https://forums.asp.net/](https://forums.asp.net/)nei forum ASP.NET ( ), in cui i membri della comunità ASP.NET sono spesso in grado di fornire supporto informale.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Requisiti software

Il ASP.NET e Web Tools 2012.2 richiede Visual Studio 2012 o Visual Studio Express 2012 per il Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Nuove funzionalità di ASP.NET e Web Tools 2012.2

In questa sezione vengono descritte le funzionalità introdotte nella versione ASP.NET e Web Tools 2012.2.

<a id="_Tooling"></a>
### <a name="tooling"></a>Strumenti

- Controllo pagina 

    - Supporta il mapping di selezione JavaScript che consente a Controllo pagina di mappare gli elementi aggiunti dinamicamente alla pagina al codice JavaScript corrispondente.
    - La possibilità di vedere gli aggiornamenti CSS in tempo reale.
    - Per ulteriori informazioni, leggere [CSS Auto-Sync and JavaScript Selection Mapping in Page Inspector](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Editor 

    - Supportare l'evidenziazione della sintassi per CoffeeScript, Mustache, Handlebars e JsRender.
    - L'editor HTML fornisce Intellisense per le associazioni Knockout.
    - LESS modifica e supporto del compilatore per consentire la compilazione di CSS dinamico utilizzando LESS.
    - Incollare JSON come classe .NET. Usando questo comando Incolla speciale per incollare il file JSON in un file di codice di C o VB.NET e Visual Studio genererà automaticamente le classi .NET dedotte dal codice JSON.
- Il supporto dell'emulatore mobile aggiunge hook di estendibilità in modo che gli emulatori di terze parti possano essere installati come VSIX. Gli emulatori installati verranno visualizzati nel menu a discesa F5, in modo che gli sviluppatori possano visualizzare in anteprima i propri siti Web su una varietà di dispositivi mobili. Per ulteriori informazioni su questa funzionalità, vedere il post di blog di Scott Hanselman sulla [nuova integrazione BrowserStack con Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>pubblicazione sul Web

- I progetti di sito Web hanno ora la stessa esperienza di pubblicazione dei progetti di applicazione Web, inclusa la pubblicazione in siti Web di Windows Azure.
- Pubblicazione selettiva &#8211; per uno o più file è possibile eseguire le azioni seguenti (dopo la pubblicazione in un endpoint di distribuzione Web): 

    - Pubblicare i file selezionati.
    - Vedere la differenza tra un file locale e un file remoto.
    - Aggiornare il file locale con il file remoto o aggiornare il file remoto con il file locale.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>ASP.NET modelli MVC

- Il nuovo modello Applicazione Facebook consente di creare facilmente applicazioni Canvas per Facebook. In pochi e semplici passaggi, è possibile creare un'applicazione per Facebook in grado di ottenere dati da un utente connesso e integrarsi con i relativi contatti. Nel modello è inclusa una nuova libreria per la gestione del plumbing relativo alla creazione di app per Facebook, ad esempio autenticazione, autorizzazioni, accesso ai dati di Facebook e altro ancora, Per ulteriori informazioni sull'utilizzo [https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921)del modello Applicazione Facebook, vedere .
- Un nuovo modello MVC per applicazioni a pagina singola consente agli sviluppatori di creare app Web interattive sul lato client, mediante HTML 5, CSS 3 e le più conosciute librerie JavaScript, Knockout e jQuery, basate su ASP.NET Web API. Il modello include un'applicazione elenco "todo" che illustra le procedure comuni per la compilazione di un'applicazione HTML5 JavaScript che utilizza un'API server RESTful. Puoi leggere di [https://www.asp.net/single-page-application](../../../single-page-application/index.md)più su .
- È ora possibile creare un progetto VSIX che aggiunge nuovi modelli alla finestra di dialogo ASP.NET MVC Nuovo progetto. Scopri come fare qui:[https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- Il pacchetto FixedDisplayModes &#8211; i modelli di progetto MVC sono stati aggiornati per includere il nuovo pacchetto NuGet 'FixedDisplayModes', che contiene una soluzione alternativa per un bug in MVC 4. Per ulteriori informazioni sulla correzione contenuta nel pacchetto,[https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)fare riferimento a questo post di blog ( ) del team MVC.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>API Web ASP.NET

ASP.NET API Web è stata migliorata con diverse nuove funzionalità:

- oData dell'API Web ASP.NET
- Traccia API Web ASP.NET
- pagina della Guida dell'API Web ASP.NET

#### <a name="aspnet-web-api-odata"></a>oData dell'API Web ASP.NET

ASP.NET API Web OData offre la flessibilità necessaria per creare endpoint OData con logica di business avanzata su qualsiasi origine dati. Con ASP.NET API Web OData è possibile controllare la quantità di semantica OData che si desidera esporre. ASP.NET API Web OData è incluso nei modelli di progetto[http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)ASP.NET MVC 4 ed è disponibile anche da NuGet ( ).

ASP.NET API Web OData supporta attualmente le funzionalità seguenti:

- Abilitare la semantica della query OData applicando l'attributo [Queryable].
- Convalida facilmente le query OData e limita il set di opzioni, operatori e funzioni di query supportate.
- Parametro associare a ODataQueryOptions direttamente per ottenere una rappresentazione astratta dell'albero della sintassi della query che può quindi essere convalidata e applicata a un IQueryable o IEnumerable.
- Abilitare il paging basato sul servizio e la generazione dei collegamenti di pagina successiva specificando i limiti dei risultati sull'attributo [Queryable].
- Richiedere un conteggio inline del numero totale di risorse corrispondenti utilizzando $inlinecount.
- Controllare la propagazione null.
- Operatori qualsiasi/tutti in $filter.
- Dedurre un modello di dati di entità per convenzione o personalizzare in modo esplicito un modello in modo simile a Entity Framework Code-First.
- Esporre i set di entità derivando da EntitySetController.Expose entity sets by deriving from EntitySetController.
- Convenzioni semplici e personalizzabili per esporre le proprietà di navigazione, modificare i collegamenti e implementare le azioni OData.Simple, customizable conventions for exposing navigation properties, manipulating links and implementing OData actions.
- Routing semplificato tramite il metodo di estensione MapODataRoute.Simplified routing using the MapODataRoute extension method.
- Supporto per il controllo delle versioni mediante l'esposizione di più modelli EDM.
- Esporre il documento di servizio e $metadata in modo da poter generare client (.NET, Windows Phone, Windows Store e così via) per l'API Web.
- Supporto per i formati dettagliati OData Atom, JSON e JSON.
- Creare, aggiornare, aggiornare parzialmente (PATCH) ed eliminare entità.
- Eseguire query e modificare le relazioni tra le entità.
- Creare collegamenti di relazione che si collegano ai percorsi.
- Tipi complessi.
- Ereditarietà del tipo di entità.
- Proprietà della raccolta.
- Enumerazioni.
- Azioni OData.
- Costruito sulla stessa base di WCF Data ServicesWCF[http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)Data Services, vale a dire ODataLib ( ).

Per ulteriori informazioni su ASP.NET [https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141)API Web OData, vedere .

#### <a name="aspnet-web-api-tracing"></a>Traccia API Web ASP.NET

ASP.NET traccia API Web integra i dati di traccia dalle API Web con la traccia .NET. È ora abilitato per impostazione predefinita nel modello di progetto API Web. I dati di traccia per le API Web vengono inviati alla finestra Output e resi disponibili tramite IntelliTrace.Tracing data for your web APIs is sent to the Output window and is made available through IntelliTrace. ASP.NET web API Tracing consente di tracciare informazioni sull'API Web quando è ospitata in Windows Azure tramite l'integrazione con [Diagnostica di Windows Azure.](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx) È inoltre possibile installare e abilitare ASP.NET traccia di API Web[http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)in qualsiasi applicazione utilizzando il pacchetto NuGet di traccia dell'API Web di ASP.NET ( ).

Per ulteriori informazioni sulla configurazione e l'utilizzo di ASP.NET Web API Tracing, vedere [https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>pagina della Guida dell'API Web ASP.NET

La pagina della Guida dell'API Web ASP.NET è ora inclusa per impostazione predefinita nel modello di progetto API Web. La pagina della Guida dell'API Web ASP.NET genera automaticamente la documentazione per le API Web, inclusi gli endpoint HTTP, i metodi HTTP supportati, i parametri e i payload dei messaggi di richiesta e risposta di esempio. La documentazione viene estratta automaticamente dai commenti nel codice. È inoltre possibile aggiungere la pagina della Guida dell'API Web ASP.NET[http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)a qualsiasi applicazione utilizzando il pacchetto NuGet della pagina della guida dell'API Web ASP.NET ( ).

Per ulteriori informazioni sull'impostazione e la personalizzazione della pagina della Guida dell'API Web ASP.NET, vedere [https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

ASP.NET SignalR semplifica l'aggiunta di funzionalità Web in tempo reale all'applicazione ASP.NET, utilizzando WebSockets se disponibile e ritornando automaticamente ad altre tecniche quando non lo è.

Per ulteriori informazioni sull'utilizzo [https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271)di ASP.NET SignalR, vedere .

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>URL brevi di ASP.NET

ASP.NET FriendlyURLs rende molto facile per gli sviluppatori di moduli web generare URL dall'aspetto più pulito (senza l'estensione aspx). Richiede poca o nessuna configurazione e può essere utilizzato con le applicazioni ASP.NET v4.0 esistenti. La funzionalità FriendlyURLs semplifica inoltre agli sviluppatori l'aggiunta del supporto per dispositivi mobili alle applicazioni, supportando il passaggio tra le visualizzazioni desktop e per dispositivi mobili.

Per ulteriori informazioni sull'installazione e l'utilizzo di ASP.NET URL brevi, vedere [http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievoKnown Issues and Breaking Changes

In questa sezione vengono descritti i problemi noti e le modifiche di rilievo presenti nella versione ASP.NET e Web Tools 2012.2.

### <a name="installation-issues"></a>Problemi di installazione

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Installazioni non in ordine di Visual Studio 2012

L'installazione di uno SKU aggiuntivo di Visual Studio 2012 dopo l'installazione del ASP.NET e Web Tools 2012.2 richiederà un'operazione di ripristino. Considerare la sequenza seguente:

1. Installare Visual Studio 2012 Express per Il Web
2. Installare ASP.NET e Web Tools 2012.2
3. Installare Visual Studio 2012 Professional, Premium o Ultimate

Il passaggio 2 comporterebbe solo l'installazione degli aggiornamenti per Express per il Web. Per assicurarsi che lo SKU aggiuntivo installato durante il passaggio 3 contiene l'aggiornamento è necessario ripristinare il ASP.NET e Web Tools 2012.2 per installare gli aggiornamenti per l'ultimo SKU installato. Ciò vale anche se gli SKU nei passaggi 1 e 3 vengono invertiti.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Installazione di Microsoft ASP.NET e Web Tools 2012.2 all'apertura di Visual Studio

Se VS è aperto durante l'installazione di Microsoft ASP.NET e Web Tools 2012.2, Visual Studio potrebbe finire in uno stato non valido. Si consiglia agli utenti di chiudere tutte le istanze di Visual Studio prima di procedere con l'installazione.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Annullamento dell'installazione di ASP.NET e Web Tools 2012.2 durante l'installazione

L'annullamento dell'installazione di ASP.NET e Web Tools 2012.2 durante l'installazione lascerà Visual Studio in uno stato non valido. Per risolvere questo problema, attenersi alla seguente procedura: 

- Andare a Installazione applicazioni.
- Disinstallare Microsoft ASP.NET e Web Tools 2012.2, se presente.
- Reinstallare Microsoft ASP.NET e Web Tools 2012.2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Dopo la disinstallazione di ASP.NET e Web Tools 2012.2 i modelli ASP.NET MVC 4 e i modelli di sito Web Razor v2 sono mancanti

La disinstallazione di ASP.NET e degli strumenti Web 2012.2 disinstallerà anche tutti ASP.NET modelli di sito Web MVC 4 e Razor v2 da Visual Studio 2012.

La soluzione consiste nel ripristinare l'installazione di Visual Studio 2012 per reinstallare ASP.NET modelli di sito Web MVC 4 e Razor v2.

### <a name="tooling-issues"></a>Problemi relativi agli strumenti

#### <a name="nuget-error-reported-during-project-creation"></a>Errore NuGet segnalato durante la creazione del progetto

Dopo l'installazione di ASP.NET e Web Tools 2012.2 è possibile che venga visualizzato il seguente errore durante la creazione di un progetto MVC 4

![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.png)

Il ASP.NET e Web Tools 2012.2 viene fornito NuGet 2.1 e aggiornerà l'estensione in Visual Studio 2012. In alcuni casi, il programma di installazione VSIX non riuscirà ad aggiornare correttamente il codice VSIX. I seguenti passaggi consentono di risolvere questo problema:

1. Avviare Visual Studio 2012 come amministratore
2. Vai a&gt;Strumenti- Estensioni e aggiornamenti e disinstalla NuGet.
3. Chiudere Visual Studio.
4. Passare alla cartella di installazione di ASP.NET e Web Tools 2012.2:

    1. Per Visual Studio 2012: **Programmi, Microsoft ASP.NET, Stack Web ASP.NET, Visual Studio 2012**
    2. Per Visual Studio 2012 Express per Web: **Programmi, Microsoft ASP.NET, ASP.NET Web Stack, Visual Studio Express 2012 per Web**
5. Fare doppio clic sul file NuGet.Tools.vsix per reinstallare NuGet

### <a name="web-api-issues"></a>Problemi relativi alle API Web

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Problemi di analisi nei valori letterali $filter e DateTime

Il parser URI OData non riesce a analizzare correttamente i valori letterali datetime parziali. Ad esempio, $filter-start eq datetime'2012-12-31T12:00' non viene analizzato correttamente. Una soluzione alternativa consiste nell'utilizzare il valore letterale completo, $filter, start eq datetime'2012-12-31T12:00:00'.

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData non supporta i nomi delle proprietà senza distinzione tra maiuscole e minuscole.

OData non supporta i nomi delle proprietà senza distinzione tra maiuscole e minuscole nelle query OData e nel percorso odata. Vedere elementi di lavoro:See workitems:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Se gli utenti hanno maiuscole e minuscole diverse sul lato client javascript e sul lato server, probabilmente incontreranno questo problema. Questo problema è legato nel protocollo odata. Tuttavia, molti utenti segnalano questo problema. Per aggirarlo, gli utenti devono correggere i propri casi in URL.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>Le convenzioni di routing OData predefinite non supportano POST/PUT nella proprietà di navigazione.

Le convenzioni di routing OData predefinite non supportano POST/PUT nella proprietà di navigazione. Vedere workitem [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366). Questa convenzione comunemente utilizzata nelle convenzioni predefinite manca.

Per risolvere il sistema, gli utenti devono estendere la nuova convenzione di routing per supportarla.

### <a name="facebook-template-issues"></a>Problemi relativi ai modelli di Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>Il modello di applicazione Facebook funziona solo con .NET 4.5

È necessario selezionare .NET 4.5 nell'elenco a discesa del framework nella finestra di dialogo Nuovo progetto per visualizzare il modello di applicazione Facebook in ASP.NET MVC 4.

#### <a name="real-time-update-controller"></a>Controller di aggiornamento in tempo reale

Il modello di applicazione Facebook consente all'utente di creare facilmente un Controller API Web per gestire gli aggiornamenti in tempo reale da Facebook. Se il computer di sviluppo è dietro NAT, il controller potrebbe non funzionare senza ulteriore configurazione di rete. Vedi qui per i dettagli:[http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>I valori delle stringhe di query sono in conflitto con i parametri OAuth di FacebookQuery string values conflict with Facebook OAuth parameters

I seguenti campi sono in conflitto con l'URL di richiamata della finestra di dialogo OAuth di Facebook. Non aggiungere valori di stringa di query personalizzati con\_i nomi\_seguenti: codice, errore, descrizione dell'errore, motivo dell'errore.

#### <a name="using-page-inspector-with-facebook-template"></a>Utilizzo di Controllo pagina con il modello Facebook

Non è possibile utilizzare la funzionalità Controllo pagina in Visual Studio 2012 durante il debug dell'applicazione Facebook.You can't use the Page Inspector feature in Visual Studio 2012 while debugging your Facebook Application. Controllo pagina non supporta attualmente gli iframe.

### <a name="single-page-application-template-issues"></a>Problemi relativi ai modelli di applicazione a pagina singola

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Con JQuery 1.9/Knockout 2.2.1 aggiornamento, quando si esegue il progetto MVC SPA predefinito, nuovo elemento di modifica modifica entrare evento dello stato attivo non viene gestito correttamente.

Con l'aggiornamento JQuery 1.9/Knockout 2.2.1, quando si esegue il progetto MVC SPA predefinito, la modifica del nuovo elemento di attività non sposta più lo stato attivo sulla casella di modifica del nuovo elemento Todo dopo aver immesso il nuovo elemento ToDo nell'elenco delle attività.

Per risolvere [http://knockoutjs.com/documentation/hasfocus-binding.html](http://knockoutjs.com/documentation/hasfocus-binding.html)il problema e apportare una correzione simile al codice di esempio seguente:

File todo.model.js  
 funzione todolist(data), aggiungere quanto segue:  
 **self.isSelected : ko.observable(false);**

todoList.prototype.addTodo, aggiungere il seguente testo nero:  
 **self.isSelected(true);**  
 self.newTodoTitle(&quot;&quot;);

File index.cshtml, aggiungere il seguente testo nero:  
 &lt;form data-bind:&quot;submit: addTodo&quot;&gt;  
 &lt;valore di&quot;tipo&quot; addTodo, tipo&quot;addTodo, text&quot; data-bind:&quot;newTodoTitle, segnaposto: 'Digitare qui per aggiungere', blurOnEnter: true, **hasfocus: isSelected**, evento: sfoca: addTodoTitle.&quot; /&gt;  
 &lt;/forma&gt;
