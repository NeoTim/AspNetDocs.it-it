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
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="9faad-103">Serializzazione JSON e XML nellASP.NETAPI Web</span><span class="sxs-lookup"><span data-stu-id="9faad-103">JSON and XML Serialization in ASP.NET Web API</span></span>

<span data-ttu-id="9faad-104">da parte di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9faad-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9faad-105">Questo articolo descrive i formattatori JSON e XML in ASP.NETAPI Web.</span><span class="sxs-lookup"><span data-stu-id="9faad-105">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="9faad-106">NellASP.NETAPI Web, un *formattatore di tipo supporto* è un oggetto che consente di:</span><span class="sxs-lookup"><span data-stu-id="9faad-106">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="9faad-107">Leggere oggetti CLR dal corpo di un messaggio HTTPRead CLR objects from an HTTP message body</span><span class="sxs-lookup"><span data-stu-id="9faad-107">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="9faad-108">Scrivere oggetti CLR nel corpo di un messaggio HTTPWrite CLR objects into an HTTP message body</span><span class="sxs-lookup"><span data-stu-id="9faad-108">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="9faad-109">API Web fornisce formattatori di tipo mediaper sia per JSON che per XML.</span><span class="sxs-lookup"><span data-stu-id="9faad-109">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="9faad-110">Il framework inserisce questi formattatori nella pipeline per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="9faad-110">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="9faad-111">I client possono richiedere JSON o XML nell'intestazione Accept della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9faad-111">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="9faad-112">Sommario</span><span class="sxs-lookup"><span data-stu-id="9faad-112">Contents</span></span>

- [<span data-ttu-id="9faad-113">Formattatore di tipo di supporto JSON</span><span class="sxs-lookup"><span data-stu-id="9faad-113">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="9faad-114">Proprietà di sola letturaRead-Only Properties</span><span class="sxs-lookup"><span data-stu-id="9faad-114">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="9faad-115">Dates</span><span class="sxs-lookup"><span data-stu-id="9faad-115">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="9faad-116">Stili rientri</span><span class="sxs-lookup"><span data-stu-id="9faad-116">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="9faad-117">Involucro di cammello</span><span class="sxs-lookup"><span data-stu-id="9faad-117">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="9faad-118">Oggetti anonimi e debolmente tipizzati</span><span class="sxs-lookup"><span data-stu-id="9faad-118">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="9faad-119">Formattatore di tipo supporto XMLXML Media-Type Formatter</span><span class="sxs-lookup"><span data-stu-id="9faad-119">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="9faad-120">Proprietà di sola letturaRead-Only Properties</span><span class="sxs-lookup"><span data-stu-id="9faad-120">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="9faad-121">Dates</span><span class="sxs-lookup"><span data-stu-id="9faad-121">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="9faad-122">Stili rientri</span><span class="sxs-lookup"><span data-stu-id="9faad-122">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="9faad-123">Impostazione di serializzatori XML per tipoSetting Per-Type XML Serializers</span><span class="sxs-lookup"><span data-stu-id="9faad-123">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="9faad-124">Rimozione del formattatore JSON o XML</span><span class="sxs-lookup"><span data-stu-id="9faad-124">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="9faad-125">Gestione dei riferimenti a oggetti circolariHandling Circular Object References</span><span class="sxs-lookup"><span data-stu-id="9faad-125">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="9faad-126">Test della serializzazione degli oggettiTesting Object Serialization</span><span class="sxs-lookup"><span data-stu-id="9faad-126">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="9faad-127">Formattatore di tipo di supporto JSON</span><span class="sxs-lookup"><span data-stu-id="9faad-127">JSON Media-Type Formatter</span></span>

<span data-ttu-id="9faad-128">La formattazione JSON viene fornita dalla classe **JsonMediaTypeFormatter.JSON** format is provided by the JsonMediaTypeFormatter class.</span><span class="sxs-lookup"><span data-stu-id="9faad-128">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="9faad-129">Per impostazione predefinita, **JsonMediaTypeFormatter** utilizza la [libreria Json.NET](https://github.com/JamesNK/Newtonsoft.Json) per eseguire la serializzazione.</span><span class="sxs-lookup"><span data-stu-id="9faad-129">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="9faad-130">Json.NET è un progetto open source di terze parti.</span><span class="sxs-lookup"><span data-stu-id="9faad-130">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="9faad-131">Se si preferisce, è possibile configurare la classe **JsonMediaTypeFormatter** per utilizzare **DataContractJsonSerializer** anziché Json.NET.</span><span class="sxs-lookup"><span data-stu-id="9faad-131">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="9faad-132">A tale scopo, impostare la proprietà **UseDataContractJsonSerializer** su **true**:</span><span class="sxs-lookup"><span data-stu-id="9faad-132">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="9faad-133">Serializzazione JSON</span><span class="sxs-lookup"><span data-stu-id="9faad-133">JSON Serialization</span></span>

<span data-ttu-id="9faad-134">In questa sezione vengono descritti alcuni comportamenti specifici del formattatore JSON, usando il serializzatore [di Json.NET](https://github.com/JamesNK/Newtonsoft.Json) predefinito.</span><span class="sxs-lookup"><span data-stu-id="9faad-134">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="9faad-135">Questo non è pensato per essere una documentazione completa della biblioteca Json.NET; Per ulteriori informazioni, vedere la [documentazione Json.NET](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="9faad-135">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="9faad-136">Che cosa viene serializzato?</span><span class="sxs-lookup"><span data-stu-id="9faad-136">What Gets Serialized?</span></span>

<span data-ttu-id="9faad-137">Per impostazione predefinita, tutte le proprietà e i campi pubblici sono inclusi nel codice JSON serializzato.</span><span class="sxs-lookup"><span data-stu-id="9faad-137">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="9faad-138">Per omettere una proprietà o un campo, decorarlo con l'attributo **JsonIgnore.**</span><span class="sxs-lookup"><span data-stu-id="9faad-138">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="9faad-139">Se si &quot;preferisce&quot; un approccio opt-in, decorare la classe con il **DataContract** attributo.</span><span class="sxs-lookup"><span data-stu-id="9faad-139">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="9faad-140">Se questo attributo è presente, i membri vengono ignorati a meno che non dispongano di **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="9faad-140">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="9faad-141">È inoltre possibile utilizzare **DataMember** per serializzare i membri privati.</span><span class="sxs-lookup"><span data-stu-id="9faad-141">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="9faad-142">Proprietà di sola letturaRead-Only Properties</span><span class="sxs-lookup"><span data-stu-id="9faad-142">Read-Only Properties</span></span>

<span data-ttu-id="9faad-143">Le proprietà di sola lettura vengono serializzate per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="9faad-143">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="9faad-144">Date</span><span class="sxs-lookup"><span data-stu-id="9faad-144">Dates</span></span>

<span data-ttu-id="9faad-145">Per impostazione predefinita, Json.NET scrive le date in formato [ISO 8601.](http://www.w3.org/TR/NOTE-datetime)</span><span class="sxs-lookup"><span data-stu-id="9faad-145">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="9faad-146">Le date in formato UTC (Coordinated Universal Time) vengono scritte con un suffisso "".</span><span class="sxs-lookup"><span data-stu-id="9faad-146">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="9faad-147">Le date nell'ora locale includono un offset del fuso orario.</span><span class="sxs-lookup"><span data-stu-id="9faad-147">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="9faad-148">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9faad-148">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="9faad-149">Per impostazione predefinita, Json.NET mantiene il fuso orario.</span><span class="sxs-lookup"><span data-stu-id="9faad-149">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="9faad-150">È possibile eseguire l'override impostando la proprietà DateTime-oneHandling:You can override this by setting the DateTime'oneHandling property:</span><span class="sxs-lookup"><span data-stu-id="9faad-150">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="9faad-151">Se si preferisce utilizzare il`"\/Date(ticks)\/"`formato data Microsoft [JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) ( ) anziché ISO 8601, impostare la proprietà **DateFormatHandling** sulle impostazioni del serializzatore:</span><span class="sxs-lookup"><span data-stu-id="9faad-151">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="9faad-152">Stili rientri</span><span class="sxs-lookup"><span data-stu-id="9faad-152">Indenting</span></span>

<span data-ttu-id="9faad-153">Per scrivere un JSON rientrato, impostare l'impostazione **Formattazione** su **Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="9faad-153">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="9faad-154">Involucro di cammello</span><span class="sxs-lookup"><span data-stu-id="9faad-154">Camel Casing</span></span>

<span data-ttu-id="9faad-155">Per scrivere i nomi delle proprietà JSON con maiuscole e minuscole camel, senza modificare il modello di dati, impostare **CamelCasePropertyNamesContractResolver** sul serializzatore:</span><span class="sxs-lookup"><span data-stu-id="9faad-155">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="9faad-156">Oggetti anonimi e debolmente tipizzati</span><span class="sxs-lookup"><span data-stu-id="9faad-156">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="9faad-157">Un metodo di azione può restituire un oggetto anonimo e serializzarlo in JSON.</span><span class="sxs-lookup"><span data-stu-id="9faad-157">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="9faad-158">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9faad-158">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="9faad-159">The response message body will contain the following JSON:</span><span class="sxs-lookup"><span data-stu-id="9faad-159">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="9faad-160">Se l'API Web riceve oggetti JSON strutturati in modo non uniforme dai client, è possibile deserializzare il corpo della richiesta in un tipo **Newtonsoft.Json.Linq.JObject.**</span><span class="sxs-lookup"><span data-stu-id="9faad-160">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="9faad-161">Tuttavia, è in genere preferibile utilizzare oggetti dati fortemente tipizzati.</span><span class="sxs-lookup"><span data-stu-id="9faad-161">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="9faad-162">Quindi non è necessario analizzare i dati manualmente e si ottengono i vantaggi della convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="9faad-162">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="9faad-163">Il serializzatore XML non supporta i tipi anonimi o istanze **JObject.**</span><span class="sxs-lookup"><span data-stu-id="9faad-163">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="9faad-164">Se si usano queste funzionalità per i dati JSON, è necessario rimuovere il formattatore XML dalla pipeline, come descritto più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="9faad-164">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="9faad-165">Formattatore di tipo supporto XMLXML Media-Type Formatter</span><span class="sxs-lookup"><span data-stu-id="9faad-165">XML Media-Type Formatter</span></span>

<span data-ttu-id="9faad-166">La formattazione XML viene fornita dalla classe **XmlMediaTypeFormatter.**</span><span class="sxs-lookup"><span data-stu-id="9faad-166">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="9faad-167">Per impostazione predefinita, **XmlMediaTypeFormatter** utilizza la classe **DataContractSerializer** per eseguire la serializzazione.</span><span class="sxs-lookup"><span data-stu-id="9faad-167">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="9faad-168">Se si preferisce, è possibile configurare **XmlMediaTypeFormatter** in modo che utilizzi **XmlSerializer** anziché **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="9faad-168">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="9faad-169">A tale scopo, impostare la proprietà **UseXmlSerializer** su **true**:</span><span class="sxs-lookup"><span data-stu-id="9faad-169">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="9faad-170">La classe **XmlSerializer** supporta un set di tipi più ristretto rispetto a **DataContractSerializer**, ma offre un maggiore controllo sul codice XML risultante.</span><span class="sxs-lookup"><span data-stu-id="9faad-170">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="9faad-171">Considerare l'utilizzo di **XmlSerializer** se è necessario associare uno schema XML esistente.</span><span class="sxs-lookup"><span data-stu-id="9faad-171">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="9faad-172">Serializzazione XML</span><span class="sxs-lookup"><span data-stu-id="9faad-172">XML Serialization</span></span>

<span data-ttu-id="9faad-173">In questa sezione vengono descritti alcuni comportamenti specifici del formattatore XML, utilizzando l'impostazione predefinita **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="9faad-173">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="9faad-174">Per impostazione predefinita, DataContractSerializer si comporta come segue:By default, the DataContractSerializer behaves as follows:</span><span class="sxs-lookup"><span data-stu-id="9faad-174">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="9faad-175">Tutte le proprietà e i campi pubblici di lettura/scrittura vengono serializzati.</span><span class="sxs-lookup"><span data-stu-id="9faad-175">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="9faad-176">Per omettere una proprietà o un campo, decorarlo con l'attributo **IgnoreDataMember.**</span><span class="sxs-lookup"><span data-stu-id="9faad-176">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="9faad-177">I membri privati e protetti non vengono serializzati.</span><span class="sxs-lookup"><span data-stu-id="9faad-177">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="9faad-178">Le proprietà di sola lettura non vengono serializzate.</span><span class="sxs-lookup"><span data-stu-id="9faad-178">Read-only properties are not serialized.</span></span> <span data-ttu-id="9faad-179">(Tuttavia, il contenuto di una proprietà di raccolta di sola lettura vengono serializzati.)</span><span class="sxs-lookup"><span data-stu-id="9faad-179">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="9faad-180">I nomi delle classi e dei membri vengono scritti nel codice XML esattamente come appaiono nella dichiarazione di classe.</span><span class="sxs-lookup"><span data-stu-id="9faad-180">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="9faad-181">Viene utilizzato uno spazio dei nomi XML predefinito.</span><span class="sxs-lookup"><span data-stu-id="9faad-181">A default XML namespace is used.</span></span>

<span data-ttu-id="9faad-182">Se è necessario un maggiore controllo sulla serializzazione, è possibile decorare la classe con il **DataContract** attributo.</span><span class="sxs-lookup"><span data-stu-id="9faad-182">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="9faad-183">Quando questo attributo è presente, la classe viene serializzata come segue:</span><span class="sxs-lookup"><span data-stu-id="9faad-183">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="9faad-184">&quot;Approccio&quot; opt-in: le proprietà e i campi non vengono serializzati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="9faad-184">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="9faad-185">Per serializzare una proprietà o un campo, decorarlo con l'attributo **DataMember.**</span><span class="sxs-lookup"><span data-stu-id="9faad-185">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="9faad-186">Per serializzare un membro privato o protetto, decorarlo con l'attributo **DataMember.**</span><span class="sxs-lookup"><span data-stu-id="9faad-186">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="9faad-187">Le proprietà di sola lettura non vengono serializzate.</span><span class="sxs-lookup"><span data-stu-id="9faad-187">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="9faad-188">Per modificare la modalità di visualizzazione del nome della classe nel codice XML, impostare il parametro *Name* nell'attributo **DataContract.**</span><span class="sxs-lookup"><span data-stu-id="9faad-188">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="9faad-189">Per modificare la modalità di visualizzazione del nome di un membro nel codice XML, impostare il parametro *Name* nell'attributo **DataMember.**</span><span class="sxs-lookup"><span data-stu-id="9faad-189">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="9faad-190">Per modificare lo spazio dei nomi XML, impostare il parametro *Namespace* nella classe **DataContract.**</span><span class="sxs-lookup"><span data-stu-id="9faad-190">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="9faad-191">Proprietà di sola letturaRead-Only Properties</span><span class="sxs-lookup"><span data-stu-id="9faad-191">Read-Only Properties</span></span>

<span data-ttu-id="9faad-192">Le proprietà di sola lettura non vengono serializzate.</span><span class="sxs-lookup"><span data-stu-id="9faad-192">Read-only properties are not serialized.</span></span> <span data-ttu-id="9faad-193">Se una proprietà di sola lettura dispone di un campo privato di tipo backing, è possibile contrassegnare il campo privato con l'attributo **DataMember.**</span><span class="sxs-lookup"><span data-stu-id="9faad-193">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="9faad-194">Questo approccio richiede l'attributo **DataContract** nella classe.</span><span class="sxs-lookup"><span data-stu-id="9faad-194">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="9faad-195">Date</span><span class="sxs-lookup"><span data-stu-id="9faad-195">Dates</span></span>

<span data-ttu-id="9faad-196">Le date sono scritte in formato ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="9faad-196">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="9faad-197">Ad esempio, &quot;2012-05-23T20:21:37.9116538 .&quot;</span><span class="sxs-lookup"><span data-stu-id="9faad-197">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="9faad-198">Stili rientri</span><span class="sxs-lookup"><span data-stu-id="9faad-198">Indenting</span></span>

<span data-ttu-id="9faad-199">Per scrivere codice XML rientrato, impostare la proprietà **Indent** su **true**:</span><span class="sxs-lookup"><span data-stu-id="9faad-199">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="9faad-200">Impostazione di serializzatori XML per tipoSetting Per-Type XML Serializers</span><span class="sxs-lookup"><span data-stu-id="9faad-200">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="9faad-201">È possibile impostare serializzatori XML diversi per tipi CLR diversi.</span><span class="sxs-lookup"><span data-stu-id="9faad-201">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="9faad-202">Ad esempio, si potrebbe avere un particolare oggetto dati che richiede **XmlSerializer** per la compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="9faad-202">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="9faad-203">È possibile utilizzare **XmlSerializer** per questo oggetto e continuare a utilizzare **DataContractSerializer** per altri tipi.</span><span class="sxs-lookup"><span data-stu-id="9faad-203">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="9faad-204">Per impostare un serializzatore XML per un determinato tipo, chiamare **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="9faad-204">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="9faad-205">È possibile specificare un **XmlSerializer** o qualsiasi oggetto che deriva da **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="9faad-205">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="9faad-206">Rimozione del formattatore JSON o XML</span><span class="sxs-lookup"><span data-stu-id="9faad-206">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="9faad-207">È possibile rimuovere il formattatore JSON o il formattatore XML dall'elenco dei formattatori, se non si desidera utilizzarli.</span><span class="sxs-lookup"><span data-stu-id="9faad-207">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="9faad-208">I motivi principali per fare questo sono:</span><span class="sxs-lookup"><span data-stu-id="9faad-208">The main reasons to do this are:</span></span>

- <span data-ttu-id="9faad-209">Per limitare le risposte dell'API Web a un particolare tipo di supporto.</span><span class="sxs-lookup"><span data-stu-id="9faad-209">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="9faad-210">Ad esempio, è possibile decidere di supportare solo le risposte JSON e rimuovere il formattatore XML.</span><span class="sxs-lookup"><span data-stu-id="9faad-210">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="9faad-211">Per sostituire il formattatore predefinito con un formattatore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="9faad-211">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="9faad-212">Ad esempio, è possibile sostituire il formattatore JSON con l'implementazione personalizzata di un formattatore JSON.</span><span class="sxs-lookup"><span data-stu-id="9faad-212">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="9faad-213">Nel codice seguente viene illustrato come rimuovere i formattatori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="9faad-213">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="9faad-214">Chiamare questo dal metodo **di avvio\_dell'applicazione,** definito in Global.asax.Call this from your Application Start method, defined in Global.asax.</span><span class="sxs-lookup"><span data-stu-id="9faad-214">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="9faad-215">Gestione dei riferimenti a oggetti circolariHandling Circular Object References</span><span class="sxs-lookup"><span data-stu-id="9faad-215">Handling Circular Object References</span></span>

<span data-ttu-id="9faad-216">Per impostazione predefinita, i formattatori JSON e XML scrivono tutti gli oggetti come valori.</span><span class="sxs-lookup"><span data-stu-id="9faad-216">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="9faad-217">Se due proprietà fanno riferimento allo stesso oggetto o se lo stesso oggetto viene visualizzato due volte in una raccolta, il formattatore serializzerà l'oggetto due volte.</span><span class="sxs-lookup"><span data-stu-id="9faad-217">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="9faad-218">Si tratta di un problema particolare se l'oggetto grafico contiene cicli, perché il serializzatore genererà un'eccezione quando rileva un ciclo nel grafico.</span><span class="sxs-lookup"><span data-stu-id="9faad-218">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="9faad-219">Si considerino i modelli a oggetti e il controller seguenti.</span><span class="sxs-lookup"><span data-stu-id="9faad-219">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="9faad-220">Se si richiama questa azione, il formattatore genererà un'eccezione, che si traduce in una risposta di codice di stato 500 (Errore interno del server) al client.</span><span class="sxs-lookup"><span data-stu-id="9faad-220">Invoking this action will cause the formatter to throw an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="9faad-221">Per mantenere i riferimenti agli oggetti in JSON, aggiungere il codice seguente al metodo **Application\_Start** nel file Global.asax:</span><span class="sxs-lookup"><span data-stu-id="9faad-221">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="9faad-222">Ora l'azione del controller restituirà JSON simile al seguente:Now the controller action will return JSON that looks like this:</span><span class="sxs-lookup"><span data-stu-id="9faad-222">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="9faad-223">Si noti che &quot;&quot; il serializzatore aggiunge una proprietà $id a entrambi gli oggetti.</span><span class="sxs-lookup"><span data-stu-id="9faad-223">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="9faad-224">Inoltre, rileva che la proprietà Employee.Department crea un ciclo, in modo da&quot;&quot;sostituire&quot;&quot;il valore con un riferimento all'oggetto: $ref : 1 .</span><span class="sxs-lookup"><span data-stu-id="9faad-224">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="9faad-225">I riferimenti agli oggetti non sono standard in JSON.</span><span class="sxs-lookup"><span data-stu-id="9faad-225">Object references are not standard in JSON.</span></span> <span data-ttu-id="9faad-226">Prima di utilizzare questa funzionalità, valutare se i client saranno in grado di analizzare i risultati.</span><span class="sxs-lookup"><span data-stu-id="9faad-226">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="9faad-227">Potrebbe essere meglio semplicemente per rimuovere cicli dal grafico.</span><span class="sxs-lookup"><span data-stu-id="9faad-227">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="9faad-228">Ad esempio, il collegamento da Dipendente a Reparto non è realmente necessario in questo esempio.</span><span class="sxs-lookup"><span data-stu-id="9faad-228">For example, the link from Employee back to Department is not really needed in this example.</span></span>

<span data-ttu-id="9faad-229">Per mantenere i riferimenti agli oggetti in XML, sono disponibili due opzioni.</span><span class="sxs-lookup"><span data-stu-id="9faad-229">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="9faad-230">L'opzione più `[DataContract(IsReference=true)]` semplice consiste nell'aggiungere alla classe del modello.</span><span class="sxs-lookup"><span data-stu-id="9faad-230">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="9faad-231">Il parametro *IsReference* consente di abilitare i riferimenti agli oggetti.</span><span class="sxs-lookup"><span data-stu-id="9faad-231">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="9faad-232">Tenere presente che DataContract accetta la serializzazione, pertanto sarà necessario aggiungere anche gli attributi DataMember alle proprietà:Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span><span class="sxs-lookup"><span data-stu-id="9faad-232">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="9faad-233">Ora il formattatore produrrà CODICE XML simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="9faad-233">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="9faad-234">Se si desidera evitare gli attributi nella classe del modello, è disponibile un'altra opzione: creare una nuova istanza **di DataContractSerializer** specifica del tipo e impostare *preserveObjectReferences* su **true** nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="9faad-234">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="9faad-235">Impostare quindi questa istanza come serializzatore per tipo nel formattatore di tipo supporto XML.</span><span class="sxs-lookup"><span data-stu-id="9faad-235">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="9faad-236">Il codice seguente mostra come eseguire questa operazione:The following code show how to do this:</span><span class="sxs-lookup"><span data-stu-id="9faad-236">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="9faad-237">Test della serializzazione degli oggettiTesting Object Serialization</span><span class="sxs-lookup"><span data-stu-id="9faad-237">Testing Object Serialization</span></span>

<span data-ttu-id="9faad-238">Durante la progettazione dell'API Web, è utile testare la modalità di serializzazione degli oggetti dati.</span><span class="sxs-lookup"><span data-stu-id="9faad-238">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="9faad-239">È possibile eseguire questa operazione senza creare un controller o richiamare un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="9faad-239">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
