---
title: La migrazione della configurazione in ASP.NET Core
author: ardalis
description: Informazioni su come eseguire la migrazione di configurazione da un progetto ASP.NET MVC per un progetto ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/configuration
ms.openlocfilehash: 5a1c4d0cbbdf74a00073c654e78a05f44948caae
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048238"
---
# <a name="migrate-configuration-to-aspnet-core"></a><span data-ttu-id="038ce-103">La migrazione della configurazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="038ce-103">Migrate configuration to ASP.NET Core</span></span>

<span data-ttu-id="038ce-104">[Steve Smith](https://ardalis.com/) e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="038ce-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="038ce-105">Nell'articolo precedente, abbiamo iniziato [eseguire la migrazione di un progetto ASP.NET MVC ad ASP.NET Core MVC](xref:migration/mvc).</span><span class="sxs-lookup"><span data-stu-id="038ce-105">In the previous article, we began [migrate an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/mvc).</span></span> <span data-ttu-id="038ce-106">In questo articolo, si esegue la migrazione configurazione.</span><span class="sxs-lookup"><span data-stu-id="038ce-106">In this article, we migrate configuration.</span></span>

<span data-ttu-id="038ce-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="038ce-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="setup-configuration"></a><span data-ttu-id="038ce-108">Configurazione dell'installazione</span><span class="sxs-lookup"><span data-stu-id="038ce-108">Setup configuration</span></span>

<span data-ttu-id="038ce-109">ASP.NET Core non vengono più usati di *Global. asax* e *Web. config* file utilizzato le versioni precedenti di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="038ce-109">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="038ce-110">Nelle versioni precedenti di ASP.NET, la logica di avvio dell'applicazione è stata inserita un' `Application_StartUp` metodo all'interno *Global. asax*.</span><span class="sxs-lookup"><span data-stu-id="038ce-110">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="038ce-111">Più avanti, in ASP.NET MVC, una *Startup.cs* file è stato incluso nella radice del progetto; e, è stato chiamato quando l'applicazione è stata avviata.</span><span class="sxs-lookup"><span data-stu-id="038ce-111">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="038ce-112">ASP.NET Core ha adottato questo approccio completamente inserendo tutta la logica di avvio nel *Startup.cs* file.</span><span class="sxs-lookup"><span data-stu-id="038ce-112">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="038ce-113">Il *Web. config* file è stato sostituito anche in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="038ce-113">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="038ce-114">Configurazione stessa è ora possibile configurare, come parte della procedura di avvio dell'applicazione descritto in *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="038ce-114">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="038ce-115">Configurazione puoi comunque usare i file XML, ma in genere i progetti ASP.NET Core inseriranno i valori di configurazione in un file in formato JSON, ad esempio *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="038ce-115">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="038ce-116">Sistema di configurazione di ASP.NET Core anche facilmente accessibili le variabili di ambiente, che possono fornire una [posizione più protetta e solida](xref:security/app-secrets) per valori specifici dell'ambiente.</span><span class="sxs-lookup"><span data-stu-id="038ce-116">ASP.NET Core's configuration system can also easily access environment variables, which can provide a [more secure and robust location](xref:security/app-secrets) for environment-specific values.</span></span> <span data-ttu-id="038ce-117">Ciò vale soprattutto per i segreti, ad esempio le stringhe di connessione e le chiavi API che non devono essere verificate nel controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="038ce-117">This is especially true for secrets like connection strings and API keys that shouldn't be checked into source control.</span></span> <span data-ttu-id="038ce-118">Visualizzare [configurazione](xref:fundamentals/configuration/index) per altre informazioni sulla configurazione in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="038ce-118">See [Configuration](xref:fundamentals/configuration/index) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="038ce-119">In questo articolo si inizierà il progetto ASP.NET Core migrato parzialmente dalla [articolo precedente](xref:migration/mvc).</span><span class="sxs-lookup"><span data-stu-id="038ce-119">For this article, we are starting with the partially migrated ASP.NET Core project from [the previous article](xref:migration/mvc).</span></span> <span data-ttu-id="038ce-120">Configurazione di installazione, aggiungere il seguente costruttore e proprietà per il *Startup.cs* file che si trova nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="038ce-120">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

<span data-ttu-id="038ce-121">Si noti che a questo punto, il *Startup.cs* file non verrà compilato, poiché è necessario aggiungere il codice seguente `using` istruzione:</span><span class="sxs-lookup"><span data-stu-id="038ce-121">Note that at this point, the *Startup.cs* file won't compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="038ce-122">Aggiungere un *appSettings. JSON* file nella radice del progetto usando il modello di elemento appropriato:</span><span class="sxs-lookup"><span data-stu-id="038ce-122">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![Aggiungere AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="038ce-124">Migrazione delle impostazioni di configurazione dal Web. config</span><span class="sxs-lookup"><span data-stu-id="038ce-124">Migrate configuration settings from web.config</span></span>

<span data-ttu-id="038ce-125">Il progetto ASP.NET MVC incluso la stringa di connessione di database necessari nel *Web. config*, nella `<connectionStrings>` elemento.</span><span class="sxs-lookup"><span data-stu-id="038ce-125">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="038ce-126">Nel progetto di ASP.NET Core, si intende archiviare queste informazioni nel *appSettings. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="038ce-126">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="038ce-127">Aprire *appSettings. JSON*e notare che già include quanto segue:</span><span class="sxs-lookup"><span data-stu-id="038ce-127">Open *appsettings.json*, and note that it already includes the following:</span></span>

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

<span data-ttu-id="038ce-128">Nella riga evidenziata illustrata in precedenza, modificare il nome del database da **_CHANGE_ME** al nome del database.</span><span class="sxs-lookup"><span data-stu-id="038ce-128">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="038ce-129">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="038ce-129">Summary</span></span>

<span data-ttu-id="038ce-130">ASP.NET Core colloca tutta la logica di avvio per l'applicazione in un singolo file, in cui i servizi necessari e le dipendenze possono essere definite e configurate.</span><span class="sxs-lookup"><span data-stu-id="038ce-130">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="038ce-131">Sostituisce il *Web. config* file con una funzionalità di configurazione flessibili che può sfruttare un'ampia gamma di formati di file, ad esempio JSON, nonché le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="038ce-131">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>
