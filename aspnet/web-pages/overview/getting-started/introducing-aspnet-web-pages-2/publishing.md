---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: Introduzione a Pagine Web ASP.NET pubblicazione di un sito con WebMatrix | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione è l'ultima installazione del set di esercitazioni che introduce Pagine Web ASP.NET e Microsoft WebMatrix. Viene illustrato come pubblicare il sito t...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: a09339a833ea0b4a2d3c3a9323cce777577ea048
ms.sourcegitcommit: 0cf7d06071a8ff986e6c028ac9daf0c0e7490412
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/23/2020
ms.locfileid: "85240595"
---
# <a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>Introduzione a Pagine Web ASP.NET pubblicazione di un sito con WebMatrix

 di [Tom FitzMacken](https://github.com/tfitzmac)

> Questa esercitazione è l'ultima installazione del set di esercitazioni che introduce Pagine Web ASP.NET e Microsoft WebMatrix. Viene illustrato come pubblicare il sito su Internet in modo che altri utenti possano utilizzarlo. Si presuppone che la serie sia stata completata attraverso la [creazione di un aspetto coerente per i siti pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251585).
> 
> Si apprenderà come pubblicare il sito usando:
> 
> - Microsoft Azure
> - Società di hosting Web

## <a name="about-publishing-your-site"></a>Informazioni sulla pubblicazione del sito

Fino a questo momento, è stato eseguito tutto il lavoro in un computer locale, incluso il test delle pagine. Per eseguire le pagine con<em>estensione cshtml</em> , è stato usato il server Web incorporato in WebMatrix, ovvero IIS Express. Naturalmente, tuttavia, non è possibile visualizzare il sito creato, ad eccezione dell'utente. Per consentire ad altri utenti di lavorare con il sito, è necessario pubblicarlo in Internet.

A meno che non si disponga già dell'accesso a un server Web pubblico, la pubblicazione significa che è necessario disporre di un account con una *piattaforma cloud* o un *provider di hosting*. Una piattaforma cloud, ad esempio Microsoft Azure, fornisce un'infrastruttura su richiesta per le applicazioni. Un provider di hosting è una società che possiede server Web accessibili pubblicamente e che consentirà di affittare spazio per il sito. I piani di hosting vengono eseguiti da pochi dollari al mese (o addirittura gratuiti) per i siti di piccole dimensioni a molte centinaia di dollari al mese per siti web commerciali a volume elevato.

> [!NOTE]
> Potrebbe essere possibile accedere a un server Web pubblico tramite il provider di servizi Internet (ISP) usato per ottenere il servizio Internet a casa. Tuttavia, il provider di hosting deve supportare Pagine Web ASP.NET. Molti ISP non lo fanno, ma è sempre opportuno verificare.

In questa esercitazione verrà illustrata una panoramica su come pubblicare. Non è pratico fornire dettagli esatti per tutti gli elementi, perché il processo differisce per ogni provider di hosting. Ma si otterrà una valida idea del funzionamento del processo.

Questa esercitazione contiene quattro sezioni:

1. [Impostazione della pagina predefinita](#defaultpage)
2. Pubblicazione (scegliere una delle opzioni seguenti)  
 a. [Pubblicazione del sito Microsoft Azure](#azure)  
 b. [Pubblicazione del sito in una società di hosting Web](#host)
3. [Aggiornamento del sito Live: ripubblicazione](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>Impostazione della pagina predefinita

Quando un utente passa all'indirizzo di base per il sito Web, viene visualizzata la pagina predefinita per il sito. Ad esempio, quando *Default.htm* viene impostato come pagina predefinita per il sito in, il passaggio `www.contoso.com` a `www.contoso.com` è identico a quello che si sta spostando a `www.contoso.com/Default.htm` .

Attualmente, il sito usa **default. cshtml** come pagina predefinita. Questa pagina è corretta per la pagina predefinita, ma in questa esercitazione non è stato aggiunto alcun contenuto alla pagina in modo da visualizzare una pagina vuota. Aprire default. cshtml e sostituire il contenuto con il codice seguente.

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

Il sito è ora pronto per la pubblicazione. In primo luogo, si vedrà come distribuire il sito in Azure e come distribuirlo a una società di hosting Web. Entrambe le opzioni funzionano per il sito Web ed è sufficiente seguire una delle opzioni di distribuzione.

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>Pubblicazione del sito Microsoft Azure

Questa esercitazione illustra prima di tutto come distribuire il sito per Microsoft Azure. Accedendo con una account Microsoft, è possibile creare fino a 10 siti gratuiti in Azure. Questi siti gratuiti rappresentano un modo pratico per testare i siti. È sempre possibile eliminare questo sito di esempio in un secondo momento per evitare di usare tutti i siti gratuiti. È possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere la pagina relativa alla [versione di valutazione gratuita di Azure](https://azure.microsoft.com/free/dotnet/).

Sulla barra multifunzione di WebMatrix fare clic sul pulsante **pubblica** .

![Pulsante ' pubblica ' nella barra multifunzione di WebMatrix](publishing/_static/image1.png)

Verrà visualizzata la finestra di dialogo **pubblica il sito** . Se non è stato effettuato l'accesso alla account Microsoft, nella finestra di dialogo sarà contenuto un collegamento **Introduzione ad Azure** . Fare clic su questo collegamento.

![Pubblicare il sito](publishing/_static/image2.png)

Se non è stato effettuato l'accesso a un account Microsoft, si ha la possibilità di effettuare l'accesso. È necessario accedere a una account Microsoft per pubblicare il sito in Azure.

![Accedi](publishing/_static/image3.png)

Dopo aver eseguito l'accesso alla account Microsoft, la finestra di dialogo contiene collegamenti per creare un nuovo sito in Azure o connettersi a uno dei siti esistenti in Azure.

![Crea nuovo sito](publishing/_static/image4.png)

Selezionare **Crea un nuovo sito**.

Se è stato denominato il progetto **WebPagesMovies**, il nome predefinito per il sito sarà **webpagesmovies.azurewebsites.NET**. Questo nome predefinito è probabilmente non disponibile, come indicato dal punto esclamativo rosso.

![Nome sito Web predefinito](publishing/_static/image5.png)

Modificare il nome del sito in un elemento disponibile e selezionare un percorso vicino alla propria posizione.

![Nome sito modificato](publishing/_static/image6.png)

Fare clic su **OK**.

WebMatrix esegue un test per determinare se il server è compatibile con il sito.

![test di compatibilità](publishing/_static/image7.png)

Selezionare **Continua**.

Verranno visualizzati i risultati del test di compatibilità.

![risultato di compatibilità](publishing/_static/image8.png)

Selezionare **Continua**.

WebMatrix consente di visualizzare i file e i database che verranno pubblicati nel sito. Poiché questa è la prima volta che si pubblica il sito, vengono elencati tutti i file. È possibile deselezionare un file che non è pronto per la pubblicazione. Nelle pubblicazioni successive verranno visualizzati solo i file che sono stati modificati. Vedere [aggiornamento del sito Live: ripubblicazione](#update).

![Publish Preview](publishing/_static/image9.png)

Selezionare **Continua**.

Dopo che il sito è stato distribuito in Azure, viene visualizzato un messaggio che indica che la distribuzione è stata completata.

![Pubblicazione completata](publishing/_static/image10.png)

Il sito e il database sono stati pubblicati in Azure e sono ora disponibili per il pubblico. Fare clic sul collegamento nel messaggio che indica che la pubblicazione è stata completata e che ora verrà visualizzato il sito distribuito. L'utente o qualsiasi utente con accesso a Internet può aggiungere o modificare i record nel database.

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>Pubblicazione del sito in una società di hosting Web

Se si decide di non eseguire la pubblicazione in Azure, è invece possibile pubblicare il sito in una società di hosting Web.

Fare clic sul collegamento **trova hosting Web** .

![Pulsante ' trova hosting Web ' nella finestra di dialogo Impostazioni di pubblicazione](publishing/_static/image12.png)

Si passa a una pagina nel sito Microsoft che elenca i provider di hosting che supportano ASP.NET.

![Pagina nel sito Microsoft che elenca i provider di hosting](publishing/_static/image13.png)

Ovviamente, può essere difficile conoscere esattamente le funzionalità di hosting che potrebbero essere necessarie a lungo termine. Ecco alcuni aspetti da considerare:

- Per quanto riguarda il sito di WebPagesMovies, non è necessario disporre di un componente aggiuntivo separato per SQL Server, che spesso comporta costi aggiuntivi. Nel sito si usa SQL Server Compact Edition, che è indipendente. Potrebbe tuttavia essere necessario l'accesso SQL Server per alcune attività future del sito Web. Se si ritiene che sia possibile, assicurarsi che sia possibile aggiungere SQL Server funzionalità in un secondo momento.
- Controllare se il provider di hosting supporta il protocollo di pubblicazione Distribuzione Web. È possibile pubblicare usando il protocollo FTP, ma è più facile usare Distribuzione Web.

Alcuni siti offrono un periodo di valutazione gratuito. Una versione di valutazione gratuita è un modo efficace per provare a pubblicare e ospitare mentre si sta ancora sperimentando WebMatrix e Pagine Web ASP.NET.

Selezionare una delle opzioni desiderate. Per questa esercitazione è stato selezionato DiscountASP.NET, perché durante la creazione dell'esercitazione, l'azienda aveva una promozione che consente agli utenti di ospitare un sito gratuitamente per alcuni mesi.

> [!NOTE]
> La scelta di un provider di hosting per questa esercitazione non deve essere interpretata come un'approvazione di tale società rispetto ad altri. Tuttavia è stato necessario sceglierne uno per l'illustrazione e DiscountASP.NET è una delle numerose società che supporta Pagine Web ASP.NET e il protocollo di Distribuzione Web per la pubblicazione.

In genere, dopo aver effettuato l'iscrizione al provider di hosting, la società Invia un messaggio di posta elettronica contenente un nome utente e una password, l'URL del server Web e così via. Se la società di hosting supporta il protocollo Distribuzione Web, è possibile che invii un file contenente le impostazioni di pubblicazione oppure che sia possibile scaricarne uno. Un file di impostazioni di pubblicazione semplifica il processo.

Una volta effettuata l'iscrizione e si è pronti per la pubblicazione, fare clic sul pulsante **pubblica** nella barra multifunzione di WebMatrix. Verrà visualizzata la finestra di dialogo **impostazioni di pubblicazione** .

Se il provider di hosting ha inviato un file di impostazioni di pubblicazione, fare clic sul collegamento **Importa impostazioni di pubblicazione** e importare il file. Se non si dispone di un file di impostazioni di pubblicazione, compilare i campi usando i valori che la società di hosting ha inviato tramite posta elettronica. Di seguito è illustrata la finestra di dialogo **impostazioni di pubblicazione** simile al termine:

![Impostazioni di pubblicazione compilate nella finestra di dialogo ' impostazioni di pubblicazione '](publishing/_static/image14.png)

Fare clic su **convalida connessione**. Se tutto è OK, la finestra di dialogo segnala la **connessione**, il che significa che può comunicare con il server del provider di hosting.

![Messaggio di operazione riuscita se le impostazioni di pubblicazione sono corrette](publishing/_static/image15.png)

Se si verifica un problema, WebMatrix rappresenta il modo migliore per indicare il problema:

![Messaggio di errore se si verifica un problema con le impostazioni di pubblicazione](publishing/_static/image16.png)

Per salvare le impostazioni, fare clic su **Save** . WebMatrix consente di eseguire un test per assicurarsi che sia in grado di comunicare correttamente con il sito di hosting:

![Offerta di messaggi per eseguire un test del processo di pubblicazione](publishing/_static/image17.png)

Fare clic su **Sì**. WebMatrix carica alcuni file di esempio nel provider di hosting. Al termine del test di compatibilità, WebMatrix segnala i risultati:

![Risultati del test di pubblicazione](publishing/_static/image18.png)

Se si è pronti, fare clic su **continue (continua** ) per avviare il processo di pubblicazione per il vero. WebMatrix indica i file presenti nel sito e si trovano già nel server host (attualmente, nessuno) e offre un'anteprima del processo di pubblicazione:

![Anteprima dei file che vengono caricati dal processo di pubblicazione](publishing/_static/image19.png)

L'elenco dei file da pubblicare include le pagine Web create come *Movies. cshtml*. L'elenco include anche i file per gli helper installati, i file da eseguire SQL Server Compact edizione per il database e così via. Di conseguenza, il processo di pubblicazione iniziale può essere sostanziale.

Fare clic su **Continue**. WebMatrix copia i file nel server del provider di hosting. Al termine, i risultati vengono segnalati nella barra di stato:

![Messaggio della barra di stato al termine del processo di pubblicazione](publishing/_static/image20.png)

Per visualizzare il sito attivo, fare clic sul collegamento nella barra di stato. Aggiungere i *film* all'URL. verrà visualizzato il file *Movies. cshtml* creato:

![Sito Live che mostra la pagina dei filmati](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>Aggiornamento del sito Live: ripubblicazione

Dopo aver pubblicato il sito (in Azure o in una società di hosting Web), sono presenti due copie della &mdash; versione nel computer e la versione del provider di servizi. È probabile che si voglia continuare a sviluppare il sito (se non sono disponibili altre operazioni, come parte del prossimo set di esercitazioni). Quando si esegue questa operazione, è necessario ripubblicare il sito per copiare le modifiche dal computer al provider di servizi. Il processo di pubblicazione in WebMatrix può determinare quali file sono stati modificati nel sito e pubblicare solo quei file.

Per verificare il funzionamento della ripubblicazione, aprire il sito *Movies. cshtml* , apportare alcune piccole modifiche e quindi salvare il file. Ad esempio, modificare il titolo in `Movies - Updated` .

Fare clic sul pulsante **pubblica** sulla barra multifunzione. WebMatrix determina le modifiche apportate e Mostra un'anteprima dei file che verrà pubblicata.

![Finestra di dialogo ' pubblica ' che mostra i file modificati pronti per la ripubblicazione](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> Per impostazione predefinita, WebMatrix pubblica il database (file*SDF* ) solo la prima volta che si pubblica il sito. Una volta pubblicato il sito e le persone interagiscono con il sito Web, il database nel sito Live contiene in genere i dati reali del sito. È necessario prestare molta attenzione a non sovrascrivere il database attivo con il file *SDF* presente nel computer, che in genere contiene solo dati di test. Questo è il motivo per cui viene visualizzato che la pubblicazione degli avvisi **sovrascrive tutti i database remoti**e il motivo per cui la casella di controllo per *WebPagesMovies. sdf* è deselezionata per impostazione predefinita.

Fare clic su **Continue**. WebMatrix pubblica i file modificati e Mostra un messaggio di operazione completata, come per la prima volta che è stato pubblicato.

Passare al sito attivo (è possibile fare clic sul collegamento nel messaggio di operazione riuscita se è ancora visualizzato) e verificare che la modifica sia stata pubblicata.

> [!TIP] 
> 
> **Modifica di file in modalità remota**
> 
> In alternativa alla modifica del sito e alla successiva ripubblicazione, è possibile modificare i file remoti direttamente in WebMatrix. In questo scenario si apre un file che si trova nel provider di servizi e WebMatrix ne Scarica una copia per la modifica. Ogni volta che si salva il file, WebMatrix Invia le modifiche al sito.
> 
> La modifica remota è un modo semplice per apportare modifiche al sito Live. Tuttavia, le modifiche apportate in questo modo non vengono sincronizzate con i file presenti nel sito locale. Per sincronizzare i file locali con il sito remoto, è possibile scaricare i file remoti. Questo processo funziona in modo molto simile alla pubblicazione, tranne che in senso inverso.
> 
> Qui non verranno descritte altre funzionalità di WebMatrix per la modifica remota e il download remoto. Sono molto utili se più persone devono lavorare sullo stesso sito in computer diversi. Per altre informazioni, vedere [pubblicare e modificare un sito remoto con WebMatrix 2 beta](https://go.microsoft.com/fwlink/?LinkId=251591).

## <a name="additional-resources"></a>Risorse aggiuntive

- [ASP.NET WebMatrix pagine Web ASP.net Forum](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), un posto ideale per inviare domande e ottenere risposte.

> [!div class="step-by-step"]
> [Indietro](layouts.md)
