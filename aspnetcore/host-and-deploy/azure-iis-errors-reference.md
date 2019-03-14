---
title: Errori comuni di Servizio app di Azure e IIS con ASP.NET Core
author: guardrex
description: Ottenere suggerimenti per la risoluzione degli errori comuni dell'hosting di app ASP.NET Core in Servizio app di Azure e IIS.
ms.author: riande
ms.custom: mvc
ms.date: 02/21/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: d1cdac4d27ee1bc3ebb4329c1bbd3bdacb34a58c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050268"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="07059-103">Errori comuni di Servizio app di Azure e IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07059-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="07059-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="07059-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="07059-105">Questo argomento include suggerimenti per la risoluzione degli errori comuni dell'hosting di app ASP.NET Core in Servizio app di Azure e IIS.</span><span class="sxs-lookup"><span data-stu-id="07059-105">This topic offers troubleshooting advice for common errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="07059-106">Raccogliere le seguenti informazioni:</span><span class="sxs-lookup"><span data-stu-id="07059-106">Collect the following information:</span></span>

* <span data-ttu-id="07059-107">Comportamento del browser (codice di stato e messaggio di errore)</span><span class="sxs-lookup"><span data-stu-id="07059-107">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="07059-108">Voci del log eventi dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="07059-108">Application Event Log entries</span></span>
  * <span data-ttu-id="07059-109">Servizio app di Azure &ndash; Vedere <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="07059-109">Azure App Service &ndash; See <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
  * <span data-ttu-id="07059-110">IIS</span><span class="sxs-lookup"><span data-stu-id="07059-110">IIS</span></span>
    1. <span data-ttu-id="07059-111">Selezionare **Start** nel menu di **Windows**, digitare *Visualizzatore eventi* e premere **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="07059-111">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="07059-112">Dopo l'apertura di **Visualizzatore eventi**, espandere **Registri di Windows** > **Applicazione** nella barra laterale.</span><span class="sxs-lookup"><span data-stu-id="07059-112">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="07059-113">Voci del log per debug e stdout di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07059-113">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="07059-114">Servizio app di Azure &ndash; Vedere <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="07059-114">Azure App Service &ndash; See <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
  * <span data-ttu-id="07059-115">IIS &ndash; Seguire le istruzioni nelle sezioni [Creazione e reindirizzamento dei log](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) e [Log di diagnostica avanzati](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) dell'argomento Modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="07059-115">IIS &ndash; Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="07059-116">Confrontare le informazioni sugli errori con gli errori comuni seguenti.</span><span class="sxs-lookup"><span data-stu-id="07059-116">Compare error information to the following common errors.</span></span> <span data-ttu-id="07059-117">Se viene trovata una corrispondenza, seguire le indicazioni per la risoluzione dei problemi.</span><span class="sxs-lookup"><span data-stu-id="07059-117">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="07059-118">L'elenco degli errori in questo argomento non è esaustivo.</span><span class="sxs-lookup"><span data-stu-id="07059-118">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="07059-119">Se si verifica un errore non elencato qui, aprire un nuovo problema tramite il pulsante per l'invio di **feedback per il contenuto** nella parte inferiore di questo argomento con istruzioni dettagliate su come riprodurre l'errore.</span><span class="sxs-lookup"><span data-stu-id="07059-119">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="07059-120">Il programma di installazione non riesce ad ottenere VC++ Ridistribuibile</span><span class="sxs-lookup"><span data-stu-id="07059-120">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="07059-121">**Eccezione del programma di installazione:** 0x80072efd **--oppure--** 0x80072f76 - Errore non specificato</span><span class="sxs-lookup"><span data-stu-id="07059-121">**Installer Exception:** 0x80072efd **--OR--** 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="07059-122">**Eccezione nel log del programma di installazione&#8224;:** Errore 0x80072efd **--oppure--** 0x80072f76: Impossibile eseguire il pacchetto EXE</span><span class="sxs-lookup"><span data-stu-id="07059-122">**Installer Log Exception&#8224;:** Error 0x80072efd **--OR--** 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="07059-123">&#8224;Il log è disponibile in *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span><span class="sxs-lookup"><span data-stu-id="07059-123">&#8224;The log is located at *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span></span>

<span data-ttu-id="07059-124">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="07059-124">Troubleshooting:</span></span>

<span data-ttu-id="07059-125">Se il sistema non ha accesso a Internet durante l'[installazione del bundle di hosting di .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), questa eccezione si verifica quando il programma di installazione non riesce a ottenere *Microsoft Visual C++ 2015 Redistributable*.</span><span class="sxs-lookup"><span data-stu-id="07059-125">If the system doesn't have Internet access while [installing the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="07059-126">Ottenere un programma di installazione dall'[Area download Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="07059-126">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="07059-127">Se l'esecuzione del programma di installazione non riesce, il server potrebbe non ricevere il runtime .NET Core necessario per ospitare una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="07059-127">If the installer fails, the server may not receive the .NET Core runtime required to host a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span> <span data-ttu-id="07059-128">Se si ospita una distribuzione dipendente dal framework, verificare che il runtime sia installato in **Programmi e funzionalità** o in **App e funzionalità**.</span><span class="sxs-lookup"><span data-stu-id="07059-128">If hosting an FDD, confirm that the runtime is installed in **Programs & Features** or **Apps & features**.</span></span> <span data-ttu-id="07059-129">Se è necessario un runtime specifico, scaricare il runtime dagli [archivi di download .NET](https://dotnet.microsoft.com/download/archives) e installarlo nel sistema.</span><span class="sxs-lookup"><span data-stu-id="07059-129">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="07059-130">Dopo aver installato il runtime, riavviare il sistema o riavviare IIS eseguendo **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="07059-130">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="07059-131">L'aggiornamento del sistema operativo ha rimosso il modulo di ASP.NET Core a 32 bit</span><span class="sxs-lookup"><span data-stu-id="07059-131">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="07059-132">**Registro dell'applicazione:** Impossibile caricare la DLL del modulo **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**.</span><span class="sxs-lookup"><span data-stu-id="07059-132">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="07059-133">L'errore è nei dati.</span><span class="sxs-lookup"><span data-stu-id="07059-133">The data is the error.</span></span>

<span data-ttu-id="07059-134">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="07059-134">Troubleshooting:</span></span>

<span data-ttu-id="07059-135">I file non appartenenti al sistema operativo presenti nella directory **C:\Windows\SysWOW64\inetsrv** non vengono mantenuti durante un aggiornamento del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="07059-135">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="07059-136">Se il modulo ASP.NET Core viene installato prima di un aggiornamento del sistema operativo e quindi si esegue qualsiasi pool di app in modalità a 32 bit dopo l'aggiornamento, si verifica questo problema.</span><span class="sxs-lookup"><span data-stu-id="07059-136">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="07059-137">Dopo un aggiornamento del sistema operativo, ripristinare il Modulo di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="07059-137">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="07059-138">Vedere [Installare il bundle di hosting .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="07059-138">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="07059-139">Selezionare **Ripara** quando viene eseguito il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="07059-139">Select **Repair** when the installer is run.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="07059-140">Un'app x86 viene distribuita, ma il pool di app non è abilitato per le app a 32 bit</span><span class="sxs-lookup"><span data-stu-id="07059-140">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="07059-141">**Browser:** Errore HTTP 500.30 - Errore di avvio In-Process ANCM</span><span class="sxs-lookup"><span data-stu-id="07059-141">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="07059-142">**Registro dell'applicazione:** Eccezione gestita imprevista per l'applicazione '/LM/W3SVC/5/ROOT' con radice fisica '{PATH}' Codice eccezione = '0xe0434352'.</span><span class="sxs-lookup"><span data-stu-id="07059-142">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="07059-143">Controllare i log stderr per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="07059-143">Please check the stderr logs for more information.</span></span> <span data-ttu-id="07059-144">Applicazione '/LM/W3SVC/5/ROOT' con radice fisica '{PATH}'. Impossibile caricare clr e applicazione gestita.</span><span class="sxs-lookup"><span data-stu-id="07059-144">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="07059-145">Chiusura prematura del thread di lavoro CLR</span><span class="sxs-lookup"><span data-stu-id="07059-145">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="07059-146">**Log stdout del modulo ASP.NET Core:** il file di log viene creato ma vuoto.</span><span class="sxs-lookup"><span data-stu-id="07059-146">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="07059-147">**Log di debug del modulo ASP.NET Core:** Restituito HRESULT non riuscito: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="07059-147">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

<span data-ttu-id="07059-148">Questo scenario viene intercettato dall'SDK durante la pubblicazione di un'app autonoma.</span><span class="sxs-lookup"><span data-stu-id="07059-148">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="07059-149">L'SDK genera un errore se il RID non corrisponde alla piattaforma di destinazione (ad esempio, RID `win10-x64` con `<PlatformTarget>x86</PlatformTarget>` nel file di progetto).</span><span class="sxs-lookup"><span data-stu-id="07059-149">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="07059-150">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="07059-150">Troubleshooting:</span></span>

<span data-ttu-id="07059-151">Per una distribuzione dipendente dal framework x86 (`<PlatformTarget>x86</PlatformTarget>`), abilitare il pool di app IIS per le app a 32 bit.</span><span class="sxs-lookup"><span data-stu-id="07059-151">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="07059-152">In Gestione IIS aprire **Impostazioni avanzate** per il pool di app e impostare **Attiva applicazioni a 32 bit** su **True**.</span><span class="sxs-lookup"><span data-stu-id="07059-152">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="07059-153">La piattaforma è in conflitto con RID</span><span class="sxs-lookup"><span data-stu-id="07059-153">Platform conflicts with RID</span></span>

* <span data-ttu-id="07059-154">**Browser:** errore HTTP 502.5 - Errore del processo</span><span class="sxs-lookup"><span data-stu-id="07059-154">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="07059-155">**Registro dell'applicazione:** L'applicazione 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' con radice fisica 'C:\{PATH}\' non è riuscita ad avviare il processo con la riga di comando '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', Codice errore = '0x80004005 : ff.</span><span class="sxs-lookup"><span data-stu-id="07059-155">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="07059-156">**Log stdout del modulo ASP.NET Core:** Eccezione non gestita: System.BadImageFormatException: Impossibile caricare il file o l'assembly '{ASSEMBLY}.dll'.</span><span class="sxs-lookup"><span data-stu-id="07059-156">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="07059-157">Tentativo di caricare un programma con un formato non corretto.</span><span class="sxs-lookup"><span data-stu-id="07059-157">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="07059-158">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="07059-158">Troubleshooting:</span></span>

* <span data-ttu-id="07059-159">Confermare che l'app sia eseguita in locale su Kestrel.</span><span class="sxs-lookup"><span data-stu-id="07059-159">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="07059-160">Un errore del processo può essere causato da un problema interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="07059-160">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="07059-161">Per altre informazioni, vedere [Risolvere i problemi di ASP.NET Core in IIS](xref:host-and-deploy/iis/troubleshoot) oppure [Risolvere i problemi di ASP.NET Core in Servizio app di Azure](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="07059-161">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="07059-162">Se questa eccezione si verifica per una distribuzione di App di Azure durante l'aggiornamento di un'app e la distribuzione di assembly più recenti, eliminare manualmente tutti i file dalla distribuzione precedente.</span><span class="sxs-lookup"><span data-stu-id="07059-162">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="07059-163">Le assembly incompatibili con il tempo di ritardo possono causare una eccezione `System.BadImageFormatException` quando si distribuisce un'app aggiornata.</span><span class="sxs-lookup"><span data-stu-id="07059-163">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="07059-164">Endpoint dell'URI non corretto o sito web arrestato</span><span class="sxs-lookup"><span data-stu-id="07059-164">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="07059-165">**Browser:** ERR_CONNECTION_REFUSED **--oppure--** La connessione non è riuscita</span><span class="sxs-lookup"><span data-stu-id="07059-165">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="07059-166">**Registro dell'applicazione:** nessuna voce</span><span class="sxs-lookup"><span data-stu-id="07059-166">**Application Log:** No entry</span></span>

* <span data-ttu-id="07059-167">**Log stdout del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="07059-167">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="07059-168">**Log di debug del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="07059-168">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="07059-169">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="07059-169">Troubleshooting:</span></span>

* <span data-ttu-id="07059-170">Verificare che sia in uso l'endpoint URI corretto per l'app.</span><span class="sxs-lookup"><span data-stu-id="07059-170">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="07059-171">Controllare i binding.</span><span class="sxs-lookup"><span data-stu-id="07059-171">Check the bindings.</span></span>

* <span data-ttu-id="07059-172">Verificare che il sito Web IIS non sia nello stato *In arresto*.</span><span class="sxs-lookup"><span data-stu-id="07059-172">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="07059-173">Le funzionalità del server CoreWebEngine o W3SVC sono disabilitate</span><span class="sxs-lookup"><span data-stu-id="07059-173">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="07059-174">**Eccezione del sistema operativo:** per usare il modulo ASP.NET Core è necessario installare le funzionalità di IIS 7.0 CoreWebEngine e W3SVC.</span><span class="sxs-lookup"><span data-stu-id="07059-174">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="07059-175">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="07059-175">Troubleshooting:</span></span>

<span data-ttu-id="07059-176">Verificare che il ruolo e le funzionalità appropriati siano abilitati.</span><span class="sxs-lookup"><span data-stu-id="07059-176">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="07059-177">Vedere [Configurazione di IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="07059-177">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="07059-178">Percorso fisico del sito Web non corretto o app mancante</span><span class="sxs-lookup"><span data-stu-id="07059-178">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="07059-179">**Browser:** 403 Non consentito: accesso negato **--oppure--** 403.14 Non consentito - Il server Web è configurato per non consentire la visualizzazione del contenuto dalla directory.</span><span class="sxs-lookup"><span data-stu-id="07059-179">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="07059-180">**Registro dell'applicazione:** nessuna voce</span><span class="sxs-lookup"><span data-stu-id="07059-180">**Application Log:** No entry</span></span>

* <span data-ttu-id="07059-181">**Log stdout del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="07059-181">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="07059-182">**Log di debug del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="07059-182">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="07059-183">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="07059-183">Troubleshooting:</span></span>

<span data-ttu-id="07059-184">Controllare il sito Web IIS **Impostazioni di base** e la cartella dell'app fisica.</span><span class="sxs-lookup"><span data-stu-id="07059-184">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="07059-185">Verificare che l'app si trovi nella cartella nel sito Web IIS **Percorso fisico**.</span><span class="sxs-lookup"><span data-stu-id="07059-185">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="07059-186">Ruolo non corretto, modulo ASP.NET Core non installato o autorizzazioni non corrette</span><span class="sxs-lookup"><span data-stu-id="07059-186">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="07059-187">**Browser:** 500.19 Errore interno al server - Non è possibile accedere alla pagina richiesta in quanto i dati di configurazione correlati alla pagina non sono validi.</span><span class="sxs-lookup"><span data-stu-id="07059-187">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="07059-188">**--Oppure--** Non è possibile visualizzare questa pagina</span><span class="sxs-lookup"><span data-stu-id="07059-188">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="07059-189">**Registro dell'applicazione:** nessuna voce</span><span class="sxs-lookup"><span data-stu-id="07059-189">**Application Log:** No entry</span></span>

* <span data-ttu-id="07059-190">**Log stdout del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="07059-190">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="07059-191">**Log di debug del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="07059-191">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="07059-192">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="07059-192">Troubleshooting:</span></span>

* <span data-ttu-id="07059-193">Verificare che il ruolo appropriato sia abilitato.</span><span class="sxs-lookup"><span data-stu-id="07059-193">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="07059-194">Vedere [Configurazione di IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="07059-194">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="07059-195">Aprire **Programmi e funzionalità** oppure **App e funzionalità** e verificare che sia installato **Windows Server Hosting**.</span><span class="sxs-lookup"><span data-stu-id="07059-195">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="07059-196">Se **Windows Server Hosting** non è presente nell'elenco dei programmi installati, scaricare e installare il bundle di hosting .NET Core.</span><span class="sxs-lookup"><span data-stu-id="07059-196">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="07059-197">Programma di installazione del bundle di hosting .NET Core corrente (download diretto)</span><span class="sxs-lookup"><span data-stu-id="07059-197">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="07059-198">Per altre informazioni, vedere [Installare il bundle di hosting .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="07059-198">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="07059-199">Assicurarsi che **Pool di applicazioni** > **Modello di processo** > **Identità** sia impostato su **ApplicationPoolIdentity** o che l'identità personalizzata disponga delle autorizzazioni appropriate per accedere alla cartella di distribuzione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="07059-199">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="07059-200">Se il bundle di hosting ASP.NET Core è stato disinstallato e quindi è stata installata una versione del bundle di hosting precedente, il file *applicationHost.config* non include una sezione per il modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="07059-200">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="07059-201">Aprire *applicationHost.config* in *%windir%/System32/inetsrv/config* e trovare il gruppo di sezioni `<configuration><configSections><sectionGroup name="system.webServer">`.</span><span class="sxs-lookup"><span data-stu-id="07059-201">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="07059-202">Se la sezione per il modulo ASP.NET Core non è presente nel gruppo di sezioni, aggiungere l'elemento della sezione:</span><span class="sxs-lookup"><span data-stu-id="07059-202">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```
  
  <span data-ttu-id="07059-203">In alternativa installare la versione più recente del bundle di hosting ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="07059-203">Alternatively, install the lastest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="07059-204">La versione più recente è compatibile con le app ASP.NET Core supportate.</span><span class="sxs-lookup"><span data-stu-id="07059-204">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="07059-205">ProcessPath non corretto, variabile di percorso mancante, aggregazione di hosting non installata, sistema/IIS non riavviato, VC Redistributable non installato o violazione dell'accesso a dotnet.exe</span><span class="sxs-lookup"><span data-stu-id="07059-205">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="07059-206">**Browser:** Errore HTTP 500.0 - Errore di caricamento gestore In-Process ANCM</span><span class="sxs-lookup"><span data-stu-id="07059-206">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="07059-207">**Registro dell'applicazione:** L'applicazione 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' con radice fisica 'C:\{PATH}\' non è riuscita ad avviare il processo con la riga di comando "{...}"</span><span class="sxs-lookup"><span data-stu-id="07059-207">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="07059-208">', ErrorCode = '0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="07059-208">', ErrorCode = '0x80070002 : 0.</span></span> <span data-ttu-id="07059-209">Impossibile avviare l'applicazione '{PATH}'.</span><span class="sxs-lookup"><span data-stu-id="07059-209">Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="07059-210">Eseguibile non trovato in '{PATH}'.</span><span class="sxs-lookup"><span data-stu-id="07059-210">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="07059-211">Impossibile avviare l'applicazione '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span><span class="sxs-lookup"><span data-stu-id="07059-211">Failed to start application '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span></span>

* <span data-ttu-id="07059-212">**Log stdout del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="07059-212">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="07059-213">**Log di debug del modulo ASP.NET Core:** Registro eventi: 'Avvio dell'applicazione '{PATH}' non riuscito.</span><span class="sxs-lookup"><span data-stu-id="07059-213">**ASP.NET Core Module Debug Log:** Event Log: 'Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="07059-214">Eseguibile non trovato in '{PATH}'.</span><span class="sxs-lookup"><span data-stu-id="07059-214">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="07059-215">Restituito HRESULT non riuscito: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="07059-215">Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="07059-216">**Browser:** errore HTTP 502.5 - Errore del processo</span><span class="sxs-lookup"><span data-stu-id="07059-216">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="07059-217">**Registro dell'applicazione:** L'applicazione 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' con radice fisica 'C:\{PATH}\' non è riuscita ad avviare il processo con la riga di comando "{...}"</span><span class="sxs-lookup"><span data-stu-id="07059-217">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="07059-218">', ErrorCode = '0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="07059-218">', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="07059-219">**Log stdout del modulo ASP.NET Core:** il file di log viene creato ma vuoto.</span><span class="sxs-lookup"><span data-stu-id="07059-219">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="07059-220">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="07059-220">Troubleshooting:</span></span>

* <span data-ttu-id="07059-221">Confermare che l'app sia eseguita in locale su Kestrel.</span><span class="sxs-lookup"><span data-stu-id="07059-221">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="07059-222">Un errore del processo può essere causato da un problema interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="07059-222">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="07059-223">Per altre informazioni, vedere [Risolvere i problemi di ASP.NET Core in IIS](xref:host-and-deploy/iis/troubleshoot) oppure [Risolvere i problemi di ASP.NET Core in Servizio app di Azure](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="07059-223">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="07059-224">Verificare che l'attributo *processPath* dell'elemento `<aspNetCore>` in *web.config* sia `dotnet` per una distribuzione dipendente da framework (FDD, Framework-Dependent Deployment) o `.\{ASSEMBLY}.exe` per una [distribuzione autonoma (SCD, Self-Contained Deployment)](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="07059-224">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="07059-225">Per una distribuzione dipendente dal framework, *dotnet.exe* potrebbe non essere accessibile tramite le impostazioni del percorso.</span><span class="sxs-lookup"><span data-stu-id="07059-225">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="07059-226">Confermare che *C:\Programmi\dotnet\\* esiste nelle impostazioni PATH di sistema.</span><span class="sxs-lookup"><span data-stu-id="07059-226">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="07059-227">Per una distribuzione dipendente da framework, *dotnet.exe* potrebbe non essere accessibile per l'identità utente del pool di app.</span><span class="sxs-lookup"><span data-stu-id="07059-227">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="07059-228">Confermare che l'identità dell'utente del pool di app abbia accesso alla directory *C:\Programmi\dotnet*.</span><span class="sxs-lookup"><span data-stu-id="07059-228">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="07059-229">Verificare che non siano presenti regole di negazione configurate per l'identità dell'utente del pool di app in *C:\Programmi\dotnet* e nelle directory dell'app.</span><span class="sxs-lookup"><span data-stu-id="07059-229">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="07059-230">È possibile che sia stata eseguita una distribuzione FDD e che sia stato installato .NET Core senza riavviare IIS.</span><span class="sxs-lookup"><span data-stu-id="07059-230">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="07059-231">Riavviare il server o riavviare IIS eseguendo **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="07059-231">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="07059-232">È possibile che sia stata eseguita una distribuzione FDD senza installare il runtime .NET Core nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="07059-232">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="07059-233">Se il runtime .NET Core non è stato installato, eseguire il **programma di installazione del bundle di hosting .NET Core** nel sistema.</span><span class="sxs-lookup"><span data-stu-id="07059-233">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="07059-234">Programma di installazione del bundle di hosting .NET Core corrente (download diretto)</span><span class="sxs-lookup"><span data-stu-id="07059-234">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="07059-235">Per altre informazioni, vedere [Installare il bundle di hosting .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="07059-235">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="07059-236">Se è necessario un runtime specifico, scaricare il runtime dagli [archivi di download .NET](https://dotnet.microsoft.com/download/archives) e installarlo nel sistema.</span><span class="sxs-lookup"><span data-stu-id="07059-236">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="07059-237">Completare l'installazione riavviando il sistema o riavviando IIS eseguendo **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="07059-237">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="07059-238">È possibile che sia stata eseguita una distribuzione FDD e che *Microsoft Visual C++ 2015 Redistributable (x64)* non sia installato nel sistema.</span><span class="sxs-lookup"><span data-stu-id="07059-238">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="07059-239">Ottenere un programma di installazione dall'[Area download Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="07059-239">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="07059-240">Argomenti non corretti dell'elemento \<aspNetCore></span><span class="sxs-lookup"><span data-stu-id="07059-240">Incorrect arguments of \<aspNetCore> element</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="07059-241">**Browser:** Errore HTTP 500.0 - Errore di caricamento gestore In-Process ANCM</span><span class="sxs-lookup"><span data-stu-id="07059-241">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="07059-242">**Registro dell'applicazione:** Chiamata di hostfxr per trovare il gestore delle richieste In-Process non riuscita senza trovare dipendenze native.</span><span class="sxs-lookup"><span data-stu-id="07059-242">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="07059-243">Questo errore probabilmente significa che l'app non è configurata correttamente. Controllare le versioni di Microsoft.NetCore.App e Microsoft.AspNetCore.App specificate come destinazione dall'applicazione e installate nel computer.</span><span class="sxs-lookup"><span data-stu-id="07059-243">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="07059-244">Non è stato possibile trovare il gestore delle richieste In-Process.</span><span class="sxs-lookup"><span data-stu-id="07059-244">Could not find inprocess request handler.</span></span> <span data-ttu-id="07059-245">Output acquisito dalla chiamata di hostfxr: Si intendeva eseguire comandi di dotnet SDK?</span><span class="sxs-lookup"><span data-stu-id="07059-245">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="07059-246">Installare dotnet SDK da: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Impossibile avviare l'applicazione '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span><span class="sxs-lookup"><span data-stu-id="07059-246">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed to start application '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="07059-247">**Log stdout del modulo ASP.NET Core:** Si intendeva eseguire comandi di dotnet SDK?</span><span class="sxs-lookup"><span data-stu-id="07059-247">**ASP.NET Core Module stdout Log:** Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="07059-248">Installare dotnet SDK da: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="07059-248">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span></span>

* <span data-ttu-id="07059-249">**Log di debug del modulo ASP.NET Core:** Chiamata di hostfxr per trovare il gestore delle richieste In-Process non riuscita senza trovare dipendenze native.</span><span class="sxs-lookup"><span data-stu-id="07059-249">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="07059-250">Questo errore probabilmente significa che l'app non è configurata correttamente. Controllare le versioni di Microsoft.NetCore.App e Microsoft.AspNetCore.App specificate come destinazione dall'applicazione e installate nel computer.</span><span class="sxs-lookup"><span data-stu-id="07059-250">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="07059-251">Restituito HRESULT non riuscito: 0x8000ffff Non è stato possibile trovare il gestore delle richieste In-Process.</span><span class="sxs-lookup"><span data-stu-id="07059-251">Failed HRESULT returned: 0x8000ffff Could not find inprocess request handler.</span></span> <span data-ttu-id="07059-252">Output acquisito dalla chiamata di hostfxr: Si intendeva eseguire comandi di dotnet SDK?</span><span class="sxs-lookup"><span data-stu-id="07059-252">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="07059-253">Installare dotnet SDK da: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Restituito HRESULT non riuscito: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="07059-253">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="07059-254">**Browser:** errore HTTP 502.5 - Errore del processo</span><span class="sxs-lookup"><span data-stu-id="07059-254">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="07059-255">**Registro dell'applicazione:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span><span class="sxs-lookup"><span data-stu-id="07059-255">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="07059-256">**Log stdout del modulo ASP.NET Core:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span><span class="sxs-lookup"><span data-stu-id="07059-256">**ASP.NET Core Module stdout Log:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span></span>

::: moniker-end

<span data-ttu-id="07059-257">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="07059-257">Troubleshooting:</span></span>

* <span data-ttu-id="07059-258">Confermare che l'app sia eseguita in locale su Kestrel.</span><span class="sxs-lookup"><span data-stu-id="07059-258">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="07059-259">Un errore del processo può essere causato da un problema interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="07059-259">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="07059-260">Per altre informazioni, vedere [Risolvere i problemi di ASP.NET Core in IIS](xref:host-and-deploy/iis/troubleshoot) oppure [Risolvere i problemi di ASP.NET Core in Servizio app di Azure](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="07059-260">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="07059-261">Verificare che l'attributo *arguments* dell'elemento `<aspNetCore>` in *web.config* sia (a) `.\{ASSEMBLY}.dll` per una distribuzione dipendente da framework (FDD) o (b) non disponibile, una stringa vuota (`arguments=""`) o un elenco degli argomenti dell'app (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) per una distribuzione autonoma (SCD).</span><span class="sxs-lookup"><span data-stu-id="07059-261">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a><span data-ttu-id="07059-262">Framework condiviso di .NET Core mancante</span><span class="sxs-lookup"><span data-stu-id="07059-262">Missing .NET Core shared framework</span></span>

* <span data-ttu-id="07059-263">**Browser:** Errore HTTP 500.0 - Errore di caricamento gestore In-Process ANCM</span><span class="sxs-lookup"><span data-stu-id="07059-263">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="07059-264">**Registro dell'applicazione:** Chiamata di hostfxr per trovare il gestore delle richieste In-Process non riuscita senza trovare dipendenze native.</span><span class="sxs-lookup"><span data-stu-id="07059-264">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="07059-265">Questo errore probabilmente significa che l'app non è configurata correttamente. Controllare le versioni di Microsoft.NetCore.App e Microsoft.AspNetCore.App specificate come destinazione dall'applicazione e installate nel computer.</span><span class="sxs-lookup"><span data-stu-id="07059-265">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="07059-266">Non è stato possibile trovare il gestore delle richieste In-Process.</span><span class="sxs-lookup"><span data-stu-id="07059-266">Could not find inprocess request handler.</span></span> <span data-ttu-id="07059-267">Output acquisito dalla chiamata di hostfxr: Non è stato possibile trovare qualsiasi versione del framework compatibile.</span><span class="sxs-lookup"><span data-stu-id="07059-267">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="07059-268">Impossibile trovare la versione del framework 'Microsoft.AspNetCore.App' specificata '{VERSION}'.</span><span class="sxs-lookup"><span data-stu-id="07059-268">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

<span data-ttu-id="07059-269">Impossibile avviare l'applicazione '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span><span class="sxs-lookup"><span data-stu-id="07059-269">Failed to start application '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="07059-270">**Log stdout del modulo ASP.NET Core:** Non è stato possibile trovare qualsiasi versione del framework compatibile.</span><span class="sxs-lookup"><span data-stu-id="07059-270">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="07059-271">Impossibile trovare la versione del framework 'Microsoft.AspNetCore.App' specificata '{VERSION}'.</span><span class="sxs-lookup"><span data-stu-id="07059-271">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

* <span data-ttu-id="07059-272">**Log di debug del modulo ASP.NET Core:** Restituito HRESULT non riuscito: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="07059-272">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

<span data-ttu-id="07059-273">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="07059-273">Troubleshooting:</span></span>

<span data-ttu-id="07059-274">Per una distribuzione dipendente da framework (FDD), verificare che nel sistema sia installato il runtime corretto.</span><span class="sxs-lookup"><span data-stu-id="07059-274">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="07059-275">Pool di applicazioni arrestato</span><span class="sxs-lookup"><span data-stu-id="07059-275">Stopped Application Pool</span></span>

* <span data-ttu-id="07059-276">**Browser:** 503 Servizio non disponibile</span><span class="sxs-lookup"><span data-stu-id="07059-276">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="07059-277">**Registro dell'applicazione:** nessuna voce</span><span class="sxs-lookup"><span data-stu-id="07059-277">**Application Log:** No entry</span></span>

* <span data-ttu-id="07059-278">**Log stdout del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="07059-278">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="07059-279">**Log di debug del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="07059-279">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="07059-280">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="07059-280">Troubleshooting:</span></span>

<span data-ttu-id="07059-281">Confermare che il pool di applicazioni non sia nello stato *Arrestato*.</span><span class="sxs-lookup"><span data-stu-id="07059-281">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="07059-282">L'app secondaria include una sezione \<handlers></span><span class="sxs-lookup"><span data-stu-id="07059-282">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="07059-283">**Browser:** errore HTTP 500.19 - Errore del server interno</span><span class="sxs-lookup"><span data-stu-id="07059-283">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="07059-284">**Registro dell'applicazione:** nessuna voce</span><span class="sxs-lookup"><span data-stu-id="07059-284">**Application Log:** No entry</span></span>

* <span data-ttu-id="07059-285">**Log stdout del modulo ASP.NET Core:** il file di log dell'app radice viene creato e mostra un funzionamento normale.</span><span class="sxs-lookup"><span data-stu-id="07059-285">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="07059-286">Il file di log dell'app secondaria non viene creato.</span><span class="sxs-lookup"><span data-stu-id="07059-286">The sub-app's log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="07059-287">**Log di debug del modulo ASP.NET Core:** il file di log dell'app radice viene creato e mostra un funzionamento normale.</span><span class="sxs-lookup"><span data-stu-id="07059-287">**ASP.NET Core Module Debug Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="07059-288">Il file di log dell'app secondaria non viene creato.</span><span class="sxs-lookup"><span data-stu-id="07059-288">The sub-app's log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="07059-289">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="07059-289">Troubleshooting:</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="07059-290">Verificare che il file *web.config* dell'app secondaria non includa una sezione `<handlers>` o che l'app secondaria non erediti i gestori dell'app padre.</span><span class="sxs-lookup"><span data-stu-id="07059-290">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section or that the sub-app doesn't inherit the parent app's handlers.</span></span>

<span data-ttu-id="07059-291">La sezione `<system.webServer>` dell'app padre di *web.config* viene inserita all'interno di un elemento `<location>`.</span><span class="sxs-lookup"><span data-stu-id="07059-291">The parent app's `<system.webServer>` section of *web.config* is placed inside of a `<location>` element.</span></span> <span data-ttu-id="07059-292">La proprietà <xref:System.Configuration.SectionInformation.InheritInChildApplications*> è impostata su `false` per indicare che le impostazioni specificate nell'elemento [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) non sono ereditate da app che risiedono in una sottodirectory dell'app padre.</span><span class="sxs-lookup"><span data-stu-id="07059-292">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the parent app.</span></span> <span data-ttu-id="07059-293">Per altre informazioni, vedere <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="07059-293">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="07059-294">Verificare che il file *web.config* della sotto-applicazione non includa una sezione `<handlers>`.</span><span class="sxs-lookup"><span data-stu-id="07059-294">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

::: moniker-end

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="07059-295">percorso errato del log stdout</span><span class="sxs-lookup"><span data-stu-id="07059-295">stdout log path incorrect</span></span>

* <span data-ttu-id="07059-296">**Browser:** l'app risponde normalmente.</span><span class="sxs-lookup"><span data-stu-id="07059-296">**Browser:** The app responds normally.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="07059-297">**Registro dell'applicazione:** non è possibile avviare il reindirizzamento di stdout in C:\Programmi\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="07059-297">**Application Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="07059-298">Messaggio di eccezione: HRESULT 0x80070005 restituito in {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span><span class="sxs-lookup"><span data-stu-id="07059-298">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="07059-299">Non è possibile arrestare il reindirizzamento di stdout in C:\Programmi\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="07059-299">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="07059-300">Messaggio di eccezione: HRESULT 0x80070002 restituito in {PATH}.</span><span class="sxs-lookup"><span data-stu-id="07059-300">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="07059-301">Non è possibile avviare il reindirizzamento di stdout in {PATH}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="07059-301">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

* <span data-ttu-id="07059-302">**Log stdout del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="07059-302">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="07059-303">**Log di debug del modulo ASP.NET Core:** non è possibile avviare il reindirizzamento di stdout in C:\Programmi\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="07059-303">**ASP.NET Core Module debug Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="07059-304">Messaggio di eccezione: HRESULT 0x80070005 restituito in {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span><span class="sxs-lookup"><span data-stu-id="07059-304">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="07059-305">Non è possibile arrestare il reindirizzamento di stdout in C:\Programmi\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="07059-305">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="07059-306">Messaggio di eccezione: HRESULT 0x80070002 restituito in {PATH}.</span><span class="sxs-lookup"><span data-stu-id="07059-306">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="07059-307">Non è possibile avviare il reindirizzamento di stdout in {PATH}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="07059-307">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="07059-308">**Registro dell'applicazione:** Avviso: Impossibile creare stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -214702489.</span><span class="sxs-lookup"><span data-stu-id="07059-308">**Application Log:** Warning: Could not create stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="07059-309">**Log stdout del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="07059-309">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="07059-310">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="07059-310">Troubleshooting:</span></span>

* <span data-ttu-id="07059-311">Il percorso `stdoutLogFile` specificato nell'elemento `<aspNetCore>` di *web.config* non esiste.</span><span class="sxs-lookup"><span data-stu-id="07059-311">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="07059-312">Per altre informazioni, vedere [Modulo ASP.NET Core: Creazione e reindirizzamento dei log](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span><span class="sxs-lookup"><span data-stu-id="07059-312">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="07059-313">L'utente del pool di app non ha accesso in scrittura al percorso del log di stdout.</span><span class="sxs-lookup"><span data-stu-id="07059-313">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="07059-314">Problema generale della configurazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="07059-314">Application configuration general issue</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="07059-315">**Browser:** Errore HTTP 500.0 - Errore di caricamento gestore In-Process ANCM **--oppure--** Errore HTTP 500.30 - Errore di avvio In-Process ANCM</span><span class="sxs-lookup"><span data-stu-id="07059-315">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure **--OR--** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="07059-316">**Registro dell'applicazione:** Variabile</span><span class="sxs-lookup"><span data-stu-id="07059-316">**Application Log:** Variable</span></span>

* <span data-ttu-id="07059-317">**Log stdout del modulo ASP.NET Core:** il file di log viene creato, ma vuoto o con voci normali fino al punto di errore dell'app.</span><span class="sxs-lookup"><span data-stu-id="07059-317">**ASP.NET Core Module stdout Log:** The log file is created but empty or created with normal entries until the point of the app failing.</span></span>

* <span data-ttu-id="07059-318">**Log di debug del modulo ASP.NET Core:** Variabile</span><span class="sxs-lookup"><span data-stu-id="07059-318">**ASP.NET Core Module Debug Log:** Variable</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="07059-319">**Browser:** errore HTTP 502.5 - Errore del processo</span><span class="sxs-lookup"><span data-stu-id="07059-319">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="07059-320">**Registro dell'applicazione:** L'applicazione "MACHINE/WEBROOT/APPHOST/{ASSEMBLY}" con radice fisica "C:\{PATH}\' ha creato un processo con la riga di comando" "C:\{PATH}\{ASSEMBLY}.{exe|dll}" ma ha subito un arresto anomalo o non ha risposto o non era in ascolto sulla porta specificata "{PORT}", ErrorCode = '{ERROR CODE}'</span><span class="sxs-lookup"><span data-stu-id="07059-320">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' created process with commandline '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' but either crashed or did not respond or did not listen on the given port '{PORT}', ErrorCode = '{ERROR CODE}'</span></span>

* <span data-ttu-id="07059-321">**Log stdout del modulo ASP.NET Core:** il file di log viene creato ma vuoto.</span><span class="sxs-lookup"><span data-stu-id="07059-321">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="07059-322">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="07059-322">Troubleshooting:</span></span>

<span data-ttu-id="07059-323">non è stato possibile avviare il processo, molto probabilmente a causa di un problema di configurazione dell'app o di programmazione.</span><span class="sxs-lookup"><span data-stu-id="07059-323">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="07059-324">Per altre informazioni, vedere i seguenti argomenti:</span><span class="sxs-lookup"><span data-stu-id="07059-324">For more information, see the following topics:</span></span>

* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:test/troubleshoot>