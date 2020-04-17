---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Apri i tipi in OData v4 con ASP.NETAPI Web Documenti Microsoft
author: rick-anderson
description: In OData v4, un tipo aperto è un tipo strutturato che contiene proprietà dinamiche, oltre a tutte le proprietà dichiarate nella definizione del tipo. Apri...
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: d63c96df6614896b3b67eef94a8907e528572567
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543730"
---
# <a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Tipi aperti in OData v4 con API Web ASP.NET

da parte [di Microsoft](https://github.com/microsoft)

> In OData v4, un *tipo aperto* è un tipo strutturato che contiene proprietà dinamiche, oltre a tutte le proprietà dichiarate nella definizione del tipo. I tipi aperti consentono di aggiungere flessibilità ai modelli di dati. In questa esercitazione viene illustrato come utilizzare i tipi aperti in ASP.NET API Web OData.This tutorial shows how to use open types in ASP.NET Web API OData.
> 
> In questa esercitazione si presuppone che si sappia già come creare un endpoint OData nellASP.NETAPI Web.This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API. In caso contrario, iniziare leggendo [prima Create an OData v4 Endpoint.If](create-an-odata-v4-endpoint.md) not, start by reading Create an OData v4 Endpoint first.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni software utilizzate nell'esercitazione
> 
> 
> - API Web OData 5.3
> - OData v4

In primo luogo, alcuni terminologia OData:First, some OData terminology:

- Tipo di entità: un tipo strutturato con una chiave.
- Tipo complesso: un tipo strutturato senza chiave.Complex type: A structured type without a key.
- Tipo aperto: un tipo con proprietà dinamiche. Sia i tipi di entità che i tipi complessi possono essere aperti.

Il valore di una proprietà dinamica può essere un tipo primitivo, un tipo complesso o un tipo di enumerazione. o una raccolta di uno qualsiasi di questi tipi. Per ulteriori informazioni sui tipi aperti, vedere la [specifica OData v4](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Installare le librerie OData Web

Utilizzare NuGet Package Manager per installare le librerie OData dell'API Web più recenti. Dalla finestra Console di Gestione pacchetti:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Definire i tipi CLRDefine the CLR Types

Iniziare definendo i modelli EDM come tipi CLR.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Quando viene creato Entity Data Model (EDM),

- `Category`è un tipo di enumerazione.
- `Address`è un tipo complesso. Non dispone di una chiave, pertanto non è un tipo di entità.
- `Customer`è un tipo di entità. (Ha una chiave.)
- `Press`è un tipo complesso aperto.
- `Book`è un tipo di entità aperto.

Per creare un tipo aperto, il tipo `IDictionary<string, object>`CLR deve avere una proprietà di tipo , che contiene le proprietà dinamiche.

## <a name="build-the-edm-model"></a>Creare il modello EDMBuild the EDM Model

Se si utilizza **ODataConventionModelBuilder** per `Press` creare `Book` il modello EDM e vengono aggiunti `IDictionary<string, object>` automaticamente come tipi aperti, in base alla presenza di una proprietà.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

È anche possibile compilare l'EDM in modo esplicito, utilizzando **ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Aggiungere un controller ODataAdd an OData Controller

Successivamente, aggiungere un controller OData.Next, add an OData controller. Per questa esercitazione useremo un controller semplificato che supporta solo le richieste GET e POST e usa un elenco in memoria per archiviare le entità.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Si noti `Book` che la prima istanza non ha proprietà dinamiche. La `Book` seconda istanza ha le seguenti proprietà dinamiche:

- "Published": Tipo primitivo
- "Authors": raccolta di tipi primitivi
- "OtherCategories": raccolta di tipi di enumerazione.

Inoltre, `Press` la proprietà `Book` di tale istanza ha le seguenti proprietà dinamiche:

- "Blog": tipo primitivo
- "Indirizzo": tipo complesso

## <a name="query-the-metadata"></a>Interrogare i metadati

Per ottenere il documento di metadati `~/$metadata`OData, inviare una richiesta GET a . Il corpo della risposta dovrebbe essere simile al seguente:The response body should look similar to this:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

Dal documento di metadati, è possibile vedere che:

- Per `Book` i `Press` tipi e, `OpenType` il valore dell'attributo è true. I `Customer` `Address` tipi e non dispongono di questo attributo.
- Il `Book` tipo di entità ha tre proprietà dichiarate: ISBN, Title e Press. I metadati OData `Book.Properties` non includono la proprietà della classe CLR.
- Analogamente, `Press` il tipo complesso ha solo due proprietà dichiarate: Name e Category. I metadati non `Press.DynamicProperties` includono la proprietà della classe CLR.

## <a name="query-an-entity"></a>Query an Entity

Per ottenere il libro con ISBN uguale a "978-0-7356-7942-9", inviare una richiesta GET a `~/Books('978-0-7356-7942-9')`. Il corpo della risposta dovrebbe essere simile al seguente. (Rientrato per renderlo più leggibile.)

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Si noti che le proprietà dinamiche sono incluse inline con le proprietà dichiarate.

## <a name="post-an-entity"></a>POST di un'entità

Per aggiungere un'entità Book, `~/Books`inviare una richiesta POST a . Il client può impostare proprietà dinamiche nel payload della richiesta.

Ecco una richiesta di esempio. Prendere nota delle proprietà "Price" e "Published".

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Se si imposta un punto di interruzione nel metodo del controller, è possibile vedere che l'API Web ha aggiunto queste proprietà al `Properties` dizionario.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Risorse aggiuntive

[Esempio di tipo open OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
