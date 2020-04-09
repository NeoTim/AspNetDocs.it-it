---
uid: mvc/mvc3
title: ASP.NET MVC 3
author: rick-anderson
description: (include l'aggiornamento degli strumenti di aprile 2011) ASP.NET MVC 3 è un framework per la creazione di applicazioni Web scalabili e basate su standard utilizzando un modello di progettazione ben consolidato...
ms.author: riande
ms.date: 10/05/2010
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 6fe734dc0d0106bb346df154e1e03f97b307567a
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675143"
---
# <a name="aspnet-mvc-3"></a>ASP.NET MVC 3

> *(include l'aggiornamento degli strumenti di aprile 2011)*
> 
> ASP.NET MVC 3 è un framework per la creazione di applicazioni Web scalabili e basate su standard utilizzando modelli di progettazione consolidati e la potenza di ASP.NET e .NET Framework.
> 
> Si installa side-by-side con ASP.NET MVC 2, quindi inizia a usarlo oggi stesso!
> 
> Scarica il [programma di installazione qui](https://microsoft.com/download/details.aspx?id=4211)

## <a name="top-features"></a>Caratteristiche principali

- Sistema di scaffolding integrato estensibile tramite NuGet
- Modelli di progetto abilitati per HTML 5
- Viste espressive, incluso il nuovo motore di visualizzazione Razor
- Potenti ganci con Dependency Injection e filtri azione globale
- Supporto JavaScript avanzato con javaScript discreto, convalida jQuery e associazione JSON
- *Leggi l'elenco completo delle funzionalità [qui sotto](#overview)*

## <a name="top-links"></a>Collegamenti principali

Novità di ASP.NET MVC 3

- Phil Haack: [ASP.NET MVC 3 Rilasciato](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MVC3, WebMatrix, NuGet, IIS Express e Frutteto rilasciato - La microsoft gennaio Web Release nel contesto](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [Annuncio del rilascio di ASP.NET MVC 3, IIS Express, SQL CE 4, Web Farm Framework, Orchard, WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Note sulla versione per ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Installazione e Guida

- Installare ASP.NET MVC 3 utilizzando [l'Installazione guidata piattaforma Web (scelta consigliata)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Installare ASP.NET MVC 3 utilizzando [l'eseguibile del programma di installazione](https://go.microsoft.com/fwlink/?LinkID=208140)
- Installare [ASP.NET MVC 3 per Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Leggere [l'esercitazione Introduzione a ASP.NET MVC 3](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Ottenere assistenza e discutere ASP.NET MVC 3 nei [forum](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>ASP.NET MVC 3 Overview

ASP.NET MVC 3 si basa su ASP.NET MVC 1 e 2, aggiungendo grandi funzionalità che semplificano il codice e consentono un'estendibilità più approfondita. In questo argomento viene fornita una panoramica di molte delle nuove funzionalità incluse in questa versione, organizzate nelle sezioni seguenti:

- [Scaffolding estensibile con l'integrazione di MvcScaffoldExtensible Scaffolding with MvcScaffold integration](#BM_MvcScaffolding)
- [Modelli di progetto abilitati per HTML 5](#BM_HTML5)
- [Il motore di visualizzazione RazorThe Razor View Engine](#BM_TheRazorViewEngine)
- [Supporto per più motori di visualizzazioneSupport for Multiple View Engines](#BM_Support_for_Multiple_View_Engines)
- [Miglioramenti del controller](#BM_Controller_Improvements)
- [JavaScript e Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Miglioramenti della convalida del modello](#BM_Model_Validation_Improvements)
- [Miglioramenti dell'inserimento delle dipendenzeDependency Injection Improvements](#BM_Dependency_Injection_Improvements)
- [Altre nuove funzionalità](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Scaffolding estensibile con l'integrazione di MvcScaffoldExtensible Scaffolding with MvcScaffold integration

Il nuovo sistema di scaffolding rende più facile raccogliere e iniziare a utilizzare in modo produttivo se sei completamente nuovo al framework e per automatizzare le attività di sviluppo comuni se hai esperienza e sai già cosa stai facendo.

Questa operazione è supportata dal nuovo pacchetto di *scaffolding* NuGet denominato **MvcScaffolding**. Il termine "Scaffolding" è usato da molte tecnologie software per significare "generare rapidamente una struttura di base del software che è quindi possibile modificare e personalizzare". Il pacchetto di scaffolding che stiamo creando per ASP.NET MVC è molto utile in diversi scenari:The scaffolding package we're creating for ASP.NET MVC is greatly beneficial in several scenarios:

- **Se stai imparando ASP.NET MVC per la prima volta,** perché ti dà un modo rapido per ottenere un codice utile e funzionante, che puoi quindi modificare e adattare in base alle tue esigenze. Ti salva dal trauma di guardare una pagina vuota e non avere idea da dove cominciare!
- **Se si conosce bene ASP.NET MVC e ora si sta esplorando una nuova tecnologia** del componente aggiuntivo, ad esempio un mappatore relazionale a oggetti, un motore di visualizzazione, una libreria di test e così via, perché l'autore di tale tecnologia potrebbe aver creato anche un pacchetto di scaffolding per tale pacchetto.
- **Se il lavoro prevede ripetutamente la creazione di classi o file simili di qualche tipo,** poiché è possibile creare scaffolder personalizzate che generano dispositivi di test, script di distribuzione o qualsiasi altra cosa necessaria. Tutti i membri del tuo team possono utilizzare anche le tue impalcature personalizzate.

Altre funzionalità di MvcScaffolding includono:Other features in MvcScaffolding include:

- Supporto per i progetti in C e VB
- Supporto per i motori di visualizzazione Razor e ASPX
- Supporta lo scaffolding in aree MVC ASP.NET e l'utilizzo di layout/master di visualizzazione personalizzatiSupports scaffolding into ASP.NET MVC areas and using custom view layouts/masterss
- È possibile personalizzare facilmente l'output modificando i modelli T4
- È possibile aggiungere scaffolders completamente nuovi usando la logica di PowerShell personalizzata e modelli T4 personalizzati. Questi (e tutti i parametri personalizzati che hai dato loro) vengono visualizzati automaticamente nell'elenco di completamento della scheda della console.
- È possibile ottenere pacchetti NuGet contenenti scaffolders aggiuntivi per diverse tecnologie (ad esempio, c'è un proof-of-concept uno per LINQ to SQLLINQ to SQL ora) e mescolare e li combinano insieme

Il ASP.NET di aggiornamento degli strumenti MVC 3 include un ottimo supporto di Visual Studio per questo sistema di scaffolding, ad esempio:The network MVC 3 Tools Update includes great Visual Studio support for this scaffolding system, such as:

- Aggiungi finestra di dialogo controller supporta ora lo scaffolding automatico completo delle azioni create, Read, Update e Delete del controller e delle visualizzazioni corrispondenti. Per impostazione predefinita, questo scaffolding codice di accesso ai dati utilizzando EF Code First.
- Aggiungi finestra di dialogo del controller supporta *scaffold estensibili* tramite pacchetti NuGet, ad esempio *MvcScaffolding*. Ciò consente di collegare scaffold personalizzati nella finestra di dialogo che consente di creare scaffold per altre tecnologie di accesso ai dati, ad esempio NHibernate o anche JET con ODBCDirect, se si è così inclini!

Per ulteriori informazioni su Scaffolding in ASP.NET MVC 3, vedere le risorse seguenti:

- La serie postale di Steve Sanderson, tra cui: 

    1. [Introduzione: Scaffold il ASP.NET progetto MVC 3 con il pacchetto MvcScaffolding](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Utilizzo standard: casi d'uso e opzioni tipiche](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Relazioni uno-a-molti](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Azioni di scaffolding e unit test](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Sostituzione dei modelli T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Questo post: Creazione di scaffolders personalizzati](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Post di Scott Hanselman dalla sua sessione PDC 2010 [Costruire un blog con Microsoft "Unnamed Package of Web Love"](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>Modelli di progetto HTML 5

La finestra di dialogo Nuovo progetto include una casella di controllo per abilitare le versioni HTML 5 dei modelli di progetto. Questi modelli sfruttano Modernizr 1.7 per fornire supporto compatibilità per HTML 5 e CSS 3 nei browser di livello inferiore.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Il motore di visualizzazione RazorThe Razor View Engine

ASP.NET MVC 3 viene fornito con un nuovo motore di visualizzazione denominato Razor che offre i vantaggi seguenti:

- La sintassi del rasoio è pulita e concisa e richiede un numero minimo di sequenze di tasti.
- Razor è facile da imparare, in parte perché è basato su linguaggi esistenti come C .
- Visual Studio include IntelliSense e la colorazione del codice per la sintassi Razor.Visual Studio includes IntelliSense and code colorization for Razor syntax.
- Le visualizzazioni Razor possono essere sottoposte a unit test senza richiedere l'esecuzione dell'applicazione o l'avvio di un server Web.

Alcune nuove funzionalità di Razor includono quanto segue:

- `@model`sintassi per specificare il tipo passato alla visualizzazione.
- `@* *@`sintassi dei commenti.
- La possibilità di specificare `layoutpage`i valori predefiniti (ad esempio ) una volta per un intero sito.
- Metodo `Html.Raw` per la visualizzazione del testo senza codifica HTML.
- Supporto per la condivisione di codice tra più visualizzazioni (*\_viewstart.cshtml* o * \_viewstart.vbhtml).*

Razor include anche nuovi helper HTML, ad esempio:

- `Chart`. Esegue il rendering di un grafico, offrendo le stesse funzionalità del controllo grafico in ASP.NET 4.
- `WebGrid`. Esegue il rendering di una griglia di dati, completa di funzionalità di paging e ordinamento.
- `Crypto`. Utilizza algoritmi hash per creare password con salt e hash correttamente.
- `WebImage`. Esegue il rendering di un'immagine.
- `WebMail`. Invia un messaggio di posta elettronica.

Per altre informazioni su Razor, vedere le risorse seguenti:For more information about Razor, see the following resources:

- [Il post sul blog di Scott Guthrie che introduce Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Post sul blog di Scott Guthrie che introduce la @model parola chiave](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Il post sul blog di Scott Guthrie che introduce i layout Razor](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Guida di riferimento rapido all'API RazorRazor API Quick Reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Supporto per più motori di visualizzazioneSupport for Multiple View Engines

La finestra di dialogo **Aggiungi visualizzazione** in ASP.NET MVC 3 consente di scegliere il motore di visualizzazione che si desidera utilizzare e la finestra di dialogo **Nuovo progetto** consente di specificare il motore di visualizzazione predefinito per un progetto. È possibile scegliere il motore di visualizzazione Web Form (ASPX), Razor o un motore di visualizzazione open source, ad esempio [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/)o [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Miglioramenti del controller

### <a name="global-action-filters"></a>Filtri azione globale

Talvolta si desidera eseguire la logica prima dell'esecuzione di un metodo di azione o dopo l'esecuzione di un metodo di azione. A tale scopo, ASP.NET MVC 2 fornito filtri azione. I filtri azione sono attributi personalizzati che forniscono un mezzo dichiarativo per aggiungere un comportamento pre-azione e post-azione a metodi di azione del controller specifici. Tuttavia, in alcuni casi potrebbe essere necessario specificare un comportamento pre-azione o post-azione che si applica a tutti i metodi di azione. MVC 3 consente di specificare i `GlobalFilters` filtri globali aggiungendoli alla raccolta. Per altre informazioni sui filtri azione globali, vedere le risorse seguenti:For more information about global action filters, see the following resources:

- [Blog di Scott Guthrie sull'anteprima di MVC 3](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Applicazione di filtri in ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nuova proprietà "ViewBag"

I controller MVC2 supportano una `ViewData` proprietà che consente di passare dati a un modello di visualizzazione utilizzando un'API del dizionario ad associazione tardiva. In MVC 3, è anche possibile utilizzare `ViewBag` una sintassi un po 'più semplice con la proprietà per raggiungere lo stesso scopo. Ad esempio, anziché scrivere `ViewData["Message"]="text"` `ViewBag.Message="text"`, è possibile scrivere . Non è necessario definire classi fortemente tipizzate per utilizzare la `ViewBag` proprietà . Poiché si tratta di una proprietà dinamica, è invece possibile ottenere o impostare solo le proprietà e le risolverà in modo dinamico in fase di esecuzione. Internamente, `ViewBag` le proprietà vengono archiviate `ViewData` come coppie nome/valore nel dizionario. (Nota: nella maggior parte delle versioni `ViewBag` non definitive di MVC 3, la proprietà è stata denominata la `ViewModel` proprietà.)

### <a name="new-actionresult-types"></a>Nuovi tipi di "ActionResult"

I `ActionResult` tipi seguenti e i metodi di supporto corrispondenti sono nuovi o migliorati in MVC 3:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Restituisce un codice di stato HTTP 404 al client.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Restituisce un reindirizzamento temporaneo (codice di stato HTTP 302) o un reindirizzamento permanente (codice di stato HTTP 301), a seconda di un parametro booleano. In combinazione con questa modifica, la classe [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) dispone `RedirectPermanent`ora `RedirectToRoutePermanent`di `RedirectToActionPermanent`tre metodi per l'esecuzione di reindirizzamenti permanenti: , e . Questi metodi restituiscono `RedirectResult` un'istanza di con la `Permanent` proprietà impostata su `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Restituisce un codice di stato HTTP specificato dall'utente.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>Miglioramenti di JavaScript e Ajax

Per impostazione predefinita, Ajax e gli helper di convalida in MVC 3 utilizzano un approccio JavaScript discreto. JavaScript discreto evita l'iniezione di JavaScript inline in HTML. Questo rende il codice HTML più piccolo e meno ingombra e rende più facile scambiare o personalizzare le librerie JavaScript. Gli helper di convalida in `jQueryValidate` MVC 3 usano anche il plug-in per impostazione predefinita. Se si desidera il comportamento di MVC 2, è possibile disabilitare JavaScript discreto utilizzando un'impostazione del file *web.config.* Per altre informazioni sui miglioramenti relativi a JavaScript e Ajax, vedere le risorse seguenti:For more information about JavaScript and Ajax improvements, see the following resources:

- [Introduzione di base a JavaScript discreto sul sito di Wikipedia](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Post JavaScript discreto di Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Post di convalida JavaScript discreto di Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Creazione di un'applicazione MVC 3 con Razor e JavaScript discreto](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (tutorial sul sito ASP.NET)
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Convalida lato client abilitata per impostazione predefinitaClient-Side Validation Enabled by Default

Nelle versioni precedenti di MVC, è `Html.EnableClientValidation` necessario chiamare in modo esplicito il metodo da una visualizzazione per abilitare la convalida lato client. In MVC 3 questo non è più necessario perché la convalida lato client è abilitata per impostazione predefinita. È possibile disabilitare questa impostazione utilizzando un'impostazione nel file *web.config.*

Affinché la convalida lato client funzioni, è comunque necessario fare riferimento alle librerie jQuery e jQuery Validation appropriate nel sito. Puoi ospitare tali librerie sul tuo server o farvi riferimento da una rete per la distribuzione di contenuti (CDN) come le CDN di Microsoft o Google.

### <a name="remote-validator"></a>Convalida remota

ASP.NET MVC 3 supporta la nuova classe [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) che consente di sfruttare il supporto della convalida remota del plug-in jQuery. Ciò consente alla libreria di convalida lato client di chiamare automaticamente un metodo personalizzato definito sul server per eseguire la logica di convalida che può essere eseguita solo sul lato server.

Nell'esempio seguente `Remote` l'attributo specifica che la `UserNameAvailable` convalida `UsersController` client chiamerà un'azione denominata sulla classe per convalidare il `UserName` campo.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

L'esempio seguente mostra il controller corrispondente.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Per ulteriori informazioni su `Remote` come utilizzare l'attributo, vedere [procedura: implementare la convalida remota in ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) in MSDN Library.

### <a name="json-binding-support"></a>Supporto dell'associazione JSON

ASP.NET MVC 3 include il supporto dell'associazione JSON incorporata che consente ai metodi di azione di ricevere dati con codifica JSON e di associarli ai parametri del metodo di azione. Questa funzionalità è utile negli scenari che coinvolgono modelli client e l'associazione dati. I modelli client consentono di formattare e visualizzare un singolo elemento di dati o un set di elementi di dati utilizzando i modelli eseguiti nel client. MVC 3 consente di connettere facilmente i modelli client con i metodi di azione nel server che inviano e ricevono dati JSON. Per ulteriori informazioni sul supporto dell'associazione JSON, vedere la sezione **Miglioramenti JavaScript e AJAX** del post di blog MVC 3 Preview di Scott [Guthrie.](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Miglioramenti della convalida del modello

### <a name="dataannotations-metadata-attributes"></a>Attributi dei metadati "DataAnnotations"

ASP.NET MVC 3 `DataAnnotations` supporta gli `DisplayAttribute`attributi dei metadati, ad esempio .

### <a name="validationattribute-class"></a>Classe "ValidationAttribute"

La `ValidationAttribute` classe è stata migliorata in .NET Framework 4 per supportare un nuovo `IsValid` overload che fornisce ulteriori informazioni sul contesto di convalida corrente, ad esempio quale oggetto viene convalidato. In questo modo gli scenari più ricchi in cui è possibile convalidare il valore corrente in base a un'altra proprietà del modello. Ad esempio, `CompareAttribute` il nuovo attributo consente di confrontare i valori di due proprietà di un modello. Nell'esempio seguente, `ComparePassword` la proprietà `Password` deve corrispondere al campo per essere valida.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Interfacce di convalida

Il [IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) interfaccia consente di eseguire la convalida a livello di modello e consente di fornire messaggi di errore di convalida specifici per lo stato del modello complessivo o tra due proprietà all'interno del modello. MVC 3 ora recupera `IValidatableObject` gli errori dall'interfaccia durante l'associazione del modello e contrassegna automaticamente o evidenzia i campi interessati all'interno di una visualizzazione utilizzando gli helper di form HTML incorporati.

Il [IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) interfaccia consente ASP.NET MVC per scoprire in fase di esecuzione se un validator dispone del supporto per la convalida client. Questa interfaccia è stata progettata in modo che possa essere integrata con una varietà di framework di convalida.

Per ulteriori informazioni sulle interfacce di convalida, vedere la sezione Miglioramenti della **convalida** dei modelli del post di blog di Mvc 3 Preview di [Scott Guthrie.](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx) (Tuttavia, si noti che il riferimento a "IValidateObject" nel blog deve essere "IValidatableObject".)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Miglioramenti dell'inserimento delle dipendenzeDependency Injection Improvements

ASP.NET MVC 3 fornisce un supporto migliore per l'applicazione di Inserimento di dipendenze (DI) e per l'integrazione con l'inserimento di dipendenze o l'inversione del controllo (IOC) contenitori. Il supporto per DI è stato aggiunto nelle seguenti aree:

- Controllori (registrazione e iniezione di fabbriche di controller, controllori per l'iniezione).
- Visualizzazioni (registrazione e inserimento dei motori di visualizzazione, inserimento delle dipendenze nelle pagine di visualizzazione).
- Filtri di azione (localizzazione e iniezione di filtri).
- Leganti modello (registrazione e iniezione).
- Provider di convalida del modello (registrazione e iniezione).
- Provider di metadati del modello (registrazione e inserimento).
- Fornitori di valore (registrazione e iniezione).

MVC 3 supporta la libreria [Common Service Locator](https://github.com/unitycontainer/commonservicelocator) e qualsiasi `IServiceLocator` contenitore DI che supporta l'interfaccia di tale libreria. Supporta inoltre una `IDependencyResolver` nuova interfaccia che semplifica l'integrazione dei framework DI.

Per ulteriori informazioni su DI in MVC 3, vedere le risorse seguenti:For more information about DI in MVC 3, see the following resources:

- [La serie di post di blog di Brad Wilson su Service Location](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Altre nuove funzionalità

### <a name="nuget-integration"></a>Integrazione NuGet

ASP.NET MVC 3 installa e abilita automaticamente NuGet come parte della sua configurazione. NuGet è un gestore di pacchetti open source gratuito che semplifica l'individuazione, l'installazione e l'utilizzo di librerie e strumenti .NET nei progetti. Funziona con tutti i tipi di progetto di Visual Studio (inclusi ASP.NET Web Form e ASP.NET MVC).

NuGet consente agli sviluppatori che gestiscono progetti open source (ad esempio, progetti come Moq, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks ed Elmah) di creare pacchetti delle librerie e registrarle in una galleria online. È quindi facile per gli sviluppatori .NET che desiderano utilizzare una di queste librerie per trovare il pacchetto e installarlo nei progetti su cui stanno lavorando.

Con il ASP.NET 3 Strumenti Aggiornamento, i modelli di progetto includono librerie JavaScript preinstallati pacchetti NuGet, in modo che siano aggiornabili tramite NuGet. Anche Entity Framework Code First è preinstallato come pacchetto NuGet.

Per altre informazioni su NuGet, vedere la [documentazione di NuGet](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Memorizzazione nella cache di output a pagina parzialePartial-Page Output Caching

ASP.NET MVC supporta la memorizzazione nella cache di output delle risposte a pagina intera dalla versione 1.The MVC has supported output caching of full page responses since version 1. MVC 3 supporta anche la memorizzazione nella cache di output a pagina parziale, che consente di memorizzare facilmente nella cache aree o frammenti di una risposta. Per ulteriori informazioni sulla memorizzazione nella cache, vedere la sezione relativa alla **memorizzazione nella cache** di output di pagina parziale del post di blog di Scott [Guthrie nel candidato](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) alla versione MVC 3 e nella sezione **Caching dell'output** delle azioni figlio delle [note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Controllo granulare sulla convalida delle richieste

ASP.NET MVC dispone di una convalida delle richieste incorporata che consente automaticamente la protezione dagli attacchi XSS e HTML injection. Tuttavia, a volte si desidera disabilitare in modo esplicito la convalida delle richieste, ad esempio se si desidera consentire agli utenti di pubblicare contenuto HTML (ad esempio, nelle voci del blog o nel contenuto CMS). È ora possibile aggiungere un attributo [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) ai modelli o ai modelli di visualizzazione per disabilitare la convalida delle richieste in base alle proprietà durante l'associazione del modello. Per altre informazioni sulla convalida delle richieste, vedere le risorse seguenti:For more information about request validation, see the following resources:

- La sezione **JavaScript e convalida discreta** nel post di blog di [Scott Guthrie sul candidato](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx)alla versione MVC 3 .
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Finestra di dialogo "Nuovo progetto" estensibile

In ASP.NET MVC 3 è possibile aggiungere modelli di progetto, motori di visualizzazione e framework di progetti di unit test alla finestra di dialogo **Nuovo progetto.**

### <a name="template-scaffolding-improvements"></a>Miglioramenti dello scaffolding dei modelli

ASP.NET modelli di scaffolding MVC 3 consentono di identificare le proprietà della chiave primaria nei modelli e gestirle in modo appropriato rispetto alle versioni precedenti di MVC. Ad esempio, i modelli di impalcatura ora assicurarsi che la chiave primaria non viene sottoposta a scaffolding come campo modulo modificabile.

Per impostazione predefinita, gli scaffold `Html.EditorFor` di creazione `Html.TextBoxFor` e modifica ora usano l'helper anziché l'helper. Ciò migliora il supporto per i metadati nel modello sotto forma di attributi di annotazione dei dati quando la finestra di dialogo **Aggiungi visualizzazione** genera una vista.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Nuovi overload per "Html.LabelFor" e "Html.LabelForModel"

Sono stati aggiunti nuovi overload `LabelFor` `LabelForModel` del metodo per i metodi di supporto e . I nuovi overload consentono di specificare o sostituire il testo dell'etichetta.

### <a name="sessionless-controller-support"></a>Supporto del controller senza sessioneSessionless Controller Support

In ASP.NET MVC 3 è possibile indicare se si desidera che una classe controller utilizzi lo stato sessione e, in caso affermativo, se lo stato della sessione deve essere di lettura/scrittura o di sola lettura. Per ulteriori informazioni sul supporto del controller senza sessione, vedere [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nuova classe "AdditionalMetadataAttribute"

È possibile utilizzare l'attributo `ModelMetadata.AdditionalValues` [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) per popolare il dizionario per una proprietà del modello. Ad esempio, se un modello di visualizzazione dispone di una proprietà che deve essere visualizzata solo a un amministratore, è possibile annotare tale proprietà come illustrato nell'esempio seguente:For example, if a view model has a property that should be displayed only to an administrator, you can annotate that property as shown in the following example:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Questi metadati vengono resi disponibili per qualsiasi modello di visualizzazione o editor quando viene eseguito il rendering di un modello di visualizzazione prodotto. Spetta all'utente interpretare le informazioni sui metadati.

### <a name="accountcontroller-improvements"></a>Miglioramenti di AccountController

Il AccountController nel modello di progetto Internet è stato notevolmente migliorato.

### <a name="new-intranet-project-template"></a>Nuovo modello di progetto Intranet

È incluso un nuovo modello di progetto Intranet che abilita l'autenticazione di Windows e rimuove il AccountController.
