---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Creazione di bundle e minimizzazione di risorse in un Web ASP.NET le pagine del sito (Razor) | Microsoft Docs
author: microsoft
description: Creazione di bundle e minimizzazione modi per rendere più rapida del sito. Creazione di bundle consente di combinare più file JavaScript (js) o più fogli di stile CSS (...
ms.author: riande
ms.date: 06/21/2012
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: 7d2cb2fe311b8aff20bfb378b329286701ed5b7d
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57059918"
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="dc300-104">Creazione di bundle e minimizzazione degli asset nelle pagine Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="dc300-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="dc300-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="dc300-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="dc300-106">Creazione di bundle e minimizzazione modi per rendere più rapida del sito.</span><span class="sxs-lookup"><span data-stu-id="dc300-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="dc300-107">Creazione di bundle consente di combinare più JavaScript (*js*) i file o più fogli di stile CSS (*CSS*) file in modo che possono essere scaricati come un'unità, anziché uno alla volta.</span><span class="sxs-lookup"><span data-stu-id="dc300-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="dc300-108">Minimizzazione comprime out lo spazio vuoto ed esegue altri tipi di compressione per rendere i file scaricati come piccole un possibile.</span><span class="sxs-lookup"><span data-stu-id="dc300-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="dc300-109">La versione RC di ASP.NET Web Pages 2 nepodporuje aggregazione e minimizzazione perché il pacchetto che contiene gli elementi necessari non è ancora disponibile in Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="dc300-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="dc300-110">Ci scusiamo per l'inconveniente.</span><span class="sxs-lookup"><span data-stu-id="dc300-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="dc300-111">Il pacchetto dovrà essere disponibile nella versione finale di ASP.NET Web Pages 2 e WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="dc300-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>