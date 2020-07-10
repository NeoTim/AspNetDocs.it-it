---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: Novità di ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Questo documento descrive le nuove funzionalità e i miglioramenti introdotti in ASP.NET MVC 2. Questo documento è disponibile anche per il download.
ms.author: riande
ms.date: 04/20/2010
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: 1a0a29241d8afecd295b11013b27621b21c9ed52
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "86162709"
---
# <a name="whats-new-in-aspnet-mvc-2"></a>Novità di ASP.NET MVC 2

> Questo documento descrive le nuove funzionalità e i miglioramenti introdotti in ASP.NET MVC 2. Questo documento è disponibile anche per il [download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)

[Introduzione](#_TOC1)   
[Aggiornamento di un progetto ASP.NET MVC 1,0 a ASP.NET MVC 2](#_TOC2)   
[Nuove funzionalità](#_TOC3)   
[Helper basati su modelli](#_TOC3_1)   
[Aree](#_TOC3_2)   
[Supporto per i controller asincroni](#_TOC3_3)   
[Supporto per DefaultValueAttribute nei parametri del metodo di azione](#_TOC3_4)   
[Supporto per l'associazione di dati binari con gli associazione di modelli](#_TOC3_5)   
[Classi ModelMetadata e ModelMetadataProvider](#_TOC3_6)   
[Supporto per gli attributi DataAnnotations](#_TOC3_7)   
[Provider di validator del modello](#_TOC3_8)   
[Convalida lato client](#_TOC3_9)   
[Nuovi frammenti di codice per Visual Studio 2010](#_TOC3_10)   
[Nuovo filtro azione RequireHttpsAttribute](#_TOC3_11)   
[Override del verbo del metodo HTTP](#_TOC3_12)   
[Nuova classe HiddenInputAttribute per helper basati su modelli](#_TOC3_13)   
[Il metodo helper HTML. ValidationSummary può visualizzare errori a livello di modello](#_TOC3_14)   
I [modelli T4 in Visual Studio generano codice specifico per la versione di destinazione dei](#_TOC3_15)[miglioramenti dell'API](#_TOC4) .NET Framework  
[Modifiche di rilievo](#_TOC5)  
[Dichiarazione di non responsabilità](#_TOC6)  

## <a name="introduction"></a><a id="_TOC1"></a>Introduzione

ASP.NET MVC 2 si basa su ASP.NET MVC 1,0 e introduce un ampio set di miglioramenti e funzionalità che si concentrano sull'aumento della produttività. Questa versione è compatibile con ASP.NET MVC 1,0, pertanto tutte le informazioni, le competenze, il codice e le estensioni per ASP.NET MVC 1,0 continueranno a essere applicate.

Per altre informazioni su ASP.NET MVC, vedere le risorse seguenti:

- [Documentazione di ASP.NET MVC su MSDN](https://go.microsoft.com/fwlink/?LinkId=159758)
- [Sito Web ASP.NET MVC](https://asp.net/mvc/)
- [Forum su MVC ASP.NET](https://forums.asp.net/1146.aspx)

## <a name="upgrading-an-aspnet-mvc-10-project-to-aspnet-mvc-2"></a><a id="_TOC2"></a>Aggiornamento di un progetto ASP.NET MVC 1,0 a ASP.NET MVC 2

ASP.NET MVC 2 può essere installato side-by-side con ASP.NET MVC 1,0 nello stesso server, che offre agli sviluppatori di applicazioni la flessibilità di scegliere quando aggiornare un'applicazione ASP.NET MVC 1,0 a ASP.NET MVC 2. Per informazioni su come eseguire l'aggiornamento, vedere il documento [aggiornamento di un'applicazione ASP.net mvc 1,0 a ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185459).

## <a name="new-features"></a><a id="_TOC3"></a>Nuove funzionalità

Questa sezione descrive le funzionalità introdotte nella versione MVC 2.

### <a name="templated-helpers"></a><a id="_TOC3_1"></a>Helper basati su modelli

Gli helper basati su modelli consentono di associare automaticamente gli elementi HTML per la modifica e la visualizzazione con i tipi di dati. Ad esempio, quando i dati di tipo System. DateTime vengono visualizzati in una vista, è possibile eseguire automaticamente il rendering di un elemento dell'interfaccia utente della selezione data. Questa operazione è simile alla modalità di funzionamento di modelli di campo in ASP.NET Dynamic Data. Per ulteriori informazioni, vedere [utilizzo degli helper basati su modelli per visualizzare i dati](https://go.microsoft.com/fwlink/?LinkId=159062) nel sito Web MSDN.

### <a name="areas"></a><a id="_TOC3_2"></a>Aree

Le aree consentono di organizzare un progetto di grandi dimensioni in più sezioni più piccole per gestire la complessità di un'applicazione Web di grandi dimensioni. Ogni sezione ("area") rappresenta in genere una sezione separata di un sito Web di grandi dimensioni e viene utilizzata per raggruppare set correlati di controller e visualizzazioni. Per ulteriori informazioni, vedere [procedura dettagliata: organizzazione di un'applicazione MVC ASP.NET per aree](https://go.microsoft.com/fwlink/?LinkId=158978) nel sito Web MSDN.

Per creare una nuova area, in Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto, scegliere Aggiungi, quindi fare clic su area. Verrà visualizzata una finestra di dialogo in cui viene richiesto il nome dell'area. Dopo aver immesso il nome dell'area, Visual Studio aggiunge una nuova area al progetto.

Nella figura seguente viene illustrato un layout di esempio per un progetto con due aree, amministrazione e Blog.

![](what-is-new-in-aspnet-mvc/_static/image1.png)

Quando si crea un'area, Visual Studio aggiunge una classe che deriva da AreaRegistration a ogni area. Questa classe è necessaria per registrare l'area e le relative route, come illustrato nell'esempio seguente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

Il modello di progetto predefinito per ASP.NET MVC 2 include una chiamata al metodo RegisterAllAreas nel codice del file Global. asax. Questo metodo registra ogni area del progetto cercando tutti i tipi che derivano dalla classe AreaRegistration, creando un'istanza del tipo e quindi chiamando il metodo RegisterArea sull'istanza. Nell'esempio seguente viene illustrato come eseguire questa operazione.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

Se non si specifica lo spazio dei nomi nel metodo RegisterArea chiamando il contesto. Namespaces. Add, lo spazio dei nomi della classe di registrazione viene usato per impostazione predefinita.

### <a name="support-for-asynchronous-controllers"></a><a id="_TOC3_3"></a>Supporto per i controller asincroni

ASP.NET MVC 2 consente ora ai controller di elaborare le richieste in modo asincrono. Questo può comportare un miglioramento delle prestazioni consentendo ai server che chiamano spesso le operazioni di blocco (ad esempio le richieste di rete) di chiamare invece le controparti non bloccanti. Per ulteriori informazioni, vedere l'argomento relativo all' [utilizzo di un controller asincrono in ASP.NET MVC](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx) su MSDN.

### <a name="support-for-defaultvalueattribute-in-action-method-parameters"></a><a id="_TOC3_4"></a>Supporto per DefaultValueAttribute nei parametri del metodo di azione

La classe System. ComponentModel. DefaultValueAttribute consente di fornire un valore predefinito per il parametro dell'argomento a un metodo di azione. Si supponga, ad esempio, che venga definita la route predefinita seguente:

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

Si supponga inoltre che il controller e il metodo di azione seguenti siano definiti:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

Uno degli URL di richiesta seguenti richiamerà il metodo di azione di visualizzazione definito nell'esempio precedente.

- /Article/View/123
- /Article/View/123? Page = 1 (in effetti corrisponde alla richiesta precedente)
- /Article/View/123? pagina = 2

Senza l'attributo DefaultValueAttribute, il primo URL dall'elenco precedente non funziona, perché l'argomento della pagina è un tipo di valore non nullable il cui valore non è stato fornito.

Se il codice è scritto in Visual Basic 2010 o Visual C# 2010, è possibile utilizzare parametri facoltativi anziché l'attributo DefaultValueAttribute, come illustrato nell'esempio seguente:

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a name="support-for-binding-binary-data-with-model-binders"></a><a id="_TOC3_5"></a>Supporto per l'associazione di dati binari con gli associazione di modelli

Sono disponibili due nuovi overload dell'helper HTML. Hidden che codificano i valori binari come stringhe con codifica base 64:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

Un utilizzo tipico consiste nell'incorporare un timestamp per un oggetto nella visualizzazione. Ad esempio, l'applicazione potrebbe includere il seguente oggetto prodotto:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

Un modulo di modifica può eseguire il rendering della proprietà TimeStamp nel form, come illustrato nell'esempio seguente:

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

Questo markup esegue il rendering di un elemento di input nascosto con il valore timestamp come stringa codificata in base 64 simile all'esempio seguente:

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

Questo modulo può essere inserito in un metodo di azione con un argomento di tipo Product, come illustrato nell'esempio seguente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

Nel metodo di azione, la proprietà TimeStamp viene popolata correttamente perché la stringa con codifica base 64 inviata viene convertita in una matrice di byte.

### <a name="modelmetadata-and-modelmetadataprovider-classes"></a><a id="_TOC3_6"></a>Classi ModelMetadata e ModelMetadataProvider

La classe ModelMetadataProvider fornisce un'astrazione per ottenere i metadati per il modello all'interno di una visualizzazione. MVC 2 include un provider predefinito che rende disponibili i metadati esposti dagli attributi nello spazio dei nomi System. ComponentModel. DataAnnotations. È possibile creare provider di metadati che forniscono metadati da altri archivi dati, ad esempio database o file XML.

La classe ViewDataDictionary espone un oggetto ModelMetadata che contiene i metadati estratti dal modello dalla classe ModelMetadataProvider. Ciò consente agli helper basati su modelli di utilizzare questi metadati e di regolare l'output di conseguenza.

Per ulteriori informazioni, vedere la documentazione per le classi [ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) e [ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) .

### <a name="support-for-dataannotations-attributes"></a><a id="_TOC3_7"></a>Supporto per gli attributi DataAnnotations

ASP.NET MVC 2 supporta l'uso degli attributi di convalida RangeAttribute, RequiredAttribute, StringLengthAttribute e RegexAttribute (definiti nello spazio dei nomi System. ComponentModel. DataAnnotations) quando si esegue l'associazione a un modello per fornire la convalida dell'input.

Per ulteriori informazioni, vedere [procedura: convalidare i dati del modello utilizzando gli attributi DataAnnotations](https://go.microsoft.com/fwlink/?LinkId=159063) sul sito Web MSDN. Un progetto di esempio che illustra l'uso di questi attributi è disponibile per il download all'indirizzo [https://go.microsoft.com/fwlink/?LinkId=157753](https://go.microsoft.com/fwlink/?LinkId=157753) .

### <a name="model-validator-providers"></a><a id="_TOC3_8"></a>Provider di validator del modello

La classe del provider di convalida del modello rappresenta un'astrazione che fornisce la logica di convalida per il modello. ASP.NET MVC include un provider predefinito basato sugli attributi di convalida inclusi nello spazio dei nomi System. ComponentModel. DataAnnotations. È inoltre possibile creare provider di convalida personalizzati che definiscono regole di convalida personalizzate e mapping personalizzati delle regole di convalida al modello. Per ulteriori informazioni, vedere la documentazione relativa alla classe [ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx) .

### <a name="client-side-validation"></a><a id="_TOC3_9"></a>Convalida lato client

La classe del provider del validator del modello espone i metadati di convalida al browser sotto forma di dati serializzati in JSON che possono essere utilizzati da una libreria di convalida lato client. ASP.NET MVC 2 include una libreria e una scheda di convalida client che supportano gli attributi di convalida dello spazio dei nomi DataAnnotations annotati in precedenza. La classe provider consente inoltre di utilizzare altre librerie di convalida client scrivendo un adapter che elabora i dati JSON e chiama nella libreria alternativa.

### <a name="new-code-snippets-for-visual-studio-2010"></a><a id="_TOC3_10"></a>Nuovi frammenti di codice per Visual Studio 2010

Un set di frammenti di codice HTML per ASP.NET MVC 2 viene installato con Visual Studio 2010. Per visualizzare un elenco di questi frammenti, scegliere Gestione frammenti di codice dal menu strumenti. Per la lingua, selezionare HTML e per percorso selezionare ASP.NET MVC 2. Per ulteriori informazioni sull'utilizzo dei frammenti di codice, vedere la documentazione di Visual Studio.

### <a name="new-requirehttpsattribute-action-filter"></a><a id="_TOC3_11"></a>Nuovo filtro azione RequireHttpsAttribute

ASP.NET MVC 2 include una nuova classe RequireHttpsAttribute che può essere applicata ai controller e ai metodi di azione. Per impostazione predefinita, il filtro reindirizza una richiesta non SSL (HTTP) all'equivalente abilitato per SSL (HTTPS).

### <a name="overriding-the-http-method-verb"></a><a id="_TOC3_12"></a>Override del verbo del metodo HTTP

Quando si compila un sito Web utilizzando lo stile di architettura REST, i verbi HTTP vengono utilizzati per determinare l'azione da eseguire per una risorsa. REST richiede che le applicazioni supportino l'intera gamma di verbi HTTP comuni, ad esempio GET, PUT, POST e DELETE.

ASP.NET MVC 2 include nuovi attributi che è possibile applicare ai metodi di azione e che presentano una sintassi compatta. Questi attributi consentono a ASP.NET MVC di selezionare un metodo di azione basato sul verbo HTTP. Nell'esempio seguente, una richiesta POST chiamerà il primo metodo di azione e una richiesta PUT chiamerà il secondo metodo di azione.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

Nelle versioni precedenti di ASP.NET MVC, questi metodi di azione richiedevano una sintassi più dettagliata, come illustrato nell'esempio seguente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

Poiché i browser supportano solo i verbi HTTP GET e POST, non è possibile pubblicare in un'azione che richiede un verbo diverso. Non è quindi possibile supportare in modo nativo tutte le richieste RESTful.

Tuttavia, per supportare le richieste RESTful durante le operazioni POST, ASP.NET MVC 2 introduce un nuovo metodo helper HTML di HttpMethodOverride. Questo metodo esegue il rendering di un elemento input nascosto che induce il form a emulare efficacemente qualsiasi metodo HTTP. Se ad esempio si usa il metodo helper HTML HttpMethodOverride, è possibile fare in modo che l'invio di un modulo appaia una richiesta PUT o DELETE. Il comportamento di HttpMethodOverride influiscono sui seguenti attributi:

- HttpPostAttribute
- HttpPutAttribute
- HttpGetAttribute
- HttpDeleteAttribute
- AcceptVerbsAttribute

Il nome dell'elemento di input nascosto è X-HTTP-method-override e il relativo valore è impostato sul verbo HTTP da emulare. Il valore di sostituzione può essere specificato anche in un'intestazione HTTP o in un valore della stringa di query come coppia nome/valore.

L'override può essere utilizzato solo quando la richiesta reale è una richiesta POST. Il valore di sostituzione verrà ignorato per le richieste che usano qualsiasi altro verbo HTTP.

### <a name="new-hiddeninputattribute-class-for-templated-helpers"></a><a id="_TOC3_13"></a>Nuova classe HiddenInputAttribute per helper basati su modelli

È possibile applicare il nuovo attributo HiddenInputAttribute a una proprietà del modello per indicare se è necessario eseguire il rendering di un elemento di input nascosto quando si Visualizza il modello in un modello di editor. (L'attributo imposta un valore UIHint implicito di HiddenInput). La proprietà DisplayValue dell'attributo consente di specificare se il valore viene visualizzato in modalità editor e visualizzazione. Quando DisplayValue è impostato su false, non viene visualizzato nulla, neanche il markup HTML che racchiude normalmente un campo. Il valore predefinito per DisplayValue è true.

È possibile utilizzare l'attributo HiddenInputAttribute negli scenari seguenti:

- Quando una visualizzazione consente agli utenti di modificare l'ID di un oggetto ed è necessario visualizzare il valore e fornire un elemento di input nascosto contenente l'ID precedente, in modo che possa essere passato di nuovo al controller.
- Quando una visualizzazione consente agli utenti di modificare una proprietà binaria che non dovrebbe mai essere visualizzata, ad esempio una proprietà timestamp. In tal caso, il valore e il markup HTML circostante, ad esempio l'etichetta e il valore, non vengono visualizzati.

Nell'esempio seguente viene illustrato come utilizzare la classe HiddenInputAttribute.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

Quando l'attributo è impostato su true (o non viene specificato alcun parametro), si verifica quanto segue:

- Nei modelli di visualizzazione viene eseguito il rendering di un'etichetta e il valore viene visualizzato all'utente.
- Nei modelli di editor viene eseguito il rendering di un'etichetta e viene eseguito il rendering del valore in un elemento input nascosto.

Quando l'attributo è impostato su false, si verifica quanto segue:

- Nei modelli di visualizzazione non viene eseguito il rendering di alcun elemento per quel campo.
- Nei modelli di editor non viene eseguito il rendering di alcuna etichetta e viene eseguito il rendering del valore in un elemento input nascosto.

### <a name="htmlvalidationsummary-helper-method-can-display-model-level-errors"></a><a id="_TOC3_14"></a>Il metodo helper HTML. ValidationSummary può visualizzare errori a livello di modello

Anziché visualizzare sempre tutti gli errori di convalida, il metodo helper HTML. ValidationSummary dispone di una nuova opzione per visualizzare solo gli errori a livello di modello. Questo consente di visualizzare gli errori a livello di modello nel Riepilogo di convalida e gli errori specifici del campo da visualizzare accanto a ogni campo.

### <a name="t4-templates-in-visual-studio-generate-code-that-is-specific-to-the-target-version-of-the-net-framework"></a><a id="_TOC3_15"></a>I modelli T4 in Visual Studio generano codice specifico per la versione di destinazione del .NET Framework

Una nuova proprietà è disponibile per i file T4 dall'host ASP.NET MVC T4 che specifica la versione del .NET Framework usata dall'applicazione. In questo modo, i modelli T4 consentono di generare codice e markup specifici per una versione del .NET Framework. In Visual Studio 2008 il valore è sempre .NET 3,5. In Visual Studio 2010 il valore è .NET 3,5 o .NET 4.

## <a name="api-improvements"></a><a id="_TOC4"></a>Miglioramenti delle API

In questa sezione vengono descritte le modifiche apportate ai tipi e ai membri MVC ASP.NET esistenti.

- È stato aggiunto un metodo CreateActionInvoker virtuale protetto nella classe controller. Questo metodo viene richiamato dalla proprietà ActionInvoker del controller e consente la creazione di un'istanza Lazy dell'invoker se non è già impostato alcun invoker.
- È stato aggiunto un metodo HandleUnauthorizedRequest virtuale protetto nella classe AuthorizeAttribute. Questo consente ai filtri che derivano da AuthorizeAttribute di controllare il comportamento in caso di errore dell'autorizzazione.
- Aggiunto un metodo Add (String key, Object value) nella classe ValueProviderDictionary. Questo consente di usare la sintassi dell'inizializzatore di dizionario per ValueProviderDictionary, come nell'esempio seguente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- Aggiunto un \_ metodo Get Object nella classe Sys. Mvc. AjaxContext. Si tratta di un metodo JavaScript simile al \_ metodo Get data, ma se il tipo di contenuto della risposta è Application/JSON, Get \_ Object restituisce l'oggetto JSON.
- È stata aggiunta una proprietà ActionDescriptor nella classe AuthorizationContext.
- Aggiunta di un token UrlParameter. optional che può essere utilizzato per risolvere i problemi durante l'associazione a un modello che contiene una proprietà ID quando la proprietà è assente in un post del form. Per informazioni dettagliate, vedere la voce relativa ai [parametri dell'URL facoltativo ASP.NET MVC 2](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) nel Blog di Phil Haack.

## <a name="breaking-changes"></a><a id="_TOC5"></a>Modifiche di rilievo

Le modifiche seguenti potrebbero causare errori nelle applicazioni ASP.NET MVC 1,0 esistenti.

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a>Modifica del comportamento di convalida delle proprietà per le classi che implementano IDataErrorInfo

Per gli oggetti modello che usano IDataErrorInfo per eseguire la convalida, ogni proprietà viene convalidata, indipendentemente dal fatto che sia stato impostato un nuovo valore. In ASP.NET MVC 1,0 sono state convalidate solo le proprietà con nuovi valori impostati. In ASP.NET MVC 2 la proprietà Error di IDataErrorInfo viene chiamata solo se tutti i validator delle proprietà sono stati completati correttamente.

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a>Lo script di mapping degli script IIS non è più disponibile nel programma di installazione

Lo script di mapping di script di IIS è uno script della riga di comando utilizzato per configurare le mappe di script per IIS 6 e per IIS 7 in modalità classica. Lo script di mapping degli script non è necessario se si usa il Server di sviluppo Visual Studio o se si usa IIS 7 in modalità integrata. Gli script sono disponibili come download separato non supportato in [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a>Il metodo helper HTML. Sostituisci nei futuri MVC non è più disponibile

A causa delle modifiche apportate al comportamento di rendering dei motori di visualizzazione MVC, il metodo helper HTML. Sostituisci non funziona ed è stato rimosso.

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a>L'interfaccia IValueProvider sostituisce tutti gli utilizzi di IDictionary

Ogni argomento di proprietà o metodo che accetta IDictionary in MVC 1,0 ora accetta IValueProvider. Questa modifica ha effetto solo sulle applicazioni che includono provider di valori personalizzati o associazioni di modelli personalizzate. Di seguito sono riportati alcuni esempi di proprietà e metodi interessati da questa modifica:

- Proprietà ValueProvider delle classi ControllerBase e ModelBindingContext.
- Metodi TryUpdateModel della classe controller.

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a>Sono state aggiunte nuove classi CSS nel file site. CSS

Il file site. CSS nei modelli di progetto MVC ASP.NET è stato aggiornato in modo da includere nuovi stili usati dalla funzionalità di convalida e dagli helper basati su modelli.

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a>Gli helper restituiscono ora un oggetto MvcHtmlString

Per sfruttare i vantaggi della nuova sintassi delle espressioni di codifica HTML in ASP.NET 4, il tipo restituito per gli helper HTML è ora MvcHtmlString anziché una stringa. Se si usa ASP.NET MVC 2 e i nuovi Helper in ASP.NET 3,5, non sarà possibile sfruttare la sintassi di codifica HTML; la nuova sintassi è disponibile solo quando si esegue ASP.NET MVC 2 in ASP.NET 4.

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a>JsonResult risponde ora solo alle richieste HTTP POST

Per mitigare gli attacchi di hijacker JSON che possono causare la divulgazione di informazioni, per impostazione predefinita, la classe JsonResult ora risponde solo alle richieste HTTP POST. Per usare POST è necessario modificare le chiamate GET AJAX ai metodi di azione che restituiscono un oggetto JsonResult. Se necessario, è possibile eseguire l'override di questo comportamento impostando la nuova proprietà JsonRequestBehavior di JsonResult. Per altre informazioni sui potenziali exploit, vedere il post di Blog relativo al hijack del codice [JSON](http://haacked.com/archive/2009/06/25/json-hijacking.aspx) nel Blog di Phil Haack.

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a>I setter di proprietà Model e tipo modelType in ModelBindingContext sono obsoleti

Alla classe ModelBindingContext è stata aggiunta una nuova proprietà ModelMetadata impostabile. La nuova proprietà incapsula sia il modello che le proprietà tipo modelType. Sebbene le proprietà Model e tipo modelType siano obsolete, per compatibilità con le versioni precedenti i getter della proprietà continuano a funzionare. delegano alla proprietà ModelMetadata per recuperare il valore.

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a>Le modifiche alla classe DefaultControllerFactory interrompono le factory del controller personalizzate che derivano da esso

La classe DefaultControllerFactory è stata corretta rimuovendo la proprietà RequestContext. Al posto di questa proprietà, l'istanza del contesto della richiesta viene passata ai metodi virtuali GetControllerInstance e GetControllerType protetti. Questa modifica influiscono sulle Factory controller personalizzate che derivano da DefaultControllerFactory.

Le factory di controller personalizzate vengono spesso usate per fornire l'inserimento delle dipendenze per le applicazioni MVC ASP.NET. Per aggiornare le factory del controller personalizzate per il supporto di ASP.NET MVC 2, modificare la firma o le firme del metodo in modo che corrispondano alle nuove firme e usare il parametro di contesto della richiesta anziché la proprietà.

#### <a name="area-is-a-now-a-reserved-route-value-key"></a>"Area" è ora una chiave del valore di route riservata

La stringa "area" nei valori di route ora ha un significato speciale in ASP.NET MVC, allo stesso modo di "controller" e "azione". Un'implicazione è che se gli helper HTML vengono forniti con un dizionario di valori di route contenente "area", gli helper non aggiungono più "area" nella stringa di query.

Se si usa la funzionalità aree, assicurarsi di non usare {area} come parte dell'URL della route.

## <a name="disclaimer"></a><a id="_TOC6"></a>Non responsabilità

Il presente documento è una versione preliminare e può essere modificato in modo sostanziale prima della versione finale commerciale del software qui descritto.

Le informazioni contenute in questo documento rappresentano la posizione attuale di Microsoft Corporation sulle problematiche trattate alla data di pubblicazione. Poiché Microsoft deve rispondere alle mutevoli condizioni del mercato, questo non deve essere interpretato come un impegno da parte di Microsoft, che non garantisce l'accuratezza delle informazioni presentate dopo la data di pubblicazione.

Il presente white paper è fornito solo a scopi informativi. MICROSOFT NON RILASCIA ALCUNA GARANZIA, ESPRESSA, IMPLICITA O LEGALE, IN MERITO ALLE INFORMAZIONI DEL PRESENTE DOCUMENTO.

Il rispetto di tutte le applicabili leggi in materia di copyright è esclusivamente a carico dell'utente. Fermi restando tutti i diritti coperti da copyright, nessuna parte di questa documentazione potrà comunque essere riprodotta o inserita in un sistema di riproduzione o trasmessa in qualsiasi forma e con qualsiasi mezzo (in formato elettronico, meccanico, fotocopia, tramite registrazione o altro) per qualsiasi scopo, senza il permesso scritto di Microsoft Corporation.

Microsoft può essere titolare di brevetti, domande di brevetto, marchi, copyright o altri diritti di proprietà intellettuale relativi all'oggetto della presente documentazione. Salvo quanto espressamente previsto in un contratto scritto di licenza Microsoft, la consegna della presente documentazione non implica la concessione di alcuna licenza su tali brevetti, marchi, copyright o altra proprietà intellettuale.

Se non specificato diversamente, le aziende, le organizzazioni, i prodotti, i nomi di dominio, gli indirizzi di posta elettronica, i logo, le persone, le località e gli eventi citati in questo documento sono fittizi e nessuna associazione con nessuna società, organizzazione, prodotto, nome di dominio, indirizzo di posta elettronica, logo, persona, luogo o evento è intenzionale o può essere presupposta.

© 2010 Microsoft Corporation. Tutti i diritti sono riservati.

Microsoft e Windows sono marchi registrati o marchi di Microsoft Corporation negli Stati Uniti e/o in altri paesi.

I nomi di società e prodotti reali citati nel presente documento possono essere marchi dei rispettivi proprietari.
