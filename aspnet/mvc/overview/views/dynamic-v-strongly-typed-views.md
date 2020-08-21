---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Confronto tra visualizzazioni dinamiche Visualizzazioni fortemente tipizzate | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 30b84c71c86e455f15a659abf566750f1c6eea90
ms.sourcegitcommit: feb88edfb01b32f6fc9488f0f0ddb3c5b34e6ff0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/21/2020
ms.locfileid: "88702946"
---
# <a name="dynamic-v-strongly-typed-views"></a>Confronto tra visualizzazioni dinamiche e fortemente tipizzate

di [Rick Anderson](https://twitter.com/RickAndMSFT)

Esistono tre modi per passare le informazioni da un controller a una visualizzazione in ASP.NET MVC 3:

1. Come oggetto modello fortemente tipizzato.
2. Come tipo dinamico (tramite @model Dynamic)
3. Uso di ViewBag

Ho scritto una semplice applicazione di Blog MVC 3 top per confrontare e contrapporre le visualizzazioni dinamiche e fortemente tipizzate. Il controller inizia con un semplice elenco di Blog:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

Fare clic con il pulsante destro del mouse sul metodo IndexNotStonglyTyped () e aggiungere una visualizzazione Razor.

[![8475. NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Assicurarsi che la casella **Crea una visualizzazione fortemente tipizzata** non sia selezionata. La visualizzazione risultante non contiene molto:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Poiché si sta usando una visualizzazione dinamica e non fortemente tipizzata, IntelliSense non ci aiuta. Il codice completato è illustrato di seguito:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646. NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

A questo punto si aggiungerà una visualizzazione fortemente tipizzata. Aggiungere il codice seguente al controller:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]

Si noti che si tratta esattamente della stessa visualizzazione restituita (Blogs); chiamare come visualizzazione non fortemente tipizzata. Fare clic con il pulsante destro del mouse all'interno di *StonglyTypedIndex ()* e scegliere **Aggiungi visualizzazione**. Questa volta selezionare la classe del modello di **Blog** e selezionare **Elenca** come modello di impalcatura.

[![5658. StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

All'interno del nuovo modello di vista si ottiene il supporto IntelliSense.

[![7002. IntelliSense [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)
