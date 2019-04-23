---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Host OWIN in un ruolo di lavoro di Azure | Microsoft Docs
author: MikeWasson
description: Questa esercitazione illustra come a Self-hosting OWIN in un ruolo di lavoro di Microsoft Azure. Open Web Interface for .NET (OWIN) definisce un'astrazione tra i server web di .NET...
ms.author: riande
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 129b6a8f411d482de75e7e5edc5cc919b4d2de52
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59419522"
---
# <a name="host-owin-in-an-azure-worker-role"></a><span data-ttu-id="31e4d-104">Ospitare OWIN in un ruolo di lavoro di Azure</span><span class="sxs-lookup"><span data-stu-id="31e4d-104">Host OWIN in an Azure Worker Role</span></span>

<span data-ttu-id="31e4d-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="31e4d-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="31e4d-106">Questa esercitazione illustra come a Self-hosting OWIN in un ruolo di lavoro di Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="31e4d-106">This tutorial shows how to self-host OWIN in a Microsoft Azure worker role.</span></span>
>
> <span data-ttu-id="31e4d-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) definisce un'astrazione tra i server web .NET e applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="31e4d-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="31e4d-108">OWIN consente di disaccoppiare l'applicazione web dal server, che rende ideale per un'applicazione web nel proprio processo, all'esterno di IIS di self-hosting OWIN – ad esempio, all'interno di un ruolo di lavoro di Azure.</span><span class="sxs-lookup"><span data-stu-id="31e4d-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
>
> <span data-ttu-id="31e4d-109">In questa esercitazione si apprenderà come indipendente su delle applicazioni OWIN all'interno di un ruolo di lavoro di Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="31e4d-109">In this tutorial, you'll learn how to self-host an OWIN applications inside a Microsoft Azure worker role.</span></span> <span data-ttu-id="31e4d-110">Per altre informazioni sui ruoli di lavoro, vedere [modelli di esecuzione di Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span><span class="sxs-lookup"><span data-stu-id="31e4d-110">To learn more about worker roles, see [Azure Execution Models](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="31e4d-111">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="31e4d-111">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="31e4d-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="31e4d-112">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - [<span data-ttu-id="31e4d-113">Azure SDK per .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="31e4d-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)
> - [<span data-ttu-id="31e4d-114">Microsoft.Owin.Selfhost 2.1.0</span><span class="sxs-lookup"><span data-stu-id="31e4d-114">Microsoft.Owin.Selfhost 2.1.0</span></span>](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="31e4d-115">Creare un progetto di Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="31e4d-115">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="31e4d-116">Avviare Visual Studio con privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="31e4d-116">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="31e4d-117">Sono necessari privilegi di amministratore per eseguire il debug in locale, l'applicazione usa l'emulatore di calcolo di Azure.</span><span class="sxs-lookup"><span data-stu-id="31e4d-117">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="31e4d-118">Nel **File** menu, fare clic su **New**, quindi fare clic su **progetto**.</span><span class="sxs-lookup"><span data-stu-id="31e4d-118">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="31e4d-119">Dal **modelli installati**, in Visual c#, fare clic su **Cloud** e quindi fare clic su **servizio Cloud Azure**.</span><span class="sxs-lookup"><span data-stu-id="31e4d-119">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="31e4d-120">Denominare il progetto "AzureApp" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="31e4d-120">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="31e4d-121">Nel **nuovo servizio Cloud di Windows Azure** finestra di dialogo, fare doppio clic su **ruolo di lavoro**.</span><span class="sxs-lookup"><span data-stu-id="31e4d-121">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="31e4d-122">Lasciare il nome predefinito ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="31e4d-122">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="31e4d-123">Questo passaggio aggiunge un ruolo di lavoro alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="31e4d-123">This step adds a worker role to the solution.</span></span> <span data-ttu-id="31e4d-124">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="31e4d-124">Click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="31e4d-125">La soluzione di Visual Studio creato contiene due progetti:</span><span class="sxs-lookup"><span data-stu-id="31e4d-125">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="31e4d-126">&quot;AzureApp&quot; definisce i ruoli e la configurazione per l'applicazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="31e4d-126">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="31e4d-127">&quot;WorkerRole1&quot; contiene il codice per il ruolo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="31e4d-127">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="31e4d-128">In generale, un'applicazione Azure può contenere più ruoli, sebbene questa esercitazione Usa un singolo ruolo.</span><span class="sxs-lookup"><span data-stu-id="31e4d-128">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a><span data-ttu-id="31e4d-129">Aggiungere i pacchetti di self-hosting OWIN</span><span class="sxs-lookup"><span data-stu-id="31e4d-129">Add the OWIN Self-Host Packages</span></span>

<span data-ttu-id="31e4d-130">Dal **degli strumenti** menu, fare clic su **Gestione pacchetti NuGet**, quindi fare clic su **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="31e4d-130">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="31e4d-131">Nella finestra della Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="31e4d-131">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="31e4d-132">Aggiungere un HTTP Endpoint</span><span class="sxs-lookup"><span data-stu-id="31e4d-132">Add an HTTP Endpoint</span></span>

<span data-ttu-id="31e4d-133">In Esplora soluzioni espandere il progetto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="31e4d-133">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="31e4d-134">Espandere il nodo ruoli, selezionare e fare doppio clic su WorkerRole1 **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="31e4d-134">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="31e4d-135">Fare clic su **endpoint**, quindi fare clic su **Add Endpoint**.</span><span class="sxs-lookup"><span data-stu-id="31e4d-135">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="31e4d-136">Nel **protocollo** dall'elenco a discesa selezionare "http".</span><span class="sxs-lookup"><span data-stu-id="31e4d-136">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="31e4d-137">Nelle **porta pubblica** e **porta privata**, digitare 80.</span><span class="sxs-lookup"><span data-stu-id="31e4d-137">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="31e4d-138">Questi numeri di porta possono essere diversi.</span><span class="sxs-lookup"><span data-stu-id="31e4d-138">These port numbers can be different.</span></span> <span data-ttu-id="31e4d-139">La porta pubblica è ciò che i client usano quando si invia una richiesta al ruolo.</span><span class="sxs-lookup"><span data-stu-id="31e4d-139">The public port is what clients use when they send a request to the role.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a><span data-ttu-id="31e4d-140">Creare la classe di avvio OWIN</span><span class="sxs-lookup"><span data-stu-id="31e4d-140">Create the OWIN Startup Class</span></span>

<span data-ttu-id="31e4d-141">In Esplora soluzioni fare clic con il pulsante destro del progetto WorkerRole1 e selezionare **Add** / **classe** per aggiungere una nuova classe.</span><span class="sxs-lookup"><span data-stu-id="31e4d-141">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="31e4d-142">Assegnare alla classe il nome `Startup`.</span><span class="sxs-lookup"><span data-stu-id="31e4d-142">Name the class `Startup`.</span></span>

<span data-ttu-id="31e4d-143">Sostituire tutto il codice boilerplate con gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="31e4d-143">Replace all of the boilerplate code with the following:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

<span data-ttu-id="31e4d-144">Il `UseWelcomePage` metodo di estensione aggiunge una pagina HTML semplice all'applicazione, per verificare il sito sia funzionante.</span><span class="sxs-lookup"><span data-stu-id="31e4d-144">The `UseWelcomePage` extension method adds a simple HTML page to your application, to verify the site is working.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="31e4d-145">Avviare l'Host OWIN</span><span class="sxs-lookup"><span data-stu-id="31e4d-145">Start the OWIN Host</span></span>

<span data-ttu-id="31e4d-146">Aprire il file WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="31e4d-146">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="31e4d-147">Questa classe definisce il codice eseguito quando il ruolo di lavoro viene avviato e arrestato.</span><span class="sxs-lookup"><span data-stu-id="31e4d-147">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="31e4d-148">Aggiungere la seguente istruzione using:</span><span class="sxs-lookup"><span data-stu-id="31e4d-148">Add the following using statement:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="31e4d-149">Aggiungere un **IDisposable** membro per il `WorkerRole` classe:</span><span class="sxs-lookup"><span data-stu-id="31e4d-149">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="31e4d-150">Nel `OnStart` metodo, aggiungere il codice seguente per avviare l'host:</span><span class="sxs-lookup"><span data-stu-id="31e4d-150">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

<span data-ttu-id="31e4d-151">Il **WebApp.Start** metodo avvia l'host OWIN.</span><span class="sxs-lookup"><span data-stu-id="31e4d-151">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="31e4d-152">Il nome del `Startup` classe è un parametro di tipo al metodo.</span><span class="sxs-lookup"><span data-stu-id="31e4d-152">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="31e4d-153">Per convenzione, l'host chiamerà il `Configure` overload di questa classe.</span><span class="sxs-lookup"><span data-stu-id="31e4d-153">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="31e4d-154">Eseguire l'override di `OnStop` per eliminare il  *\_app* istanza:</span><span class="sxs-lookup"><span data-stu-id="31e4d-154">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

<span data-ttu-id="31e4d-155">Ecco il codice completo per WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="31e4d-155">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="31e4d-156">Compilare la soluzione e premere F5 per eseguire l'applicazione in locale nell'emulatore di calcolo di Azure.</span><span class="sxs-lookup"><span data-stu-id="31e4d-156">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="31e4d-157">A seconda delle impostazioni di firewall, potrebbe essere necessario consentire all'emulatore attraverso il firewall.</span><span class="sxs-lookup"><span data-stu-id="31e4d-157">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

<span data-ttu-id="31e4d-158">L'emulatore di calcolo viene assegnato un indirizzo IP locale per l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="31e4d-158">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="31e4d-159">È possibile trovare l'indirizzo IP, visualizzare l'interfaccia utente emulatore di calcolo.</span><span class="sxs-lookup"><span data-stu-id="31e4d-159">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="31e4d-160">Fare doppio clic sull'icona dell'emulatore nella barra area di notifica delle attività e selezionare **Mostra interfaccia emulatore di calcolo**.</span><span class="sxs-lookup"><span data-stu-id="31e4d-160">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="31e4d-161">Trovare l'indirizzo IP sotto le distribuzioni del servizio, la distribuzione [id], i dettagli del servizio.</span><span class="sxs-lookup"><span data-stu-id="31e4d-161">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="31e4d-162">Aprire un web browser e passare a http:\/\/*indirizzo*, dove *indirizzo* è l'indirizzo IP assegnato dall'emulatore di calcolo; ad esempio, `http://127.0.0.1:80`.</span><span class="sxs-lookup"><span data-stu-id="31e4d-162">Open a web browser and navigate to http:\/\/*address*, where *address* is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80`.</span></span> <span data-ttu-id="31e4d-163">Si dovrebbe vedere la pagina di benvenuto OWIN:</span><span class="sxs-lookup"><span data-stu-id="31e4d-163">You should see the OWIN welcome page:</span></span>

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="31e4d-164">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="31e4d-164">Deploy to Azure</span></span>

<span data-ttu-id="31e4d-165">Per questo passaggio, è necessario disporre di un account Azure.</span><span class="sxs-lookup"><span data-stu-id="31e4d-165">For this step, you must have an Azure account.</span></span> <span data-ttu-id="31e4d-166">Se non si ha già uno, è possibile creare un account di valutazione gratuito in pochi minuti.</span><span class="sxs-lookup"><span data-stu-id="31e4d-166">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="31e4d-167">Per informazioni dettagliate, vedere [versione di valutazione gratuita di Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="31e4d-167">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="31e4d-168">In Esplora soluzioni fare clic sul progetto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="31e4d-168">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="31e4d-169">Selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="31e4d-169">Select **Publish**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image12.png)

<span data-ttu-id="31e4d-170">Se non è stato eseguito l'accesso al proprio account Azure, fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="31e4d-170">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="31e4d-171">Dopo che è stato effettuato, scegliere una sottoscrizione e fare clic su **successivo**.</span><span class="sxs-lookup"><span data-stu-id="31e4d-171">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

<span data-ttu-id="31e4d-172">Immettere un nome per il servizio cloud e scegliere un'area.</span><span class="sxs-lookup"><span data-stu-id="31e4d-172">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="31e4d-173">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="31e4d-173">Click **Create**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image17.png)

<span data-ttu-id="31e4d-174">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="31e4d-174">Click **Publish**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="31e4d-175">La finestra Log attività di Azure Mostra lo stato di avanzamento della distribuzione.</span><span class="sxs-lookup"><span data-stu-id="31e4d-175">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="31e4d-176">Quando l'app viene distribuita, passa a `http://appname.cloudapp.net/`, dove *appname* è il nome del servizio cloud.</span><span class="sxs-lookup"><span data-stu-id="31e4d-176">When the app is deployed, browse to `http://appname.cloudapp.net/`, where *appname* is the name of your cloud service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="31e4d-177">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="31e4d-177">Additional Resources</span></span>

- [<span data-ttu-id="31e4d-178">Panoramica del progetto Katana</span><span class="sxs-lookup"><span data-stu-id="31e4d-178">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)
- [<span data-ttu-id="31e4d-179">Katana Project su GitHub</span><span class="sxs-lookup"><span data-stu-id="31e4d-179">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana/)
