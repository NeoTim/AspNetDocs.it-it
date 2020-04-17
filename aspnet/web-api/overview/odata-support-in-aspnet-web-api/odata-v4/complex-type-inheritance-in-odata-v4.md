---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Ereditarietà dei tipi complessi in OData v4 con ASP.NETAPI Web Documenti Microsoft
author: rick-anderson
description: In base alla specifica OData v4, un tipo complesso può ereditare da un altro tipo complesso. (Un tipo complesso è un tipo strutturato senza una chiave.) API Web...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: f433cf625c7d6ff4922d8c4a9954682fc0f1cc33
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543600"
---
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Ereditarietà dei tipi complessi in OData v4 con ASP.NET API WebComplex Type Inheritance in OData v4 with ASP.NET Web API

da parte [di Microsoft](https://github.com/microsoft)

> In base alla [specifica](http://www.odata.org/documentation/odata-version-4-0/)OData v4 , un tipo complesso può ereditare da un altro tipo complesso. (Un tipo *complesso* è un tipo strutturato senza una chiave.) L'API Web OData 5.3 supporta l'ereditarietà dei tipi complessi.
> 
> In questo argomento viene illustrato come compilare un modello EDM (Entity Data Model) con tipi di ereditarietà complessi. Per il codice sorgente completo, vedere [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni software utilizzate nell'esercitazione
> 
> 
> - API Web OData 5.3
> - OData v4

## <a name="model-hierarchy"></a>Gerarchia dei modelli

Per illustrare l'ereditarietà dei tipi complessi, verrà utilizzata la gerarchia di classi seguente.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape`è un tipo complesso astratto. `Rectangle`, `Triangle`e `Circle` sono tipi `Shape`complessi `RoundRectangle` derivati `Rectangle`da e deriva da . `Window`è un tipo di `Shape` entità e contiene un'istanza.

Di seguito sono riportate le classi CLR che definiscono questi tipi.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Creare il modello EDMBuild the EDM Model

Per creare l'EDM, è possibile utilizzare **ODataConventionModelBuilder**, che deduce le relazioni di ereditarietà dai tipi CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

È anche possibile compilare l'EDM in modo esplicito, utilizzando **ODataModelBuilder**. Questa operazione richiede più codice, ma offre un maggiore controllo sull'EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Questi due esempi creano lo stesso schema EDM.

## <a name="metadata-document"></a>Documento metadati

Ecco il documento di metadati OData, che mostra l'ereditarietà dei tipi complessi.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

Dal documento di metadati, è possibile vedere che:

- Il `Shape` tipo complesso è astratto.
- I `Rectangle` `Triangle`tipi `Circle` , e complex `Shape`hanno il tipo di base .
- Il `RoundRectangle` tipo ha `Rectangle`il tipo di base .

## <a name="casting-complex-types"></a>Casting di tipi complessi

Il cast su tipi complessi è ora supportato. Ad esempio, la query `Shape` seguente `Rectangle`esegue il cast di a a .

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Ecco il payload della risposta:Here's the response payload:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
