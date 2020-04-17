---
uid: web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programmazione di pagine Web ASP.NET (Razor) tramite Visual Studio Documenti Microsoft
author: Rick-Anderson
description: In questa appendice viene illustrato come utilizzare Visual Studio 2010 o Visual Web Developer 2010 Express per programmare ASP.NET pagine Web con la sintassi Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: e586a116fc48a797bdd40befad5851a548282ce6
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542898"
---
# <a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Programmazione di pagine Web ASP.NET (Razor) tramite Visual Studio

 di [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene illustrato come utilizzare Visual Studio o Visual Web Developer Express per programmare ASP.NET siti Web (Razor).
>
> Contenuto dell'esercitazione
>
> - Elementi che è necessario installare (semmai) per lavorare con ASP.NET pagine Web nella versione di Visual Studio.
> - Come aggiungere il supporto per ASP.NET pagine Web a Visual Web Developer 2010 Express.
> - Come usare le funzionalità di Visual Studio per lavorare con ASP.NET pagine Razor, tra cui IntelliSense e il debugger.
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni software utilizzate nell'esercitazione
>
>
> - Pagine Web ASP.NET (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>
>
> Questa esercitazione funziona anche con ASP.NET pagine Web 2, Visual Studio 2012, Visual Studio 2010 e WebMatrix 2.

È possibile programmare ASP.NET pagine Web con la sintassi Razor utilizzando WebMatrix o molti altri editor di codice. È inoltre possibile utilizzare Microsoft Visual Studio, un ambiente di sviluppo integrato (IDE) completo che fornisce un potente set di strumenti per la creazione di molti tipi di applicazioni (non solo siti Web). Per utilizzare ASP.NET pagine Razor, è possibile utilizzare [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).

Due funzionalità particolarmente utili che Visual Studio fornisce per la programmazione con ASP.NET pagine Web Razor sono:

- *IntelliSense*. La funzionalità IntelliSense incorporata in Visual Studio è più completa di IntelliSense in WebMatrix.The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.
- *Debugger*. Il debugger consente di risolvere i problemi del codice arrestando un programma durante l'esecuzione, esaminando le variabili ed eseguendo il codice riga per riga.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Utilizzo di Visual Studio con versioni diverse di pagine Web di ASP.NET

Per sviluppare app Web ASP.NET in Visual Studio 2017, installare il carico di lavoro **di sviluppo Web e di ASP.NET.**

Visual Studio 2012 e Visual Studio 2013 includono il supporto per le pagine Web ASP.NET. I pacchetti necessari per supportare ASP.NET pagine Web vengono installati quando si installa Visual Studio.

Visual Studio 2010 non include il supporto per impostazione predefinita per ASP.NET pagine Web. Per utilizzare ASP.NET pagine Web con Visual Studio 2010, è necessario installare il pacchetto MVC ASP.NET. Per ottenere ASP.NET pagine Web 2, si installa ASP.NET MVC 4.

Nella tabella seguente viene riepilogato il supporto per ASP.NET pagine Web in diverse versioni di Visual Studio.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **Pagine Web ASP.NET 2** | Installare ASP.NET MVC 4 | (Incluso) | (Incluso) |
| **Pagine Web ASP.NET 3** |  | Aggiornamento a pagine Web ASP.NET 3 tramite NuGet | (Incluso) |

Per utilizzare Visual Studio 2010, vedere [Installazione del supporto per ASP.NET pagine Web in Visual Studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>Avvio di Visual Studio da WebMatrix

Se è stato avviato un progetto in WebMatrix e si desidera passare a Visual Studio, WebMatrix fornisce un pulsante per aprire facilmente il progetto in Visual Studio.If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio. Per abilitare questo pulsante, è necessario che Visual Studio sia installato nel computer. The following image shows the button in WebMatrix.

![avviare Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Quando si fa clic sul pulsante, il progetto viene aperto in Visual Studio.When you click the button, the project is opened in Visual Studio. È possibile passare avanti e indietro tra WebMatrix e Visual Studio senza problemi. Si riceverà una notifica se i file sono stati modificati nell'altro ambiente e devono essere ricaricati per ottenere le modifiche più recenti.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Creazione di ASP.NET Razor Site in Visual StudioCreating a Razor Site in Visual Studio

Per creare un sito Web Razor ASP.NET in Visual Studio:To create an ASP.NET Razor website in Visual Studio:

1. Aprire Visual Studio.
2. Scegliere Nuovo sito **Web**dal menu **File** .

    ![creare un nuovo sito Web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. Nella finestra di dialogo **Nuovo sito Web,** selezionare il linguaggio da utilizzare (Visual C.NET o Visual Basic).
4. Selezionare il modello **ASP.NET sito Web (Razor).**

    ![sito rasoio](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Fare clic su **OK**.

Il nuovo progetto esiste e viene popolato con alcune pagine Web predefinite per iniziare.

### <a name="using-intellisense"></a>Using IntelliSense

Dopo aver creato un sito, è possibile vedere il funzionamento di IntelliSense in Visual Studio.Now that you've created a site, you can see how IntelliSense works in Visual Studio.

1. Nel sito Web appena creato aprire la pagina *Default.cshtml.*
2. Dopo `<h3>` i tag nella `@ServerInfo.` pagina, digitare (incluso il punto). Si noti come IntelliSense `ServerInfo` visualizza i metodi disponibili per l'helper in un elenco a discesa.

    ![Intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Selezionare `GetHtml` il metodo dall'elenco e quindi premere INVIO. IntelliSense inserisce automaticamente il metodo . (Come con qualsiasi metodo in C `()` , è necessario aggiungere caratteri dopo il metodo.) Il codice completato `GetHtml` per il metodo è simile all'esempio seguente:The completed code for the method looks like the following example:

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Premete Ctrl e F5 per eseguire la pagina. Questo è l'aspetto della pagina quando viene visualizzata in un browser:

    ![pagina predefinita nel browser](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Chiudere il browser.

### <a name="using-the-debugger"></a>Utilizzo del debugger

1. Nella parte superiore della pagina *Default.cshtml,* dopo `Page.Title`la riga che inizia con , aggiungere la seguente riga di codice:

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. Nel margine grigio dell'editor a sinistra del codice fare clic accanto a questa nuova riga per aggiungere un *punto di interruzione*. Un punto di interruzione è un marcatore che indica al debugger di interrompere l'esecuzione del programma in quel punto in modo da poter vedere cosa sta succedendo.

    ![impostare il punto di interruzione](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Rimuovere la chiamata `ServerInfo.GetHtml` al metodo e aggiungere `@myTime` una chiamata alla variabile al suo posto. Questa chiamata visualizza il valore di ora corrente restituito dalla nuova riga di codice.
4. Premere F5 per eseguire la pagina nel debugger. La pagina si interrompe in corrispondenza del punto di interruzione impostato. L'immagine seguente mostra l'aspetto della pagina nell'editor con il punto di interruzione (in giallo).

    ![punto di interruzione di debug](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. Nella barra degli strumenti Debug, fare clic sul pulsante **Esegui istruzione** (o premere F11) per eseguire la riga di codice successiva. Ogni volta che si fa clic su questo pulsante, si passa l'esecuzione alla riga di codice successiva.

    ![Pulsante Entra in](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Esaminare il `myTime` valore della variabile posizionando il puntatore del mouse su di essa o controllando i valori **visualizzati** nelle finestre Variabili locali e **Stack di chiamate.** In Visual Studio viene visualizzato il valore della variabile.

    ![Mostra valore temporale](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Al termine dell'esame della variabile e dell'esecuzione del codice, premere F5 per continuare l'esecuzione della pagina senza fermarsi a ogni riga. Al termine dell'esecuzione di un'istruzione alla volta in tutto il codice, il browser visualizza la pagina.

Per ulteriori informazioni sul debugger e su come eseguire il debug del codice in Visual Studio, vedere [Procedura dettagliata: debug di pagine Web in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Utilizzo di Razor in ASP.NET progetti MVC con Visual StudioUsing Razor in ASP.NET MVC projects with Visual Studio

La sintassi Razor viene utilizzata ampiamente anche nei progetti MVC ASP.NET. MVC è un modo potente e basato su modelli per creare siti Web dinamici. Se il sito ASP.NET pagine Web diventa difficile da gestire, è consigliabile convertirlo in un'applicazione MVC ASP.NET. Per un esempio di creazione di un'applicazione MVC, vedere [Introduzione all'ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Installazione del supporto per ASP.NET pagine Web in Visual Studio 2010

In questa sezione viene illustrato come installare Visual Web Developer Express 2010 e gli strumenti di ASP.NET pagine Web per Visual Studio.

1. Se non si dispone già dell'Installazione guidata piattaforma Web, scaricarla dal seguente URL:

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Eseguire l'Installazione guidata piattaforma Web.
3. Fare clic sulla scheda **Prodotti.**

    ![Scheda Prodotti WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Cercare **ASP.NET MVC 4** (per ASP.NET pagine Web 2) e quindi fare clic su **Aggiungi**. Questi prodotti includono strumenti di Visual Studio per la creazione di siti Web ASP.NET Razor.These products include Visual Studio tools for building ASP.NET Razor websites.

    ![Opzioni di installazione di WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Fare clic su **Installa** per completare l'installazione.
