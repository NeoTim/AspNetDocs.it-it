---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Guida introduttiva a AJAX Control Toolkit (C ) Documenti Microsoft
author: rick-anderson
description: Scopri tutto quello che devi sapere per iniziare a usare AJAX Control Toolkit.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: b5954019ec3312f06f38012e4d3f9b1f71573f76
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543587"
---
# <a name="get-started-with-the-ajax-control-toolkit-c"></a><span data-ttu-id="79cd9-103">Introduzione ad AJAX Control Toolkit (C#)</span><span class="sxs-lookup"><span data-stu-id="79cd9-103">Get Started with the AJAX Control Toolkit (C#)</span></span>

<span data-ttu-id="79cd9-104">da parte [di Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="79cd9-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="79cd9-105">Scopri tutto quello che devi sapere per iniziare a usare AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="79cd9-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>

<span data-ttu-id="79cd9-106">AJAX Control Toolkit contiene più di 30 controlli liberi che è possibile utilizzare nelle applicazioni ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="79cd9-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="79cd9-107">In questa esercitazione viene illustrato come scaricare AJAX Control Toolkit e aggiungere i controlli del toolkit alla casella degli strumenti di Visual Studio/Visual Web Developer Express.</span><span class="sxs-lookup"><span data-stu-id="79cd9-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="79cd9-108">Download di AJAX Control Toolkit</span><span class="sxs-lookup"><span data-stu-id="79cd9-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="79cd9-109">[L'AJAX Control Toolkit](http://devexpress.com/act) è un progetto open source sviluppato dai membri della comunità ASP.NET e dal team di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="79cd9-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span> 

<span data-ttu-id="79cd9-110">[![Download di AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="79cd9-110">[![Downloading the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span></span>

<span data-ttu-id="79cd9-111">**Figura 01**: Download di AJAX Control Toolkit ([Fare clic per visualizzare l'immagine a dimensioni intere](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="79cd9-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span></span>

<span data-ttu-id="79cd9-112">Dopo aver scaricato il file, è necessario sbloccarlo.</span><span class="sxs-lookup"><span data-stu-id="79cd9-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="79cd9-113">Fare clic con il pulsante destro del mouse sul file, selezionare Proprietà e fare clic sul pulsante **Sblocca** (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="79cd9-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>

<span data-ttu-id="79cd9-114">[![Sblocco del file zip di AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="79cd9-114">[![Unblocking the AJAX Control Toolkit ZIP file](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span></span>

<span data-ttu-id="79cd9-115">**Figura 02**: Sblocco del file zip ajax Control Toolkit[(Fare clic per visualizzare l'immagine a dimensioni intere)](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="79cd9-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span></span>

<span data-ttu-id="79cd9-116">Dopo aver sbloccato il file, è possibile decomprimere il file: fare clic con il pulsante destro del mouse sul file e selezionare l'opzione di menu **Estrai tutto.**</span><span class="sxs-lookup"><span data-stu-id="79cd9-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="79cd9-117">A questo punto, siamo pronti per aggiungere il toolkit alla casella degli strumenti di Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="79cd9-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="79cd9-118">Aggiunta di AJAX Control Toolkit alla casella degli strumenti</span><span class="sxs-lookup"><span data-stu-id="79cd9-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="79cd9-119">Il modo più semplice per utilizzare AJAX Control Toolkit consiste nell'aggiungere il toolkit alla casella degli strumenti di Visual Studio/Visual Web Developer (vedere Figura 3).</span><span class="sxs-lookup"><span data-stu-id="79cd9-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="79cd9-120">In questo modo, è sufficiente trascinare un controllo toolkit in una pagina quando si desidera utilizzarlo.</span><span class="sxs-lookup"><span data-stu-id="79cd9-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>

<span data-ttu-id="79cd9-121">[![AJAX Control Toolkit viene visualizzato nella casella degli strumenti](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="79cd9-121">[![AJAX Control Toolkit appears in toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span></span>

<span data-ttu-id="79cd9-122">**Figura 03:** AJAX Control Toolkit viene visualizzato nella casella degli[strumenti(Fare clic per visualizzare l'immagine](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png)a dimensioni intere)</span><span class="sxs-lookup"><span data-stu-id="79cd9-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span></span>

<span data-ttu-id="79cd9-123">In primo luogo, è necessario aggiungere una scheda AJAX Control Toolkit alla casella degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="79cd9-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="79cd9-124">Seguire questa procedura.</span><span class="sxs-lookup"><span data-stu-id="79cd9-124">Follow these steps.</span></span>

1. <span data-ttu-id="79cd9-125">Creare un nuovo ASP.NET sito Web selezionando l'opzione di menu File, Nuovo sito Web.</span><span class="sxs-lookup"><span data-stu-id="79cd9-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="79cd9-126">Fare doppio clic su Default.aspx nella finestra Esplora soluzioni per aprire il file nell'editor.</span><span class="sxs-lookup"><span data-stu-id="79cd9-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="79cd9-127">Fare clic con il pulsante destro del mouse sulla Casella degli strumenti sotto la scheda Generale e selezionare l'opzione di menu **Aggiungi scheda** (vedere la figura 4).</span><span class="sxs-lookup"><span data-stu-id="79cd9-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="79cd9-128">Immettere una nuova scheda denominata AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="79cd9-128">Enter a new tab named AJAX Control Toolkit.</span></span>

<span data-ttu-id="79cd9-129">[![Aggiunta di una nuova scheda](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="79cd9-129">[![Adding a new tab](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span></span>

<span data-ttu-id="79cd9-130">**Figura 04**: Aggiunta di una nuova scheda([Fare clic per visualizzare l'immagine a dimensioni intere)](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="79cd9-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span></span>

<span data-ttu-id="79cd9-131">Successivamente, è necessario aggiungere i controlli AJAX Control Toolkit alla nuova scheda.</span><span class="sxs-lookup"><span data-stu-id="79cd9-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="79cd9-132">Fare clic con il pulsante destro del mouse sotto la scheda AJAX Control Toolkit e selezionare l'opzione di menu **Choose Items (vedere la Figura 5)**.</span><span class="sxs-lookup"><span data-stu-id="79cd9-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="79cd9-133">Passare al percorso in cui è stato decompresso L'AJAX Control Toolkit e selezionare l'assembly AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="79cd9-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>

<span data-ttu-id="79cd9-134">[![Scegliere gli elementi da aggiungere alla casella degli strumenti](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="79cd9-134">[![Choose items to add to the toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span></span>

<span data-ttu-id="79cd9-135">**Figura 05**: Scegliere gli elementi da aggiungere alla casella degli strumenti([Fare clic per visualizzare l'immagine](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png)a dimensioni intere)</span><span class="sxs-lookup"><span data-stu-id="79cd9-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span></span>

<span data-ttu-id="79cd9-136">Dopo aver completato questi passaggi, tutti i controlli del toolkit verranno visualizzati nella casella degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="79cd9-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="79cd9-137">Aggiornamento a una nuova versione del toolkit</span><span class="sxs-lookup"><span data-stu-id="79cd9-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="79cd9-138">Se si utilizza una versione precedente del Toolkit e ora è necessario passare a una versione successiva, ecco i passaggi consigliati:</span><span class="sxs-lookup"><span data-stu-id="79cd9-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="79cd9-139">Binaries - Elimina la versione precedente dell'assembly AjaxControlToolkit.dll dalla cartella Bin del sito Web.</span><span class="sxs-lookup"><span data-stu-id="79cd9-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="79cd9-140">Elementi della casella degli strumenti: eliminare la scheda AJAX Control Toolkit e seguire i passaggi precedenti per ricreare la scheda con la nuova versione dell'assembly AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="79cd9-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="79cd9-141">Avanti</span><span class="sxs-lookup"><span data-stu-id="79cd9-141">Next</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
