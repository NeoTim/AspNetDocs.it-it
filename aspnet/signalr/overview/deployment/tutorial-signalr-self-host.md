---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Esercitazione: Self-host SignalR | Microsoft Docs'
author: bradygaster
description: Questa esercitazione illustra come creare un server SignalR 2 self-hosted e come connettersi a esso con un client JavaScript. Versioni del software usate nell'esercitazione V...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 7470e0d6b68772ccfd979c834b7db81fbba3ca78
ms.sourcegitcommit: 0cf7d06071a8ff986e6c028ac9daf0c0e7490412
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/23/2020
ms.locfileid: "85240610"
---
# <a name="tutorial-signalr-self-host"></a><span data-ttu-id="bec1e-104">Esercitazione: Hosting indipendente di SignalR</span><span class="sxs-lookup"><span data-stu-id="bec1e-104">Tutorial: SignalR Self-Host</span></span>

<span data-ttu-id="bec1e-105">di [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="bec1e-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="bec1e-106">Questa esercitazione illustra come creare un server SignalR 2 self-hosted e come connettersi a esso con un client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bec1e-106">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bec1e-107">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="bec1e-107">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="bec1e-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="bec1e-108">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="bec1e-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="bec1e-109">.NET 4.5</span></span>
> - <span data-ttu-id="bec1e-110">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="bec1e-110">SignalR version 2</span></span>
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="bec1e-111">Uso di Visual Studio 2012 con questa esercitazione</span><span class="sxs-lookup"><span data-stu-id="bec1e-111">Using Visual Studio 2012 with this tutorial</span></span>
>
>
> <span data-ttu-id="bec1e-112">Per usare Visual Studio 2012 con questa esercitazione, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="bec1e-112">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
>
> - <span data-ttu-id="bec1e-113">Aggiornare [Gestione pacchetti](http://docs.nuget.org/docs/start-here/installing-nuget) alla versione più recente.</span><span class="sxs-lookup"><span data-stu-id="bec1e-113">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="bec1e-114">Installare l' [installazione guidata piattaforma Web](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="bec1e-114">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="bec1e-115">Nell'installazione guidata piattaforma Web cercare e installare **ASP.NET and Web Tools 2013,1 per Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="bec1e-115">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="bec1e-116">In questo modo vengono installati i modelli di Visual Studio per le classi SignalR, ad esempio **Hub**.</span><span class="sxs-lookup"><span data-stu-id="bec1e-116">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="bec1e-117">Alcuni modelli, ad esempio **OWIN Startup Class**, non saranno disponibili. per questi, usare invece un file di classe.</span><span class="sxs-lookup"><span data-stu-id="bec1e-117">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="bec1e-118">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="bec1e-118">Questions and comments</span></span>
>
> <span data-ttu-id="bec1e-119">Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="bec1e-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="bec1e-120">In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="bec1e-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="bec1e-121">Panoramica</span><span class="sxs-lookup"><span data-stu-id="bec1e-121">Overview</span></span>

<span data-ttu-id="bec1e-122">Un server SignalR è in genere ospitato in un'applicazione ASP.NET in IIS, ma può anche essere indipendente (ad esempio in un'applicazione console o in un servizio Windows) usando la libreria Self-host.</span><span class="sxs-lookup"><span data-stu-id="bec1e-122">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="bec1e-123">Questa libreria, come all of SignalR 2, si basa su OWIN ([Open Web Interface for .NET](http://owin.org)).</span><span class="sxs-lookup"><span data-stu-id="bec1e-123">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="bec1e-124">OWIN definisce un'astrazione tra i server Web .NET e le applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="bec1e-124">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="bec1e-125">OWIN separa l'applicazione Web dal server, rendendo OWIN ideale per l'hosting automatico di un'applicazione Web nel proprio processo, al di fuori di IIS.</span><span class="sxs-lookup"><span data-stu-id="bec1e-125">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="bec1e-126">I motivi per non ospitare in IIS includono:</span><span class="sxs-lookup"><span data-stu-id="bec1e-126">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="bec1e-127">Ambienti in cui IIS non è disponibile o auspicabile, ad esempio un server farm esistente senza IIS.</span><span class="sxs-lookup"><span data-stu-id="bec1e-127">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="bec1e-128">È necessario evitare il sovraccarico delle prestazioni di IIS.</span><span class="sxs-lookup"><span data-stu-id="bec1e-128">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="bec1e-129">La funzionalità SignalR deve essere aggiunta a un'applicazione esistente eseguita in un servizio Windows, in un ruolo di lavoro di Azure o in un altro processo.</span><span class="sxs-lookup"><span data-stu-id="bec1e-129">SignalR functionality is to be added to an existing application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="bec1e-130">Se una soluzione viene sviluppata come Self-host per motivi di prestazioni, è consigliabile testare anche l'applicazione ospitata in IIS per determinare il vantaggio in termini di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="bec1e-130">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="bec1e-131">Questa esercitazione contiene le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="bec1e-131">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="bec1e-132">Creazione del server</span><span class="sxs-lookup"><span data-stu-id="bec1e-132">Creating the server</span></span>](#server)
- [<span data-ttu-id="bec1e-133">Accesso al server con un client JavaScript</span><span class="sxs-lookup"><span data-stu-id="bec1e-133">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="bec1e-134">Creazione del server</span><span class="sxs-lookup"><span data-stu-id="bec1e-134">Creating the server</span></span>

<span data-ttu-id="bec1e-135">In questa esercitazione verrà creato un server ospitato in un'applicazione console, ma il server può essere ospitato in qualsiasi tipo di processo, ad esempio un servizio Windows o un ruolo di lavoro di Azure.</span><span class="sxs-lookup"><span data-stu-id="bec1e-135">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="bec1e-136">Per il codice di esempio per l'hosting di un server SignalR in un servizio Windows, vedere la pagina relativa all' [hosting automatico di SignalR in un servizio Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span><span class="sxs-lookup"><span data-stu-id="bec1e-136">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="bec1e-137">Aprire Visual Studio 2013 con privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="bec1e-137">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="bec1e-138">Selezionare **file**, **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="bec1e-138">Select **File**, **New Project**.</span></span> <span data-ttu-id="bec1e-139">Selezionare **Windows** sotto il nodo **Visual C#** nel riquadro **modelli** e selezionare il modello **applicazione console** .</span><span class="sxs-lookup"><span data-stu-id="bec1e-139">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="bec1e-140">Assegnare al nuovo progetto il nome "SignalRSelfHost" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="bec1e-140">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="bec1e-141">Aprire la console di gestione pacchetti NuGet selezionando **strumenti**gestione pacchetti  >  **NuGet**  >  **console di gestione**pacchetti.</span><span class="sxs-lookup"><span data-stu-id="bec1e-141">Open the NuGet package manager console by selecting **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
3. <span data-ttu-id="bec1e-142">Nella console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="bec1e-142">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="bec1e-143">Questo comando aggiunge al progetto le librerie a host automatico SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="bec1e-143">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="bec1e-144">Nella console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="bec1e-144">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="bec1e-145">Questo comando aggiunge la libreria Microsoft. Owin. CORS al progetto.</span><span class="sxs-lookup"><span data-stu-id="bec1e-145">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="bec1e-146">Questa libreria verrà usata per il supporto tra domini, operazione necessaria per le applicazioni che ospitano SignalR e un client di pagine Web in domini diversi.</span><span class="sxs-lookup"><span data-stu-id="bec1e-146">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="bec1e-147">Poiché si ospiterà il server SignalR e il client Web su porte diverse, ciò significa che tra domini deve essere abilitato per la comunicazione tra questi componenti.</span><span class="sxs-lookup"><span data-stu-id="bec1e-147">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="bec1e-148">Sostituire il contenuto del file Program.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="bec1e-148">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="bec1e-149">Il codice precedente include tre classi:</span><span class="sxs-lookup"><span data-stu-id="bec1e-149">The above code includes three classes:</span></span>

    - <span data-ttu-id="bec1e-150">**Programma**, incluso il metodo **principale** che definisce il percorso principale di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="bec1e-150">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="bec1e-151">In questo metodo, un'applicazione Web di tipo **Startup** viene avviata all'URL specificato ( `http://localhost:8080` ).</span><span class="sxs-lookup"><span data-stu-id="bec1e-151">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="bec1e-152">Se per l'endpoint è richiesta la sicurezza, è possibile implementare SSL.</span><span class="sxs-lookup"><span data-stu-id="bec1e-152">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="bec1e-153">Per altre informazioni [, vedere Procedura: configurare una porta con un certificato SSL](https://msdn.microsoft.com/library/ms733791.aspx) .</span><span class="sxs-lookup"><span data-stu-id="bec1e-153">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="bec1e-154">**Avvio**, la classe che contiene la configurazione per il server SignalR (l'unica configurazione utilizzata in questa esercitazione è la chiamata a `UseCors` ) e la chiamata a `MapSignalR` , che consente di creare route per gli oggetti Hub nel progetto.</span><span class="sxs-lookup"><span data-stu-id="bec1e-154">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="bec1e-155">**MyHub**, la classe dell'hub SignalR che l'applicazione fornirà ai client.</span><span class="sxs-lookup"><span data-stu-id="bec1e-155">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="bec1e-156">Questa classe dispone di un solo metodo, **Send**, che i client chiameranno per trasmettere un messaggio a tutti gli altri client connessi.</span><span class="sxs-lookup"><span data-stu-id="bec1e-156">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="bec1e-157">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="bec1e-157">Compile and run the application.</span></span> <span data-ttu-id="bec1e-158">L'indirizzo che il server sta eseguendo dovrebbe essere visualizzato in una finestra della console.</span><span class="sxs-lookup"><span data-stu-id="bec1e-158">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="bec1e-159">Se l'esecuzione ha esito negativo con l'eccezione `System.Reflection.TargetInvocationException was unhandled` , sarà necessario riavviare Visual Studio con privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="bec1e-159">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="bec1e-160">Arrestare l'applicazione prima di procedere alla sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="bec1e-160">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="bec1e-161">Accesso al server con un client JavaScript</span><span class="sxs-lookup"><span data-stu-id="bec1e-161">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="bec1e-162">In questa sezione si userà lo stesso client JavaScript dell' [esercitazione Introduzione](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="bec1e-162">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="bec1e-163">Verrà apportata una sola modifica al client, ovvero definire in modo esplicito l'URL dell'hub.</span><span class="sxs-lookup"><span data-stu-id="bec1e-163">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="bec1e-164">Con un'applicazione indipendente, il server potrebbe non essere necessariamente nello stesso indirizzo dell'URL di connessione (a causa dei proxy inversi e dei bilanciamenti del carico), quindi l'URL deve essere definito in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="bec1e-164">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="bec1e-165">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla soluzione e scegliere **Aggiungi**, **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="bec1e-165">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="bec1e-166">Selezionare il nodo **Web** e selezionare il modello **applicazione Web ASP.NET** .</span><span class="sxs-lookup"><span data-stu-id="bec1e-166">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="bec1e-167">Denominare il progetto "JavascriptClient" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="bec1e-167">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="bec1e-168">Selezionare il modello **vuoto** e lasciare deselezionate le opzioni rimanenti.</span><span class="sxs-lookup"><span data-stu-id="bec1e-168">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="bec1e-169">Selezionare **Crea progetto**.</span><span class="sxs-lookup"><span data-stu-id="bec1e-169">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="bec1e-170">Nella console di gestione pacchetti selezionare il progetto "JavascriptClient" nell'elenco a discesa **progetto predefinito** ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="bec1e-170">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="bec1e-171">Questo comando consente di installare le librerie SignalR e JQuery necessarie nel client.</span><span class="sxs-lookup"><span data-stu-id="bec1e-171">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="bec1e-172">Fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi**, **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="bec1e-172">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="bec1e-173">Selezionare il nodo **Web** e selezionare pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="bec1e-173">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="bec1e-174">Denominare la pagina **Default.html**.</span><span class="sxs-lookup"><span data-stu-id="bec1e-174">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="bec1e-175">Sostituire il contenuto della nuova pagina HTML con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="bec1e-175">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="bec1e-176">Verificare che i riferimenti agli script corrispondano agli script nella cartella Scripts del progetto.</span><span class="sxs-lookup"><span data-stu-id="bec1e-176">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="bec1e-177">Il codice seguente (evidenziato nell'esempio di codice precedente) è l'aggiunta effettuata al client usato nell'esercitazione di acquisizione a stella (oltre all'aggiornamento del codice a SignalR versione 2 beta).</span><span class="sxs-lookup"><span data-stu-id="bec1e-177">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="bec1e-178">Questa riga di codice imposta in modo esplicito l'URL della connessione di base per SignalR nel server.</span><span class="sxs-lookup"><span data-stu-id="bec1e-178">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="bec1e-179">Fare clic con il pulsante destro del mouse sulla soluzione e selezionare **Imposta progetti di avvio...**. Selezionare il pulsante di opzione **progetti di avvio multipli** e impostare l' **azione** di entrambi i progetti su **Avvia**.</span><span class="sxs-lookup"><span data-stu-id="bec1e-179">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="bec1e-180">Fare clic con il pulsante destro del mouse su "Default.html" e selezionare **Imposta come pagina iniziale**.</span><span class="sxs-lookup"><span data-stu-id="bec1e-180">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="bec1e-181">Eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bec1e-181">Run the application.</span></span> <span data-ttu-id="bec1e-182">Viene avviato il server e la pagina.</span><span class="sxs-lookup"><span data-stu-id="bec1e-182">The server and page will launch.</span></span> <span data-ttu-id="bec1e-183">Potrebbe essere necessario ricaricare la pagina Web (oppure selezionare **continua** nel debugger) se la pagina viene caricata prima dell'avvio del server.</span><span class="sxs-lookup"><span data-stu-id="bec1e-183">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="bec1e-184">Nel browser fornire un nome utente quando richiesto.</span><span class="sxs-lookup"><span data-stu-id="bec1e-184">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="bec1e-185">Copiare l'URL della pagina in un'altra scheda o finestra del browser e specificare un nome utente diverso.</span><span class="sxs-lookup"><span data-stu-id="bec1e-185">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="bec1e-186">Sarà possibile inviare messaggi da un riquadro del browser all'altro, come nell'esercitazione Introduzione.</span><span class="sxs-lookup"><span data-stu-id="bec1e-186">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
