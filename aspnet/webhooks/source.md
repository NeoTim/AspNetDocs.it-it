---
uid: webhooks/source
title: Webhook ASP.NET il codice sorgente e i pacchetti NuGet | Microsoft Docs
author: rick-anderson
description: Collegamenti a codice sorgente Webhook ASP.NET e i pacchetti NuGet
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: f88d9247f9d8aa0c5edc1ffc462be21d9319a725
ms.sourcegitcommit: dd0dc556a3d99a31d8fdbc763e9a2e53f3441b70
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/27/2019
ms.locfileid: "67410794"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="7b87e-103">Webhook ASP.NET il codice sorgente e i pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="7b87e-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="7b87e-104">Microsoft ASP.NET WebHooks fa parte della famiglia Microsoft ASP.NET di moduli e viene ospitato come un [Open Source Project su GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="7b87e-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="7b87e-105">Ciò significa che abbiamo accetterà i contributi, ma, esaminare i [linee guida specifiche](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) prima di inviare una richiesta pull.</span><span class="sxs-lookup"><span data-stu-id="7b87e-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="7b87e-106">Questa documentazione online di cui si esegue la lettura a questo punto è ospitata anche come [Open Source su GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) e accetta anche i contributi.</span><span class="sxs-lookup"><span data-stu-id="7b87e-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="7b87e-107">Pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="7b87e-107">NuGet packages</span></span>

<span data-ttu-id="7b87e-108">Il [i pacchetti NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) sono suddivisi in tre parti:</span><span class="sxs-lookup"><span data-stu-id="7b87e-108">The [NuGet packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are divided into three parts:</span></span>

* <span data-ttu-id="7b87e-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): Un pacchetto più comune che è condiviso tra mittenti e ricevitori.</span><span class="sxs-lookup"><span data-stu-id="7b87e-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="7b87e-110">[Mittente](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): Un set di pacchetti che supportano l'invio di Webhook personalizzati ad altri utenti.</span><span class="sxs-lookup"><span data-stu-id="7b87e-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="7b87e-111">La funzionalità per l'invio di Webhook è descritto più dettagliatamente [l'invio di Webhook](sending/senders).</span><span class="sxs-lookup"><span data-stu-id="7b87e-111">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/senders).</span></span>

* <span data-ttu-id="7b87e-112">[I ricevitori](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): Un set di pacchetti che supportano la ricezione di Webhook da altri utenti.</span><span class="sxs-lookup"><span data-stu-id="7b87e-112">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="7b87e-113">La funzionalità per ricevere i Webhook è descritto più dettagliatamente [ricezione Webhook](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="7b87e-113">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
