---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: Miglioramento delle prestazioni con la memorizzazione nella cache di output (C ) Documenti Microsoft
author: rick-anderson
description: In questa esercitazione si apprenderà come migliorare notevolmente le prestazioni delle applicazioni Web MVC ASP.NET sfruttando la memorizzazione nella cache di output. tu...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: a6dd43320117e235d12a13aa894302bbc767727c
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542716"
---
# <a name="improving-performance-with-output-caching-c"></a>Miglioramento delle prestazioni con la cache di output (C#)

da parte [di Microsoft](https://github.com/microsoft)

> In questa esercitazione si apprenderà come migliorare notevolmente le prestazioni delle applicazioni Web MVC ASP.NET sfruttando la memorizzazione nella cache di output. Si apprenderà come memorizzare nella cache il risultato restituito da un'azione del controller in modo che non sia necessario creare lo stesso contenuto ogni volta che un nuovo utente richiama l'azione.

L'obiettivo di questa esercitazione è spiegare come è possibile migliorare notevolmente le prestazioni di un'applicazione MVC ASP.NET sfruttando la cache di output. La cache di output consente di memorizzare nella cache il contenuto restituito da un'azione del controller. In questo modo, non è necessario generare lo stesso contenuto ogni volta che viene richiamata la stessa azione del controller.

Si supponga, ad esempio, che l'applicazione ASP.NET MVC visualizza un elenco di record di database in una visualizzazione denominata Indice. In genere, ogni volta che un utente richiama l'azione del controller che restituisce la visualizzazione dell'indice, il set di record del database deve essere recuperato dal database eseguendo una query di database.

Se, d'altra parte, si sfrutta la cache di output, è possibile evitare di eseguire una query di database ogni volta che un utente richiama la stessa azione del controller. La visualizzazione può essere recuperata dalla cache anziché essere rigenerata dall'azione del controller. La memorizzazione nella cache consente di evitare di eseguire operazioni ridondanti sul server.

## <a name="enabling-output-caching"></a>Abilitazione della memorizzazione nella cache di outputEnabling Output Caching

Per abilitare la memorizzazione nella cache di output, aggiungere un attributo [OutputCache] a una singola azione del controller o a un'intera classe controller. Ad esempio, il controller nel listato 1 espone un'azione denominata Index(). L'output dell'azione Index() viene memorizzato nella cache per 10 secondi.

**Listato 1 – Controller-HomeController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

Nelle versioni Beta di ASP.NET MVC, la memorizzazione [http://www.MySite.com/](http://www.mysite.com/)nella cache di output non funziona per un URL come . È invece necessario immettere un URL come [http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index). 

Nel listato 1, l'output dell'azione Index() viene memorizzato nella cache per 10 secondi. Se si preferisce, è possibile specificare una durata della cache molto più lunga. Ad esempio, se si desidera memorizzare nella cache l'output di un'azione del controller per un \* giorno, \* è possibile specificare una durata della cache di 86400 secondi (60 secondi 60 minuti 24 ore).

Non vi è alcuna garanzia che il contenuto verrà memorizzato nella cache per il periodo di tempo specificato. Quando le risorse di memoria diventano inesaurite, la cache inizia a rimuovere automaticamente il contenuto.

Il controller Home nel listato 1 restituisce la visualizzazione Dell'indice nel listato 2. Non c'è niente di speciale in questo punto di vista. La vista Indice visualizza semplicemente l'ora corrente (vedere Figura 1).

**Listato 2 – Visualizzazioni, Home, Indice.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

**Figura 1 – Visualizzazione dell'indice memorizzato nella cache**

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

Se invocate l'azione Indice( più volte immettendo l'URL /Home/Index nella barra degli indirizzi del browser e premendo ripetutamente il pulsante Aggiorna/Ricarica nel browser), il tempo visualizzato dalla vista Indice non cambierà per 10 secondi. Viene visualizzata la stessa ora perché la visualizzazione viene memorizzata nella cache.

È importante comprendere che la stessa visualizzazione viene memorizzata nella cache per tutti gli utenti che visitano l'applicazione. Chiunque richiami l'azione Index() otterrà la stessa versione memorizzata nella cache della vista Indice. Ciò significa che la quantità di lavoro che il server web deve eseguire per servire la visualizzazione indice è drasticamente ridotta.

La vista nella lista 2 sembra fare qualcosa di veramente semplice. La vista mostra solo l'ora corrente. Tuttavia, è possibile memorizzare nella cache una visualizzazione nella cache con cui viene visualizzato un set di record del database. In tal caso, non è necessario recuperare il set di record del database dal database ogni volta che viene richiamata l'azione del controller che restituisce la visualizzazione. La memorizzazione nella cache può ridurre la quantità di lavoro che il server Web e il server di database devono eseguire.

Non utilizzare la &lt;pagina % ,&gt; OutputCache % direttiva in una visualizzazione MVC. Questa direttiva non viene eseguito dal mondo Web Form e non deve essere utilizzata in un'applicazione MVC ASP.NET.

## <a name="where-content-is-cached"></a>Dove il contenuto viene memorizzato nella cacheWhere Content is Cached

Per impostazione predefinita, quando si utilizza l'attributo [OutputCache], il contenuto viene memorizzato nella cache in tre posizioni: il server Web, tutti i server proxy e il browser Web. È possibile controllare esattamente dove il contenuto viene memorizzato nella cache modificando il Location proprietà del [OutputCache] attributo.

È possibile impostare la proprietà Location su uno dei seguenti valori:

> · Qualsiasi
> 
> · Client
> 
> · Downstream
> 
> · Server
> 
> · Nessuno
> 
> · ServerAndClient

Per impostazione predefinita, il Location proprietà ha il valore Any.By default, the Location property has the value Any. Tuttavia, esistono situazioni in cui è possibile memorizzare nella cache solo nel browser o solo sul server. Ad esempio, se si memorizzano nella cache informazioni personalizzate per ogni utente, è consigliabile non memorizzare nella cache le informazioni sul server. Se si visualizzano informazioni diverse a utenti diversi, è necessario memorizzare nella cache le informazioni solo sul client.

Ad esempio, il controller nel listato 3 espone un'azione denominata GetName() che restituisce il nome utente corrente. Se Jack accede al sito Web e richiama l'azione GetName(), l'azione restituisce la stringa "Hi Jack". Se, successivamente, Jill accede al sito Web e richiama l'azione GetName(), otterrà anche la stringa "Hi Jack". La stringa viene memorizzata nella cache del server Web per tutti gli utenti dopo che Jack richiama inizialmente l'azione del controller.

**Listato 3 – Controller- BadUserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

Molto probabilmente, il controller nel listato 3 non funziona nel modo desiderato. Non si desidera visualizzare il messaggio "Ciao Jack" a Jill.

Non memorizzare mai nella cache il contenuto personalizzato nella cache del server. Tuttavia, è possibile memorizzare nella cache il contenuto personalizzato nella cache del browser per migliorare le prestazioni. Se si memorizza il contenuto nella cache nel browser e un utente richiama più volte la stessa azione del controller, il contenuto può essere recuperato dalla cache del browser anziché dal server.

Il controller modificato nel listato 4 memorizza nella cache l'output dell'azione GetName(). Tuttavia, il contenuto viene memorizzato nella cache solo nel browser e non nel server. In questo modo, quando più utenti richiamano il metodo GetName(), ogni persona ottiene il proprio nome utente e non il nome utente di un'altra persona.

**Listato 4 – Controllers, UserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

Si noti che l'attributo [OutputCache] nel listato 4 include una proprietà Location impostata sul valore OutputCacheLocation.Client. L'attributo [OutputCache] include anche una proprietà NoStore. Il NoStore proprietà viene utilizzata per informare i server proxy e browser che non devono archiviare una copia permanente del contenuto memorizzato nella cache.

## <a name="varying-the-output-cache"></a>Variazione della cache di outputVarying the Output Cache

In alcune situazioni, potrebbero essere esigenze diverse versioni memorizzate nella cache dello stesso contenuto. Si supponga, ad esempio, che si sta creando una pagina master/dettaglio. La pagina master visualizza un elenco di titoli di film. Quando si fa clic su un titolo, si ottengono i dettagli per il filmato selezionato.

Se si memorizza nella cache la pagina dei dettagli, verranno visualizzati i dettagli per lo stesso filmato, indipendentemente dal filmato su cui si fa clic. Il primo filmato selezionato dal primo utente verrà visualizzato a tutti gli utenti futuri.

È possibile risolvere questo problema sfruttando la proprietà VaryByParam dell'attributo [OutputCache]. Questa proprietà consente di creare diverse versioni memorizzate nella cache dello stesso contenuto quando un parametro del form o un parametro di stringa di query varia.

Ad esempio, il controller nel listato 5 espone due azioni denominate Master() e Details(). L'azione Master() restituisce un elenco di titoli di film e l'azione Details() restituisce i dettagli per il filmato selezionato.

**Listato 5 – Controller-MoviesController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

L'azione Master() include una proprietà VaryByParam con il valore "none". Quando viene richiamata l'azione Master(), viene restituita la stessa versione memorizzata nella cache della visualizzazione master. Tutti i parametri del modulo o i parametri della stringa di query vengono ignorati (vedere la Figura 2).

**Figura 2 – La vista /Movies/Master**

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

**Figura 3 – La visualizzazione /Movies/Details**

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

L'azione Details() include una proprietà VaryByParam con il valore "Id". Quando diversi valori del Id parametro vengono passati all'azione del controller, vengono generate diverse versioni memorizzate nella cache della visualizzazione Dettagli.

È importante comprendere che l'utilizzo della proprietà VaryByParam comporta una maggiore memorizzazione nella cache e non meno. Viene creata una versione memorizzata nella cache diversa della visualizzazione Dettagli per ogni versione diversa del parametro Id.

È possibile impostare la proprietà VaryByParam sui valori seguenti:

> \*Creare una versione memorizzata nella cache diversa ogni volta che un parametro di stringa di query o modulo varia.
> 
> none - Non creare mai versioni memorizzate nella cache diverse
> 
> Elenco di parametri a punto e virgola: consente di creare versioni memorizzate nella cache diverse ogni volta che uno qualsiasi dei parametri della stringa di query o del modulo nell'elenco varia

## <a name="creating-a-cache-profile"></a>Creazione di un profilo di cacheCreating a Cache Profile

In alternativa alla configurazione delle proprietà della cache di output modificando le proprietà dell'attributo [OutputCache], è possibile creare un profilo di cache nel file di configurazione Web (web.config). La creazione di un profilo di cache nel file di configurazione Web offre un paio di vantaggi importanti.

In primo luogo, configurando la memorizzazione nella cache di output nel file di configurazione Web, è possibile controllare il modo in cui le azioni del controller memorizzano il contenuto nella cache in un'unica posizione centrale. È possibile creare un profilo di cache e applicare il profilo a diversi controller o azioni controller.

In secondo luogo, è possibile modificare il file di configurazione Web senza ricompilare l'applicazione. Se è necessario disabilitare la memorizzazione nella cache per un'applicazione che è già stata distribuita nell'ambiente di produzione, è sufficiente modificare i profili di cache definiti nel file di configurazione Web. Eventuali modifiche al file di configurazione Web verranno rilevate automaticamente e applicate.

Ad esempio, &lt;la&gt; sezione di configurazione Web di memorizzazione nella cache nel listato 6 definisce un profilo di cache denominato Cache1Hour.For example, the caching web configuration section in Listing 6 defines a cache profile named Cache1Hour. La &lt;sezione&gt; di memorizzazione &lt;nella&gt; cache deve essere visualizzata all'interno della sezione system.web di un file di configurazione Web.

**Listato 6 – Sezione di memorizzazione nella cache per web.configListing 6 – Caching section for web.config**

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

Il controller nel listato 7 illustra come è possibile applicare il profilo Cache1Hour a un'azione del controller con l'attributo [OutputCache].

**Listato 7 – Controllers-ProfileController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

Se si richiama l'azione Index() esposta dal controller nel listato 7, verrà restituito lo stesso orario per 1 ora.

## <a name="summary"></a>Riepilogo

La memorizzazione nella cache di output offre un metodo molto semplice per migliorare notevolmente le prestazioni delle applicazioni MVC ASP.NET. In questa esercitazione è stato illustrato come usare l'attributo [OutputCache] per memorizzare nella cache l'output delle azioni del controller. È stato inoltre illustrato come modificare le proprietà dell'attributo [OutputCache], ad esempio le proprietà Duration e VaryByParam, per modificare la modalità di memorizzazione nella cache del contenuto. Infine, è stato illustrato come definire i profili di cache nel file di configurazione Web.

> [!div class="step-by-step"]
> [Successivo](understanding-action-filters-cs.md)
> [precedente](adding-dynamic-content-to-a-cached-page-cs.md)
