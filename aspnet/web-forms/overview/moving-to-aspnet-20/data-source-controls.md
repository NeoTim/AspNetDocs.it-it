---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: Controlli dell'origine dati Documenti Microsoft
author: rick-anderson
description: Il controllo DataGrid in ASP.NET 1.x ha evidenziato un notevole miglioramento dell'accesso ai dati nelle applicazioni Web. Tuttavia, non era così user-friendly come avrebbe potuto essere.
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: 2b4302b509af57dc5d9db9de9ee824df767d0737
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540220"
---
# <a name="data-source-controls"></a>Controlli origine dati

da parte [di Microsoft](https://github.com/microsoft)

> Il controllo DataGrid in ASP.NET 1.x ha evidenziato un notevole miglioramento dell'accesso ai dati nelle applicazioni Web. Tuttavia, non era così user-friendly come avrebbe potuto essere. Richiedeva ancora una notevole quantità di codice per ottenere molte funzionalità utili da esso. Tale è il modello in tutti gli sforzi di accesso ai dati in 1.x.

Il controllo DataGrid in ASP.NET 1.x ha evidenziato un notevole miglioramento dell'accesso ai dati nelle applicazioni Web. Tuttavia, non era così user-friendly come avrebbe potuto essere. Richiedeva ancora una notevole quantità di codice per ottenere molte funzionalità utili da esso. Tale è il modello in tutti gli sforzi di accesso ai dati in 1.x.

ASP.NET 2.0 risolve questo problema con in parte con i controlli origine dati. I controlli origine dati in ASP.NET 2.0 forniscono agli sviluppatori un modello dichiarativo per il recupero, la visualizzazione dei dati e la modifica dei dati. Lo scopo dei controlli origine dati è fornire una rappresentazione coerente dei dati ai controlli con associazione a dati indipendentemente dall'origine di tali dati. Il cuore dei controlli origine dati in ASP.NET 2.0 è la classe astratta DataSourceControl.At the heart of the data source controls in ASP.NET 2.0 is the DataSourceControl abstract class. La classe DataSourceControl fornisce un'implementazione di base dell'interfaccia IDataSource e dell'interfaccia IListSource, quest'ultima che consente di assegnare il controllo origine dati come DataSource di un controllo con associazione a dati (tramite la nuova proprietà DataSourceId descritta più avanti) ed esporre i dati in essi presenti come elenco. Ogni elenco di dati da un controllo origine dati viene esposto come un DataSourceView oggetto. L'accesso alle istanze di DataSourceView viene fornito dall'interfaccia IDataSource.Access to the DataSourceView instances is provided by the IDataSource interface. Ad esempio, il GetViewNames metodo restituisce un ICollection che consente di enumerare il DataSourceViews associato a un controllo origine dati specifico e il GetView metodo consente di accedere a una particolare DataSourceView istanza in base al nome.

I controlli origine dati non dispongono di interfaccia utente. Vengono implementati come controlli server in modo che possano supportare la sintassi dichiarativa e in modo che abbiano accesso allo stato della pagina, se lo si desidera. I controlli origine dati non eseguono il rendering di alcun markup HTML al client.

> [!NOTE]
> Come si vedrà più avanti, esistono anche vantaggi di memorizzazione nella cache ottenuti utilizzando i controlli origine dati.

## <a name="storing-connection-strings"></a>Archiviazione delle stringhe di connessione

Prima di esaminare come configurare i controlli origine dati, è necessario coprire una nuova funzionalità in ASP.NET 2.0 per quanto riguarda le stringhe di connessione. ASP.NET 2.0 introduce una nuova sezione nel file di configurazione che consente di archiviare facilmente le stringhe di connessione che possono essere lette dinamicamente in fase di esecuzione. La &lt;sezione&gt; connectionStrings semplifica l'archiviazione delle stringhe di connessione.

Il frammento di codice seguente aggiunge una nuova stringa di connessione.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Come con &lt;la&gt; &lt;sezione appSettings,&gt; la sezione &lt;connectionStrings&gt; viene visualizzata all'esterno della sezione system.web nel file di configurazione.

Per utilizzare questa stringa di connessione, è possibile utilizzare la sintassi seguente quando si imposta l'attributo ConnectionString di un controllo server.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

La &lt;sezione&gt; connectionStrings può anche essere crittografata in modo che le informazioni riservate non vengano esposte. Tale abilità verrà trattata in un modulo successivo.

## <a name="caching-data-sources"></a>Memorizzazione nella cache delle origini datiCaching Data Sources

Ogni DataSourceControl fornisce quattro proprietà per la configurazione della memorizzazione nella cache; EnableCaching, CacheDuration, CacheExpirationPolicy e CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching (cache)EnableCaching

EnableCaching è una proprietà booleana che determina se la memorizzazione nella cache è abilitata o meno per il controllo origine dati.

## <a name="cacheduration-property"></a>CacheDuration Proprietà

La proprietà CacheDuration imposta il numero di secondi in cui la cache rimane valida. L'impostazione di questa proprietà su **0** fa sì che la cache rimanga valida fino a quando non viene invalidata in modo esplicito.

## <a name="cacheexpirationpolicy-property"></a>CacheExpirationPolicy Proprietà

La proprietà CacheExpirationPolicy può essere impostata su **Absolute** o **Sliding**. L'impostazione su Assoluto indica che la quantità massima di tempo per cui i dati verranno memorizzati nella cache è il numero di secondi specificato dalla proprietà CacheDuration. Impostandolo su Sliding, l'ora di scadenza viene reimpostata quando viene eseguita ogni operazione.

## <a name="cachekeydependency-property"></a>CacheKeyDependency Proprietà

Se viene specificato un valore stringa per la proprietà CacheKeyDependency, ASP.NET imposterà una nuova dipendenza della cache in base a tale stringa. In questo modo è possibile invalidare in modo esplicito la cache semplicemente modificando o rimuovendo CacheKeyDependency.

**Importante**: se la rappresentazione è abilitata e l'accesso all'origine dati e/o al contenuto dei dati è basato sull'identità del client, è consigliabile disabilitare la memorizzazione nella cache impostando EnableCaching su False.Important : If impersonation is enabled and access to the data source and/or content of data are based on client identity, it is recommended that caching be disabled by setting EnableCaching to False. Se la memorizzazione nella cache è abilitata in questo scenario e un utente diverso dall'utente che ha originariamente richiesto i dati invia una richiesta, l'autorizzazione all'origine dati non viene applicata. I dati saranno semplicemente serviti dalla cache.

## <a name="the-sqldatasource-control"></a>Il controllo SqlDataSourceThe SqlDataSource Control

Il controllo SqlDataSource consente a uno sviluppatore di accedere ai dati archiviati in qualsiasi database relazionale che supporta ADO.NET. Può utilizzare il provider System.Data.SqlClient per accedere a un database SQL Server, al provider System.Data.OleDb, al provider System.Data.Odbc o al provider System.Data.OracleClient per accedere a Oracle. Pertanto, il SqlDataSource non viene certamente utilizzato solo per l'accesso ai dati in un database di SQL Server.

Per utilizzare SqlDataSource, è sufficiente fornire un valore per il ConnectionString proprietà e specificare un comando SQL o stored procedure. Il controllo SqlDataSource si occupa dell'utilizzo dell'architettura di ADO.NET sottostante. Apre la connessione, esegue una query sull'origine dati o esegue la stored procedure, restituisce i dati e quindi chiude automaticamente la connessione.

> [!NOTE]
> Poiché la classe DataSourceControl chiude automaticamente la connessione, è necessario ridurre il numero di chiamate dei clienti generate dalla perdita di connessioni al database.

Il frammento di codice seguente associa un controllo DropDownList a un controllo SqlDataSource utilizzando la stringa di connessione archiviata nel file di configurazione, come illustrato in precedenza.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Come illustrato in precedenza, il DataSourceMode proprietà del SqlDataSource specifica la modalità per l'origine dati. Nell'esempio precedente, il DataSourceMode è impostato su DataReader.In the example above, the DataSourceMode is set to DataReader. In tal caso, il SqlDataSource restituirà un IDataReader oggetto utilizzando un cursore forward-only e di sola lettura. Il tipo specificato di oggetto restituito è controllato dal provider utilizzato. In questo caso, si utilizza il provider System.Data.SqlClient come specificato nella sezione &lt;connectionStrings&gt; del file web.config. Pertanto, l'oggetto restituito sarà di tipo SqlDataReader.Therefore, the object that is returned will be of type SqlDataReader. Specificando un DataSourceMode valore di DataSet, i dati possono essere archiviati in un DataSet sul server. Questa modalità consente di aggiungere funzionalità quali l'ordinamento, il paging e così via. Se fossi stato l'associazione dati di SqlDataSource a un controllo GridView, avrei scelto la modalità DataSet. Tuttavia, nel caso di un controllo DropDownList, la modalità DataReader è la scelta corretta.

> [!NOTE]
> Quando si memorizza nella cache un oggetto SqlDataSource o AccessDataSource, la proprietà DataSourceMode deve essere impostata su DataSet. Si verificherà un'eccezione se si abilita la memorizzazione nella cache con un DataSourceMode di DataReader.

## <a name="sqldatasource-properties"></a>SqlDataSource Proprietà

Di seguito sono riportate alcune delle proprietà del controllo SqlDataSource.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Valore booleano che specifica se un comando select viene annullato se uno dei parametri è null. True per impostazione predefinita.

### <a name="conflictdetection"></a>Conflictdetection

In una situazione in cui più utenti possono aggiornare un'origine dati contemporaneamente, il ConflictDetection proprietà determina il comportamento del SqlDataSource controllo. Questa proprietà restituisce uno dei valori del ConflictOptions enumerazione. Tali valori sono **CompareAllValues** e **OverwriteChanges**. Se impostato su OverwriteChanges, l'ultima persona che scrive dati nell'origine dati sovrascrive tutte le modifiche precedenti. Tuttavia, se la proprietà ConflictDetection è impostata su CompareAllValues, vengono creati parametri per le colonne restituite dal SelectCommand e vengono creati anche parametri per contenere i valori originali in ognuna di tali colonne consentendo a SqlDataSource di determinare se i valori sono stati modificati dopo l'esecuzione di SelectCommand.

### <a name="deletecommand"></a>Deletecommand

Imposta o ottiene la stringa SQL utilizzata quando si eliminano righe dal database. Può trattarsi di una query SQL o di un nome di stored procedure.

### <a name="deletecommandtype"></a>DeleteCommandType

Imposta o ottiene il tipo di comando delete, una query SQL (Text) o una stored procedure (StoredProcedure).

### <a name="deleteparameters"></a>Deleteparameters

Restituisce i parametri utilizzati dal DeleteCommand del SqlDataSourceView oggetto associato al SqlDataSource controllo.

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

Questa proprietà viene utilizzata per specificare il formato dei parametri del valore originale nei casi in cui la proprietà ConflictDetection è impostata su CompareAllValues. Il valore {0} predefinito è il che significa che i parametri del valore originale avranno lo stesso nome del parametro originale. In altre parole, se il nome del campo è @EmployeeIDEmployeeID, il parametro del valore originale sarà .

### <a name="selectcommand"></a>SelectCommand

Imposta o ottiene la stringa SQL utilizzata per recuperare i dati dal database. Può trattarsi di una query SQL o di un nome di stored procedure.

### <a name="selectcommandtype"></a>SelectCommandType (Tipo di comando)

Imposta o ottiene il tipo di comando select, una query SQL (Text) o una stored procedure (StoredProcedure).

### <a name="selectparameters"></a>Selectparameters

Restituisce i parametri utilizzati dal SelectCommand del SqlDataSourceView oggetto associato al SqlDataSource controllo.

### <a name="sortparametername"></a>SortParameterName (NomeParametro)

Ottiene o imposta il nome di un parametro di stored procedure utilizzato durante l'ordinamento dei dati recuperati dal controllo origine dati. Valido solo quando SelectCommandType è impostato su StoredProcedure.

### <a name="sqlcachedependency"></a>Sqlcachedependency

Stringa delimitata da punti e virgola che specifica i database e le tabelle utilizzati in una dipendenza della cache di SQL Server. (Le dipendenze della cache SQL verranno illustrate in un modulo successivo.)

### <a name="updatecommand"></a>Updatecommand

Imposta o ottiene la stringa SQL utilizzata durante l'aggiornamento dei dati nel database. Può trattarsi di una query SQL o di un nome di stored procedure.

### <a name="updatecommandtype"></a>UpdateCommandType (Tipo di comando)

Imposta o ottiene il tipo di comando di aggiornamento, una query SQL (Text) o una stored procedure (StoredProcedure).

### <a name="updateparameters"></a>Updateparameters

Restituisce i parametri utilizzati dal UpdateCommand del SqlDataSourceView oggetto associato al SqlDataSource controllo.

## <a name="the-accessdatasource-control"></a>Controllo AccessDataSource

Il controllo AccessDataSource deriva dalla classe SqlDataSource e viene utilizzato per l'associazione dati a un database di Microsoft Access. La proprietà ConnectionString per il controllo AccessDataSource è una proprietà di sola lettura. Anziché utilizzare la proprietà ConnectionString , la proprietà DataFile viene utilizzata per puntare al database di Access, come illustrato di seguito.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

AccessDataSource imposterà sempre il ProviderName di base SqlDataSource su System.Data.OleDb e si connetterà al database utilizzando il provider OLE DB Microsoft.Jet.OLEDB.4.0. Non è possibile utilizzare il controllo AccessDataSource per connettersi a un database di Access protetto da password. Se è necessario connettersi a un database protetto da password, è necessario utilizzare il controllo SqlDataSource.If you have to connect to a password protected database, you should use the SqlDataSource control.

> [!NOTE]
> I database di Access archiviati nel\_sito Web devono essere inseriti nella directory App Data. ASP.NET non consente l'esplorazione dei file in questa directory. Quando si utilizzano i database di Access,\_è necessario concedere all'account di processo le autorizzazioni di lettura e scrittura per la directory Dati app.

## <a name="the-xmldatasource-control"></a>Il controllo XmlDataSourceThe XmlDataSource Control

XmlDataSource viene utilizzato per l'associazione dati XML ai controlli con associazione a dati. È possibile eseguire l'associazione a un file XML utilizzando il DataFile proprietà oppure è possibile eseguire l'associazione a una stringa XML utilizzando il Data proprietà. XmlDataSource espone gli attributi XML come campi associabili. Nei casi in cui è necessario eseguire l'associazione a valori non rappresentati come attributi, è necessario utilizzare una trasformazione XSL. È inoltre possibile utilizzare espressioni XPath per filtrare i dati XML.

Si consideri il seguente file XML:

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Si noti che XmlDataSource utilizza una proprietà XPath di *People/Person* per filtrare solo i &lt;nodi Person.&gt; Il controllo DropDownList quindi data-bindss per il LastName attributo utilizzando il DataTextField proprietà.

Mentre il controllo XmlDataSource viene utilizzato principalmente per l'associazione dati ai dati XML di sola lettura, è possibile modificare il file di dati XML. Si noti che in questi casi, l'inserimento automatico, l'aggiornamento e l'eliminazione di informazioni nel file XML non vengono eseguite automaticamente come avviene con altri controlli origine dati. Sarà invece necessario scrivere codice per modificare manualmente i dati utilizzando i metodi seguenti del controllo XmlDataSource.

### <a name="getxmldocument"></a>GetXmlDocument (documento GetXml)

Recupera un oggetto XmlDocument contenente il codice XML recuperato da XmlDataSource.

### <a name="save"></a>Salvare

Salva l'XmlDocument in memoria nell'origine dati.

È importante tenere presente che il Metodo Save funzionerà solo quando vengono soddisfatte le due condizioni seguenti:

1. XmlDataSource utilizza la proprietà DataFile per eseguire l'associazione a un file XML anziché la proprietà Data da associare ai dati XML in memoria.
2. Nessuna trasformazione viene specificata tramite il Transform o TransformFile proprietà.

Si noti inoltre che il Save metodo può produrre risultati imprevisti quando viene chiamato da più utenti contemporaneamente.

## <a name="the-objectdatasource-control"></a>Controllo ObjectDataSource

I controlli origine dati che abbiamo coperto fino a questo punto sono scelte eccellenti per le applicazioni a due livelli in cui il controllo origine dati comunica direttamente all'archivio dati. Tuttavia, molte applicazioni reali sono applicazioni multilivello in cui un controllo origine dati potrebbe essere necessario comunicare con un oggetto business che, a sua volta, comunica con il livello dati. In queste situazioni, ObjectDataSource riempie il disegno di legge in modo esordio. ObjectDataSource funziona in combinazione con un oggetto di origine. Il controllo ObjectDataSource creerà un'istanza dell'oggetto di origine, chiamerà il metodo specificato ed elimina l'istanza dell'oggetto nell'ambito di una singola richiesta, se l'oggetto dispone di metodi di istanza anziché di metodi statici (Shared in Visual Basic). Pertanto, l'oggetto deve essere senza stato. Ovvero, l'oggetto deve acquisire e rilasciare tutte le risorse necessarie all'interno dell'intervallo di una singola richiesta. È possibile controllare la modalità di creazione dell'oggetto di origine gestendo l'evento ObjectCreating del controllo ObjectDataSource. È possibile creare un'istanza dell'oggetto di origine e quindi impostare la proprietà ObjectInstance della classe ObjectDataSourceEventArgs su tale istanza. Il objectDataSource controllo utilizzerà l'istanza creata nel ObjectCreating evento anziché creare un'istanza da solo.

Se l'oggetto di origine per un controllo ObjectDataSource espone metodi statici pubblici (Shared in Visual Basic) che possono essere chiamati per recuperare e modificare i dati, un controllo ObjectDataSource chiamerà direttamente tali metodi. Se un controllo ObjectDataSource deve creare un'istanza dell'oggetto di origine per effettuare chiamate al metodo, l'oggetto deve includere un costruttore pubblico che non accetta parametri. Il controllo ObjectDataSource chiamerà questo costruttore quando crea una nuova istanza dell'oggetto di origine.

Se l'oggetto di origine non contiene un costruttore pubblico senza parametri, è possibile creare un'istanza dell'oggetto di origine che verrà utilizzato dal controllo ObjectDataSource nell'evento ObjectCreating.

## <a name="specifying-object-methods"></a>Specifica dei metodi degli oggettiSpecifying Object Methods

L'oggetto di origine per un controllo ObjectDataSource può contenere un numero qualsiasi di metodi utilizzati per selezionare, inserire, aggiornare o eliminare dati. Questi metodi vengono chiamati dal controllo ObjectDataSource in base al nome del metodo, come identificato utilizzando la proprietà SelectMethod, InsertMethod, UpdateMethod o DeleteMethod del controllo ObjectDataSource. L'oggetto di origine può anche includere un metodo SelectCount facoltativo, identificato dal controllo ObjectDataSource utilizzando la proprietà SelectCountMethod, che restituisce il conteggio del numero totale di oggetti nell'origine dati. Il ObjectDataSource controllo chiamerà il SelectCount metodo dopo un Select metodo è stato chiamato per recuperare il numero totale di record nell'origine dati da utilizzare durante il paging.

## <a name="lab-using-data-source-controls"></a>Lab Using Data Source Controls

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Esercizio 1 - Visualizzazione di dati con il controllo SqlDataSource

Nell'esercizio seguente viene utilizzato il controllo SqlDataSource per connettersi al database Northwind. Si presuppone che si disponga dell'accesso al database Northwind in un'istanza di SQL Server 2000.

1. Creare un nuovo sito Web ASP.NET.
2. Aggiungere un nuovo file web.config.

    1. Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
    2. Scegliere File di configurazione Web dall'elenco dei modelli e fare clic su Aggiungi.
3. Modificare &lt;la&gt; sezione connectionStrings come segue: 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Passare alla visualizzazione Codice e aggiungere un attributo &lt;ConnectionString e&gt; un attributo SelectCommand al controllo asp:SqlDataSource nel modo seguente: 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. In visualizzazione Progettazione aggiungere un nuovo controllo GridView.From Design view, add a new GridView control.
6. Nell'elenco a discesa Scegli origine dati scegliere SqlDataSource1 dal menu Attività GridView.From the Choose Data Source dropdown in the GridView Tasks menu, choose SqlDataSource1.
7. Fare clic con il pulsante destro del mouse su Default.aspx e scegliere Visualizza nel browser dal menu. Fare clic su Sì quando viene richiesto di salvare.
8. Il controllo GridView visualizza i dati dalla tabella Products.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Esercizio 2 - Modifica dei dati con il controllo SqlDataSource

Nell'esercizio seguente viene illustrato come associare dati un controllo DropDownList utilizzando la sintassi dichiarativa e consente di modificare i dati presentati nel controllo DropDownList.

1. In visualizzazione Progettazione eliminare il controllo GridView da Default.aspx. 

    **Importante**: lasciare il controllo SqlDataSource nella pagina.
2. Aggiungere un controllo DropDownList a Default.aspx.Add a DropDownList control to Default.aspx.
3. Passare alla visualizzazione Origine.
4. Aggiungere un attributo DataSourceId, DataTextField e &lt;DataValueField&gt; al controllo asp:DropDownList come segue: 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Salvare Default.aspx e visualizzarlo nel browser. Si noti che il controllo DropDownList contiene tutti i prodotti del database Northwind.
6. Chiudere il browser.
7. Nella visualizzazione Origine di Default.aspx aggiungere un nuovo controllo TextBox sotto il controllo DropDownList. Modificare la proprietà ID della casella di testo in txtProductName.
8. Nel controllo TextBox aggiungere un nuovo controllo Button. Modificare la proprietà ID del pulsante in btnUpdate e la proprietà Text in **Update Product Name**.
9. Nella visualizzazione Origine di Default.aspx, aggiungere una proprietà UpdateCommand e due nuovi UpdateParameters al tag SqlDataSource nel modo seguente: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Si noti che in questo codice sono stati aggiunti due parametri di aggiornamento (ProductName e ProductID). Questi parametri vengono mappati alla proprietà Text della casella di testo txtProductName e alla proprietà SelectedValue di ddlProducts DropDownList.
10. Passare alla visualizzazione Progettazione e fare doppio clic sul controllo Button per aggiungere un gestore eventi.
11. Aggiungere il codice seguente al\_codice btnUpdate Click: 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Fare clic con il pulsante destro del mouse su Default.aspx e scegliere di visualizzarlo nel browser. Fare clic su Sì quando viene richiesto di salvare tutte le modifiche.
13. ASP.NET 2.0 classi parziali consentono la compilazione in fase di esecuzione. Non è necessario compilare un'applicazione per rendere effettive le modifiche al codice.
14. Selezionare un prodotto da DropDownList.
15. Immettere un nuovo nome per il prodotto selezionato nella casella di testo e quindi fare clic sul pulsante Aggiorna.
16. Il nome del prodotto viene aggiornato nel database.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Esercizio 3 Utilizzo del controllo ObjectDataSource

In questo esercizio verrà illustrato come utilizzare il controllo ObjectDataSource e un oggetto di origine per interagire con il database Northwind.

1. Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
2. Selezionare Web Form nell'elenco dei modelli. Modificare il nome in object.aspx e fare clic su Aggiungi.
3. Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
4. Selezionare Classe nell'elenco dei modelli. Modificare il nome della classe in NorthwindData.cs e fare clic su Aggiungi.
5. Fare clic su Sì quando viene\_richiesto di aggiungere la classe alla cartella App Code.
6. Aggiungere il codice seguente al file di NorthwindData.cs: 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Aggiungere il codice seguente alla visualizzazione Origine di object.aspx: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Salvare tutti i file e sfogliare object.aspx.
9. Interagisci con l'interfaccia visualizzando i dettagli, modificando i dipendenti, aggiungendo dipendenti ed eliminando i dipendenti.
