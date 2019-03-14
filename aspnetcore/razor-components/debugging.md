---
title: Eseguire il debug di componenti Razor
author: guardrex
description: Informazioni su come eseguire il debug di App Blazor e componenti di Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/debug
ms.openlocfilehash: fb7ddcf3ae40ec28a372adf724a293b375be28a1
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57034118"
---
# <a name="debug-razor-components"></a><span data-ttu-id="fe4f4-103">Eseguire il debug di componenti Razor</span><span class="sxs-lookup"><span data-stu-id="fe4f4-103">Debug Razor Components</span></span>

[<span data-ttu-id="fe4f4-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="fe4f4-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="fe4f4-105">I componenti di Razor presenta alcune *primissime* supporto per il debug delle applicazioni Blazor lato client in esecuzione su WebAssembly in Chrome.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-105">Razor Components has some *very early* support for debugging client-side Blazor apps running on WebAssembly in Chrome.</span></span> <span data-ttu-id="fe4f4-106">Anche se questo supporto iniziale per il debug è molto limitato e unpolished, Mostra l'infrastruttura di debug base assemblati.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-106">While this initial debugging support is very limited and unpolished, it shows the basic debugging infrastructure coming together.</span></span>

<span data-ttu-id="fe4f4-107">Eseguire il debug di un'app di Blazor lato client in Chrome:</span><span class="sxs-lookup"><span data-stu-id="fe4f4-107">To debug a client-side Blazor app in Chrome:</span></span>

* <span data-ttu-id="fe4f4-108">Creare un'app Blazor in `Debug` configurazione (il valore predefinito per le app non pubblicate).</span><span class="sxs-lookup"><span data-stu-id="fe4f4-108">Build a Blazor app in `Debug` configuration (the default for non-published apps).</span></span>
* <span data-ttu-id="fe4f4-109">Eseguire l'app Blazor in Chrome (versione 70 o versione successiva).</span><span class="sxs-lookup"><span data-stu-id="fe4f4-109">Run the Blazor app in Chrome (version 70 or later).</span></span>
* <span data-ttu-id="fe4f4-110">Con lo stato attivo sull'app (non nel Pannello di strumenti per sviluppatori, che probabilmente è necessario chiudere un'esperienza di debug chiarezza), selezionare la seguente Blazor specifico tasto di scelta rapida:</span><span class="sxs-lookup"><span data-stu-id="fe4f4-110">With the keyboard focus on the app (not in the dev tools panel, which you should probably close for a less confusing debugging experience), select the following Blazor-specific keyboard shortcut:</span></span>
  * <span data-ttu-id="fe4f4-111">`Shift+Alt+D` in Windows/Linux</span><span class="sxs-lookup"><span data-stu-id="fe4f4-111">`Shift+Alt+D` on Windows/Linux</span></span>
  * <span data-ttu-id="fe4f4-112">`Shift+Cmd+D` in macOS</span><span class="sxs-lookup"><span data-stu-id="fe4f4-112">`Shift+Cmd+D` on macOS</span></span>

<span data-ttu-id="fe4f4-113">Eseguire Chrome con il debug remoto abilitato per il debug dell'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-113">Run Chrome with remote debugging enabled to debug the Blazor app.</span></span> <span data-ttu-id="fe4f4-114">Se il debug remoto è disabilitato, una pagina di errore viene generata in Chrome.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-114">If remote debugging is disabled, an error page is generated by Chrome.</span></span> <span data-ttu-id="fe4f4-115">La pagina di errore è contenute le istruzioni per l'esecuzione di Chrome con la porta di debug aperto in modo che il proxy di debug Blazor possa connettersi all'app.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-115">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="fe4f4-116">*Chiudere tutte le istanze di Chrome* e riavviare Chrome come richiesto.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-116">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

![Pagina di errore debug Blazor](https://user-images.githubusercontent.com/1874516/43123091-01ec0796-8ed8-11e8-844c-23b4e6e9d069.png)

<span data-ttu-id="fe4f4-118">Chrome è in esecuzione con il debug remoto abilitato, il tasto di scelta rapida di debug consente di aprire una nuova scheda del debugger. Dopo qualche istante, la *origini* scheda Visualizza un elenco degli assembly .NET nell'app.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-118">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the *Sources* tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="fe4f4-119">Espandere ciascun assembly e individuare il *cs*/*cshtml* disponibile per il debug dei file di origine.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-119">Expand each assembly and find the *.cs*/*.cshtml* source files available for debugging.</span></span> <span data-ttu-id="fe4f4-120">Impostare punti di interruzione, passare alla scheda dell'app e i punti di interruzione vengono raggiunti.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-120">Set breakpoints, switch back to the app's tab, and the breakpoints are hit.</span></span> <span data-ttu-id="fe4f4-121">Dopo un punto di interruzione viene raggiunto, passo a passo (`F10`) o riprendere (`F8`) normalmente.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-121">After a breakpoint is hit, single-step (`F10`) or resume (`F8`) normally.</span></span>

![Debug Blazor](https://user-images.githubusercontent.com/1874516/43123060-efb0b3b0-8ed7-11e8-9ea5-97aa34247a0b.png)

<span data-ttu-id="fe4f4-123">Blazor fornisce un proxy di debug che implementa il [Chrome DevTools protocollo](https://chromedevtools.github.io/devtools-protocol/) e aumenta con il protocollo. Informazioni specifiche di NET.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-123">Blazor provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="fe4f4-124">Quando viene premuto debug tasti di scelta rapida, Chrome DevTools Blazor punti in corrispondenza del proxy.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-124">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="fe4f4-125">Il proxy si connette alla finestra del browser si sta tentando di eseguire il debug (di conseguenza la necessità di abilitare il debug remoto).</span><span class="sxs-lookup"><span data-stu-id="fe4f4-125">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

<span data-ttu-id="fe4f4-126">È lecito chiedersi perché non utilizzare semplicemente il mapping di origine del browser.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-126">You might be wondering why we don't just use browser source maps.</span></span> <span data-ttu-id="fe4f4-127">Mapping di origine Consenti browser eseguire il mapping dei file compilati i file di origine.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-127">Source maps allow the browser to map compiled files back to their original source files.</span></span> <span data-ttu-id="fe4f4-128">Tuttavia, Blazor non esegue il mapping C# direttamente in JS/WASM (almeno non ancora).</span><span class="sxs-lookup"><span data-stu-id="fe4f4-128">However, Blazor does not map C# directly to JS/WASM (at least not yet).</span></span> <span data-ttu-id="fe4f4-129">Al contrario, Blazor esegue interpretazione IL all'interno del browser, pertanto mapping di origine non sono rilevanti.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-129">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

<span data-ttu-id="fe4f4-130">Si noti che sono le funzionalità del debugger **molto limitato**.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-130">Note that the debugger capabilities are **very limited**.</span></span> <span data-ttu-id="fe4f4-131">È possibile attualmente solo:</span><span class="sxs-lookup"><span data-stu-id="fe4f4-131">You can currently only:</span></span>

* <span data-ttu-id="fe4f4-132">Impostare e rimuovere i punti di interruzione.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-132">Set and remove breakpoints.</span></span>
* <span data-ttu-id="fe4f4-133">Passo a passo il codice o resume (`F8`).</span><span class="sxs-lookup"><span data-stu-id="fe4f4-133">Single-step through the code or resume (`F8`).</span></span>
* <span data-ttu-id="fe4f4-134">Nel *variabili locali* visualizzati, osservare i valori delle variabili locali di tipo `int`, `string`, e `bool`.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-134">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="fe4f4-135">Vedere lo stack di chiamate, tra cui catene di chiamate che vanno da JavaScript in .NET e da .NET a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-135">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="fe4f4-136">Si *non è possibile*:</span><span class="sxs-lookup"><span data-stu-id="fe4f4-136">You *can't*:</span></span>

* <span data-ttu-id="fe4f4-137">Osservare i valori di eventuali variabili locali che non sono un' `int`, `string`, o `bool`.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-137">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="fe4f4-138">Osservare i valori di proprietà della classe o campi.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-138">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="fe4f4-139">Passare il mouse sulle variabili per visualizzarne i relativi valori</span><span class="sxs-lookup"><span data-stu-id="fe4f4-139">Hover over variables to see their values</span></span>
* <span data-ttu-id="fe4f4-140">Valutare le espressioni nella console.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-140">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="fe4f4-141">Passaggio tra le chiamate asincrone.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-141">Step across async calls.</span></span>
* <span data-ttu-id="fe4f4-142">Eseguire più altri comuni scenari di debug.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-142">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="fe4f4-143">Sviluppo di ulteriori scenari di debug è un'attenzione costante del team di progettazione.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-143">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="troubleshooting-tip"></a><span data-ttu-id="fe4f4-144">Suggerimento di risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="fe4f4-144">Troubleshooting tip</span></span>

<span data-ttu-id="fe4f4-145">Se si verificano errori, il suggerimento seguente può essere utile:</span><span class="sxs-lookup"><span data-stu-id="fe4f4-145">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="fe4f4-146">Nel **debugger** scheda, aprire gli strumenti di sviluppo nel browser.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-146">In the **debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="fe4f4-147">Nella console, eseguire `localStorage.clear()` per rimuovere i punti di interruzione.</span><span class="sxs-lookup"><span data-stu-id="fe4f4-147">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>