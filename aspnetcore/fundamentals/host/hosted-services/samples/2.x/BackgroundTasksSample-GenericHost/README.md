---
ms.openlocfilehash: 6b2bc386ec179e786de205af0ca6dbd610e000d9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57056488"
---
# <a name="aspnet-core-background-tasks-sample-generic-host"></a><span data-ttu-id="c70aa-101">Esempio di attività in background ASP.NET Core (host generico)</span><span class="sxs-lookup"><span data-stu-id="c70aa-101">ASP.NET Core Background Tasks Sample (Generic Host)</span></span>

<span data-ttu-id="c70aa-102">In questo esempio viene illustrato l'uso di [IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="c70aa-102">This sample illustrates the use of [IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span></span> <span data-ttu-id="c70aa-103">L'esempio presenta le funzionalità descritte nell'argomento [Attività in background con servizi ospitati in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="c70aa-103">This sample demonstrates the features described in the [Background tasks with hosted services in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services) topic.</span></span>

<span data-ttu-id="c70aa-104">Per l'esecuzione dell'esempio in [Visual Studio Code](https://code.visualstudio.com/), impostare il valore **console** della configurazione della console in *.vscode/launch.json* su `externalTerminal` o `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="c70aa-104">When running the sample in [Visual Studio Code](https://code.visualstudio.com/), set the **console** value of the console configuration in *.vscode/launch.json* to either `externalTerminal` or `integratedTerminal`.</span></span> <span data-ttu-id="c70aa-105">L'uso di `internalConsole` non è compatibile con l'input sequenza di tasti dalla console usato dall'app per accodare gli elementi di lavoro in background.</span><span class="sxs-lookup"><span data-stu-id="c70aa-105">Use of the `internalConsole` is incompatible with console keystroke input that the app uses to enqueue background work items.</span></span>
