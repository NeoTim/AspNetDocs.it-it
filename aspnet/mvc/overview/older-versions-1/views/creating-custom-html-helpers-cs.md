---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Creazione di helper HTML personalizzati (c#) | Microsoft Docs
author: microsoft
description: L'obiettivo di questa esercitazione è illustrare come è possibile creare helper HTML personalizzati che è possibile usare all'interno delle visualizzazioni MVC. Grazie all'uso di HTML Helper...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 23741d7974713102e6ccb46ced5d62ec202505e8
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59400854"
---
# <a name="creating-custom-html-helpers-c"></a>Creazione di helper HTML personalizzati (C#)

by [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> L'obiettivo di questa esercitazione è illustrare come è possibile creare helper HTML personalizzati che è possibile usare all'interno delle visualizzazioni MVC. Grazie all'uso di helper HTML, è possibile ridurre la quantità di noioso digitazione dei tag HTML che è necessario eseguire per creare una pagina HTML standard.


L'obiettivo di questa esercitazione è illustrare come è possibile creare helper HTML personalizzati che è possibile usare all'interno delle visualizzazioni MVC. Grazie all'uso di helper HTML, è possibile ridurre la quantità di noioso digitazione dei tag HTML che è necessario eseguire per creare una pagina HTML standard.

Nella prima parte di questa esercitazione, descriverò alcuni degli helper HTML esistenti incluse con il framework ASP.NET MVC. Successivamente, descrivono due metodi di creazione di helper HTML personalizzati: Spiegherò come creare helper HTML personalizzati mediante la creazione di un metodo statico e la creazione di un metodo di estensione.

## <a name="understanding-html-helpers"></a>Comprendere gli helper HTML

Un HTML Helper è semplicemente un metodo che restituisce una stringa. La stringa può rappresentare qualsiasi tipo di contenuto che desidera. Ad esempio, è possibile utilizzare gli helper HTML per eseguire il rendering di tag HTML standard come HTML `<input>` e `<img>` tag. È inoltre possibile utilizzare gli helper HTML per il rendering del contenuto più complesso, ad esempio un elenco di schede o una tabella HTML di dati del database.

Il framework ASP.NET MVC include il set seguente di helper HTML standard (non un elenco completo):

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

Si consideri, ad esempio, il modulo nel listato 1. Questo modulo viene eseguito il rendering con l'aiuto di due degli helper HTML standard (vedere la figura 1). Questo modulo Usa la `Html.BeginForm()` e `Html.TextBox()` metodi Helper per eseguire il rendering di un semplice form HTML.


[![PAge sottoposto a rendering con gli helper HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Figura 01**: Pagina sottoposta a rendering con gli helper HTML ([fare clic per visualizzare l'immagine con dimensioni normali](creating-custom-html-helpers-cs/_static/image3.png))


**Listato 1: `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

Il metodo Helper Html.BeginForm() viene utilizzato per creare il codice HTML di apertura e chiusura `<form>` tag. Si noti che il `Html.BeginForm()` viene chiamato all'interno di un utilizzo istruzione. L'istruzione using garantisce che il `<form>` tag viene chiuso al termine del tramite blocco.

Se si preferisce, invece di crearne un'usando blocco, è possibile chiamare il metodo Helper Html.EndForm() per chiudere il `<form>` tag. Usare qualsiasi approccio alla creazione di apertura e chiusura `<form>` tag che sembra più intuitivo per l'utente.

Il `Html.TextBox()` metodi di supporto vengono usati nel listato 1 per il rendering HTML `<input>` tag. Se si seleziona HTML nel browser viene visualizzato l'origine HTML nel listato 2. Si noti che l'origine contiene tag HTML standard.

> [!IMPORTANT]
> Si noti che il `Html.TextBox()`-HTML Helper viene eseguito il rendering con `<%= %>` tag anziché `<% %>` tag. Se non si include il segno di uguale, quindi non viene sottoposto a rendering nel browser.

Il framework ASP.NET MVC contiene un piccolo set di supporti. È molto probabilmente, sarà necessario estendere il framework MVC con gli helper HTML personalizzati. La parte restante di questa esercitazione, si descrive due metodi di creazione di helper HTML personalizzati.

**Listato 2: `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>Creazione di helper HTML con metodi statici

Il modo più semplice per creare un nuovo HTML Helper consiste nel creare un metodo statico che restituisce una stringa. Si supponga, ad esempio, che si decide di creare un nuovo HTML Helper che esegue il rendering HTML `<label>` tag. È possibile usare la classe nel listato 2 per eseguire il rendering di un `<label>` .

**Listato 2: `Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

Non ci sono particolari sulla classe nel listato 2. Il `Label()` metodo semplicemente restituisce una stringa.

La visualizzazione dell'indice modificata nel listato 3 Usa il `LabelHelper` per eseguire il rendering HTML `<label>` tag. Si noti che la visualizzazione include un' `<%@ imports %>` direttiva che importa il `Application1.Helpers` dello spazio dei nomi.

**Listato 2: `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Creazione di helper HTML con i metodi di estensione

Se si desidera creare gli helper HTML che funzionano solo come gli helper HTML standard incluso nel framework di MVC ASP.NET necessario creare i metodi di estensione. I metodi di estensione consentono di aggiungere nuovi metodi a una classe esistente. Quando si crea un metodo HTML Helper, aggiungere nuovi metodi alla classe HtmlHelper rappresentata dalla proprietà Html della visualizzazione.

La classe nel listato 3 viene aggiunto un metodo di estensione per il `HtmlHelper` classe denominata `Label()`. Esistono un paio di aspetti che è possibile notare su questa classe. In primo luogo, si noti che la classe è una classe statica. È necessario definire un metodo di estensione con una classe statica.

In secondo luogo, si noti che il primo parametro del `Label()` metodo è preceduto dalla parola chiave `this`. Il primo parametro di un metodo di estensione indica la classe che estende il metodo di estensione.

**Listato 3: `Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Dopo aver creato un metodo di estensione e compilare correttamente l'applicazione, il metodo di estensione viene visualizzata in Intellisense di Visual Studio, ad esempio tutti gli altri metodi di una classe (vedere la figura 2). L'unica differenza è che estensione vengono visualizzati i metodi con un simbolo speciale accanto agli (un'icona di freccia verso il basso).


[![Uaccesso il metodo di estensione Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Figura 02**: Usando il metodo di estensione Html.Label() ([fare clic per visualizzare l'immagine con dimensioni normali](creating-custom-html-helpers-cs/_static/image6.png))


La visualizzazione dell'indice modificata nel listato 4 utilizza il metodo di estensione Html.Label() per eseguire il rendering di tutti i relativi `<label>` tag.

**Listato 4: `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Riepilogo

In questa esercitazione si è appreso due metodi di creazione di helper HTML personalizzati. In primo luogo, si è appreso come creare un oggetto personalizzato `Label()` HTML Helper mediante la creazione di un metodo statico che restituisce una stringa. Successivamente, si è appreso come creare una classe personalizzata `Label()` metodo di HTML Helper mediante la creazione di un metodo di estensione sul `HtmlHelper` classe.

In questa esercitazione, mi sono concentrato sulla creazione di un metodo HTML Helper estremamente semplice. Tenere presente che un HTML Helper può essere complesso desiderato. È possibile compilare gli helper HTML che eseguono il rendering di contenuto avanzato, ad esempio visualizzazioni dell'albero, i menu o tabelle di dati del database.

> [!div class="step-by-step"]
> [Precedente](asp-net-mvc-views-overview-cs.md)
> [Successivo](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
