---
uid: webhooks/source
title: ASP.NET il codice sorgente di WebHooks e i pacchetti NuGet Documenti Microsoft
author: rick-anderson
description: Collegamenti a ASP.NET codice sorgente WebHooks e pacchetti NuGetLinks to WebHooks source code and NuGet packages
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: ad368125878871c0e38f35152c86fe4eea143924
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675318"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>ASP.NET codice sorgente WebHooks e pacchetti NuGet

Microsoft ASP.NET WebHooks fa parte della famiglia di moduli Microsoft ASP.NET ed è ospitata come [progetto open source su GitHub](https://github.com/aspnet/WebHooks). Ciò significa che accettiamo i contributi, ma si prega di guardare le [linee guida](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) per i contributi prima di inviare una richiesta pull.

Questa documentazione online che si sta leggendo ora è ospitata anche come [Open Source su GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) e accetta anche contributi.

## <a name="nuget-packages"></a>Pacchetti NuGet

I [pacchetti NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) sono divisi in tre parti:

* [Comune](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): Un pacchetto comune condiviso tra mittenti e destinatari.

* [Mittente](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): Un insieme di pacchetti che supportano l'invio di webHook ad altri. La funzionalità per l'invio di WebHook è descritta in modo più dettagliato in [Invio di WebHook.](sending/senders.md)

* [Ricevitori](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): Un insieme di pacchetti che supportano la ricezione di WebHook da altri. La funzionalità per la ricezione di WebHook è descritta in modo più dettagliato in [Ricezione di WebHook.](receiving/index.md)
