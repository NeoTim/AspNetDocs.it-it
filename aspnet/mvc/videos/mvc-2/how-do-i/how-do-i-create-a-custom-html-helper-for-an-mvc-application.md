---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: "Procedura: Creare un Helper HTML personalizzati per un'applicazione MVC? | Microsoft Docs"
author: rick-anderson
description: In questo video Chris Pels illustra come creare un HtmlHelper personalizzato che non è disponibile nel set di standard in un'applicazione MVC. Primo, un applica MVC di esempio...
ms.author: riande
ms.date: 12/11/2009
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 60953243d3038667e4f729b1394e68f0c9d7c178
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59415050"
---
# <a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a><span data-ttu-id="58dad-105">Procedura: Creare un Helper HTML personalizzati per un'applicazione MVC?</span><span class="sxs-lookup"><span data-stu-id="58dad-105">How Do I: Create a Custom HTML Helper for an MVC Application?</span></span>

<span data-ttu-id="58dad-106">da [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="58dad-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="58dad-107">In questo video Chris Pels illustra come creare un HtmlHelper personalizzato che non è disponibile nel set di standard in un'applicazione MVC.</span><span class="sxs-lookup"><span data-stu-id="58dad-107">In this video Chris Pels shows how to create a custom HtmlHelper that is not available in the standard set in an MVC application.</span></span> <span data-ttu-id="58dad-108">In primo luogo, un'applicazione MVC di esempio viene creata con un controller di demo e una vista per eseguire il test di HtmlHelper personalizzato.</span><span class="sxs-lookup"><span data-stu-id="58dad-108">First, a sample MVC application is created with a demo controller and view to test the custom HtmlHelper.</span></span> <span data-ttu-id="58dad-109">Successivamente, viene creato un modulo con una funzione pubblica che rappresenta un metodo di estensione che rappresenta l'implementazione di HtmlHelper personalizzato.</span><span class="sxs-lookup"><span data-stu-id="58dad-109">Next, a module is created with a public function that is an extension method which represents the implementation of the custom HtmlHelper.</span></span> <span data-ttu-id="58dad-110">L'helper personalizzato sia per la creazione di `<img>` tag in una pagina e riceve alcuni parametri in entrata, inclusi l'id, l'url e il testo alternativo per il tag di immagine.</span><span class="sxs-lookup"><span data-stu-id="58dad-110">The custom helper is for creating `<img>` tags in a page and receives several inbound parameters including the id, url, and alt text for the image tag.</span></span> <span data-ttu-id="58dad-111">La logica viene quindi aggiunto alla funzione per la restituzione dell'oggetto completo `<img>` tag con le informazioni specificate.</span><span class="sxs-lookup"><span data-stu-id="58dad-111">The logic is then added to the function for returning the completed `<img>` tag with the specified information.</span></span> <span data-ttu-id="58dad-112">Il HtmlHelper personalizzata viene utilizzata nella pagina di demo per visualizzare un'immagine.</span><span class="sxs-lookup"><span data-stu-id="58dad-112">Then the custom HtmlHelper is used on the demo page to display an image.</span></span> <span data-ttu-id="58dad-113">Infine, il HtmlHelper personalizzato viene espanso per includere più sostituzioni costruttore che offrono flessibilità per più facilmente creando diversi `<img>` tag.</span><span class="sxs-lookup"><span data-stu-id="58dad-113">Finally, the custom HtmlHelper is expanded to include multiple constructor overrides which provide flexibility for more easily creating different `<img>` tags.</span></span>

[<span data-ttu-id="58dad-114">&#9654;Guarda il video (18 minuti)</span><span class="sxs-lookup"><span data-stu-id="58dad-114">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> <span data-ttu-id="58dad-115">[Precedente](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [Successivo](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="58dad-115">[Previous](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
[Next](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span></span>
