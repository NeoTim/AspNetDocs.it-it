---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Aggiunta di un modello (c#) | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro e molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 8cf8693b71509163860c78a87370c4765414fd5d
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65130344"
---
# <a name="adding-a-model-c"></a>Aggiunta di un modello (C#)

da [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul collegamento seguente: [Installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul collegamento seguente: [Prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente c# è disponibile a complemento di questo argomento. [Scaricare la versione c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce Visual Basic, passare al [versione Visual Basic](../vb/adding-a-model.md) di questa esercitazione.

## <a name="adding-a-model"></a>Aggiunta di un modello

In questa sezione si aggiungeranno alcune classi per la gestione di film in un database. Queste classi saranno la parte "modello" dell'applicazione ASP.NET MVC.

Si userà una tecnologia di accesso ai dati di .NET Framework nota come Entity Framework per definire e usare queste classi del modello. Entity Framework (noto anche come Entity Framework) supporta un paradigma di sviluppo denominato *Code First*. Prima di tutto i codice consente di creare gli oggetti del modello mediante la scrittura di classi semplici. (Queste sono anche note come classi POCO, da "plain-old CLR objects".) È quindi possibile avere il database creato in tempo reale dalle classi, che consente a un flusso di lavoro molto pulito e rapida evoluzione.

## <a name="adding-model-classes"></a>Aggiunta di classi di modello

Nella **Esplora soluzioni**, fare clic il *modelli* cartella, selezionare **Add**e quindi selezionare **classe**.

![](adding-a-model/_static/image1.png)

Nome di *classe* "Movie".

[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)

Aggiungere le seguenti cinque proprietà per il `Movie` classe:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Si userà il `Movie` classe per rappresentare i film in un database. Ogni istanza di un `Movie` oggetto corrisponderà a una riga all'interno di una tabella di database e ogni proprietà del `Movie` classe verrà eseguito il mapping a una colonna nella tabella.

Nello stesso file, aggiungere il codice seguente `MovieDBContext` classe:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

Il `MovieDBContext` classe rappresenta il contesto di database di film Entity Framework, che gestisce il recupero, l'archiviazione e aggiornando `Movie` classe istanze in un database. Il `MovieDBContext` deriva dal `DbContext` classe fornita da Entity Framework di base. Per altre informazioni sulle `DbContext` e `DbSet`, vedere [miglioramenti della produttività per Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Per poter fare riferimento a `DbContext` e `DbSet`, è necessario aggiungere il codice seguente `using` informativa nella parte superiore del file:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

L'intero *Movie.cs* file è illustrato di seguito.

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Creazione di una stringa di connessione e l'utilizzo di SQL Server Compact

Il `MovieDBContext` classe creata gestisce le attività di mapping e la connessione al database `Movie` oggetti per i record del database. Una domanda che ci si potrebbe chiedere, tuttavia, è illustrato come specificare i database a cui si connette. È necessario usare l'aggiunta di informazioni di connessione nel *Web. config* file dell'applicazione.

Aprire la radice dell'applicazione *Web. config* file. (Non il *Web. config* del file nei *viste* cartella.) Nell'immagine seguente mostra entrambi *Web. config* i file, aprire il *Web. config* file cerchiata in rosso.

![](adding-a-model/_static/image4.png)

Aggiungere la seguente stringa di connessione per il `<connectionStrings>` elemento il *Web. config* file.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

L'esempio seguente mostra una parte i *Web. config* file con la nuova stringa di connessione aggiunta:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Questa piccola quantità di codice e XML è tutto ciò che occorre per scrivere allo scopo di rappresentare e archiviare i dati dei film in un database.

Successivamente, si creerà un nuovo `MoviesController` classe che è possibile usare per visualizzare i dati dei film e consentire agli utenti di creare nuove voci di filmato.

> [!div class="step-by-step"]
> [Precedente](adding-a-view.md)
> [Successivo](accessing-your-models-data-from-a-controller.md)
