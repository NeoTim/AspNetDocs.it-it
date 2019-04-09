---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Rendering Web ASP.NET (Razor) siti con pagine per i dispositivi mobili | Microsoft Docs
author: Rick-Anderson
description: 'Questo articolo descrive come creare le pagine in un sito di ASP.NET Web Pages (Razor) che verrà eseguito il rendering in modo appropriato nei dispositivi mobili. Che cosa si apprenderà come: Come è...'
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: dbcd25331387f8606343e551302bc3ed1f9b2c25
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59379508"
---
# <a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Il rendering di ASP.NET Web Pages (Razor) siti per dispositivi mobili

da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo descrive come creare le pagine in un sito di ASP.NET Web Pages (Razor) che verrà eseguito il rendering in modo appropriato nei dispositivi mobili.
> 
> Che cosa si apprenderà come:
> 
> - Come usare una convenzione di denominazione per specificare che una pagina è progettato specificamente per i dispositivi mobili.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


ASP.NET Web Pages consente di creare visualizzazioni personalizzate per il rendering del contenuto in altri dispositivi o per dispositivi mobili.

Il modo più semplice per creare specifiche del dispositivo pagina in un sito di ASP.NET Web Pages consiste nell'usare un modello di denominazione file simile al seguente: *FileName.Mobile.cshtml*. È possibile creare due versioni di una pagina (ad esempio, una denominata *MyFile.cshtml* e uno denominato *MyFile.Mobile.cshtml*). In fase di esecuzione, quando un dispositivo mobile richiede *MyFile.cshtml*, ASP.NET esegue il rendering del contenuto dalla *MyFile.Mobile.cshtml*. In caso contrario, *MyFile.cshtml* viene eseguito il rendering.

Nell'esempio seguente viene illustrato come abilitare il rendering per dispositivi mobili tramite l'aggiunta di una pagina di contenuto per i dispositivi mobili. *Page1.cshtml* contiene contenuto oltre a una barra laterale di navigazione. *Page1.Mobile.cshtml* lo stesso contenuto, ma omette l'intestazione laterale.

1. In un sito di ASP.NET Web Pages, creare un file denominato *Page1.cshtml* e sostituire il contenuto corrente con il markup seguente.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Creare un file denominato *Page1.Mobile.cshtml* e sostituire il contenuto esistente con il markup seguente. Si noti che la versione mobile della pagina omette la sezione di spostamento per una migliore riproduzione su uno schermo piccolo.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Eseguire un browser desktop e passare alla *Page1.cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Eseguire un browser per dispositivi mobili (o un emulatore di dispositivi mobili) e passare alla *Page1.cshtml*. (Si noti che non si include *. Mobile.* come parte dell'URL.) Anche se la richiesta è su *Page1.cshtml*, ASP.NET esegue il rendering *Page1.Mobile.cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Per testare le pagine per dispositivi mobili, è possibile usare un simulatore di dispositivi mobili che viene eseguito in un computer desktop. Questo strumento consente di testare le pagine web come appaiono nei dispositivi mobili (vale a dire, in genere con un notevolmente ridotta visualizzare l'area). Un esempio di un simulatore è il [componente aggiuntivo di selezione dell'agente utente](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) per Mozilla Firefox, che consente di emulare diversi browser per dispositivi mobili da una versione desktop di Firefox.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


[Emulatore Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
