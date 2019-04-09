---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: Miglioramento delle prestazioni con Output di memorizzazione nella cache (c#) | Microsoft Docs
author: microsoft
description: In questa esercitazione descrive come è possibile migliorare notevolmente le prestazioni delle applicazioni web ASP.NET MVC, sfruttando i vantaggi della memorizzazione nella cache di output. È...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: 5dd5b96d0365c55cbbfa2dfe0856beda41f915e1
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59384766"
---
# <a name="improving-performance-with-output-caching-c"></a>Miglioramento delle prestazioni con la cache di output (C#)

by [Microsoft](https://github.com/microsoft)

> In questa esercitazione descrive come è possibile migliorare notevolmente le prestazioni delle applicazioni web ASP.NET MVC, sfruttando i vantaggi della memorizzazione nella cache di output. Informazioni su come memorizzare nella cache il risultato restituito da un'azione del controller in modo che lo stesso contenuto non devono essere creati ogni volta che un nuovo utente richiama l'azione.


L'obiettivo di questa esercitazione è spiegare come si possono migliorare notevolmente le prestazioni di un'applicazione ASP.NET MVC, sfruttando i vantaggi della cache di output. La cache di output consente di memorizzare nella cache il contenuto restituito da un'azione del controller. In questo modo, lo stesso contenuto non dovrà essere generato ogni volta che viene richiamata l'azione del controller stesso.

Si supponga, ad esempio, che l'applicazione ASP.NET MVC Visualizza un elenco di record di database in una visualizzazione denominata indice. In genere, ogni volta che un utente richiama l'azione del controller che restituisce la visualizzazione dell'indice, il set di record di database di tipo deve essere recuperato dal database eseguendo una query sul database.

Se, d'altra parte, è possibile sfruttare la cache di output è possibile evitare l'esecuzione di una query sul database ogni volta che un utente richiama l'azione del controller stesso. La vista può essere recuperata dalla cache anziché essere rigenerato dall'azione del controller. La memorizzazione nella cache attiva è possibile evitare di eseguire ridondanti funzionano nel server.

## <a name="enabling-output-caching"></a>Abilitare la memorizzazione nella cache di Output

Abilitare la memorizzazione nella cache di output mediante l'aggiunta di un attributo [OutputCache] a un'azione di singoli controller o una classe intero controller. Ad esempio, il controller nel listato 1 espone un'azione denominata index (). L'output dell'azione Index () viene memorizzato nella cache per 10 secondi.

**Listato 1 – controllers\homecontroller.cs.**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

Nelle versioni Beta di ASP.NET MVC, la memorizzazione nella cache di output non funziona per un URL come [ http://www.MySite.com/ ](http://www.mysite.com/). In alternativa, è necessario immettere un URL come [ http://www.MySite.com/Home/Index ](http://www.mysite.com/Home/Index). 

Nel listato 1, l'output dell'azione Index () viene memorizzato nella cache per 10 secondi. Se si preferisce, è possibile specificare una durata molto più tempo della cache. Ad esempio, se si vuole memorizzare nella cache l'output di un'azione del controller per un giorno è possibile specificare una durata della cache di 86400 secondi (60 secondi \* 60 minuti \* 24 ore).

Vi è alcuna garanzia che contenuto non nella cache per la quantità di tempo specificato. Quando le risorse di memoria diventano insufficiente, la cache viene avviata automaticamente la rimozione del contenuto.

Il controller Home nel listato 1 restituisce la visualizzazione dell'indice nel listato 2. Non è niente di speciale su questa vista. La visualizzazione dell'indice visualizza semplicemente l'ora corrente (vedere la figura 1).

**Listato 2 – Views\Home\Index.aspx.**

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

**Figura 1 – vista Index memorizzata nella cache**

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

Se si richiama l'azione Index () più volte immettendo l'URL avremo/indice nella barra degli indirizzi del browser e premendo il pulsante di aggiornamento/ricaricamento nel browser più volte, l'ora visualizzata per la visualizzazione dell'indice non verrà modificato per 10 secondi. Contemporaneamente viene visualizzato perché la vista viene memorizzato nella cache.

È importante comprendere che la stessa vista viene memorizzato nella cache per chiunque visiti l'applicazione. Tutti coloro che richiama l'azione Index () otterrà la stessa versione memorizzata nella cache della visualizzazione Index. Ciò significa che la quantità di lavoro che il server web deve eseguire per servire la visualizzazione dell'indice viene notevolmente ridotto.

La visualizzazione nel listato 2 si verifica per eseguire un'operazione molto semplice. La visualizzazione Mostra solo l'ora corrente. Tuttavia, è possibile solo come cache con facilità una vista che viene visualizzato un set di record di database. In tal caso, il set di record del database non sarebbe necessario da recuperare dal database ogni volta che viene richiamata l'azione del controller che restituisce la visualizzazione. La memorizzazione nella cache, è possibile ridurre la quantità di lavoro che deve eseguire il server web e server di database.

Non utilizzare la pagina &lt;% @ OutputCache %&gt; direttiva in una visualizzazione MVC. Questa direttiva sanguinare dal mondo Web Form e non deve essere utilizzata in un'applicazione ASP.NET MVC.

## <a name="where-content-is-cached"></a>In cui viene memorizzato nella cache il contenuto

Per impostazione predefinita, quando si usa l'attributo [OutputCache], il contenuto viene memorizzato nella cache in tre posizioni: il server web, tutti i server proxy e il web browser. È possibile controllare esattamente in cui il contenuto viene memorizzato nella cache modificando la proprietà Location dell'attributo [OutputCache].

È possibile impostare la proprietà Location a uno dei valori seguenti:

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


Per impostazione predefinita, la proprietà Location contiene il valore Any. Tuttavia, esistono situazioni in cui potrebbe voler cache solo in browser o solo sul server. Ad esempio, se si memorizza informazioni personalizzati per ogni utente quindi è necessario non memorizzare nella cache le informazioni sul server. Se si visualizzano informazioni diverse a utenti diversi è necessario memorizzare nella cache le informazioni solo sul client.

Ad esempio, il controller nel listato 3 espone un'azione denominata GetName() che restituisce il nome dell'utente corrente. Se Jack accede al sito Web e richiama l'azione GetName() quindi l'azione restituisce la stringa "Hi Jack". Se, successivamente, Jill accede al sito Web e richiama l'azione GetName() quindi Lei anche otterrà la stringa "Hi Jack". La stringa viene memorizzato nella cache sul server web per tutti gli utenti dopo Jack richiama inizialmente l'azione del controller.

**Listato 3 – Controllers\BadUserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

Molto probabilmente, il controller nel listato 3 non funzionare nel modo desiderato. Non si desidera visualizzare il messaggio "Jack Hi" a Jill.

Non si devono mai memorizzare nella cache di contenuti personalizzati nella cache del server. Tuttavia, si potrebbe voler memorizzare nella cache il contenuto personalizzato nella cache del browser per migliorare le prestazioni. Se si memorizzano nella cache il contenuto nel browser e un utente richiama l'azione del controller stesso più volte, il contenuto può essere recuperato dalla cache del browser invece del server.

Il controller modificato nel listato 4 memorizza nella cache l'output dell'azione GetName(). Tuttavia, il contenuto rimane nella cache solo in browser e non nel server. In questo modo, quando più utenti richiamano il metodo GetName(), ogni persona che ottiene il proprio nome utente e nome utente della persona non in un'altra.

**Listato 4 – Controllers\UserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

Si noti che l'attributo [OutputCache] nel listato 4 include una proprietà di posizione impostata sul valore OutputCacheLocation.Client. L'attributo [OutputCache] include anche una proprietà NoStore. La proprietà NoStore viene utilizzata per indicare di browser e server proxy che non devono archiviare una copia permanente del contenuto memorizzato nella cache.

## <a name="varying-the-output-cache"></a>Variare la Cache di Output

In alcune situazioni, è possibile utilizzare diverse versioni memorizzate nella cache il contenuto stesso. Si supponga, ad esempio, che si sta creando una pagina master-details. La pagina master consente di visualizzare un elenco di titoli di film. Quando si fa clic su un titolo, si ricevono dettagli per il film selezionato.

Se si memorizza nella cache la pagina dei dettagli, i dettagli per lo stesso film verranno quindi visualizzati indipendentemente da quale film si fa clic su. Verrà visualizzato il primo film selezionato dall'utente prima a tutti gli utenti future.

È possibile risolvere questo problema, sfruttando i vantaggi della proprietà VaryByParam dell'attributo [OutputCache]. Questa proprietà consente di creare diverse versioni memorizzate nella cache del contenuto molto stesso quando un parametro per il form o parametro della stringa di query varia.

Ad esempio, il controller nel listato 5 espone due azioni denominate Master() e Details(). L'azione Master() restituisce un elenco di titoli di film e l'azione Details() restituisce i dettagli per il film selezionato.

**Listato 5 – Controllers\MoviesController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

L'azione Master() includa una proprietà VaryByParam con il valore "none". Viene restituito quando visualizzano il Master() viene richiamata l'azione, la stessa versione memorizzata nella cache del Master. Eventuali parametri del modulo o la stringa di query i parametri vengono ignorati (vedere la figura 2).

**Figura 2: la vista /Movies/Master**

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

**Figura 3 – vista Dettagli/Movies /**

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

L'azione Details() include una proprietà VaryByParam con il valore "Id". Quando vengono passati valori diversi del parametro Id per l'azione del controller, vengono generate diverse versioni memorizzate nella cache della vista Dettagli.

È importante comprendere che con la proprietà VaryByParam determina la memorizzazione nella cache più e meno. Una versione memorizzata nella cache diversa della visualizzazione dettagli viene creata per ogni versione del parametro Id.

È possibile impostare la proprietà VaryByParam sui valori seguenti:

> \* = Crea una versione memorizzata nella cache diversa ogni volta che varia in un parametro di stringa di formato o la query.
> 
> None = mai creare diverse versioni memorizzate nella cache
> 
> Elenco di punti e virgola di parametri = crea diverse versioni memorizzate nella cache, ogni volta che uno dei parametri di stringa query o di modulo nell'elenco varia


## <a name="creating-a-cache-profile"></a>Creazione di un profilo della Cache

Come alternativa alla configurazione delle proprietà della cache di output modificando le proprietà dell'attributo [OutputCache], è possibile creare un profilo della cache nel file di configurazione (Web. config) web. Creazione di un profilo di cache nel file di configurazione web offre due vantaggi importanti.

In primo luogo, configurando la memorizzazione nella cache di output nel file di configurazione web, è possibile controllare come le azioni del controller nella cache il contenuto in un'unica posizione centrale. È possibile creare un profilo della cache e applicare il profilo ai diversi controller o azioni del controller.

In secondo luogo, è possibile modificare il file di configurazione web senza ricompilare l'applicazione. Se è necessario disabilitare la memorizzazione nella cache per un'applicazione che è già stata distribuita nell'ambiente di produzione, è possibile modificare semplicemente i profili della cache definiti nel file di configurazione web. Verranno rilevate automaticamente e applicare eventuali modifiche al file di configurazione web.

Ad esempio, il &lt;memorizzazione nella cache&gt; sezione di configurazione web nel listato 6 definisce un profilo della cache denominato Cache1Hour. Il &lt;memorizzazione nella cache&gt; sezione deve essere racchiuso tra i &lt;System. Web&gt; sezione di un file di configurazione web.

**Listato 6 – sezione memorizzazione nella cache per Web. config**

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

Il controller nel listato 7 viene illustrato come è possibile applicare il profilo Cache1Hour a un'azione del controller con l'attributo [OutputCache].

**Listato 7 – Controllers\ProfileController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

Se si richiama l'azione Index () esposto dal controller nel listato 7, verrà restituito contemporaneamente per 1 ora.

## <a name="summary"></a>Riepilogo

La memorizzazione nella cache di output fornisce un metodo molto semplice di migliorare notevolmente le prestazioni delle applicazioni ASP.NET MVC. In questa esercitazione è stato descritto come usare l'attributo [OutputCache] per memorizzare nella cache l'output di azioni del controller. Inoltre appreso come modificare le proprietà dell'attributo [OutputCache] ad esempio le proprietà di durata e VaryByParam per modificare la modalità con cui viene memorizzato nella cache il contenuto. Infine, si è appreso come definire profili per la cache nel file di configurazione web.

> [!div class="step-by-step"]
> [Precedente](understanding-action-filters-cs.md)
> [Successivo](adding-dynamic-content-to-a-cached-page-cs.md)
