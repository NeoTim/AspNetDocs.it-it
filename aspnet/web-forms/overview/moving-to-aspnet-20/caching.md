---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Memorizzazione nella cache . Documenti Microsoft
author: rick-anderson
description: La conoscenza della memorizzazione nella cache è importante per un'applicazione ASP.NET le prestazioni. ASP.NET 1.x offrivatre diverse opzioni per la memorizzazione nella cache; memorizzazione nella cache di output,...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: a199a9c0352dfb054e8d4e5e67652db9bd38851c
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542937"
---
# <a name="caching"></a>Memorizzazione nella cache

da parte [di Microsoft](https://github.com/microsoft)

> La conoscenza della memorizzazione nella cache è importante per un'applicazione ASP.NET le prestazioni. ASP.NET 1.x offrivatre diverse opzioni per la memorizzazione nella cache; memorizzazione nella cache di output, memorizzazione nella cache dei frammenti e l'API della cache.

La conoscenza della memorizzazione nella cache è importante per un'applicazione ASP.NET le prestazioni. ASP.NET 1.x offrivatre diverse opzioni per la memorizzazione nella cache; memorizzazione nella cache di output, memorizzazione nella cache dei frammenti e l'API della cache. ASP.NET 2.0 offre tutti e tre questi metodi, ma aggiunge alcune funzionalità aggiuntive significative. Esistono diverse nuove dipendenze della cache e gli sviluppatori ora hanno la possibilità di creare anche dipendenze della cache personalizzate. Anche la configurazione della memorizzazione nella cache è stata migliorata in modo significativo in ASP.NET 2.0.

## <a name="new-features"></a>Nuove funzionalità

## <a name="cache-profiles"></a>Profili cache

I profili di cache consentono agli sviluppatori di definire impostazioni della cache specifiche che possono quindi essere applicate a singole pagine. Ad esempio, se si dispone di alcune pagine che devono essere scadute dalla cache dopo 12 ore, è possibile creare facilmente un profilo di cache che può essere applicato a tali pagine. Per aggiungere un nuovo profilo &lt;di&gt; cache, utilizzare la sezione outputCacheSettings nel file di configurazione. Ad esempio, di seguito è riportata la configurazione di un profilo di cache denominato *due giorni* che configura una durata della cache di 12 ore.

[!code-xml[Main](caching/samples/sample1.xml)]

Per applicare questo profilo di cache a una pagina specifica, utilizzare l'attributo CacheProfile della direttiva OutputCache, come illustrato di seguito:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Dipendenze cache personalizzate

ASP.NET 1.x sviluppatori hanno soddisfatto le dipendenze personalizzate della cache. In ASP.NET 1.x, la classe CacheDependency è stata sealed che ha impedito agli sviluppatori di derivare le proprie classi da esso. In ASP.NET 2.0, tale limitazione viene rimossa e gli sviluppatori sono liberi di sviluppare le proprie dipendenze di cache personalizzate. La classe CacheDependency consente la creazione di una dipendenza della cache personalizzata basata su file, directory o chiavi della cache.

Ad esempio, il codice seguente crea una nuova dipendenza della cache personalizzata basata su un file denominato stuff.xml situato nella radice dell'applicazione Web:

[!code-csharp[Main](caching/samples/sample3.cs)]

In questo scenario, quando il file stuff.xml viene modificato, l'elemento memorizzato nella cache viene invalidato.

È anche possibile creare una dipendenza della cache personalizzata usando le chiavi della cache. Utilizzando questo metodo, la rimozione della chiave di cache invaliderà i dati memorizzati nella cache. L'esempio seguente illustra questi concetti.

[!code-csharp[Main](caching/samples/sample4.cs)]

Per invalidare l'elemento inserito in precedenza, è sufficiente rimuovere l'elemento inserito nella cache in modo che funga da chiave di cache.

[!code-csharp[Main](caching/samples/sample5.cs)]

Si noti che la chiave dell'elemento che funge da chiave di cache deve essere uguale al valore aggiunto alla matrice di chiavi della cache.

## <a name="polling-based-sql-cache-dependenciesalso-called-table-based-dependencies"></a>Dipendenze della cache SQL basata sul polling (denominate anche dipendenze basate su tabelle)Polling-Based SQL Cache Dependencies (Also called Table-Based Dependencies)

SQL Server 7 e 2000 utilizzano il modello basato sul polling per le dipendenze della cache SQL. Il modello basato sul polling usa un trigger in una tabella di database che viene attivato quando i dati nella tabella vengono modificati. Tale trigger aggiorna un campo **changeId** nella tabella di notifica che ASP.NET controlla periodicamente. Se il campo **changeId** è stato aggiornato, ASP.NET sa che i dati sono stati modificati e invalida i dati memorizzati nella cache.

> [!NOTE]
> SQL Server 2005 può anche utilizzare il modello basato sul polling, ma poiché il modello basato sul polling non è il modello più efficiente, è consigliabile utilizzare un modello basato su query (illustrato più avanti) con SQL Server 2005.

Affinché una dipendenza della cache SQL che utilizza il modello basato sul polling funzioni correttamente, le tabelle devono avere le notifiche abilitate. Questa operazione può essere eseguita a livello di codice utilizzando\_la classe SqlCacheDependencyAdmin o l'utilità aspnet regsql.exe.

La riga di comando seguente registra la tabella Products del database Northwind che si trova in un'istanza di SQL Server denominata *dbase* per la dipendenza della cache SQL.

[!code-console[Main](caching/samples/sample6.cmd)]

Di seguito è riportata una spiegazione delle opzioni della riga di comando utilizzate nel comando precedente:

| **Opzione della riga di comando** | **Scopo** |
| --- | --- |
| -S *server* | Specifica il nome del server. |
| -ed (ed) | Specifica che il database deve essere abilitato per la dipendenza della cache SQL. |
| -d *\_nome database* | Specifica il nome del database che deve essere abilitato per la dipendenza della cache SQL. |
| -E | Specifica che aspnet\_regsql deve utilizzare l'autenticazione di Windows per la connessione al database. |
| -et | Specifica che si sta abilitando una tabella di database per la dipendenza della cache SQL. |
| -t *\_nome tabella* | Specifica il nome della tabella di database da abilitare per la dipendenza della cache SQL. |

> [!NOTE]
> Sono disponibili altre opzioni\_per aspnet regsql.exe. Per un elenco completo,\_eseguire aspnet regsql.exe -? da una riga di comando.

Quando questo comando esegue le seguenti modifiche vengono apportate al database di SQL Server:

- Viene aggiunta una tabella **AspNet\_SqlCacheTablesForChangeNotification.** Questa tabella contiene una riga per ogni tabella del database per cui è stata abilitata una dipendenza della cache SQL.
- All'interno del database vengono create le seguenti stored procedure:

| AspNet\_SqlCachePollingStoredProcedure | Esegue una\_query nella tabella AspNet SqlCacheTablesForChangeNotification e restituisce tutte le tabelle abilitate per la dipendenza della cache SQL e il valore di changeId per ogni tabella. Questa stored procedure viene utilizzata per il polling per determinare se i dati sono stati modificati. |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | Restituisce tutte le tabelle abilitate per la dipendenza\_dalla cache SQL eseguendo una query nella tabella AspNet SqlCacheTablesForChangeNotification e vengono restituite tutte le tabelle abilitate per la dipendenza della cache SQL. |
| AspNet\_SqlCacheRegisterTableStoredProcedure | Registra una tabella per la dipendenza della cache SQL aggiungendo la voce necessaria nella tabella di notifica e aggiunge il trigger. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | Annulla la registrazione di una tabella per la dipendenza della cache SQL rimuovendo la voce nella tabella di notifica e rimuove il trigger. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Aggiorna la tabella di notifica incrementando changeId per la tabella modificata. ASP.NET utilizza questo valore per determinare se i dati sono stati modificati. Come indicato di seguito, questo proc archiviato viene eseguito dal trigger creato quando la tabella è abilitata. |

- Un trigger di SQL Server denominato ** _nome\_di_\_tabella AspNet\_SqlCacheNotification\_Trigger** viene creato per la tabella. Questo trigger esegue aspNet\_SqlCacheUpdateChangeIdStoredProcedure quando viene eseguita un'istruzione INSERT, UPDATE o DELETE sulla tabella.
- Un ruolo di SQL Server denominato **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** viene aggiunto al database.

Il ruolo di SQL Server **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** dispone delle autorizzazioni EXEC per AspNet\_SqlCachePollingStoredProcedure. Affinché il modello di polling funzioni correttamente, è necessario\_aggiungere\_l'account del processo al ruolo aspnet ChangeNotification ReceiveNotificationsOnlyAccess. Lo strumento\_aspnet regsql.exe non consente di eseguire questa operazione.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Configurazione delle dipendenze della cache SQL basata sul pollingConfiguring Polling-Based SQL Cache Dependencies

Esistono diversi passaggi necessari per la configurazione delle dipendenze della cache SQL basata sul polling. Il primo passaggio consiste nell'abilitare il database e la tabella come descritto in precedenza. Una volta completato questo passaggio, il resto della configurazione è il seguente:

- Configurazione del file di configurazione ASP.NET.
- Configurazione di SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>Configurazione del file di configurazione ASP.NET

Oltre ad aggiungere una stringa di connessione come illustrato in &lt;&gt; un modulo &lt;precedente,&gt; è necessario configurare anche un elemento della cache con un elemento sqlCacheDependency, come illustrato di seguito:In addition to adding a connection string as discussed in a previous module, you must also configure a cache element with a sqlCacheDependency element as shown below:

[!code-xml[Main](caching/samples/sample7.xml)]

Questa configurazione consente una dipendenza della cache SQL nel database *pubs.* Si noti che l'attributo pollTime nell'elemento &lt;sqlCacheDependency&gt; per impostazione predefinita è 60000 millisecondi o 1 minuto. Questo valore non può essere inferiore a 500 millisecondi. In questo esempio, l'elemento &lt;add&gt; aggiunge un nuovo database ed esegue l'override di pollTime, impostandolo su 9000000 millisecondi.

#### <a name="configuring-the-sqlcachedependency"></a>Configurazione di SqlCacheDependency

The next step is to configure the SqlCacheDependency. Il modo più semplice per eseguire questa operazione consiste nello specificare il valore per l'attributo SqlDependency nella direttiva Outcache, come indicato di seguito:

[!code-aspx[Main](caching/samples/sample8.aspx)]

Nella precedente direttiva OutputCache, una dipendenza della cache SQL è configurata per la tabella *authors* nel database *pubs.* È possibile configurare più dipendenze separandole con un punto e virgola in questo modo:Multiple dependencies can be configured by separating them with a e-colon like so:

[!code-aspx[Main](caching/samples/sample9.aspx)]

Un altro metodo per configurare il SqlCacheDependency consiste nell'eseguire questa operazione a livello di codice. Il codice seguente crea una nuova dipendenza della cache SQL nella tabella *authors* del database *pubs.*

[!code-csharp[Main](caching/samples/sample10.cs)]

Uno dei vantaggi della definizione a livello di codice della dipendenza della cache SQL è che è possibile gestire tutte le eccezioni che potrebbero verificarsi. Ad esempio, se si tenta di definire una dipendenza della cache SQL per un database che non è stato abilitato per la notifica, un **DatabaseNotEnabledForNotificationException** verrà generata un'eccezione. In tal caso, è possibile tentare di abilitare il database per le notifiche chiamando il **SqlCacheDependencyAdmin.EnableNotifications** metodo e passando il nome del database.

Analogamente, se si tenta di definire una dipendenza della cache SQL per una tabella che non è stata abilitata per la notifica, verrà generata **un'eccezione TableNotEnabledForNotificationException.** È quindi possibile chiamare il metodo **SqlCacheDependencyAdmin.EnableTableForNotifications** passandogli il nome del database e il nome della tabella.

Nell'esempio di codice seguente viene illustrato come configurare correttamente la gestione delle eccezioni durante la configurazione di una dipendenza della cache SQL.

[!code-csharp[Main](caching/samples/sample11.cs)]

Ulteriori informazioni:[https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Dipendenze cache SQL basata su query (solo SQL Server 2005)

Quando si utilizza SQL Server 2005 per la dipendenza della cache SQL, il modello basato sul polling non è necessario. Se utilizzate con SQL Server 2005, le dipendenze della cache SQL comunicano direttamente tramite connessioni SQL all'istanza di SQL Server (non sono necessarie ulteriori configurazioni) utilizzando le notifiche di query di SQL Server 2005.

Il modo più semplice per abilitare la notifica basata su query consiste nell'eseguire questa operazione in modo dichiarativo impostando l'attributo **SqlCacheDependency** dell'oggetto origine dati **su CommandNotification** e impostando l'attributo **EnableCaching** su **true**. Utilizzando questo metodo, non è necessario alcun codice. Se il risultato di un comando eseguito sull'origine dati viene modificato, verrà invalidato i dati della cache.

Nell'esempio seguente viene configurato un controllo origine dati per la dipendenza della cache SQL:The following example configures a data source control for SQL cache dependency:

[!code-aspx[Main](caching/samples/sample12.aspx)]

In questo caso, se la query specificata in **SelectCommand** restituisce un risultato diverso da quello originale, i risultati memorizzati nella cache vengono invalidati.

È inoltre possibile specificare che tutte le origini dati devono essere abilitate per le dipendenze della cache SQL impostando l'attributo **SqlDependency** della direttiva **OutputCache** su **CommandNotification**. L'esempio seguente illustra questo.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Per ulteriori informazioni sulle notifiche di query in SQL Server 2005, vedere la documentazione in linea di SQL Server.

Un altro metodo per configurare una dipendenza della cache SQL basata su query consiste nell'eseguire questa operazione a livello di codice utilizzando la classe SqlCacheDependency.Another method of configuring a query-based SQL cache dependency is to do so programmatically using the SqlCacheDependency class. Nell'esempio di codice seguente viene illustrato come eseguire questa operazione.

[!code-csharp[Main](caching/samples/sample14.cs)]

Ulteriori informazioni:[https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Sostituzione post-Cache

La memorizzazione nella cache di una pagina può aumentare notevolmente le prestazioni di un'applicazione Web. Tuttavia, in alcuni casi è necessario che la maggior parte della pagina venga memorizzata nella cache e che alcuni frammenti all'interno della pagina siano dinamici. Ad esempio, se si crea una pagina di notizie completamente statica per determinati periodi di tempo, è possibile impostare la cache dell'intera pagina. Se desideri includere un banner pubblicitario rotante modificato in ogni richiesta di pagina, la parte della pagina contenente l'annuncio deve essere dinamica. Per consentire di memorizzare nella cache una pagina ma sostituire alcuni contenuti in modo dinamico, è possibile utilizzare ASP.NET sostituzione post-cache. Con la sostituzione post-cache, l'intera pagina viene memorizzata nella cache di output con parti specifiche contrassegnate come esenti dalla memorizzazione nella cache. Nell'esempio dei banner pubblicitari, il controllo AdRotator consente di sfruttare la sostituzione post-cache in modo che gli annunci creati dinamicamente per ogni utente e per ogni aggiornamento della pagina.

Esistono tre modi per implementare la sostituzione post-cache:There are three ways to implement post-cache substitution:

- In modo dichiarativo, utilizzando il Substitution controllo.
- A livello di codice, usando l'API di controllo di sostituzione.
- In modo implicito, utilizzando il AdRotator controllo.

### <a name="substitution-control"></a>Controllo della sostituzione

Il controllo ASP.NET sostituzione specifica una sezione di una pagina memorizzata nella cache che viene creata dinamicamente anziché memorizzata nella cache. Si inserisce un controllo Sostituzione nella posizione della pagina in cui si desidera visualizzare il contenuto dinamico. In fase di esecuzione, il Substitution controllo chiama un metodo specificato con il MethodName proprietà. Il metodo deve restituire una stringa, che quindi sostituisce il contenuto del Substitution controllo. Il metodo deve essere un metodo statico nel controllo Page o UserControl che lo contiene. L'utilizzo del controllo di sostituzione determina la modifica della memorizzazione nella cache sul lato client in cache, in modo che la pagina non venga memorizzata nella cache del client. In questo modo si garantisce che le richieste future alla pagina chiamino nuovamente il metodo per generare contenuto dinamico.

### <a name="substitution-api"></a>API di sostituzione

Per creare contenuto dinamico per una pagina memorizzata nella cache a livello di codice, è possibile chiamare il [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) metodo nel codice della pagina, passando il nome di un metodo come parametro. Il metodo che gestisce la creazione del contenuto dinamico accetta un singolo parametro [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) e restituisce una stringa. La stringa restituita è il contenuto che verrà sostituito nella posizione specificata. Un vantaggio della chiamata di WriteSubstitution metodo anziché utilizzare il Substitution controllo in modo dichiarativo è che è possibile chiamare un metodo di qualsiasi oggetto arbitrario anziché chiamare un metodo statico del Page o Il UserControl oggetto.

La chiamata di WriteSubstitution metodo causa la memorizzazione nella cache sul lato client per la memorizzazione nella cache del server, in modo che la pagina non verrà memorizzata nella cache del client. In questo modo si garantisce che le richieste future alla pagina chiamino nuovamente il metodo per generare contenuto dinamico.

### <a name="adrotator-control"></a>Controllo AdRotator

Il controllo server AdRotator implementa il supporto per la sostituzione interna della post-cache. Se si inserisce un controllo AdRotator nella pagina, verrà eseguito il rendering di annunci univoci in ogni richiesta, indipendentemente dal fatto che la pagina padre sia memorizzata nella cache. Di conseguenza, una pagina che include un AdRotator controllo viene memorizzato nella cache solo lato server.

## <a name="controlcachepolicy-class"></a>ControlCachePolicy (classe)

La classe ControlCachePolicy consente il controllo a livello di codice della memorizzazione nella cache dei frammenti utilizzando i controlli utente. ASP.NET incorpora i controlli utente all'interno di un [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) istanza. La classe BasePartialCachingControl rappresenta un controllo utente con la memorizzazione nella cache di output abilitata.

Quando si accede alla proprietà [BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) di un controllo [PartialCachingControl,](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) si riceverà sempre un oggetto ControlCachePolicy valido. Tuttavia, se si accede alla proprietà [UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) di un controllo [UserControl,](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) si riceve un oggetto ControlCachePolicy valido solo se il controllo utente è già incapsulato da un controllo BasePartialCachingControl. Se non ne viene eseguito il wrapping, l'oggetto ControlCachePolicy restituito dalla proprietà genererà eccezioni quando si tenta di modificarlo perché non dispone di un oggetto BasePartialCachingControl associato. Per determinare se un'istanza di UserControl supporta la memorizzazione nella cache senza generare eccezioni, esaminare la proprietà [SupportsCaching.To](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) determine whether a UserControl instance supports caching without generating exceptions, inspect the SupportsCaching property.

L'utilizzo della classe ControlCachePolicy è uno dei diversi modi per abilitare la memorizzazione nella cache di output. Nell'elenco seguente vengono descritti i metodi che è possibile utilizzare per abilitare la memorizzazione nella cache di output:The following list describes methods you can enable output caching:

- Utilizzare il [OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) direttiva per abilitare la memorizzazione nella cache di output in scenari dichiarativi.
- Utilizzare il [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) attributo per abilitare la memorizzazione nella cache per un controllo utente in un file code-behind.
- Utilizzare la ControlCachePolicy classe per specificare le impostazioni della cache in scenari a livello di codice in cui si lavora con BasePartialCachingControl istanze che sono state abilitate per la cache utilizzando uno dei metodi precedenti e caricate in modo dinamico utilizzando il [System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) metodo.

Un'istanza ControlCachePolicy può essere modificata correttamente solo tra le fasi Init e PreRender del ciclo di vita del controllo. Se si modifica un Oggetto ControlCachePolicy dopo la fase PreRender, ASP.NET genera un'eccezione perché le modifiche apportate dopo il rendering del controllo non possono influire effettivamente sulle impostazioni della cache (un controllo viene memorizzato nella cache durante la fase di rendering). Infine, un'istanza del controllo utente (e pertanto il relativo ControlCachePolicy oggetto) è disponibile solo per la modifica a livello di codice quando viene effettivamente eseguito il rendering.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Modifiche alla configurazione della &lt;memorizzazione nella cache - L'elemento di memorizzazione nella cacheChanges to Caching Configuration - The caching&gt; Element

Esistono diverse modifiche alla configurazione della memorizzazione nella cache in ASP.NET 2.0.There are several changes to caching configuration in ASP.NET 2.0. &lt;L'elemento&gt; di memorizzazione nella cache è nuovo in ASP.NET 2.0 e consente di apportare modifiche alla configurazione della memorizzazione nella cache nel file di configurazione. Sono disponibili i seguenti attributi.

| **Elemento** | **Descrizione** |
| --- | --- |
| **Cache** | Elemento facoltativo. Definisce le impostazioni globali della cache dell'applicazione. |
| **Outputcache** | Elemento facoltativo. Specifica le impostazioni della cache di output a livello di applicazione. |
| **outputCacheSettings** | Elemento facoltativo. Specifica le impostazioni della cache di output che possono essere applicate alle pagine dell'applicazione. |
| **sqlCacheDependency** | Elemento facoltativo. Configura le dipendenze della cache di SQL per un'applicazione ASP.NET. |

### <a name="the-ltcachegt-element"></a>L'elemento &lt;cache&gt;

Nell'elemento cache&gt; sono &lt;disponibili i seguenti attributi:

| **Attributo** | **Descrizione** |
| --- | --- |
| **disableMemoryCollection (informazioni in base alla memoria disableMemory** | Attributo **Boolean** facoltativo. Ottiene o imposta un valore che indica se la raccolta di memoria cache che si verifica quando il computer è sotto pressione della memoria è disabilitata. |
| **disableExpiration (disabilitaExpiration)** | Attributo **Boolean** facoltativo. Ottiene o imposta un valore che indica se la scadenza della cache è disabilitata. Quando è disabilitato, gli elementi memorizzati nella cache non scadono e lo scavenging in background degli elementi scaduti della cache non si verifica. |
| **privateBytesLimite** | Attributo **Int64** facoltativo. Ottiene o imposta un valore che indica la dimensione massima dei byte privati di un'applicazione prima che la cache inizi a scaricare gli elementi scaduti e a tentare di recuperare memoria. Questo limite include sia la memoria utilizzata dalla cache che il normale sovraccarico di memoria dell'applicazione in esecuzione. L'impostazione zero indica che ASP.NET utilizzerà la propria euristica per determinare quando avviare il recupero della memoria. |
| **percentagePhysicalMemoryUsedLimit** | Attributo **Int32** facoltativo. Ottiene o imposta un valore che indica la percentuale massima di memoria fisica di un computer che può essere utilizzata da un'applicazione prima che la cache inizi a scaricare gli elementi scaduti e a tentare di recuperare la memoria Questo utilizzo della memoria include sia la memoria utilizzata dalla cache che il normale utilizzo della memoria dell'applicazione in esecuzione. L'impostazione zero indica che ASP.NET utilizzerà la propria euristica per determinare quando avviare il recupero della memoria. |
| **privateBytesPollTime (informazioni in privato)** | Attributo **TimeSpan** facoltativo. Ottiene o imposta un valore che indica l'intervallo di tempo tra il polling per l'utilizzo della memoria di byte privati dell'applicazione. |

### <a name="the-ltoutputcachegt-element"></a>Elemento &lt;outputCache&gt;

Gli attributi seguenti sono &lt;disponibili&gt; per l'elemento outputCache.

|       <strong>Attributo</strong>        |                                                                                                                                                                                                                                                       <strong>Descrizione</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache (EnableOutputCache)</strong>    |                                                                                                                                                          Attributo <strong>Boolean</strong> facoltativo. Abilita/disabilita la cache di output delle pagine. Se disabilitata, nessuna pagina viene memorizzata nella cache indipendentemente dalle impostazioni a livello di codice o dichiarative. Il valore predefinito è <strong>true</strong>.                                                                                                                                                           |
|  <strong>enableFragmentCache (EnableFragmentCache)</strong>   |                                                Attributo <strong>Boolean</strong> facoltativo. Abilita/disabilita la cache dei frammenti dell'applicazione. Se disabilitata, nessuna pagina viene memorizzata nella cache, indipendentemente dalla direttiva [OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) o dal profilo di memorizzazione nella cache utilizzato. Include un'intestazione di controllo della cache che indica che i server proxy upstream e i client browser non devono tentare di memorizzare nella cache l'output della pagina. Il valore predefinito è <strong>false</strong>.                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      Attributo <strong>Boolean</strong> facoltativo. Ottiene o imposta un valore che indica se l'intestazione <strong>cache-control:private</strong> viene inviata dal modulo della cache di output per impostazione predefinita. Il valore predefinito è <strong>false</strong>.                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | Attributo <strong>Boolean</strong> facoltativo. Abilita/disabilita l'invio<strong>di \<un'intestazione Http " Vary: /strong><em>" nella risposta. Con l'impostazione predefinita di false, viene</em>inviata un'intestazione " - Vary: \*" per le pagine memorizzate nella cache di <strong>output. Quando viene inviata l'intestazione Vary, consente di utilizzare versioni diverse nella cache in base a quanto specificato nell'intestazione Vary. Ad esempio, <em>Vary:User-Agents</em> memorizzerà versioni diverse di una pagina in base all'agente utente che invia la richiesta. Il valore predefinito è "false"</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>Elemento &lt;outputCacheSettings&gt;

L'elemento &lt;&gt; outputCacheSettings consente la creazione di profili di cache come descritto in precedenza. L'unico elemento &lt;figlio per&gt; l'elemento outputCacheSettings è l'elemento &lt;outputCacheProfiles&gt; per la configurazione dei profili di cache.

### <a name="the-ltsqlcachedependencygt-element"></a>Elemento &lt;sqlCacheDependency&gt;

Gli attributi seguenti sono &lt;disponibili per&gt; l'elemento sqlCacheDependency.

| **Attributo** | **Descrizione** |
| --- | --- |
| **Abilitato** | Attributo **booleano** obbligatorio. Indica se è in corso o meno il polling delle modifiche. |
| **pollTime (in stato di polling)** | Attributo **Int32** facoltativo. Imposta la frequenza con cui SqlCacheDependency esegue il polling delle modifiche nella tabella di database. Questo valore corrisponde al numero di millisecondi tra polling successivi. Non può essere impostato su meno di 500 millisecondi. Il valore predefinito è 1 minuto. |

### <a name="more-information"></a>Altre informazioni

Sono disponibili alcune informazioni aggiuntive di cui è necessario tenere conto della configurazione della cache.

- Se il limite di byte privati del processo di lavoro non è impostato, la cache utilizzerà uno dei seguenti limiti: 

    - x86 2GB: 800 MB o il 60% della RAM fisica, a seconda di quale sia minore
    - x86 3GB: 1800MB o il 60% della RAM fisica, a seconda del fatto che sia minore
    - x64: 1 terabyte o il 60% della RAM fisica, a seconda di quale sia minore
- Se sono impostati sia &lt;il limite dei&gt; byte privati del processo di lavoro che la cache privateBytesLimit/, la cache utilizzerà il minimo dei due.
- Proprio come in 1.x, rilasciamo le voci della cache e chiamiamo GC. Raccogli per due motivi: 

    - Siamo molto vicini al limite di byte privati
    - La memoria disponibile è vicina o inferiore al 10%
- È possibile disabilitare in modo efficace il &lt;taglio e la&gt; cache per le condizioni di memoria disponibile insufficiente impostando la percentuale di cachePhysicalMemoryUseLimit/ su 100.
- A differenza di 1.x, 2.0 sospenderà l'assetto e raccoglierà le chiamate se l'ultimo GC. Collect non ha ridotto i byte privati o le dimensioni degli heap gestiti di oltre l'1% del limite di memoria (cache).

## <a name="lab1-custom-cache-dependencies"></a>Lab1: Dipendenze cache personalizzate

1. Creare un nuovo sito Web.
2. Aggiungere un nuovo file XML denominato cache.xml e salvarlo nella radice dell'applicazione Web.
3. Aggiungere il codice seguente\_al metodo Page Load nel code-behind di default.aspx: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Aggiungere quanto segue all'inizio di default.aspx nella visualizzazione origine: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Esplorare Default.aspx. Cosa dice il tempo?
6. Aggiornare il browser. Cosa dice il tempo?
7. Aprire cache.xml e aggiungere il codice seguente: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Salvare cache.xml.
9. Aggiornare il browser. Cosa dice il tempo?
10. Spiegare perché l'ora aggiornata invece di visualizzare i valori precedentemente memorizzati nella cache:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Laboratorio 2: Utilizzo delle dipendenze della cache basata sul polling

In questo lab viene utilizzato il progetto creato nel modulo precedente che consente la modifica dei dati nel database Northwind tramite un controllo GridView e DetailsView.

1. Aprire il progetto in Visual Studio 2005.
2. Eseguire l'utilità aspnet\_regsql sul database Northwind per abilitare il database e la tabella Products. Utilizzare il comando seguente da un prompt dei comandi di Visual Studio:Use the following command from a Visual Studio Command Prompt: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Aggiungere quanto segue al file web.config: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Aggiungere un nuovo webform denominato showdata.aspx.
5. Aggiungere la seguente direttiva outputcache alla pagina showdata.aspx: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Aggiungere il codice seguente\_al caricamento della pagina di showdata.aspx: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Aggiungere un nuovo controllo SqlDataSource a showdata.aspx e configurarlo per l'utilizzo della connessione al database Northwind. Scegliere Avanti.
8. Selezionare le caselle di controllo ProductName e ProductID e fare clic su Avanti.
9. Fare clic su Fine.
10. Aggiungere un nuovo controllo GridView alla pagina showdata.aspx.
11. Scegliere SqlDataSource1 dall'elenco a discesa.
12. Salvare e sfogliare showdata.aspx. Prendere nota dell'ora visualizzata.
