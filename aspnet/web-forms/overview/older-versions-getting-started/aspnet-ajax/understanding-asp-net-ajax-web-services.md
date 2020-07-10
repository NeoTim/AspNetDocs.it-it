---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: Informazioni sui servizi Web di ASP.NET AJAX | Microsoft Docs
author: scottcate
description: I servizi Web sono parte integrante di .NET Framework che fornisce una soluzione multipiattaforma per lo scambio di dati tra sistemi distribuiti. Sebbene Web...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: eac3d53fd871d0cb5a2870488ce752c057cc5b1a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "86162822"
---
# <a name="understanding-aspnet-ajax-web-services"></a>Informazioni sui servizi Web di ASP.NET AJAX

di [Scott Cate](https://github.com/scottcate)

[Scarica il PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> I servizi Web sono parte integrante di .NET Framework che fornisce una soluzione multipiattaforma per lo scambio di dati tra sistemi distribuiti. Sebbene i servizi Web vengono in genere utilizzati per consentire diversi sistemi operativi, modelli a oggetti e linguaggi di programmazione per inviare e ricevere dati, possono essere utilizzati anche per inserire in modo dinamico i dati in una pagina ASP.NET AJAX o inviare dati da una pagina a un sistema back-end. Questa operazione può essere eseguita senza ricorrere a operazioni di postback.

## <a name="calling-web-services-with-aspnet-ajax"></a>Chiamata di servizi Web con ASP.NET AJAX

Dan Wahlin

I servizi Web sono parte integrante di .NET Framework che fornisce una soluzione multipiattaforma per lo scambio di dati tra sistemi distribuiti. Sebbene i servizi Web vengono in genere utilizzati per consentire diversi sistemi operativi, modelli a oggetti e linguaggi di programmazione per inviare e ricevere dati, possono essere utilizzati anche per inserire in modo dinamico i dati in una pagina ASP.NET AJAX o inviare dati da una pagina a un sistema back-end. Questa operazione può essere eseguita senza ricorrere a operazioni di postback.

Sebbene il controllo UpdatePanel ASP.NET AJAX fornisca un modo semplice per consentire a AJAX qualsiasi pagina di ASP.NET, in alcuni casi è possibile che sia necessario accedere in modo dinamico ai dati sul server senza utilizzare un UpdatePanel. Questo articolo illustra come eseguire questa operazione tramite la creazione e l'utilizzo di servizi Web all'interno di pagine ASP.NET AJAX.

Questo articolo è incentrato sulle funzionalità disponibili nelle estensioni di base di ASP.NET AJAX, oltre che su un controllo abilitato per il servizio Web in ASP.NET AJAX Toolkit denominato AutoCompleteExtender. Gli argomenti trattati includono la definizione di servizi Web abilitati per AJAX, la creazione di proxy client e la chiamata di servizi Web con JavaScript. Si vedrà anche come le chiamate del servizio Web possono essere effettuate direttamente ai metodi della pagina ASP.NET.

## <a name="web-services-configuration"></a>Configurazione dei servizi Web

Quando viene creato un nuovo progetto di sito Web con Visual Studio 2008, il file di web.config include una serie di nuove aggiunte che potrebbero non essere note agli utenti delle versioni precedenti di Visual Studio. Alcune di queste modifiche eseguono il mapping del prefisso "ASP" ai controlli AJAX ASP.NET in modo che possano essere utilizzate nelle pagine mentre altre definiscono gli elementi HttpHandlers e HttpModules richiesti. Il listato 1 Mostra le modifiche apportate all' `<httpHandlers>` elemento in web.config che influiscono sulle chiamate al servizio Web. Il valore predefinito di HttpHandler utilizzato per elaborare le chiamate con estensione asmx viene rimosso e sostituito con una classe ScriptHandlerFactory situata nell'assembly System.Web.Extensions.dll. System.Web.Extensions.dll contiene tutte le funzionalità di base usate da ASP.NET AJAX.

**Listato 1. Configurazione del gestore del servizio Web AJAX ASP.NET**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

Questa sostituzione HttpHandler viene eseguita per consentire le chiamate JavaScript Object Notation (JSON) da pagine ASP.NET AJAX ai servizi Web .NET usando un proxy del servizio Web JavaScript. ASP.NET AJAX invia messaggi JSON ai servizi Web in contrapposizione alle chiamate di Simple Object Access Protocol Standard (SOAP) generalmente associate ai servizi Web. In questo modo si ottiene un messaggio di richiesta e risposta più piccolo complessivo. Consente inoltre di elaborare i dati in modo più efficiente dal lato client poiché la libreria JavaScript AJAX ASP.NET è ottimizzata per l'uso con oggetti JSON. L'elenco 2 e l'elenco 3 mostrano esempi di messaggi di richiesta e risposta del servizio Web serializzati in formato JSON. Il messaggio di richiesta visualizzato nel listato 2 passa un parametro Country con un valore "Belgium", mentre il messaggio di risposta nel listato 3 passa una matrice di oggetti Customer e le relative proprietà associate.

**Listato 2. Messaggio di richiesta del servizio Web serializzato in JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *>[!NOTE]il nome dell'operazione viene definito come parte dell'URL del servizio Web. i messaggi di richiesta, inoltre, non vengono sempre inviati tramite JSON. I servizi Web possono usare l'attributo ScriptMethod con il parametro UseHttpGet impostato su true, che fa sì che i parametri vengano passati tramite i parametri della stringa di query.*

**Listato 3. Messaggio di risposta del servizio Web serializzato in JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

Nella sezione successiva si vedrà come creare servizi Web in grado di gestire i messaggi di richiesta JSON e rispondere con tipi semplici e complessi.

## <a name="creating-ajax-enabled-web-services"></a>Creazione di servizi Web abilitati per AJAX

Il framework ASP.NET AJAX offre diversi modi per chiamare i servizi Web. È possibile usare il controllo AutoCompleteExtender (disponibile in ASP.NET AJAX Toolkit) o JavaScript. Tuttavia, prima di chiamare un servizio, è necessario abilitarlo per AJAX, in modo che possa essere chiamato dal codice di script client.

Se non si ha familiarità con i servizi Web di ASP.NET, è facile creare e abilitare i servizi AJAX. In .NET Framework è supportata la creazione di servizi Web di ASP.NET, poiché la versione iniziale in 2002 e le estensioni AJAX di ASP.NET forniscono funzionalità AJAX aggiuntive che si basano sul set predefinito di funzionalità di .NET Framework. Visual Studio .NET 2008 Beta 2 dispone del supporto incorporato per la creazione di file del servizio Web. asmx e deriva automaticamente il codice associato accanto alle classi della classe System. Web. Services. WebService. Quando si aggiungono metodi alla classe, è necessario applicare l'attributo WebMethod per poter essere chiamati dagli utenti del servizio Web.

Il listato 4 illustra un esempio di applicazione dell'attributo WebMethod a un metodo denominato GetCustomersByCountry ().

**Listato 4. Uso dell'attributo WebMethod in un servizio Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

Il metodo GetCustomersByCountry () accetta un parametro Country e restituisce una matrice di oggetti Customer. Il valore Country passato al metodo viene inoltrato a una classe di livello business che a sua volta chiama una classe del livello dati per recuperare i dati dal database, compilare le proprietà dell'oggetto Customer con i dati e restituire la matrice.

## <a name="using-the-scriptservice-attribute"></a>Utilizzo dell'attributo ScriptService

Mentre l'aggiunta dell'attributo WebMethod consente la chiamata al metodo GetCustomersByCountry () da parte dei client che inviano messaggi SOAP standard al servizio Web, non sono consentite le chiamate JSON dalle applicazioni ASP.NET AJAX predefinite. Per consentire le chiamate JSON, è necessario applicare l'attributo dell'estensione ASP.NET AJAX `ScriptService` alla classe del servizio Web. Questo consente a un servizio Web di inviare messaggi di risposta formattati usando JSON e consente allo script lato client di chiamare un servizio inviando messaggi JSON.

L'elenco 5 Mostra un esempio di applicazione dell'attributo ScriptService a una classe del servizio Web denominata CustomersService.

**Listato 5. Uso dell'attributo ScriptService per l'abilitazione di un servizio Web in AJAX**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

L'attributo ScriptService funge da indicatore che indica che può essere chiamato dal codice di script AJAX. Non gestisce effettivamente alcuna delle attività di serializzazione o deserializzazione JSON eseguite dietro le quinte. ScriptHandlerFactory (configurato in web.config) e altre classi correlate eseguono la maggior parte dell'elaborazione JSON.

## <a name="using-the-scriptmethod-attribute"></a>Utilizzo dell'attributo ScriptMethod

L'attributo ScriptService è l'unico attributo ASP.NET AJAX che deve essere definito in un servizio Web .NET per poter essere usato da pagine ASP.NET AJAX. Un altro attributo denominato ScriptMethod, tuttavia, può anche essere applicato direttamente ai metodi Web in un servizio. ScriptMethod definisce tre proprietà, tra cui `UseHttpGet` , `ResponseFormat` e `XmlSerializeString` . La modifica dei valori di queste proprietà può essere utile nei casi in cui il tipo di richiesta accettato da un metodo Web deve essere modificato per ottenere, quando un metodo Web deve restituire dati XML non elaborati sotto forma di `XmlDocument` un `XmlElement` oggetto o o quando i dati restituiti da un servizio devono sempre essere serializzati come XML anziché JSON.

È possibile utilizzare la proprietà UseHttpGet quando un metodo Web accetta richieste GET anziché richieste POST. Le richieste vengono inviate tramite un URL con parametri di input del metodo Web convertiti in parametri QueryString. Per impostazione predefinita, la proprietà UseHttpGet è impostata su false e deve essere impostata su solo `true` quando le operazioni sono note come sicure e quando i dati sensibili non vengono passati a un servizio Web. L'elenco 6 Mostra un esempio di utilizzo dell'attributo ScriptMethod con la proprietà UseHttpGet.

**Elenco 6. Utilizzo dell'attributo ScriptMethod con la proprietà UseHttpGet.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

Di seguito è riportato un esempio delle intestazioni inviate quando viene chiamato il metodo Web HttpGetEcho illustrato nel listato 6:

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

Oltre a consentire ai metodi Web di accettare richieste HTTP GET, l'attributo ScriptMethod può essere usato anche quando è necessario restituire le risposte XML da un servizio anziché da JSON. Ad esempio, un servizio Web può recuperare un feed RSS da un sito remoto e restituirlo come oggetto XmlDocument o XmlElement. L'elaborazione dei dati XML può quindi essere eseguita nel client.

L'elenco 7 Mostra un esempio di utilizzo della proprietà ResponseFormat per specificare che i dati XML devono essere restituiti da un metodo Web.

**Listato 7. Utilizzo dell'attributo ScriptMethod con la proprietà ResponseFormat.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

È inoltre possibile utilizzare la proprietà ResponseFormat insieme alla proprietà XmlSerializeString. Il valore predefinito della proprietà XmlSerializeString è false, che indica che tutti i tipi restituiti, ad eccezione delle stringhe restituite da un metodo Web, vengono serializzati come XML quando la `ResponseFormat` proprietà è impostata su `ResponseFormat.Xml` . Quando `XmlSerializeString` è impostato su `true` , tutti i tipi restituiti da un metodo Web vengono serializzati come XML, inclusi i tipi di stringa. Se la proprietà ResponseFormat ha un valore della `ResponseFormat.Json` proprietà XmlSerializeString, viene ignorato.

L'elenco 8 Mostra un esempio di utilizzo della proprietà XmlSerializeString per forzare la serializzazione delle stringhe come XML.

**Listato 8. Utilizzo dell'attributo ScriptMethod con la proprietà XmlSerializeString**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

Il valore restituito dalla chiamata al metodo Web GetXmlString illustrato nel listato 8 è riportato di seguito:

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

Sebbene il formato JSON predefinito riduca al minimo le dimensioni complessive dei messaggi di richiesta e risposta e venga utilizzato più facilmente dai client ASP.NET AJAX in modo incrociato, le proprietà ResponseFormat e XmlSerializeString possono essere utilizzate quando le applicazioni client quali Internet Explorer 5 o versioni successive prevedono la restituzione di dati XML da un metodo Web.

## <a name="working-with-complex-types"></a>Utilizzo di tipi complessi

Il listato 5 ha mostrato un esempio di restituzione di un tipo complesso denominato Customer da un servizio Web. La classe Customer definisce diversi tipi semplici interni come proprietà quali FirstName e LastName. I tipi complessi usati come parametro di input o tipo restituito in un metodo Web con supporto AJAX vengono serializzati automaticamente in JSON prima di essere inviati al lato client. Tuttavia, i tipi complessi annidati (quelli definiti internamente in un altro tipo) non vengono resi disponibili per il client come oggetti autonomi per impostazione predefinita.

Nei casi in cui è necessario utilizzare anche un tipo complesso annidato utilizzato da un servizio Web in una pagina client, è possibile aggiungere l'attributo ASP.NET AJAX GenerateScriptType al servizio Web. Ad esempio, la classe CustomerDetails mostrata nel listato 9 contiene le proprietà Address e Gender che ***rappresentano tipi complessi annidati.***

**Elenco 9. La classe CustomerDetails illustrata di seguito contiene due tipi complessi annidati.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

Gli oggetti Address e Gender definiti nella classe CustomerDetails illustrata nel listato 9 non verranno automaticamente resi disponibili per l'uso sul lato client tramite JavaScript perché sono tipi annidati (Address è una classe e Gender è un'enumerazione). Nelle situazioni in cui è necessario che un tipo annidato utilizzato all'interno di un servizio Web sia disponibile sul lato client, è possibile usare l'attributo GenerateScriptType indicato in precedenza (vedere l'elenco 10). Questo attributo può essere aggiunto più volte nei casi in cui vengono restituiti tipi complessi nidificati diversi da un servizio. Può essere applicato direttamente alla classe del servizio Web o a metodi Web specifici.

**Listato 10. Utilizzo dell'attributo GenerateScriptService per definire i tipi annidati che devono essere disponibili per il client.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

Applicando l' `GenerateScriptType` attributo al servizio Web, i tipi di indirizzo e sesso saranno resi disponibili automaticamente per l'uso da parte del codice JavaScript AJAX ASP.NET sul lato client. Un esempio di JavaScript generato e inviato automaticamente al client mediante l'aggiunta dell'attributo GenerateScriptType in un servizio Web viene visualizzato nel listato 11. Più avanti nell'articolo verrà illustrato come utilizzare i tipi complessi annidati.

**Listato 11. Tipi complessi annidati resi disponibili per una pagina ASP.NET AJAX.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Ora che si è appreso come creare servizi Web e renderli accessibili alle pagine di ASP.NET AJAX, è possibile esaminare come creare e usare proxy JavaScript in modo che i dati possano essere recuperati o inviati ai servizi Web.

## <a name="creating-javascript-proxies"></a>Creazione di proxy JavaScript

La chiamata a un servizio Web standard (.NET o un'altra piattaforma) implica in genere la creazione di un oggetto proxy che protegge dall'utente le complessità dell'invio di messaggi di richiesta e risposta SOAP. Con le chiamate al servizio Web AJAX ASP.NET, i proxy JavaScript possono essere creati e usati per chiamare facilmente i servizi senza doversi preoccupare della serializzazione e deserializzazione dei messaggi JSON. I proxy JavaScript possono essere generati automaticamente tramite il controllo ASP.NET AJAX ScriptManager.

La creazione di un proxy JavaScript che può chiamare servizi Web viene eseguita utilizzando la proprietà dei servizi di ScriptManager. Questa proprietà consente di definire uno o più servizi che una pagina ASP.NET AJAX può chiamare in modo asincrono per inviare o ricevere dati senza richiedere operazioni di postback. Si definisce un servizio usando il controllo AJAX ASP.NET `ServiceReference` e assegnando l'URL del servizio Web alla proprietà del controllo `Path` . L'elenco 12 Mostra un esempio di riferimento a un servizio denominato CustomersService. asmx.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**Listato 12. Definizione di un servizio Web utilizzato in una pagina ASP.NET AJAX.**

L'aggiunta di un riferimento a CustomersService. asmx tramite il controllo ScriptManager causa la generazione dinamica di un proxy JavaScript che fa riferimento alla pagina. Il proxy viene incorporato usando il &lt; tag script &gt; e caricato dinamicamente chiamando il file CustomersService. asmx e aggiungendo/JS alla fine. Nell'esempio seguente viene illustrato il modo in cui il proxy JavaScript viene incorporato nella pagina quando il debug è disabilitato in web.config:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *>[!NOTE]Se si vuole visualizzare il codice proxy JavaScript effettivo che viene generato, è possibile digitare l'URL del servizio Web .NET desiderato nella casella degli indirizzi di Internet Explorer e aggiungere/JS alla fine.*

Se il debug è abilitato in web.config una versione di debug del proxy JavaScript verrà incorporata nella pagina, come illustrato di seguito:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

Il proxy JavaScript creato da ScriptManager può anche essere incorporato direttamente nella pagina anziché fare riferimento usando l' &lt; &gt; attributo src del tag script. Questa operazione può essere eseguita impostando la proprietà InlineScript del controllo ServiceReference su true (il valore predefinito è false). Questo può essere utile quando un proxy non è condiviso tra più pagine e quando si vuole ridurre il numero di chiamate di rete effettuate al server. Quando InlineScript è impostato su true, lo script del proxy non verrà memorizzato nella cache dal browser, quindi il valore predefinito di false è consigliato nei casi in cui il proxy viene usato da più pagine in un'applicazione ASP.NET AJAX. Di seguito è riportato un esempio di utilizzo della proprietà InlineScript:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>Uso di proxy JavaScript

Quando si fa riferimento a un servizio Web da una pagina ASP.NET AJAX usando il controllo ScriptManager, è possibile effettuare una chiamata al servizio Web e i dati restituiti possono essere gestiti tramite funzioni di callback. Un servizio Web viene chiamato facendo riferimento al relativo spazio dei nomi (se esistente), al nome della classe e al nome del metodo Web. Tutti i parametri passati al servizio Web possono essere definiti insieme a una funzione di callback che gestisce i dati restituiti.

Un esempio di utilizzo di un proxy JavaScript per chiamare un metodo Web denominato GetCustomersByCountry () viene visualizzato nel listato 13. La funzione GetCustomersByCountry () viene chiamata quando un utente finale fa clic su un pulsante nella pagina.

**Listato 13. Chiamata di un servizio Web con un proxy JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

Questa chiamata fa riferimento allo spazio dei nomi InterfaceTraining, alla classe CustomersService e al metodo Web GetCustomersByCountry definito nel servizio. Passa un valore Country ottenuto da una casella di testo e una funzione di callback denominata OnWSRequestComplete che deve essere richiamata quando la chiamata al servizio Web asincrono restituisce. OnWSRequestComplete gestisce la matrice di oggetti Customer restituiti dal servizio e li converte in una tabella visualizzata nella pagina. L'output generato dalla chiamata è illustrato nella figura 1.

[![Dati di binding ottenuti eseguendo una chiamata AJAX asincrona a un servizio Web.](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**Figura 1**: associazione dei dati ottenuti tramite una chiamata AJAX asincrona a un servizio Web.  ([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-web-services/_static/image3.png))

I proxy JavaScript possono anche effettuare chiamate unidirezionali ai servizi Web nei casi in cui è necessario chiamare un metodo Web, ma il proxy non deve attendere una risposta. Ad esempio, è possibile chiamare un servizio Web per avviare un processo, ad esempio un flusso di lavoro, ma non attendere un valore restituito dal servizio. Nei casi in cui deve essere effettuata una chiamata unidirezionale a un servizio, la funzione di callback mostrata nel listato 13 può semplicemente essere omessa. Poiché non viene definita alcuna funzione di callback, l'oggetto proxy non attende che il servizio Web restituisca dati.

## <a name="handling-errors"></a>Gestione degli errori

I callback asincroni ai servizi Web possono rilevare tipi diversi di errori, ad esempio la rete inattiva, il servizio Web non disponibile o un'eccezione restituita. Fortunatamente, gli oggetti proxy JavaScript generati da ScriptManager consentono di definire più callback per gestire errori ed errori, oltre al callback di esito positivo mostrato in precedenza. Una funzione di callback di errore può essere definita immediatamente dopo la funzione di callback standard nella chiamata al metodo Web, come illustrato nel listato 14.

**Listato 14. Definizione di una funzione di callback di errore e visualizzazione degli errori.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Qualsiasi errore che si verifica quando viene chiamato il servizio Web attiverà la chiamata della funzione di callback OnWSRequestFailed () che accetta un oggetto che rappresenta l'errore come parametro. L'oggetto Error espone diverse funzioni per determinare la cause dell'errore, nonché l'eventuale timeout della chiamata. L'elenco 14 Mostra un esempio di utilizzo delle diverse funzioni di errore e la figura 2 illustra un esempio dell'output generato dalle funzioni.

[![Output generato chiamando le funzioni di errore ASP.NET AJAX.](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**Figura 2**: output generato chiamando le funzioni di errore ASP.NET AJAX.  ([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-web-services/_static/image6.png))

## <a name="handling-xml-data-returned-from-a-web-service"></a>Gestione dei dati XML restituiti da un servizio Web

In precedenza è stato illustrato come un metodo Web può restituire dati XML non elaborati usando l'attributo ScriptMethod insieme alla relativa proprietà ResponseFormat. Quando ResponseFormat è impostato su ResponseFormat.Xml, i dati restituiti dal servizio Web vengono serializzati come XML anziché come JSON. Questa operazione può essere utile quando i dati XML devono essere passati direttamente al client per l'elaborazione utilizzando JavaScript o XSLT. Attualmente, Internet Explorer 5 o versione successiva fornisce il migliore modello a oggetti lato client per l'analisi e il filtraggio dei dati XML a causa del supporto incorporato di MSXML.

Il recupero di dati XML da un servizio Web non è diverso dal recupero di altri tipi di dati. Per iniziare, richiamare il proxy JavaScript per chiamare la funzione appropriata e definire una funzione di callback. Una volta restituita la chiamata, è possibile elaborare i dati nella funzione di callback.

L'elenco 15 Mostra un esempio di chiamata a un metodo Web denominato GetRssFeed () che restituisce un oggetto XmlElement. GetRssFeed () accetta un solo parametro che rappresenta l'URL per il feed RSS da recuperare.

**Listato 15. Utilizzo di dati XML restituiti da un servizio Web.**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

Questo esempio passa un URL a un feed RSS ed elabora i dati XML restituiti nella funzione OnWSRequestComplete (). OnWSRequestComplete () verifica innanzitutto se il browser è Internet Explorer per sapere se il parser MSXML è disponibile o meno. In tal caso, viene usata un'istruzione XPath per individuare tutti &lt; i &gt; tag degli elementi all'interno del feed RSS. Ogni elemento viene quindi iterato e i &lt; &gt; tag title e &lt; link associati &gt; vengono individuati ed elaborati per visualizzare i dati di ogni elemento. Nella figura 3 viene illustrato un esempio di output generato dall'esecuzione di una chiamata ASP.NET AJAX tramite un proxy JavaScript al metodo Web GetRssFeed ().

## <a name="handling-complex-types"></a>Gestione di tipi complessi

I tipi complessi accettati o restituiti da un servizio Web vengono esposti automaticamente tramite un proxy JavaScript. Tuttavia, i tipi complessi annidati non sono direttamente accessibili sul lato client, a meno che l'attributo GenerateScriptType non venga applicato al servizio come descritto in precedenza. Perché si vuole usare un tipo complesso annidato sul lato client?

Per rispondere a questa domanda, si supponga che una pagina ASP.NET AJAX visualizzi i dati dei clienti e consenta agli utenti finali di aggiornare l'indirizzo di un cliente. Se il servizio Web specifica che il tipo di indirizzo (un tipo complesso definito all'interno di una classe CustomerDetails) può essere inviato al client, il processo di aggiornamento può essere suddiviso in funzioni separate per un migliore riutilizzo del codice.

[![Output creato dalla chiamata a un servizio Web che restituisce dati RSS.](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**Figura 3**: output creato dalla chiamata a un servizio Web che restituisce dati RSS.  ([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-web-services/_static/image9.png))

Il listato 16 Mostra un esempio di codice sul lato client che richiama un oggetto Address definito in uno spazio dei nomi del modello, lo compila con dati aggiornati e lo assegna alla proprietà Address di un oggetto CustomerDetails. L'oggetto CustomerDetails viene quindi passato al servizio Web per l'elaborazione.

**Listato 16. Uso di tipi complessi annidati**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>Creazione e utilizzo di metodi di pagina

I servizi Web offrono un modo eccellente per esporre i servizi riutilizzabili a una vasta gamma di client, incluse le pagine ASP.NET AJAX. In alcuni casi, tuttavia, una pagina deve recuperare i dati che non verranno mai usati o condivisi da altre pagine. In questo caso, la creazione di un file con estensione asmx per consentire alla pagina di accedere ai dati può sembrare esagerata perché il servizio viene usato solo da una singola pagina.

ASP.NET AJAX fornisce un altro meccanismo per effettuare chiamate simili ai servizi Web senza creare file con estensione asmx autonomi. Questa operazione viene eseguita usando una tecnica denominata "metodi di pagina". I metodi di pagina sono metodi statici (Shared in VB.NET) incorporati direttamente in una pagina o in un file code-beside a cui è applicato l'attributo WebMethod. L'applicazione dell'attributo WebMethod può essere chiamata usando un oggetto JavaScript speciale denominato PageMethods che viene creato dinamicamente in fase di esecuzione. L'oggetto PageMethods funge da proxy che protegge dall'utente dal processo di serializzazione/deserializzazione JSON. Si noti che, per utilizzare l'oggetto PageMethods, è necessario impostare la proprietà EnablePageMethods di ScriptManager su true.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

L'elenco 17 Mostra un esempio di definizione di due metodi di pagina in una classe code-beside ASP.NET. Questi metodi recuperano i dati da una classe di livello business presente nella \_ cartella del codice dell'app del sito Web.

**Listato 17. Definizione di metodi di pagina.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

Quando ScriptManager rileva la presenza di metodi Web nella pagina, genera un riferimento dinamico all'oggetto PageMethods indicato in precedenza. La chiamata a un metodo Web viene eseguita facendo riferimento alla classe PageMethods seguita dal nome del metodo e da eventuali dati di parametro necessari che devono essere passati. Il listato 18 Mostra esempi di chiamata dei due metodi di pagina illustrati in precedenza.

**Listato 18. Chiamata di metodi di pagina con l'oggetto JavaScript PageMethods.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

L'uso dell'oggetto PageMethods è molto simile all'uso di un oggetto proxy JavaScript. È necessario innanzitutto specificare tutti i dati dei parametri che devono essere passati al metodo di pagina e quindi definire la funzione di callback da chiamare quando viene restituita la chiamata asincrona. È anche possibile specificare un callback di errore (vedere l'elenco 14 per un esempio di gestione degli errori).

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>AutoCompleteExtender e ASP.NET AJAX Toolkit

ASP.NET AJAX Toolkit (disponibile in [http://ajax.asp.net](http://ajax.asp.net) ) offre diversi controlli che possono essere usati per accedere ai servizi Web. In particolare, il Toolkit contiene un utile controllo denominato `AutoCompleteExtender` che può essere usato per chiamare i servizi Web e visualizzare i dati nelle pagine senza scrivere alcun codice JavaScript.

Il controllo AutoCompleteExtender può essere usato per estendere le funzionalità esistenti di una casella di testo e consentire agli utenti di individuare più facilmente i dati che stanno cercando. Quando digitano in una casella di testo, il controllo può essere usato per eseguire query in un servizio Web e Visualizza i risultati sotto la casella di testo in modo dinamico. Nella figura 4 viene illustrato un esempio di utilizzo del controllo AutoCompleteExtender per visualizzare gli ID cliente per un'applicazione di supporto. Quando l'utente digita caratteri diversi nella casella di testo, verranno visualizzati elementi diversi sotto in base al relativo input. Gli utenti possono quindi selezionare l'ID cliente desiderato.

L'uso di AutoCompleteExtender all'interno di una pagina ASP.NET AJAX richiede che l'assembly AjaxControlToolkit.dll venga aggiunto alla cartella bin del sito Web. Una volta aggiunto l'assembly del Toolkit, è necessario farvi riferimento in web.config in modo che i controlli in esso contenuti siano disponibili per tutte le pagine di un'applicazione. A tale scopo, è possibile aggiungere il tag seguente nel &lt; tag dei controlli web.config &gt; :

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

Nei casi in cui è sufficiente usare il controllo in una pagina specifica, è possibile farvi riferimento aggiungendo la direttiva Reference nella parte superiore di una pagina, come illustrato di seguito anziché aggiornare web.config:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]

[![Uso del controllo AutoCompleteExtender.](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**Figura 4**: uso del controllo AutoCompleteExtender.  ([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-web-services/_static/image12.png))

Una volta che il sito Web è stato configurato per l'uso di ASP.NET AJAX Toolkit, è possibile aggiungere un controllo AutoCompleteExtender nella pagina in modo analogo all'aggiunta di un normale controllo server ASP.NET. L'elenco 19 Mostra un esempio di utilizzo del controllo per chiamare un servizio Web.

**Listato 19. Uso del controllo AutoCompleteExtender di ASP.NET AJAX Toolkit.**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

AutoCompleteExtender dispone di diverse proprietà, tra cui le proprietà ID standard e runat trovate nei controlli server. Inoltre, consente di definire il numero di caratteri che un utente finale digita prima di eseguire una query sul servizio Web per i dati. La proprietà MinimumPrefixLength mostrata nel listato 19 fa in modo che il servizio venga chiamato ogni volta che un carattere viene digitato nella casella di testo. È necessario prestare attenzione a impostare questo valore poiché ogni volta che l'utente digita un carattere, il servizio Web verrà chiamato per cercare i valori corrispondenti ai caratteri della casella di testo. Il servizio Web da chiamare e il metodo Web di destinazione vengono definiti rispettivamente usando le proprietà ServicePath e ServiceMethod. Infine, la proprietà TargetControlID identifica la casella di testo a cui associare il controllo AutoCompleteExtender.

Il servizio Web chiamato deve avere l'attributo ScriptService applicato come descritto in precedenza e il metodo Web di destinazione deve accettare due parametri denominati prefixText e count. Il parametro prefixText rappresenta i caratteri digitati dall'utente finale e il parametro Count rappresenta il numero di elementi da restituire (il valore predefinito è 10). L'elenco 20 Mostra un esempio del metodo Web getcustomerids chiamato dal controllo AutoCompleteExtender illustrato in precedenza nel listato 19. Il metodo Web chiama un metodo del livello business che a sua volta chiama un metodo a livello di dati che gestisce il filtro dei dati e la restituzione dei risultati corrispondenti. Il codice per il metodo livello dati è illustrato nel listato 21.

**Listato 20. Filtraggio dei dati inviati dal controllo AutoCompleteExtender.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**Listato 21. Filtrare i risultati in base all'input dell'utente finale.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>Conclusioni

ASP.NET AJAX offre un supporto eccellente per chiamare i servizi Web senza scrivere molto codice JavaScript personalizzato per gestire i messaggi di richiesta e risposta. In questo articolo è stato illustrato come abilitare AJAX per l'elaborazione dei messaggi JSON e come definire proxy JavaScript tramite il controllo ScriptManager. È stato anche illustrato come i proxy JavaScript possono essere usati per chiamare i servizi Web, gestire tipi semplici e complessi e gestire gli errori. Infine, è stato illustrato come usare i metodi di pagina per semplificare il processo di creazione e di chiamata del servizio Web e il modo in cui il controllo AutoCompleteExtender può fornire supporto agli utenti finali durante la digitazione. Sebbene l'UpdatePanel disponibile in ASP.NET AJAX sarà sicuramente il controllo scelto per molti programmatori AJAX grazie alla sua semplicità, sapere come chiamare i servizi Web tramite proxy JavaScript può essere utile in molte applicazioni.

## <a name="bio"></a>Biografia

Dan Wahlin (Microsoft Most Valuable Professional per ASP.NET e i servizi Web XML) è un consulente per lo sviluppo di .NET e un consulente di architettura in formazione tecnica sull'interfaccia ( [http://www.interfacett.com](http://www.interfacett.com) ). Dan ha fondato il sito Web di XML for ASP.NET Developers ([www.XMLforASP.NET](http://www.XMLforASP.NET)), si trova nell'ufficio del relatore di INETA e parla di diverse conferenze. Dan co-creato Professional Windows DNA (Wrox), ASP.NET: suggerimenti, esercitazioni e codice (Sams), ASP.NET 1,1 Insider Solutions, Professional ASP.NET 2,0 AJAX (Wrox), ASP.NET 2,0 MVP hack e authored XML for ASP.NET Developers (Sams). Quando non scrive codice, articoli o libri, Dan gode di scrivere e registrare musica e giocare a golf e basket con la moglie e i figli.

Scott Cate collabora con le tecnologie Web Microsoft a partire dal 1997 ed è il Presidente di myKB.com ([www.myKB.com](http://www.myKB.com)), dove si specializza nella scrittura di applicazioni basate su ASP.NET incentrate sulle soluzioni software della Knowledge base. Scott può essere contattato tramite posta elettronica all'indirizzo [scott.cate@myKB.com](mailto:scott.cate@myKB.com) o al suo Blog all' [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Precedente](understanding-asp-net-ajax-localization.md) 
>  [Avanti](understanding-asp-net-ajax-debugging-capabilities.md)
