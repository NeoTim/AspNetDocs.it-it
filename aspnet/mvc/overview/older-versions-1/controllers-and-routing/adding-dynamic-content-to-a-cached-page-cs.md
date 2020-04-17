---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Aggiunta di contenuto dinamico a una pagina memorizzata nella cache (C Documenti Microsoft
author: rick-anderson
description: Scopri come combinare contenuti dinamici e memorizzati nella cache nella stessa pagina. La sostituzione post-cache consente di visualizzare contenuto dinamico, ad esempio banner pubblicitari o...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 6c8cd70a15c1ae93f7cf9b0a026b37b07e489040
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542287"
---
# <a name="adding-dynamic-content-to-a-cached-page-c"></a>Aggiunta di contenuto dinamico a una pagina memorizzata nella cache (C#)

da parte [di Microsoft](https://github.com/microsoft)

> Scopri come combinare contenuti dinamici e memorizzati nella cache nella stessa pagina. La sostituzione post-cache consente di visualizzare contenuto dinamico, ad esempio banner pubblicitari o notizie, all'interno di una pagina memorizzata nella cache di output.

Sfruttando la memorizzazione nella cache di output, è possibile migliorare notevolmente le prestazioni di un'applicazione MVC ASP.NET. Anziché rigenerare una pagina ogni volta che viene richiesta, la pagina può essere generata una sola volta e memorizzata nella cache per più utenti.

Ma c'è un problema. Cosa succede se è necessario visualizzare contenuto dinamico nella pagina? Ad esempio, si supponga di voler visualizzare un banner pubblicitario nella pagina. Non vuoi che l'annuncio banner venga memorizzato nella cache in modo che ogni utente veda lo stesso annuncio. Non faresti soldi in quel modo!

Fortunatamente, c'è una soluzione facile. È possibile sfruttare una funzionalità del framework di ASP.NET denominata *post-cache substitution*. La sostituzione post-cache consente di sostituire il contenuto dinamico in una pagina memorizzata nella cache.

In genere, quando si esegue l'output della cache di una pagina utilizzando l'attributo [OutputCache], la pagina viene memorizzata nella cache sia sul server che sul client (browser Web). Quando si utilizza la sostituzione post-cache, una pagina viene memorizzata nella cache solo sul server.

#### <a name="using-post-cache-substitution"></a>Utilizzo della sostituzione post-CacheUsing Post-Cache Substitution

L'utilizzo della sostituzione post-cache richiede due passaggi. In primo luogo, è necessario definire un metodo che restituisce una stringa che rappresenta il contenuto dinamico che si desidera visualizzare nella pagina memorizzata nella cache. Successivamente, chiamare il metodo HttpResponse.WriteSubstitution() per inserire il contenuto dinamico nella pagina.

Si supponga, ad esempio, che si desidera visualizzare in modo casuale diversi elementi di notizie in una pagina memorizzata nella cache. La classe nel listato 1 espone un singolo metodo, denominato RenderNews(), che restituisce in modo casuale una notizia da un elenco di tre notizie.

**Listato 1 – Modelli: News.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

Per sfruttare i vantaggi della sostituzione post-cache, chiamate il metodo HttpResponse.WriteSubstitution(). Il metodo WriteSubstitution() imposta il codice per sostituire un'area della pagina memorizzata nella cache con contenuto dinamico. Il metodo WriteSubstitution() viene utilizzato per visualizzare la notizia casuale nella visualizzazione nel listato 2.

**Listato 2 – Visualizzazioni, Home, Indice.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

Il metodo RenderNews viene passato al metodo WriteSubstitution(). Si noti che il RenderNews metodo non viene chiamato (non sono presenti parentesi). Al contrario, un riferimento al metodo viene passato a WriteSubstitution().

La vista Indice viene memorizzata nella cache. La visualizzazione viene restituita dal controller nel listato 3. Si noti che l'azione Index() è decorata con un attributo [OutputCache] che causa la memorizzazione nella cache della visualizzazione Indice per 60 secondi.

**Listato 3 – Controller-HomeController.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

Anche se la visualizzazione Indice è memorizzata nella cache, diverse notizie casuali vengono visualizzate quando si richiede la pagina Indice. Quando si richiede la pagina di indice, il tempo visualizzato dalla pagina non cambia per 60 secondi (vedere Figura 1). Il fatto che l'ora non cambia dimostra che la pagina è memorizzata nella cache. Tuttavia, il contenuto inserito dal metodo WriteSubstitution() – la notizia casuale – cambia ad ogni richiesta.

**Figura 1 – Inserimento di notizie dinamiche in una pagina memorizzata nella cacheFigure 1 – Injecting dynamic news items in a cached page**

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Utilizzo della sostituzione post-Cache nei metodi di supportoUsing Post-Cache Substitution in Helper Methods

Un modo più semplice per sfruttare i vantaggi della sostituzione post-cache consiste nell'incapsulare la chiamata al metodo WriteSubstitution() all'interno di un metodo helper personalizzato. Questo approccio è illustrato dal metodo helper nel listato 4.This approach is illustrated by the helper method in Listing 4.

**Lista 4 – AdHelper.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

Il listato 4 contiene una classe statica che espone due metodi: RenderBanner() e RenderBannerInternal(). Il metodo RenderBanner() rappresenta il metodo helper effettivo. Questo metodo estende la classe HtmlHelper MVC standard ASP.NET in modo che sia possibile chiamare Html.RenderBanner() in una visualizzazione come qualsiasi altro metodo helper.

Il metodo RenderBanner() chiama il metodo HttpResponse.WriteSubstitution() passando il metodo RenderBannerInternal() al metodo WriteSubstitution().

Il metodo RenderBannerInternal() è un metodo privato. Questo metodo non verrà esposto come metodo di supporto. Il metodo RenderBannerInternal() restituisce in modo casuale un'immagine pubblicitaria banner da un elenco di tre immagini pubblicitarie banner.

La vista Index modificata nel listato 5 illustra come utilizzare il metodo helper RenderBanner(). Si noti &lt;che nella&gt; parte superiore della visualizzazione per importare lo spazio dei nomi MvcApplication1.Helpers è inclusa una direttiva % . Se si trascura di importare questo spazio dei nomi, il metodo RenderBanner() non verrà visualizzato come metodo nella proprietà Html.

**Listato 5 – Views, Home, Index.aspx (con il metodo RenderBanner())**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

Quando si richiede la pagina visualizzata dalla visualizzazione nel listato 5, viene visualizzato un banner diverso con ogni richiesta (vedere Figura 2). La pagina viene memorizzata nella cache, ma l'annuncio banner viene inserito dinamicamente dal metodo helper RenderBanner().

**Figura 2 – La vista indice che visualizza un banner pubblicitario casuale**

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come aggiornare dinamicamente il contenuto in una pagina memorizzata nella cache. Si è appreso come utilizzare il metodo HttpResponse.WriteSubstitution() per consentire l'invio di contenuto dinamico in una pagina memorizzata nella cache. Si è inoltre appreso come incapsulare la chiamata al metodo WriteSubstitution() all'interno di un metodo helper HTML.

Sfruttare la memorizzazione nella cache ogni volta che è possibile, può avere un impatto significativo sulle prestazioni delle applicazioni Web. Come spiegato in questa esercitazione, è possibile sfruttare la memorizzazione nella cache anche quando è necessario visualizzare contenuto dinamico nelle pagine.

> [!div class="step-by-step"]
> [Successivo](improving-performance-with-output-caching-cs.md)
> [precedente](creating-a-controller-cs.md)
