---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Uso di SignalR con App Web nel servizio App di Azure | Microsoft Docs
author: bradygaster
description: Questo documento descrive come configurare un'applicazione di SignalR in esecuzione su Microsoft Azure. Le versioni del software utilizzato nell'esercitazione di Visual Studio 2013 o vis...
ms.author: bradyg
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 13eb5d29a2c40f52aed4b569ec8695f014a05f03
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57036518"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="c8385-104">Uso di SignalR con app Web in Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="c8385-104">Using SignalR with Web Apps in Azure App Service</span></span>
====================
<span data-ttu-id="c8385-105">da [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="c8385-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="c8385-106">Questo documento descrive come configurare un'applicazione di SignalR in esecuzione su Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c8385-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c8385-107">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="c8385-107">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="c8385-108">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) o Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="c8385-108">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) or Visual Studio 2012</span></span>
> - <span data-ttu-id="c8385-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="c8385-109">.NET 4.5</span></span>
> - <span data-ttu-id="c8385-110">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="c8385-110">SignalR version 2</span></span>
> - <span data-ttu-id="c8385-111">Azure SDK 2.3 per Visual Studio 2013 o 2012</span><span class="sxs-lookup"><span data-stu-id="c8385-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="c8385-112">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="c8385-112">Questions and comments</span></span>
>
> <span data-ttu-id="c8385-113">Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="c8385-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="c8385-114">Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), o il [forum di Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="c8385-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>


## <a name="table-of-contents"></a><span data-ttu-id="c8385-115">Sommario</span><span class="sxs-lookup"><span data-stu-id="c8385-115">Table of Contents</span></span>

- [<span data-ttu-id="c8385-116">Introduzione</span><span class="sxs-lookup"><span data-stu-id="c8385-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="c8385-117">Distribuzione di un'App Web di SignalR in servizio App di Azure</span><span class="sxs-lookup"><span data-stu-id="c8385-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="c8385-118">Abilitazione di WebSocket nel servizio App di Azure</span><span class="sxs-lookup"><span data-stu-id="c8385-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="c8385-119">Usando il Backplane di Cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="c8385-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="c8385-120">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="c8385-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="c8385-121">Introduzione</span><span class="sxs-lookup"><span data-stu-id="c8385-121">Introduction</span></span>

<span data-ttu-id="c8385-122">ASP.NET SignalR è utilizzabile per visualizzare un nuovo livello di interattività tra i server e web o i client .NET.</span><span class="sxs-lookup"><span data-stu-id="c8385-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="c8385-123">Quando ospitata in Azure, SignalR le applicazioni possono sfruttare la disponibilità elevata, scalabili, e ambiente ad alte prestazioni per l'esecuzione nel cloud.</span><span class="sxs-lookup"><span data-stu-id="c8385-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="c8385-124">Distribuzione di un'App Web di SignalR in servizio App di Azure</span><span class="sxs-lookup"><span data-stu-id="c8385-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="c8385-125">SignalR non aggiunge le complicazioni particolare alla distribuzione di un'applicazione in Azure rispetto alla distribuzione a un server in locale.</span><span class="sxs-lookup"><span data-stu-id="c8385-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="c8385-126">Un'applicazione che usa SignalR può essere ospitata in Azure senza apportare modifiche di configurazione o altre impostazioni (anche se per il supporto di WebSocket, vedere [abilitazione di WebSocket nel servizio App di Azure](#websocket) sotto.) Per questa esercitazione si distribuirà l'applicazione creata nel [esercitazione introduttiva su](../getting-started/tutorial-getting-started-with-signalr.md) in Azure.</span><span class="sxs-lookup"><span data-stu-id="c8385-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="c8385-127">**Prerequisiti**</span><span class="sxs-lookup"><span data-stu-id="c8385-127">**Prerequisites**</span></span>

- <span data-ttu-id="c8385-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="c8385-128">Visual Studio 2013.</span></span> <span data-ttu-id="c8385-129">Se non hai Visual Studio, Visual Studio 2013 Express per Web è incluso nell'installazione di Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="c8385-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="c8385-130">[Azure SDK 2.3 per Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) oppure [Azure SDK 2.3 per Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span><span class="sxs-lookup"><span data-stu-id="c8385-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="c8385-131">Per completare questa esercitazione, è necessaria una sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="c8385-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="c8385-132">È possibile [attivare i benefici della sottoscrizione MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), o [iscriversi per una sottoscrizione di valutazione](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c8385-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="c8385-133">Distribuzione di un'app web di SignalR in Azure</span><span class="sxs-lookup"><span data-stu-id="c8385-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="c8385-134">Completare la [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md), o scaricare il progetto completato dal [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="c8385-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="c8385-135">In Visual Studio, selezionare **compilare**, **pubblicare SignalR Chat**.</span><span class="sxs-lookup"><span data-stu-id="c8385-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="c8385-136">Nella finestra di dialogo "Pubblica sul Web", selezionare "siti Web di Windows Azure".</span><span class="sxs-lookup"><span data-stu-id="c8385-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Selezionare i siti Web di Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="c8385-138">Se si non sono connessi al tuo account Microsoft, fare clic su **Accedi...**  nella finestra di dialogo "selezionare le sito Web esistente" e di accesso.</span><span class="sxs-lookup"><span data-stu-id="c8385-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![Selezionare il sito Web esistente](using-signalr-with-azure-web-sites/_static/image2.png)    ![Accedere ad Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="c8385-141">Nella finestra di dialogo "selezionare le sito Web esistente", fare clic su **New**.</span><span class="sxs-lookup"><span data-stu-id="c8385-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![Nuovo sito Web](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="c8385-143">Nella finestra di dialogo "Crea sito in Windows Azure", immettere un nome univoco dell'app.</span><span class="sxs-lookup"><span data-stu-id="c8385-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="c8385-144">Selezionare l'area più vicina all'utente nell'elenco a discesa area.</span><span class="sxs-lookup"><span data-stu-id="c8385-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="c8385-145">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="c8385-145">Click **Create**.</span></span>

    ![Crea sito in Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="c8385-147">Nella finestra di dialogo "Pubblica sul Web", fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="c8385-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![Pubblicazione del sito](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="c8385-149">Quando l'app è stata completata la pubblicazione, l'applicazione di SignalR Chat ospitata in App Web di servizio App di Azure verrà aperta in un browser.</span><span class="sxs-lookup"><span data-stu-id="c8385-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![Sito aperto in un browser](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="c8385-151">Abilitazione di protocolli WebSocket in App Web di servizio App di Azure</span><span class="sxs-lookup"><span data-stu-id="c8385-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="c8385-152">WebSocket deve essere abilitata in modo esplicito nell'app web da utilizzare in un'applicazione di SignalR. in caso contrario, sarà possibile usare altri protocolli (vedere [trasporti e i fallback](../getting-started/introduction-to-signalr.md#transports) per informazioni dettagliate).</span><span class="sxs-lookup"><span data-stu-id="c8385-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="c8385-153">Per poter usare i WebSocket in App Web di servizio App di Azure, attivarlo nella sezione di configurazione dell'app web.</span><span class="sxs-lookup"><span data-stu-id="c8385-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="c8385-154">A tale scopo, aprire l'app web nel [portale di gestione Azure](https://manage.windowsazure.com/)e scegliere Configura.</span><span class="sxs-lookup"><span data-stu-id="c8385-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![Scheda Configura](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="c8385-156">Nella parte superiore della pagina di configurazione, assicurarsi che .NET 4.5 viene utilizzato per l'app web.</span><span class="sxs-lookup"><span data-stu-id="c8385-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![Impostazione di .NET framework versione 4.5](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="c8385-158">Nella pagina di configurazione, nelle **WebSockets** impostazione, selezionare **su**.</span><span class="sxs-lookup"><span data-stu-id="c8385-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![Impostazione WebSocket: Attivato](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="c8385-160">Nella parte inferiore della pagina di configurazione, selezionare **salvare** per salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="c8385-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![Salvare le impostazioni](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="c8385-162">Usando il Backplane di Cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="c8385-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="c8385-163">Se si usano più istanze per l'app web e gli utenti di tali istanze devono interagire tra loro (in modo che, ad esempio, i messaggi di chat creati in una singola istanza possono raggiungere gli utenti connessi alle altre istanze), il [Cache Redis di Azure backplane](../performance/scaleout-with-redis.md) devono essere implementati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c8385-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="c8385-164">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="c8385-164">Next Steps</span></span>

<span data-ttu-id="c8385-165">Per altre informazioni sulle App Web in servizio App di Azure, vedere [Panoramica di App Web](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span><span class="sxs-lookup"><span data-stu-id="c8385-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
