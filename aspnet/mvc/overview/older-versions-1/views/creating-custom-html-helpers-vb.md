---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Creazione di helper HTML personalizzati (VB) Documenti Microsoft
author: rick-anderson
description: L'obiettivo di questa esercitazione è illustrare come è possibile creare helper HTML personalizzati che è possibile usare all'interno delle visualizzazioni MVC. Sfruttando HTML Helper...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 52f16bf666edc9f1c95c01faf004e303fcb6a0b2
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542508"
---
# <a name="creating-custom-html-helpers-vb"></a><span data-ttu-id="c6fde-104">Creazione di helper HTML personalizzati (VB)</span><span class="sxs-lookup"><span data-stu-id="c6fde-104">Creating Custom HTML Helpers (VB)</span></span>

<span data-ttu-id="c6fde-105">da parte [di Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c6fde-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="c6fde-106">Scarica il PDF</span><span class="sxs-lookup"><span data-stu-id="c6fde-106">Download PDF</span></span>](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> <span data-ttu-id="c6fde-107">L'obiettivo di questa esercitazione è illustrare come è possibile creare helper HTML personalizzati che è possibile usare all'interno delle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="c6fde-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="c6fde-108">Sfruttando gli helper HTML, è possibile ridurre la quantità di noiosa digitazione di tag HTML che è necessario eseguire per creare una pagina HTML standard.</span><span class="sxs-lookup"><span data-stu-id="c6fde-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="c6fde-109">L'obiettivo di questa esercitazione è illustrare come è possibile creare helper HTML personalizzati che è possibile usare all'interno delle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="c6fde-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="c6fde-110">Sfruttando gli helper HTML, è possibile ridurre la quantità di noiosa digitazione di tag HTML che è necessario eseguire per creare una pagina HTML standard.</span><span class="sxs-lookup"><span data-stu-id="c6fde-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="c6fde-111">Nella prima parte di questa esercitazione, descrivere alcuni degli helper HTML esistenti inclusi con il framework di MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c6fde-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="c6fde-112">Successivamente, descrivo due metodi di creazione di helper HTML personalizzati: spiego come creare helper HTML personalizzati creando un metodo condiviso e creando un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="c6fde-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a shared method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="c6fde-113">Informazioni sugli helper HTML</span><span class="sxs-lookup"><span data-stu-id="c6fde-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="c6fde-114">Un HTML Helper è solo un metodo che restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="c6fde-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="c6fde-115">La stringa può rappresentare qualsiasi tipo di contenuto desiderato.</span><span class="sxs-lookup"><span data-stu-id="c6fde-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="c6fde-116">Ad esempio, è possibile utilizzare gli helper HTML `<input>` `<img>` per eseguire il rendering di tag HTML standard come HTML e tag.</span><span class="sxs-lookup"><span data-stu-id="c6fde-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="c6fde-117">È inoltre possibile utilizzare gli helper HTML per eseguire il rendering di contenuto più complesso, ad esempio una striscia di schede o una tabella HTML di dati del database.</span><span class="sxs-lookup"><span data-stu-id="c6fde-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="c6fde-118">Il framework di ASP.NET riferimento MVC include il set di helper HTML standard (questo non è un elenco completo):</span><span class="sxs-lookup"><span data-stu-id="c6fde-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="c6fde-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="c6fde-119">Html.ActionLink()</span></span>
- <span data-ttu-id="c6fde-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="c6fde-120">Html.BeginForm()</span></span>
- <span data-ttu-id="c6fde-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="c6fde-121">Html.CheckBox()</span></span>
- <span data-ttu-id="c6fde-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="c6fde-122">Html.DropDownList()</span></span>
- <span data-ttu-id="c6fde-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="c6fde-123">Html.EndForm()</span></span>
- <span data-ttu-id="c6fde-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="c6fde-124">Html.Hidden()</span></span>
- <span data-ttu-id="c6fde-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="c6fde-125">Html.ListBox()</span></span>
- <span data-ttu-id="c6fde-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="c6fde-126">Html.Password()</span></span>
- <span data-ttu-id="c6fde-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="c6fde-127">Html.RadioButton()</span></span>
- <span data-ttu-id="c6fde-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="c6fde-128">Html.TextArea()</span></span>
- <span data-ttu-id="c6fde-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="c6fde-129">Html.TextBox()</span></span>

<span data-ttu-id="c6fde-130">Si consideri, ad esempio, il modulo nel listato 1.</span><span class="sxs-lookup"><span data-stu-id="c6fde-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="c6fde-131">Questo form viene eseguito il rendering con l'aiuto di due degli helper HTML standard (vedere Figura 1).</span><span class="sxs-lookup"><span data-stu-id="c6fde-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="c6fde-132">Questo modulo `Html.BeginForm()` utilizza `Html.TextBox()` i metodi e Helper.</span><span class="sxs-lookup"><span data-stu-id="c6fde-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods.</span></span>

<span data-ttu-id="c6fde-133">[![Pagina di rendering con helper HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c6fde-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="c6fde-134">**Figura 01**: Pagina di cui è stato eseguito il rendering con elementi di supporto HTML ([Fare clic per visualizzare l'immagine](creating-custom-html-helpers-vb/_static/image3.png)a dimensioni intere)</span><span class="sxs-lookup"><span data-stu-id="c6fde-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image3.png))</span></span>

<span data-ttu-id="c6fde-135">**Lista torto 1 –`Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="c6fde-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

<span data-ttu-id="c6fde-136">Il `Html.BeginForm()` metodo Helper viene utilizzato per `<form>` creare i tag HTML di apertura e chiusura.</span><span class="sxs-lookup"><span data-stu-id="c6fde-136">The `Html.BeginForm()` Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="c6fde-137">Si noti che il `Html.BeginForm()` metodo viene chiamato all'interno di un'istruzione using.</span><span class="sxs-lookup"><span data-stu-id="c6fde-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="c6fde-138">L'istruzione using assicura `<form>` che il tag venga chiuso alla fine del blocco using.</span><span class="sxs-lookup"><span data-stu-id="c6fde-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="c6fde-139">Se preferite, invece di creare un using block, potete chiamare `<form>` il metodo Helper Html.EndForm() per chiudere il tag.</span><span class="sxs-lookup"><span data-stu-id="c6fde-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="c6fde-140">Usa qualsiasi approccio per creare `<form>` un tag di apertura e chiusura che ti sembri più intuitivo.</span><span class="sxs-lookup"><span data-stu-id="c6fde-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="c6fde-141">I `Html.TextBox()` metodi helper vengono utilizzati `<input>` nel listato 1 per eseguire il rendering dei tag HTML.</span><span class="sxs-lookup"><span data-stu-id="c6fde-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="c6fde-142">Se si seleziona visualizza origine nel browser, l'origine HTML viene visualizzata nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="c6fde-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="c6fde-143">Si noti che l'origine contiene tag HTML standard.</span><span class="sxs-lookup"><span data-stu-id="c6fde-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c6fde-144">si noti che il `<%= %>` -HTML `<% %>` Helper viene eseguito il `Html.TextBox()`rendering con tag anziché tag.</span><span class="sxs-lookup"><span data-stu-id="c6fde-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="c6fde-145">Se non si include il segno di uguale, non viene eseguito il rendering di alcun elemento nel browser.</span><span class="sxs-lookup"><span data-stu-id="c6fde-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="c6fde-146">Il framework di ASP.NET MVC contiene un piccolo set di helper.</span><span class="sxs-lookup"><span data-stu-id="c6fde-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="c6fde-147">Molto probabilmente, sarà necessario estendere il framework MVC con helper HTML personalizzati.</span><span class="sxs-lookup"><span data-stu-id="c6fde-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="c6fde-148">Nella parte restante di questa esercitazione vengono appesi due metodi di creazione di helper HTML personalizzati.</span><span class="sxs-lookup"><span data-stu-id="c6fde-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="c6fde-149">**Listato 2 –`Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="c6fde-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a><span data-ttu-id="c6fde-150">Creazione di helper HTML con metodi condivisiCreating HTML Helpers with Shared Methods</span><span class="sxs-lookup"><span data-stu-id="c6fde-150">Creating HTML Helpers with Shared Methods</span></span>

<span data-ttu-id="c6fde-151">Il modo più semplice per creare un nuovo helper HTML consiste nel creare un metodo condiviso che restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="c6fde-151">The easiest way to create a new HTML Helper is to create a shared method that returns a string.</span></span> <span data-ttu-id="c6fde-152">Si supponga, ad esempio, che si decide di `<label>` creare un nuovo helper HTML che esegue il rendering di un tag HTML.</span><span class="sxs-lookup"><span data-stu-id="c6fde-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="c6fde-153">È possibile utilizzare la classe nel `<label>`listato 2 per eseguire il rendering di un file .</span><span class="sxs-lookup"><span data-stu-id="c6fde-153">You can use the class in Listing 2 to render a `<label>`.</span></span>

<span data-ttu-id="c6fde-154">**Listato 2 –`Helpers\LabelHelper.vb`**</span><span class="sxs-lookup"><span data-stu-id="c6fde-154">**Listing 2 – `Helpers\LabelHelper.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

<span data-ttu-id="c6fde-155">Non c'è niente di speciale nella classe nella lista 2.</span><span class="sxs-lookup"><span data-stu-id="c6fde-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="c6fde-156">Il `Label()` metodo restituisce semplicemente una stringa.</span><span class="sxs-lookup"><span data-stu-id="c6fde-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="c6fde-157">La vista Indice modificata `LabelHelper` nel listato 3 utilizza la per eseguire il rendering dei tag HTML. `<label>`</span><span class="sxs-lookup"><span data-stu-id="c6fde-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="c6fde-158">Si noti che `<%@ imports %>` la visualizzazione include una direttiva che importa lo spazio dei nomi Application1.Helpers.Notice that the view includes an directive that imports the Application1.Helpers namespace.</span><span class="sxs-lookup"><span data-stu-id="c6fde-158">Notice that the view includes an `<%@ imports %>` directive that imports the Application1.Helpers namespace.</span></span>

<span data-ttu-id="c6fde-159">**Listato 2 –`Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="c6fde-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="c6fde-160">Creazione di helper HTML con metodi di estensioneCreating HTML Helpers with Extension Methods</span><span class="sxs-lookup"><span data-stu-id="c6fde-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="c6fde-161">Se si desidera creare helper HTML che funzionano come gli helper HTML standard inclusi nel framework di ASP.NET MVC, è necessario creare metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="c6fde-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="c6fde-162">I metodi di estensione consentono di aggiungere nuovi metodi a una classe esistente.</span><span class="sxs-lookup"><span data-stu-id="c6fde-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="c6fde-163">Quando si crea un metodo HTML Helper, si aggiungono nuovi metodi alla `HtmlHelper` classe rappresentata dalla proprietà Html di una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c6fde-163">When creating an HTML Helper method, you add new methods to the `HtmlHelper` class represented by a view's Html property.</span></span>

<span data-ttu-id="c6fde-164">Il modulo di Visual Basic nel `Label()` listato `HtmlHelper` 3 aggiunge un metodo di estensione denominato alla classe.</span><span class="sxs-lookup"><span data-stu-id="c6fde-164">The Visual Basic module in Listing 3 adds an extension method named `Label()` to the `HtmlHelper` class.</span></span> <span data-ttu-id="c6fde-165">Ci sono un paio di cose che dovresti notare su questo modulo.</span><span class="sxs-lookup"><span data-stu-id="c6fde-165">There are a couple of things that you should notice about this module.</span></span> <span data-ttu-id="c6fde-166">In primo luogo, si noti che il modulo è decorato con l'attributo. `<Extension()>`</span><span class="sxs-lookup"><span data-stu-id="c6fde-166">First, notice that the module is decorated with the `<Extension()>` attribute.</span></span> <span data-ttu-id="c6fde-167">Per utilizzare questo attributo, è `System.Runtime.CompilerServices` necessario importare lo spazio dei nomi</span><span class="sxs-lookup"><span data-stu-id="c6fde-167">In order to use this attribute, you must import the `System.Runtime.CompilerServices` namespace</span></span>

<span data-ttu-id="c6fde-168">In secondo luogo, si `Label()` noti `HtmlHelper` che il primo parametro del metodo rappresenta la classe.</span><span class="sxs-lookup"><span data-stu-id="c6fde-168">Second, notice that the first parameter of the `Label()` method represents the `HtmlHelper` class.</span></span> <span data-ttu-id="c6fde-169">Il primo parametro di un metodo di estensione indica la classe che il metodo di estensione estende.</span><span class="sxs-lookup"><span data-stu-id="c6fde-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="c6fde-170">**Lista 3 –`Helpers\LabelExtensions.vb`**</span><span class="sxs-lookup"><span data-stu-id="c6fde-170">**Listing 3 – `Helpers\LabelExtensions.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

<span data-ttu-id="c6fde-171">Dopo aver creato un metodo di estensione e compilare correttamente l'applicazione, il metodo di estensione viene visualizzato in Visual Studio Intellisense come tutti gli altri metodi di una classe (vedere Figura 2).</span><span class="sxs-lookup"><span data-stu-id="c6fde-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="c6fde-172">L'unica differenza è che i metodi di estensione vengono visualizzati con un simbolo speciale accanto a loro (un'icona di una freccia verso il basso).</span><span class="sxs-lookup"><span data-stu-id="c6fde-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>

<span data-ttu-id="c6fde-173">[![Utilizzo del metodo di estensione Html.Label()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="c6fde-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span></span>

<span data-ttu-id="c6fde-174">**Figura 02**: Utilizzo del metodo di estensione Html.Label() ([Fare clic per visualizzare l'immagine](creating-custom-html-helpers-vb/_static/image6.png)a dimensioni intere )</span><span class="sxs-lookup"><span data-stu-id="c6fde-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image6.png))</span></span>

<span data-ttu-id="c6fde-175">La vista Index modificata nel listato 4 utilizza il &lt;metodo&gt; di estensione Html.Label() per eseguire il rendering di tutti i tag etichetta.</span><span class="sxs-lookup"><span data-stu-id="c6fde-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its &lt;label&gt; tags.</span></span>

<span data-ttu-id="c6fde-176">**Lista 4 –`Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="c6fde-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="c6fde-177">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="c6fde-177">Summary</span></span>

<span data-ttu-id="c6fde-178">In questa esercitazione sono stati acquisiti due metodi per creare helper HTML personalizzati.</span><span class="sxs-lookup"><span data-stu-id="c6fde-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="c6fde-179">In primo luogo, hai `Label()` imparato a creare un helper HTML personalizzato creando un metodo condiviso che restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="c6fde-179">First, you learned how to create a custom `Label()` HTML Helper by creating a shared method that returns a string.</span></span> <span data-ttu-id="c6fde-180">Successivamente, hai imparato a `Label()` creare un metodo helper HTML `HtmlHelper` personalizzato creando un metodo di estensione nella classe.</span><span class="sxs-lookup"><span data-stu-id="c6fde-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="c6fde-181">In questo tutorial, mi sono concentrato sulla creazione di un metodo HTML Helper estremamente semplice.</span><span class="sxs-lookup"><span data-stu-id="c6fde-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="c6fde-182">Rendersi conto che un HTML Helper può essere complicato come si desidera.</span><span class="sxs-lookup"><span data-stu-id="c6fde-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="c6fde-183">È possibile creare helper HTML che eseguono il rendering di contenuto avanzato, ad esempio visualizzazioni struttura ad albero, menu o tabelle di dati del database.</span><span class="sxs-lookup"><span data-stu-id="c6fde-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c6fde-184">[Successivo](asp-net-mvc-views-overview-vb.md)
> [precedente](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c6fde-184">[Previous](asp-net-mvc-views-overview-vb.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span></span>
