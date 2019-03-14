---
uid: web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
title: '[Procedura:] Usare la modalità di aggiornamento condizionale del controllo UpdatePanel? | Microsoft Docs'
author: JoeStagner
description: UpdatePanel di ASP.NET AJAX include una proprietà UpdateMode che può essere impostata su 'Sempre' o 'Condizionale'. Il valore predefinito è sempre, nel qual caso il UpdatePan...
ms.author: riande
ms.date: 08/01/2007
ms.assetid: 10b5bad3-4c18-464f-9454-0b3e60b7b8be
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
msc.type: video
ms.openlocfilehash: 96d7bc95fd80e410feb332264695835e68793ae2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050898"
---
<a name="how-do-i-use-the-conditional-updatemode-of-the-updatepanel"></a><span data-ttu-id="e973f-105">[Procedura:] Usare la modalità di aggiornamento condizionale del controllo UpdatePanel?</span><span class="sxs-lookup"><span data-stu-id="e973f-105">[How Do I:] Use the Conditional UpdateMode of the UpdatePanel?</span></span>
====================
<span data-ttu-id="e973f-106">da [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e973f-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="e973f-107">UpdatePanel di ASP.NET AJAX include una proprietà UpdateMode che può essere impostata su 'Sempre' o 'Condizionale'.</span><span class="sxs-lookup"><span data-stu-id="e973f-107">The ASP.NET AJAX UpdatePanel includes an UpdateMode property that may be set to 'Always' or 'Conditional'.</span></span> <span data-ttu-id="e973f-108">Il valore predefinito è sempre, nel qual caso UpdatePanel sempre aggiornerà il relativo contenuto durante un postback asincrono.</span><span class="sxs-lookup"><span data-stu-id="e973f-108">The default is Always, in which case the UpdatePanel will always update its content during an asychronous postback.</span></span> <span data-ttu-id="e973f-109">In questo video viene spiegato come è possibile impostare il UpdateMode su Conditional, nel quale caso UpdatePanel verrà aggiornati solo il contenuto quando il codice lato server chiama il metodo Update.</span><span class="sxs-lookup"><span data-stu-id="e973f-109">In this video we learn how we can set the UpdateMode to Conditional, in which case the UpdatePanel will only update its content when our server-side code calls its Update method.</span></span> <span data-ttu-id="e973f-110">In questo modo è possibile utilizzare per la logica condizionale nel codice Visual Basic o c# per determinare se UpdatePanel aggiornerà il relativo contenuto durante il postback asincrono corrente.</span><span class="sxs-lookup"><span data-stu-id="e973f-110">This allows you to use conditional logic in your C# or Visual Basic code to determine whether the UpdatePanel will update its content during the current asynchronous postback.</span></span>

[<span data-ttu-id="e973f-111">&#9654;Guarda il video (13 minuti)</span><span class="sxs-lookup"><span data-stu-id="e973f-111">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-conditional-updatemode-of-the-updatepanel)

> [!div class="step-by-step"]
> <span data-ttu-id="e973f-112">[Precedente](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
> [Successivo](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span><span class="sxs-lookup"><span data-stu-id="e973f-112">[Previous](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
[Next](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span></span>
