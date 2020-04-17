---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Creazione di helper HTML personalizzati Documenti Microsoft
author: rick-anderson
description: L'obiettivo di questa esercitazione è illustrare come è possibile creare helper HTML personalizzati che è possibile usare all'interno delle visualizzazioni MVC. Sfruttando HTML Helper...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 82e4118fd404051b891489b62d531169e83f450d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542560"
---
# <a name="creating-custom-html-helpers-c"></a>Creazione di helper HTML personalizzati (C#)

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> L'obiettivo di questa esercitazione è illustrare come è possibile creare helper HTML personalizzati che è possibile usare all'interno delle visualizzazioni MVC. Sfruttando gli helper HTML, è possibile ridurre la quantità di noiosa digitazione di tag HTML che è necessario eseguire per creare una pagina HTML standard.

L'obiettivo di questa esercitazione è illustrare come è possibile creare helper HTML personalizzati che è possibile usare all'interno delle visualizzazioni MVC. Sfruttando gli helper HTML, è possibile ridurre la quantità di noiosa digitazione di tag HTML che è necessario eseguire per creare una pagina HTML standard.

Nella prima parte di questa esercitazione, descrivere alcuni degli helper HTML esistenti inclusi con il framework di MVC ASP.NET. Successivamente, descrivo due metodi di creazione di helper HTML personalizzati: spiego come creare helper HTML personalizzati creando un metodo statico e creando un metodo di estensione.

## <a name="understanding-html-helpers"></a>Informazioni sugli helper HTML

Un HTML Helper è solo un metodo che restituisce una stringa. La stringa può rappresentare qualsiasi tipo di contenuto desiderato. Ad esempio, è possibile utilizzare gli helper HTML `<input>` `<img>` per eseguire il rendering di tag HTML standard come HTML e tag. È inoltre possibile utilizzare gli helper HTML per eseguire il rendering di contenuto più complesso, ad esempio una striscia di schede o una tabella HTML di dati del database.

Il framework di ASP.NET riferimento MVC include il set di helper HTML standard (questo non è un elenco completo):

- Html.ActionLink()
- Html.BeginForm()
- Html.CheckBox()
- Html.DropDownList()
- Html.EndForm()
- Html.Hidden()
- Html.ListBox()
- Html.Password()
- Html.RadioButton()
- Html.TextArea()
- Html.TextBox()

Si consideri, ad esempio, il modulo nel listato 1. Questo form viene eseguito il rendering con l'aiuto di due degli helper HTML standard (vedere Figura 1). Questo modulo `Html.BeginForm()` usa `Html.TextBox()` i metodi e Helper per eseguire il rendering di un semplice modulo HTML.

[![Pagina di rendering con helper HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Figura 01**: Pagina di cui è stato eseguito il rendering con elementi di supporto HTML ([Fare clic per visualizzare l'immagine](creating-custom-html-helpers-cs/_static/image3.png)a dimensioni intere)

**Lista torto 1 –`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

Il metodo Helper Di Html.BeginForm() viene utilizzato `<form>` per creare i tag HTML di apertura e chiusura. Si noti che il `Html.BeginForm()` metodo viene chiamato all'interno di un'istruzione using. L'istruzione using assicura `<form>` che il tag venga chiuso alla fine del blocco using.

Se preferite, invece di creare un using block, potete chiamare `<form>` il metodo Helper Html.EndForm() per chiudere il tag. Usa qualsiasi approccio per creare `<form>` un tag di apertura e chiusura che ti sembri più intuitivo.

I `Html.TextBox()` metodi helper vengono utilizzati `<input>` nel listato 1 per eseguire il rendering dei tag HTML. Se si seleziona visualizza origine nel browser, l'origine HTML viene visualizzata nel listato 2. Si noti che l'origine contiene tag HTML standard.

> [!IMPORTANT]
> si noti che il `<%= %>` -HTML `<% %>` Helper viene eseguito il `Html.TextBox()`rendering con tag anziché tag. Se non si include il segno di uguale, non viene eseguito il rendering di alcun elemento nel browser.

Il framework di ASP.NET MVC contiene un piccolo set di helper. Molto probabilmente, sarà necessario estendere il framework MVC con helper HTML personalizzati. Nella parte restante di questa esercitazione vengono appesi due metodi di creazione di helper HTML personalizzati.

**Listato 2 –`Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>Creazione di helper HTML con metodi staticiCreating HTML Helpers with Static Methods

Il modo più semplice per creare un nuovo helper HTML consiste nel creare un metodo statico che restituisce una stringa. Si supponga, ad esempio, che si decide di `<label>` creare un nuovo helper HTML che esegue il rendering di un tag HTML. È possibile utilizzare la classe nel `<label>` listato 2 per eseguire il rendering di un file .

**Listato 2 –`Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

Non c'è niente di speciale nella classe nella lista 2. Il `Label()` metodo restituisce semplicemente una stringa.

La vista Indice modificata `LabelHelper` nel listato 3 utilizza la per eseguire il rendering dei tag HTML. `<label>` Si noti che `<%@ imports %>` la visualizzazione `Application1.Helpers` include una direttiva che importa lo spazio dei nomi.

**Listato 2 –`Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Creazione di helper HTML con metodi di estensioneCreating HTML Helpers with Extension Methods

Se si desidera creare helper HTML che funzionano come gli helper HTML standard inclusi nel framework di ASP.NET MVC, è necessario creare metodi di estensione. I metodi di estensione consentono di aggiungere nuovi metodi a una classe esistente. Quando si crea un metodo HTML Helper, si aggiungono nuovi metodi alla classe HtmlHelper rappresentata dalla proprietà Html di una visualizzazione.

La classe nel listato 3 `HtmlHelper` aggiunge `Label()`un metodo di estensione alla classe denominata . Ci sono un paio di cose che dovresti notare su questa classe. In primo luogo, si noti che la classe è una classe statica. È necessario definire un metodo di estensione con una classe statica.

In secondo luogo, si `Label()` noti che il `this`primo parametro del metodo è preceduto dalla parola chiave . Il primo parametro di un metodo di estensione indica la classe che il metodo di estensione estende.

**Lista 3 –`Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Dopo aver creato un metodo di estensione e compilare correttamente l'applicazione, il metodo di estensione viene visualizzato in Visual Studio Intellisense come tutti gli altri metodi di una classe (vedere Figura 2). L'unica differenza è che i metodi di estensione vengono visualizzati con un simbolo speciale accanto a loro (un'icona di una freccia verso il basso).

[![Utilizzo del metodo di estensione Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Figura 02**: Utilizzo del metodo di estensione Html.Label() ([Fare clic per visualizzare l'immagine](creating-custom-html-helpers-cs/_static/image6.png)a dimensioni intere )

La vista Index modificata nel listato 4 utilizza il `<label>` metodo di estensione Html.Label() per eseguire il rendering di tutti i relativi tag.

**Lista 4 –`Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Riepilogo

In questa esercitazione sono stati acquisiti due metodi per creare helper HTML personalizzati. In primo luogo, si `Label()` è appreso come creare un helper HTML personalizzato creando un metodo statico che restituisce una stringa. Successivamente, hai imparato a `Label()` creare un metodo helper HTML `HtmlHelper` personalizzato creando un metodo di estensione nella classe.

In questo tutorial, mi sono concentrato sulla creazione di un metodo HTML Helper estremamente semplice. Rendersi conto che un HTML Helper può essere complicato come si desidera. È possibile creare helper HTML che eseguono il rendering di contenuto avanzato, ad esempio visualizzazioni struttura ad albero, menu o tabelle di dati del database.

> [!div class="step-by-step"]
> [Successivo](asp-net-mvc-views-overview-cs.md)
> [precedente](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
