---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Compilazione il capitolo download per Entity Framework 5 MVC 4 esercitazioni | Microsoft Docs
author: Rick-Anderson
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 6b5d10ba9e878908953e999bd1fd44970acf4ca5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57065568"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="bce22-103">Compilazione il capitolo download per Entity Framework 5 MVC 4 esercitazioni</span><span class="sxs-lookup"><span data-stu-id="bce22-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>
====================
<span data-ttu-id="bce22-104">da [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="bce22-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[<span data-ttu-id="bce22-105">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="bce22-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="bce22-106">L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="bce22-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="bce22-107">Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="bce22-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


## <a name="building-the-chapter-downloads"></a><span data-ttu-id="bce22-108">Creazione dei download dei capitoli</span><span class="sxs-lookup"><span data-stu-id="bce22-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="bce22-109">Scaricare e decomprimere il file con estensione zip di esempio progetto.</span><span class="sxs-lookup"><span data-stu-id="bce22-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="bce22-110">Nel pacchetto di download decompresso, si noterà il file zip aggiuntivi, uno per il completamento di ogni capitolo.</span><span class="sxs-lookup"><span data-stu-id="bce22-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="bce22-111">Fare clic con il pulsante destro sul file zip desiderata, fare clic su **delle proprietà**, fare clic sui **Unblock** pulsante.</span><span class="sxs-lookup"><span data-stu-id="bce22-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="bce22-112">Decomprimere il file.</span><span class="sxs-lookup"><span data-stu-id="bce22-112">Unzip the file.</span></span>
4. <span data-ttu-id="bce22-113">Fare doppio clic il *CUx.sln* file per avviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bce22-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="bce22-114">Dal **strumenti** menu, fare clic su **Gestione pacchetti NuGet**, quindi **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="bce22-114">From the **Tools** menu, click **NuGet Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="bce22-115">In Package Manager Console (PMC), fare clic su **ripristinare**.</span><span class="sxs-lookup"><span data-stu-id="bce22-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="bce22-116">Uscire da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bce22-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="bce22-117">Riavviare Visual Studio, aprire il file della soluzione che è stata chiusa nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="bce22-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="bce22-118">In Package Manager Console (PMC), immettere il `Update-Database` comando:</span><span class="sxs-lookup"><span data-stu-id="bce22-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="bce22-119">Se viene visualizzato l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="bce22-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="bce22-120">*Il termine "Update-Database" non è riconosciuto come il nome di un cmdlet, funzione, file di script o programma eseguibile. Controllare l'ortografia del nome, o se è stato incluso un percorso, verificare che il percorso sia corretto e riprovare.*</span><span class="sxs-lookup"><span data-stu-id="bce22-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="bce22-121">Chiudere e riavviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bce22-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="bce22-122">Verrà eseguita ogni migrazione, quindi verrà eseguito il metodo di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="bce22-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="bce22-123">È ora possibile eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="bce22-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="bce22-124">Precedente</span><span class="sxs-lookup"><span data-stu-id="bce22-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
