---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Contenimento in OData V4 con l'API Web 2,2 | Microsoft Docs
author: rick-anderson
description: Tradizionalmente, è possibile accedere a un'entità solo se è stata incapsulata all'interno di un set di entità. Tuttavia OData V4 fornisce due opzioni aggiuntive, singleton e con...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 3be81eac9de4686a0d187396e951b121ea65bac4
ms.sourcegitcommit: a4c3c7e04e5f53cf8cd334f036d324976b78d154
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/29/2020
ms.locfileid: "84173004"
---
# <a name="containment-in-odata-v4-using-web-api-22"></a>Contenimento in OData V4 con l'API Web 2,2

di Jinfu Tan

> Tradizionalmente, è possibile accedere a un'entità solo se è stata incapsulata all'interno di un set di entità. Tuttavia OData V4 fornisce due opzioni aggiuntive, singleton e contenimento, entrambe supportate da WebAPI 2,2.

Questo argomento illustra come definire un contenuto in un endpoint OData in WebApi 2,2. Per altre informazioni sul contenimento, vedere la pagina relativa [all'indipendenza con OData V4](https://devblogs.microsoft.com/odata/tutorial-sample-containment-is-coming-with-odata-v4/). Per creare un endpoint OData V4 nell'API Web, vedere [creare un endpoint OData V4 usando API Web ASP.NET 2,2](create-an-odata-v4-endpoint.md).

In primo luogo, verrà creato un modello di dominio di contenimento nel servizio OData, usando questo modello di dati:

![Modello di dati](odata-containment-in-web-api-22/_static/image1.png)

Un account contiene molti PaymentInstruments (PI), ma non viene definito un set di entità per un PI. Al contrario, è possibile accedere al PIs solo tramite un account.

È possibile scaricare la soluzione usata in questo argomento da [codeplex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>Definizione del modello di dati

1. Definire i tipi CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    L' `Contained` attributo viene utilizzato per le proprietà di navigazione di contenimento.
2. Generare il modello EDM basato sui tipi CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    L'oggetto `ODataConventionModelBuilder` gestirà la compilazione del modello EDM se l' `Contained` attributo viene aggiunto alla proprietà di navigazione corrispondente. Se la proprietà è un tipo di raccolta, `GetCount(string NameContains)` verrà creata anche una funzione.

    I metadati generati appariranno come segue:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    L' `ContainsTarget` attributo indica che la proprietà di navigazione è un contenimento.

## <a name="define-the-containing-entity-set-controller"></a>Definire il controller del set di entità contenitore

Le entità contenute non hanno il proprio controller; l'azione è definita nel controller del set di entità contenitore. In questo esempio è presente un AccountsController, ma nessun PaymentInstrumentsController.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Se il percorso OData è di 4 o più segmenti, funziona solo il routing degli attributi, ad esempio `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` nel controller precedente. In caso contrario, sia l'attributo che il routing convenzionale funzionano: ad esempio, `GetPayInPIs(int key)` corrisponde a `GET ~/Accounts(1)/PayinPIs` .

*Grazie a Leo Hu per il contenuto originale di questo articolo.*
