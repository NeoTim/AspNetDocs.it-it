---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
title: Uso delle dipendenze della cache SQL (VB) | Microsoft Docs
author: rick-anderson
description: La strategia di memorizzazione nella cache più semplice consiste nel consentire la scadenza dei dati memorizzati nella cache dopo un periodo di tempo specificato. Ma questo semplice approccio significa che i dati memorizzati nella cache Maintai...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: bd347d93-4251-4532-801c-a36f2dfa7f96
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
msc.type: authoredcontent
ms.openlocfilehash: 175169772ba04376e1bd1cb143b13a200652a9bf
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2020
ms.locfileid: "89044792"
---
# <a name="using-sql-cache-dependencies-vb"></a>Uso delle dipendenze della cache SQL (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_VB.zip) o [Scarica PDF](using-sql-cache-dependencies-vb/_static/datatutorial61vb1.pdf)

> La strategia di memorizzazione nella cache più semplice consiste nel consentire la scadenza dei dati memorizzati nella cache dopo un periodo di tempo specificato. Tuttavia, questo semplice approccio significa che i dati memorizzati nella cache non mantengono alcuna associazione con l'origine dati sottostante, ottenendo dati non aggiornati mantenuti troppo lunghi o dati correnti scaduti troppo a breve. Un approccio migliore consiste nell'usare la classe SqlCacheDependency in modo che i dati rimangano memorizzati nella cache fino a quando i dati sottostanti non vengono modificati nel database SQL. Questa esercitazione illustra come.

## <a name="introduction"></a>Introduzione

Le tecniche di memorizzazione nella cache esaminate nei [dati di memorizzazione nella cache con i](caching-data-with-the-objectdatasource-vb.md) dati di ObjectDataSource e [di memorizzazione](caching-data-in-the-architecture-vb.md) nella cache nelle esercitazioni sull'architettura hanno usato una scadenza basata sul tempo per rimuovere i dati dalla cache dopo un periodo specificato. Questo approccio rappresenta il modo più semplice per bilanciare il miglioramento delle prestazioni di memorizzazione nella cache rispetto all'obsolescenza dei dati. Selezionando una scadenza temporale di *x* secondi, uno sviluppatore di pagine cede per sfruttare i vantaggi in termini di prestazioni derivanti dalla memorizzazione nella cache solo per *x* secondi, ma può essere facile che i dati non saranno mai obsoleti più di un massimo di *x* secondi. Naturalmente, per i dati statici, *x* può essere esteso alla durata dell'applicazione Web, come è stato esaminato nell'esercitazione sull' [avvio dell'applicazione Caching Data](caching-data-at-application-startup-vb.md) .

Quando si memorizzano nella cache i dati del database, una scadenza basata sul tempo viene spesso scelta per la facilità d'uso, ma è spesso una soluzione inadeguata. Idealmente, i dati del database rimangono memorizzati nella cache fino a quando i dati sottostanti non vengono modificati nel database. solo a questo punto la cache verrà rimossa. Questo approccio massimizza i vantaggi in merito alle prestazioni della memorizzazione nella cache e riduce al minimo la durata dei dati non aggiornati. Tuttavia, per sfruttare questi vantaggi è necessario che sia presente un sistema che sappia quando i dati del database sottostanti sono stati modificati e rimuove gli elementi corrispondenti dalla cache. Prima di ASP.NET 2,0, gli sviluppatori di pagine erano responsabili dell'implementazione di questo sistema.

ASP.NET 2,0 fornisce una [ `SqlCacheDependency` classe](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx) e l'infrastruttura necessaria per determinare quando si è verificata una modifica nel database, in modo che sia possibile rimuovere gli elementi memorizzati nella cache corrispondenti. Esistono due tecniche per determinare quando i dati sottostanti sono stati modificati: notifica e polling. Dopo aver illustrato le differenze tra le notifiche e il polling, verrà creata l'infrastruttura necessaria per supportare il polling, quindi verrà illustrato come utilizzare la `SqlCacheDependency` classe in scenari dichiarativi e a livello di codice.

## <a name="understanding-notification-and-polling"></a>Informazioni sulle notifiche e sul polling

Esistono due tecniche che è possibile utilizzare per determinare quando i dati di un database sono stati modificati: notifiche e polling. Con la notifica, il database avvisa automaticamente il runtime ASP.NET quando i risultati di una determinata query sono stati modificati dopo l'ultima esecuzione della query. a quel punto gli elementi memorizzati nella cache associati alla query vengono rimossi. Con il polling, il server di database gestisce le informazioni relative all'ultimo aggiornamento di determinate tabelle. Il runtime di ASP.NET esegue periodicamente il polling del database per verificare quali tabelle sono state modificate dopo l'immissione nella cache. Alle tabelle i cui dati sono stati modificati sono stati rimossi gli elementi della cache associati.

L'opzione di notifica richiede un minor numero di configurazioni rispetto al polling ed è più granulare perché tiene traccia delle modifiche a livello di query anziché a livello di tabella. Sfortunatamente, le notifiche sono disponibili solo nelle edizioni complete di Microsoft SQL Server 2005 (ad esempio, le edizioni non Express). Tuttavia, l'opzione di polling può essere usata per tutte le versioni di Microsoft SQL Server da 7,0 a 2005. Poiché queste esercitazioni usano l'edizione Express di SQL Server 2005, si concentrerà sulla configurazione e sull'uso dell'opzione di polling. Consultare la sezione ulteriori informazioni alla fine di questa esercitazione per ulteriori risorse sulle funzionalità di notifica di SQL Server 2005.

Con il polling, il database deve essere configurato in modo da includere una tabella denominata con `AspNet_SqlCacheTablesForChangeNotification` tre colonne `tableName` , `notificationCreated` , `changeId` e. Questa tabella contiene una riga per ogni tabella che contiene dati che potrebbero dover essere utilizzati in una dipendenza della cache SQL nell'applicazione Web. La `tableName` colonna specifica il nome della tabella, mentre `notificationCreated` indica la data e l'ora in cui la riga è stata aggiunta alla tabella. La `changeId` colonna è di tipo `int` e ha un valore iniziale pari a 0. Il valore viene incrementato a ogni modifica apportata alla tabella.

Oltre alla `AspNet_SqlCacheTablesForChangeNotification` tabella, è inoltre necessario che il database includa trigger in ognuna delle tabelle che possono essere visualizzate in una dipendenza della cache SQL. Questi trigger vengono eseguiti ogni volta che viene inserita, aggiornata o eliminata una riga e viene incrementato il valore della tabella `changeId` in `AspNet_SqlCacheTablesForChangeNotification` .

Il runtime di ASP.NET tiene traccia dell' `changeId` oggetto corrente per una tabella durante la memorizzazione nella cache dei dati usando un `SqlCacheDependency` oggetto. Il database viene controllato periodicamente e tutti `SqlCacheDependency` gli oggetti il cui `changeId` valore differisce dal valore nel database vengono rimossi poiché un valore diverso `changeId` indica che è stata apportata una modifica alla tabella perché i dati sono stati memorizzati nella cache.

## <a name="step-1-exploring-theaspnet_regsqlexecommand-line-program"></a>Passaggio 1: esplorazione del `aspnet_regsql.exe` programma da riga di comando

Con l'approccio di polling è necessario configurare il database in modo che contenga l'infrastruttura descritta sopra: una tabella predefinita ( `AspNet_SqlCacheTablesForChangeNotification` ), un numero limitato di stored procedure e trigger in ognuna delle tabelle che possono essere utilizzate nelle dipendenze della cache SQL nell'applicazione Web. Le tabelle, le stored procedure e i trigger possono essere creati tramite il programma da riga `aspnet_regsql.exe` di comando, disponibile nella `$WINDOWS$\Microsoft.NET\Framework\version` cartella. Per creare la `AspNet_SqlCacheTablesForChangeNotification` tabella e le stored procedure associate, eseguire il comando seguente dalla riga di comando:

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample1.cmd)]

> [!NOTE]
> Per eseguire questi comandi, è necessario che l'account di accesso al database specificato sia nei [`db_securityadmin`](https://msdn.microsoft.com/library/ms188685.aspx) [`db_ddladmin`](https://msdn.microsoft.com/library/ms190667.aspx) ruoli e. Per esaminare il T-SQL inviato al database dal programma da `aspnet_regsql.exe` riga di comando, fare riferimento a [questo intervento di Blog](http://scottonwriting.net/sowblog/posts/10709.aspx).

Ad esempio, per aggiungere l'infrastruttura per il polling a un database Microsoft SQL Server denominato `pubs` in un server di database denominato `ScottsServer` utilizzando l'autenticazione di Windows, passare alla directory appropriata e, dalla riga di comando, immettere:

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample2.cmd)]

Una volta aggiunta l'infrastruttura a livello di database, è necessario aggiungere i trigger alle tabelle che verranno utilizzate nelle dipendenze della cache SQL. Utilizzare `aspnet_regsql.exe` nuovamente il programma da riga di comando, ma specificare il nome della tabella utilizzando l' `-t` opzione e anziché `-ed` utilizzare l'opzione utilizza `-et` , come segue:

[!code-html[Main](using-sql-cache-dependencies-vb/samples/sample3.html)]

Per aggiungere i trigger alle `authors` tabelle e del `titles` `pubs` database in `ScottsServer` , utilizzare:

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample4.cmd)]

Per questa esercitazione aggiungere i trigger alle `Products` tabelle, `Categories` e `Suppliers` . Si esaminerà la sintassi della riga di comando specifica nel passaggio 3.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inapp_data"></a>Passaggio 2: fare riferimento a un database Microsoft SQL Server 2005 Express Edition in`App_Data`

Il `aspnet_regsql.exe` programma da riga di comando richiede il nome del database e del server per poter aggiungere l'infrastruttura di polling necessaria. Ma qual è il nome del database e del server per un database Microsoft SQL Server 2005 Express che risiede nella `App_Data` cartella? Anziché dover individuare i nomi del database e del server, ho scoperto che l'approccio più semplice consiste nell'allineare il database all'istanza del `localhost\SQLExpress` database e rinominare i dati usando [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). Se nel computer è installata una delle versioni complete di SQL Server 2005, probabilmente è già installato SQL Server Management Studio nel computer. Se si dispone solo di Express Edition, è possibile scaricare il [Microsoft SQL Server gratuito Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Per iniziare, chiudere Visual Studio. Successivamente, aprire SQL Server Management Studio e scegliere di connettersi al `localhost\SQLExpress` Server utilizzando l'autenticazione di Windows.

![Connetti al server localhost\SQLExpress](using-sql-cache-dependencies-vb/_static/image1.gif)

**Figura 1**: connessione al `localhost\SQLExpress` Server

Dopo la connessione al server, Management Studio mostrerà il server e le sottocartelle per i database, la sicurezza e così via. Fare clic con il pulsante destro del mouse sulla cartella database e scegliere l'opzione Connetti. Verrà visualizzata la finestra di dialogo Connetti database (vedere la figura 2). Fare clic sul pulsante Aggiungi e selezionare la `NORTHWND.MDF` cartella del database nella cartella dell'applicazione Web `App_Data` .

[![Alleghi NORTHWND. Database MDF dalla cartella App_Data](using-sql-cache-dependencies-vb/_static/image2.gif)](using-sql-cache-dependencies-vb/_static/image1.png)

**Figura 2**: allineare il `NORTHWND.MDF` database dalla `App_Data` cartella ([fare clic per visualizzare l'immagine con dimensioni complete](using-sql-cache-dependencies-vb/_static/image2.png))

Il database verrà aggiunto alla cartella database. Il nome del database potrebbe essere il percorso completo del file di database o il percorso completo anteposto a un [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Per evitare di dover digitare questo nome di database lungo quando si utilizza lo \_ strumento della riga di comando aspnetregsql.exe, rinominare il database con un nome più descrittivo facendo clic con il pulsante destro del mouse sul database appena collegato e scegliendo Rinomina. Il database è stato rinominato in datatutorials.

![Rinominare il database collegato con un nome più descrittivo](using-sql-cache-dependencies-vb/_static/image3.gif)

**Figura 3**: rinominare il database collegato con un nome più descrittivo

## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Passaggio 3: aggiunta dell'infrastruttura di polling al database Northwind

Ora che il database è stato collegato alla `NORTHWND.MDF` `App_Data` cartella, è possibile aggiungere l'infrastruttura di polling. Supponendo che il database sia stato rinominato in datatutorials, eseguire i quattro comandi seguenti:

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample5.cmd)]

Dopo aver eseguito questi quattro comandi, fare clic con il pulsante destro del mouse sul nome del database in Management Studio, passare al sottomenu attività e scegliere Scollega. Chiudere quindi Management Studio e riaprire Visual Studio.

Dopo la riapertura di Visual Studio, eseguire il drill-through del database tramite la Esplora server. Si noti la nuova tabella ( `AspNet_SqlCacheTablesForChangeNotification` ), le nuove stored procedure e i trigger nelle `Products` tabelle, `Categories` e `Suppliers` .

![Il database include ora l'infrastruttura di polling necessaria](using-sql-cache-dependencies-vb/_static/image4.gif)

**Figura 4**: il database include ora l'infrastruttura di polling necessaria

## <a name="step-4-configuring-the-polling-service"></a>Passaggio 4: configurazione del servizio di polling

Dopo aver creato le tabelle, i trigger e le stored procedure necessarie nel database, il passaggio finale consiste nel configurare il servizio di polling, operazione eseguita tramite `Web.config` specificando i database da utilizzare e la frequenza di polling in millisecondi. Il markup seguente esegue il polling del database Northwind una volta al secondo.

[!code-xml[Main](using-sql-cache-dependencies-vb/samples/sample6.xml)]

Il `name` valore nell' `<add>` elemento (NorthwindDB) associa un nome leggibile a un determinato database. Quando si usano le dipendenze della cache SQL, è necessario fare riferimento al nome del database definito qui e alla tabella su cui si basano i dati memorizzati nella cache. Si vedrà come usare la `SqlCacheDependency` classe per associare a livello di codice le dipendenze della cache SQL con i dati memorizzati nella cache nel passaggio 6.

Una volta stabilita una dipendenza della cache SQL, il sistema di polling si connetterà ai database definiti negli `<databases>` elementi ogni `pollTime` millisecondi ed eseguirà il `AspNet_SqlCachePollingStoredProcedure` stored procedure. Questo stored procedure, che è stato aggiunto di nuovo al passaggio 3 mediante lo `aspnet_regsql.exe` strumento da riga di comando: restituisce i `tableName` `changeId` valori e per ogni record in `AspNet_SqlCacheTablesForChangeNotification` . Le dipendenze della cache SQL obsolete vengono eliminate dalla cache.

L' `pollTime` impostazione introduce un compromesso tra prestazioni e obsolescenza dei dati. Un `pollTime` valore ridotto aumenta il numero di richieste al database, ma rimuove più rapidamente i dati non aggiornati dalla cache. Un valore più grande `pollTime` riduce il numero di richieste di database, ma aumenta il ritardo tra il momento in cui i dati back-end cambiano e quando gli elementi della cache correlati vengono rimossi. Fortunatamente, la richiesta di database esegue una semplice stored procedure che restituisce solo poche righe da una semplice tabella leggera. Tuttavia, provare a usare `pollTime` valori diversi per trovare un equilibrio ideale tra l'accesso ai database e l'obsolescenza dei dati per l'applicazione. Il valore minimo `pollTime` consentito è 500.

> [!NOTE]
> Nell'esempio precedente viene fornito un singolo `pollTime` valore nell' `<sqlCacheDependency>` elemento, ma è possibile specificare facoltativamente il `pollTime` valore nell' `<add>` elemento. Questa operazione è utile se sono stati specificati più database e si desidera personalizzare la frequenza di polling per ogni database.

## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Passaggio 5: utilizzo dichiarativo delle dipendenze della cache SQL

Nei passaggi da 1 a 4 è stata esaminata la procedura di configurazione dell'infrastruttura di database necessaria e la configurazione del sistema di polling. Con questa infrastruttura, è ora possibile aggiungere elementi alla cache di dati con una dipendenza della cache SQL associata usando tecniche programmatiche o dichiarative. In questo passaggio verrà esaminato come usare in modo dichiarativo le dipendenze della cache SQL. Nel passaggio 6 verrà esaminato l'approccio programmatico.

I [dati di memorizzazione nella cache con l'](caching-data-with-the-objectdatasource-vb.md) esercitazione di ObjectDataSource esplorano le funzionalità di memorizzazione nella cache dichiarative di ObjectDataSource. Semplicemente impostando la `EnableCaching` proprietà su `True` e la `CacheDuration` proprietà su un intervallo di tempo, ObjectDataSource memorizza automaticamente nella cache i dati restituiti dall'oggetto sottostante per l'intervallo specificato. ObjectDataSource può inoltre utilizzare una o più dipendenze della cache SQL.

Per illustrare l'uso dichiarativo delle dipendenze della cache SQL, aprire la `SqlCacheDependencies.aspx` pagina nella `Caching` cartella e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Impostare GridView s `ID` su `ProductsDeclarative` e, dal relativo smart tag, scegliere di associarlo a un nuovo ObjectDataSource denominato `ProductsDataSourceDeclarative` .

[![Creare un nuovo ObjectDataSource denominato ProductsDataSourceDeclarative](using-sql-cache-dependencies-vb/_static/image5.gif)](using-sql-cache-dependencies-vb/_static/image3.png)

**Figura 5**: creare un nuovo oggetto ObjectDataSource denominato `ProductsDataSourceDeclarative` ([fare clic per visualizzare l'immagine con dimensioni complete](using-sql-cache-dependencies-vb/_static/image4.png))

Configurare ObjectDataSource per utilizzare la `ProductsBLL` classe e impostare l'elenco a discesa nella scheda Seleziona su `GetProducts()` . Nella scheda aggiornamento, scegliere l' `UpdateProduct` Overload con tre parametri di input: `productName` , `unitPrice` e `productID` . Impostare gli elenchi a discesa su (nessuno) nelle schede Inserisci ed Elimina.

[![Usare l'overload di UpdateProduct con tre parametri di input](using-sql-cache-dependencies-vb/_static/image6.gif)](using-sql-cache-dependencies-vb/_static/image5.png)

**Figura 6**: usare l'overload di UpdateProduct con tre parametri di input ([fare clic per visualizzare l'immagine con dimensioni complete](using-sql-cache-dependencies-vb/_static/image6.png))

[![Impostare l'elenco a discesa su (nessuno) per le schede Inserisci ed Elimina](using-sql-cache-dependencies-vb/_static/image7.gif)](using-sql-cache-dependencies-vb/_static/image7.png)

**Figura 7**: impostare l'elenco a discesa su (nessuno) per le schede Inserisci ed Elimina ([fare clic per visualizzare l'immagine con dimensioni complete](using-sql-cache-dependencies-vb/_static/image8.png))

Dopo aver completato la configurazione guidata origine dati, Visual Studio creerà i BoundField e CheckBoxFields in GridView per ogni campo dati. Rimuovere tutti i campi `ProductName` , ma, `CategoryName` e `UnitPrice` , quindi formattare i campi in base alle esigenze. Dallo smart tag GridView s selezionare le caselle di controllo Abilita paging, Abilita ordinamento e Abilita modifica. In Visual Studio la proprietà ObjectDataSource s viene impostata `OldValuesParameterFormatString` su `original_{0}` . Per il corretto funzionamento della funzionalità di modifica di GridView, rimuovere completamente questa proprietà dalla sintassi dichiarativa o impostarla di nuovo sul valore predefinito, ovvero `{0}` .

Infine, aggiungere un controllo Web Label sopra GridView e impostare la relativa `ID` proprietà su `ODSEvents` e la relativa `EnableViewState` proprietà su `False` . Dopo aver apportato queste modifiche, il markup dichiarativo della pagina avrà un aspetto simile al seguente. Si noti che sono state apportate alcune personalizzazioni estetiche ai campi GridView che non sono necessarie per dimostrare la funzionalità di dipendenza della cache SQL.

[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample7.aspx)]

Successivamente, creare un gestore eventi per l'evento ObjectDataSource s `Selecting` e aggiungere il codice seguente:

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample8.vb)]

Tenere presente che l'evento ObjectDataSource s viene `Selecting` generato solo quando si recuperano dati dall'oggetto sottostante. Se ObjectDataSource accede ai dati dalla propria cache, questo evento non viene generato.

A questo punto, visitare questa pagina tramite un browser. Poiché è ancora necessario implementare la memorizzazione nella cache, ogni volta che si esegue una pagina, si ordina o si modifica la griglia, nella pagina verrà visualizzato il testo "selezione evento generato", come illustrato nella figura 8.

[![L'evento di selezione di ObjectDataSource s viene attivato ogni volta che il controllo GridView viene sottoposto a paging, modificato o ordinato](using-sql-cache-dependencies-vb/_static/image8.gif)](using-sql-cache-dependencies-vb/_static/image9.png)

**Figura 8**: l'evento ObjectDataSource s `Selecting` viene attivato ogni volta che il GridView viene sottoposto a paging, modificato o ordinato ([fare clic per visualizzare l'immagine con dimensioni complete](using-sql-cache-dependencies-vb/_static/image10.png))

Come illustrato nell'esercitazione relativa alla [memorizzazione dei dati nella cache con ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) , l'impostazione della `EnableCaching` proprietà su `True` fa sì che ObjectDataSource memorizzi nella cache i dati per la durata specificata dalla relativa `CacheDuration` Proprietà. ObjectDataSource dispone anche di una [ `SqlCacheDependency` Proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx)che aggiunge una o più dipendenze della cache SQL ai dati memorizzati nella cache usando il modello:

[!code-css[Main](using-sql-cache-dependencies-vb/samples/sample9.css)]

Dove *DatabaseName* è il nome del database specificato nell' `name` attributo dell' `<add>` elemento in `Web.config` e *TableName* è il nome della tabella di database. Ad esempio, per creare un ObjectDataSource che memorizza nella cache i dati a tempo indeterminato in base a una dipendenza della cache SQL rispetto alla `Products` tabella Northwind, impostare la proprietà ObjectDataSource s `EnableCaching` su `True` e la relativa `SqlCacheDependency` proprietà su NorthwindDB: Products.

> [!NOTE]
> È possibile utilizzare una dipendenza della cache SQL *e* una scadenza basata sul tempo impostando `EnableCaching` su `True` , `CacheDuration` sull'intervallo di tempo e `SqlCacheDependency` sui nomi di database e di tabella. ObjectDataSource eliminerà i dati quando viene raggiunta la scadenza basata sul tempo o quando il sistema di polling rileva che i dati del database sottostanti sono stati modificati, a seconda di quale evento si verifica per primo.

GridView in `SqlCacheDependencies.aspx` consente di visualizzare i dati di due tabelle `Products` e `Categories` (il campo Product s `CategoryName` viene recuperato tramite `JOIN` un `Categories` ). Pertanto, si desidera specificare due dipendenze della cache SQL: NorthwindDB: Products; NorthwindDB: categorie.

[![Configurare ObjectDataSource per supportare la memorizzazione nella cache usando le dipendenze della cache SQL da prodotti e categorie](using-sql-cache-dependencies-vb/_static/image9.gif)](using-sql-cache-dependencies-vb/_static/image11.png)

**Figura 9**: configurare ObjectDataSource per supportare la memorizzazione nella cache usando le dipendenze della cache SQL in `Products` e `Categories` ([fare clic per visualizzare l'immagine con dimensioni complete](using-sql-cache-dependencies-vb/_static/image12.png))

Dopo aver configurato ObjectDataSource per supportare la memorizzazione nella cache, rivedere la pagina tramite un browser. Anche in questo caso, il testo "selezione evento generato dovrebbe essere visualizzato nella prima pagina visita, ma dovrebbe scomparire quando si esegue il paging, l'ordinamento o si fa clic sui pulsanti modifica o Annulla. Questo è dovuto al fatto che, dopo il caricamento dei dati nella cache di ObjectDataSource, resta presente fino a quando le `Products` tabelle o non `Categories` vengono modificate o i dati vengono aggiornati tramite GridView.

Dopo aver eseguito il paging della griglia e annotando la mancanza del testo ' selezione evento generato, aprire una nuova finestra del browser e passare all'esercitazione nozioni di base nella sezione modifica, inserimento ed eliminazione ( `~/EditInsertDelete/Basics.aspx` ). Aggiornare il nome o il prezzo di un prodotto. Quindi, da alla prima finestra del browser, visualizzare un'altra pagina di dati, ordinare la griglia oppure fare clic sul pulsante di modifica di una riga. Questa volta, l'evento di selezione generato dovrebbe essere visualizzato nuovamente, perché i dati del database sottostanti sono stati modificati (vedere la figura 10). Se il testo non viene visualizzato, attendere qualche istante e riprovare. Tenere presente che il servizio di polling sta controllando le modifiche apportate alla `Products` tabella ogni `pollTime` millisecondi, pertanto si verifica un ritardo tra il momento in cui i dati sottostanti vengono aggiornati e il momento in cui vengono rimossi i dati memorizzati nella cache.

[![La modifica della tabella Products rimuove i dati del prodotto memorizzati nella cache](using-sql-cache-dependencies-vb/_static/image10.gif)](using-sql-cache-dependencies-vb/_static/image13.png)

**Figura 10**: la modifica della tabella Products consente di rimuovere i dati del prodotto memorizzati nella cache ([fare clic per visualizzare l'immagine con dimensioni complete](using-sql-cache-dependencies-vb/_static/image14.png))

## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Passaggio 6: utilizzo della classe a livello di codice `SqlCacheDependency`

I [dati di memorizzazione nella cache nell'](caching-data-in-the-architecture-vb.md) esercitazione sull'architettura hanno esaminato i vantaggi derivanti dall'utilizzo di un livello di memorizzazione nella cache separato nell'architettura, invece di associare strettamente la memorizzazione nella cache a ObjectDataSource. In questa esercitazione è stata creata una `ProductsCL` classe per dimostrare l'uso a livello di codice della cache dei dati. Per utilizzare le dipendenze della cache SQL nel livello di memorizzazione nella cache, utilizzare la `SqlCacheDependency` classe.

Con il sistema di polling, un `SqlCacheDependency` oggetto deve essere associato a una particolare coppia di tabelle e database. Il codice seguente, ad esempio, crea un `SqlCacheDependency` oggetto basato sulla tabella del database Northwind `Products` :

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample10.vb)]

I due parametri di input per il `SqlCacheDependency` Costruttore s sono rispettivamente i nomi di database e di tabella. Analogamente alla Proprietà ObjectDataSource s `SqlCacheDependency` , il nome del database utilizzato corrisponde al valore specificato nell' `name` attributo dell' `<add>` elemento in `Web.config` . Il nome della tabella è il nome effettivo della tabella di database.

Per associare un `SqlCacheDependency` oggetto a un elemento aggiunto alla cache di dati, usare uno degli `Insert` Overload del metodo che accetta una dipendenza. Il codice seguente aggiunge *valore* alla cache dei dati per una durata indefinita, ma lo associa a un oggetto `SqlCacheDependency` nella `Products` tabella. In breve, il *valore* rimarrà nella cache fino a quando non viene rimosso a causa di vincoli di memoria o perché il sistema di polling ha rilevato che la `Products` tabella è stata modificata dopo essere stata memorizzata nella cache.

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample11.vb)]

La classe del livello di memorizzazione nella cache `ProductsCL` attualmente memorizza nella cache i dati della `Products` tabella usando una scadenza di 60 secondi basata sul tempo. Let s Aggiorna questa classe in modo che utilizzi invece le dipendenze della cache SQL. Il `ProductsCL` metodo della classe `AddCacheItem` , che è responsabile dell'aggiunta dei dati alla cache, attualmente contiene il codice seguente:

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample12.vb)]

Aggiornare questo codice per usare un `SqlCacheDependency` oggetto anziché la `MasterCacheKeyArray` dipendenza della cache:

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample13.vb)]

Per testare questa funzionalità, aggiungere GridView alla pagina sotto il `ProductsDeclarative` GridView esistente. Impostare questo nuovo GridView `ID` su `ProductsProgrammatic` e, tramite il relativo smart tag, associarlo a un nuovo ObjectDataSource denominato `ProductsDataSourceProgrammatic` . Configurare ObjectDataSource per l'utilizzo della `ProductsCL` classe, impostando rispettivamente gli elenchi a discesa nelle schede seleziona e aggiorna su `GetProducts` e `UpdateProduct` .

[![Configurare ObjectDataSource per l'uso della classe ProductsCL](using-sql-cache-dependencies-vb/_static/image11.gif)](using-sql-cache-dependencies-vb/_static/image15.png)

**Figura 11**: configurare ObjectDataSource per l'uso della `ProductsCL` classe ([fare clic per visualizzare l'immagine con dimensioni complete](using-sql-cache-dependencies-vb/_static/image16.png))

[![Selezionare il metodo GetProducts dall'elenco a discesa Seleziona scheda](using-sql-cache-dependencies-vb/_static/image12.gif)](using-sql-cache-dependencies-vb/_static/image17.png)

**Figura 12**: selezionare il `GetProducts` metodo nell'elenco a discesa Seleziona scheda ([fare clic per visualizzare l'immagine con dimensioni complete](using-sql-cache-dependencies-vb/_static/image18.png))

[![Scegliere il metodo UpdateProduct dall'elenco a discesa scheda aggiornamento](using-sql-cache-dependencies-vb/_static/image13.gif)](using-sql-cache-dependencies-vb/_static/image19.png)

**Figura 13**: scegliere il metodo UpdateProduct dall'elenco a discesa scheda aggiornamento ([fare clic per visualizzare l'immagine con dimensioni complete](using-sql-cache-dependencies-vb/_static/image20.png))

Dopo aver completato la configurazione guidata origine dati, Visual Studio creerà i BoundField e CheckBoxFields in GridView per ogni campo dati. Analogamente al primo GridView aggiunto a questa pagina, rimuovere tutti i campi, ma `ProductName` , `CategoryName` e `UnitPrice` , quindi formattare i campi in base alle esigenze. Dallo smart tag GridView s selezionare le caselle di controllo Abilita paging, Abilita ordinamento e Abilita modifica. Come con `ProductsDataSourceDeclarative` ObjectDataSource, Visual Studio imposterà la `ProductsDataSourceProgrammatic` Proprietà ObjectDataSource s `OldValuesParameterFormatString` su `original_{0}` . Per il corretto funzionamento della funzionalità di modifica di GridView, impostare nuovamente questa proprietà su `{0}` (oppure rimuovere completamente l'assegnazione della proprietà dalla sintassi dichiarativa).

Al termine di queste attività, il markup dichiarativo GridView e ObjectDataSource risultante sarà simile al seguente:

[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample14.aspx)]

Per testare la dipendenza della cache SQL nel livello di memorizzazione nella cache, impostare un punto di interruzione nel `ProductCL` metodo della classe `AddCacheItem` e quindi avviare il debug. Quando si visita la prima `SqlCacheDependencies.aspx` volta, il punto di interruzione deve essere raggiunto quando i dati vengono richiesti per la prima volta e inseriti nella cache. Passare quindi a un'altra pagina in GridView o ordinare una delle colonne. In questo modo GridView esegue nuovamente la query dei dati, ma i dati devono trovarsi nella cache poiché la `Products` tabella di database non è stata modificata. Se i dati non vengono rilevati più volte nella cache, verificare che nel computer sia disponibile una quantità di memoria sufficiente e riprovare.

Dopo aver eseguito il paging di alcune pagine del GridView, aprire una seconda finestra del browser e passare all'esercitazione nozioni di base nella sezione modifica, inserimento ed eliminazione ( `~/EditInsertDelete/Basics.aspx` ). Aggiornare un record dalla tabella Products e quindi, dalla prima finestra del browser, visualizzare una nuova pagina o fare clic su una delle intestazioni di ordinamento.

In questo scenario verrà visualizzato uno dei due elementi seguenti: il punto di interruzione verrà raggiunto, a indicare che i dati memorizzati nella cache sono stati eliminati a causa della modifica del database. in alternativa, il punto di interruzione non viene raggiunto, vale a dire che `SqlCacheDependencies.aspx` ora Visualizza dati non aggiornati. Se il punto di interruzione non viene raggiunto, è probabile che il servizio di polling non sia stato ancora generato dopo che i dati sono stati modificati. Tenere presente che il servizio di polling sta controllando le modifiche apportate alla `Products` tabella ogni `pollTime` millisecondi, pertanto si verifica un ritardo tra il momento in cui i dati sottostanti vengono aggiornati e il momento in cui vengono rimossi i dati memorizzati nella cache.

> [!NOTE]
> Questo ritardo è più probabile che venga visualizzato quando si modifica uno dei prodotti tramite GridView in `SqlCacheDependencies.aspx` . Nell'esercitazione sulla [memorizzazione dei dati nella cache nell'architettura](caching-data-in-the-architecture-vb.md) `MasterCacheKeyArray` è stata aggiunta la dipendenza della cache per assicurarsi che i dati modificati tramite il metodo della classe siano `ProductsCL` `UpdateProduct` stati rimossi dalla cache. Tuttavia, questa dipendenza della cache è stata sostituita quando si modifica il `AddCacheItem` metodo precedentemente in questo passaggio e pertanto la classe continuerà `ProductsCL` a mostrare i dati memorizzati nella cache fino a quando il sistema di polling non rileva la modifica apportata alla `Products` tabella. Si vedrà come reintrodurre la dipendenza della `MasterCacheKeyArray` cache nel passaggio 7.

## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Passaggio 7: associazione di più dipendenze a un elemento memorizzato nella cache

Ricordare che la `MasterCacheKeyArray` dipendenza della cache viene utilizzata per garantire che *tutti* i dati correlati al prodotto vengano rimossi dalla cache quando viene aggiornato un singolo elemento associato al suo interno. Il metodo, ad esempio, `GetProductsByCategoryID(categoryID)` memorizza nella cache le `ProductsDataTables` istanze per ogni valore *CategoryID* univoco. Se uno di questi oggetti viene eliminato, la `MasterCacheKeyArray` dipendenza della cache garantisce che vengano rimossi anche gli altri. Senza questa dipendenza della cache, quando i dati memorizzati nella cache vengono modificati, esiste la possibilità che altri dati del prodotto memorizzati nella cache non siano aggiornati. Di conseguenza, è importante mantenere la dipendenza della `MasterCacheKeyArray` cache quando si usano le dipendenze della cache SQL. Tuttavia, il metodo della cache dei dati `Insert` consente solo un singolo oggetto dipendenza.

Inoltre, quando si utilizzano le dipendenze della cache SQL, potrebbe essere necessario associare più tabelle di database come dipendenze. Ad esempio, nella `ProductsDataTable` cache della `ProductsCL` classe sono contenuti i nomi di categoria e Supplier per ogni prodotto, ma il `AddCacheItem` metodo utilizza solo una dipendenza da `Products` . In questa situazione, se l'utente aggiorna il nome di una categoria o di un fornitore, i dati del prodotto memorizzati nella cache rimarranno nella cache e non saranno aggiornati. Pertanto, si desidera che i dati del prodotto memorizzati nella cache dipendano solo dalla `Products` tabella, ma anche dalle `Categories` `Suppliers` tabelle e.

La [ `AggregateCacheDependency` classe](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx) fornisce un mezzo per associare più dipendenze a un elemento della cache. Iniziare creando un' `AggregateCacheDependency` istanza. Aggiungere quindi il set di dipendenze usando il `AggregateCacheDependency` `Add` Metodo s. Quando si inserisce l'elemento nella cache dei dati in seguito, passare l' `AggregateCacheDependency` istanza. Quando *una* delle `AggregateCacheDependency` dipendenze dell'istanza viene modificata, l'elemento memorizzato nella cache verrà rimosso.

Di seguito è riportato il codice aggiornato per il `ProductsCL` metodo della classe `AddCacheItem` . Il metodo crea la `MasterCacheKeyArray` dipendenza della cache insieme agli `SqlCacheDependency` oggetti per `Products` le `Categories` tabelle, e `Suppliers` . Tutti questi vengono combinati in un unico `AggregateCacheDependency` oggetto denominato `aggregateDependencies` , che viene quindi passato al `Insert` metodo.

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample15.vb)]

Testare questo nuovo codice. Ora le modifiche apportate alle `Products` `Categories` tabelle, o `Suppliers` provocano la rimozione dei dati memorizzati nella cache. Inoltre, il `ProductsCL` metodo della classe `UpdateProduct` , che viene chiamato durante la modifica di un prodotto tramite GridView, rimuove la `MasterCacheKeyArray` dipendenza della cache, causando la rimozione dell'oggetto memorizzato nella cache `ProductsDataTable` e il recupero dei dati nella richiesta successiva.

> [!NOTE]
> Le dipendenze della cache SQL possono essere utilizzate anche con la [memorizzazione nella cache di output](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Per una dimostrazione di questa funzionalità, vedere [uso della memorizzazione nella cache dell'output di ASP.NET con SQL Server](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx).

## <a name="summary"></a>Riepilogo

Quando si memorizzano nella cache i dati del database, i dati rimarranno idealmente nella cache fino a quando non vengono modificati nel database. Con ASP.NET 2,0, le dipendenze della cache SQL possono essere create e usate in scenari dichiarativi e programmatici. Uno dei problemi di questo approccio consiste nell'individuare il momento in cui i dati sono stati modificati. Le versioni complete di Microsoft SQL Server 2005 forniscono funzionalità di notifica che consentono di avvisare un'applicazione quando viene modificato il risultato di una query. Per l'edizione Express di SQL Server 2005 e versioni precedenti di SQL Server, è necessario utilizzare un sistema di polling. Fortunatamente, la configurazione dell'infrastruttura di polling necessaria è piuttosto semplice.

Buona programmazione!

## <a name="further-reading"></a>Altre informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Uso delle notifiche delle query in Microsoft SQL Server 2005](https://msdn.microsoft.com/library/ms175110.aspx)
- [Creazione di una notifica di query](https://msdn.microsoft.com/library/ms188669.aspx)
- [Memorizzazione nella cache in ASP.NET con la `SqlCacheDependency` classe](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [Strumento di registrazione SQL Server ASP.NET ( `aspnet_regsql.exe` )](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [Panoramica di `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). E può essere raggiunto all'indirizzo [ mitchell@4GuysFromRolla.com .](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile all'indirizzo [http://ScottOnWriting.NET](http://ScottOnWriting.NET) .

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Marko Rangel, Teresa Murphy e Hilton Giesenow. Sei interessato a esaminare i miei prossimi articoli MSDN? In caso affermativo, rilasciare una riga in [ mitchell@4GuysFromRolla.com .](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Indietro](caching-data-at-application-startup-vb.md)
