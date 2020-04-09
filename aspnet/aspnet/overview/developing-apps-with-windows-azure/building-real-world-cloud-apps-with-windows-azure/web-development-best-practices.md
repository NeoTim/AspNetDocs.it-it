---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Procedure consigliate per lo sviluppo Web (creazione di app cloud reali con Azure) Documenti Microsoft
author: MikeWasson
description: L'e-book Building Real World Cloud Apps with Azure si basa su una presentazione sviluppata da Scott Guthrie. Spiega 13 schemi e pratiche che possono...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: dfd8a3ac2328d3f17dfbe36e68b37d181177b0f4
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675999"
---
# <a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Web Development Best Practices (Building Real-World Cloud Apps with Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Scarica Fix It Project](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica E-book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> L'e-book **Building Real World Cloud Apps with Azure** si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare con successo app Web per il cloud. Per informazioni sull'e-book, vedere [il primo capitolo](introduction.md).

I primi tre modelli riguardavano l'impostazione di un processo di sviluppo agile; il resto riguarda l'architettura e il codice. Questa è una raccolta di best practice per lo sviluppo web:

- [Server Web senza stato](#stateless) dietro un servizio di bilanciamento del carico intelligente.
- [Evitare lo stato della sessione](#sessionstate) (o se non è possibile evitarlo, utilizzare la cache distribuita anziché un database).
- [Usare una rete CDN](#cdn) per memorizzare nella cache edge le risorse di file statici (immagini, script).
- [Usare il supporto asincrono di .NET 4.5](#async) per evitare di bloccare le chiamate.

Queste procedure sono valide per tutto lo sviluppo Web, non solo per le app cloud, ma sono particolarmente importanti per le app cloud. Collaborano per aiutarti a sfruttare al tempo ottimale la scalabilità altamente flessibile offerta dall'ambiente cloud. Se non si seguono queste procedure, si incorrererà in limitazioni quando si tenta di ridimensionare l'applicazione.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Livello Web senza stato dietro un servizio di bilanciamento del carico intelligenteStateless web tier behind a smart load balancer

*Il livello Web senza stato* significa che non si archiviano i dati dell'applicazione nella memoria del server Web o nel file system. Mantenere il livello Web senza stato consente di fornire una migliore esperienza del cliente e di risparmiare denaro:

- Se il livello Web è senza stato e si trova dietro un servizio di bilanciamento del carico, è possibile rispondere rapidamente alle modifiche nel traffico delle applicazioni aggiungendo o rimuovendo server in modo dinamico. Nell'ambiente cloud in cui si paga solo per le risorse del server per tutto il tempo in cui le si utilizza effettivamente, la capacità di rispondere ai cambiamenti della domanda può tradursi in enormi risparmi.
- Un livello Web senza stato è molto più semplice per la scalabilità orizzontale dell'applicazione. Anche questo ti consente di rispondere più rapidamente alle esigenze di scalabilità e di spendere meno denaro per lo sviluppo e i test nel processo.
- I server cloud, come i server locali, devono essere sottoposti a patch e riavviati occasionalmente; e se il livello Web è senza stato, il reinstradamento del traffico quando un server si arresta temporaneamente non causerà errori o comportamenti imprevisti.

La maggior parte delle applicazioni reali è necessario archiviare lo stato per una sessione Web. il punto principale qui non è quello di memorizzarlo sul server web. È possibile archiviare lo stato in altri modi, ad esempio sul client nei cookie o sul lato server out-of-process in ASP.NET stato della sessione utilizzando un provider di cache. È possibile archiviare i file [nell'archivio BLOB](unstructured-blob-storage.md) di Windows Azure anziché nel file system locale.

Come esempio di come sia facile ridimensionare un'applicazione nei siti Web di Windows Azure se il livello Web è senza stato, vedere la scheda **Scalabilità** per un sito Web di Windows Azure nel portale di gestione:

![Scheda Scalabilità](web-development-best-practices/_static/image1.png)

Se si desidera aggiungere server Web, è sufficiente trascinare il dispositivo di scorrimento del numero di istanze verso destra. Impostarlo su 5 e fare clic su **Salva**e in pochi secondi sono disponibili 5 server Web in Windows Azure che gestiscono il traffico del sito Web.

![Cinque istanze](web-development-best-practices/_static/image2.png)

È possibile impostare con la stessa facilità il conto alla rovescia dell'istanza su 3 o su 1. Quando si esegue il ridimensionamento, si inizia a risparmiare denaro immediatamente perché Windows Azure viene addebitato in base al minuto, non all'ora.

È inoltre possibile indicare a Windows Azure di aumentare o ridurre automaticamente il numero di server Web in base all'utilizzo della CPU. Nell'esempio seguente, quando l'utilizzo della CPU scende al di sotto del 60%, il numero di server Web scenderà a un minimo di 2 e se l'utilizzo della CPU supera l'80%, il numero di server Web verrà aumentato fino a un massimo di 4.

![Scalabilità in base all'utilizzo della CPU](web-development-best-practices/_static/image3.png)

O se sai che il tuo sito sarà occupato solo durante l'orario di lavoro? È possibile indicare a Windows Azure di eseguire più server durante il giorno e di ridursi a un singolo server serali, notti e fine settimana. La seguente serie di schermate mostra come configurare il sito Web per l'esecuzione di un server in orari non lavorative e 4 server durante le ore lavorative dalle 8:00 alle 17:00.

![Scalabilità in base alla pianificazione](web-development-best-practices/_static/image4.png)

![Impostare gli orari di pianificazione](web-development-best-practices/_static/image5.png)

![Pianificazione diurna](web-development-best-practices/_static/image6.png)

![Programma della settimana](web-development-best-practices/_static/image7.png)

![Pianificazione del fine settimana](web-development-best-practices/_static/image8.png)

E, naturalmente, tutto questo può essere fatto in script così come nel portale.

La capacità dell'applicazione di scalare orizzontalmente è quasi illimitata in Windows Azure, purché si evitino impedimenti all'aggiunta o alla rimozione dinamica delle macchine virtuali del server, mantenendo il livello Web senza stato.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Evitare lo stato della sessione

In un'app reale per il cloud spesso non è facile evitare di archiviare qualche forma di stato per una sessione utente, ma alcuni approcci hanno più effetto di altri sulle prestazioni e sulla scalabilità. Se è necessario archiviare lo stato, la soluzione migliore è mantenere piccola la quantità di stato e archiviarla nei cookie. Se ciò non è fattibile, la soluzione migliore successiva consiste nell'utilizzare ASP.NET stato sessione con un provider per la [cache distribuita in memoria.](distributed-caching.md#sessionstate) La soluzione peggiore dal punto di vista delle prestazioni e della scalabilità è usare un provider di stato della sessione supportato da un database.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Usare una rete CDN per memorizzare nella cache gli asset di file staticiUse a CDN to cache static file assets

CDN è l'acronimo di Content Delivery Network. Gli asset di file statici, ad esempio immagini e file di script, vengono memorizzati nella cache e il provider li memorizza nella cache nei data center di tutto il mondo, in modo che ovunque gli utenti accedano all'applicazione, ottenete una risposta relativamente rapida e una bassa latenza per gli asset memorizzati nella cache. Ciò velocizza il tempo di caricamento complessivo del sito e riduce il carico sui server web. Le reti CDN sono particolarmente importanti se si raggiunge un pubblico ampiamente distribuito geograficamente.

Windows Azure dispone di una rete CDN ed è possibile usare altre reti CDN in un'applicazione in esecuzione in Windows Azure o in qualsiasi ambiente di hosting Web.Windows Azure has a CDN, and you can use other CDNs in an application that runs in Windows Azure or any web hosting environment.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Usare il supporto asincrono di .NET 4.5 per evitare il blocco delle chiamateUse .NET 4.5's async support to avoid blocking calls

.NET 4.5 ha migliorato i linguaggi di programmazione di VB e c'è per rendere molto più semplice gestire le attività in modo asincrono. Il vantaggio della programmazione asincrona non è solo per situazioni di elaborazione parallela, ad esempio quando si desidera avviare più chiamate al servizio web contemporaneamente. Consente inoltre al server Web di ottenere prestazioni più efficienti e affidabili in condizioni di carico elevato. Un server Web ha solo un numero limitato di thread disponibili e in condizioni di carico elevato quando tutti i thread sono in uso, le richieste in ingresso devono attendere fino a quando i thread vengono liberati. Se il codice dell'applicazione non gestisce attività come le query di database e le chiamate al servizio Web in modo asincrono, molti thread sono inutilmente legati mentre il server è in attesa di una risposta di I/O. Ciò limita la quantità di traffico che il server può gestire in condizioni di carico elevato. Con la programmazione asincrona, i thread in attesa che un servizio Web o un database restituiscano i dati vengono liberati per soddisfare le nuove richieste fino alla ricezione dei dati. In un server web occupato, centinaia o migliaia di richieste possono quindi essere elaborate prontamente che altrimenti sarebbe in attesa di thread da liberare.

Come si è visto in precedenza, è facile ridurre il numero di server web che gestiscono il tuo sito web come è quello di aumentarli. Pertanto, se un server può ottenere una velocità effettiva maggiore, non è necessario altrettanto di essi ed è possibile ridurre i costi perché è necessario un numero inferiore di server per un determinato volume di traffico rispetto a quanto sarebbe altrimenti.

Il supporto per il modello di programmazione asincrona .NET 4.5 è incluso in ASP.NET 4.5 per Web Form, MVC e API Web; in Entity Framework 6 e [nell'API di archiviazione](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx)di Windows Azure .

### <a name="async-support-in-aspnet-45"></a>Supporto asincrono in ASP.NET 4.5Async support in ASP.NET 4.5

In ASP.NET 4.5, il supporto per la programmazione asincrona è stato aggiunto non solo al linguaggio, ma anche amvc, Web Form e framework API Web. Ad esempio, un metodo di azione del controller MVC ASP.NET riceve i dati da una richiesta Web e passa i dati a una visualizzazione che quindi crea il codice HTML da inviare al browser. Spesso il metodo di azione deve ottenere dati da un database o da un servizio Web per visualizzarli in una pagina Web o per salvare i dati immessi in una pagina Web. In questi scenari è facile rendere asincrono il metodo di azione: anziché restituire un oggetto *ActionResult,* restituire *&lt;Task ActionResult&gt; * e contrassegnare il metodo con la parola chiave *async.* All'interno del metodo, quando una riga di codice avvia un'operazione che prevede il tempo di attesa, contrassegnarla con la parola chiave await.

Di seguito è riportato un metodo di azione semplice che chiama un metodo di repository per una query di database:Here is a simple action method that calls a repository method for a database query:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

Ed ecco lo stesso metodo che gestisce la chiamata al database in modo asincrono:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

Sotto le coperte il compilatore genera il codice asincrono appropriato. Quando l'applicazione effettua `FindTaskByIdAsync`la chiamata `FindTask` a , ASP.NET effettua la richiesta e quindi riavvolge il thread di lavoro e lo rende disponibile per elaborare un'altra richiesta. Al `FindTask` termine della richiesta, viene riavviato un thread per continuare l'elaborazione del codice successivo a tale chiamata. Durante l'intervallo `FindTask` tra quando viene avviata la richiesta e quando vengono restituiti i dati, si dispone di un thread disponibile per eseguire operazioni utili che altrimenti sarebbero legati in attesa della risposta.

Esiste un sovraccarico per il codice asincrono, ma in condizioni di basso carico, tale sovraccarico è trascurabile, mentre in condizioni di carico elevato è possibile elaborare le richieste che altrimenti verrebbero trattenute in attesa di thread disponibili.

È stato possibile eseguire questo tipo di programmazione asincrona fin dalla ASP.NET 1.1, ma era difficile scrivere, soggetto a errori e difficile da eseguire il debug. Ora che abbiamo semplificato la codifica per esso in ASP.NET 4.5, non c'è motivo di non farlo più.

### <a name="async-support-in-entity-framework-6"></a>Supporto asincrono in Entity Framework 6Async support in Entity Framework 6

Come parte del supporto asincrono in 4.5 è stato fornito il supporto asincrono per le chiamate al servizio web, i socket e l'I/O del file system, ma il modello più comune per le applicazioni Web è quello di colpire un database e le librerie di dati non supportano async. Ora Entity Framework 6 aggiunge il supporto asincrono per l'accesso al database.

In Entity Framework 6 tutti i metodi che causano l'invio di una query o di un comando al database hanno versioni asincrone. L'esempio seguente mostra la versione asincrona del *metodo Find.*

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

E questo supporto asincrono funziona non solo per inserti, eliminazioni, aggiornamenti e ricerche semplici, funziona anche con le query LINQ:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

C'è `Async` una versione `ToList` del metodo perché in questo codice che è il metodo che fa sì che una query da inviare al database. I `Where` `OrderByDescending` metodi e configurano solo `ToListAsync` la query, mentre il metodo `result` esegue la query e archivia la risposta nella variabile.

## <a name="summary"></a>Riepilogo

È possibile implementare le procedure consigliate per lo sviluppo Web descritte di seguito in qualsiasi framework di programmazione Web e in qualsiasi ambiente cloud, ma sono disponibili strumenti in ASP.NET e Windows Azure per semplificare l'operazione. Se si seguono questi modelli, è possibile scalare facilmente orizzontalmente il livello Web e ridurre al minimo le spese perché ogni server sarà in grado di gestire più traffico.

Il [capitolo successivo](single-sign-on.md) esamina il modo in cui il cloud abilita gli scenari Single Sign-On.

## <a name="resources"></a>Risorse

Per altre informazioni, vedere le risorse seguenti.

Server Web senza stato:

- [Microsoft Patterns and Practices - Linee guida](https://msdn.microsoft.com/library/dn589774.aspx)per la scalabilità automatica .
- [Disabilitazione dell'affinità dell'istanza di ARR nei siti Web](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/)di Windows Azure. Post di blog di Erez Benari, spiega l'affinità di sessione nei siti Web di Windows Azure.

Cdn:

- [FailSafe: Creazione di servizi cloud scalabili e resilienti](https://channel9.msdn.com/Series/FailSafe). Serie di video in nove parti di Ulrich Homann, Marc Mercuri e Mark Simms. Vedere la discussione CDN nell'episodio 3 a partire da 1:34:00.See the CDN discussion in episode 3 starting at 1:34:00.
- [Modello di hosting di contenuto statico di modelli e procedure MicrosoftMicrosoft Patterns and Practices Static Content Hosting pattern](https://msdn.microsoft.com/library/dn589776.aspx)
- [Recensioni CDN](http://www.cdnreviews.com/). Panoramica di molte reti CDN.

Programmazione asincrona:

- [Utilizzo di metodi asincroni in ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Tutorial di Rick Anderson.
- [Programmazione asincrona con Async e Await (Cè e Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). White paper MSDN che illustra le motivazioni per la programmazione asincrona, come funziona in ASP.NET 4.5 e come scrivere codice per implementarlo.
- [Query asincrona di Entity Framework e salvataggio](https://msdn.microsoft.com/data/jj819165)
- [Come compilare applicazioni Web ASP.NET utilizzando Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Video di Rowan Miller. Include una dimostrazione grafica di come la programmazione asincrona può facilitare un notevole aumento della velocità effettiva del server Web in condizioni di carico elevato.
- [FailSafe: Creazione di servizi cloud scalabili e resilienti](https://channel9.msdn.com/Series/FailSafe). Serie di video in nove parti di Ulrich Homann, Marc Mercuri e Mark Simms. Per discussioni sull'impatto della programmazione asincrona sulla scalabilità, vedere l'episodio 4 e l'episodio 8.
- [La magia dell'utilizzo di metodi asincroni in ASP.NET 4.5 più un importante gotcha](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Post di blog di Scott Hanselman, principalmente sull'utilizzo di async nelle applicazioni Web Form ASP.NET.

Per altre procedure consigliate per lo sviluppo Web, vedere le risorse seguenti:For additional web development best practices, see the following resources:

- [Applicazione di esempio Fix It - Procedure consigliate](the-fix-it-sample-application.md#bestpractices). L'appendice a questo e-book elenca una serie di best practice implementate nell'applicazione Fix It.
- [Elenco di controllo per sviluppatori Web](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Successivo](continuous-integration-and-continuous-delivery.md)
> [precedente](single-sign-on.md)
