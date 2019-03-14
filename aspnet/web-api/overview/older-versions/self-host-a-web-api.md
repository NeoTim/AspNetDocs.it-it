---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Self-hosting di API Web ASP.NET 1 (c#) | Microsoft Docs
author: MikeWasson
description: API Web ASP.NET non sia necessario IIS. È possibile self-ospitare un'API web nel proprio processo host. Questa esercitazione illustra come ospitare un'API web all'interno di una console UT...
ms.author: riande
ms.date: 01/26/2012
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 63d192a6fa2aafef3770d5b0b97ec32e001b69db
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57040758"
---
<a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="9a37b-105">Self-hosting di API Web ASP.NET 1 (c#)</span><span class="sxs-lookup"><span data-stu-id="9a37b-105">Self-Host ASP.NET Web API 1 (C#)</span></span>
====================
<span data-ttu-id="9a37b-106">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9a37b-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="9a37b-107">API Web ASP.NET non sia necessario IIS.</span><span class="sxs-lookup"><span data-stu-id="9a37b-107">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="9a37b-108">È possibile self-ospitare un'API web nel proprio processo host.</span><span class="sxs-lookup"><span data-stu-id="9a37b-108">You can self-host a web API in your own host process.</span></span> <span data-ttu-id="9a37b-109">Questa esercitazione illustra come ospitare un'API web all'interno di un'applicazione console.</span><span class="sxs-lookup"><span data-stu-id="9a37b-109">This tutorial shows how to host a web API inside a console application.</span></span>
> 
> <span data-ttu-id="9a37b-110">**Le nuove applicazioni devono usare OWIN per l'hosting indipendente di API Web.**</span><span class="sxs-lookup"><span data-stu-id="9a37b-110">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="9a37b-111">Visualizzare [usare OWIN per l'hosting indipendente di API Web ASP.NET 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="9a37b-111">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9a37b-112">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="9a37b-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="9a37b-113">API Web 1</span><span class="sxs-lookup"><span data-stu-id="9a37b-113">Web API 1</span></span>
> - <span data-ttu-id="9a37b-114">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="9a37b-114">Visual Studio 2012</span></span>


## <a name="create-the-console-application-project"></a><span data-ttu-id="9a37b-115">Creare il progetto di applicazione Console</span><span class="sxs-lookup"><span data-stu-id="9a37b-115">Create the Console Application Project</span></span>

<span data-ttu-id="9a37b-116">Avviare Visual Studio e selezionare **nuovo progetto** dalle **avviare** pagina.</span><span class="sxs-lookup"><span data-stu-id="9a37b-116">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="9a37b-117">E viceversa, il **File** dal menu **New** e quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-117">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="9a37b-118">Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo.</span><span class="sxs-lookup"><span data-stu-id="9a37b-118">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="9a37b-119">Sotto **Visual c#**, selezionare **Windows**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-119">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="9a37b-120">Nell'elenco dei modelli di progetto, selezionare **applicazione Console**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-120">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="9a37b-121">Denominare il progetto &quot;SelfHost&quot; e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-121">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="9a37b-122">Impostare il Framework di destinazione (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="9a37b-122">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="9a37b-123">Se si usa Visual Studio 2010, modificare il framework di destinazione a .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="9a37b-123">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="9a37b-124">(Per impostazione predefinita, le destinazioni di modello di progetto di [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span><span class="sxs-lookup"><span data-stu-id="9a37b-124">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="9a37b-125">In Esplora soluzioni fare clic sul progetto e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-125">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="9a37b-126">Nel **framework di destinazione** elenco a discesa elenco, modificare il framework di destinazione a .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="9a37b-126">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="9a37b-127">Quando viene richiesto di applicare la modifica, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-127">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="9a37b-128">Installare Gestione pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="9a37b-128">Install NuGet Package Manager</span></span>

<span data-ttu-id="9a37b-129">Gestione pacchetti NuGet è il modo più semplice per aggiungere gli assembly di API Web a un progetto non ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9a37b-129">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="9a37b-130">Per verificare se è installato Gestione pacchetti NuGet, scegliere il **strumenti** menu di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9a37b-130">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="9a37b-131">Se viene visualizzato un menu chiamato **Gestisci pacchetti NuGet**, è necessario Gestione pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="9a37b-131">If you see a menu item called **NuGet Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="9a37b-132">Per installare Gestione pacchetti NuGet:</span><span class="sxs-lookup"><span data-stu-id="9a37b-132">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="9a37b-133">Avviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9a37b-133">Start Visual Studio.</span></span>
2. <span data-ttu-id="9a37b-134">Dal **degli strumenti** dal menu **estensioni e aggiornamenti**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-134">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="9a37b-135">Nel **estensioni e aggiornamenti** finestra di dialogo, seleziona **Online**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-135">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="9a37b-136">Se non viene visualizzato "Gestisci pacchetti NuGet", digitare "nuget package manager" nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="9a37b-136">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="9a37b-137">Selezionare Gestisci pacchetti NuGet e fare clic su **scaricare**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-137">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="9a37b-138">Al termine del download, verrà chiesto di installare.</span><span class="sxs-lookup"><span data-stu-id="9a37b-138">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="9a37b-139">Al termine dell'installazione, potrebbe essere necessario riavviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9a37b-139">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="9a37b-140">Aggiungere il pacchetto NuGet dell'API Web</span><span class="sxs-lookup"><span data-stu-id="9a37b-140">Add the Web API NuGet Package</span></span>

<span data-ttu-id="9a37b-141">Dopo aver installato Gestione pacchetti NuGet, aggiungere il pacchetto dell'API Web al progetto.</span><span class="sxs-lookup"><span data-stu-id="9a37b-141">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="9a37b-142">Dal **degli strumenti** dal menu **Gestione pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-142">From the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="9a37b-143">*Nota*: Se non possibile visualizzare il menu di elemento, assicurarsi che Gestione pacchetti NuGet è installato correttamente.</span><span class="sxs-lookup"><span data-stu-id="9a37b-143">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="9a37b-144">Selezionare **Gestisci pacchetti NuGet per la soluzione**</span><span class="sxs-lookup"><span data-stu-id="9a37b-144">Select **Manage NuGet Packages for Solution**</span></span>
3. <span data-ttu-id="9a37b-145">Nel **Gestisci pacchetti NugGet** finestra di dialogo, seleziona **Online**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-145">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="9a37b-146">Nella casella di ricerca, digitare &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="9a37b-146">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="9a37b-147">Selezionare il pacchetto di ASP.NET Web API Self Host e fare clic su **installare**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-147">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="9a37b-148">Dopo aver installato il pacchetto, fare clic su **chiudere** per chiudere la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="9a37b-148">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="9a37b-149">Assicurarsi di installare il pacchetto denominato Microsoft.AspNet.WebApi.SelfHost, non AspNetWebApi.SelfHost.</span><span class="sxs-lookup"><span data-stu-id="9a37b-149">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="9a37b-150">Creare il modello e Controller</span><span class="sxs-lookup"><span data-stu-id="9a37b-150">Create the Model and Controller</span></span>

<span data-ttu-id="9a37b-151">Questa esercitazione Usa le stesse classi del modello e controller come il [introduttiva](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="9a37b-151">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="9a37b-152">Aggiungere una classe pubblica denominata `Product`.</span><span class="sxs-lookup"><span data-stu-id="9a37b-152">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="9a37b-153">Aggiungere una classe pubblica denominata `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="9a37b-153">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="9a37b-154">Questa classe da derivare **System.Web.Http.ApiController**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-154">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="9a37b-155">Per altre informazioni sul codice in questo controller, vedere la [introduttiva](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="9a37b-155">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="9a37b-156">Questo controller definisce tre operazioni GET:</span><span class="sxs-lookup"><span data-stu-id="9a37b-156">This controller defines three GET actions:</span></span>

| <span data-ttu-id="9a37b-157">URI</span><span class="sxs-lookup"><span data-stu-id="9a37b-157">URI</span></span> | <span data-ttu-id="9a37b-158">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9a37b-158">Description</span></span> |
| --- | --- |
| <span data-ttu-id="9a37b-159">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="9a37b-159">/api/products</span></span> | <span data-ttu-id="9a37b-160">Ottenere un elenco di tutti i prodotti.</span><span class="sxs-lookup"><span data-stu-id="9a37b-160">Get a list of all products.</span></span> |
| <span data-ttu-id="9a37b-161">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="9a37b-161">/api/products/*id*</span></span> | <span data-ttu-id="9a37b-162">Ottenere un prodotto base all'ID.</span><span class="sxs-lookup"><span data-stu-id="9a37b-162">Get a product by ID.</span></span> |
| <span data-ttu-id="9a37b-163">/api/products/?category=*category*</span><span class="sxs-lookup"><span data-stu-id="9a37b-163">/api/products/?category=*category*</span></span> | <span data-ttu-id="9a37b-164">Ottenere un elenco di prodotti per categoria.</span><span class="sxs-lookup"><span data-stu-id="9a37b-164">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="9a37b-165">Ospitare l'API Web</span><span class="sxs-lookup"><span data-stu-id="9a37b-165">Host the Web API</span></span>

<span data-ttu-id="9a37b-166">Aprire il file Program.cs e aggiungere quanto segue usando istruzioni:</span><span class="sxs-lookup"><span data-stu-id="9a37b-166">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="9a37b-167">Aggiungere il codice seguente per il **programma** classe.</span><span class="sxs-lookup"><span data-stu-id="9a37b-167">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="9a37b-168">(Facoltativo) Aggiungere una prenotazione di Namespace URL HTTP</span><span class="sxs-lookup"><span data-stu-id="9a37b-168">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="9a37b-169">Questa applicazione ascolta `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="9a37b-169">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="9a37b-170">Per impostazione predefinita, in attesa in un particolare indirizzo HTTP richiede privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="9a37b-170">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="9a37b-171">Pertanto, quando si esegue l'esercitazione, si può visualizzare questo errore: "HTTP non è stato possibile registrare l'URL http://+:8080/" sono disponibili due modi per evitare questo errore:</span><span class="sxs-lookup"><span data-stu-id="9a37b-171">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="9a37b-172">Eseguire Visual Studio con autorizzazioni di amministratore con privilegi elevati, o</span><span class="sxs-lookup"><span data-stu-id="9a37b-172">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="9a37b-173">Usare Netsh.exe per concedere le autorizzazioni dell'account per riservare l'URL.</span><span class="sxs-lookup"><span data-stu-id="9a37b-173">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="9a37b-174">Per usare Netsh.exe, aprire un prompt dei comandi con privilegi di amministratore e immettere il comando seguente: comando seguente:</span><span class="sxs-lookup"><span data-stu-id="9a37b-174">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="9a37b-175">in cui *computer\nome utente* è l'account utente.</span><span class="sxs-lookup"><span data-stu-id="9a37b-175">where *machine\username* is your user account.</span></span>

<span data-ttu-id="9a37b-176">Al termine self-hosting, assicurarsi di eliminare la prenotazione:</span><span class="sxs-lookup"><span data-stu-id="9a37b-176">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="9a37b-177">Chiamare l'API Web da un'applicazione Client (c#)</span><span class="sxs-lookup"><span data-stu-id="9a37b-177">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="9a37b-178">È possibile scrivere una semplice applicazione console che chiama l'API web.</span><span class="sxs-lookup"><span data-stu-id="9a37b-178">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="9a37b-179">Aggiungere un nuovo progetto applicazione console alla soluzione:</span><span class="sxs-lookup"><span data-stu-id="9a37b-179">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="9a37b-180">In Esplora soluzioni fare doppio clic la soluzione e selezionare **Aggiungi nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-180">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="9a37b-181">Creare una nuova applicazione console denominata &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="9a37b-181">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="9a37b-182">Usare Gestione pacchetti NuGet per aggiungere il pacchetto di librerie di base di ASP.NET Web API:</span><span class="sxs-lookup"><span data-stu-id="9a37b-182">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="9a37b-183">Dal menu Strumenti, selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-183">From the Tools menu, select **NuGet Package Manager**.</span></span>
- <span data-ttu-id="9a37b-184">Selezionare **Gestisci pacchetti NuGet per la soluzione**</span><span class="sxs-lookup"><span data-stu-id="9a37b-184">Select **Manage NuGet Packages for Solution**</span></span>
- <span data-ttu-id="9a37b-185">Nel **Gestisci pacchetti NuGet** finestra di dialogo, seleziona **Online**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-185">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="9a37b-186">Nella casella di ricerca, digitare &quot;ASPNET&quot;.</span><span class="sxs-lookup"><span data-stu-id="9a37b-186">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="9a37b-187">Selezionare il pacchetto di librerie Client di Microsoft ASP.NET Web API e fare clic su **installare**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-187">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="9a37b-188">Aggiungere un riferimento in ClientApp al progetto SelfHost:</span><span class="sxs-lookup"><span data-stu-id="9a37b-188">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="9a37b-189">In Esplora soluzioni fare doppio clic su progetto ClientApp.</span><span class="sxs-lookup"><span data-stu-id="9a37b-189">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="9a37b-190">Selezionare **Aggiungi riferimento**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-190">Select **Add Reference**.</span></span>
- <span data-ttu-id="9a37b-191">Nel **gestione riferimenti** finestra di dialogo, sotto **soluzione**, selezionare **progetti**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-191">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="9a37b-192">Selezionare il progetto SelfHost.</span><span class="sxs-lookup"><span data-stu-id="9a37b-192">Select the SelfHost project.</span></span>
- <span data-ttu-id="9a37b-193">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-193">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="9a37b-194">Aprire il file Client/Program.cs.</span><span class="sxs-lookup"><span data-stu-id="9a37b-194">Open the Client/Program.cs file.</span></span> <span data-ttu-id="9a37b-195">Aggiungere il codice seguente **usando** istruzione:</span><span class="sxs-lookup"><span data-stu-id="9a37b-195">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="9a37b-196">Aggiungere un valore statico **HttpClient** istanza:</span><span class="sxs-lookup"><span data-stu-id="9a37b-196">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="9a37b-197">Aggiungere i metodi seguenti per elencare tutti i prodotti, elenco di ID di un prodotto ed elencano i prodotti per categoria.</span><span class="sxs-lookup"><span data-stu-id="9a37b-197">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="9a37b-198">Ognuno di questi metodi segue lo stesso modello:</span><span class="sxs-lookup"><span data-stu-id="9a37b-198">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="9a37b-199">Chiamare **HttpClient.GetAsync** per inviare una richiesta GET all'URI appropriato.</span><span class="sxs-lookup"><span data-stu-id="9a37b-199">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="9a37b-200">Chiamare **HttpResponseMessage.EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-200">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="9a37b-201">Questo metodo genera un'eccezione se lo stato della risposta HTTP è un codice di errore.</span><span class="sxs-lookup"><span data-stu-id="9a37b-201">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="9a37b-202">Chiamare **ReadAsAsync&lt;T&gt;**  per deserializzare un tipo CLR dalla risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9a37b-202">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="9a37b-203">Questo metodo è un metodo di estensione definito in **System.Net.Http.HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="9a37b-203">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="9a37b-204">Il **GetAsync** e **ReadAsAsync** metodi sono entrambi sono asincroni.</span><span class="sxs-lookup"><span data-stu-id="9a37b-204">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="9a37b-205">Restituiscono **attività** gli oggetti che rappresentano l'operazione asincrona.</span><span class="sxs-lookup"><span data-stu-id="9a37b-205">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="9a37b-206">Ottenere il **risultato** proprietà blocca il thread fino al completamento dell'operazione.</span><span class="sxs-lookup"><span data-stu-id="9a37b-206">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="9a37b-207">Per altre informazioni sull'uso di HttpClient, incluse le procedure effettuare chiamate senza blocchi, vedere [chiamata di una Web API da un Client .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="9a37b-207">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="9a37b-208">Prima di chiamare questi metodi, impostare la proprietà BaseAddress sull'istanza di HttpClient per "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="9a37b-208">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="9a37b-209">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9a37b-209">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="9a37b-210">Questa deve restituire il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="9a37b-210">This should output the following.</span></span> <span data-ttu-id="9a37b-211">(Ricordarsi di eseguire l'applicazione SelfHost prima).</span><span class="sxs-lookup"><span data-stu-id="9a37b-211">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
