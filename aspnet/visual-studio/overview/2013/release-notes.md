---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET e Web Tools per Visual Studio 2013 Note sulla versione Documenti Microsoft
author: rick-anderson
description: In questo documento viene descritto il rilascio di ASP.NET e Web Tools per Visual Studio 2013.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: 4494b4acdd30384e1def89b7fff9bad30933e38e
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543496"
---
# <a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>Note sulla versione di ASP.NET and Web Tools per Visual Studio 2013

da parte [di Microsoft](https://github.com/microsoft)

> In questo documento viene descritto il rilascio di ASP.NET e Web Tools per Visual Studio 2013.

## <a name="contents"></a>Sommario

- [Note di installazione](#TOC1)
- [Documentazione](#TOC2)
- [Requisiti software](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nuove funzionalità in ASP.NET e strumenti Web per Visual Studio 2013

- [Un ASP.NET](#TOC6)
- [Nuova esperienza di progetto Web](#newproj)
- [ASP.NET Scaffolding](#scaffold)
- [Browser Link](#browser-link)
- [Miglioramenti di Visual Studio Web Editor](#web-editor)
- [Azure App Service Web Apps Support in Visual Studio](#waws)
- [Miglioramenti della pubblicazione sul Web](#publish)
- [NuGet 2.7](#nuget)
- [Web Form ASP.NET](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [API Web ASP.NET 2](#TOC11)
- [ASP.NET SignalR](#TOC13)
- [Identità ASP.NET](#TOC8)
- [Componenti Microsoft OWIN](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Rasoio 3](#TOC14)
- [Sospensione dell'app ASP.NET](#TOC15)
- [Problemi noti e modifiche di rilievoKnown Issues and Breaking Changes](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Note di installazione

ASP.NET e Web Tools per Visual Studio 2013 sono inclusi nel programma di installazione principale e possono essere scaricati [qui](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Documentazione

Esercitazioni e altre informazioni su ASP.NET e Web Tools per Visual Studio 2013 sono disponibili sul [sito Web ASP.NET](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Requisiti software

ASP.NET e Web Tools richiede Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nuove funzionalità in ASP.NET e strumenti Web per Visual Studio 2013

Nelle sezioni seguenti vengono descritte le funzionalità introdotte nella versione.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>Un ASP.NET

Con il rilascio di Visual Studio 2013, abbiamo fatto un passo verso l'unificazione dell'esperienza di utilizzo di tecnologie ASP.NET, in modo che è possibile facilmente mescolare e abbinare quelli che si desidera. Ad esempio, è possibile avviare un progetto utilizzando MVC e aggiungere facilmente pagine Web Form al progetto in un secondo momento o eseguire lo scaffolding delle API Web in un progetto Web Form. Un ASP.NET è tutto di rendere più facile per voi come sviluppatore di fare le cose che ami in ASP.NET. Indipendentemente dalla tecnologia scelta, si può avere fiducia che si sta basando sul framework sottostante attendibile di One ASP.NET.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Nuova esperienza di progetto Web

Abbiamo migliorato l'esperienza di creazione di nuovi progetti web in Visual Studio 2013. Nella finestra di dialogo **Nuovo progetto Web ASP.NET** è possibile selezionare il tipo di progetto desiderato, configurare qualsiasi combinazione di tecnologie (Web Form, MVC, API Web), configurare le opzioni di autenticazione e aggiungere un progetto di unit test.

![Nuovo progetto ASP.NET](release-notes/_static/image1.png)

La nuova finestra di dialogo consente di modificare le opzioni di autenticazione predefinite per molti dei modelli. Ad esempio, quando si crea un ASP.NET progetto Web Form è possibile selezionare una delle opzioni seguenti:

- Nessuna autenticazione
- Singoli account utente (ASP.NET di appartenenza o di accedere al provider di social network)
- Account dell'organizzazione (Active Directory in un'applicazione Internet)
- Autenticazione di Windows (Active Directory in un'applicazione Intranet)

![Opzioni di autenticazione](release-notes/_static/image2.png)

Per ulteriori informazioni sul nuovo processo di creazione di progetti Web, vedere [Creazione di progetti Web ASP.NET in Visual Studio 2013](creating-web-projects-in-visual-studio.md). Per ulteriori informazioni sulle nuove opzioni di autenticazione, vedere [ASP.NET Identity](#TOC8) più avanti in questo documento.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

ASP.NET Scaffolding è un framework di generazione di codice per applicazioni Web ASP.NET. Semplifica l'aggiunta di codice boilerplate al progetto che interagisce con un modello di dati.

Nelle versioni precedenti di Visual Studio, lo scaffolding era limitato aASP.NET progetti MVC. Con Visual Studio 2013, è ora possibile utilizzare lo scaffolding per qualsiasi progetto di ASP.NET, incluso Web Form. Visual Studio 2013 non supporta attualmente la generazione di pagine per un progetto Web Form, ma è comunque possibile utilizzare lo scaffolding con Web Form aggiungendo dipendenze MVC al progetto. Il supporto per la generazione di pagine per Web Form verrà aggiunto in un aggiornamento futuro.

Quando si usa lo scaffolding, si garantisce che tutte le dipendenze necessarie siano installate nel progetto. Ad esempio, se si inizia con un ASP.NET progetto Web Form e quindi si utilizza lo scaffolding per aggiungere un Controller API Web, i pacchetti E i riferimenti NuGet necessari vengono aggiunti automaticamente al progetto.

Per aggiungere lo scaffolding MVC a un progetto Web Form, aggiungere un **nuovo elemento con scaffolding** e selezionare **Dipendenze MVC 5** nella finestra di dialogo. Sono disponibili due opzioni per lo scaffolding MVC; Minimo e completo. Se si seleziona Minimal, solo i pacchetti NuGet e i riferimenti per ASP.NET MVC vengono aggiunti al progetto. Se si seleziona l'opzione Completo, vengono aggiunte le dipendenze minime, nonché i file di contenuto necessari per un progetto MVC.

Il supporto per lo scaffolding dei controller asincroni usa le nuove funzionalità asincrone di Entity Framework 6.Support for scaffolding async controllers uses the new async features from Entity Framework 6.

Per ulteriori informazioni ed esercitazioni, vedere [Cenni ASP.NET scaffolding](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Collegamento browser – canale SignalR tra browser e Visual Studio

La nuova funzionalità [collegamento browser](using-browser-link.md) consente di connettere più browser a Visual Studio e aggiornarli tutti facendo clic su un pulsante nella barra degli strumenti. È possibile connettere più browser al sito di sviluppo, inclusi gli emulatori per dispositivi mobili, e fare clic su Aggiorna per aggiornare tutti i browser contemporaneamente. Browser Link espone anche un'API per consentire agli sviluppatori di scrivere estensioni di browser link.

![](release-notes/_static/image3.png)

Consentendo agli sviluppatori di sfruttare l'API di collegamento del browser, diventa possibile creare scenari molto avanzati che attraversano i limiti tra Visual Studio e qualsiasi browser connesso. Web Essentials sfrutta l'API per creare un'esperienza integrata tra Visual Studio e gli strumenti di sviluppo del browser, emulatori mobili di controllo remoto e molto altro ancora.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Miglioramenti di Visual Studio Web Editor

Visual Studio 2013 include un nuovo editor HTML per i file Razor e i file HTML nelle applicazioni Web. Il nuovo editor HTML fornisce un unico schema unificato basato su HTML5. Ha il completamento automatico delle parentesi graffe, jQuery UI e AngularJS attributo IntelliSense, attributo IntelliSense raggruppamento, ID e nome di classe Intellisense, e altri miglioramenti tra cui migliori prestazioni, formattazione e SmartTags.

Nella schermata seguente viene illustrato l'utilizzo dell'attributo Bootstrap IntelliSense nell'editor HTML.

![Intellisense nell'editor HTML](release-notes/_static/image4.png)

Visual Studio 2013 include anche sia gli editor CoffeeScript che LESS incorporati. L'editor LESS viene fornito con tutte le caratteristiche interessanti dell'editor CSS e ha Intellisense specifico per variabili e mixin in tutti i documenti LESS nella @import catena.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Azure App Service Web Apps Support in Visual Studio

In Visual Studio 2013 con Azure SDK per .NET 2.2 è possibile usare **Esplora server** per interagire direttamente con le app Web remote. È possibile accedere al proprio account Azure, creare nuove app Web, configurare app, visualizzare log in tempo reale e altro ancora. Prossimamente dopo il rilascio di SDK 2.2, sarà possibile eseguire in modalità di debug in remoto in Azure.Coming soon after SDK 2.2 is released, you'll be able to run in debug mode remotely in Azure. La maggior parte delle nuove funzionalità per le app Web del servizio app di Azure funzionano anche in Visual Studio 2012 quando si installa la versione corrente di Azure SDK per .NET.

Per altre informazioni, vedere le seguenti risorse:

- [Creare un'app Web ASP.NET nel servizio app di AzureCreate an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Risoluzione dei problemi di un'app Web nel servizio app di Azure tramite Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Miglioramenti della pubblicazione sul Web

Visual Studio 2013 include funzionalità di pubblicazione Web nuove e migliorate. Alcuni esempi sono i seguenti:

- Automatizza facilmente la crittografia dei [file Web.config](https://go.microsoft.com/fwlink/?LinkId=325529). (Questo collegamento e le due seguenti puntano alla documentazione su MSDN che potrebbe non essere disponibile fino a tardi il 17/17.)
- Automatizza facilmente [l'accesso offline di un'applicazione durante](https://go.microsoft.com/fwlink/?LinkId=325530)la distribuzione.
- Configurare Distribuzione Web per utilizzare il [checksum dei file anziché la data dell'ultima modifica](https://go.microsoft.com/fwlink/?LinkId=325531) per determinare quali file devono essere copiati nel server.
- Pubblicare rapidamente singoli file selezionati (incluso Web.config) quando si utilizzano i metodi di pubblicazione FTP o del file system e con Distribuzione Web.

Per ulteriori informazioni sulla distribuzione Web di ASP.NET, vedere [il sito ASP.NET](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 include un ricco set di nuove funzionalità descritte in dettaglio in [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Questa versione di NuGet elimina anche la necessità di fornire il consenso esplicito per la funzionalità di ripristino del pacchetto di NuGet per scaricare i pacchetti. Il consenso (e la casella di controllo associata nella finestra di dialogo delle preferenze di NuGet) viene ora concesso installando NuGet.Consent (and the associated checkbox in NuGet's preferences dialog) is now granted by installing NuGet. Ora il ripristino del pacchetto funziona semplicemente per impostazione predefinita.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>Web Form ASP.NET

### <a name="one-aspnet"></a>Un ASP.NET

I modelli di progetto Web Form si integrano perfettamente con la nuova esperienza di one ASP.NET. È possibile aggiungere il supporto MVC e API Web al progetto Web Form ed è possibile configurare l'autenticazione utilizzando la procedura guidata di creazione di un progetto ASP.NET. Per ulteriori informazioni, vedere [Creazione di ASP.NET progetti Web in Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>Identità ASP.NET

I modelli di progetto Web Form supportano il nuovo framework di identità ASP.NET. Inoltre, i modelli supportano ora la creazione di un progetto Intranet Web Form. Per ulteriori informazioni, vedere Metodi di [autenticazione](creating-web-projects-in-visual-studio.md#auth) in **Creazione di ASP.NET progetti Web in Visual Studio 2013.**

### <a name="bootstrap"></a>Bootstrap

I modelli di Web Form utilizzano [Bootstrap](http://twitter.github.io/bootstrap/) per fornire un aspetto elegante e reattivo che è possibile personalizzare facilmente. Per ulteriori informazioni, vedere Bootstrap nei modelli di progetto Web di [Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>Un ASP.NET

I modelli di progetto Web MVC si integrano perfettamente con la nuova esperienza One ASP.NET. È possibile personalizzare il progetto MVC e configurare l'autenticazione utilizzando la procedura guidata di creazione del progetto One ASP.NET. Un'esercitazione introduttiva a ASP.NET MVC 5 è disponibile in [Introduzione all'ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Per informazioni sull'aggiornamento di progetti MVC 4 a MVC 5, vedere [Come aggiornare un ASP.NET MVC 4 e Progetto API Web per ASP.NET MVC 5 e Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>Identità ASP.NET

I modelli di progetto MVC sono stati aggiornati per utilizzare ASP.NETidentità per l'autenticazione e la gestione delle identità. Un'esercitazione con l'autenticazione di Facebook e Google e la nuova API di appartenenza è disponibile in [Creare un'app ASP.NET MVC 5 con Facebook e Google OAuth2 e OpenID Sign-On](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) e [Creare un'app MVC ASP.NET con autenticazione e DATABASE SQL e distribuzione nel servizio app di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Bootstrap

Il modello di progetto MVC è stato aggiornato per utilizzare [Bootstrap](http://getbootstrap.com/) per fornire un aspetto elegante e reattivo che è possibile personalizzare facilmente. Per ulteriori informazioni, vedere Bootstrap nei modelli di progetto Web di [Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtri di autenticazione

I filtri di autenticazione sono un nuovo tipo di filtro in ASP.NET MVC che vengono eseguiti prima dei filtri di autorizzazione nella pipeline di mvc ASP.NET e consentono di specificare la logica di autenticazione per azione, per controller o a livello globale per tutti i controller. I filtri di autenticazione elaborano le credenziali nella richiesta e forniscono un'entità corrispondente. I filtri di autenticazione possono anche aggiungere problemi di autenticazione in risposta a richieste non autorizzate.

### <a name="filter-overrides"></a>Sostituzioni di filtri

È ora possibile eseguire l'override dei filtri da applicare a un determinato metodo di azione o controller specificando un filtro di sostituzione. I filtri di override specificano un set di tipi di filtro che non devono essere eseguiti per un determinato ambito (azione o controller). In questo modo è possibile configurare i filtri che si applicano a livello globale, ma quindi escludere determinati filtri globali dall'applicazione a specifiche azioni o controller.

### <a name="attribute-routing"></a>Routing con attributi

ASP.NET MVC supporta ora il routing degli attributi, grazie a [http://attributerouting.net](http://attributerouting.net)un contributo di Tim McCall, autore di . Con il routing degli attributi è possibile specificare le route annotando le azioni e i controller.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>API Web ASP.NET 2

### <a name="attribute-routing"></a>Routing con attributi

ASP.NET API Web supporta ora il routing degli attributi, grazie [http://attributerouting.net](http://attributerouting.net)a un contributo di Tim McCall, autore di . Con il routing degli attributi è possibile specificare le route API Web annotando le azioni e i controller come questo:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Il routing degli attributi offre un maggiore controllo sugli URI nell'API Web. Ad esempio, è possibile definire facilmente una gerarchia di risorse usando un singolo controller API:For example, you can easily define a resource hierarchy using a single API controller:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Il routing degli attributi fornisce inoltre una sintassi comoda per specificare parametri facoltativi, valori predefiniti e vincoli di route:Attribute routing also provides a convenient syntax for specifying optional parameters, default values, and route constraints:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Per ulteriori informazioni sul routing degli attributi, vedere [Routing degli attributi nell'API Web 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

I modelli di progetto API Web e Applicazione a pagina singola ora supportano l'autorizzazione tramite OAuth 2.0. OAuth 2.0 è un framework per autorizzare l'accesso client alle risorse protette. Funziona per una varietà di client, tra cui browser e dispositivi mobili.

Il supporto per OAuth 2.0 si basa sul nuovo middleware di sicurezza fornito dai componenti Microsoft OWIN per l'autenticazione al portatore e l'implementazione del ruolo del server di autorizzazione. In alternativa, i client possono essere autorizzati utilizzando un server di autorizzazione dell'organizzazione, ad esempio Azure Active Directory o ADFS in Windows Server 2012 R2.

### <a name="odata-improvements"></a>Miglioramenti di OData

**Supporto per $select, $expand, $batch e $value**

ASP.NET API Web OData dispone ora del supporto completo per $select, $expand e $value. È inoltre possibile utilizzare $batch per l'invio in batch delle richieste e l'elaborazione di insiemi di modifiche.

Le opzioni $select e $expand consentono di modificare la forma dei dati restituiti da un endpoint OData. Per ulteriori informazioni, vedere [Introduzione al supporto di $select e $expand nell'API Web OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Estendibilità migliorata**

I formattatori OData sono ora estensibili. È possibile aggiungere metadati della voce Atom, supportare le voci di collegamento flusso e supporto denominate, aggiungere annotazioni di istanza e personalizzare la modalità di generazione dei collegamenti.

**Supporto senza tipi**

È ora possibile compilare servizi OData senza dover definire tipi CLR per i tipi di entità. Al contrario, i controller OData possono accettare o restituire istanze di **IEdmObject**, che sono i formattatori OData serializzare/deserializzare.

**Riutilizzare un modello esistente**

Se si dispone già di un modello di dati di entità (EDM) esistente, è ora possibile riutilizzarlo direttamente, anziché doverla creare uno nuovo. Ad esempio, se si utilizza Entity Framework, è possibile utilizzare l'EDM che Entity Framework compila per l'utente.

### <a name="request-batching"></a>Richiesta batch

L'invio in batch delle richieste combina più operazioni in un'unica richiesta HTTP POST, per ridurre il traffico di rete e fornire un'interfaccia utente più fluida e meno chiacchierone. ASP.NET API Web supporta ora diverse strategie per l'invio in batch delle richieste:

- Usare l'endpoint $batch di un servizio OData.Use the endpoint are endpoint of an OData service.
- Creare un pacchetto di più richieste in una singola richiesta multiparte MIME.
- Utilizzare un formato di batch personalizzato.

Per abilitare l'invio in batch delle richieste, è sufficiente aggiungere una route con un gestore di invio in batch alla configurazione dell'API Web:To enable request batching, simply add a route with a batching handler to your Web API configuration:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

È inoltre possibile controllare se le richieste o eseguite in sequenza o in qualsiasi ordine.

### <a name="portable-aspnet-web-api-client"></a>Client API Web ASP.NET portatile

È ora possibile utilizzare il client API Web ASP.NET per creare librerie di classi portabili che funzionano nelle applicazioni Windows Store e Windows Phone 8. È inoltre possibile creare formattatori portabili che possono essere condivisi tra client e server.

### <a name="improved-testability"></a>Testabilità migliorata

Web API 2 rende molto più semplice per unit test i controller API. È sufficiente creare un'istanza del controller API con il messaggio di richiesta e la configurazione e quindi chiamare il metodo di azione che si desidera testare. È anche facile simulare la classe **UrlHelper,** per i metodi di azione che eseguono la generazione dei collegamenti.

### <a name="ihttpactionresult"></a>IHttpActionResult

È ora possibile implementare IHttpActionResult per incapsulare il risultato dei metodi di azione dell'API Web.You can now implement IHttpActionResult to encapsulate the result of your Web API action methods. Un IHttpActionResult restituito da un metodo di azione API Web viene eseguito dal runtime api Web ASP.NET per produrre il messaggio di risposta risultante. Un IHttpActionResult può essere restituito da qualsiasi azione API Web per semplificare unit test dell'implementazione dell'API Web. Per comodità, vengono fornite all'esterno delle implementazioni di IHttpActionResult, inclusi i risultati per la restituzione di codici di stato specifici, contenuto formattato o risposte negoziate sul contenuto.

### <a name="httprequestcontext"></a>HttpRequestContext (Informazioni in base a HttpRequestContext

Il nuovo **HttpRequestContext** tiene traccia di qualsiasi stato legato alla richiesta ma non immediatamente disponibile dalla richiesta. Ad esempio, è possibile usare **HttpRequestContext** per ottenere i dati della route, l'entità associata alla richiesta, il certificato client, **UrlHelper** e la radice del percorso virtuale. È possibile creare facilmente un **HttpRequestContext** per scopi di unit test.

Poiché l'entità per la richiesta viene trasmessa con la richiesta anziché basarsi su **Thread.CurrentPrincipal**, l'entità è ora disponibile per tutta la durata della richiesta mentre si trova nella pipeline dell'API Web.

### <a name="cors"></a>CORS

Grazie a un altro grande contributo di Brock Allen, ASP.NET ora supporta pienamente Cross Origin Request Sharing (CORS).

La sicurezza del browser impedisce a una pagina Web di creare richieste AJAX per un altro dominio. [CORS](http://www.w3.org/TR/cors/) è uno standard W3C che consente a un server di ridurre i criteri della stessa origine. Con CORS un server può consentire in modo esplicito alcune richieste multiorigine e rifiutarne altre.

Web API 2 ora supporta CORS, inclusa la gestione automatica delle richieste di verifica preliminare. Per altre informazioni, vedere [Abilitare la condivisione di richieste tra le origini nelle API Web di ASP.NET](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Filtri di autenticazione

I filtri di autenticazione sono un nuovo tipo di filtro nellASP.NETAPI Web che viene eseguita prima dei filtri di autorizzazione nella pipeline dell'API Web ASP.NET e consentono di specificare la logica di autenticazione per azione, per controller o a livello globale per tutti i controller. I filtri di autenticazione elaborano le credenziali nella richiesta e forniscono un'entità corrispondente. I filtri di autenticazione possono anche aggiungere problemi di autenticazione in risposta a richieste non autorizzate.

### <a name="filter-overrides"></a>Sostituzioni di filtri

È ora possibile eseguire l'override dei filtri da applicare a un determinato metodo di azione o controller, specificando un filtro di sostituzione. I filtri di override specificano un set di tipi di filtro che non devono essere eseguiti per un determinato ambito (azione o controller). In questo modo è possibile aggiungere filtri globali, ma escluderne alcuni da azioni o controller specifici.

### <a name="owin-integration"></a>Integrazione OWIN

ASP.NET API Web ora supporta completamente OWIN e può essere eseguito su qualsiasi host oWIN. È incluso anche un **HostAuthenticationFilter** che fornisce l'integrazione con il sistema di autenticazione OWIN.

Con l'integrazione OWIN, è possibile auto-ospitare API Web nel proprio processo insieme ad altri middleware OWIN, come SignalR. Per ulteriori informazioni, vedere [Usare OWIN per self-host ASP.NET API Web.](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET SignalR 2.0

Nelle sezioni seguenti vengono descritte le funzionalità di SignalR 2.0.

- [Costruito su OWIN](#builtonowin)
- [MapHubs e MapConnection sono ora MapSignalR](#MapSignalR)
- [Supporto tra domini](#crossdomain)
- [Supporto per iOS e Android tramite MonoTouch e MonoDroid](#mobile)
- [Client .NET portatile](#portable)
- [Nuovo pacchetto Self-Host](#selfhost)
- [Supporto server compatibile con le versioni precedenti](#backwardcompat)
- [Rimosso il supporto del server per .NET 4.0](#remove40)
- [Invio di un messaggio a un elenco di client e gruppi](#messagelist)
- [Invio di un messaggio a un utente specifico](#sendtouser)
- [Migliore supporto per la gestione degli errori](#errorhandling)
- [Unit test più facili degli hub](#unittesting)
- [Gestione degli errori JavaScript](#javascripterror)

Per un esempio di come aggiornare un progetto 1.x esistente a SignalR 2.0, vedere Aggiornamento di [un progetto SignalR 1.x](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Costruito su OWIN

SignalR 2.0 si basa completamente su [OWIN (Open Web Interface per .NET)](http://owin.org/). Questa modifica rende il processo di installazione per SignalR molto più coerente tra le applicazioni SignalR ospitate sul Web e self-hosted, ma ha anche richiesto una serie di modifiche API.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs e MapConnection sono ora MapSignalR

Per garantire la compatibilità con gli standard `MapSignalR`OWIN, questi metodi sono stati rinominati in . `MapSignalR`chiamato senza parametri verrà eseguito `MapHubs` il mapping di tutti gli hub (come avviene nella versione 1.x); Per eseguire il mapping di singoli oggetti **PersistentConnection,** specificare il tipo di connessione come parametro di tipo e l'estensione URL per la connessione come primo argomento.

Il `MapSignalR` metodo viene chiamato in una classe di avvio Owin. Visual Studio 2013 contiene un nuovo modello per una classe di avvio Owin; per utilizzare questo modello, procedere come segue:

1. Fare clic con il pulsante destro del mouse sul progetto
2. Selezionare **Aggiungi**, **Nuovo elemento...**
3. Selezionare **Classe di avvio Owin**. Denominare la nuova classe **Startup.cs**.

In **un'applicazione Web,** la classe `MapSignalR` di avvio Owin contenente il metodo viene quindi aggiunta al processo di avvio di Owin utilizzando una voce nel nodo delle impostazioni dell'applicazione del file Web.Config, come illustrato di seguito.

In **un'applicazione self-hosted**, la classe Startup `WebApp.Start` viene passata come parametro di tipo del metodo.

**Mappatura di hub e connessioni in SignalR 1.x (dal file dell'applicazione globale in un'applicazione Web):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Mappatura di hub e connessioni in SignalR 2.0 (da un file di classe di avvio Owin):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

In **un'applicazione self-hosted**, la classe Startup `WebApp.Start` viene passata come parametro di tipo per il metodo, come illustrato di seguito.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Supporto tra domini

In SignalR 1.x, le richieste tra domini erano controllate da un singolo flag EnableCrossDomain. Questo flag controllava le richieste JSONP e CORS. Per una maggiore flessibilità, tutto il supporto CORS è stato rimosso dal componente server di SignalR (i client JavaScript usano ancora CORS normalmente se viene rilevato che il browser lo supporta) e il nuovo middleware OWIN è stato reso disponibile per supportare questi scenari.

In SignalR 2.0, se JSONP è richiesto sul client (per supportare le richieste tra domini `EnableJSONP` nei `HubConfiguration` browser `true`meno recenti), dovrà essere abilitato in modo esplicito impostando l'oggetto su , come illustrato di seguito. JSONP è disabilitato per impostazione predefinita, in quanto è meno sicuro di CORS.

Per aggiungere il nuovo middleware CORS in SignalR 2.0, aggiungere la `Microsoft.Owin.Cors` libreria al progetto e chiamare `UseCors` prima del middleware SignalR, come illustrato nella sezione seguente.

**Aggiunta di Microsoft.Owin.Cors al progetto:** per installare questa libreria, eseguire il comando seguente nella Console di gestione pacchetti:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Questo comando aggiungerà la versione 2.0.0 del pacchetto al progetto.

**Chiamata di UseCorsCalling UseCors**

I frammenti di codice seguenti illustrano come implementare connessioni tra domini in SignalR 1.x e 2.0.The following code snippets demonstrate how to implement cross-domain connections in SignalR 1.x and 2.0.

**Implementazione di richieste tra domini in SignalR 1.x (dal file globale dell'applicazione)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implementazione di richieste tra domini in SignalR 2.0 (da un file di codice C**

Il codice seguente illustra come abilitare CORS o JSONP in un progetto SignalR 2.0.The following code demonstrates how to enable CORS or JSONP in a SignalR 2.0 project. In questo `Map` esempio `RunSignalR` di `MapSignalR`codice viene utilizzato e anziché , in modo che il middleware CORS venga eseguito solo per le richieste SignalR che richiedono il supporto CORS (anziché per tutto il traffico nel percorso specificato in `MapSignalR`.) `Map` può essere utilizzato anche per qualsiasi altro middleware che deve essere eseguito per un prefisso URL specifico, anziché per l'intera applicazione.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>Supporto per iOS e Android tramite MonoTouch e MonoDroid

È stato aggiunto il supporto per i client iOS e Android che utilizzano i componenti MonoTouch e MonoDroid dalla [libreria Xamarin.](https://xamarin.com/) Per ulteriori informazioni su come utilizzarli, consultate [Utilizzo dei componenti Xamarin.](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln) Questi componenti saranno disponibili nel negozio [Xamarin](https://store.xamarin.com/) quando la versione SignalR RTW sarà disponibile.

<a id="portable"></a>Client .NET portabile

Per facilitare meglio lo sviluppo multipiattaforma, i client Silverlight, WinRT e Windows Phone sono stati sostituiti con un unico client .NET portatile che supporta le seguenti piattaforme:

- RETE 4.5
- Silverlight 5
- WinRT (.NET per le app di Windows Store)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Nuovo pacchetto Self-Host

È ora disponibile un pacchetto NuGet per semplificare l'introduzione all'installazione di SignalR Self-Host (applicazioni SignalR ospitate in un processo o in un'altra applicazione, anziché essere ospitate in un server Web). Per aggiornare un progetto self-host compilato con SignalR 1.x, rimuovere il pacchetto Microsoft.AspNet.SignalR.Owin e aggiungere il pacchetto Microsoft.AspNet.SignalR.SelfHost. Per ulteriori informazioni su come iniziare a utilizzare il pacchetto self-host, vedere [Esercitazione: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Supporto server compatibile con le versioni precedenti

Nelle versioni precedenti di SignalR, le versioni del pacchetto SignalR utilizzate nel client e nel server dovevano essere identiche. Per supportare applicazioni thick-client che sarebbero difficili da aggiornare, SignalR 2.0 ora supporta l'utilizzo di una versione server più recente con un client precedente. **Nota: SignalR 2.0 non supporta i server compilati con versioni precedenti con client più recenti.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Rimosso il supporto del server per .NET 4.0

SignalR 2.0 ha abbandonato il supporto per l'interoperabilità del server con .NET 4.0. .NET 4.5 deve essere utilizzato con i server SignalR 2.0. Esiste ancora un client .NET 4.0 per SignalR 2.0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Invio di un messaggio a un elenco di client e gruppi

In SignalR 2.0, è possibile inviare un messaggio utilizzando un elenco di ID client e gruppo. I frammenti di codice seguenti illustrano come eseguire questa operazione.

**Invio di un messaggio a un elenco di client e gruppi tramite PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Invio di un messaggio a un elenco di client e gruppi tramite Hub**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Invio di un messaggio a un utente specifico

Questa funzionalità consente agli utenti di specificare ciò che l'id utente è basato su un IRequest tramite una nuova interfaccia IUserIdProvider:This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider:

**Interfaccia IUserIdProvider**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Per impostazione predefinita sarà presente un'implementazione che utilizza il IPrincipal.Identity.Name dell'utente come nome utente.

Negli hub, sarai in grado di inviare messaggi a questi utenti tramite una nuova API:

**Utilizzo dell'API Clients.User**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Migliore supporto per la gestione degli errori

Gli utenti possono ora **generare HubException** da qualsiasi chiamata di hub. Il costruttore di **HubException** può accettare un messaggio stringa e un oggetto dati di errore aggiuntivi. SignalR serializzerà automaticamente l'eccezione e la invierà al client dove verrà utilizzata per rifiutare/non superare la chiamata al metodo dell'hub.

L'impostazione **Mostra eccezioni hub dettagliate** non influisce sull'invio o meno di **HubException** al client; viene sempre inviato.

**Codice lato server che dimostra l'invio di un HubException al clientServer-side code demonstrating sending a HubException to the client**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Codice client JavaScript che dimostra la risposta a un'eccezione HubException inviata dal server**

[!code-html[Main](release-notes/samples/sample16.html)]

**Codice client .NET che dimostra la risposta a un'eccezione HubException inviata dal server**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Unit test più facili degli hub

SignalR 2.0 include `IHubCallerConnectionContext` un'interfaccia chiamata su Hub che semplifica la creazione di chiamate lato client fittizie. I frammenti di codice seguenti illustrano l'utilizzo di questa interfaccia con i test cablaggi più diffusi [xUnit.net](https://github.com/xunit/xunit) e [moq](https://code.google.com/p/moq/).

**Unit test SignalR con xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Unit test SignalR con moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>Gestione degli errori JavaScript

In SignalR 2.0, tutti i callback di gestione degli errori JavaScript restituiscono oggetti errore JavaScript anziché stringhe non elaborate. Ciò consente a SignalR di scorrere informazioni più ricche ai gestori degli errori. È possibile ottenere l'eccezione interna dalla `source` proprietà dell'errore.

**Codice client JavaScript che gestisce l'eccezione Start.Fail**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>Identità ASP.NET

### <a name="new-aspnet-membership-system"></a>Nuovo ASP.NET Membership System

ASP.NET Identity è il nuovo sistema di appartenenze per le applicazioni ASP.NET. ASP.NET Identity semplifica l'integrazione dei dati di profilo specifici dell'utente con i dati dell'applicazione. ASP.NET'identità consente inoltre di scegliere il modello di persistenza per i profili utente nell'applicazione. È possibile archiviare i dati in un database SQL Server o in un altro archivio dati, inclusi gli archivi dati NoSQL , come ad esempio le tabelle di Archiviazione di Azure. Per ulteriori informazioni, vedere [Singoli account utente](creating-web-projects-in-visual-studio.md#indauth) in Creazione di progetti Web ASP.NET in Visual Studio **2013**.

### <a name="claims-based-authentication"></a>Autenticazione basata sulle attestazioni

ASP.NET ora supporta l'autenticazione basata sulle attestazioni, in cui l'identità dell'utente è rappresentata come un set di attestazioni da un'autorità di certificazione attendibile. Gli utenti possono essere autenticati utilizzando un nome utente e una password gestiti in un database dell'applicazione o tramite provider di identità social (ad esempio: Account Microsoft, Facebook, Google, Twitter) o tramite account dell'organizzazione tramite Azure Active Directory o Active Directory Federation Services (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Integrazione con Azure Active Directory e Windows Server Active Directory

È ora possibile creare ASP.NET progetti che usano Azure Active Directory o Windows Server Active Directory (AD) per l'autenticazione. Per ulteriori informazioni, vedere [Organizational Accounts](creating-web-projects-in-visual-studio.md#orgauth) in **Creating ASP.NET Web Projects in Visual Studio 2013**.

### <a name="owin-integration"></a>Integrazione OWIN

ASP.NET l'autenticazione è ora basata sul middleware OWIN che può essere utilizzato su qualsiasi host basato su OWIN. Per ulteriori informazioni su OWIN, vedere la seguente sezione [Microsoft OWIN Components.](#TOC7)

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Componenti Microsoft OWIN

[Open Web Interface for .NET](http://owin.org/) (OWIN) definisce un'astrazione tra i server Web .NET e le applicazioni Web. OWIN separa l'applicazione Web dal server, rendendo le applicazioni Web indipendenti dall'host. Ad esempio, è possibile ospitare un'applicazione Web basata su OWIN in IIS o auto-ospitarla in un processo personalizzato.

Le modifiche introdotte nei componenti Microsoft OWIN (noti anche come progetto Katana) includono nuovi componenti server e host, nuove librerie helper e middleware e nuovi middleware di autenticazione.

Per ulteriori informazioni su OWIN e Katana, vedere Novità di [OWIN e Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Nota: le applicazioni [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) non possono essere eseguite in modalità classica di IIS; devono essere eseguiti in modalità integrata.**

**Nota: le applicazioni [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) devono essere eseguite con attendibilità totale.**

### <a name="new-servers-and-hosts"></a>Nuovi server e host

Con questa versione, sono stati aggiunti nuovi componenti per abilitare gli scenari self-host. Questi componenti includono i seguenti pacchetti NuGet:

- **Microsoft.Owin.Host.HttpListener**. Fornisce un server OWIN che utilizza **HttpListener** per restare in ascolto delle richieste HTTP e indirizzarle alla pipeline OWIN.
- **Microsoft.Owin.Hosting** Fornisce una libreria per gli sviluppatori che desiderano ospitare autonomamente una pipeline OWIN in un processo personalizzato, ad esempio un'applicazione console o un servizio Windows.
- **OwinHost**. Fornisce un eseguibile autonomo `Microsoft.Owin.Hosting` che esegue il wrapping e consente di ospitare autonomamente una pipeline OWIN senza dover scrivere un'applicazione host personalizzata.

Inoltre, il `Microsoft.Owin.Host.SystemWeb` pacchetto consente ora al middleware di fornire suggerimenti al server **SystemWeb,** indicando che il middleware deve essere chiamato durante una fase specifica della pipeline di ASP.NET. Questa funzionalità è particolarmente utile per il middleware di autenticazione, che deve essere eseguito nelle prime fasi della pipeline di ASP.NET.

### <a name="helper-libraries-and-middleware"></a>Librerie helper e middleware

Sebbene sia possibile scrivere componenti OWIN utilizzando solo le definizioni `Microsoft.Owin` di funzione e tipo dalla specifica OWIN, il nuovo pacchetto fornisce un set di astrazioni più intuitivo. Questo pacchetto combina diversi pacchetti precedenti (ad esempio, `Owin.Extensions`, `Owin.Types`) in un unico modello a oggetti ben strutturato che può quindi essere facilmente utilizzato da altri componenti OWIN. Infatti, la maggior parte dei componenti di Microsoft OWIN ora utilizzano questo pacchetto.

> [!NOTE]
> Le applicazioni [OWIN](http://www.owin.org) non possono essere eseguite in modalità classica di IIS; devono essere eseguiti in modalità integrata.

> [!NOTE]
> Le applicazioni [OWIN](http://www.owin.org) devono essere eseguite con attendibilità totale.

Questa versione include anche il pacchetto Microsoft.Owin.Diagnostics, che include il middleware per convalidare un'applicazione OWIN in esecuzione, oltre al middleware della pagina di errore per analizzare gli errori.

### <a name="authentication-components"></a>Componenti di autenticazione

Sono disponibili i seguenti componenti di autenticazione.

- **Microsoft.Owin.Security.ActiveDirectory**. Abilita l'autenticazione tramite servizi directory locali o basati su cloud.
- **Microsoft.Owin.Security.Cookies** Abilita l'autenticazione utilizzando i cookie. Questo pacchetto è `Microsoft.Owin.Security.Forms`stato denominato in precedenza .
- **Microsoft.Owin.Security.Facebook** Abilita l'autenticazione utilizzando il servizio basato su OAuth di Facebook.
- **Microsoft.Owin.Security.Google** Abilita l'autenticazione utilizzando il servizio basato su OpenID di Google.
- **Microsoft.Owin.Security.Jwt** Abilita l'autenticazione utilizzando i token JWT.
- **Microsoft.Owin.Security.MicrosoftAccount** Abilita l'autenticazione utilizzando gli account Microsoft.
- **Microsoft.Owin.Security.OAuth**. Fornisce un server di autorizzazione OAuth e un middleware per l'autenticazione dei token di connessione.
- **Microsoft.Owin.Security.Twitter** Abilita l'autenticazione utilizzando il servizio basato su OAuth di Twitter.

Questa versione include `Microsoft.Owin.Cors` anche il pacchetto, che contiene middleware per l'elaborazione di richieste HTTP tra origini.

> [!NOTE]
> Il supporto per la firma JWT è stato rimosso nella versione finale di Visual Studio 2013.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Per un elenco delle nuove funzionalità e altre modifiche in Entity Framework 6, vedere [Cronologia delle versioni](https://msdn.com/data/jj574253)di Entity Framework .

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Rasoio 3

ASP.NET Razor 3 include le seguenti nuove funzionalità:

- Supporto per la modifica delle schede. In precedenza, il comando **Formato documento,** il rientro automatico e la formattazione automatica in Visual Studio non funzionavano correttamente quando si utilizzava l'opzione **Mantieni tabulazioni.** Questa modifica corregge la formattazione di Visual Studio per il codice Razor per la formattazione delle schede.
- Supporto per le regole di riscrittura URL durante la generazione di collegamenti.
- Rimozione dell'attributo security transparent.
  > [!NOTE]
  > Si tratta di una modifica sostanziale e rende Razor 3 incompatibile con MVC4 e versioni precedenti, mentre Razor 2 non è compatibile con MVC5 o assembly compilati con MVC5.

Razor 3 problemi risolti in Visual Studio 2013 da versioni non definitive sono disponibili [qui](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>Sospensione dell'app ASP.NET

ASP.NET sospensione dell'app è una funzionalità che cambia il gioco in .NET Framework 4.5.1 che modifica radicalmente l'esperienza utente e il modello economico per ospitare un numero elevato di siti di ASP.NET in un singolo computer. Per ulteriori informazioni, consultate [ASP.NET sospensione delle app: hosting Web .NET condiviso reattivo](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievoKnown Issues and Breaking Changes

In questa sezione vengono descritti i problemi noti e le modifiche di rilievo nel ASP.NET e Web Tools per Visual Studio 2013.

### <a name="nuget"></a>NuGet

- Il ripristino del nuovo pacchetto non funziona su Mono quando si utilizza il [file SLN:](https://nuget.codeplex.com/workitem/3596) verrà risolto in un download di nuget.exe imminente e un aggiornamento del [pacchetto NuGet.CommandLine.](http://www.nuget.org/packages/NuGet.CommandLine/)
- Il ripristino del nuovo pacchetto non funziona con i [progetti Wix:](https://nuget.codeplex.com/workitem/3598) verrà risolto in un download di nuget.exe imminente e nell'aggiornamento del [pacchetto NuGet.CommandLine.](http://www.nuget.org/packages/NuGet.CommandLine/)
- [Il ripristino automatico](https://nuget.codeplex.com/workitem/3625) dei pacchetti non funziona per i progetti in una cartella della soluzione: verrà risolto in NuGet 2.8.

### <a name="aspnet-web-api"></a>API Web ASP.NET

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)`non ritorna `IQueryable<T>` sempre, come abbiamo `$select` `$expand`aggiunto il supporto per e .

    I campioni `ODataQueryOptions<T>` precedenti per sempre cast `ApplyTo` `IQueryable<T>`il valore restituito da a . Questa operazione ha funzionato in precedenza `$orderby` `$skip`perché `$top`le opzioni di query supportate in precedenza (`$filter`, , , ) non modificano la forma della query. Ora che `$select` sosteniamo `$expand` e il `ApplyTo` valore `IQueryable<T>` restituito da non sarà sempre.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Se si utilizza il codice di esempio precedente, continuerà `$select` a `$expand`funzionare se il client non invia e . Tuttavia, se si `$select` `$expand` desidera supportare ed è necessario modificare il codice in questo.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url o RequestContext.Url è null durante una richiesta batch**

    In uno scenario di batch, **UrlHelper** è null quando si accede da **Request.Url** o **RequestContext.Url**.

    Questo problema è attualmente registrato qui: [BatchRequestContext.Url è null per l'invio in batch richiesta](http://aspnetwebstack.codeplex.com/workitem/1301).

    La soluzione per questo problema consiste nel creare una nuova istanza di **UrlHelper**, come nell'esempio seguente:

    **Creazione di una nuova istanza di UrlHelperCreating a new instance of UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Quando si utilizza MVC5 e OrgAuth, se si dispone di visualizzazioni che eseguire la convalida AntiForgerToken, è possibile che si verifichi il seguente errore quando si pubblicano dati nella visualizzazione:

    **Errore**:

    *Errore del server nell'applicazione '/'.*

    <em>Un'attestazione<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>di tipo<https://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' ' o ' ' non era presente nell'oggetto ClaimsIdentity fornito. Per abilitare il supporto di token anti-falsificazione con l'autenticazione basata sulle attestazioni, verificare che il provider di attestazioni configurato fornisca entrambe le attestazioni nelle istanze di ClaimsIdentity generate. Se il provider di attestazioni configurato utilizza invece un tipo di attestazione diverso come identificatore univoco, può essere configurato impostando la proprietà statica AntiForgeryConfig.UniqueClaimTypeIdentifier.If the configured claims provider instead uses a different claim type as a unique identifier, it can be configured by setting the static property AntiForgeryConfig.UniqueClaimTypeIdentifier.</em>

    **Soluzione temporanea**:

    Aggiungere la riga seguente in Global.asax per risolvere il problema:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Questo problema verrà risolto per la prossima versione.
2. Dopo l'aggiornamento di un'app MVC4 a MVC5, compilare la soluzione e avviarla. Dovrebbe essere visualizzato il seguente errore:

    [A] Impossibile eseguire il cast di System.Web.WebPages.Razor.Configuration.HostSection su [B]System.Web.WebPages.Razor.Configuration.HostSection. Il\_tipo A ha origine da 'System.Web.WebPages.Razor, Versione 2.0.0.0, Impostazioni cultura neutro, PublicKeyToken , 31bf3856ad364e35' nel contesto 'Predefinito' nel percorso 'C:\_\_\_. Il tipo B ha origine da 'System.Web.WebPages.Razor, Versione PublicKeyToken 31bf3856ad364e35' nel contesto 'Predefinito' nel percorso 'C ASP.NET: 5bbd0 , e8b5908e , assembly , dl3 , c9cbca63 , f8910382\_6273ce01 , System.Web.WebPages.Razor.dll'.

    Per correggere l'errore precedente, aprire *tutti i* file Web.config (inclusi quelli nella cartella Views) nel progetto ed eseguire le operazioni seguenti:

   1. Aggiornare tutte le occorrenze della versione "4.0.0.0" di "System.Web.Mvc" a "5.0.0.0".
   2. Aggiornare tutte le occorrenze della versione "2.0.0.0" di &quot;"System.Web.Helpers", System.Web.WebPages&quot; e &quot;System.Web.WebPages.Razor&quot; su "3.0.0.0"

      Ad esempio, dopo aver apportato le modifiche precedenti, le associazioni dell'assembly dovrebbero essere simili alle seguente:For example, after you make the above changes, the assembly bindings should look like this:

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Per informazioni sull'aggiornamento di progetti MVC 4 a MVC 5, vedere [Come aggiornare un ASP.NET MVC 4 e Progetto API Web per ASP.NET MVC 5 e Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Quando si utilizza la convalida lato client con la convalida jQuery Unobtrusive, il messaggio di convalida è talvolta errato per un elemento di input HTML con type ' number'. L'errore di convalida per un valore obbligatorio ("Il campo Età è obbligatorio") viene visualizzato quando viene immesso un numero non valido anziché il messaggio corretto che indica che è necessario un numero valido.

    Questo problema si verifica in genere con il codice con scaffolding per un modello con una proprietà integer nelle visualizzazioni di creazione e modifica.

    Per risolvere questo problema, modificare l'helper dell'editor da:

    `@Html.EditorFor(person => person.Age)`

    Con:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 non supporta più l'attendibilità parziale. I progetti che si contraeno ai file binari MVC o WebAPI devono rimuovere l'attributo [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) e l'attributo [AllowPartiallyTrustedCallers.](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) La rimozione di questi attributi eliminerà gli errori del compilatore come il seguente.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Si noti, come effetto collaterale di questo non è possibile utilizzare gli assembly 4.0 e 5.0 nella stessa applicazione. È necessario aggiornarli tutti alla 5.0.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>Modello SPA con autorizzazione di Facebook può causare instabilità in Internet IE mentre il sito web è ospitato nell'area intranet

Il modello SPA fornisce l'accesso esterno con Facebook. Quando il progetto creato con il modello è in esecuzione in locale, l'accesso potrebbe causare l'arresto anomalo di IEE.

Soluzione

1. Ospitare il sito web nell'area Internet; O

2. Testare lo scenario in un browser diverso da Internet IE.Test the scenario in a browser other than IE.

### <a name="web-forms-scaffolding"></a>Web Forms Scaffolding

Lo scaffolding di Web Form è stato rimosso da VS2013 e sarà disponibile in un aggiornamento futuro di Visual Studio. Tuttavia, è comunque possibile utilizzare lo scaffolding all'interno di un progetto Web Form aggiungendo dipendenze MVC e generando scaffolding per MVC. Il progetto conterrà una combinazione di Web Form e MVC.

Per aggiungere MVC al progetto Web Form, aggiungere un nuovo elemento con scaffolding e selezionare **Dipendenze MVC 5**. Selezionare Minimo o Completo a seconda che siano necessari o meno file di contenuto, ad esempio script. Quindi, aggiungere un elemento con scaffolding per MVC, che creerà visualizzazioni e un controller nel progetto.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC e API Web Scaffolding - ERRORE HTTP 404, non trovato

Se viene rilevato un errore durante l'aggiunta di un elemento con scaffolding a un progetto, è possibile che il progetto venga lasciato in uno stato incoerente. Verrà eseguito il rollback di alcune delle modifiche apportate come scaffolding, ma non verrà eseguito il rollback di altre modifiche, ad esempio i pacchetti NuGet installati. Se viene eseguito il rollback delle modifiche alla configurazione di routing, gli utenti riceveranno un errore HTTP 404 durante lo spostamento agli elementi con scaffolding.

Soluzione alternativa:

- Per correggere questo errore per MVC, aggiungere un nuovo elemento con scaffolding e selezionare Dipendenze MVC 5 (minimo o completo). Questo processo aggiungerà tutte le modifiche necessarie al progetto.
- Per correggere questo errore per l'API Web:

  1. Aggiungere la classe WebApiConfig al progetto.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Configurare WebApiConfig.Register\_nel metodo Application Start in Global.asax come segue:

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
