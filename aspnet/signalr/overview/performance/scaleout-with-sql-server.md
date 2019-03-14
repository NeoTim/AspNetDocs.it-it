---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Scalabilità orizzontale di SignalR con SQL Server | Microsoft Docs
author: bradygaster
description: Le versioni del software utilizzato in questo argomento di Visual Studio 2013 .NET 4.5 SignalR le versioni precedenti la versione 2 di questo argomento per informazioni sulle versioni precedenti di...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 5984a5e6c3215e7dde8c09ef702bf6453730a3ee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57053308"
---
<a name="signalr-scaleout-with-sql-server"></a>Scale-out di SignalR con SQL Server
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software utilizzate in questo argomento
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR versione 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
>
> Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).


In questa esercitazione si userà SQL Server per distribuire i messaggi tra un'applicazione di SignalR distribuito in due istanze separate di IIS. È anche possibile eseguire questa esercitazione in un computer singolo test, ma per ottenere l'effetto completo, è necessario distribuire l'applicazione di SignalR in due o più server. È anche necessario installare SQL Server in uno dei server o in un server dedicato distinto. Un'altra opzione consiste nell'eseguire l'esercitazione usando le macchine virtuali in Azure.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Prerequisiti

Microsoft SQL Server 2005 o versione successiva. Backplane supporta le edizioni desktop e server di SQL Server. Non supporta SQL Server Compact Edition o Database SQL di Azure. (Se l'applicazione è ospitata in Azure, prendere in considerazione il backplane del Bus di servizio invece.)

## <a name="overview"></a>Panoramica

Prima di passare all'esercitazione dettagliata, ecco una rapida panoramica delle azioni da eseguire.

1. Creare un nuovo database vuoto. Backplane creerà le tabelle necessarie in questo database.
2. Aggiungere i pacchetti NuGet per l'applicazione:

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Creare un'applicazione di SignalR.
4. Aggiungere il codice seguente per configurare backplane Startup.cs:

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   Questo codice consente di configurare con i valori predefiniti per il backplane [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) e [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Per informazioni sulla modifica di questi valori, vedere [prestazioni di SignalR: La metrica di scalabilità orizzontale](signalr-performance.md#scaleout_metrics).

## <a name="configure-the-database"></a>Configurare il Database

Decidere se l'applicazione utilizzerà l'autenticazione di Windows o SQL Server per accedere al database. Indipendentemente dal fatto che, assicurarsi che l'utente del database disponga delle autorizzazioni per accedere, creare gli schemi e creare tabelle.

Creare un nuovo database per il backplane da utilizzare. È possibile assegnare al database un nome qualsiasi. Non è necessario creare tutte le tabelle nel database. backplane verrà create le tabelle necessarie.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Abilitare Service Broker

È consigliabile abilitare Service Broker per il database backplane. Service Broker fornisce il supporto nativo per la messaggistica e Accodamento in SQL Server, che consente il backplane di ricevere gli aggiornamenti in modo più efficiente. (Tuttavia, il backplane funziona anche senza Service Broker.)

Per verificare se Service Broker è abilitato, eseguire una query il **viene\_broker\_abilitata** colonna il **Sys. Databases** vista del catalogo.

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Per abilitare Service Broker, usare la query SQL seguente:

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Se questa query viene visualizzato un deadlock, assicurarsi che non sono presenti applicazioni connesse al database.


Se è stata abilitata la traccia, le tracce verranno inoltre descritto se Service Broker è abilitato.

## <a name="create-a-signalr-application"></a>Creare un'applicazione di SignalR

Creare un'applicazione di SignalR seguendo una di queste esercitazioni:

- [Introduzione a SignalR 2.0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Introduzione a SignalR 2.0 e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

Successivamente, modifichiamo l'applicazione di chat per supportare scalabilità orizzontale con SQL Server. In primo luogo, aggiungere il pacchetto SignalR.SqlServer NuGet al progetto. In Visual Studio dal **degli strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti immettere il comando seguente:

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

Successivamente, aprire il file Startup.cs. Aggiungere il codice seguente per il **configura** metodo:

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Distribuire ed eseguire l'applicazione

Preparare le istanze di Windows Server per distribuire l'applicazione di SignalR.

Aggiungere il ruolo IIS. Include funzionalità di "Sviluppo di applicazioni", tra cui il protocollo WebSocket.

![](scaleout-with-sql-server/_static/image4.png)

Includere anche il servizio di gestione (elencati in "Strumenti di gestione").

![](scaleout-with-sql-server/_static/image5.png)

**Installare Web Deploy 3.0.** Quando si esegue Gestione IIS, verrà richiesto di installare piattaforma Web Microsoft oppure è possibile [Scarica il intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). Nel programma di installazione della piattaforma, eseguire la ricerca di Web Deploy e installare distribuzione Web 3.0

![](scaleout-with-sql-server/_static/image6.png)

Verificare che il servizio di gestione Web sia in esecuzione. In caso contrario, avviare il servizio. (Se il servizio di gestione Web non viene visualizzata nell'elenco dei servizi di Windows, assicurarsi che il servizio di gestione installato quando è stato aggiunto il ruolo IIS.)

Infine, aprire la porta 8172 per TCP. Questa è la porta che utilizza lo strumento distribuzione Web.

A questo punto si è pronti per distribuire il progetto di Visual Studio dal computer di sviluppo al server. In Esplora soluzioni fare doppio clic la soluzione e fare clic su **pubblica**.

Per informazioni dettagliate documentazione sulla distribuzione web, vedere [mappa del contenuto di distribuzione Web per Visual Studio e ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

Se si distribuisce l'applicazione a due server, è possibile aprire ogni istanza in una finestra distinta del browser e vedere che possano ricevere i messaggi SignalR da un altro. (Naturalmente, in un ambiente di produzione, i due server sarebbe si trovano dietro un bilanciamento del carico.)

![](scaleout-with-sql-server/_static/image7.png)

Dopo aver eseguito l'applicazione, è possibile vedere che SignalR ha creato automaticamente le tabelle nel database:

![](scaleout-with-sql-server/_static/image8.png)

SignalR gestisce le tabelle. Fino a quando l'applicazione viene distribuita, non eliminare le righe, modificare la tabella e così via.
