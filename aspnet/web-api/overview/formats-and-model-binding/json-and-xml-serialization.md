---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: Serializzazione JSON e XML nellASP.NETAPI Web - ASP.NET 4.x
author: MikeWasson
description: Descrive i formattatori JSON e XML in ASP.NETAPI Web per ASP.NET 4.x.
ms.author: riande
ms.date: 05/30/2012
ms.custom: seoapril2019
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: e6e02fa1c48e9c5fb8499379508619ddb317ccc9
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676223"
---
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a>Serializzazione JSON e XML nellASP.NETAPI Web

da parte di [Mike Wasson](https://github.com/MikeWasson)

Questo articolo descrive i formattatori JSON e XML in ASP.NETAPI Web.

NellASP.NETAPI Web, un *formattatore di tipo supporto* è un oggetto che consente di:

- Leggere oggetti CLR dal corpo di un messaggio HTTPRead CLR objects from an HTTP message body
- Scrivere oggetti CLR nel corpo di un messaggio HTTPWrite CLR objects into an HTTP message body

API Web fornisce formattatori di tipo mediaper sia per JSON che per XML. Il framework inserisce questi formattatori nella pipeline per impostazione predefinita. I client possono richiedere JSON o XML nell'intestazione Accept della richiesta HTTP.

## <a name="contents"></a>Sommario

- [Formattatore di tipo di supporto JSON](#json_media_type_formatter)

    - [Proprietà di sola letturaRead-Only Properties](#json_readonly)
    - [Dates](#json_dates)
    - [Stili rientri](#json_indenting)
    - [Involucro di cammello](#json_camelcasing)
    - [Oggetti anonimi e debolmente tipizzati](#json_anon)
- [Formattatore di tipo supporto XMLXML Media-Type Formatter](#xml_media_type_formatter)

    - [Proprietà di sola letturaRead-Only Properties](#xml_readonly)
    - [Dates](#xml_dates)
    - [Stili rientri](#xml_indenting)
    - [Impostazione di serializzatori XML per tipoSetting Per-Type XML Serializers](#xml_pertype)
- [Rimozione del formattatore JSON o XML](#removing_the_json_or_xml_formatter)
- [Gestione dei riferimenti a oggetti circolariHandling Circular Object References](#handling_circular_object_references)
- [Test della serializzazione degli oggettiTesting Object Serialization](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Formattatore di tipo di supporto JSON

La formattazione JSON viene fornita dalla classe **JsonMediaTypeFormatter.JSON** format is provided by the JsonMediaTypeFormatter class. Per impostazione predefinita, **JsonMediaTypeFormatter** utilizza la [libreria Json.NET](https://github.com/JamesNK/Newtonsoft.Json) per eseguire la serializzazione. Json.NET è un progetto open source di terze parti.

Se si preferisce, è possibile configurare la classe **JsonMediaTypeFormatter** per utilizzare **DataContractJsonSerializer** anziché Json.NET. A tale scopo, impostare la proprietà **UseDataContractJsonSerializer** su **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Serializzazione JSON

In questa sezione vengono descritti alcuni comportamenti specifici del formattatore JSON, usando il serializzatore [di Json.NET](https://github.com/JamesNK/Newtonsoft.Json) predefinito. Questo non è pensato per essere una documentazione completa della biblioteca Json.NET; Per ulteriori informazioni, vedere la [documentazione Json.NET](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Che cosa viene serializzato?

Per impostazione predefinita, tutte le proprietà e i campi pubblici sono inclusi nel codice JSON serializzato. Per omettere una proprietà o un campo, decorarlo con l'attributo **JsonIgnore.**

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Se si &quot;preferisce&quot; un approccio opt-in, decorare la classe con il **DataContract** attributo. Se questo attributo è presente, i membri vengono ignorati a meno che non dispongano di **DataMember**. È inoltre possibile utilizzare **DataMember** per serializzare i membri privati.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Proprietà di sola letturaRead-Only Properties

Le proprietà di sola lettura vengono serializzate per impostazione predefinita.

<a id="json_dates"></a>
### <a name="dates"></a>Date

Per impostazione predefinita, Json.NET scrive le date in formato [ISO 8601.](http://www.w3.org/TR/NOTE-datetime) Le date in formato UTC (Coordinated Universal Time) vengono scritte con un suffisso "". Le date nell'ora locale includono un offset del fuso orario. Ad esempio:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Per impostazione predefinita, Json.NET mantiene il fuso orario. È possibile eseguire l'override impostando la proprietà DateTime-oneHandling:You can override this by setting the DateTime'oneHandling property:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Se si preferisce utilizzare il`"\/Date(ticks)\/"`formato data Microsoft [JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) ( ) anziché ISO 8601, impostare la proprietà **DateFormatHandling** sulle impostazioni del serializzatore:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Stili rientri

Per scrivere un JSON rientrato, impostare l'impostazione **Formattazione** su **Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Involucro di cammello

Per scrivere i nomi delle proprietà JSON con maiuscole e minuscole camel, senza modificare il modello di dati, impostare **CamelCasePropertyNamesContractResolver** sul serializzatore:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Oggetti anonimi e debolmente tipizzati

Un metodo di azione può restituire un oggetto anonimo e serializzarlo in JSON. Ad esempio:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

The response message body will contain the following JSON:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Se l'API Web riceve oggetti JSON strutturati in modo non uniforme dai client, è possibile deserializzare il corpo della richiesta in un tipo **Newtonsoft.Json.Linq.JObject.**

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Tuttavia, è in genere preferibile utilizzare oggetti dati fortemente tipizzati. Quindi non è necessario analizzare i dati manualmente e si ottengono i vantaggi della convalida del modello.

Il serializzatore XML non supporta i tipi anonimi o istanze **JObject.** Se si usano queste funzionalità per i dati JSON, è necessario rimuovere il formattatore XML dalla pipeline, come descritto più avanti in questo articolo.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Formattatore di tipo supporto XMLXML Media-Type Formatter

La formattazione XML viene fornita dalla classe **XmlMediaTypeFormatter.** Per impostazione predefinita, **XmlMediaTypeFormatter** utilizza la classe **DataContractSerializer** per eseguire la serializzazione.

Se si preferisce, è possibile configurare **XmlMediaTypeFormatter** in modo che utilizzi **XmlSerializer** anziché **DataContractSerializer**. A tale scopo, impostare la proprietà **UseXmlSerializer** su **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

La classe **XmlSerializer** supporta un set di tipi più ristretto rispetto a **DataContractSerializer**, ma offre un maggiore controllo sul codice XML risultante. Considerare l'utilizzo di **XmlSerializer** se è necessario associare uno schema XML esistente.

### <a name="xml-serialization"></a>Serializzazione XML

In questa sezione vengono descritti alcuni comportamenti specifici del formattatore XML, utilizzando l'impostazione predefinita **DataContractSerializer**.

Per impostazione predefinita, DataContractSerializer si comporta come segue:By default, the DataContractSerializer behaves as follows:

- Tutte le proprietà e i campi pubblici di lettura/scrittura vengono serializzati. Per omettere una proprietà o un campo, decorarlo con l'attributo **IgnoreDataMember.**
- I membri privati e protetti non vengono serializzati.
- Le proprietà di sola lettura non vengono serializzate. (Tuttavia, il contenuto di una proprietà di raccolta di sola lettura vengono serializzati.)
- I nomi delle classi e dei membri vengono scritti nel codice XML esattamente come appaiono nella dichiarazione di classe.
- Viene utilizzato uno spazio dei nomi XML predefinito.

Se è necessario un maggiore controllo sulla serializzazione, è possibile decorare la classe con il **DataContract** attributo. Quando questo attributo è presente, la classe viene serializzata come segue:

- &quot;Approccio&quot; opt-in: le proprietà e i campi non vengono serializzati per impostazione predefinita. Per serializzare una proprietà o un campo, decorarlo con l'attributo **DataMember.**
- Per serializzare un membro privato o protetto, decorarlo con l'attributo **DataMember.**
- Le proprietà di sola lettura non vengono serializzate.
- Per modificare la modalità di visualizzazione del nome della classe nel codice XML, impostare il parametro *Name* nell'attributo **DataContract.**
- Per modificare la modalità di visualizzazione del nome di un membro nel codice XML, impostare il parametro *Name* nell'attributo **DataMember.**
- Per modificare lo spazio dei nomi XML, impostare il parametro *Namespace* nella classe **DataContract.**

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Proprietà di sola letturaRead-Only Properties

Le proprietà di sola lettura non vengono serializzate. Se una proprietà di sola lettura dispone di un campo privato di tipo backing, è possibile contrassegnare il campo privato con l'attributo **DataMember.** Questo approccio richiede l'attributo **DataContract** nella classe.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Date

Le date sono scritte in formato ISO 8601. Ad esempio, &quot;2012-05-23T20:21:37.9116538 .&quot;

<a id="xml_indenting"></a>
### <a name="indenting"></a>Stili rientri

Per scrivere codice XML rientrato, impostare la proprietà **Indent** su **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Impostazione di serializzatori XML per tipoSetting Per-Type XML Serializers

È possibile impostare serializzatori XML diversi per tipi CLR diversi. Ad esempio, si potrebbe avere un particolare oggetto dati che richiede **XmlSerializer** per la compatibilità con le versioni precedenti. È possibile utilizzare **XmlSerializer** per questo oggetto e continuare a utilizzare **DataContractSerializer** per altri tipi.

Per impostare un serializzatore XML per un determinato tipo, chiamare **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

È possibile specificare un **XmlSerializer** o qualsiasi oggetto che deriva da **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Rimozione del formattatore JSON o XML

È possibile rimuovere il formattatore JSON o il formattatore XML dall'elenco dei formattatori, se non si desidera utilizzarli. I motivi principali per fare questo sono:

- Per limitare le risposte dell'API Web a un particolare tipo di supporto. Ad esempio, è possibile decidere di supportare solo le risposte JSON e rimuovere il formattatore XML.
- Per sostituire il formattatore predefinito con un formattatore personalizzato. Ad esempio, è possibile sostituire il formattatore JSON con l'implementazione personalizzata di un formattatore JSON.

Nel codice seguente viene illustrato come rimuovere i formattatori predefiniti. Chiamare questo dal metodo **di avvio\_dell'applicazione,** definito in Global.asax.Call this from your Application Start method, defined in Global.asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Gestione dei riferimenti a oggetti circolariHandling Circular Object References

Per impostazione predefinita, i formattatori JSON e XML scrivono tutti gli oggetti come valori. Se due proprietà fanno riferimento allo stesso oggetto o se lo stesso oggetto viene visualizzato due volte in una raccolta, il formattatore serializzerà l'oggetto due volte. Si tratta di un problema particolare se l'oggetto grafico contiene cicli, perché il serializzatore genererà un'eccezione quando rileva un ciclo nel grafico.

Si considerino i modelli a oggetti e il controller seguenti.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Se si richiama questa azione, il formattatore genererà un'eccezione, che si traduce in una risposta di codice di stato 500 (Errore interno del server) al client.

Per mantenere i riferimenti agli oggetti in JSON, aggiungere il codice seguente al metodo **Application\_Start** nel file Global.asax:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Ora l'azione del controller restituirà JSON simile al seguente:Now the controller action will return JSON that looks like this:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Si noti che &quot;&quot; il serializzatore aggiunge una proprietà $id a entrambi gli oggetti. Inoltre, rileva che la proprietà Employee.Department crea un ciclo, in modo da&quot;&quot;sostituire&quot;&quot;il valore con un riferimento all'oggetto: $ref : 1 .

> [!NOTE]
> I riferimenti agli oggetti non sono standard in JSON. Prima di utilizzare questa funzionalità, valutare se i client saranno in grado di analizzare i risultati. Potrebbe essere meglio semplicemente per rimuovere cicli dal grafico. Ad esempio, il collegamento da Dipendente a Reparto non è realmente necessario in questo esempio.

Per mantenere i riferimenti agli oggetti in XML, sono disponibili due opzioni. L'opzione più `[DataContract(IsReference=true)]` semplice consiste nell'aggiungere alla classe del modello. Il parametro *IsReference* consente di abilitare i riferimenti agli oggetti. Tenere presente che DataContract accetta la serializzazione, pertanto sarà necessario aggiungere anche gli attributi DataMember alle proprietà:Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Ora il formattatore produrrà CODICE XML simile al seguente:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Se si desidera evitare gli attributi nella classe del modello, è disponibile un'altra opzione: creare una nuova istanza **di DataContractSerializer** specifica del tipo e impostare *preserveObjectReferences* su **true** nel costruttore. Impostare quindi questa istanza come serializzatore per tipo nel formattatore di tipo supporto XML. Il codice seguente mostra come eseguire questa operazione:The following code show how to do this:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Test della serializzazione degli oggettiTesting Object Serialization

Durante la progettazione dell'API Web, è utile testare la modalità di serializzazione degli oggetti dati. È possibile eseguire questa operazione senza creare un controller o richiamare un'azione del controller.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
