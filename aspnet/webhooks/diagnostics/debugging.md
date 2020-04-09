---
uid: webhooks/diagnostics/debugging
title: ASP.NET di debug di WebHooks Documenti Microsoft
author: rick-anderson
description: Come eseguire il debug di ASP.NET WebHook.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 2f1f8196478e7025a0467acb945d9ed36c8fd0ca
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675374"
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="f31e9-103">ASP.NET debug di WebHook</span><span class="sxs-lookup"><span data-stu-id="f31e9-103">ASP.NET WebHooks debugging</span></span>

## <a name="debugging-in-azure"></a><span data-ttu-id="f31e9-104">Debug in AzureDebugging in Azure</span><span class="sxs-lookup"><span data-stu-id="f31e9-104">Debugging in Azure</span></span>

<span data-ttu-id="f31e9-105">Per eseguire il debug dell'applicazione Web durante l'esecuzione in Azure, vedere l'esercitazione [Risolvere i problemi di un'app Web nel servizio app di Azure usando Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="f31e9-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="f31e9-106">Debug con origine e simboliDebugging with Source and Symbols</span><span class="sxs-lookup"><span data-stu-id="f31e9-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="f31e9-107">Oltre a eseguire il debug del proprio codice, è possibile eseguire il debug direttamente in Microsoft ASP.NET WebHook, e in realtà tutti .NET.</span><span class="sxs-lookup"><span data-stu-id="f31e9-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="f31e9-108">Questo funziona indipendentemente dal fatto che si esegue il debug in locale o in remoto.</span><span class="sxs-lookup"><span data-stu-id="f31e9-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="f31e9-109">Configurare innanzitutto Visual Studio per trovare l'origine e i simboli scegliendo **Debug** , quindi **Opzioni e impostazioni**.</span><span class="sxs-lookup"><span data-stu-id="f31e9-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="f31e9-110">Impostare le opzioni in questo modo:</span><span class="sxs-lookup"><span data-stu-id="f31e9-110">Set the options like this:</span></span>

![Opzioni e impostazioni](_static/SourceSymbols.png)

<span data-ttu-id="f31e9-112">Quindi aggiungi un link a [symbolsource.org](http://symbolsource.org) per scaricare l'origine e i simboli.</span><span class="sxs-lookup"><span data-stu-id="f31e9-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="f31e9-113">Vai alla scheda **Simboli** del menu qui sopra e aggiungi quanto segue come posizione del simbolo:</span><span class="sxs-lookup"><span data-stu-id="f31e9-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="f31e9-114">Inoltre, assicurarsi che la directory della cache abbia un nome breve; in caso contrario, i nomi dei file possono diventare troppo lunghi, il che causerà il non caricamento dei simboli.</span><span class="sxs-lookup"><span data-stu-id="f31e9-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="f31e9-115">Un percorso di esempio è:A sample path is:</span><span class="sxs-lookup"><span data-stu-id="f31e9-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="f31e9-116">Le impostazioni dovrebbero essere simili alle seguente:The settings should look similar to this:</span><span class="sxs-lookup"><span data-stu-id="f31e9-116">The settings should look similar to this:</span></span>

![Esempio di posizione del file dei simboli delle opzioni](_static/SymSource.png)
