---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: "Associazione di parametri nell'API Web ASP.NET: ASP.NET 4.x"
author: MikeWasson
description: Viene descritto come API Web associa i parametri e come personalizzare il processo di associazione in ASP.NET 4.x.
ms.author: riande
ms.date: 07/11/2013
ms.custom: seoapril2019
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f121f12ce689a079412bbd5392fde4fea863ff1f
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59401972"
---
# <a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="e6a78-103">Associazione di parametri nell'API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e6a78-103">Parameter Binding in ASP.NET Web API</span></span>

<span data-ttu-id="e6a78-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e6a78-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e6a78-105">Questo articolo descrive come API Web associa parametri e come è possibile personalizzare il processo di associazione.</span><span class="sxs-lookup"><span data-stu-id="e6a78-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span> <span data-ttu-id="e6a78-106">Quando l'API Web chiama un metodo in un controller, è necessario impostare i valori per i parametri, un processo denominato *associazione*.</span><span class="sxs-lookup"><span data-stu-id="e6a78-106">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> 

<span data-ttu-id="e6a78-107">Per impostazione predefinita, API Web Usa le regole seguenti per associare i parametri:</span><span class="sxs-lookup"><span data-stu-id="e6a78-107">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="e6a78-108">Se il parametro è un tipo "semplice", API Web prova a ottenere il valore dall'URI.</span><span class="sxs-lookup"><span data-stu-id="e6a78-108">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="e6a78-109">I tipi semplici includono .NET [i tipi primitivi](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**e così via), oltre a **TimeSpan**, **DateTime**, **Guid**, **decimal**, e **stringa**, *plus* qualsiasi tipo con un convertitore di tipi può convertire da una stringa.</span><span class="sxs-lookup"><span data-stu-id="e6a78-109">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="e6a78-110">(Ulteriori informazioni sui convertitori di tipi in un secondo momento.)</span><span class="sxs-lookup"><span data-stu-id="e6a78-110">(More about type converters later.)</span></span>
- <span data-ttu-id="e6a78-111">Per i tipi complessi, API Web tenta di leggere il valore dal corpo del messaggio, usando un [formattatore di media type](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="e6a78-111">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="e6a78-112">Ad esempio, ecco un metodo del controller API Web tipico:</span><span class="sxs-lookup"><span data-stu-id="e6a78-112">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="e6a78-113">Il *id* parametro è un &quot;semplice&quot; digitare, in modo che l'API Web prova a ottenere il valore dall'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="e6a78-113">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="e6a78-114">Il *elemento* parametro è un tipo complesso, API Web Usa quindi un formattatore di media type per leggere il valore dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="e6a78-114">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="e6a78-115">Per ottenere un valore dall'URI, API Web Cerca nei dati di route e la stringa di query URI.</span><span class="sxs-lookup"><span data-stu-id="e6a78-115">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="e6a78-116">I dati della route viene popolati quando il sistema di routing analizza l'URI e lo fa corrisponda a una route.</span><span class="sxs-lookup"><span data-stu-id="e6a78-116">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="e6a78-117">Per altre informazioni, vedere [Routing e selezione dell'azione](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="e6a78-117">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="e6a78-118">Nella parte restante di questo articolo, mostrerò come è possibile personalizzare il processo di associazione del modello.</span><span class="sxs-lookup"><span data-stu-id="e6a78-118">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="e6a78-119">Per i tipi complessi, tuttavia, è consigliabile usare i formattatori di media type laddove possibile.</span><span class="sxs-lookup"><span data-stu-id="e6a78-119">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="e6a78-120">Un principio chiave del protocollo HTTP è che le risorse vengano inviate nel corpo del messaggio, utilizzando la negoziazione del contenuto per specificare la rappresentazione della risorsa.</span><span class="sxs-lookup"><span data-stu-id="e6a78-120">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="e6a78-121">Formattatori di Media type sono stati progettati per esattamente questo scopo.</span><span class="sxs-lookup"><span data-stu-id="e6a78-121">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="e6a78-122">Utilizzo [FromUri]</span><span class="sxs-lookup"><span data-stu-id="e6a78-122">Using [FromUri]</span></span>

<span data-ttu-id="e6a78-123">Per forzare l'API Web per la lettura di un tipo complesso dall'URI, aggiungere il **[FromUri]** al parametro.</span><span class="sxs-lookup"><span data-stu-id="e6a78-123">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="e6a78-124">L'esempio seguente definisce una `GeoPoint` tipo, insieme a un metodo del controller che ottiene il `GeoPoint` dall'URI.</span><span class="sxs-lookup"><span data-stu-id="e6a78-124">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="e6a78-125">Il client può inserire i valori di latitudine e longitudine nella stringa di query e API Web verranno usati per costruire un `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="e6a78-125">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="e6a78-126">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e6a78-126">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="e6a78-127">Utilizzo [FromBody]</span><span class="sxs-lookup"><span data-stu-id="e6a78-127">Using [FromBody]</span></span>

<span data-ttu-id="e6a78-128">Per forzare l'API Web da cui leggere il corpo della richiesta di un tipo semplice, aggiungere il **[FromBody]** al parametro:</span><span class="sxs-lookup"><span data-stu-id="e6a78-128">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="e6a78-129">In questo esempio, API Web utilizzerà un formattatore di media type per leggere il valore della *nome* dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="e6a78-129">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="e6a78-130">Di seguito è riportato un esempio di richiesta client.</span><span class="sxs-lookup"><span data-stu-id="e6a78-130">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="e6a78-131">Quando un parametro è [FromBody], API Web Usa l'intestazione Content-Type per selezionare un formattatore.</span><span class="sxs-lookup"><span data-stu-id="e6a78-131">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="e6a78-132">In questo esempio, è il tipo di contenuto &quot;application/json&quot; e il corpo della richiesta è una stringa JSON non elaborata (non un oggetto JSON).</span><span class="sxs-lookup"><span data-stu-id="e6a78-132">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="e6a78-133">Al massimo un parametro è consentito leggere dal corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="e6a78-133">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="e6a78-134">Pertanto, non funzionerà:</span><span class="sxs-lookup"><span data-stu-id="e6a78-134">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="e6a78-135">Il motivo di questa regola è che il corpo della richiesta potrebbe essere archiviato in un flusso non memorizzato nel buffer che può essere letto una sola volta.</span><span class="sxs-lookup"><span data-stu-id="e6a78-135">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="e6a78-136">Convertitori di tipi</span><span class="sxs-lookup"><span data-stu-id="e6a78-136">Type Converters</span></span>

<span data-ttu-id="e6a78-137">È possibile rendere API Web considera una classe come un tipo semplice (in modo che l'API Web proverà a eseguirne l'associazione dall'URI) mediante la creazione di un **TypeConverter** e fornendo una conversione di stringhe.</span><span class="sxs-lookup"><span data-stu-id="e6a78-137">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="e6a78-138">Il codice seguente illustra un `GeoPoint` classe che rappresenta un punto geografico, oltre a una **TypeConverter** che consente di convertire da stringhe a `GeoPoint` istanze.</span><span class="sxs-lookup"><span data-stu-id="e6a78-138">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="e6a78-139">Il `GeoPoint` classe è decorata con un **[TypeConverter]** attributo per specificare il convertitore di tipi.</span><span class="sxs-lookup"><span data-stu-id="e6a78-139">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="e6a78-140">(In questo esempio è stato ispirato dal post di blog di Mike stallo [come eseguire l'associazione agli oggetti personalizzati in firme di azione in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="e6a78-140">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="e6a78-141">A questo punto l'API Web considererà `GeoPoint` come un tipo semplice, vale a dire si tenterà di associare `GeoPoint` parametri dall'URI.</span><span class="sxs-lookup"><span data-stu-id="e6a78-141">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="e6a78-142">Non è necessario includere **[FromUri]** sul parametro.</span><span class="sxs-lookup"><span data-stu-id="e6a78-142">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="e6a78-143">Il client può richiamare il metodo con un URI simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="e6a78-143">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="e6a78-144">Strumenti di associazione</span><span class="sxs-lookup"><span data-stu-id="e6a78-144">Model Binders</span></span>

<span data-ttu-id="e6a78-145">Un'opzione più flessibile rispetto a un convertitore di tipi consiste nel creare uno strumento di associazione di modelli personalizzato.</span><span class="sxs-lookup"><span data-stu-id="e6a78-145">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="e6a78-146">Con uno strumento di associazione, è necessario accedere come richiesta HTTP, la descrizione dell'azione e i valori non elaborati dai dati di route.</span><span class="sxs-lookup"><span data-stu-id="e6a78-146">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="e6a78-147">Per creare uno strumento di associazione, implementare il **IModelBinder** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="e6a78-147">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="e6a78-148">Questa interfaccia definisce un singolo metodo, **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="e6a78-148">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="e6a78-149">Di seguito è uno strumento di associazione per `GeoPoint` oggetti.</span><span class="sxs-lookup"><span data-stu-id="e6a78-149">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="e6a78-150">Uno strumento di associazione ottiene i valori di input non elaborati da un *provider di valori*.</span><span class="sxs-lookup"><span data-stu-id="e6a78-150">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="e6a78-151">Questo progetto separa le due funzioni distinte:</span><span class="sxs-lookup"><span data-stu-id="e6a78-151">This design separates two distinct functions:</span></span>

- <span data-ttu-id="e6a78-152">Il provider di valori accetta la richiesta HTTP e popola un dizionario di coppie chiave-valore.</span><span class="sxs-lookup"><span data-stu-id="e6a78-152">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="e6a78-153">Lo strumento di associazione di modelli Usa questo dizionario per popolare il modello.</span><span class="sxs-lookup"><span data-stu-id="e6a78-153">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="e6a78-154">Il provider di valori predefiniti nell'API Web ottiene i valori di dati della route e la stringa di query.</span><span class="sxs-lookup"><span data-stu-id="e6a78-154">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="e6a78-155">Ad esempio, se l'URI è `http://localhost/api/values/1?location=48,-122`, il provider di valori Crea le seguenti coppie chiave-valore:</span><span class="sxs-lookup"><span data-stu-id="e6a78-155">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="e6a78-156">id = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="e6a78-156">id = &quot;1&quot;</span></span>
- <span data-ttu-id="e6a78-157">percorso = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="e6a78-157">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="e6a78-158">(Si suppone che il modello di route predefinito, ovvero &quot;api / {controller} / {id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="e6a78-158">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="e6a78-159">Il nome del parametro da associare è archiviato nel **ModelBindingContext.ModelName** proprietà.</span><span class="sxs-lookup"><span data-stu-id="e6a78-159">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="e6a78-160">Lo strumento di associazione di modelli cerca di una chiave con questo valore nel dizionario.</span><span class="sxs-lookup"><span data-stu-id="e6a78-160">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="e6a78-161">Se il valore presente e può essere convertito in un `GeoPoint`, lo strumento individuerebbe assegna il valore associato per il **ModelBindingContext.Model** proprietà.</span><span class="sxs-lookup"><span data-stu-id="e6a78-161">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="e6a78-162">Si noti che lo strumento di associazione del modello non è limitata a una conversione del tipo semplice.</span><span class="sxs-lookup"><span data-stu-id="e6a78-162">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="e6a78-163">In questo esempio viene innanzitutto esaminato lo strumento di associazione di modelli in una tabella di percorsi noti e se il problema persiste, utilizza la conversione di tipo.</span><span class="sxs-lookup"><span data-stu-id="e6a78-163">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

**<span data-ttu-id="e6a78-164">Impostare il gestore di associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="e6a78-164">Setting the Model Binder</span></span>**

<span data-ttu-id="e6a78-165">Esistono diversi modi per impostare un raccoglitore di modelli.</span><span class="sxs-lookup"><span data-stu-id="e6a78-165">There are several ways to set a model binder.</span></span> <span data-ttu-id="e6a78-166">In primo luogo, è possibile aggiungere un **[ModelBinder]** al parametro.</span><span class="sxs-lookup"><span data-stu-id="e6a78-166">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="e6a78-167">È anche possibile aggiungere un **[ModelBinder]** al tipo di attributo.</span><span class="sxs-lookup"><span data-stu-id="e6a78-167">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="e6a78-168">API Web utilizzerà lo strumento di associazione del modello specificato per tutti i parametri di quel tipo.</span><span class="sxs-lookup"><span data-stu-id="e6a78-168">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="e6a78-169">Infine, è possibile aggiungere un provider dello strumento di associazione del modello per il **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="e6a78-169">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="e6a78-170">Un provider dello strumento di associazione del modello è semplicemente una classe factory che crea uno strumento di associazione.</span><span class="sxs-lookup"><span data-stu-id="e6a78-170">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="e6a78-171">È possibile creare un provider mediante la derivazione dal [elemento ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) classe.</span><span class="sxs-lookup"><span data-stu-id="e6a78-171">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="e6a78-172">Tuttavia, se il gestore di associazione di modello gestisce un solo tipo, è più facile da utilizzare l'oggetto incorporato **SimpleModelBinderProvider**, che è progettato per questo scopo.</span><span class="sxs-lookup"><span data-stu-id="e6a78-172">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="e6a78-173">A tal fine, osservare il codice indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e6a78-173">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="e6a78-174">Con un provider di associazione di modelli, è comunque necessario aggiungere il **[ModelBinder]** al parametro, a indicare API Web che è necessario utilizzare uno strumento di associazione di modello e non un formattatore di media type.</span><span class="sxs-lookup"><span data-stu-id="e6a78-174">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="e6a78-175">Ma ora non è necessario specificare il tipo di strumento di associazione nell'attributo:</span><span class="sxs-lookup"><span data-stu-id="e6a78-175">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="e6a78-176">Provider di valori</span><span class="sxs-lookup"><span data-stu-id="e6a78-176">Value Providers</span></span>

<span data-ttu-id="e6a78-177">Ho accennato che uno strumento di associazione ottiene i valori da un provider di valori.</span><span class="sxs-lookup"><span data-stu-id="e6a78-177">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="e6a78-178">Per scrivere un provider di valori personalizzati, implementare il **IValueProvider** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="e6a78-178">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="e6a78-179">Di seguito è riportato un esempio che esegue il pull dei valori dai cookie nella richiesta:</span><span class="sxs-lookup"><span data-stu-id="e6a78-179">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="e6a78-180">È anche necessario creare una factory del provider di valore mediante la derivazione dal **ValueProviderFactory** classe.</span><span class="sxs-lookup"><span data-stu-id="e6a78-180">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="e6a78-181">Aggiungere la factory del provider di valore per il **HttpConfiguration** come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e6a78-181">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="e6a78-182">API Web di comporre tutte i provider di valori, pertanto, quando chiama uno strumento di associazione **ValueProvider.GetValue**, lo strumento individuerebbe riceve il valore del primo provider di valori che è in grado di produrre.</span><span class="sxs-lookup"><span data-stu-id="e6a78-182">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="e6a78-183">In alternativa, è possibile impostare la factory del provider di valore a livello di parametro usando il **ValueProvider** attributo, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="e6a78-183">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="e6a78-184">Questa impostazione, API Web Usa l'associazione di modelli con la factory del provider di valore specificato di non usare uno qualsiasi di altri provider di valore registrato.</span><span class="sxs-lookup"><span data-stu-id="e6a78-184">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="e6a78-185">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="e6a78-185">HttpParameterBinding</span></span>

<span data-ttu-id="e6a78-186">Gli strumenti di associazione sono un'istanza specifica di un meccanismo più generale.</span><span class="sxs-lookup"><span data-stu-id="e6a78-186">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="e6a78-187">Se si esamina il **[ModelBinder]** attributo, si noterà che deriva dalla classe astratta **ParameterBindingAttribute** classe.</span><span class="sxs-lookup"><span data-stu-id="e6a78-187">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="e6a78-188">Questa classe definisce un singolo metodo, **GetBinding**, che restituisce un' **HttpParameterBinding** oggetto:</span><span class="sxs-lookup"><span data-stu-id="e6a78-188">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="e6a78-189">Un' **HttpParameterBinding** è responsabile per l'associazione di un parametro a un valore.</span><span class="sxs-lookup"><span data-stu-id="e6a78-189">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="e6a78-190">Nel caso del **[ModelBinder]**, l'attributo restituisce un **HttpParameterBinding** implementazione che usa un' **interfaccia IModelBinder** per eseguire l'associazione effettiva.</span><span class="sxs-lookup"><span data-stu-id="e6a78-190">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="e6a78-191">È anche possibile implementare una propria **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="e6a78-191">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="e6a78-192">Ad esempio, si supponga di voler ottenere eTag dalla `if-match` e `if-none-match` intestazioni nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="e6a78-192">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="e6a78-193">Si inizierà con la definizione di una classe per rappresentare gli ETag.</span><span class="sxs-lookup"><span data-stu-id="e6a78-193">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="e6a78-194">Si definirà anche un'enumerazione per indicare se il valore ETag da ottenere il `if-match` intestazione o il `if-none-match` intestazione.</span><span class="sxs-lookup"><span data-stu-id="e6a78-194">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="e6a78-195">Di seguito è riportato un **HttpParameterBinding** che ottiene il valore ETag nell'intestazione desiderato e lo associa a un parametro di tipo ETag:</span><span class="sxs-lookup"><span data-stu-id="e6a78-195">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="e6a78-196">Il **ExecuteBindingAsync** metodo esegue l'associazione.</span><span class="sxs-lookup"><span data-stu-id="e6a78-196">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="e6a78-197">All'interno di questo metodo, aggiungere il valore del parametro associato per il **oggetto ActionArgument** dizionario nel **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="e6a78-197">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="e6a78-198">Se il **ExecuteBindingAsync** metodo legge il corpo del messaggio di richiesta, sostituire il **WillReadBody** proprietà da restituire true.</span><span class="sxs-lookup"><span data-stu-id="e6a78-198">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="e6a78-199">Il corpo della richiesta potrebbe essere un flusso non memorizzato nel buffer che può essere letto una sola volta, in modo che l'API Web consente di applicare una regola che al massimo uno di associazione può leggere il corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="e6a78-199">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="e6a78-200">Per applicare un oggetto personalizzato **HttpParameterBinding**, è possibile definire un attributo che deriva da **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="e6a78-200">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="e6a78-201">Per la `ETagParameterBinding`, verranno definiti due attributi, uno per `if-match` informazioni sulle intestazioni e uno per `if-none-match` intestazioni.</span><span class="sxs-lookup"><span data-stu-id="e6a78-201">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="e6a78-202">Entrambi derivano dalla classe base astratta.</span><span class="sxs-lookup"><span data-stu-id="e6a78-202">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="e6a78-203">Ecco un metodo del controller che utilizza il `[IfNoneMatch]` attributo.</span><span class="sxs-lookup"><span data-stu-id="e6a78-203">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="e6a78-204">Altre origini oltre a **ParameterBindingAttribute**, è presente un altro hook per l'aggiunta di un oggetto personalizzato **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="e6a78-204">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="e6a78-205">Nel **HttpConfiguration** oggetti, il **ParameterBindingRules** proprietà è una raccolta di funzioni anonime del tipo (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="e6a78-205">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anonymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="e6a78-206">Ad esempio, è possibile aggiungere una regola che utilizza qualsiasi parametro ETag in un metodo GET `ETagParameterBinding` con `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="e6a78-206">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="e6a78-207">La funzione deve restituire `null` per i parametri in cui l'associazione non è applicabile.</span><span class="sxs-lookup"><span data-stu-id="e6a78-207">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="e6a78-208">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="e6a78-208">IActionValueBinder</span></span>

<span data-ttu-id="e6a78-209">L'intero processo di associazione di parametri è controllato da un servizio modulare **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="e6a78-209">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="e6a78-210">L'implementazione predefinita di **IActionValueBinder** esegue le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="e6a78-210">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="e6a78-211">Cercare un **ParameterBindingAttribute** sul parametro.</span><span class="sxs-lookup"><span data-stu-id="e6a78-211">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="e6a78-212">Sono inclusi **[FromBody]**, **[FromUri]**, e **[ModelBinder]**, o gli attributi personalizzati.</span><span class="sxs-lookup"><span data-stu-id="e6a78-212">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="e6a78-213">In caso contrario, esaminare **HttpConfiguration.ParameterBindingRules** per una funzione che restituisce un valore non null **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="e6a78-213">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="e6a78-214">In caso contrario, usare le regole predefinite che descritti in precedenza.</span><span class="sxs-lookup"><span data-stu-id="e6a78-214">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="e6a78-215">Se il tipo di parametro è "semplice" o ha un convertitore di tipi, eseguire l'associazione dall'URI.</span><span class="sxs-lookup"><span data-stu-id="e6a78-215">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="e6a78-216">Ciò equivale a inserire il **[FromUri]** attributo sul parametro.</span><span class="sxs-lookup"><span data-stu-id="e6a78-216">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="e6a78-217">In caso contrario, provare a leggere il parametro dal corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="e6a78-217">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="e6a78-218">Ciò equivale a inserire **[FromBody]** sul parametro.</span><span class="sxs-lookup"><span data-stu-id="e6a78-218">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="e6a78-219">Se si desidera, è possibile sostituire l'intera **IActionValueBinder** servizio con un'implementazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="e6a78-219">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e6a78-220">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e6a78-220">Additional Resources</span></span>

[<span data-ttu-id="e6a78-221">Esempio di associazione di parametri personalizzati</span><span class="sxs-lookup"><span data-stu-id="e6a78-221">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="e6a78-222">Mike stallo ha scritto una buona serie di post di blog sull'associazione di parametri di API Web:</span><span class="sxs-lookup"><span data-stu-id="e6a78-222">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="e6a78-223">Funzionamento delle API Web sull'associazione di parametri</span><span class="sxs-lookup"><span data-stu-id="e6a78-223">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="e6a78-224">Associazione di parametro di tipo MVC per API Web</span><span class="sxs-lookup"><span data-stu-id="e6a78-224">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="e6a78-225">Come eseguire l'associazione agli oggetti personalizzati in firme di azione MVC o Web API</span><span class="sxs-lookup"><span data-stu-id="e6a78-225">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="e6a78-226">Come creare un provider di valori personalizzati in API Web</span><span class="sxs-lookup"><span data-stu-id="e6a78-226">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="e6a78-227">Associazione di parametri di API Web dietro le quinte</span><span class="sxs-lookup"><span data-stu-id="e6a78-227">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
