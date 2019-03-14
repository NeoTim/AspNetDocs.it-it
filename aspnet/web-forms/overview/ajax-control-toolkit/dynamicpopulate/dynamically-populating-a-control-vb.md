---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Popolamento dinamico di un controllo (VB) | Microsoft Docs
author: wenz
description: Il controllo di DynamicPopulate di ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e inserisce il valore risultante in un controllo di destinazione in t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: cda816cab99867aac8770c420cab7a78ba699e4e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57052028"
---
<a name="dynamically-populating-a-control-vb"></a><span data-ttu-id="9d028-103">Popolamento dinamico di un controllo (VB)</span><span class="sxs-lookup"><span data-stu-id="9d028-103">Dynamically Populating a Control (VB)</span></span>
====================
<span data-ttu-id="9d028-104">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9d028-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9d028-105">[Scaricare il codice](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="9d028-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span></span>

> <span data-ttu-id="9d028-106">Il controllo di DynamicPopulate di ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e inserisce il valore risultante in un controllo di destinazione nella pagina senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="9d028-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>


## <a name="overview"></a><span data-ttu-id="9d028-107">Panoramica</span><span class="sxs-lookup"><span data-stu-id="9d028-107">Overview</span></span>

<span data-ttu-id="9d028-108">Il `DynamicPopulate` chiama un servizio web (o un metodo di pagina) e inserisce il valore risultante in un controllo di destinazione nella pagina senza un aggiornamento della pagina di controllo in ASP.NET AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="9d028-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="9d028-109">Questa esercitazione viene illustrato come impostare questa funzionalità.</span><span class="sxs-lookup"><span data-stu-id="9d028-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="9d028-110">Passaggi</span><span class="sxs-lookup"><span data-stu-id="9d028-110">Steps</span></span>

<span data-ttu-id="9d028-111">Prima di tutto, è necessario un servizio Web ASP.NET che implementa il metodo che verrà chiamata da `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="9d028-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="9d028-112">Classe del servizio web richiede la `ScriptService` attributo di cui è definito all'interno `Microsoft.Web.Script.Services`; in caso contrario, ASP.NET AJAX non è possibile creare il proxy di JavaScript lato client per il servizio web che a sua volta è richiesta da `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="9d028-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="9d028-113">Il metodo web deve prevedere un unico argomento di tipo string, denominato `contextKey`, poiché il `DynamicPopulate` controllo Invia un'informazione rapida con ogni chiamata al servizio web.</span><span class="sxs-lookup"><span data-stu-id="9d028-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="9d028-114">Il seguente servizio web restituisce la data corrente in un formato rappresentato dal `contextKey` argomento:</span><span class="sxs-lookup"><span data-stu-id="9d028-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="9d028-115">Il servizio web viene quindi salvato come `DynamicPopulate.vb.asmx`.</span><span class="sxs-lookup"><span data-stu-id="9d028-115">The web service is then saved as `DynamicPopulate.vb.asmx`.</span></span> <span data-ttu-id="9d028-116">In alternativa, è possibile implementare il `getDate()` metodo come metodo di pagina all'interno della pagina ASP.NET effettivo con il `DynamicPopulate` controllo.</span><span class="sxs-lookup"><span data-stu-id="9d028-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="9d028-117">Nel passaggio successivo, creare un nuovo file ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9d028-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="9d028-118">Come sempre, il primo passaggio per includere il `ScriptManager` nella tabella corrente per caricare la libreria ASP.NET AJAX e rendere il lavoro Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="9d028-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="9d028-119">Quindi, aggiungere un controllo etichetta (ad esempio usando il controllo HTML lo stesso nome, o la &lt; `asp:Label`  / &gt; controllo web) che in un secondo momento verrà visualizzato il risultato della chiamata al servizio web.</span><span class="sxs-lookup"><span data-stu-id="9d028-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

<span data-ttu-id="9d028-120">Verrà usato un pulsante HTML (come un controllo HTML, perché non sono necessari un postback al server) per attivare la compilazione dinamica:</span><span class="sxs-lookup"><span data-stu-id="9d028-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="9d028-121">Infine, è necessario il `DynamicPopulateExtender` controllo per le operazioni wire.</span><span class="sxs-lookup"><span data-stu-id="9d028-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="9d028-122">Verranno impostati gli attributi seguenti (oltre a quelli ovvie, `ID` e `runat` = `"server"`):</span><span class="sxs-lookup"><span data-stu-id="9d028-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="9d028-123">`TargetControlID` posizione in cui inserire il risultato dalla chiamata al servizio web</span><span class="sxs-lookup"><span data-stu-id="9d028-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="9d028-124">`ServicePath` percorso del servizio web (omettere se si desidera utilizzare un metodo di pagina)</span><span class="sxs-lookup"><span data-stu-id="9d028-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="9d028-125">`ServiceMethod` nome del metodo web o del metodo di pagina</span><span class="sxs-lookup"><span data-stu-id="9d028-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="9d028-126">`ContextKey` informazioni di contesto da inviare al servizio web</span><span class="sxs-lookup"><span data-stu-id="9d028-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="9d028-127">`PopulateTriggerControlID` elemento che attiva la chiamata al servizio web</span><span class="sxs-lookup"><span data-stu-id="9d028-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="9d028-128">`ClearContentsDuringUpdate` Se su un valore vuoto l'elemento di destinazione durante la chiamata al servizio web</span><span class="sxs-lookup"><span data-stu-id="9d028-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="9d028-129">Come si può notare, il controllo richiede alcune informazioni ma l'inserimento di tutti gli elementi nella posizione corretta è piuttosto semplice.</span><span class="sxs-lookup"><span data-stu-id="9d028-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="9d028-130">Ecco il markup per il `DynamicPopulateExtender` controllo nello scenario corrente:</span><span class="sxs-lookup"><span data-stu-id="9d028-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="9d028-131">Eseguire la pagina ASP.NET nel browser e fare clic sul pulsante. si riceverà la data corrente nel formato mese-giorno-anno.</span><span class="sxs-lookup"><span data-stu-id="9d028-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>


<span data-ttu-id="9d028-132">[![Un clic sul pulsante Recupera la data dal server](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9d028-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="9d028-133">Un clic sul pulsante Recupera la data dal server ([fare clic per visualizzare l'immagine con dimensioni normali](dynamically-populating-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9d028-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9d028-134">[Precedente](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [Successivo](dynamically-populating-a-control-using-javascript-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9d028-134">[Previous](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
[Next](dynamically-populating-a-control-using-javascript-code-vb.md)</span></span>