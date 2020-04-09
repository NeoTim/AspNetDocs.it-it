---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Gestione degli errori globali in ASP.NET API Web 2 - ASP.NET 4.x
author: davidmatson
description: Panoramica della gestione degli errori globale in ASP.NET API Web 2 per ASP.NET 4.x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 5ff54d2e4ed881ce927d0a401fb79d9b8bc5b8a1
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675423"
---
# <a name="global-error-handling-in-aspnet-web-api-2"></a>Gestione degli errori globali in ASP.NET API Web 2

di [David Matson](https://github.com/davidmatson), [Rick Anderson](https://twitter.com/RickAndMSFT)

In questo argomento viene fornita una panoramica della gestione degli errori globale in ASP.NET API Web 2 per ASP.NET 4.x. Oggi non c'è un modo semplice nell'API Web per registrare o gestire gli errori a livello globale. Alcune eccezioni non gestite possono essere elaborate tramite [filtri eccezioni](exception-handling.md), ma esistono diversi casi che i filtri eccezioni non sono in grado di gestire. Ad esempio:

1. Eccezioni generate dai costruttori dei controller.
2. Eccezioni generate dai gestori di messaggi.
3. Eccezioni generate durante il routing.
4. Eccezioni generate durante la serializzazione del contenuto della risposta.

Vogliamo fornire un modo semplice e coerente per registrare e gestire (ove possibile) queste eccezioni. 

Ci sono due casi principali per la gestione delle eccezioni, il caso in cui siamo in grado di inviare una risposta di errore e il caso in cui tutto quello che possiamo fare è registrare l'eccezione. Un esempio per quest'ultimo caso è quando viene generata un'eccezione nel mezzo del contenuto della risposta di streaming; in questo caso è troppo tardi per inviare un nuovo messaggio di risposta poiché il codice di stato, le intestazioni e il contenuto parziale sono già passati attraverso la rete, quindi interrompiamo semplicemente la connessione. Anche se l'eccezione non può essere gestita per produrre un nuovo messaggio di risposta, è comunque supportata la registrazione dell'eccezione. Nei casi in cui è possibile rilevare un errore, è possibile restituire una risposta di errore appropriata, come illustrato di seguito:In cases where we can detect an error, we can return an appropriate error response as shown in the following:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Opzioni esistenti

Oltre ai [filtri](exception-handling.md) [eccezioni, i gestori di messaggi](../advanced/http-message-handlers.md) possono essere utilizzati oggi per osservare tutte le risposte di livello 500, ma agire su tali risposte è difficile, in quanto mancano di contesto sull'errore originale. I gestori di messaggi hanno anche alcune delle stesse limitazioni dei filtri eccezioni per quanto riguarda i casi che possono gestire. Mentre l'API Web dispone di un'infrastruttura di traccia che acquisisce le condizioni di errore l'infrastruttura di traccia è a scopo diagnostico e non è progettata o adatta per l'esecuzione in ambienti di produzione. La gestione e la registrazione delle eccezioni globali devono essere servizi che possono essere eseguiti durante l'ambiente di produzione ed essere collegati a soluzioni di monitoraggio esistenti , ad esempio [ELMAH](https://code.google.com/p/elmah/).

### <a name="solution-overview"></a>Panoramica della soluzione

 Forniamo due nuovi servizi sostituibili dall'utente, [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) e IExceptionHandler, per registrare e gestire le eccezioni non gestite. I servizi sono molto simili, con due differenze principali:

1. Supportiamo la registrazione di più logger di eccezioni, ma solo un singolo gestore di eccezioni.
2. I logger di eccezioni vengono sempre chiamati, anche se stiamo per interrompere la connessione. I gestori di eccezioni vengono chiamati solo quando siamo ancora in grado di scegliere quale messaggio di risposta inviare.

Entrambi i servizi forniscono l'accesso a un contesto di eccezione contenente informazioni rilevanti dal punto in cui è stata rilevata l'eccezione, in particolare [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), l'eccezione generata e l'origine dell'eccezione (dettagli di seguito).

### <a name="design-principles"></a>Principi di progettazione

1. **Nessuna modifica di rilievo** Poiché questa funzionalità viene aggiunta in una versione secondaria, un vincolo importante che influisce sulla soluzione è che non vengono apportate modifiche di rilievo, ai contratti di tipo o al comportamento. Questo vincolo escludeva alcune operazioni di pulizia che avremmo voluto eseguire in termini di blocchi catch esistenti che trasformano le eccezioni in 500 risposte. Questa pulizia aggiuntiva è qualcosa che potremmo prendere in considerazione per una successiva versione principale. Se questo è importante per voi si prega di votare su di esso a [ASP.NET voce utente WEB API](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Mantenere la coerenza con i costrutti API Web** La pipeline di filtro dell'API Web è un ottimo modo per gestire i problemi trasversali con la flessibilità di applicare la logica in un ambito globale specifico dell'azione, specifico del controller o. I filtri, inclusi i filtri eccezioni, hanno sempre contesti di azione e controller, anche se registrati nell'ambito globale. Tale contratto ha senso per i filtri, ma significa che i filtri di eccezione, anche quelli con ambito globale, non sono adatti per alcuni casi di gestione delle eccezioni, ad esempio le eccezioni dai gestori di messaggi, in cui non esiste alcun contesto di azione o controller. Se vogliamo usare l'ambito flessibile offerto dai filtri per la gestione delle eccezioni, abbiamo ancora bisogno di filtri eccezioni. Ma se abbiamo bisogno di gestire le eccezioni all'esterno di un contesto del controller, abbiamo anche bisogno di un costrutto separato per la gestione completa degli errori globali (qualcosa senza i vincoli di contesto del controller e del contesto dell'azione).

### <a name="when-to-use"></a>Utilizzo

- I logger di eccezione sono la soluzione per visualizzare tutte le eccezioni non gestite rilevate dall'API Web.
- I gestori di eccezioni sono la soluzione per personalizzare tutte le possibili risposte alle eccezioni non gestite rilevate dall'API Web.
- I filtri eccezioni sono la soluzione più semplice per l'elaborazione del sottoinsieme di eccezioni non gestite correlate a un'azione o a un controller specifico.

### <a name="service-details"></a>Dettagli del servizio

 Il logger di eccezione e le interfacce del servizio del gestore delle eccezioni sono semplici metodi asincroni che utilizzano i rispettivi contesti:The exception logger and handler service interfaces are simple async methods taking the respective contexts: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Forniamo anche le classi di base per entrambe queste interfacce. L'override dei metodi principali (sync o async) è tutto ciò che è necessario per registrare o gestire gli orari consigliati. Per la `ExceptionLogger` registrazione, la classe di base garantirà che il metodo di registrazione di base viene chiamato solo una volta per ogni eccezione (anche se in seguito si propaga più in alto nello stack di chiamate e viene intercettato nuovamente). La `ExceptionHandler` classe base chiamerà il metodo di gestione principale solo per le eccezioni all'inizio dello stack di chiamate, ignorando i blocchi catch annidati legacy. (Le versioni semplificate di queste classi di base sono nell'appendice sottostante.) Entrambi `IExceptionLogger` `IExceptionHandler` e ricevere informazioni sull'eccezione tramite un `ExceptionContext`file .

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Quando il framework chiama un logger di eccezione `Exception` o `Request`un gestore di eccezioni, fornirà sempre un e un oggetto . Fatta eccezione per gli unit `RequestContext`test, fornirà sempre un . Raramente fornirà `ControllerContext` `ActionContext` a (solo quando si chiama dal blocco catch per i filtri eccezioni). Esso fornirà molto `Response`raramente un (solo in alcuni casi di IIS quando nel bel mezzo del tentativo di scrivere la risposta). Si noti che, poiché alcune di queste proprietà possono essere `null` spetta al consumer controllare prima `null` di accedere ai membri della classe di eccezione.`CatchBlock` è una stringa che indica quale blocco catch ha visto l'eccezione. Le stringhe di blocco catch sono le seguenti:The catch block strings are as follows:

- HttpServer (metodo SendAsync)
- HttpControllerDispatcher (metodo SendAsync)
- HttpBatchHandler (metodo SendAsync)
- IExceptionFilter (elaborazione di ApiController della pipeline del filtro eccezioni in ExecuteAsync)
- Host OWIN:

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (per l'output di buffer)
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (per l'output di flusso)
- Host Web:

    - HttpControllerHandler.WriteBufferedResponseContentAsync (per l'output di buffer)
    - HttpControllerHandler.WriteStreamedResponseContentAsync (per l'output di flusso)HttpControllerHandler.WriteStreamedResponseContentAsync (for streaming output)
    - HttpControllerHandler.WriteErrorResponseContentAsync (per errori nel recupero degli errori in modalità di output memorizzato nel buffer)

L'elenco delle stringhe di blocco catch è disponibile anche tramite proprietà readonly statiche. (La stringa di blocco catch di base si trova sul static ExceptionCatchBlocks; il resto viene visualizzato in una classe statica ogni per OWIN e host web).`IsTopLevelCatchBlock` è utile per seguire il modello consigliato di gestione delle eccezioni solo all'inizio dello stack di chiamate. Anziché trasformare le eccezioni in 500 risposte ovunque si verifichi un blocco catch annidato, un gestore di eccezioni può consentire la propagazione delle eccezioni fino a quando non stanno per essere visualizzate dall'host.

Oltre al `ExceptionContext`, un logger ottiene un'altra `ExceptionLoggerContext`informazione tramite il pieno :

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

La seconda `CanBeHandled`proprietà, , consente a un logger di identificare un'eccezione che non può essere gestita. Quando la connessione sta per essere interrotta e non è possibile inviare alcun nuovo messaggio di risposta, verranno chiamati i logger ma il gestore ***non*** verrà chiamato e i logger possono identificare questo scenario da questa proprietà.

Oltre a `ExceptionContext`, un gestore ottiene un'altra `ExceptionHandlerContext` proprietà che può impostare sul full per gestire l'eccezione:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Un gestore di eccezioni indica che ha `Result` gestito un'eccezione impostando la proprietà su un risultato dell'azione, ad esempio [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx)o un risultato personalizzato. Se `Result` la proprietà è null, l'eccezione non è gestita e l'eccezione originale verrà generata nuovamente.

Per le eccezioni all'inizio dello stack di chiamate, è stato eseguito un passaggio aggiuntivo per garantire che la risposta sia appropriata per i chiamanti dell'API. Se l'eccezione si propaga fino all'host, il chiamante vedrà la schermata gialla di morte o qualche altra risposta fornita dall'host che è in genere HTML e non in genere una risposta di errore API appropriata. In questi casi, il Result inizia non null e solo se un `null` gestore di eccezioni personalizzato imposta in modo esplicito su (non gestito) l'eccezione si propagherà all'host. L'impostazione di in questi casi può essere utile per due scenari:Setting `Result` to `null` in such cases can be useful for two scenarios:

1. API Web ospitata da OWIN con middleware personalizzato di gestione delle eccezioni registrato prima/esterno all'API Web.
2. Debug locale tramite un browser, dove la schermata gialla di morte è in realtà una risposta utile per un'eccezione non gestita.

Sia per i logger di eccezioni che per i gestori di eccezioni, non viene eseguito alcuna operazione di ripristino se il logger o il gestore stesso genera un'eccezione. (Oltre a lasciare che l'eccezione si propaghi, lasciare un feedback nella parte inferiore di questa pagina se si dispone di un approccio migliore.) Il contratto per i logger e i gestori di eccezioni è che non devono consentire che le eccezioni si propaghino ai relativi chiamanti; in caso contrario, l'eccezione si propagherà, spesso fino all'host generando un errore HTML (ad esempio ASP. Schermata gialla di NET) viene inviata al client (che in genere non è l'opzione preferita per i chiamanti API che si aspettano JSON o XML).

## <a name="examples"></a>Esempi

### <a name="tracing-exception-logger"></a>Logger di eccezioni di tracciaTracing Exception Logger

Il logger di eccezione riportato di seguito invia i dati delle eccezioni alle origini di traccia configurate (inclusa la finestra di output di Debug in Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Gestore eccezioni messaggio di errore personalizzatoCustom Error Message Exception Handler

Il gestore di eccezioni seguente produce una risposta di errore personalizzata ai client, incluso un indirizzo di posta elettronica per contattare il supporto.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Registrazione dei filtri eccezioni

Se si utilizza il modello di progetto "ASP.NET applicazione Web MVC 4" `WebApiConfig` per creare il progetto, inserire il codice di configurazione dell'API Web all'interno della classe, nella cartella *App_Start:*

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Appendice: Dettagli della classe baseAppendix: Base Class Details

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
