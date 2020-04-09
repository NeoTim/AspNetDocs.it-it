---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Creazione di helper HTML personalizzati Documenti Microsoft
author: microsoft
description: L'obiettivo di questa esercitazione è illustrare come è possibile creare helper HTML personalizzati che è possibile usare all'interno delle visualizzazioni MVC. Sfruttando HTML Helper...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 7a2e5a5b42aa5bf267a42fef2fcad7022001ce6f
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675335"
---
# <a name="creating-custom-html-helpers-c"></a><span data-ttu-id="a5484-104">Creazione di helper HTML personalizzati (C#)</span><span class="sxs-lookup"><span data-stu-id="a5484-104">Creating Custom HTML Helpers (C#)</span></span>

<span data-ttu-id="a5484-105">da parte [di Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a5484-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="a5484-106">Scarica il PDF</span><span class="sxs-lookup"><span data-stu-id="a5484-106">Download PDF</span></span>](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> <span data-ttu-id="a5484-107">L'obiettivo di questa esercitazione è illustrare come è possibile creare helper HTML personalizzati che è possibile usare all'interno delle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="a5484-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="a5484-108">Sfruttando gli helper HTML, è possibile ridurre la quantità di noiosa digitazione di tag HTML che è necessario eseguire per creare una pagina HTML standard.</span><span class="sxs-lookup"><span data-stu-id="a5484-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="a5484-109">L'obiettivo di questa esercitazione è illustrare come è possibile creare helper HTML personalizzati che è possibile usare all'interno delle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="a5484-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="a5484-110">Sfruttando gli helper HTML, è possibile ridurre la quantità di noiosa digitazione di tag HTML che è necessario eseguire per creare una pagina HTML standard.</span><span class="sxs-lookup"><span data-stu-id="a5484-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="a5484-111">Nella prima parte di questa esercitazione, descrivere alcuni degli helper HTML esistenti inclusi con il framework di MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a5484-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="a5484-112">Successivamente, descrivo due metodi di creazione di helper HTML personalizzati: spiego come creare helper HTML personalizzati creando un metodo statico e creando un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="a5484-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a static method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="a5484-113">Informazioni sugli helper HTML</span><span class="sxs-lookup"><span data-stu-id="a5484-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="a5484-114">Un HTML Helper è solo un metodo che restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="a5484-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="a5484-115">La stringa può rappresentare qualsiasi tipo di contenuto desiderato.</span><span class="sxs-lookup"><span data-stu-id="a5484-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="a5484-116">Ad esempio, è possibile utilizzare gli helper HTML `<input>` `<img>` per eseguire il rendering di tag HTML standard come HTML e tag.</span><span class="sxs-lookup"><span data-stu-id="a5484-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="a5484-117">È inoltre possibile utilizzare gli helper HTML per eseguire il rendering di contenuto più complesso, ad esempio una striscia di schede o una tabella HTML di dati del database.</span><span class="sxs-lookup"><span data-stu-id="a5484-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="a5484-118">Il framework di ASP.NET riferimento MVC include il set di helper HTML standard (questo non è un elenco completo):</span><span class="sxs-lookup"><span data-stu-id="a5484-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="a5484-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="a5484-119">Html.ActionLink()</span></span>
- <span data-ttu-id="a5484-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="a5484-120">Html.BeginForm()</span></span>
- <span data-ttu-id="a5484-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="a5484-121">Html.CheckBox()</span></span>
- <span data-ttu-id="a5484-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="a5484-122">Html.DropDownList()</span></span>
- <span data-ttu-id="a5484-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="a5484-123">Html.EndForm()</span></span>
- <span data-ttu-id="a5484-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="a5484-124">Html.Hidden()</span></span>
- <span data-ttu-id="a5484-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="a5484-125">Html.ListBox()</span></span>
- <span data-ttu-id="a5484-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="a5484-126">Html.Password()</span></span>
- <span data-ttu-id="a5484-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="a5484-127">Html.RadioButton()</span></span>
- <span data-ttu-id="a5484-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="a5484-128">Html.TextArea()</span></span>
- <span data-ttu-id="a5484-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="a5484-129">Html.TextBox()</span></span>

<span data-ttu-id="a5484-130">Si consideri, ad esempio, il modulo nel listato 1.</span><span class="sxs-lookup"><span data-stu-id="a5484-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="a5484-131">Questo form viene eseguito il rendering con l'aiuto di due degli helper HTML standard (vedere Figura 1).</span><span class="sxs-lookup"><span data-stu-id="a5484-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="a5484-132">Questo modulo `Html.BeginForm()` usa `Html.TextBox()` i metodi e Helper per eseguire il rendering di un semplice modulo HTML.</span><span class="sxs-lookup"><span data-stu-id="a5484-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods to render a simple HTML form.</span></span>

<span data-ttu-id="a5484-133">[![Pagina di rendering con helper HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a5484-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="a5484-134">**Figura 01**: Pagina di cui è stato eseguito il rendering con elementi di supporto HTML ([Fare clic per visualizzare l'immagine](creating-custom-html-helpers-cs/_static/image3.png)a dimensioni intere)</span><span class="sxs-lookup"><span data-stu-id="a5484-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image3.png))</span></span>

<span data-ttu-id="a5484-135">**Lista torto 1 –`Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="a5484-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

<span data-ttu-id="a5484-136">Il metodo Helper Di Html.BeginForm() viene utilizzato `<form>` per creare i tag HTML di apertura e chiusura.</span><span class="sxs-lookup"><span data-stu-id="a5484-136">The Html.BeginForm() Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="a5484-137">Si noti che il `Html.BeginForm()` metodo viene chiamato all'interno di un'istruzione using.</span><span class="sxs-lookup"><span data-stu-id="a5484-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="a5484-138">L'istruzione using assicura `<form>` che il tag venga chiuso alla fine del blocco using.</span><span class="sxs-lookup"><span data-stu-id="a5484-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="a5484-139">Se preferite, invece di creare un using block, potete chiamare `<form>` il metodo Helper Html.EndForm() per chiudere il tag.</span><span class="sxs-lookup"><span data-stu-id="a5484-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="a5484-140">Usa qualsiasi approccio per creare `<form>` un tag di apertura e chiusura che ti sembri più intuitivo.</span><span class="sxs-lookup"><span data-stu-id="a5484-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="a5484-141">I `Html.TextBox()` metodi helper vengono utilizzati `<input>` nel listato 1 per eseguire il rendering dei tag HTML.</span><span class="sxs-lookup"><span data-stu-id="a5484-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="a5484-142">Se si seleziona visualizza origine nel browser, l'origine HTML viene visualizzata nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="a5484-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="a5484-143">Si noti che l'origine contiene tag HTML standard.</span><span class="sxs-lookup"><span data-stu-id="a5484-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a5484-144">si noti che il `<%= %>` -HTML `<% %>` Helper viene eseguito il `Html.TextBox()`rendering con tag anziché tag.</span><span class="sxs-lookup"><span data-stu-id="a5484-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="a5484-145">Se non si include il segno di uguale, non viene eseguito il rendering di alcun elemento nel browser.</span><span class="sxs-lookup"><span data-stu-id="a5484-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="a5484-146">Il framework di ASP.NET MVC contiene un piccolo set di helper.</span><span class="sxs-lookup"><span data-stu-id="a5484-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="a5484-147">Molto probabilmente, sarà necessario estendere il framework MVC con helper HTML personalizzati.</span><span class="sxs-lookup"><span data-stu-id="a5484-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="a5484-148">Nella parte restante di questa esercitazione vengono appesi due metodi di creazione di helper HTML personalizzati.</span><span class="sxs-lookup"><span data-stu-id="a5484-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="a5484-149">**Listato 2 –`Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="a5484-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a><span data-ttu-id="a5484-150">Creazione di helper HTML con metodi staticiCreating HTML Helpers with Static Methods</span><span class="sxs-lookup"><span data-stu-id="a5484-150">Creating HTML Helpers with Static Methods</span></span>

<span data-ttu-id="a5484-151">Il modo più semplice per creare un nuovo helper HTML consiste nel creare un metodo statico che restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="a5484-151">The easiest way to create a new HTML Helper is to create a static method that returns a string.</span></span> <span data-ttu-id="a5484-152">Si supponga, ad esempio, che si decide di `<label>` creare un nuovo helper HTML che esegue il rendering di un tag HTML.</span><span class="sxs-lookup"><span data-stu-id="a5484-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="a5484-153">È possibile utilizzare la classe nel `<label>` listato 2 per eseguire il rendering di un file .</span><span class="sxs-lookup"><span data-stu-id="a5484-153">You can use the class in Listing 2 to render a `<label>` .</span></span>

<span data-ttu-id="a5484-154">**Listato 2 –`Helpers\LabelHelper.cs`**</span><span class="sxs-lookup"><span data-stu-id="a5484-154">**Listing 2 – `Helpers\LabelHelper.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

<span data-ttu-id="a5484-155">Non c'è niente di speciale nella classe nella lista 2.</span><span class="sxs-lookup"><span data-stu-id="a5484-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="a5484-156">Il `Label()` metodo restituisce semplicemente una stringa.</span><span class="sxs-lookup"><span data-stu-id="a5484-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="a5484-157">La vista Indice modificata `LabelHelper` nel listato 3 utilizza la per eseguire il rendering dei tag HTML. `<label>`</span><span class="sxs-lookup"><span data-stu-id="a5484-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="a5484-158">Si noti che `<%@ imports %>` la visualizzazione `Application1.Helpers` include una direttiva che importa lo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="a5484-158">Notice that the view includes an `<%@ imports %>` directive that imports the `Application1.Helpers` namespace.</span></span>

<span data-ttu-id="a5484-159">**Listato 2 –`Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="a5484-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="a5484-160">Creazione di helper HTML con metodi di estensioneCreating HTML Helpers with Extension Methods</span><span class="sxs-lookup"><span data-stu-id="a5484-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="a5484-161">Se si desidera creare helper HTML che funzionano come gli helper HTML standard inclusi nel framework di ASP.NET MVC, è necessario creare metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="a5484-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="a5484-162">I metodi di estensione consentono di aggiungere nuovi metodi a una classe esistente.</span><span class="sxs-lookup"><span data-stu-id="a5484-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="a5484-163">Quando si crea un metodo HTML Helper, si aggiungono nuovi metodi alla classe HtmlHelper rappresentata dalla proprietà Html di una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="a5484-163">When creating an HTML Helper method, you add new methods to the HtmlHelper class represented by a view's Html property.</span></span>

<span data-ttu-id="a5484-164">La classe nel listato 3 `HtmlHelper` aggiunge `Label()`un metodo di estensione alla classe denominata .</span><span class="sxs-lookup"><span data-stu-id="a5484-164">The class in Listing 3 adds an extension method to the `HtmlHelper` class named `Label()`.</span></span> <span data-ttu-id="a5484-165">Ci sono un paio di cose che dovresti notare su questa classe.</span><span class="sxs-lookup"><span data-stu-id="a5484-165">There are a couple of things that you should notice about this class.</span></span> <span data-ttu-id="a5484-166">In primo luogo, si noti che la classe è una classe statica.</span><span class="sxs-lookup"><span data-stu-id="a5484-166">First, notice that the class is a static class.</span></span> <span data-ttu-id="a5484-167">È necessario definire un metodo di estensione con una classe statica.</span><span class="sxs-lookup"><span data-stu-id="a5484-167">You must define an extension method with a static class.</span></span>

<span data-ttu-id="a5484-168">In secondo luogo, si `Label()` noti che il `this`primo parametro del metodo è preceduto dalla parola chiave .</span><span class="sxs-lookup"><span data-stu-id="a5484-168">Second, notice that the first parameter of the `Label()` method is preceded by the keyword `this`.</span></span> <span data-ttu-id="a5484-169">Il primo parametro di un metodo di estensione indica la classe che il metodo di estensione estende.</span><span class="sxs-lookup"><span data-stu-id="a5484-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="a5484-170">**Lista 3 –`Helpers\LabelExtensions.cs`**</span><span class="sxs-lookup"><span data-stu-id="a5484-170">**Listing 3 – `Helpers\LabelExtensions.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

<span data-ttu-id="a5484-171">Dopo aver creato un metodo di estensione e compilare correttamente l'applicazione, il metodo di estensione viene visualizzato in Visual Studio Intellisense come tutti gli altri metodi di una classe (vedere Figura 2).</span><span class="sxs-lookup"><span data-stu-id="a5484-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="a5484-172">L'unica differenza è che i metodi di estensione vengono visualizzati con un simbolo speciale accanto a loro (un'icona di una freccia verso il basso).</span><span class="sxs-lookup"><span data-stu-id="a5484-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>

<span data-ttu-id="a5484-173">[![Utilizzo del metodo di estensione Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="a5484-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span></span>

<span data-ttu-id="a5484-174">**Figura 02**: Utilizzo del metodo di estensione Html.Label() ([Fare clic per visualizzare l'immagine](creating-custom-html-helpers-cs/_static/image6.png)a dimensioni intere )</span><span class="sxs-lookup"><span data-stu-id="a5484-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image6.png))</span></span>

<span data-ttu-id="a5484-175">La vista Index modificata nel listato 4 utilizza il `<label>` metodo di estensione Html.Label() per eseguire il rendering di tutti i relativi tag.</span><span class="sxs-lookup"><span data-stu-id="a5484-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its `<label>` tags.</span></span>

<span data-ttu-id="a5484-176">**Lista 4 –`Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="a5484-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="a5484-177">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="a5484-177">Summary</span></span>

<span data-ttu-id="a5484-178">In questa esercitazione sono stati acquisiti due metodi per creare helper HTML personalizzati.</span><span class="sxs-lookup"><span data-stu-id="a5484-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="a5484-179">In primo luogo, si `Label()` è appreso come creare un helper HTML personalizzato creando un metodo statico che restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="a5484-179">First, you learned how to create a custom `Label()` HTML Helper by creating a static method that returns a string.</span></span> <span data-ttu-id="a5484-180">Successivamente, hai imparato a `Label()` creare un metodo helper HTML `HtmlHelper` personalizzato creando un metodo di estensione nella classe.</span><span class="sxs-lookup"><span data-stu-id="a5484-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="a5484-181">In questo tutorial, mi sono concentrato sulla creazione di un metodo HTML Helper estremamente semplice.</span><span class="sxs-lookup"><span data-stu-id="a5484-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="a5484-182">Rendersi conto che un HTML Helper può essere complicato come si desidera.</span><span class="sxs-lookup"><span data-stu-id="a5484-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="a5484-183">È possibile creare helper HTML che eseguono il rendering di contenuto avanzato, ad esempio visualizzazioni struttura ad albero, menu o tabelle di dati del database.</span><span class="sxs-lookup"><span data-stu-id="a5484-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a5484-184">[Successivo](asp-net-mvc-views-overview-cs.md)
> [precedente](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span><span class="sxs-lookup"><span data-stu-id="a5484-184">[Previous](asp-net-mvc-views-overview-cs.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span></span>
