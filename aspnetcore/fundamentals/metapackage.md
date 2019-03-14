---
title: Metapacchetto Microsoft.AspNetCore.All per ASP.NET Core 2.0
author: Rick-Anderson
description: Il metapacchetto Microsoft.AspNetCore.All non è consigliato per ASP.NET Core 2.1 e versioni successive.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/metapackage
ms.openlocfilehash: d95bafd412969bb8db38499bd2ff01af510d872c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57064858"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="3a4cc-103">Metapacchetto Microsoft.AspNetCore.All per ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="3a4cc-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="3a4cc-104">È consigliabile che le applicazioni destinate a ASP.NET Core 2.1 e versioni successive usino il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) invece di questo pacchetto.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="3a4cc-105">Vedere [Migrazione da Microsoft.AspNetCore.All a Microsoft.AspNetCore.App](#migrate) in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="3a4cc-106">Questa funzionalità richiede ASP.NET Core 2.x con destinazione .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="3a4cc-107">Il metapacchetto [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) per ASP.NET include:</span><span class="sxs-lookup"><span data-stu-id="3a4cc-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="3a4cc-108">Tutti i pacchetti supportati dal team ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="3a4cc-109">Tutti i pacchetti supportati da Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-109">All supported packages by the Entity Framework Core.</span></span>
* <span data-ttu-id="3a4cc-110">Le dipendenze interne e di terze parti usate da ASP.NET Core e da Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="3a4cc-111">Tutte le funzionalità di ASP.NET Core 2.x e Entity Framework Core 2.x sono incluse nel pacchetto `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="3a4cc-112">I modelli di progetto predefiniti destinati ad ASP.NET Core 2.0 usano questo pacchetto.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="3a4cc-113">Il numero di versione del metapacchetto `Microsoft.AspNetCore.All` rappresenta la versione di ASP.NET Core e la versione di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="3a4cc-114">Le applicazioni che usano il metapacchetto `Microsoft.AspNetCore.All` sfruttano automaticamente l'[archivio di runtime di .NET Core](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="3a4cc-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="3a4cc-115">L'archivio di runtime contiene tutti gli asset di runtime necessari per eseguire le applicazioni ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="3a4cc-116">Quando si usa il metapacchetto `Microsoft.AspNetCore.All`, con l'applicazione **non** vengono distribuiti asset dai pacchetti NuGet di riferimento di ASP.NET Core. Questi asset sono infatti inclusi nell'archivio di runtime di .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="3a4cc-117">Gli asset contenuti nell'archivio di runtime sono precompilati per migliorare i tempi di avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="3a4cc-118">Per rimuovere i pacchetti che non vengono usati è possibile usare il processo di trimming dei pacchetti.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="3a4cc-119">I pacchetti di cui è stato eseguito il trimming sono esclusi dall'output dell'applicazione pubblicata.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="3a4cc-120">Il file con estensione *csproj* seguente fa riferimento al metapacchetto `Microsoft.AspNetCore.All` per ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="3a4cc-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=8)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implicit-versioning"></a><span data-ttu-id="3a4cc-121">Controllo delle versioni implicito</span><span class="sxs-lookup"><span data-stu-id="3a4cc-121">Implicit versioning</span></span>

<span data-ttu-id="3a4cc-122">In ASP.NET Core 2.1 o versioni successive è possibile specificare il riferimento al pacchetto `Microsoft.AspNetCore.All` senza la versione.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-122">In ASP.NET Core 2.1 or later, you can specify the `Microsoft.AspNetCore.All` package reference without a version.</span></span> <span data-ttu-id="3a4cc-123">Quando la versione non è specificata, il SDK specifica una versione implicita (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="3a4cc-123">When the version isn't specified, an implicit version is specified by the SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="3a4cc-124">È consigliabile basarsi sulla versione implicita specificata dall'SDK e non impostando in modo esplicito il numero di versione sul riferimento al pacchetto.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-124">We recommend relying on the implicit version specified by the SDK and not explicitly setting the version number on the package reference.</span></span> <span data-ttu-id="3a4cc-125">In caso di domande su questo approccio, lasciare un commento GitHub nella pagina della [discussione per la versione implicita Microsoft.AspNetCore.App](https://github.com/aspnet/Docs/issues/6430).</span><span class="sxs-lookup"><span data-stu-id="3a4cc-125">If you have questions about this approach, leave a GitHub comment at the [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/Docs/issues/6430).</span></span>

<span data-ttu-id="3a4cc-126">La versione implicita è impostata su `major.minor.0` per le app portabili.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-126">The implicit version is set to `major.minor.0` for portable apps.</span></span> <span data-ttu-id="3a4cc-127">Il meccanismo di roll forward del framework condiviso esegue l'app sulla versione compatibile più recente tra i framework condivisi installati.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-127">The shared framework roll-forward mechanism runs the app on the latest compatible version among the installed shared frameworks.</span></span> <span data-ttu-id="3a4cc-128">Per garantire che venga usata la stessa versione in fase di sviluppo, test e produzione, verificare che in tutti gli ambienti sia installata la stessa versione del framework condiviso.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-128">To guarantee the same version is used in development, test, and production, ensure the same version of the shared framework is installed in all environments.</span></span> <span data-ttu-id="3a4cc-129">Per le app autonome, il numero di versione implicita è impostato sul valore `major.minor.patch` del framework condiviso nel bundle nell'SDK installato.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-129">For self-contained apps, the implicit version number is set to the `major.minor.patch` of the shared framework bundled in the installed SDK.</span></span>

<span data-ttu-id="3a4cc-130">Il fatto di specificare un numero di versione nel riferimento del pacchetto `Microsoft.AspNetCore.All` **non** garantisce che verrà scelta la versione corrispondente del framework condiviso.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-130">Specifying a version number on the `Microsoft.AspNetCore.All` package reference does **not** guarantee that version of the shared framework is chosen.</span></span> <span data-ttu-id="3a4cc-131">Si supponga ad esempio che venga specificata la versione "2.1.1" ma che sia installata la versione "2.1.3".</span><span class="sxs-lookup"><span data-stu-id="3a4cc-131">For example, suppose version "2.1.1" is specified, but "2.1.3" is installed.</span></span> <span data-ttu-id="3a4cc-132">In tal caso, l'app userà "2.1.3".</span><span class="sxs-lookup"><span data-stu-id="3a4cc-132">In that case, the app will use "2.1.3".</span></span> <span data-ttu-id="3a4cc-133">Benché non sia consigliabile, è possibile disabilitare il roll forward (patch e/o versioni secondarie).</span><span class="sxs-lookup"><span data-stu-id="3a4cc-133">Although not recommended, you can disable roll forward (patch and/or minor).</span></span> <span data-ttu-id="3a4cc-134">Per altre informazioni su come eseguire il roll forward dell'host dotnet e configurarne il comportamento, vedere [dotnet host rollforward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md) (Roll forward dell'host dotnet).</span><span class="sxs-lookup"><span data-stu-id="3a4cc-134">For more information regarding dotnet host roll-forward and how to configure its behavior, see [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span></span>

<span data-ttu-id="3a4cc-135">Il SDK del progetto deve essere impostato su `Microsoft.NET.Sdk.Web` nel file di progetto perché venga usata la versione implicita di `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-135">The project's SDK must be set to `Microsoft.NET.Sdk.Web` in the project file to use the implicit version of `Microsoft.AspNetCore.All`.</span></span> <span data-ttu-id="3a4cc-136">Se è specificato il SDK `Microsoft.NET.Sdk` (`<Project Sdk="Microsoft.NET.Sdk">` nella parte superiore del file di progetto), viene generato l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="3a4cc-136">When the `Microsoft.NET.Sdk` SDK is specified (`<Project Sdk="Microsoft.NET.Sdk">` at the top of the project file), the following warning is generated:</span></span>

<span data-ttu-id="3a4cc-137">*Avviso NU1604: Dipendenza del progetto Microsoft. aspnetcore non contiene un limite inferiore inclusivo. Includere un limite inferiore nella versione della dipendenza per garantire risultati di ripristino coerenti.*</span><span class="sxs-lookup"><span data-stu-id="3a4cc-137">*Warning NU1604: Project dependency Microsoft.AspNetCore.All does not contain an inclusive lower bound. Include a lower bound in the dependency version to ensure consistent restore results.*</span></span>

<span data-ttu-id="3a4cc-138">Si tratta di un problema noto con .NET Core 2.1 SDK, che verrà risolto in .NET Core 2.2 SDK.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-138">This is a known issue with the .NET Core 2.1 SDK and will be fixed in the .NET Core 2.2 SDK.</span></span>

::: moniker-end

<a name="migrate"></a>

## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="3a4cc-139">Migrazione da Microsoft.AspNetCore.All a Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="3a4cc-139">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="3a4cc-140">I pacchetti seguenti sono inclusi in `Microsoft.AspNetCore.All` ma non il pacchetto `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-140">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span>

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

<span data-ttu-id="3a4cc-141">Per passare da `Microsoft.AspNetCore.All` a `Microsoft.AspNetCore.App`, se l'app usa qualsiasi API dai pacchetti sopra o pacchetti inseriti da tali pacchetti, aggiungere i riferimenti a tali pacchetti nel progetto.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-141">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="3a4cc-142">Tutte le dipendenze dei pacchetti precedenti che non sono in altro modo dipendenze di `Microsoft.AspNetCore.App` non sono incluse in modo implicito.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-142">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="3a4cc-143">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3a4cc-143">For example:</span></span>

* <span data-ttu-id="3a4cc-144">`StackExchange.Redis` come dipendenza di `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="3a4cc-144">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="3a4cc-145">`Microsoft.ApplicationInsights` come dipendenza di `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="3a4cc-145">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>

## <a name="update-aspnet-core-21"></a><span data-ttu-id="3a4cc-146">Aggiornare ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="3a4cc-146">Update ASP.NET Core 2.1</span></span>

<span data-ttu-id="3a4cc-147">È consigliabile eseguire la migrazione al metapacchetto `Microsoft.AspNetCore.App` per la versione 2.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-147">We recommend migrating to the `Microsoft.AspNetCore.App` metapackage for 2.1 and later.</span></span> <span data-ttu-id="3a4cc-148">Per continuare a usare il metapacchetto `Microsoft.AspNetCore.All` e assicurarsi che venga distribuita la versione della patch più recente:</span><span class="sxs-lookup"><span data-stu-id="3a4cc-148">To keep using the `Microsoft.AspNetCore.All` metapackage and ensure the latest patch version is deployed:</span></span>

* <span data-ttu-id="3a4cc-149">Nel computer di sviluppo e i server di compilazione: Installare la versione più recente [.NET Core SDK](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="3a4cc-149">On development machines and build servers: Install the latest [.NET Core SDK](https://www.microsoft.com/net/download).</span></span>
* <span data-ttu-id="3a4cc-150">Nel server di distribuzione: Installare la versione più recente [runtime di .NET Core](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="3a4cc-150">On deployment servers: Install the latest [.NET Core runtime](https://www.microsoft.com/net/download).</span></span>
 <span data-ttu-id="3a4cc-151">L'app eseguirà il roll forward all'ultima versione installata al riavvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3a4cc-151">Your app will roll forward to the latest installed version on an application restart.</span></span>
