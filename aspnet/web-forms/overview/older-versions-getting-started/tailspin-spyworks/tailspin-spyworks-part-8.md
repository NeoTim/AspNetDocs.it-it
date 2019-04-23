---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Parte 8: Pagine finali, gestione delle eccezioni e conclusione | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 8 aggiunge una pagina di contatto, sulla pagina e l'eccezione...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: db8db4e3bff8047b48a7528b5146873ab6d84714
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59398683"
---
# <a name="part-8-final-pages-exception-handling-and-conclusion"></a>Parte 8: Pagine finali, gestione delle eccezioni e conclusione

da [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks viene illustrato come saliente è davvero semplice per creare applicazioni potenti e scalabili per la piattaforma .NET. Illustra come usare le nuove funzionalità in ASP.NET 4 per creare un negozio online, tra cui acquisti, estrazione e l'amministrazione.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 8 aggiunge una pagina di contatto, sulla pagina e la gestione delle eccezioni. Si tratta la conclusione della serie.


## <a id="_Toc260221680"></a>  Contattare pagina (invio messaggio di posta elettronica da ASP.NET)

Creare una nuova pagina denominata ContactUs.aspx

Usando la finestra di progettazione, creare il formato seguente, preso nota speciale per includere il ToolkitScriptManager e il controllo dell'Editor dal AjaxControlToolkit. .

![](tailspin-spyworks-part-8/_static/image1.jpg)

Fare doppio clic sul pulsante "Invia" per generare un gestore eventi click nel file dietro il codice e implementare un metodo per inviare le informazioni di contatto come messaggio di posta elettronica.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Questo codice si presuppone che il file Web. config contenga una voce nella sezione di configurazione che specifica il server SMTP da utilizzare per l'invio di posta elettronica.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  Informazioni sulla pagina

Creare una pagina denominata AboutUs. aspx e aggiungere il contenuto desiderato.

## <a id="_Toc260221682"></a>  Gestore eccezioni globale

Infine, in tutta l'applicazione è stato generato l'eccezione e vi sono circostanze impreviste che a freddo anche le eccezioni non gestita causa nella nostra applicazione web.

Non vogliamo mai un'eccezione non gestita da visualizzare per un visitatore di un sito web.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Oltre ad avere un'esperienza utente terribili le eccezioni non gestite possono anche essere un problema di sicurezza.

Per risolvere questo problema verrà implementato un gestore eccezioni globale.

A tale scopo, aprire il file Global. asax e notare il seguente gestore eventi generati in precedenza.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Aggiungere il codice per implementare l'applicazione\_gestore errori come indicato di seguito.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Quindi aggiungere una pagina denominata aspx alla soluzione e aggiungere questo frammento di markup.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Ora nella pagina\_caricare extract di gestore eventi i messaggi di errore dall'oggetto Request.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  Conclusioni

Abbiamo visto che Web Form ASP.NET rende più semplice per creare un sito Web sofisticate con accesso al database, l'appartenenza, AJAX, e così via. abbastanza rapidamente.

Si spera in questa esercitazione è stata fornita gli strumenti che necessari per iniziare a compilare applicazioni proprie Web Form ASP.NET.

> [!div class="step-by-step"]
> [Precedente](tailspin-spyworks-part-7.md)
