---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: Novità di ASP.NET 4.5 e Visual Studio 2012 Documenti Microsoft
author: rick-anderson
description: In questo documento vengono descritte le nuove funzionalità e i miglioramenti introdotti in ASP.NET 4.5. Descrive anche i miglioramenti apportati per lo sviluppo web...
ms.author: riande
ms.date: 02/29/2012
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 32fbf7c25b00f3f0796c4c3fdd38ca2a86c89199
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676286"
---
# <a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>Novità di ASP.NET 4.5 e Visual Studio 2012

> In questo documento vengono descritte le nuove funzionalità e i miglioramenti introdotti in ASP.NET 4.5. Vengono inoltre descritti i miglioramenti apportati per lo sviluppo Web in Visual Studio 2012. Questo documento è stato originariamente pubblicato il 29 febbraio 2012.

- [ASP.NET di base e framework](#_Toc318097372)

    - [Lettura e scrittura asincrona di richieste e risposte HTTP](#_Toc318097373)
    - [Miglioramenti alla gestione di HttpRequestImprovements to HttpRequest handling](#_Toc318097374)
    - [Scaricamento asincrono di una risposta](#_Toc318097375)
    - [Supporto per moduli asincroni e gestori basati su attività e *awaitSupport* for await and *Task-Based*Asynchronous Modules and Handlers](#_Toc318097376)
    - [Moduli HTTP asincroni](#_Toc318097377)
    - [Gestori HTTP asincroni](#_Toc318097378)
    - [Nuove funzionalità di convalida delle richieste di ASP.NET](#_Toc318097379)
    - [Convalida della richiesta differita ("pigro")](#_Toc318097380)
    - [Supporto per le richieste non convalidateSupport for unvalidated requests](#_Toc318097381)
    - [Libreria AntiXSS](#_Toc318097382)
    - [Supporto per il protocollo WebSockets](#_Toc318097383)
    - [Bundling e minimizzazione](#_Toc318097384)
    - [Miglioramenti delle prestazioni per l'hosting Web](#_Toc_perf)

        - [Fattori chiave delle prestazioni](#_Toc_perf_1)
        - [Requisiti per le nuove funzionalità delle prestazioniRequirements for New Performance Features](#_Toc_perf_2)
        - [Condivisione di assembly comuni](#_Toc_perf_3)
        - [Utilizzo della compilazione JIT multi-core per un avvio più rapido](#_Toc_perf_4)
        - [Ottimizzazione dell'operazione di Garbage Collection per ottimizzare la memoria](#_Toc_perf_5)
        - [Prelettura per applicazioni Web](#_Toc_perf_6)
- [Web Form ASP.NET](#_Toc318097385)

    - [Controlli dati fortemente tipizzati](#_Toc318097386)
    - [Associazione di modelli](#_Toc318097387)

        - [Selezione dei dati](#_Toc318097388)
        - [Fornitori di valori](#_Toc318097389)
        - [Filtraggio in base ai valori di un controlloFiltering by values from a control](#_Toc318097390)
    - [Espressioni di associazione dati codificate HTMLHTML Encoded Data-Binding Expressions](#_Toc318097391)
    - [Convalida discreta](#_Toc318097392)
    - [Aggiornamenti HTML5](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [Pagine Web ASP.NET 2](#_Toc318097395)
- [Visual Studio 2012 Release Candidate](#_Toc318097396)

    - [Condivisione di progetti tra Visual Studio 2010 e Visual Studio 2012 Release Candidate (compatibilità dei progetti)](#project-compatibility)
    - [Modifiche alla configurazione in ASP.NET modelli di sito Web 4.5](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Supporto nativo in IIS 7 per il routing ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [Editor HTML](#_Toc318097397)

        - [Attività intelligenti](#_Toc318097398)
        - [Supporto WAI-ARIA](#_Toc318097399)
        - [Nuovi frammenti HTML5](#_Toc318097400)
        - [Estrai nel controllo utente](#_Toc318097401)
        - [IntelliSense per le pepite di codice negli attributi](#_Toc318097402)
        - [Ridenominazione automatica del tag corrispondente quando si rinomina un tag di apertura o di chiusura](#_Toc318097403)
        - [Generazione del gestore eventi](#_Toc318097404)
        - [Rientro intelligente](#_Toc318097405)
        - [Riduci automaticamente il completamento delle istruzioni](#_Toc318097406)
    - [JavaScript Editor](#_Toc318097407)

        - [Struttura del codice](#_Toc318097408)
        - [Corrispondenza parentesi graffe](#_Toc318097409)
        - [Vai a Definizione](#_Toc318097410)
        - [Supporto ECMAScript5](#_Toc318097411)
        - [DOM IntelliSense](#_Toc318097412)
        - [Sovraccarichi della firma VSDOC](#_Toc318097413)
        - [Riferimenti impliciti](#_Toc318097414)
    - [CSS (editor)](#_Toc318097415)

        - [Riduci automaticamente il completamento delle istruzioni](#_Toc318097416)
        - [Rientro gerarchico.](#_Toc318097417)
        - [Supporto per hack CSS](#_Toc318097418)
        - [Schemi specifici del fornitore (-moz-,-webkit)](#_Toc318097419)
        - [Supporto per i commenti e gli utenti](#_Toc318097420)
        - [Selezione colori](#_Toc318097421)
        - [Frammenti](#_Toc318097422)
        - [Aree personalizzate](#_Toc318097423)
    - [Controllo pagina](#_Toc318097424)
    - [Pubblicazione](#_Toc318097425)

        - [Profili di pubblicazione](#_Toc318097426)
        - [ASP.NET precompilazione e unione](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Dichiarazione di non responsabilità](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>ASP.NET di base e framework

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>Lettura e scrittura asincrona di richieste e risposte HTTP

ASP.NET 4 è stata introdotta la possibilità di leggere un'entità richiesta HTTP come flusso utilizzando il metodo *HttpRequest.GetBufferlessInputStream.ASP.NET* 4 introduced the ability to read an HTTP request entity as a stream using the HttpRequest.GetBufferlessInputStream method. Questo metodo ha fornito l'accesso in streaming all'entità richiesta. Tuttavia, è stato eseguito in modo sincrono, che ha collegato un thread per la durata di una richiesta.

ASP.NET 4.5 supporta la possibilità di leggere i flussi in modo asincrono su un'entità richiesta HTTP e la possibilità di eseguire lo scaricamento asincrono. ASP.NET 4.5 offre anche la possibilità di eseguire il doppio buffer inganamento di un'entità di richiesta HTTP, che consente una più semplice integrazione con i gestori HTTP downstream, ad esempio gestori di pagine aspx e ASP.NET controller MVC.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Miglioramenti alla gestione di HttpRequestImprovements to HttpRequest handling

Il riferimento Stream restituito da ASP.NET 4.5 da *HttpRequest.GetBufferlessInputStream* supporta metodi di lettura sincroni e asincroni. L'oggetto *Stream* restituito da *GetBufferlessInputStream* implementa ora entrambi i metodi BeginRead ed EndRead. I metodi *Stream* asincroni consentono di leggere in modo asincrono l'entità richiesta in blocchi, mentre ASP.NET rilascia il thread corrente tra ogni iterazione di un ciclo di lettura asincrono.

ASP.NET 4.5 è stato aggiunto anche un metodo complementare per la lettura dell'entità richiesta in modo memorizzato nel buffer: *HttpRequest.GetBufferedInputStream*. Questo nuovo overload funziona come *GetBufferlessInputStream*, che supporta letture sincrone e asincrone. Tuttavia, durante la lettura, *GetBufferedInputStream* copia anche i byte dell'entità in ASP.NET buffer interni in modo che i moduli e i gestori downstream possano comunque accedere all'entità richiesta. Ad esempio, se un codice upstream nella pipeline ha già letto l'entità richiesta utilizzando *GetBufferedInputStream*, è comunque possibile utilizzare *HttpRequest.Form* o *HttpRequest.Files*. In questo modo è possibile eseguire l'elaborazione asincrona su una richiesta (ad esempio, lo streaming di un caricamento di file di grandi dimensioni in un database), ma eseguire comunque le pagine aspx e MVC ASP.NET controller in un secondo momento.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Scaricamento asincrono di una risposta

L'invio di risposte a un client HTTP può richiedere molto tempo quando il client è lontano o dispone di una connessione a larghezza di banda ridotta. In genere ASP.NET memorizza nel buffer i byte di risposta man mano che vengono creati da un'applicazione. ASP.NET quindi esegue una singola operazione di invio dei buffer accumulati alla fine dell'elaborazione della richiesta.

Se la risposta memorizzata nel buffer è di grandi dimensioni (ad esempio, lo streaming di un file di grandi dimensioni a un client), è necessario chiamare periodicamente *HttpResponse.Flush* per inviare l'output memorizzato nel buffer al client e mantenere l'utilizzo della memoria sotto controllo. Tuttavia, poiché *Flush* è una chiamata sincrona, la chiamata iterativa *Flush* utilizza ancora un thread per la durata delle richieste potenzialmente a esecuzione prolungata.

ASP.NET 4.5 aggiunge il supporto per l'esecuzione di scarichi in modo asincrono utilizzando i metodi *BeginFlush* ed *EndFlush* della classe *HttpResponse.* Utilizzando questi metodi, è possibile creare moduli asincroni e gestori asincroni che inviano dati in modo incrementale a un client senza legare i thread del sistema operativo. Tra le chiamate *BeginFlush* e *EndFlush,* ASP.NET rilascia il thread corrente. Ciò riduce notevolmente il numero totale di thread attivi necessari per supportare download HTTP a esecuzione prolungata.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Supporto per await e Task - Moduli asincroni basati e gestoriSupport for *await* and *Task* - Based Asynchronous Modules and Handlers

In .NET Framework 4 è stato introdotto un concetto di programmazione asincrona definito *attività*. Le attività sono rappresentate dal tipo Task e dai tipi correlati nello spazio dei nomi *System.Threading.Tasks.Tasks* are represented by the *Task* type and related types in the System.Threading.Tasks namespace. .NET Framework 4.5.NET Framework 4.5 si basa su questo con i miglioramenti del compilatore che rendono semplice l'utilizzo degli oggetti *Task.* In .NET Framework 4.5.NET Framework 4.5, i compilatori supportano due nuove parole chiave: *await* e *async*. La parola chiave *await* è l'abbreviazione sintattica per indicare che una parte di codice deve attendere in modo asincrono su un altro frammento di codice. La parola chiave *async* rappresenta un suggerimento che è possibile utilizzare per contrassegnare i metodi come metodi asincroni basati su attività.

La combinazione di *await*, *async*e l'oggetto *Task* semplifica la scrittura di codice asincrono in .NET 4.5. ASP.NET 4.5 supporta queste semplificazioni con nuove API che consentono di scrivere moduli HTTP asincroni e gestori HTTP asincroni utilizzando i nuovi miglioramenti del compilatore.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Moduli HTTP asincroni

Si supponga di voler eseguire operazioni asincrone all'interno di un metodo che restituisce un *Task* oggetto. Nell'esempio di codice riportato di seguito viene definito un metodo asincrono che effettua una chiamata asincrona per scaricare la home page Microsoft.The following code example defines an asynchronous method that makes an asynchronous call to download the Microsoft home page. Si noti l'utilizzo della parola chiave *async* nella firma del metodo e la chiamata *await* a *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

Questo è tutto ciò che devi scrivere: .NET Framework gestirà automaticamente la rimozione dello stack di chiamate durante l'attesa del completamento del download, nonché il ripristino automatico dello stack di chiamate al termine del download.

Si supponga ora di voler utilizzare questo metodo asincrono in un modulo HTTP di ASP.NET asincrono. ASP.NET 4.5 include un metodo di supporto (*EventHandlerTaskAsyncHelper*) e un nuovo tipo delegato (*TaskEventHandler*) che è possibile utilizzare per integrare metodi asincroni basati su attività con il modello di programmazione asincrona precedente esposto dalla pipeline HTTP ASP.NET. In questo esempio viene illustrato come:This example shows how:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Gestori HTTP asincroni

L'approccio tradizionale alla scrittura di gestori asincroni in ASP.NET consiste nell'implementare il *IHttpAsyncHandler* interfaccia. ASP.NET 4.5 introduce il tipo di base asincrono *HttpTaskAsyncHandler* da cui è possibile derivare, che semplifica molto la scrittura di gestori asincroni.

Il *tipo HttpTaskAsyncHandler* è astratto e richiede l'override del metodo *ProcessRequestAsync.The* HttpTaskAsyncHandler type is abstract and requires you to override the ProcessRequestAsync method. Internamente ASP.NET si occupa dell'integrazione della firma restituita (un oggetto *Task)* di *ProcessRequestAsync* con il modello di programmazione asincrona precedente usato dalla pipeline di ASP.NET.

L'esempio seguente mostra come usare *Task* e *await* come parte dell'implementazione di un gestore HTTP asincrono:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Nuove funzionalità di convalida delle richieste di ASP.NET

Per impostazione predefinita, ASP.NET esegue la convalida delle richieste, ovvero esamina le richieste di ricerca di markup o script in campi, intestazioni, cookie e così via. Se ne viene rilevato uno, ASP.NET genera un'eccezione. Questo agisce come una prima linea di difesa contro potenziali attacchi cross-site scripting.

ASP.NET 4.5 semplifica la lettura selettiva dei dati delle richieste non convalidati. ASP.NET 4.5 integra anche la popolare libreria AntiXSS, che in precedenza era una libreria esterna.

Gli sviluppatori hanno spesso richiesto la possibilità di disattivare in modo selettivo la convalida delle richieste per le applicazioni. Ad esempio, se l'applicazione è un software per forum, è possibile consentire agli utenti di inviare post e commenti del forum in formato HTML, ma assicurarsi comunque che la convalida delle richieste stia controllando tutto il resto.

ASP.NET 4.5 vengono introdotte due funzionalità che semplificano l'utilizzo selettivo dell'input non convalidato: la convalida della richiesta differita ("lazy") e l'accesso ai dati delle richieste non convalidati.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Convalida della richiesta differita ("pigro")

In ASP.NET 4.5, per impostazione predefinita, tutti i dati della richiesta sono soggetti alla convalida della richiesta. Tuttavia, è possibile configurare l'applicazione per rinviare la convalida delle richieste fino a quando non si accede effettivamente ai dati della richiesta. Questa operazione viene talvolta definita convalida delle richieste lazy, in base a termini come caricamento lazy per determinati scenari di dati. È possibile configurare l'applicazione per l'utilizzo della convalida posticipata nel file Web.config impostando l'attributo *requestValidationMode* su 4.5 nell'elemento *httpRUntime,* come nell'esempio seguente:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Quando la modalità di convalida della richiesta è impostata su 4.5, la convalida della richiesta viene attivata solo per un valore di richiesta specifico e solo quando il codice accede a tale valore. Ad esempio, se il codice ottiene il valore\_di Request.Form["forum post"], la convalida della richiesta viene richiamata solo per tale elemento nella raccolta di moduli. Nessuno degli altri elementi dell'insieme *Form* viene convalidato. Nelle versioni precedenti di ASP.NET, la convalida delle richieste veniva attivata per l'intera raccolta di richieste quando si accedeva a qualsiasi elemento nella raccolta. Il nuovo comportamento rende più semplice per i diversi componenti dell'applicazione esaminare i diversi dati della richiesta senza attivare la convalida della richiesta su altre parti.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Supporto per le richieste non convalidateSupport for unvalidated requests

La convalida delle richieste posticipate da sola non risolve il problema di ignorare in modo selettivo la convalida della richiesta. La chiamata a Request.Form["forum\_post"] attiva ancora la convalida della richiesta per quel valore di richiesta specifico. Tuttavia, è possibile accedere a questo campo senza attivare la convalida perché si desidera consentire il markup in tale campo.

A tale scopo, ASP.NET 4.5 supporta ora l'accesso non convalidato per richiedere i dati. ASP.NET 4.5 include una nuova proprietà di raccolta *non convalidata* nella classe *HttpRequest.* Questo insieme fornisce l'accesso a tutti i valori comuni dei dati della richiesta, ad esempio *Form*, *QueryString*, *Cookies*e *Url*.

Utilizzando l'esempio del forum, per poter leggere i dati delle richieste non convalidati, è innanzitutto necessario configurare l'applicazione per l'utilizzo della nuova modalità di convalida delle richieste:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

È quindi possibile utilizzare la proprietà *HttpRequest.Unvalidated* per leggere il valore del form non convalidato:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]

> [!WARNING]
> Sicurezza - Utilizzare i dati di *richiesta non convalidati con attenzione!* ASP.NET 4.5 sono state aggiunte le proprietà e le raccolte delle richieste non convalidate per semplificare l'accesso ai dati delle richieste non convalidati molto specifici. Tuttavia, è comunque necessario eseguire la convalida personalizzata sui dati della richiesta non elaborati per garantire che non venga eseguito il rendering del testo pericoloso per gli utenti.

<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>Libreria AntiXSS

A causa della popolarità della libreria Microsoft AntiXSS, ASP.NET 4.5 ora incorpora le routine di codifica di base dalla versione 4.0 di tale libreria.

Le routine di codifica vengono implementate dal tipo *AntiXssEncoder* nel nuovo spazio dei nomi *System.Web.Security.AntiXss* . È possibile utilizzare direttamente il tipo *AntiXssEncoder* chiamando uno dei metodi di codifica statici implementati nel tipo. Tuttavia, l'approccio più semplice per l'utilizzo delle nuove routine anti-XSS consiste nel configurare un'applicazione ASP.NET per utilizzare la classe *AntiXssEncoder* per impostazione predefinita. A tale scopo, aggiungere il seguente attributo al file Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Quando l'attributo *encoderType* è impostato per utilizzare il tipo *AntiXssEncoder,* tutta la codifica di output in ASP.NET utilizza automaticamente le nuove routine di codifica.

Queste sono le parti della libreria AntiXSS esterna che sono state incorporate in ASP.NET 4.5:

- *HtmlEncode*, *HtmlFormUrlEncode*e *HtmlAttributeEncode*
- *XmlAttributeEncode* e *XmlEncode*
- *UrlEncode* e *UrlPathEncode* (nuovo)
- *Codice CssEn*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Supporto per il protocollo WebSockets

Il protocollo WebSockets è un protocollo di rete basato su standard che definisce come stabilire comunicazioni bidirezionali sicure e in tempo reale tra un client e un server su HTTP. Microsoft ha collaborato con gli organismi di standard IETF e W3C per definire il protocollo. Il protocollo WebSockets è supportato da qualsiasi client (non solo browser), con Microsoft che investe notevoli risorse che supportano il protocollo WebSockets su sistemi operativi client e mobili.

Il protocollo WebSockets semplifica molto la creazione di trasferimenti di dati a esecuzione prolungata tra un client e un server. Ad esempio, la scrittura di un'applicazione di chat è molto più semplice perché è possibile stabilire una vera connessione a esecuzione prolungata tra un client e un server. Non è necessario ricorrere a soluzioni alternative come polling periodico o http a lungo polling per simulare il comportamento di un socket.

ASP.NET 4.5 e IIS 8 includono il supporto WebSockets di basso livello, consentendo agli sviluppatori di ASP.NET di utilizzare le API gestite per la lettura e la scrittura asincrona di dati stringa e binari su un oggetto WebSockets. Per ASP.NET 4.5, è disponibile un nuovo spazio dei nomi *System.Web.WebSockets* che contiene i tipi per l'utilizzo del protocollo WebSockets.

Un client browser stabilisce una connessione WebSockets creando un oggetto WebSocket DOM che punta a un URL in un'applicazione ASP.NET, come nell'esempio seguente:A browser client establishes a WebSockets connection by creating a DOM *WebSocket* object that points to a URL in an ASP.NET application, as in the following example:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

È possibile creare endpoint WebSockets in ASP.NET usando qualsiasi tipo di modulo o gestore. Nell'esempio precedente è stato utilizzato un file con estensione ashx, poiché i file con estensione ashx sono un modo rapido per creare un gestore.

In base al protocollo WebSockets, un'applicazione ASP.NET accetta la richiesta WebSockets di un client indicando che la richiesta deve essere aggiornata da una richiesta HTTP GET a una richiesta WebSockets. Ad esempio:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

Il *AcceptWebSocketRequest* metodo accetta un delegato di funzione perché ASP.NET savvolge la richiesta HTTP corrente e quindi trasferisce il controllo al delegato di funzione. Concettualmente questo approccio è simile alla modalità di utilizzo di *System.Threading.Thread*, in cui si definisce un delegato di avvio del thread in cui viene eseguito il lavoro in background.

Dopo ASP.NET e il client ha completato correttamente un handshake WebSockets, ASP.NET chiama il delegato e l'applicazione WebSockets inizia l'esecuzione. The following code example shows a simple echo application that uses the built-in WebSockets support in ASP.NET:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

Il supporto in .NET 4.5 per la parola chiave await e le operazioni asincrone basate su attività è una soluzione naturale per la scrittura di applicazioni WebSockets.The support in .NET 4.5 for the *await* keyword and asynchronous task-based operations is a natural fit for writing WebSockets applications. L'esempio di codice mostra che una richiesta WebSockets viene eseguita in modo completamente asincrono all'interno di ASP.NET. L'applicazione attende in modo asincrono l'invio di un messaggio da un client chiamando *await socket. ReceiveAsync*. Analogamente, è possibile inviare un messaggio asincrono a un client chiamando *await socket. SendAsync*.

Nel browser, un'applicazione riceve messaggi WebSockets tramite una funzione *onmessage.* Per inviare un messaggio da un browser, chiamare il metodo *send* del tipo DOM *WebSocket,* come illustrato nell'esempio seguente:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

In futuro, potremmo rilasciare aggiornamenti a questa funzionalità che astraggono parte della codifica di basso livello necessaria in questa versione per le applicazioni WebSockets.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Creazione di bundle e minimizzazione

Bundling consente di combinare singoli file JavaScript e CSS in un bundle che può essere trattato come un singolo file. La minimizzazione condensa i file JavaScript e CSS rimuovendo spazi vuoti e altri caratteri non necessari. Queste funzionalità funzionano con Web Form, ASP.NET MVC e pagine Web.

I bundle vengono creati utilizzando la classe Bundle o una delle relative classi figlio, ScriptBundle e StyleBundle. Dopo aver configurato un'istanza di un pacchetto, il pacchetto viene reso disponibile alle richieste in ingresso semplicemente aggiungendolo a un'istanza globale di BundleCollection. Nei modelli predefiniti, la configurazione del bundle viene eseguita in un file BundleConfig.In the default templates, bundle configuration is performed in a BundleConfig file. Questa configurazione predefinita crea pacchetti per tutti gli script di base e i file css utilizzati dai modelli.

I pacchetti sono referenziati dall'interno delle visualizzazioni utilizzando uno dei due metodi helper possibili. Per supportare il rendering di markup diverso per un bundle in modalità di debug e di rilascio, le classi ScriptBundle e StyleBundle dispongono del metodo helper Render. In modalità di debug, Render genererà il markup per ogni risorsa nel bundle. In modalità di rilascio, Render genererà un singolo elemento di markup per l'intero bundle. È possibile alternare tra la modalità di debug e la modalità di rilascio modificando l'attributo debug dell'elemento compilation in web.config, come illustrato di seguito:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Inoltre, l'abilitazione o la disabilitazione dell'ottimizzazione può essere impostata direttamente tramite la proprietà BundleTable.EnableOptimizations.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Quando i file vengono aggregati, vengono prima ordinati alfabeticamente (il modo in cui vengono visualizzati in **Esplora soluzioni).** Vengono quindi organizzati in modo che le librerie note e le relative estensioni personalizzate (ad esempio jQuery, MooTools e Dojo) vengano caricate per prime. Ad esempio, l'ordine finale per l'aggregazione della cartella Script come illustrato in precedenza sarà:

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js
4. a.js (informazioni in due)

I file CSS vengono anche ordinati alfabeticamente e quindi riorganizzati in modo che reset.css e normalize.css vengano prima di qualsiasi altro file. L'ordinamento finale dell'aggregazione della cartella Stili mostrata sopra sarà questo:

1. reset.css
2. content.css (informazioni in stato inquesto e in
3. forms.css (informazioni in forma in questo formato)
4. globals.css
5. menu.css
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Miglioramenti delle prestazioni per l'hosting Web

In .NET Framework 4.5 e Windows 8 vengono introdotte funzionalità che consentono di ottenere un aumento significativo delle prestazioni per i carichi di lavoro dei server Web. Ciò include una riduzione (fino al 35%) sia nel tempo di avvio che nel footprint di memoria dei siti di web hosting che utilizzano ASP.NET.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Fattori chiave delle prestazioni

Idealmente, tutti i siti web dovrebbero essere attivi e in memoria per assicurare una risposta rapida alla richiesta successiva, ogni volta che si tratta. I fattori che possono influire sulla velocità di risposta del sito includono:Factors that can affect site responsiveness include:

- Tempo necessario per il riavvio di un sito dopo il riciclo di un pool di app. Questo è il tempo necessario per avviare un processo del server web per il sito quando gli assembly del sito non sono più in memoria. (Gli assembly della piattaforma sono ancora in memoria, poiché vengono utilizzati da altri siti.) Questa situazione è denominata "sito freddo, avvio del framework a caldo" o semplicemente "avvio del sito freddo".
- Quanta memoria occupa il sito. I termini per questo sono "consumo di memoria per sito" o "working set non condiviso".

I nuovi miglioramenti delle prestazioni si concentrano su entrambi questi fattori.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Requisiti per le nuove funzionalità delle prestazioniRequirements for New Performance Features

I requisiti per le nuove funzionalità possono essere suddivisi in queste categorie:

- Miglioramenti eseguiti in .NET Framework 4.
- Miglioramenti che richiedono .NET Framework 4.5, ma possono essere eseguiti in qualsiasi versione di Windows.
- Miglioramenti disponibili solo con .NET Framework 4.5 in esecuzione in Windows 8.

Le prestazioni aumentano a ogni livello di miglioramento che è possibile abilitare.

Alcuni dei miglioramenti di .NET Framework 4.5 sfruttano le funzionalità di prestazioni più ampie che si applicano ad altri scenari.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Condivisione di assembly comuni

**Requisito**: .NET Framework 4 e Visual Studio 11 Developer Preview SDK

Siti diversi in un server spesso utilizzano gli stessi assembly di supporto (ad esempio, assembly da un kit di avvio o un'applicazione di esempio). Ogni sito ha la propria copia di questi assembly nella directory Bin. Anche se il codice oggetto per gli assembly è identico, sono assembly fisicamente separati, pertanto ogni assembly deve essere letto separatamente durante l'avvio a freddo del sito e mantenuto separatamente in memoria.

La nuova funzionalità di stagione risolve questa inefficienza e riduce sia i requisiti di RAM che i tempi di caricamento. L'interning consente a Windows di conservare una singola copia di ogni assieme nel file system e i singoli assembly nelle cartelle Bin del sito vengono sostituiti con collegamenti simbolici alla singola copia. Se un singolo sito necessita di una versione distinta dell'assieme, il collegamento simbolico viene sostituito dalla nuova versione dell'assembly e solo tale sito è interessato.

La condivisione di assembly tramite\_collegamenti simbolici richiede un nuovo strumento denominato aspnet intern.exe, che consente di creare e gestire l'archivio di assembly interinati. Viene fornito come parte di Visual Studio 11 Developer Preview SDK. (Tuttavia, funzionerà su un sistema che dispone solo di.NET Framework 4 installato, supponendo che sia stato installato [l'aggiornamento](https://support.microsoft.com/kb/2468871)più recente.)

Per assicurarsi che tutti gli assembly idonei\_siano stati internati, eseguire periodicamente aspnet intern.exe (ad esempio, una volta alla settimana come operazione pianificata). Un uso tipico è il seguente:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Per visualizzare tutte le opzioni, eseguire lo strumento senza argomenti.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Utilizzo della compilazione JIT multi-core per un avvio più rapido

**Requisito**: .NET Framework 4.5Requirement : .NET Framework 4.5

Per l'avvio a freddo del sito, non solo gli assembly devono essere letti dal disco, ma il sito deve essere compilato tramite JIT. Per un sito complesso, questo può aggiungere ritardi significativi. Una nuova tecnica generica in .NET Framework 4.5.NET Framework 4.5 riduce questi ritardi diffondendo JIT-compilation tra i core del processore disponibili. Lo fa il più presto possibile utilizzando le informazioni raccolte durante i precedenti lanci del sito. Funzionalità implementata dal metodo [System.Runtime.ProfileOptimization.StartProfile.](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx)

La compilazione JIT con più core è abilitata per impostazione predefinita in ASP.NET, pertanto non è necessario eseguire alcuna operazione per sfruttare questa funzionalità. Se si desidera disattivare questa funzionalità, effettuare la seguente impostazione nel file Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Ottimizzazione dell'operazione di Garbage Collection per ottimizzare la memoria

**Requisito**: .NET Framework 4.5Requirement : .NET Framework 4.5

Una volta che un sito è in esecuzione, il suo utilizzo dell'heap Garbage Collector (GC) può essere un fattore significativo nel consumo di memoria. Come qualsiasi Garbage Collector, .NET Framework GC effettua compromessi tra il tempo della CPU (frequenza e significato delle raccolte) e il consumo di memoria (spazio aggiuntivo utilizzato per oggetti nuovi, liberati o liberi). Per le versioni precedenti, sono state fornite indicazioni su come configurare il catalogo globale per ottenere il giusto equilibrio (ad esempio, vedere [ASP.NET 2.0/3.5 Shared Hosting Configuration](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

Per .NET Framework 4.5, anziché più impostazioni autonome, è disponibile un'impostazione di configurazione definita dal carico di lavoro che abilita tutte le impostazioni GC consigliate in precedenza, nonché una nuova ottimizzazione che offre prestazioni aggiuntive per il working set per sito.

Per abilitare l'ottimizzazione della memoria GC, aggiungere l'impostazione seguente al file di configurazione di Windows:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

Se si ha familiarità con le indicazioni precedenti per le modifiche apportate a aspnet.config, tenere presente che questa impostazione sostituisce le impostazioni precedenti, ad esempio non è necessario impostare gcServer, gcConcurrent e così via. Non è necessario rimuovere le vecchie impostazioni.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Prelettura per applicazioni Web

**Requisito**: .NET Framework 4.5 in esecuzione su Windows 8

Per diverse versioni, Windows ha incluso una tecnologia nota come [prefetcher](http://en.wikipedia.org/wiki/Prefetcher) che riduce il costo di lettura del disco di avvio dell'applicazione. Poiché l'avvio a freddo è un problema prevalentemente per le applicazioni client, questa tecnologia non è stata inclusa in Windows Server, che include solo i componenti essenziali per un server. La prelettura è ora disponibile nell'ultima versione di Windows Server, dove è possibile ottimizzare il lancio di singoli siti Web.

Per Windows Server, il prefetcher non è abilitato per impostazione predefinita. Per abilitare e configurare il prefetcher per l'hosting Web ad alta densità, eseguire il seguente set di comandi dalla riga di comando:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Quindi, per integrare il prefetcher con ASP.NET applicazioni, aggiungere quanto segue al file Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>Web Form ASP.NET

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Controlli dati fortemente tipizzati

In ASP.NET 4.5, Web Form include alcuni miglioramenti per l'utilizzo dei dati. Il primo miglioramento è controlli dati fortemente tipizzati. Per i controlli Web Form nelle versioni precedenti di ASP.NET, si visualizza un valore associato a dati utilizzando *Eval* e un'espressione di associazione dati:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Per l'associazione dati bidirezionale, si usa *Bind*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

In fase di esecuzione, queste chiamate utilizzano la reflection per leggere il valore del membro specificato e quindi visualizzare il risultato nel markup. Questo approccio semplifica l'associazione dei dati a dati arbitrari e non modellati.

Tuttavia, le espressioni di associazione dati come questa non supportano funzionalità come IntelliSense per i nomi dei membri, l'esplorazione (ad esempio Vai a definizione) o il controllo in fase di compilazione per questi nomi.

Per risolvere questo problema, ASP.NET 4.5 aggiunge la possibilità di dichiarare il tipo di dati dei dati a cui è associato un controllo. A tale scopo, utilizzare la nuova proprietà *ItemType.* Quando si imposta questa proprietà, nell'ambito delle espressioni di associazione dati sono disponibili due nuove variabili tipizzate: *Item* e *BindItem*. Poiché le variabili sono fortemente tipizzate, si ottengono tutti i vantaggi dell'esperienza di sviluppo di Visual Studio.

Per le espressioni di associazione dati bidirezionali, usare la variabile *BindItem:For* two-way data-binding expressions, use the BindItem variable:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]

La maggior parte dei controlli nel framework Web Form ASP.NET che supportano l'associazione dati sono stati aggiornati per supportare il *ItemType* proprietà.

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Associazione di modelli

L'associazione di modelli estende l'associazione dati in ASP.NET controlli Web Form per utilizzare l'accesso ai dati incentrato sul codice. Incorpora concetti dal controllo *ObjectDataSource* e dall'associazione di modelli in ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Selezione dei dati

Per configurare un controllo dati per l'utilizzo dell'associazione di modelli per selezionare i dati, impostare la proprietà *SelectMethod* del controllo sul nome di un metodo nel codice della pagina. Il controllo dati chiama il metodo nel momento appropriato del ciclo di vita della pagina e associa automaticamente i dati restituiti. Non è necessario chiamare in modo esplicito il *DataBind* metodo.

Nell'esempio seguente, il controllo *GridView* è configurato per utilizzare un metodo denominato *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Creare il *GetCategories* metodo nel codice della pagina. Per una semplice operazione di selezione, il metodo non richiede parametri e deve restituire un *oggetto IEnumerable* o *IQueryable.For* a simple select operation, the method needs no parameters and should return an IEnumerable or IQueryable object. Se la nuova proprietà *ItemType* è impostata (che consente espressioni di associazione dati fortemente tipizzate, come illustrato in [Controlli dati fortemente tipizzati](#_Toc318097386) in precedenza), devono essere restituite le versioni generiche di queste interfacce, ovvero *IEnumerable&lt;&gt; T* o *IQueryable&lt;T&gt;*, con il parametro *T* corrispondente al tipo della proprietà *ItemType* (ad esempio, *IQueryable&lt;Category&gt;*).

Nell'esempio seguente viene illustrato il codice per un *GetCategories* metodo. In questo esempio viene utilizzato il modello Entity Framework Code First con il database di esempio Northwind. Il codice assicura che la query restituisca i dettagli dei prodotti correlati per ogni categoria tramite *il* include metodo. In questo modo si garantisce che l'elemento *TemplateField* nel markup visualizzi il numero di prodotti in ogni categoria senza che sia [necessario selezionare n.1](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Quando la pagina viene eseguita, il controllo *GridView* chiama automaticamente il metodo *GetCategories* ed esegue il rendering dei dati restituiti utilizzando i campi configurati:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Poiché il metodo select restituisce un oggetto *IQueryable,* il controllo *GridView* può modificare ulteriormente la query prima di eseguirla. Ad esempio, il controllo *GridView* può aggiungere espressioni di query per l'ordinamento e il paging all'oggetto *IQueryable* restituito prima che venga eseguito, in modo che tali operazioni vengano eseguite dal provider LINQ sottostante. In questo caso, Entity Framework garantirà che tali operazioni vengano eseguite nel database.

Nell'esempio seguente viene illustrato il controllo GridView modificato per consentire l'ordinamento e il paging:The following example shows the *GridView* control modified to allow sorting and paging:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Ora, quando la pagina viene eseguita, il controllo può assicurarsi che venga visualizzata solo la pagina di dati corrente e che sia ordinata in base alla colonna selezionata:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Per filtrare i dati restituiti, i parametri devono essere aggiunti al metodo select. Questi parametri verranno popolati dall'associazione del modello in fase di esecuzione ed è possibile utilizzarli per modificare la query prima di restituire i dati.

Si supponga, ad esempio, di voler consentire agli utenti di filtrare i prodotti immettendo una parola chiave nella stringa di query. È possibile aggiungere un parametro al metodo e aggiornare il codice per utilizzare il valore del parametro:You can add a parameter to the method and update the code to use the parameter value:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Questo codice include *un'espressione Where* se viene fornito un valore per la *parola chiave* e quindi restituisce i risultati della query.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Fornitori di valori

L'esempio precedente non era specifico sulla posizione da cui proveniva il valore del parametro *keyword.* Per indicare queste informazioni, è possibile utilizzare un attributo parameter. Per questo esempio, è possibile utilizzare la classe *QueryStringAttribute* nello spazio dei nomi *System.Web.ModelBinding:*

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

In questo modo viene indicato all'associazione del modello di tentare di associare un valore dalla stringa di query al parametro della *parola chiave* in fase di esecuzione. (Questo potrebbe comportare l'esecuzione di conversione del tipo, anche se in questo caso non.) Se non è possibile fornire un valore e il tipo non è nullable, viene generata un'eccezione.

Le origini dei valori per questi metodi sono denominate provider di valori e gli attributi di parametro che indicano quale provider di valori utilizzare sono detti attributi del provider di valori. I Web Form includeranno provider di valori e attributi corrispondenti per tutte le origini tipiche dell'input dell'utente in un'applicazione Web Form, ad esempio la stringa di query, i cookie, i valori dei form, i controlli, lo stato di visualizzazione, lo stato sessione e le proprietà del profilo. È inoltre possibile scrivere provider di valori personalizzati.

Per impostazione predefinita, il nome del parametro viene utilizzato come chiave per trovare un valore nella raccolta di provider di valori. Nell'esempio, il codice cercherà un valore di stringa di query denominato parola chiave (ad esempio, /default.aspx?parola chiave .chef). È possibile specificare una chiave personalizzata passandola come argomento all'attributo del parametro. Ad esempio, per utilizzare il valore della variabile di stringa di query denominata q, è possibile eseguire questa operazione:For example, to use the value of the query-string variable named q, you could do this:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Se questo metodo si trova nel codice della pagina, gli utenti possono filtrare i risultati passando una parola chiave utilizzando la stringa di query:If this method is in the page's code, users can filter the results by passing a keyword using the query string:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

L'associazione di modelli esegue molte attività che altrimenti sarebbe necessario codificare manualmente: leggere il valore, cercare un valore null, tentare di convertirlo nel tipo appropriato, verificare se la conversione ha avuto esito positivo e infine, utilizzando il valore nella query. L'associazione di modelli consente di ottenere molto meno codice e di riutilizzare la funzionalità in tutta l'applicazione.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Filtraggio in base ai valori di un controlloFiltering by values from a control

Si supponga di voler estendere l'esempio per consentire all'utente di scegliere un valore di filtro da un elenco a discesa. Aggiungere l'elenco a discesa seguente al markup e configurarlo per ottenere i dati da un altro metodo utilizzando la proprietà *SelectMethod:*

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

In genere si aggiunge anche un EmptyDataTemplate elemento per il *Controllo GridView* in modo che il controllo verrà visualizzato un messaggio se non vengono trovati prodotti corrispondenti:Typically also you would also add an *EmptyDataTemplate* element to the GridView control so that the control will display a message if no matching products are found:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

Nel codice della pagina aggiungere il nuovo metodo select per l'elenco a discesa:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Infine, aggiornare il *GetProducts* selezionare metodo per accettare un nuovo parametro che contiene l'ID della categoria selezionata dall'elenco a discesa:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Ora, quando la pagina viene eseguita, gli utenti possono selezionare una categoria dall'elenco a discesa e il controllo *GridView* viene automaticamente riassociato per visualizzare i dati filtrati. Ciò è possibile perché l'associazione del modello tiene traccia dei valori dei parametri per i metodi select e rileva se un valore di parametro è stato modificato dopo un postback. In tal caso, l'associazione del modello forza l'associazione dei dati associata ai dati.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>Espressioni di associazione dati codificate HTMLHTML Encoded Data-Binding Expressions

È ora possibile codificare in HTML il risultato delle espressioni di associazione dati. Aggiungere i due punti (:) alla fine del &lt;prefisso % :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Convalida discreta

È ora possibile configurare i controlli validator incorporati per l'utilizzo di JavaScript discreto per la logica di convalida lato client. Ciò riduce significativamente la quantità di JavaScript sottoposto a rendering inline nel markup della pagina e riduce le dimensioni complessive della pagina. È possibile configurare JavaScript discreto per i controlli validator in uno dei modi seguenti:You can configure unbtrusve JavaScript for validator controls in any of these ways:

- Globalmente aggiungendo la seguente impostazione all'elemento * &lt;appSettings&gt; * nel file Web.config: 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Globalmente impostando la proprietà statica *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* su *UnobtrusiveValidationMode.WebForms* (in genere nel metodo *Application\_Start* nel file Global.asax).
- Singolarmente per una pagina impostando la nuova proprietà *UnobtrusiveValidationMode* della classe *Page su* *UnobtrusiveValidationMode.WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>Aggiornamenti HTML5

Sono stati apportati alcuni miglioramenti ai controlli server Web Form per sfruttare le nuove funzionalità di HTML5:

- La proprietà *TextMode* del controllo *TextBox* è stata aggiornata per supportare i nuovi tipi di input HTML5, ad esempio *email,* *datetime*e così via.
- Il controllo *FileUpload* ora supporta il caricamento di più file da browser che supportano questa funzionalità HTML5.
- I controlli validator supportano ora la convalida degli elementi di input HTML5.
- I nuovi elementi HTML5 che hanno attributi che rappresentano un URL ora supportano runat "server". Di conseguenza, è possibile utilizzare ASP.NET convenzioni nei percorsi URL, ad &lt;esempio l'operatore , per rappresentare la radice dell'applicazione (ad esempio, video runat , "server" src , "/myVideo.wmv" / ).&gt;
- Il controllo *UpdatePanel* è stato corretto per supportare la registrazione di campi di input HTML5.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 Beta è ora incluso in Visual Studio 11 Beta. ASP.NET MVC è un framework per lo sviluppo di applicazioni Web altamente tecuabili e gestibili sfruttando il modello Model-View-Controller (MVC). ASP.NET MVC 4 semplifica la creazione di applicazioni per il Web mobile e include ASP.NETAPI Web, che consente di creare servizi HTTP in grado di raggiungere qualsiasi dispositivo. Per ulteriori informazioni, vedere la [ASP.NET note sulla versione di MVC 4](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>Pagine Web ASP.NET 2

Le nuove funzionalità includono quanto segue:

- Modelli di sito nuovi e aggiornati.
- Aggiunta della convalida lato server e lato client tramite l'helper *di convalida.*
- La possibilità di registrare gli script utilizzando un gestore di risorse.
- Abilitazione degli account di accesso da Facebook e da altri siti tramite OAuth e OpenID.
- Aggiunta di mappe tramite l'helper *Mappe.*
- Esecuzione affiancata di pagine Web.
- Rendering delle pagine per dispositivi mobili.

Per ulteriori informazioni su queste funzionalità ed esempi di codice a pagina intera, vedere [Le funzionalità principali in Pagine Web 2 Beta](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

In questa sezione vengono fornite informazioni sui miglioramenti apportati allo sviluppo Web in Visual Web Developer 11 Beta e Visual Studio 2012 Release Candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Condivisione di progetti tra Visual Studio 2010 e Visual Studio 2012 Release Candidate (compatibilità dei progetti)

Fino a quando Visual Studio 2012 Release Candidate, l'apertura di un progetto esistente in una versione più recente di Visual Studio ha avviato la Conversione guidata. Ciò ha forzato l'aggiornamento del contenuto (risorse) di un progetto e di una soluzione a nuovi formati non compatibili con le versioni precedenti. Pertanto, dopo la conversione non è stato possibile aprire il progetto nella versione precedente di Visual Studio.

Molti clienti ci hanno detto che questo non era l'approccio giusto. In Visual Studio 11 Beta è ora supportata la condivisione di progetti e soluzioni con Visual Studio 2010 SP1. Ciò significa che se si apre un progetto 2010 in Visual Studio 2012 Release Candidate, sarà comunque possibile aprire il progetto in Visual Studio 2010 SP1.

> [!NOTE]
> Alcuni tipi di progetti non possono essere condivisi tra Visual Studio 2010 SP1 e Visual Studio 2012 Release Candidate. Questi includono alcuni progetti meno recenti (ad esempio ASP.NET progetti MVC 2) o progetti per scopi speciali (ad esempio i progetti di installazione).

Quando si apre un progetto Web di Visual Studio 2010 SP1 per la prima volta in Visual Studio 11 Beta, le seguenti proprietà vengono aggiunte al file di progetto:

- Proprietà FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPercorso

FileUpgradeFlags, UpgradeBackupLocation e OldToolsVersion vengono utilizzati dal processo che aggiorna il file di progetto. Non hanno alcun impatto sull'utilizzo del progetto in Visual Studio 2010.

VisualStudioVersion è una nuova proprietà utilizzata da MSBuild 4.5 che indica la versione di Visual Studio per il progetto corrente. Poiché questa proprietà non esisteva in MSBuild 4.0 (la versione di MSBuild utilizzata da Visual Studio 2010 SP1), viene inserito un valore predefinito nel file di progetto.

Il VSToolsPath proprietà viene utilizzata per determinare il file con estensione targets corretto da importare dal percorso rappresentato dall'impostazione MSBuildExtensionsPath32.

Ci sono anche alcune modifiche relative agli elementi Di importazione. Queste modifiche sono necessarie per supportare la compatibilità tra entrambe le versioni di Visual Studio.These changes are required in order to support compatibility between both versions of Visual Studio.

> [!NOTE]
> Se un progetto viene condiviso tra Visual Studio 2010 SP1 e Visual Studio 11 Beta in\_due computer diversi e se il progetto include un database locale nella cartella Dati app, è necessario assicurarsi che la versione di SQL Server utilizzata dal database sia installata in entrambi i computer.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Modifiche alla configurazione in ASP.NET modelli di sito Web 4.5

Sono state apportate le modifiche seguenti al file *Web.config* predefinito per il sito creato utilizzando i modelli di sito Web in Visual Studio 2012 Release Candidate:

- Nell'elemento, `<httpRuntime>` `encoderType` l'attributo è ora impostato per impostazione predefinita per utilizzare i tipi AntiXSS aggiunti a ASP.NET. Per informazioni dettagliate, consultate [Libreria AntiXSS](#_Toc318097382).
- Anche `<httpRuntime>` nell'elemento, `requestValidationMode` l'attributo è impostato su "4.5". Ciò significa che, per impostazione predefinita, la convalida delle richieste è configurata per utilizzare la convalida posticipata ("lazy"). Per informazioni dettagliate, vedere [Nuove funzionalità di convalida](#_Toc318097379)delle richieste di ASP.NET .
- L'elemento `<modules>` `<system.webServer>` della sezione non `runAllManagedModulesForAllRequests` contiene un attributo. Il valore predefinito è false. Ciò significa che se si utilizza una versione di IIS 7 che non è stata aggiornata a SP1, potrebbero verificarsi problemi con il routing in un nuovo sito. Per ulteriori informazioni, vedere [Supporto nativo in IIS 7 per il routing ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine).

Queste modifiche non influiscono sulle applicazioni esistenti. Tuttavia, potrebbero rappresentare una differenza di comportamento tra i siti Web esistenti e i nuovi siti Web creati per ASP.NET 4.5 utilizzando i nuovi modelli.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Supporto nativo in IIS 7 per il routing ASP.NET

Non si tratta di una modifica a ASP.NET in quanto tale, ma di una modifica nei modelli per i nuovi progetti di siti Web che può influire sull'utente se si lavora su una versione di IIS 7 a cui non è stato applicato l'aggiornamento SP1.

In ASP.NET, è possibile aggiungere la seguente impostazione di configurazione alle applicazioni per supportare il routing:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Quando **runAllManagedModulesForAllRequests** è true, `http://mysite/myapp/home` un URL come va a ASP.NET, anche se nell'URL non è presente alcun atratti, *aspx,* *MVC*o estensione simile.

Un aggiornamento apportato a IIS 7 rende superflua l'impostazione **runAllManagedModulesForAllRequests** e supporta ASP.NET routing in modo nativo. Per informazioni sull'aggiornamento, vedere l'articolo del supporto tecnico Microsoft [È disponibile un aggiornamento che consente a determinati gestori IIS 7.0 o IIS 7.5](https://support.microsoft.com/kb/980368)di gestire le richieste i cui URL non terminano con un punto .

Se il sito Web è in esecuzione in IIS 7 e IIS è stato aggiornato, non è necessario impostare **runAllManagedModulesForAllRequests** su true. Infatti, l'impostazione su true non è consigliata, perché aggiunge un sovraccarico di elaborazione non necessario alla richiesta. Quando questa impostazione è vera, anche tutte le richieste, incluse quelle per *htm*, *jpg*e altri file statici, passano attraverso la pipeline di richiesta ASP.NET.

Se si crea un nuovo sito Web ASP.NET 4.5 utilizzando i modelli forniti in Visual Studio 2012 RC, la configurazione per il sito Web non include l'impostazione **runAllManagedModulesForAllRequests.** Ciò significa che per impostazione predefinita l'impostazione è false.

Se quindi si esegue il sito Web in Windows 7 senza SP1 installato, IIS 7 non includerà l'aggiornamento richiesto. Di conseguenza, il routing non funzionerà e verranno visualizzati errori. Se si verifica un problema a i casi in cui il routing non funziona, è possibile effettuare le seguenti operazioni:

- Aggiornare Windows 7 a SP1, che aggiungerà l'aggiornamento a IIS 7.
- Installare l'aggiornamento descritto nell'articolo del supporto tecnico Microsoft elencato in precedenza.
- Impostare **runAllManagedModulesForAllRequests** su true nel file Web.config del sito Web. Si noti che questo aggiungerà un certo sovraccarico alle richieste.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>Editor HTML

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Attività intelligenti

Nella visualizzazione Progettazione, alle proprietà complesse dei controlli server sono spesso associate finestre di dialogo e procedure guidate per semplificarne l'impostazione. Ad esempio, è possibile utilizzare una finestra di dialogo speciale per aggiungere un'origine dati a un controllo Repeater o aggiungere colonne a un *controllo GridView.For* example, you can use a special dialog box to add a data source to a *Repeater* control or add columns to a GridView control.

Tuttavia, questo tipo di Guida dell'interfaccia utente per le proprietà complesse non è stato disponibile nella visualizzazione Origine.However, this type of UI help for complex properties has not been available in Source view. Pertanto, Visual Studio 11 introduce smart Tasks per la visualizzazione origine. Le attività intelligenti sono tasti di scelta rapida in grado di riconoscere il contesto per le funzionalità di uso comune negli editor di Visual Basic e di C.

Per ASP.NET controlli Web Form, le attività intelligenti vengono visualizzate nei tag server come un piccolo glifo quando il punto di inserimento si trova all'interno dell'elemento:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

L'attività smart si espande quando si fa clic sul glifo o si preme CTRL . (punto), proprio come negli editor di codice. Vengono quindi visualizzati collegamenti simili alle attività intelligenti nella visualizzazione Progettazione.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Ad esempio, l'attività intelligente nella figura precedente mostra le opzioni attività GridView.For example, the Smart Task in the previous illustration shows the GridView Tasks options. Se si sceglie Modifica colonne, viene visualizzata la seguente finestra di dialogo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Compilando la finestra di dialogo vengono impostate le stesse proprietà che è possibile impostare nella vista Progettazione. Quando si fa clic su OK, il markup per il controllo viene aggiornato con le nuove impostazioni:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>Supporto WAI-ARIA

La scrittura di siti web accessibili sta diventando sempre più importante. Lo standard di [accessibilità WAI-ARIA](http://www.w3.org/WAI/intro/aria) definisce il modo in cui gli sviluppatori devono scrivere siti Web accessibili. Questo standard è ora completamente supportato in Visual Studio.This standard is now fully supported in Visual Studio.

Ad esempio, l'attributo role dispone ora di IntelliSense completo:For example, the *role* attribute now has full IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

Lo standard WAI-ARIA introduce anche gli attributi che sono preceduti da *aria,* che consentono di aggiungere semantica a un documento HTML5. Visual Studio supporta completamente anche questi attributi *aria-:Visual* Studio also fully supports these aria- attributes:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Nuovi frammenti HTML5

Per rendere più veloce e più semplice scrivere il markup HTML5 di uso comune, Visual Studio include una serie di frammenti. Un esempio è lo snippet video:An example is the video snippet:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Per richiamare il frammento di codice, premere TAB due volte quando l'elemento è selezionato in IntelliSense:To invoke the snippet, press Tab twice when the element is selected in IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Questo produce un frammento che è possibile personalizzare.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Estrai nel controllo utente

Nelle pagine Web di grandi dimensioni, può essere una buona idea spostare singole parti in controlli utente. Questa forma di refactoring può contribuire ad aumentare la leggibilità della pagina e può semplificare la struttura della pagina.

Per semplificare questa operazione, quando si modificano le pagine Web Form nella visualizzazione Origine, è ora possibile selezionare il testo in una pagina, fare clic con il pulsante destro del mouse su di esso e quindi scegliere Estrai nel controllo utente:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>IntelliSense per le pepite di codice negli attributi

Visual Studio ha sempre fornito IntelliSense per le pepite di codice lato server in qualsiasi pagina o controllo. Ora Visual Studio include IntelliSense per i clamore di codice anche negli attributi HTML.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

In questo modo è più semplice creare espressioni di associazione dati:This easier to create data-binding expressions:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Ridenominazione automatica del tag corrispondente quando si rinomina un tag di apertura o di chiusura

Se si rinomina un elemento HTML (ad esempio, si modifica un tag *div* in tag di *intestazione),* anche il tag di apertura o di chiusura corrispondente cambia in tempo reale.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Ciò consente di evitare l'errore in cui si dimentica di modificare un tag di chiusura o di quello sbagliato.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Generazione del gestore eventi

Visual Studio include ora funzionalità nella visualizzazione Origine che consentono di scrivere gestori eventi e associarli manualmente. Se si modifica il nome di un &lt;evento nella&gt;visualizzazione Origine, IntelliSense visualizza Crea nuovo evento , che creerà un gestore eventi nel codice della pagina con la firma corretta:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Per impostazione predefinita, il gestore eventi utilizzerà l'ID del controllo per il nome del metodo di gestione degli eventi:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

Il gestore dell'evento risultante sarà simile al seguente (in questo caso, in C .NET):The resulting event handler will look like this (in this case, in C's):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Rientro intelligente

Quando si preme INVIO mentre si è all'interno di un elemento HTML vuoto, l'editor inserirà il punto di inserimento nella posizione corretta:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Se si preme INVIO in questa posizione, il tag di chiusura viene spostato verso il basso e rientrato in base al tag di apertura. Anche il punto di inserimento è rientrato:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Riduci automaticamente il completamento delle istruzioni

L'elenco IntelliSense in Visual Studio ora filtra in base a ciò che si digita in modo che visualizzi solo le opzioni pertinenti:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense inoltre filtra in base alla combinazione di maiuscole e minuscole delle singole parole nell'elenco IntelliSense. Ad esempio, se si digita "dl", vengono visualizzati sia dl che asp:DataList:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Questa funzionalità rende più veloce ottenere il completamento delle istruzioni per gli elementi noti.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript (editor)

L'editor JavaScript in Visual Studio 2012 Release Candidate è completamente nuovo e migliora notevolmente l'esperienza di utilizzo di JavaScript in Visual Studio.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Struttura del codice

Le aree di struttura vengono ora create automaticamente per tutte le funzioni, consentendo di comprimere parti del file che non riguardano lo stato attivo.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Corrispondenza parentesi graffe

Quando si posiziona il punto di inserimento su una parentesi graffa di apertura o di chiusura, l'editor evidenzia quello corrispondente.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Vai a definizione

Il comando Vai a definizione consente di passare all'origine per una funzione o una variabile.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>Supporto ECMAScript5

L'editor supporta la nuova sintassi e le NUOVE API in ECMAScript5, la versione più recente dello standard che descrive il linguaggio JavaScript.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>DOM IntelliSense

IntelliSense per le API DOM è stato migliorato, con il supporto per molte nuove API HTML5, tra cui *querySelector*, DOM Storage, messaggistica tra documenti e *canvas*. DOM IntelliSense è ora guidato da un singolo file JavaScript semplice, anziché da una definizione di libreria dei tipi nativa. Questo rende facile da estendere o sostituire.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>Sovraccarichi della firma VSDOC

I commenti IntelliSense dettagliati possono ora essere dichiarati per overload separati di funzioni JavaScript usando il nuovo elemento * &lt;signature,&gt; * come illustrato nell'esempio seguente:Detailed IntelliSense comments can now be declared for separate overloads of JavaScript functions by using the new signature element, as shown in this example:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Riferimenti impliciti

È ora possibile aggiungere file JavaScript a un elenco centrale che verrà incluso in modo implicito nell'elenco dei file che qualsiasi file JavaScript o bloccare i riferimenti, il che significa che si otterrà IntelliSense per il suo contenuto. Ad esempio, è possibile aggiungere file jQuery all'elenco centrale dei file e si otterrà IntelliSense per le funzioni jQuery in qualsiasi &lt;blocco&gt;JavaScript di file, indipendentemente dal fatto che vi sia stato fatto riferimento in modo esplicito (utilizzando /// reference / ) o meno.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>CSS (editor)

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Riduci automaticamente il completamento delle istruzioni

L'elenco IntelliSense per CSS ora filtra in base alle proprietà e ai valori CSS supportati dallo schema selezionato.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense supporta anche le ricerche di maiuscole/minuscole:IntelliSense also supports title case searches:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Rientro gerarchico

L'editor CSS utilizza il rientro per visualizzare le regole gerarchiche, che offre una panoramica dell'organizzazione logica delle regole a catena. Nell'esempio seguente, il #list un selettore è un elemento figlio a catena dell'elenco ed è pertanto rientrato.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

Nell'esempio seguente viene illustrata l'ereditarietà più complessa:The following example shows more complex inheritance:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

Il rientro di una regola è determinato dalle regole padre. Il rientro gerarchico è abilitato per impostazione predefinita, ma è possibile disattivarlo nella finestra di dialogo Opzioni (Strumenti, Opzioni dalla barra dei menu):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>Supporto per hack CSS

L'analisi di centinaia di file CSS reali mostra che gli hack CSS sono molto comuni e ora Visual Studio supporta quelli più utilizzati. Questo supporto include IntelliSense e\*la convalida degli\_hack di proprietà asterisco ( ) e di sottolineatura ( ):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Sono supportati anche gli hack tipici del selettore in modo che il rientro gerarchico venga mantenuto anche quando vengono applicati. Un tipico hack selettore utilizzato per indirizzare Internet Explorer 7 è quello di anteporre un selettore con * \*:primo-figlio - html*. L'utilizzo di tale regola manterrà il rientro gerarchico:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Schemi specifici del fornitore (-moz-, -webkit)

CSS3 introduce molte proprietà che sono state implementate da diversi browser in momenti diversi. In questo modo in precedenza forzato gli sviluppatori a codice per browser specifici utilizzando la sintassi specifica del fornitore. Queste proprietà specifiche del browser sono ora incluse in IntelliSense.These browser-specific properties are now included in IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Supporto per i commenti e gli utenti

È ora possibile aggiungere e rimuovere il commento dalle regole CSS utilizzando gli stessi tasti di scelta rapida utilizzati nell'editor di codice (CTRL-K,C per commentare e Ctrl ,K per rimuovere il commento).

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Selezione colori

Nelle versioni precedenti di Visual Studio, IntelliSense per gli attributi correlati al colore consisteva in un elenco a discesa di valori di colore denominati. Tale elenco è stato sostituito da un selettore colori completo.

Quando si immette un valore di colore, il selettore colore viene visualizzato automaticamente e viene visualizzato un elenco dei colori utilizzati in precedenza seguiti da una tavolozza di colori predefinita. È possibile selezionare un colore utilizzando il mouse o la tastiera.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

L'elenco può essere espanso in un selettore colore completo. Il selettore consente di controllare il canale alfa convertendo automaticamente qualsiasi colore in RGBA quando spostate il cursore di opacità:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Frammenti di codice

I frammenti nell'editor CSS semplificano e velocizzano la creazione di stili tra browser. Molte proprietà CSS3 che richiedono impostazioni specifiche del browser sono state ora inserite in frammenti.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

I frammenti CSS supportano scenari avanzati (ad esempio le media query CSS3) digitando il simbolo di chiocciola (-) che mostra l'elenco IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Quando si @media seleziona il valore e si preme TAB, l'editor CSS inserisce il frammento di codice seguente:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Come per i frammenti di codice, è possibile creare frammenti CSS personalizzati.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Aree personalizzate

Le aree di codice denominate, che sono già disponibili nell'editor di codice, sono ora disponibili per la modifica CSS. Ciò consente di raggruppare facilmente i blocchi di stile correlati.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Quando un'area è compressa, viene visualizzato il nome dell'area:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Controllo pagina

Controllo pagina è uno strumento che esegue il rendering di una pagina Web (HTML, Web Form, ASP.NET MVC o pagine Web) nell'IDE di Visual Studio e consente di esaminare sia il codice sorgente che l'output risultante. Per ASP.NET pagine, Controllo pagina consente di determinare quale codice lato server ha prodotto il markup HTML di cui viene eseguito il rendering nel browser.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Per ulteriori informazioni su Controllo pagina, vedere le seguenti esercitazioni:

- Utilizzo di Controllo pagina in [ASP.NET MVCUsing](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md) Page Inspector in ASP.NET MVC
- Utilizzo di Controllo pagina in [ASP.NET Web Form](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Pubblicazione

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Profili di pubblicazione

In Visual Studio 2010, la pubblicazione di informazioni per i progetti di applicazione Web non viene archiviata nel controllo della versione e non è progettata per la condivisione con altri utenti. In Visual Studio 2012 Release Candidate, il formato del profilo di pubblicazione è stato modificato. È stato fatto un artefatto team, ed è ora facile sfruttare da compilazioni basate su MSBuild. Le informazioni di configurazione della compilazione sono disponibili nella finestra di dialogo Pubblica in modo che sia possibile passare facilmente da una configurazione di compilazione all'altra prima della pubblicazione.

I profili di pubblicazione vengono archiviati nella cartella PublishProfiles. Il percorso della cartella dipende dal linguaggio di programmazione in uso:

- C'è: Proprietà/PublishProfiles
- Visual Basic: Progetto personale, PublishProfiles

Ogni profilo è un file MSBuild.Each profile is an MSBuild file. Durante la pubblicazione, questo file viene importato nel file MSBuild del progetto. In Visual Studio 2010, se si desidera apportare modifiche al processo di pubblicazione o pacchetto, è necessario inserire le personalizzazioni in un file denominato **NomeProgetto**.wpp.targets. Questa operazione è ancora supportata, ma è ora possibile inserire le personalizzazioni nel profilo di pubblicazione stesso. In questo modo, le personalizzazioni verranno utilizzate solo per tale profilo.

È ora possibile sfruttare anche i profili di pubblicazione da MSBuild.You can now also leverage publish profiles from MSBuild. A tale scopo, utilizzare il comando seguente quando si compila il progetto:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Il valore project.csproj è il percorso del progetto e ProfileName è il nome del profilo da pubblicare. In alternativa, anziché passare il nome del profilo per il *PublishProfile* proprietà, è possibile passare il percorso completo al profilo di pubblicazione.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>ASP.NET precompilazione e unione

Per i progetti di applicazione Web, Visual Studio 2012 Release Candidate aggiunge un'opzione nella pagina delle proprietà Pubblicazione/creazione pacchetto Web che consente di precompilare e unire il contenuto del sito durante la pubblicazione o il pacchetto del progetto. Per visualizzare queste opzioni, fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni, scegliere Proprietà, quindi scegliere la pagina delle proprietà Pubblicazione/creazione pacchetto Web. Nella figura seguente viene illustrata l'opzione Precompilare l'applicazione prima della pubblicazione.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Quando questa opzione è selezionata, Visual Studio precompila l'applicazione ogni volta che si pubblica o si crea un pacchetto dell'applicazione Web. Se si desidera controllare la modalità di precompilazione del sito o la modalità di unione degli assembly, fare clic sul pulsante Avanzate per configurare tali opzioni.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

Il server Web predefinito per il test dei progetti Web in Visual Studio è ora IIS Express. Il server di sviluppo di Visual Studio è ancora un'opzione per il server Web locale durante lo sviluppo, ma IIS Express è ora il server consigliato. L'esperienza di utilizzo di IIS Express in Visual Studio 11 Beta è molto simile all'utilizzo in Visual Studio 2010 SP1.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Dichiarazione di non responsabilità

Il presente documento è una versione preliminare e può essere modificato in modo sostanziale prima della versione finale commerciale del software qui descritto.

Le informazioni contenute in questo documento rappresentano la posizione attuale di Microsoft Corporation sulle problematiche trattate alla data di pubblicazione. Poiché Microsoft deve rispondere alle mutevoli condizioni del mercato, questo non deve essere interpretato come un impegno da parte di Microsoft, che non garantisce l'accuratezza delle informazioni presentate dopo la data di pubblicazione.

Il presente white paper è fornito solo a scopi informativi. MICROSOFT NON RILASCIA ALCUNA GARANZIA, ESPRESSA, IMPLICITA O LEGALE, IN MERITO ALLE INFORMAZIONI DEL PRESENTE DOCUMENTO.

Il rispetto di tutte le applicabili leggi in materia di copyright è esclusivamente a carico dell'utente. Fermi restando tutti i diritti coperti da copyright, nessuna parte di questa documentazione potrà comunque essere riprodotta o inserita in un sistema di riproduzione o trasmessa in qualsiasi forma e con qualsiasi mezzo (in formato elettronico, meccanico, fotocopia, tramite registrazione o altro) per qualsiasi scopo, senza il permesso scritto di Microsoft Corporation.

Microsoft può essere titolare di brevetti, domande di brevetto, marchi, copyright o altri diritti di proprietà intellettuale relativi all'oggetto della presente documentazione. Salvo quanto espressamente previsto in un contratto scritto di licenza Microsoft, la consegna della presente documentazione non implica la concessione di alcuna licenza su tali brevetti, marchi, copyright o altra proprietà intellettuale.

Se non diversamente specificato, le aziende di esempio, le organizzazioni, i prodotti, i nomi di dominio, gli indirizzi e-mail, i loghi, le persone, i luoghi e gli eventi descritti nel presente documento sono fittizi e nessuna associazione con società, organizzazioni, prodotti, nomi di dominio, indirizzo e-mail, logo, persona, luogo o evento è destinata o deve essere dedotta.

© 2012 Microsoft Corporation. Tutti i diritti sono riservati.

Microsoft e Windows sono marchi registrati o marchi di Microsoft Corporation negli Stati Uniti e/o in altri paesi.

I nomi di società e prodotti reali citati nel presente documento possono essere marchi dei rispettivi proprietari.
