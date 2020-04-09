---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Memorizzazione nella cache distribuita (Creazione di app cloud reali con Azure) Documenti Microsoft
author: MikeWasson
description: L'e-book Building Real World Cloud Apps with Azure si basa su una presentazione sviluppata da Scott Guthrie. Spiega 13 schemi e pratiche che possono...
ms.author: riande
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 87a7516415895e761d1589fd459b93e5c15c0f85
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676020"
---
# <a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Memorizzazione nella cache distribuita (creazione di app cloud reali con Azure)Distributed Caching (Building Real-World Cloud Apps with Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Scarica Fix It Project](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica E-book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> L'e-book **Building Real World Cloud Apps with Azure** si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare con successo app Web per il cloud. Per informazioni sull'e-book, vedere [il primo capitolo](introduction.md).

Il capitolo precedente ha esaminato la gestione degli errori temporanei e ha menzionato la memorizzazione nella cache come strategia di interruttore. Questo capitolo fornisce ulteriori informazioni sulla memorizzazione nella cache, tra cui quando usarla, modelli comuni per usarlo e come implementarla in Azure.This chapter gives more background about caching, including when to use it, common patterns for using it, and how to implement it in Azure.

## <a name="what-is-distributed-caching"></a>Che cos'è la memorizzazione nella cache distribuitaWhat is distributed caching

Una cache fornisce un accesso ad alta velocità effettiva e a bassa latenza ai dati dell'applicazione a cui si accede di comune, archiviando i dati in memoria. Per un'app cloud il tipo di cache più utile è la cache distribuita, il che significa che i dati non vengono archiviati nella memoria del singolo server Web, ma in altre risorse cloud e i dati memorizzati nella cache vengono resi disponibili a tutti i server Web di un'applicazione (o altre macchine virtuali cloud utilizzate dall'applicazione).

![Diagramma che mostra più server Web che accedono agli stessi server di cache](distributed-caching/_static/image1.png)

Quando l'applicazione viene ridimensionata aggiungendo o rimuovendo server o quando i server vengono sostituiti a causa di aggiornamenti o errori, i dati memorizzati nella cache rimangono accessibili a tutti i server che eseguono l'applicazione.

Evitando l'accesso ai dati ad alta latenza di un archivio dati persistente, la memorizzazione nella cache può migliorare notevolmente la velocità di risposta dell'applicazione. Ad esempio, il recupero dei dati dalla cache è molto più veloce rispetto al recupero da un database relazionale.

Un vantaggio collaterale della memorizzazione nella cache è la riduzione del traffico verso l'archivio dati persistente, che può comportare costi inferiori in caso di costi di uscita dei dati per l'archivio dati persistente.

## <a name="when-to-use-distributed-caching"></a>Quando usare la memorizzazione nella cache distribuitaWhen to use distributed caching

La memorizzazione nella cache funziona meglio per i carichi di lavoro dell'applicazione che consentono di eseguire più letture rispetto alla scrittura dei dati e quando il modello di dati supporta l'organizzazione chiave/valore usata per archiviare e recuperare i dati nella cache. È anche più utile quando gli utenti dell'applicazione condividono molti dati comuni; ad esempio, la cache non fornirebbe tanti vantaggi se ogni utente recupera in genere dati univoci per tale utente. Un esempio in cui la memorizzazione nella cache potrebbe essere molto utile è un catalogo di prodotti, perché i dati non cambiano frequentemente e tutti i clienti stanno esaminando gli stessi dati.

Il vantaggio della memorizzazione nella cache diventa sempre più misurabile quanto più un'applicazione viene ridimensionata, poiché i limiti di velocità effettiva e i ritardi di latenza dell'archivio dati persistente diventano più di un limite alle prestazioni complessive dell'applicazione. Tuttavia, è possibile implementare la memorizzazione nella cache anche per motivi diversi dalle prestazioni. Per i dati che non devono essere perfettamente aggiornati quando vengono visualizzati all'utente, l'accesso alla cache può fungere da interruttore per quando l'archivio dati persistente non risponde o non è disponibile.

## <a name="popular-cache-population-strategies"></a>Strategie di popolazione cache più popolari

Per poter recuperare i dati dalla cache, è necessario prima archiviarli. Esistono diverse strategie per ottenere i dati necessari in una cache:There are several strategies for getting data that you need into a cache:

- Su richiesta / Cache Aside

    L'applicazione tenta di recuperare i dati dalla cache e quando la cache non dispone dei dati (un "miss"), l'applicazione archivia i dati nella cache in modo che siano disponibili la volta successiva. La volta successiva che l'applicazione tenta di ottenere gli stessi dati, trova ciò che sta cercando nella cache (un "hit"). Per impedire il recupero dei dati memorizzati nella cache che sono stati modificati nel database, invalidare la cache quando si apportano modifiche all'archivio dati.
- Push dei dati in background

    I servizi in background inviano i dati nella cache in base a una pianificazione regolare e l'app esegue sempre il pull dalla cache. Questo approccio funziona alla grande con origini dati ad alta latenza che non richiedono di restituire sempre i dati più recenti.
- Interruttore

    L'applicazione comunica in genere direttamente con l'archivio dati persistente, ma quando l'archivio dati persistente presenta problemi di disponibilità, recupera i dati dalla cache. È possibile che i dati siano stati inseriti nella cache utilizzando la strategia di push dei dati in background o della cache. Si tratta di una strategia di gestione degli errori piuttosto che di una strategia di miglioramento delle prestazioni.

Per mantenere aggiornati i dati nella cache, è possibile eliminare le voci della cache correlate quando l'applicazione crea, aggiorna o elimina i dati. Se è corretto per l'applicazione ottenere a volte dati leggermente obsoleti, è possibile fare affidamento su un'ora di scadenza configurabile per impostare un limite sulla vecchiaia dei dati della cache.

È possibile configurare la scadenza assoluta (quantità di tempo dalla creazione dell'elemento della cache) o la scadenza variabile (quantità di tempo dall'ultimo accesso a un elemento della cache). La scadenza assoluta viene utilizzata quando si dipende dal meccanismo di scadenza della cache per evitare che i dati diventino troppo obsoleti. Nell'app Fix It, rimuoveremo manualmente gli elementi della cache non aggiornati e useremo la scadenza variabile per mantenere i dati più aggiornati nella cache. Indipendentemente dal criterio di scadenza scelto, la cache rimuovere automaticamente gli elementi meno recenti (usati meno di recente o LRU) quando viene raggiunto il limite di memoria della cache.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Esempio di codice cache-aside per l'app Fix It

Nel codice di esempio seguente viene controllata prima la cache durante il recupero di un'attività Fix It. Se l'attività viene trovata nella cache, la restituiamo; se non viene trovato, lo otteniamo dal database e lo memorizziamo nella cache. Le modifiche apportate per aggiungere la `FindTaskByIdAsync` memorizzazione nella cache al metodo vengono evidenziate.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Quando si aggiorna o si elimina un'attività Fix It, è necessario invalidare (rimuovere) l'attività memorizzata nella cache. In caso contrario, i tentativi futuri di leggere l'attività continueranno a ottenere i dati precedenti dalla cache.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Si tratta di esempi per illustrare il codice di memorizzazione nella cache semplice; la memorizzazione nella cache non è stata implementata nel progetto Fix It scaricabile.

## <a name="azure-caching-services"></a>Servizi di memorizzazione nella cache di AzureAzure caching services

Azure offre i servizi di memorizzazione nella cache seguenti: [Cache redis](https://msdn.microsoft.com/library/dn690523.aspx) di Azure e [Cache gestita di Azure.](https://msdn.microsoft.com/library/dn386094.aspx) La cache di Azure Redis si basa sulla cache [Redis open source](http://redis.io/) più diffusa ed è la prima scelta per la maggior parte degli scenari di memorizzazione nella cache.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>ASP.NET lo stato della sessione utilizzando un provider di cache

Come indicato nel [capitolo sulle procedure consigliate per lo sviluppo Web](web-development-best-practices.md), è consigliabile evitare l'utilizzo dello stato sessione. Se l'applicazione richiede lo stato sessione, la procedura consigliata successiva consiste nell'evitare il provider in memoria predefinito perché ciò non abilita la scalabilità orizzontale (più istanze del server Web). Il provider dello stato sessione di ASP.NET SQL ServerSQL Server consente a un sito in esecuzione su più server Web di utilizzare lo stato sessione, ma comporta un costo di latenza elevato rispetto a un provider in memoria. La soluzione migliore se è necessario usare lo stato sessione consiste nell'usare un provider di cache, ad esempio il [provider dello stato sessione per la cache](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx)di Azure .

## <a name="summary"></a>Riepilogo

Hai visto come l'app Fix It potrebbe implementare la memorizzazione nella cache per migliorare i tempi di risposta e la scalabilità e per consentire all'app di continuare a rispondere per le operazioni di lettura quando il database non è disponibile. Nel [prossimo capitolo](queue-centric-work-pattern.md) mostreremo come migliorare ulteriormente la scalabilità e rendere l'app continua a essere reattiva per le operazioni di scrittura.

## <a name="resources"></a>Risorse

Per altre informazioni sulla memorizzazione nella cache, vedere le risorse seguenti.

Documentazione

- [Cache di Azure](https://msdn.microsoft.com/library/gg278356.aspx). Documentazione ufficiale di MSDN sulla memorizzazione nella cache in Azure.
- [Modelli e procedure Microsoft - Indicazioni di Azure](https://msdn.microsoft.com/library/dn568099.aspx).Microsoft Patterns and Practices - Azure Guidance . Vedere Guida alla memorizzazione nella cache e modello Cache-Aside.See Caching guidance and Cache-Aside pattern.
- [Failsafe: guida per le architetture cloud resilienti](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). White paper di Marc Mercuri, Ulrich Homann e Andrew Townhill. Vedere la sezione memorizzazione nella cache.
- [Procedure consigliate per la progettazione di servizi su larga scala in Servizi cloud](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)di Azure. W. White paper di Mark Simms e Michael Thomassy. Vedere la sezione sulla memorizzazione nella cache distribuita.
- [Memorizzazione nella cache distribuita nel percorso verso la scalabilità](https://msdn.microsoft.com/magazine/dd942840.aspx). Un vecchio (2009) articolo di MSDN Magazine, ma un'introduzione chiaramente scritta alla memorizzazione nella cache distribuita in generale; più approfondito rispetto alle sezioni di memorizzazione nella cache dei white paper FailSafe e Best Practices.

Video

- [FailSafe: Creazione di servizi cloud scalabili e resilienti](https://channel9.msdn.com/Series/FailSafe). Serie in nove parti di Ulrich Homann, Marc Mercuri e Mark Simms. Presenta una visualizzazione di 400 livelli su come progettare le app cloud. Questa serie si concentra sulla teoria e sui motivi per cui; per ulteriori dettagli, vedere la serie Building Big di Mark Simms. Vedere la discussione sulla memorizzazione nella cache nell'episodio 3 a partire da 1:24:14.
- [Building Big: Lezioni apprese dai clienti](https://channel9.msdn.com/Events/Build/2012/3-029)di Azure - Parte I . Simon Davies illustra la memorizzazione nella cache distribuita a partire dalle 46:00. Simile alla serie Failsafe, ma va in più dettagli procedura. La presentazione è stata data il 31 ottobre 2012, pertanto non copre il servizio di memorizzazione nella cache delle app Web nel servizio app di Azure introdotto nel 2013.

Esempio di codice

- [Nozioni fondamentali sui](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)servizi cloud in Azure. Applicazione di esempio che implementa la memorizzazione nella cache distribuita. Vedere il post di blog di accompagnamento [Cloud Service Fundamentals – Caching Basics](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Successivo](transient-fault-handling.md)
> [precedente](queue-centric-work-pattern.md)
