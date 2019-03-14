---
uid: webhooks/diagnostics/debugging
title: Webhook ASP.NET debug | Microsoft Docs
author: rick-anderson
description: Come eseguire il debug ASP.NET Webhook.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 517d282fc22703b5861b748aea51023fa0a12a26
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57062498"
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="418cb-103">Webhook ASP.NET debug</span><span class="sxs-lookup"><span data-stu-id="418cb-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="418cb-104">Debug in Azure</span><span class="sxs-lookup"><span data-stu-id="418cb-104">Debugging in Azure</span></span>

<span data-ttu-id="418cb-105">Per eseguire il debug dell'applicazione Web durante l'esecuzione in Azure, vedere l'esercitazione [risolvere i problemi di un'app web nel servizio App di Azure con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="418cb-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="418cb-106">Debug con simboli e origine</span><span class="sxs-lookup"><span data-stu-id="418cb-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="418cb-107">Oltre a debug il proprio codice, è possibile eseguire il debug direttamente in Microsoft ASP.NET WebHooks e in effetti tutti di .NET.</span><span class="sxs-lookup"><span data-stu-id="418cb-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="418cb-108">Questa opzione funziona indipendentemente dal fatto che si esegue il debug in locale o remoto.</span><span class="sxs-lookup"><span data-stu-id="418cb-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="418cb-109">Prima di tutto configurare Visual Studio per trovare l'origine e i simboli selezionando **Debug** e quindi **opzioni e impostazioni**.</span><span class="sxs-lookup"><span data-stu-id="418cb-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="418cb-110">Impostare le opzioni simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="418cb-110">Set the options like this:</span></span>

![Opzioni e impostazioni](_static/SourceSymbols.png)

<span data-ttu-id="418cb-112">Quindi aggiungere un collegamento a [symbolsource.org](http://symbolsource.org) per aver scaricato l'origine e i simboli.</span><span class="sxs-lookup"><span data-stu-id="418cb-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="418cb-113">Andare alla **simboli** scheda del precedente menu e aggiungere quanto segue come un percorso simboli:</span><span class="sxs-lookup"><span data-stu-id="418cb-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="418cb-114">Inoltre, assicurarsi che la directory della cache ha un nome breve. in caso contrario, i nomi dei file possono diventare troppo lunghi che produrrà i simboli non viene caricato.</span><span class="sxs-lookup"><span data-stu-id="418cb-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="418cb-115">È un percorso di esempio:</span><span class="sxs-lookup"><span data-stu-id="418cb-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="418cb-116">Le impostazioni dovrebbero essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="418cb-116">The settings should look similar to this:</span></span>

![Esempio di percorso di File di simboli opzioni](_static/SymSource.png)
