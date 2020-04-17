---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
title: Utilizzo di ASP.NET MVC con versioni diverse di IIS (VB) Documenti Microsoft
author: rick-anderson
description: In questa esercitazione si apprenderà come usare ASP.NET MVC e Routing URL con versioni diverse di Internet Information Services. Impari diverse strategie...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 1c1283b2-6956-4937-b568-d30de432ce23
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
msc.type: authoredcontent
ms.openlocfilehash: 5e04ae14026e6d5dd1e603be6c52ff6876a468cf
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542001"
---
# <a name="using-aspnet-mvc-with-different-versions-of-iis-vb"></a>Uso di ASP.NET MVC con versioni diverse di IIS (VB)

da parte [di Microsoft](https://github.com/microsoft)

> In questa esercitazione si apprenderà come usare ASP.NET MVC e Routing URL con versioni diverse di Internet Information Services. Vengono illustrato diverse strategie per l'utilizzo di ASP.NET MVC con IIS 7.0 (modalità classica), IIS 6.0 e versioni precedenti di IIS.

Il framework di ASP.NET MVC dipende da ASP.NET Routing per instradare le richieste del browser alle azioni del controller. Per sfruttare ASP.NET Routing, potrebbe essere necessario eseguire ulteriori passaggi di configurazione sul server Web. Tutto dipende dalla versione di Internet Information Services (IIS) e dalla modalità di elaborazione delle richieste per l'applicazione.

Di seguito è riportato un riepilogo delle diverse versioni di IIS:

- IIS 7.0 (modalità integrata) - Nessuna configurazione speciale necessaria per utilizzare ASP.NET routing.
- IIS 7.0 (modalità classica) - È necessario eseguire una configurazione speciale per utilizzare ASP.NET Routing.
- IIS 6.0 o versione inferiore: è necessario eseguire una configurazione speciale per utilizzare ASP.NET routing.

La versione più recente di IIS è la versione 7.5 (su Win7). IIS 7 di IIS è incluso in Windows Server 2008 E VISTA/SP1 e versioni successive. È inoltre possibile installare IIS 7.0 in qualsiasi versione del [https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)sistema operativo Vista, ad eccezione di Home Basic (vedere ).

IIS 7.0 supporta due modalità per l'elaborazione delle richieste. È possibile utilizzare la modalità integrata o la modalità classica. Non è necessario eseguire alcuna procedura di configurazione speciale quando si utilizza IIS 7.0 in modalità integrata. Tuttavia, è necessario eseguire una configurazione aggiuntiva quando si utilizza IIS 7.0 in modalità classica.

Microsoft Windows Server 2003 include IIS 6.0. Non è possibile aggiornare IIS 6.0 a IIS 7.0 quando si utilizza il sistema operativo Windows Server 2003. È necessario eseguire ulteriori passaggi di configurazione quando si utilizza IIS 6.0.

Microsoft Windows XP Professional include IIS 5.1. È necessario eseguire ulteriori passaggi di configurazione quando si utilizza IIS 5.1.

Infine, Microsoft Windows 2000 e Microsoft Windows 2000 Professional include IIS 5.0. È necessario eseguire ulteriori passaggi di configurazione quando si utilizza IIS 5.0.

## <a name="integrated-versus-classic-mode"></a>Modalità integrata e classica

IIS 7.0 può elaborare le richieste utilizzando due diverse modalità di elaborazione delle richieste: integrate e classiche. La modalità integrata offre prestazioni migliori e più funzionalità. La modalità classica è inclusa per garantire la compatibilità con le versioni precedenti di IIS.

La modalità di elaborazione della richiesta è determinata dal pool di applicazioni. È possibile determinare quale modalità di elaborazione viene utilizzata da una determinata applicazione Web determinando il pool di applicazioni associato all'applicazione. A tale scopo, seguire questa procedura:

1. Avviare Gestione Internet Information Services
2. Nella finestra Connessioni, selezionare un'applicazione
3. Nella finestra Azioni, fare clic sul collegamento **Impostazioni di base** per aprire la finestra di dialogo Modifica applicazione (vedere la Figura 1)
4. Prendere nota del pool di applicazioni selezionato.

Per impostazione predefinita, IIS è configurato per supportare due pool di applicazioni: **DefaultAppPool** e **Classic .NET AppPool**. Se DefaultAppPool è selezionato, l'applicazione viene eseguita in modalità di elaborazione delle richieste integrata. Se è selezionata l'opzione Classic .NET AppPool, l'applicazione viene eseguita in modalità di elaborazione delle richieste classica.

[![Finestra di dialogo relativa al nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.png)

**Figura 1:** Rilevamento della modalità di elaborazione richiesta([Fare clic per visualizzare l'immagine](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.png)a dimensioni intere)

Si noti che è possibile modificare la modalità di elaborazione delle richieste nella finestra di dialogo Modifica applicazione. Fare clic sul pulsante Seleziona e modificare il pool di applicazioni associato all'applicazione. Tenere presente che esistono problemi di compatibilità quando si modifica un'applicazione ASP.NET dalla modalità classica alla modalità integrata. Per altre informazioni, vedere gli articoli seguenti:

- Aggiornamento di ASP.NET 1.1 a IIS 7.0 in Windows Vista e Windows Server 2008 --[https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)

- ASP.NET integrazione con IIS 7.0 -[https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

Se un'applicazione ASP.NET utilizza DefaultAppPool, non è necessario eseguire ulteriori passaggi per ottenere ASP.NET Routing (e pertanto ASP.NET MVC) per funzionare. Tuttavia, se l'applicazione ASP.NET è configurata per utilizzare l'appPool .NET classico quindi continuare a leggere, è necessario eseguire altre operazioni.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>Utilizzo di ASP.NET MVC con versioni precedenti di IIS

Se è necessario utilizzare ASP.NET MVC con una versione precedente di IIS 7.0 o è necessario utilizzare IIS 7.0 in modalità classica, sono disponibili due opzioni. In primo luogo, è possibile modificare la tabella di route per utilizzare le estensioni di file. Ad esempio, anziché richiedere un URL come /Store/Details, è necessario richiedere un URL come /Store.aspx/Details.For example, instead of requesting a URL like /Store/Details, you would request a URL like /Store.aspx/Details.

La seconda opzione consiste nel creare un elemento denominato mapping di *script con caratteri jolly*. Una mappa di script con caratteri jolly consente di eseguire il mapping di ogni richiesta nel framework di ASP.NET.

Se non si ha accesso al server Web (ad esempio, l'applicazione MVC ASP.NET è ospitata da un provider di servizi Internet), sarà necessario utilizzare la prima opzione. Se non si desidera modificare l'aspetto degli URL e si ha accesso al server Web, è possibile utilizzare la seconda opzione.

Esaminiamo ogni opzione in dettaglio nelle sezioni seguenti.

## <a name="adding-extensions-to-the-route-table"></a>Aggiunta di estensioni alla tabella di route

Il modo più semplice per ottenere ASP.NET Routing per lavorare con le versioni precedenti di IIS consiste nel modificare la tabella di route nel file Global.asax. Il file Global.asax predefinito e non modificato nel listato 1 configura una route denominata Route predefinita.

**Lista 1 - Global.asax (non modificato)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample1.vb)]

La route predefinita configurata nel listato 1 consente di instradare GLI URL simili ai seguente:

/Home/Indice

/Prodotto/Dettagli/3

/Prodotto

Sfortunatamente, le versioni precedenti di IIS non passerà queste richieste al framework di ASP.NET. Pertanto, queste richieste non verranno instradate a un controller. Ad esempio, se si effettua una richiesta del browser per l'URL /Home/Index, verrà visualizzata la pagina di errore in Figura 2.

[![Finestra di dialogo relativa al nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.png)

**Figura 2**: Ricezione di un errore 404 Non trovato([Fare clic per visualizzare l'immagine a dimensioni intere](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.png))

Le versioni precedenti di IIS eseguono solo il mapping di determinate richieste al framework di ASP.NET. La richiesta deve essere per un URL con l'estensione di file corretta. Ad esempio, una richiesta per /SomePage.aspx viene mappata al framework di ASP.NET. Tuttavia, una richiesta per /SomePage.htm non lo fa.

Pertanto, per fare ASP.NET Routing al funzionamento, è necessario modificare la route predefinita in modo che includa un'estensione di file mappata al framework di ASP.NET.

Questa operazione viene eseguita utilizzando uno script denominato `registermvc.wsf`. È stato incluso nella versione `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`ASP.NET MVC 1 in , ma a partire da [http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978)ASP.NET 2 questo script è stato spostato nel ASP.NET Futures, disponibile all'indirizzo .

L'esecuzione di questo script registra una nuova estensione MVC con IIS. Dopo aver registrato l'estensione MVC, è possibile modificare le route nel file Global.asax in modo che le route utilizzino l'estensione MVC.

Il file Global.asax modificato nel listato 2 funziona con le versioni precedenti di IIS.

**Listato 2 - Global.asax (modificato con estensioni)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample2.vb)]

Importante: ricordarsi di compilare nuovamente l'applicazione MVC ASP.NET dopo aver modificato il file Global.asax.

Esistono due importanti modifiche al file Global.asax nel listato 2. Ora ci sono due rotte definite nel file Global.asax. Il modello di URL per la route predefinita, la prima route, ora è simile al:

<a0></a0><a1></a1><a2></a2><a3></a3><a4><

L'aggiunta dell'estensione MVC modifica il tipo di file intercettati dal modulo di routing ASP.NET. Con questa modifica, l'applicazione MVC ASP.NET ora indirizza le richieste come segue:

/Home.mvc/Indice/

/Product.mvc/Dettagli/3

/Product.mvc/

Il secondo percorso, il percorso principale, è nuovo. Questo modello di URL per la route radice è una stringa vuota. Questa route è necessaria per la corrispondenza delle richieste effettuate con la radice dell'applicazione. Ad esempio, la route radice corrisponderà a una richiesta simile alla seguente:For example, the Root route will match a request that looks like this:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Dopo aver apportato queste modifiche alla tabella di route, è necessario assicurarsi che tutti i collegamenti nell'applicazione siano compatibili con questi nuovi modelli di URL. In altre parole, assicurarsi che tutti i collegamenti includano l'estensione MVC. Se si utilizza il metodo helper Html.ActionLink() per generare i collegamenti, non è necessario apportare modifiche.

Anziché utilizzare lo script registermvc.wcf, è possibile aggiungere una nuova estensione a IIS mappata automaticamente al framework ASP.NET. Quando si aggiunge una nuova estensione, assicurarsi che la casella di controllo **Verifica che il file esista** non sia selezionata.

## <a name="hosted-server"></a>Server ospitato

Non sempre hai accesso al tuo server web. Ad esempio, se si ospita l'applicazione MVC ASP.NET utilizzando un provider di hosting Internet, non sarà necessario accedere a IIS.

In tal caso, è necessario utilizzare una delle estensioni di file esistenti mappate al framework di ASP.NET. Esempi di estensioni di file mappate a ASP.NET includono le estensioni aspx, axd e ashx.

Ad esempio, il file Global.asax modificato nel listato 3 utilizza l'estensione aspx anziché l'estensione mvc.

**Listato 3 - Global.asax (modificato con estensioni aspx)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample3.vb)]

Il file Global.asax nel listato 3 è esattamente lo stesso del file Global.asax precedente, ad eccezione del fatto che utilizza l'estensione aspx anziché l'estensione MVC. Non è necessario eseguire alcuna installazione sul server Web remoto per utilizzare l'estensione aspx.

## <a name="creating-a-wildcard-script-map"></a>Creazione di una mappa di script con caratteri jollyCreating a Wildcard Script Map

Se non si desidera modificare gli URL per l'applicazione MVC ASP.NET e si ha accesso al server Web, è disponibile un'opzione aggiuntiva. È possibile creare un mapping di script con caratteri jolly che esegue il mapping di tutte le richieste al server Web al framework di ASP.NET. In questo modo, è possibile utilizzare la tabella di route MVC di ASP.NET predefinita con IIS 7.0 (in modalità classica) o IIS 6.0.

Tenere presente che questa opzione fa sì che IIS intercetti ogni richiesta effettuata sul server web. Sono incluse le richieste di immagini, pagine ASP classiche e pagine HTML. Pertanto, l'abilitazione di un mapping di script con caratteri jolly a ASP.NET ha implicazioni sulle prestazioni.

Ecco come abilitare un mapping di script con caratteri jolly per IIS 7.0:

1. Selezionare l'applicazione nella finestra Connessioni
2. Assicurarsi che la vista **Funzioni** sia selezionata
3. Fare doppio clic sul pulsante **Mapping gestori**
4. Fare clic sul collegamento Aggiungi mappa **script caratteri jolly** (vedere la figura 3)
5. Immettere il percorso\_del file aspnet isapi.dll (è possibile copiare questo percorso dalla mappa dello script PageHandlerFactory)
6. Immettere il nome MVC
7. Fare clic sul pulsante **OK**

[![Finestra di dialogo relativa al nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.png)

**Figura 3**: Creazione di una mappa di script con caratteri jolly con IIS 7.0(Fare[clic per visualizzare l'immagine a dimensioni intere)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image6.png)

Per creare una mappa di script con caratteri jolly con IIS 6.0, attenersi alla seguente procedura:

1. Fare clic con il pulsante destro del mouse su un sito Web e selezionare Proprietà
2. Selezionare la scheda **Home directory**
3. Fare clic sul pulsante **Configurazione**
4. Selezionare la scheda **Mapping**
5. Fare clic sul pulsante **Inserisci** (vedere la figura 4)
6. Incollare il percorso\_del file aspnet isapi.dll nel campo Executable (è possibile copiare questo percorso dalla mappa di script per i file aspx)
7. Deselezionare la casella di controllo **Verifica l'presenza di file**
8. Fare clic sul pulsante **OK**

[![Finestra di dialogo relativa al nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image7.png)

**Figura 4**: Creazione di una mappa di script con caratteri jolly con IIS 6.0([Fare clic per visualizzare l'immagine](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image8.png)a dimensioni intere )

Dopo aver abilitato i mapping di script con caratteri jolly, è necessario modificare la tabella di route nel file Global.asax in modo che includa una route radice. In caso contrario, si otterrà la pagina di errore in Figura 5 quando si effettua una richiesta per la pagina radice dell'applicazione. È possibile utilizzare il file Global.asax modificato nel listato 4.

[![Finestra di dialogo relativa al nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image9.png)

**Figura 5:** Errore di route radice mancante([Fare clic per visualizzare l'immagine a dimensioni intere)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image10.png)

**Listato 4 - Global.asax (modificato con percorso radice)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample4.vb)]

Dopo aver attivato un mapping di script con caratteri jolly per IIS 7.0 o IIS 6.0, è possibile effettuare richieste che funzionano con la tabella di route predefinita simile alla seguente:

/

/Home/Indice

/Prodotto/Dettagli/3

/Prodotto

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione era quello di spiegare come è possibile utilizzare ASP.NET MVC quando si utilizza una versione precedente di IIS (o IIS 7.0 in modalità classica). Sono stati illustrati due metodi per fare in modo che ASP.NET Routing funzioni con le versioni precedenti di IIS: modificare la tabella di route predefinita o creare una mappa di script con caratteri jolly.

La prima opzione richiede la modifica degli URL utilizzati nell'applicazione MVC ASP.NET. Un vantaggio molto significativo di questa prima opzione è che non è necessario accedere a un server Web per modificare la tabella di route. Ciò significa che è possibile utilizzare questa prima opzione anche quando si ospita l'applicazione MVC ASP.NET con una società di hosting Internet.

La seconda opzione consiste nel creare una mappa di script con caratteri jolly. Il vantaggio di questa seconda opzione è che non è necessario modificare gli URL. Lo svantaggio di questa seconda opzione è che può influire sulle prestazioni dell'applicazione MVC ASP.NET.

> [!div class="step-by-step"]
> [Indietro](using-asp-net-mvc-with-different-versions-of-iis-cs.md)
