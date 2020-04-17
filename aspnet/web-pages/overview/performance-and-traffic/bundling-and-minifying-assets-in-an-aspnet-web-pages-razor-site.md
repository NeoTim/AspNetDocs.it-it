---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Raggruppamento e minimizzazione delle risorse in un sito di pagine Web (Razor) ASP.NET Documenti Microsoft
author: rick-anderson
description: Bundling e minimizzazione sono modi per rendere il tuo sito più veloce. Bundling consente di combinare più file JavaScript ( .js ) o più fogli di stile CSS (...
ms.author: riande
ms.date: 06/21/2012
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: 2a877c1e1a06ea2357f96b37ec4ae72f9f9c9ff3
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81539918"
---
# <a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="576ab-104">Creazione di bundle e minimizzazione degli asset nelle pagine Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="576ab-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="576ab-105">da parte [di Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="576ab-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="576ab-106">Bundling e minimizzazione sono modi per rendere il tuo sito più veloce.</span><span class="sxs-lookup"><span data-stu-id="576ab-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="576ab-107">Bundling consente di combinare più file JavaScript (*js*) o più file di foglio di stile CSS (*css*) in modo che possano essere scaricati come un'unità, anziché uno alla volta.</span><span class="sxs-lookup"><span data-stu-id="576ab-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="576ab-108">La minimizzazione spreme gli spazi vuoti ed esegue altri tipi di compressione per rendere i file scaricati il più piccoli possibili.</span><span class="sxs-lookup"><span data-stu-id="576ab-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="576ab-109">La versione RC di ASP.NET Web Pages 2 non supporta l'aggregazione e la minimizzazione perché il pacchetto che contiene gli elementi necessari non è ancora disponibile in Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="576ab-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="576ab-110">Ci scusiamo per l'inconveniente.</span><span class="sxs-lookup"><span data-stu-id="576ab-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="576ab-111">Il pacchetto dovrebbe essere disponibile nella versione finale di ASP.NET Pagine Web 2 e WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="576ab-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
