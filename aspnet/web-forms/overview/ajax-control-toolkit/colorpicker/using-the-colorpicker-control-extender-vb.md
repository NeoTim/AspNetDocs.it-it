---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: Tramite il controllo Extender ColorPicker (VB) | Microsoft Docs
author: microsoft
description: ColorPicker è un extender di AJAX ASP.NET che fornisce funzionalità di selezione colore lato client con l'interfaccia utente in un controllo popup. Può essere collegato a qualsiasi ASP.NET...
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 311cd61ae971dd6b902411eca87f75f87f5868ae
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59384059"
---
# <a name="using-the-colorpicker-control-extender-vb"></a><span data-ttu-id="cbd06-104">Tramite il controllo Extender ColorPicker (VB)</span><span class="sxs-lookup"><span data-stu-id="cbd06-104">Using the ColorPicker Control Extender (VB)</span></span>

<span data-ttu-id="cbd06-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="cbd06-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="cbd06-106">ColorPicker è un extender di AJAX ASP.NET che fornisce funzionalità di selezione colore lato client con l'interfaccia utente in un controllo popup.</span><span class="sxs-lookup"><span data-stu-id="cbd06-106">ColorPicker is an ASP.NET AJAX extender that provides client-side color-picking functionality with UI in a popup control.</span></span> <span data-ttu-id="cbd06-107">Può essere collegato a qualsiasi controllo TextBox ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cbd06-107">It can be attached to any ASP.NET TextBox control.</span></span> <span data-ttu-id="cbd06-108">It.</span><span class="sxs-lookup"><span data-stu-id="cbd06-108">It.</span></span>


<span data-ttu-id="cbd06-109">L'obiettivo di questa esercitazione è illustrare come è possibile usare il controllo extender Toolkit ColorPicker di controllo AJAX.</span><span class="sxs-lookup"><span data-stu-id="cbd06-109">The goal of this tutorial is to explain how you can use the AJAX Control Toolkit ColorPicker control extender.</span></span> <span data-ttu-id="cbd06-110">Il controllo extender ColorPicker Visualizza una finestra di dialogo popup che consente di selezionare un colore.</span><span class="sxs-lookup"><span data-stu-id="cbd06-110">The ColorPicker control extender displays a popup dialog that enables you to select a color.</span></span> <span data-ttu-id="cbd06-111">ColorPicker è utile ogni volta che si desidera fornire un'interfaccia utente intuitiva per un utente di scegliere un colore.</span><span class="sxs-lookup"><span data-stu-id="cbd06-111">The ColorPicker is useful whenever you want to provide an intuitive user interface for a user to pick a color.</span></span>

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a><span data-ttu-id="cbd06-112">Estensione di un controllo casella di testo con il controllo Extender ColorPicker</span><span class="sxs-lookup"><span data-stu-id="cbd06-112">Extending a TextBox Control with the ColorPicker Control Extender</span></span>

<span data-ttu-id="cbd06-113">Si supponga, ad esempio, che si desidera creare un sito Web che consente ai visitatori di creare schede di business personalizzate.</span><span class="sxs-lookup"><span data-stu-id="cbd06-113">Imagine, for example, that you want to create a website that enables visitors to create customized business cards.</span></span> <span data-ttu-id="cbd06-114">I visitatori possono immettere il testo di un biglietto da visita e scegliere il colore.</span><span class="sxs-lookup"><span data-stu-id="cbd06-114">Visitors can enter the text for a business card and pick the color.</span></span> <span data-ttu-id="cbd06-115">La pagina ASP.NET nel listato 1 contiene due controlli TextBox denominato txtCardText e txtCardColor.</span><span class="sxs-lookup"><span data-stu-id="cbd06-115">The ASP.NET page in Listing 1 contains two TextBox controls named txtCardText and txtCardColor.</span></span> <span data-ttu-id="cbd06-116">Quando si invia il form, i valori selezionati vengono visualizzati (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="cbd06-116">When you submit the form, the selected values are displayed (see Figure 1).</span></span>


[![S<span data-ttu-id="cbd06-117">Consent form per la creazione di un biglietto da visita]</span><span class="sxs-lookup"><span data-stu-id="cbd06-117">imple form for creating a business card]</span></span>(using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

<span data-ttu-id="cbd06-118">**Figura 01**: Modulo semplice per la creazione di un biglietto da visita ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-colorpicker-control-extender-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="cbd06-118">**Figure 01**: Simple form for creating a business card ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image2.png))</span></span>


**<span data-ttu-id="cbd06-119">Listato 1 - CreateCard.aspx</span><span class="sxs-lookup"><span data-stu-id="cbd06-119">Listing 1 - CreateCard.aspx</span></span>**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

<span data-ttu-id="cbd06-120">Il modulo in funzionamento del listato 1, ma non fornisce un'esperienza utente eccezionale.</span><span class="sxs-lookup"><span data-stu-id="cbd06-120">The form in Listing 1 works, but it does not provide a great user experience.</span></span> <span data-ttu-id="cbd06-121">L'utente deve digitare un colore nella casella di testo.</span><span class="sxs-lookup"><span data-stu-id="cbd06-121">The user has to type a color into the textbox.</span></span> <span data-ttu-id="cbd06-122">Se l'utente desidera impostare un colore specializzato -, ad esempio, semplicemente la sfumatura a destra di unta: verde - quindi l'utente deve individuare il codice di colore HTML senza alcun aiuto.</span><span class="sxs-lookup"><span data-stu-id="cbd06-122">If the user wants a specialized color - for example, just the right shade of pea green - then the user must figure out the HTML color code without any help.</span></span>

<span data-ttu-id="cbd06-123">È possibile usare il controllo extender ColorPicker per creare un'esperienza utente migliore.</span><span class="sxs-lookup"><span data-stu-id="cbd06-123">You can use the ColorPicker control extender to create a better user experience.</span></span> <span data-ttu-id="cbd06-124">ColorPicker Visualizza una finestra di dialogo colore quando si sposta lo stato attivo a un controllo casella di testo (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="cbd06-124">The ColorPicker displays a color dialog when you move focus to a TextBox control (see Figure 2).</span></span>


[![T<span data-ttu-id="cbd06-125">egli controllo Extender ColorPicker]</span><span class="sxs-lookup"><span data-stu-id="cbd06-125">he ColorPicker Control Extender]</span></span>(using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

<span data-ttu-id="cbd06-126">**Figura 02**: Il controllo Extender ColorPicker ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-colorpicker-control-extender-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="cbd06-126">**Figure 02**: The ColorPicker Control Extender ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image4.png))</span></span>


<span data-ttu-id="cbd06-127">È necessario completare due passaggi per usare il controllo extender ColorPicker con il modulo nel listato 1:</span><span class="sxs-lookup"><span data-stu-id="cbd06-127">You need to complete two steps to use the ColorPicker control extender with the form in Listing 1:</span></span>

1. <span data-ttu-id="cbd06-128">Aggiungere un controllo ScriptManager alla pagina</span><span class="sxs-lookup"><span data-stu-id="cbd06-128">Add a ScriptManager control to the page</span></span>
2. <span data-ttu-id="cbd06-129">Aggiungere il controllo extender ColorPicker alla pagina</span><span class="sxs-lookup"><span data-stu-id="cbd06-129">Add the ColorPicker control extender to the page</span></span>

<span data-ttu-id="cbd06-130">Prima di poter usare ColorPicker, è necessario aggiungere uno ScriptManager nella pagina.</span><span class="sxs-lookup"><span data-stu-id="cbd06-130">Before you can use the ColorPicker, you must add a ScriptManager to your page.</span></span> <span data-ttu-id="cbd06-131">Un buon punto di aggiunta di ScriptManager è sotto il server-side di apertura &lt;form&gt; tag.</span><span class="sxs-lookup"><span data-stu-id="cbd06-131">A good place to add the ScriptManager is right below the opening server-side &lt;form&gt; tag.</span></span> <span data-ttu-id="cbd06-132">È possibile trascinare ScriptManager nella pagina dalla casella degli strumenti (ScriptManager si trova nella scheda Estensioni AJAX).</span><span class="sxs-lookup"><span data-stu-id="cbd06-132">You can drag the ScriptManager onto the page from the toolbox (the ScriptManager is located under the AJAX Extensions tab).</span></span> <span data-ttu-id="cbd06-133">In alternativa, è possibile digitare il seguente tag nella visualizzazione origine sotto il tag di apertura form lato server:</span><span class="sxs-lookup"><span data-stu-id="cbd06-133">Alternatively, you can type the following tag into Source View beneath the opening server-side form tag:</span></span>

<span data-ttu-id="cbd06-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span><span class="sxs-lookup"><span data-stu-id="cbd06-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span></span>

<span data-ttu-id="cbd06-135">Il modo più semplice per aggiungere il controllo extender ColorPicker alla pagina è in visualizzazione progettazione.</span><span class="sxs-lookup"><span data-stu-id="cbd06-135">The easiest way to add the ColorPicker control extender to the page is in Design View.</span></span> <span data-ttu-id="cbd06-136">Se si posiziona il puntatore del mouse txtCardColor nella casella di testo, un'opzione automatica verrà visualizzata la consente è quindi necessario aggiungere un'estensione (vedere la figura 3).</span><span class="sxs-lookup"><span data-stu-id="cbd06-136">If you hover your mouse over the txtCardColor TextBox, a smart task option appears the enables you to add an extender (see Figure 3).</span></span> <span data-ttu-id="cbd06-137">Se si seleziona questa opzione, viene visualizzata la procedura guidata estensione (vedere la figura 4).</span><span class="sxs-lookup"><span data-stu-id="cbd06-137">If you pick this option, the Extender Wizard appears (see Figure 4).</span></span>


[![A<span data-ttu-id="cbd06-138">un dispositivo extender dding]</span><span class="sxs-lookup"><span data-stu-id="cbd06-138">dding an extender]</span></span>(using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

<span data-ttu-id="cbd06-139">**Figura 03**: Aggiunta di un'estensione ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-colorpicker-control-extender-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="cbd06-139">**Figure 03**: Adding an extender ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image6.png))</span></span>


[![S<span data-ttu-id="cbd06-140">designazione di un controllo extender con la procedura guidata dispositivo Extender]</span><span class="sxs-lookup"><span data-stu-id="cbd06-140">electing a control extender with the Extender Wizard]</span></span>(using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

<span data-ttu-id="cbd06-141">**Figura 04**: Selezione di un controllo extender con la procedura guidata estensione ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-colorpicker-control-extender-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="cbd06-141">**Figure 04**: Selecting a control extender with the Extender Wizard ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image8.png))</span></span>


<span data-ttu-id="cbd06-142">È possibile selezionare il dispositivo extender ColorPicker per estendere il txtCardColor nella casella di testo con il dispositivo extender ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="cbd06-142">You can pick the ColorPicker extender to extend the txtCardColor TextBox with the ColorPicker extender.</span></span> <span data-ttu-id="cbd06-143">Fare clic su OK per chiudere la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="cbd06-143">Click OK to close the dialog.</span></span>

<span data-ttu-id="cbd06-144">Dopo aver apportato queste modifiche, l'origine per la pagina è simile a listato 2.</span><span class="sxs-lookup"><span data-stu-id="cbd06-144">After you make these changes, the source for the page looks like Listing 2.</span></span>

**<span data-ttu-id="cbd06-145">Listato 2 - CreateCard.aspx (con ColorPicker)</span><span class="sxs-lookup"><span data-stu-id="cbd06-145">Listing 2 - CreateCard.aspx (with ColorPicker)</span></span>**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

<span data-ttu-id="cbd06-146">Si noti che la pagina contiene ora un controllo ColorPickerExtender visualizzata direttamente sotto il controllo TextBox txtCardColor.</span><span class="sxs-lookup"><span data-stu-id="cbd06-146">Notice that the page now contains a ColorPickerExtender control that appears directly below the txtCardColor TextBox control.</span></span> <span data-ttu-id="cbd06-147">Il controllo ColorPickerExtender estende il controllo txtCardColor affinché visualizzi una finestra di dialogo di selezione colore.</span><span class="sxs-lookup"><span data-stu-id="cbd06-147">The ColorPickerExtender control extends the txtCardColor control so that it displays a color picker dialog.</span></span>

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a><span data-ttu-id="cbd06-148">Usare un pulsante per avviare la finestra di dialogo di selezione colore</span><span class="sxs-lookup"><span data-stu-id="cbd06-148">Using a Button to Launch the Color Picker Dialog</span></span>

<span data-ttu-id="cbd06-149">Il dispositivo extender ColorPicker supporta le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="cbd06-149">The ColorPicker extender supports the following properties:</span></span>

- <span data-ttu-id="cbd06-150">PopupButtonId - l'ID di un pulsante della pagina che fa sì che la finestra di dialogo Selezione colore da visualizzare.</span><span class="sxs-lookup"><span data-stu-id="cbd06-150">PopupButtonId - The ID of a button on the page that causes the color picker dialog to appear.</span></span>
- <span data-ttu-id="cbd06-151">PopupPosition - la posizione, relativo al controllo di destinazione, della finestra di dialogo di selezione colore.</span><span class="sxs-lookup"><span data-stu-id="cbd06-151">PopupPosition - The position, relative to the target control, of the color picker dialog.</span></span> <span data-ttu-id="cbd06-152">I valori possibili sono assoluti, centro, BottomLeft, BottomRight, TopLeft, alto, destra e a sinistra (il valore predefinito è BottomLeft).</span><span class="sxs-lookup"><span data-stu-id="cbd06-152">Possible values are Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right, and Left (the default is BottomLeft).</span></span>
- <span data-ttu-id="cbd06-153">SampleControlId - l'ID di un controllo che visualizza il colore selezionato.</span><span class="sxs-lookup"><span data-stu-id="cbd06-153">SampleControlId - The ID of a control that displays the selected color.</span></span>
- <span data-ttu-id="cbd06-154">SelectedColor - il colore iniziale per ColorPicker selezionato.</span><span class="sxs-lookup"><span data-stu-id="cbd06-154">SelectedColor - The initial color selected by the ColorPicker.</span></span>

<span data-ttu-id="cbd06-155">È possibile utilizzare queste proprietà per personalizzare il modo in cui viene visualizzata la finestra di dialogo di selezione di colore e la modalità di visualizzazione del colore selezionato.</span><span class="sxs-lookup"><span data-stu-id="cbd06-155">You can use these properties to customize how the color picker dialog is displayed and how the selected color is displayed.</span></span> <span data-ttu-id="cbd06-156">La pagina nel listato 3 illustra come è possibile utilizzare alcune di queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="cbd06-156">The page in Listing 3 illustrates how you can use several of these properties.</span></span>

**<span data-ttu-id="cbd06-157">Listato 3 - CreateCardButton.aspx</span><span class="sxs-lookup"><span data-stu-id="cbd06-157">Listing 3 - CreateCardButton.aspx</span></span>**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

<span data-ttu-id="cbd06-158">La pagina nel listato 3 include un colore fate clic sul pulsante (vedere la figura 5).</span><span class="sxs-lookup"><span data-stu-id="cbd06-158">The page in Listing 3 includes a Pick Color button (see Figure 5).</span></span> <span data-ttu-id="cbd06-159">Quando si fa clic su questo pulsante, viene visualizzata la finestra di dialogo di selezione colore sopra la casella di testo.</span><span class="sxs-lookup"><span data-stu-id="cbd06-159">When you click this button, the color picker dialog appears above the TextBox.</span></span> <span data-ttu-id="cbd06-160">Se si seleziona un colore dalla finestra di dialogo colore selezionato viene visualizzato come colore di sfondo per il controllo etichetta lblSample.</span><span class="sxs-lookup"><span data-stu-id="cbd06-160">If you select a color from the dialog then the selected color appears as the background color of the lblSample Label control.</span></span>

<span data-ttu-id="cbd06-161">La proprietà ColorPicker PopupButtonID viene utilizzata per associare il pulsante Seleziona colore con il dispositivo extender ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="cbd06-161">The ColorPicker PopupButtonID property is used to associate the Pick Color button with the ColorPicker extender.</span></span> <span data-ttu-id="cbd06-162">Quando si fornisce un valore per la proprietà PopupButtonID, la finestra di dialogo di selezione colore non compare più quando il controllo di destinazione ha lo stato attivo.</span><span class="sxs-lookup"><span data-stu-id="cbd06-162">When you supply a value for the PopupButtonID property, the color picker dialog no longer appears when the target control has focus.</span></span> <span data-ttu-id="cbd06-163">È necessario fare clic sul pulsante per visualizzare la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="cbd06-163">You must click the button to display the dialog.</span></span>

<span data-ttu-id="cbd06-164">La proprietà SampleControlID viene utilizzata per associare un controllo che visualizza il colore selezionato con ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="cbd06-164">The SampleControlID property is used to associate a control that displays the selected color with the ColorPicker.</span></span> <span data-ttu-id="cbd06-165">ColorPicker di modificare il colore di sfondo di questo controllo attualmente selezionato.</span><span class="sxs-lookup"><span data-stu-id="cbd06-165">The ColorPicker changes the background color of this control to the currently selected color.</span></span>


[![D<span data-ttu-id="cbd06-166">la finestra di dialogo di selezione dei colori con un pulsante IsPlaying]</span><span class="sxs-lookup"><span data-stu-id="cbd06-166">isplaying the color picker dialog with a button]</span></span>(using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

<span data-ttu-id="cbd06-167">**Figura 05**: Visualizzare la finestra di dialogo di selezione dei colori con un pulsante ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-colorpicker-control-extender-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="cbd06-167">**Figure 05**: Displaying the color picker dialog with a button ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image10.png))</span></span>


## <a name="summary"></a><span data-ttu-id="cbd06-168">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="cbd06-168">Summary</span></span>

<span data-ttu-id="cbd06-169">In questa esercitazione è stato descritto come utilizzare il controllo extender ColorPicker per visualizzare una finestra di dialogo popup selezione colore.</span><span class="sxs-lookup"><span data-stu-id="cbd06-169">In this tutorial, you learned how to use the ColorPicker control extender to display a popup color picker dialog.</span></span> <span data-ttu-id="cbd06-170">In primo luogo, abbiamo esaminato come è possibile visualizzare la finestra di dialogo quando lo stato attivo viene spostato in un controllo casella di testo.</span><span class="sxs-lookup"><span data-stu-id="cbd06-170">First, we examined how you can display the dialog when focus is moved to a TextBox control.</span></span> <span data-ttu-id="cbd06-171">Successivamente, si è appreso come creare un pulsante che visualizza la finestra di dialogo di selezione colore quando si fa clic sul pulsante.</span><span class="sxs-lookup"><span data-stu-id="cbd06-171">Next, you learned how to create a button that displays the color picker dialog when the button is clicked.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="cbd06-172">Precedente</span><span class="sxs-lookup"><span data-stu-id="cbd06-172">Previous</span></span>](using-the-colorpicker-control-extender-cs.md)
