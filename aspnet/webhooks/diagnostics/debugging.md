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
# <a name="aspnet-webhooks-debugging"></a>ASP.NET debug di WebHook

## <a name="debugging-in-azure"></a>Debug in AzureDebugging in Azure

Per eseguire il debug dell'applicazione Web durante l'esecuzione in Azure, vedere l'esercitazione [Risolvere i problemi di un'app Web nel servizio app di Azure usando Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Debug con origine e simboliDebugging with Source and Symbols

Oltre a eseguire il debug del proprio codice, è possibile eseguire il debug direttamente in Microsoft ASP.NET WebHook, e in realtà tutti .NET. Questo funziona indipendentemente dal fatto che si esegue il debug in locale o in remoto. Configurare innanzitutto Visual Studio per trovare l'origine e i simboli scegliendo **Debug** , quindi **Opzioni e impostazioni**. Impostare le opzioni in questo modo:

![Opzioni e impostazioni](_static/SourceSymbols.png)

Quindi aggiungi un link a [symbolsource.org](http://symbolsource.org) per scaricare l'origine e i simboli. Vai alla scheda **Simboli** del menu qui sopra e aggiungi quanto segue come posizione del simbolo:

```
http://srv.symbolsource.org/pdb/Public
```

Inoltre, assicurarsi che la directory della cache abbia un nome breve; in caso contrario, i nomi dei file possono diventare troppo lunghi, il che causerà il non caricamento dei simboli. Un percorso di esempio è:A sample path is:

```
C:\SymCache
```

Le impostazioni dovrebbero essere simili alle seguente:The settings should look similar to this:

![Esempio di posizione del file dei simboli delle opzioni](_static/SymSource.png)
