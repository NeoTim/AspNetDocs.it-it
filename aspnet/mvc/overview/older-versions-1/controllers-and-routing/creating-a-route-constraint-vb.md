---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Creazione di un vincolo di Route (VB) | Microsoft Docs
author: StephenWalther
description: In questa esercitazione, Stephen Walther spiega come è possibile controllare la modalità browser richiede le route di corrispondenza creando vincoli di route con espressioni regolari.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 8c7b2274ff396f222382488ed877599e86ae5b99
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59412684"
---
# <a name="creating-a-route-constraint-vb"></a>Creazione di un vincolo di route (VB)

da [Stephen Walther](https://github.com/StephenWalther)

> In questa esercitazione, Stephen Walther spiega come è possibile controllare la modalità browser richiede le route di corrispondenza creando vincoli di route con espressioni regolari.


Vincoli di route vengono utilizzati per limitare le richieste del browser che corrispondono a una particolare route. È possibile usare un'espressione regolare per specificare un vincolo di route.

Si supponga, ad esempio, che è stata definita la route nel listato 1 nel file Global. asax.

**Listing 1 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

L'elenco 1 contiene una route denominata Product. È possibile usare la route di prodotto per eseguire il mapping alle richieste del browser il ProductController contenuta nel listato 2.

**Listing 2 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

Si noti che l'azione Details() esposto dal controller di prodotto accetta un singolo parametro denominato productId. Questo parametro è un parametro integer.

La route definita nel listato 1 corrisponderà a uno degli URL seguenti:

- / Prodotti/23
- / Prodotti/7

Sfortunatamente, la route corrisponde anche agli URL seguenti:

- / Prodotti/bLa
- / Prodotti/apple

Poiché l'azione Details() prevede un parametro intero, una richiesta che contiene un valore diverso da un valore intero verrà generato un errore. Ad esempio, se si digita il /Product/apple URL nel browser quindi si otterrà la pagina di errore nella figura 1.


[![La finestra di dialogo Nuovo progetto](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**Figura 01**: Viene visualizzata una pagina explode ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-route-constraint-vb/_static/image2.png))


Che cosa si vuole eseguire è trovare solo gli URL contenenti un productId integer appropriata. È possibile usare un vincolo quando si definisce una route per limitare gli URL che corrispondono alla route. La route di prodotto modificata nel listato 3 contiene un vincolo di espressione regolare che corrisponde solo numeri interi.

**Listato 3 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

L'espressione regolare \d+ corrisponde a uno o più numeri interi. Questo vincolo fa sì che la route di prodotto in modo che corrispondano gli URL seguenti:

- / Prodotto/3
- /Product/8999

Ma non gli URL seguenti:

- / Prodotti/apple
- / Prodotto

Queste richieste del browser verranno gestite da un'altra route o, se non esistono alcuna route corrispondente, un *la risorsa non è stata trovata* viene restituito l'errore.

> [!div class="step-by-step"]
> [Precedente](creating-custom-routes-vb.md)
> [Successivo](creating-a-custom-route-constraint-vb.md)
