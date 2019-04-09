---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: 'Contenuto la negoziazione in ASP.NET Web API: ASP.NET 4.x'
author: MikeWasson
description: Viene descritto come API Web ASP.NET implementa la negoziazione del contenuto HTTP per ASP.NET 4.x.
ms.author: riande
ms.date: 05/20/2012
ms.custom: seoapril2019
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: cb6668ff6de276d3778ce11f27ce597d8bf1f9c7
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59380160"
---
# <a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="ec340-103">Negoziazione del contenuto nell'API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ec340-103">Content Negotiation in ASP.NET Web API</span></span>

<span data-ttu-id="ec340-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ec340-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ec340-105">Questo articolo descrive come API Web ASP.NET implementa la negoziazione del contenuto per ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="ec340-105">This article describes how ASP.NET Web API implements content negotiation for ASP.NET 4.x.</span></span>

<span data-ttu-id="ec340-106">La specifica HTTP (RFC 2616) definisce la negoziazione del contenuto come "il processo di selezione la migliore rappresentazione per una determinata risposta quando sono disponibili più rappresentazioni."</span><span class="sxs-lookup"><span data-stu-id="ec340-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="ec340-107">Il meccanismo principale per la negoziazione del contenuto di tipo HTTP sono le intestazioni della richiesta:</span><span class="sxs-lookup"><span data-stu-id="ec340-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="ec340-108">**Accettare:** Quali tipi di supporto sono accettabili per la risposta, ad esempio "application/json", "application/xml" o un tipo di supporto personalizzato, ad esempio &quot;application/vnd.example+xml&quot;</span><span class="sxs-lookup"><span data-stu-id="ec340-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="ec340-109">**Charset Accept:** I set di caratteri sono accettabili, ad esempio UTF-8 o ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="ec340-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="ec340-110">**Codifica:** Le codifiche di contenuto sono accettabili, ad esempio gzip.</span><span class="sxs-lookup"><span data-stu-id="ec340-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="ec340-111">**Accept-Language:** Il linguaggio naturale preferito, ad esempio "en-us".</span><span class="sxs-lookup"><span data-stu-id="ec340-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="ec340-112">Il server può anche esaminare altre parti della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="ec340-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="ec340-113">Ad esempio, se la richiesta contiene un'intestazione X-Requested-With, che indica una richiesta AJAX, il server potrebbe per impostazione predefinita in formato JSON se non è presente alcuna intestazione Accept.</span><span class="sxs-lookup"><span data-stu-id="ec340-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="ec340-114">In questo articolo, esamineremo come API Web Usa le intestazioni Accept e Accept-Charset.</span><span class="sxs-lookup"><span data-stu-id="ec340-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="ec340-115">(A questo punto, non vi è alcun supporto predefinito per Accept-Encoding o Accept-Language).</span><span class="sxs-lookup"><span data-stu-id="ec340-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="ec340-116">Serializzazione</span><span class="sxs-lookup"><span data-stu-id="ec340-116">Serialization</span></span>

<span data-ttu-id="ec340-117">Se un controller Web API restituisce una risorsa come tipo CLR, la pipeline serializza il valore restituito e lo scrive nel corpo della risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="ec340-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="ec340-118">Ad esempio, si consideri l'azione del controller seguente:</span><span class="sxs-lookup"><span data-stu-id="ec340-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="ec340-119">Un client può inviare la richiesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="ec340-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="ec340-120">In risposta, il server potrebbe inviare:</span><span class="sxs-lookup"><span data-stu-id="ec340-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="ec340-121">In questo esempio, il client ha richiesto JSON, Javascript o "qualsiasi" (\*/\*).</span><span class="sxs-lookup"><span data-stu-id="ec340-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="ec340-122">Il server ha risposto con una rappresentazione JSON del `Product` oggetto.</span><span class="sxs-lookup"><span data-stu-id="ec340-122">The server responded with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="ec340-123">Si noti che l'intestazione Content-Type nella risposta è impostata su &quot;application/json&quot;.</span><span class="sxs-lookup"><span data-stu-id="ec340-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="ec340-124">Un controller può restituire anche un **HttpResponseMessage** oggetto.</span><span class="sxs-lookup"><span data-stu-id="ec340-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="ec340-125">Per specificare un oggetto CLR per il corpo della risposta, chiamare il **CreateResponse** metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="ec340-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="ec340-126">Questa opzione offre maggiore controllo sui dettagli della risposta.</span><span class="sxs-lookup"><span data-stu-id="ec340-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="ec340-127">È possibile impostare il codice di stato, aggiungere le intestazioni HTTP e così via.</span><span class="sxs-lookup"><span data-stu-id="ec340-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="ec340-128">L'oggetto che viene serializzata la risorsa viene chiamato un *formattatore di media*.</span><span class="sxs-lookup"><span data-stu-id="ec340-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="ec340-129">Formattatori di Media derivano dal **MediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="ec340-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="ec340-130">API Web fornisce formattatori di media per XML e JSON ed è possibile creare i formattatori personalizzati per supportare altri tipi di supporto.</span><span class="sxs-lookup"><span data-stu-id="ec340-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="ec340-131">Per informazioni sulla scrittura di un formattatore personalizzato, vedere [formattatori di Media](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="ec340-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="ec340-132">Funziona come contenuto di negoziazione</span><span class="sxs-lookup"><span data-stu-id="ec340-132">How Content Negotiation Works</span></span>

<span data-ttu-id="ec340-133">In primo luogo, la pipeline Ottiene il **IContentNegotiator** servizio il **HttpConfiguration** oggetto.</span><span class="sxs-lookup"><span data-stu-id="ec340-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="ec340-134">Ottiene anche l'elenco di formattatori di file multimediali dal **HttpConfiguration.Formatters** raccolta.</span><span class="sxs-lookup"><span data-stu-id="ec340-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="ec340-135">Successivamente, chiama la pipeline **IContentNegotiator.Negotiate**passando:</span><span class="sxs-lookup"><span data-stu-id="ec340-135">Next, the pipeline calls **IContentNegotiator.Negotiate**, passing in:</span></span>

- <span data-ttu-id="ec340-136">Il tipo di oggetto da serializzare</span><span class="sxs-lookup"><span data-stu-id="ec340-136">The type of object to serialize</span></span>
- <span data-ttu-id="ec340-137">La raccolta di formattatori di file multimediali</span><span class="sxs-lookup"><span data-stu-id="ec340-137">The collection of media formatters</span></span>
- <span data-ttu-id="ec340-138">La richiesta HTTP</span><span class="sxs-lookup"><span data-stu-id="ec340-138">The HTTP request</span></span>

<span data-ttu-id="ec340-139">Il **Negotiate** metodo restituisce due tipi di informazioni:</span><span class="sxs-lookup"><span data-stu-id="ec340-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="ec340-140">Il formattatore da utilizzare</span><span class="sxs-lookup"><span data-stu-id="ec340-140">Which formatter to use</span></span>
- <span data-ttu-id="ec340-141">Il tipo di supporto per la risposta</span><span class="sxs-lookup"><span data-stu-id="ec340-141">The media type for the response</span></span>

<span data-ttu-id="ec340-142">Se non viene trovato alcun formattatore, il **Negotiate** metodo restituisce **null**, e il client riceve errori HTTP 406 (pagina non valida).</span><span class="sxs-lookup"><span data-stu-id="ec340-142">If no formatter is found, the **Negotiate** method returns **null**, and the client receives HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="ec340-143">Il codice seguente viene illustrato come un controller può richiamare direttamente la negoziazione del contenuto:</span><span class="sxs-lookup"><span data-stu-id="ec340-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="ec340-144">Questo codice è equivalente a elementi di cui la pipeline viene eseguita automaticamente.</span><span class="sxs-lookup"><span data-stu-id="ec340-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="ec340-145">Negoziatore del contenuto predefinita</span><span class="sxs-lookup"><span data-stu-id="ec340-145">Default Content Negotiator</span></span>

<span data-ttu-id="ec340-146">Il **DefaultContentNegotiator** classe fornisce l'implementazione predefinita di **IContentNegotiator**.</span><span class="sxs-lookup"><span data-stu-id="ec340-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="ec340-147">Usa criteri diversi per selezionare un formattatore.</span><span class="sxs-lookup"><span data-stu-id="ec340-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="ec340-148">In primo luogo, il formattatore deve essere in grado di serializzare il tipo.</span><span class="sxs-lookup"><span data-stu-id="ec340-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="ec340-149">Questa operazione viene verificata chiamando **MediaTypeFormatter.CanWriteType**.</span><span class="sxs-lookup"><span data-stu-id="ec340-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="ec340-150">Successivamente, il negoziatore del contenuto esamina ciascun formattatore e viene valutata come associa la richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="ec340-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="ec340-151">Per valutare la corrispondenza, il negoziatore del contenuto esamina due cose sul formattatore:</span><span class="sxs-lookup"><span data-stu-id="ec340-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="ec340-152">Il **SupportedMediaTypes** insieme che contiene un elenco di tipi di supporto.</span><span class="sxs-lookup"><span data-stu-id="ec340-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="ec340-153">Negoziatore del contenuto Cerca la corrispondenza con l'elenco con l'intestazione Accept della richiesta.</span><span class="sxs-lookup"><span data-stu-id="ec340-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="ec340-154">Si noti che l'intestazione Accept può includere intervalli.</span><span class="sxs-lookup"><span data-stu-id="ec340-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="ec340-155">Ad esempio, "text/plain" è una corrispondenza per il testo /\* oppure \* / \*.</span><span class="sxs-lookup"><span data-stu-id="ec340-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="ec340-156">Il **MediaTypeMappings** insieme che contiene un elenco delle **MediaTypeMapping** oggetti.</span><span class="sxs-lookup"><span data-stu-id="ec340-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="ec340-157">Il **MediaTypeMapping** classe fornisce un modo generico per corrispondono alle richieste HTTP con i tipi di supporto.</span><span class="sxs-lookup"><span data-stu-id="ec340-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="ec340-158">Ad esempio, è stato possibile eseguire il mapping di un'intestazione HTTP personalizzata a un tipo di supporti particolare.</span><span class="sxs-lookup"><span data-stu-id="ec340-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="ec340-159">Se sono presenti più corrispondenze, la corrispondenza con il server wins factor qualità più elevata.</span><span class="sxs-lookup"><span data-stu-id="ec340-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="ec340-160">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ec340-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="ec340-161">In questo esempio, applicazione/json è un fattore di qualità implicita pari a 1,0, pertanto è preferito su application/xml.</span><span class="sxs-lookup"><span data-stu-id="ec340-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="ec340-162">Se viene trovata alcuna corrispondenza, il negoziatore del contenuto tenta di corrispondenza per il tipo di supporti del corpo della richiesta, se presente.</span><span class="sxs-lookup"><span data-stu-id="ec340-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="ec340-163">Ad esempio, se la richiesta contiene i dati JSON, il negoziatore del contenuto Cerca un formattatore JSON.</span><span class="sxs-lookup"><span data-stu-id="ec340-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="ec340-164">Se non sono ancora presenti corrispondenze, il negoziatore del contenuto sceglie semplicemente il primo formattatore in grado di serializzare il tipo.</span><span class="sxs-lookup"><span data-stu-id="ec340-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="ec340-165">Selezionare una codifica dei caratteri</span><span class="sxs-lookup"><span data-stu-id="ec340-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="ec340-166">Dopo aver selezionato un formattatore, il negoziatore del contenuto sceglie la codifica dei caratteri migliori esaminando il **SupportedEncodings** proprietà il formattatore e trovare una corrispondenza con l'intestazione Accept-Charset nella richiesta (se presente).</span><span class="sxs-lookup"><span data-stu-id="ec340-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
