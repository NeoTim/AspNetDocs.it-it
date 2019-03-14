---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Procedura:] Controllo la memorizzazione nella cache di una pagina ASP.NET in base alle informazioni personalizzate | Microsoft Docs'
author: rick-anderson
description: In questo video Chris Pels illustra come controllare i criteri per la memorizzazione nella cache una pagina ASP.NET in base alle informazioni personalizzate. Viene creata una pagina di esempio e quindi gli O....
ms.author: riande
ms.date: 02/19/2009
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: a9ed2baad3460441bc57d97bf74f6de5977db0c9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032228"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a>[Procedura:] Controllo la memorizzazione nella cache di una pagina ASP.NET in base alle informazioni personalizzate
====================
da [Chris Pels](https://twitter.com/chrispels)

In questo video Chris Pels illustra come controllare i criteri per la memorizzazione nella cache una pagina ASP.NET in base alle informazioni personalizzate. Viene creata una pagina di esempio e quindi la direttiva OutputCache viene usata con il VaryByCustom (attributo) che contiene un valore personalizzato. Successivamente, il metodo GetVaryCustomByString() viene sottoposto a override nel modulo di Global. asax che fornisce la gestione dell'attributo personalizzato. In tale metodo viene restituita una stringa che identifica in modo univoco la versione memorizzata nella cache della pagina. Infine, è disponibile una discussione su come la memorizzazione nella cache usando un valore personalizzato è possibile usare in diversi modi per un sito web.

[&#9654;Guarda il video (12 minuti)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
