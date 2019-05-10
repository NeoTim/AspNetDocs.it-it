---
uid: signalr/overview/older-versions/scaleout-with-redis
title: Scalabilità orizzontale di SignalR con Redis (SignalR 1.x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 5079cf3fa773faeac911eddc75dc66e94ad98bb0
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65117059"
---
# <a name="signalr-scaleout-with-redis-signalr-1x"></a><span data-ttu-id="472e5-102">Scale-out di SignalR con Redis (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="472e5-102">SignalR Scaleout with Redis (SignalR 1.x)</span></span>

<span data-ttu-id="472e5-103">dal [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="472e5-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="472e5-104">In questa esercitazione si userà [Redis](http://redis.io/) distribuire i messaggi in un'applicazione di SignalR che viene distribuita in due istanze separate di IIS.</span><span class="sxs-lookup"><span data-stu-id="472e5-104">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="472e5-105">Redis è un archivio chiave-valore in memoria.</span><span class="sxs-lookup"><span data-stu-id="472e5-105">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="472e5-106">Supporta inoltre un sistema di messaggistica con un modello publish/subscribe.</span><span class="sxs-lookup"><span data-stu-id="472e5-106">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="472e5-107">Backplane SignalR Redis Usa la funzionalità di pubblicazione/sottoscrizione per inoltrare i messaggi ad altri server.</span><span class="sxs-lookup"><span data-stu-id="472e5-107">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="472e5-108">Per questa esercitazione si userà tre server:</span><span class="sxs-lookup"><span data-stu-id="472e5-108">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="472e5-109">Due server che eseguono Windows, che verrà usato per distribuire un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="472e5-109">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="472e5-110">Un server che esegue Linux, che verrà usato per l'esecuzione di Redis.</span><span class="sxs-lookup"><span data-stu-id="472e5-110">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="472e5-111">Per le schermate contenute in questa esercitazione, ho utilizzato Ubuntu 12.04 TLS.</span><span class="sxs-lookup"><span data-stu-id="472e5-111">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="472e5-112">Se non si dispone di tre server fisici da usare, è possibile creare macchine virtuali in Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="472e5-112">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="472e5-113">Un'altra opzione consiste nel creare le macchine virtuali in Azure.</span><span class="sxs-lookup"><span data-stu-id="472e5-113">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="472e5-114">Anche se in questa esercitazione Usa l'implementazione di Redis ufficiale, è inoltre disponibile un' [porte Windows di Redis](https://github.com/MSOpenTech/redis) da MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="472e5-114">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="472e5-115">Il programma di installazione e configurazione sono diversi, ma in caso contrario, i passaggi sono uguali.</span><span class="sxs-lookup"><span data-stu-id="472e5-115">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="472e5-116">Scalabilità orizzontale di SignalR con Redis non supporta i cluster Redis.</span><span class="sxs-lookup"><span data-stu-id="472e5-116">SignalR scaleout with Redis does not support Redis clusters.</span></span>

## <a name="overview"></a><span data-ttu-id="472e5-117">Panoramica</span><span class="sxs-lookup"><span data-stu-id="472e5-117">Overview</span></span>

<span data-ttu-id="472e5-118">Prima di passare all'esercitazione dettagliata, ecco una rapida panoramica delle azioni da eseguire.</span><span class="sxs-lookup"><span data-stu-id="472e5-118">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="472e5-119">Installare Redis e avviare il server Redis.</span><span class="sxs-lookup"><span data-stu-id="472e5-119">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="472e5-120">Aggiungere i pacchetti NuGet per l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="472e5-120">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="472e5-121">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="472e5-121">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="472e5-122">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="472e5-122">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="472e5-123">Creare un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="472e5-123">Create a SignalR application.</span></span>
4. <span data-ttu-id="472e5-124">Aggiungere il codice seguente al Global. asax per configurare il backplane:</span><span class="sxs-lookup"><span data-stu-id="472e5-124">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="472e5-125">Ubuntu in Hyper-V</span><span class="sxs-lookup"><span data-stu-id="472e5-125">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="472e5-126">Con Windows Hyper-V, è possibile creare facilmente una VM Ubuntu in Windows Server.</span><span class="sxs-lookup"><span data-stu-id="472e5-126">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="472e5-127">Scaricare il file ISO di Ubuntu dalla [ http://www.ubuntu.com ](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="472e5-127">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="472e5-128">In Hyper-V, aggiungere una nuova macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="472e5-128">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="472e5-129">Nel **connessione disco rigido virtuale** passaggio, seleziona **creare un disco rigido virtuale**.</span><span class="sxs-lookup"><span data-stu-id="472e5-129">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="472e5-130">Nel **opzioni di installazione** passaggio, seleziona **file di immagine (con estensione ISO)**, fare clic su **Sfoglia**e individuare l'ISO di installazione di Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="472e5-130">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="472e5-131">Installare Redis</span><span class="sxs-lookup"><span data-stu-id="472e5-131">Install Redis</span></span>

<span data-ttu-id="472e5-132">Seguire i passaggi descritti in [ http://redis.io/download ](http://redis.io/download) per scaricare e compilare Redis.</span><span class="sxs-lookup"><span data-stu-id="472e5-132">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="472e5-133">Verranno compilati i file binari di Redis `src` directory.</span><span class="sxs-lookup"><span data-stu-id="472e5-133">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="472e5-134">Per impostazione predefinita, Redis non richiede una password.</span><span class="sxs-lookup"><span data-stu-id="472e5-134">By default, Redis does not require a password.</span></span> <span data-ttu-id="472e5-135">Per impostare una password, modificare il `redis.conf` file che si trova nella directory radice del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="472e5-135">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="472e5-136">(Eseguire una copia di backup del file prima di modificarlo!) Aggiungere la seguente direttiva a `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="472e5-136">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="472e5-137">Ora avviare il server Redis:</span><span class="sxs-lookup"><span data-stu-id="472e5-137">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="472e5-138">Aprire la porta 6379, ovvero la porta predefinita che Redis è in ascolto su.</span><span class="sxs-lookup"><span data-stu-id="472e5-138">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="472e5-139">(È possibile modificare il numero di porta nel file di configurazione).</span><span class="sxs-lookup"><span data-stu-id="472e5-139">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="472e5-140">Creare l'applicazione di SignalR</span><span class="sxs-lookup"><span data-stu-id="472e5-140">Create the SignalR Application</span></span>

<span data-ttu-id="472e5-141">Creare un'applicazione di SignalR seguendo una di queste esercitazioni:</span><span class="sxs-lookup"><span data-stu-id="472e5-141">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="472e5-142">Introduzione a SignalR</span><span class="sxs-lookup"><span data-stu-id="472e5-142">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="472e5-143">Introduzione a SignalR e MVC 4</span><span class="sxs-lookup"><span data-stu-id="472e5-143">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="472e5-144">Successivamente, modifichiamo l'applicazione di chat per supportare scalabilità orizzontale con Redis.</span><span class="sxs-lookup"><span data-stu-id="472e5-144">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="472e5-145">In primo luogo, aggiungere il pacchetto SignalR.Redis NuGet al progetto.</span><span class="sxs-lookup"><span data-stu-id="472e5-145">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="472e5-146">In Visual Studio dal **degli strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="472e5-146">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="472e5-147">Nella finestra della Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="472e5-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="472e5-148">Successivamente, aprire il file Global. asax.</span><span class="sxs-lookup"><span data-stu-id="472e5-148">Next, open the Global.asax file.</span></span> <span data-ttu-id="472e5-149">Aggiungere il codice seguente per il **Application\_avviare** metodo:</span><span class="sxs-lookup"><span data-stu-id="472e5-149">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="472e5-150">"server" è il nome del server Redis in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="472e5-150">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="472e5-151">*porta* è il numero di porta</span><span class="sxs-lookup"><span data-stu-id="472e5-151">*port* is the port number</span></span>
- <span data-ttu-id="472e5-152">"password" è la password che è definito nel file conf.</span><span class="sxs-lookup"><span data-stu-id="472e5-152">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="472e5-153">"AppName" è qualsiasi stringa.</span><span class="sxs-lookup"><span data-stu-id="472e5-153">"AppName" is any string.</span></span> <span data-ttu-id="472e5-154">SignalR crea un canale di Redis pub/sub con questo nome.</span><span class="sxs-lookup"><span data-stu-id="472e5-154">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="472e5-155">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="472e5-155">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="472e5-156">Distribuire ed eseguire l'applicazione</span><span class="sxs-lookup"><span data-stu-id="472e5-156">Deploy and Run the Application</span></span>

<span data-ttu-id="472e5-157">Preparare le istanze di Windows Server per distribuire l'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="472e5-157">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="472e5-158">Aggiungere il ruolo IIS.</span><span class="sxs-lookup"><span data-stu-id="472e5-158">Add the IIS role.</span></span> <span data-ttu-id="472e5-159">Include funzionalità di "Sviluppo di applicazioni", tra cui il protocollo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="472e5-159">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="472e5-160">Includere anche il servizio di gestione (elencati in "Strumenti di gestione").</span><span class="sxs-lookup"><span data-stu-id="472e5-160">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="472e5-161">**Installare Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="472e5-161">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="472e5-162">Quando si esegue Gestione IIS, verrà richiesto di installare piattaforma Web Microsoft oppure è possibile [scaricare il programma di installazione](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="472e5-162">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="472e5-163">Nel programma di installazione della piattaforma, eseguire la ricerca di Web Deploy e installare distribuzione Web 3.0</span><span class="sxs-lookup"><span data-stu-id="472e5-163">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="472e5-164">Verificare che il servizio di gestione Web sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="472e5-164">Check that the Web Management Service is running.</span></span> <span data-ttu-id="472e5-165">In caso contrario, avviare il servizio.</span><span class="sxs-lookup"><span data-stu-id="472e5-165">If not, start the service.</span></span> <span data-ttu-id="472e5-166">(Se il servizio di gestione Web non viene visualizzata nell'elenco dei servizi di Windows, assicurarsi che il servizio di gestione installato quando è stato aggiunto il ruolo IIS.)</span><span class="sxs-lookup"><span data-stu-id="472e5-166">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="472e5-167">Per impostazione predefinita, il servizio di gestione Web è in ascolto sulla porta TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="472e5-167">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="472e5-168">In Windows Firewall, creare una nuova regola in ingresso per consentire il traffico TCP sulla porta 8172.</span><span class="sxs-lookup"><span data-stu-id="472e5-168">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="472e5-169">Per altre informazioni, vedere [configurare le regole del Firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="472e5-169">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="472e5-170">(Se si ospita le macchine virtuali in Azure, è possibile farlo direttamente nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="472e5-170">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="472e5-171">Visualizzare [come configurare gli endpoint a una macchina virtuale](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="472e5-171">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="472e5-172">A questo punto si è pronti per distribuire il progetto di Visual Studio dal computer di sviluppo al server.</span><span class="sxs-lookup"><span data-stu-id="472e5-172">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="472e5-173">In Esplora soluzioni fare doppio clic la soluzione e fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="472e5-173">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="472e5-174">Per informazioni dettagliate documentazione sulla distribuzione web, vedere [mappa del contenuto di distribuzione Web per Visual Studio e ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="472e5-174">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="472e5-175">Se si distribuisce l'applicazione a due server, è possibile aprire ogni istanza in una finestra distinta del browser e vedere che possano ricevere i messaggi SignalR da un altro.</span><span class="sxs-lookup"><span data-stu-id="472e5-175">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="472e5-176">(Naturalmente, in un ambiente di produzione, i due server sarebbe si trovano dietro un bilanciamento del carico.)</span><span class="sxs-lookup"><span data-stu-id="472e5-176">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="472e5-177">Se si è interessati a visualizzare i messaggi che vengono inviate a Redis, è possibile usare la **redis-cli** client, che viene installato con Redis.</span><span class="sxs-lookup"><span data-stu-id="472e5-177">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
