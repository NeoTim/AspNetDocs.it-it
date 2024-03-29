---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
title: Compilazione di un'interfaccia per la selezione di un account utente da molti (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà compilata un'interfaccia utente con una griglia filtrabile di paging. In particolare, l'interfaccia utente è costituita da una serie di LinkButton per...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: da53380c-a16b-41c7-a20d-24343c735c52
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
msc.type: authoredcontent
ms.openlocfilehash: 625e27442c790e7a7228b8f78b8a40dbee8e3b03
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2020
ms.locfileid: "89044584"
---
# <a name="building-an-interface-to-select-one-user-account-from-many-vb"></a>Creazione di un'interfaccia per la selezione di un account utente tra diversi account (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.12.zip) o [Scarica PDF](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_vb.pdf)

> In questa esercitazione verrà compilata un'interfaccia utente con una griglia filtrabile di paging. In particolare, l'interfaccia utente sarà costituita da una serie di LinkButton per filtrare i risultati in base alla lettera iniziale del nome utente e da un controllo GridView per visualizzare gli utenti corrispondenti. Si inizierà elencando tutti gli account utente in un controllo GridView. Quindi, nel passaggio 3, si aggiungeranno gli LinkButton del filtro. Il passaggio 4 esamina il paging dei risultati filtrati. L'interfaccia costruita nei passaggi da 2 a 4 verrà usata nelle esercitazioni successive per eseguire attività amministrative per un determinato account utente.

## <a name="introduction"></a>Introduzione

Nell'esercitazione <a id="_msoanchor_1"></a> [*assegnazione dei ruoli agli utenti*](../roles/assigning-roles-to-users-vb.md) è stata creata un'interfaccia rudimentale che consente a un amministratore di selezionare un utente e gestirne i ruoli. In particolare, l'interfaccia ha presentato all'amministratore un elenco a discesa di tutti gli utenti. Un'interfaccia di questo tipo è utile quando sono presenti, a sua volta, un account utente, ma è difficile per i siti con centinaia o migliaia di account. Una griglia filtrabile di paging è un'interfaccia utente più adatta per i siti Web con basi utente di grandi dimensioni.

In questa esercitazione verrà compilata un'interfaccia utente di questo tipo. In particolare, l'interfaccia utente sarà costituita da una serie di LinkButton per filtrare i risultati in base alla lettera iniziale del nome utente e da un controllo GridView per visualizzare gli utenti corrispondenti. Si inizierà elencando tutti gli account utente in un controllo GridView. Quindi, nel passaggio 3, si aggiungeranno gli LinkButton del filtro. Il passaggio 4 esamina il paging dei risultati filtrati. L'interfaccia costruita nei passaggi da 2 a 4 verrà usata nelle esercitazioni successive per eseguire attività amministrative per un determinato account utente.

È possibile iniziare subito.

## <a name="step-1-adding-new-aspnet-pages"></a>Passaggio 1: aggiunta di nuove pagine di ASP.NET

In questa esercitazione e nei prossimi due verranno esaminate varie funzioni e funzionalità correlate all'amministrazione. Per implementare gli argomenti esaminati in queste esercitazioni, è necessaria una serie di pagine di ASP.NET. È ora di creare queste pagine e aggiornare la mappa del sito.

Per iniziare, creare una nuova cartella nel progetto denominata `Administration` . Aggiungere quindi due nuove pagine di ASP.NET alla cartella, collegando ogni pagina con la `Site.master` pagina master. Denominare le pagine:

- `ManageUsers.aspx`
- `UserInformation.aspx`

Aggiungere anche due pagine alla directory radice del sito Web: `ChangePassword.aspx` e `RecoverPassword.aspx` .

A questo punto, queste quattro pagine hanno due controlli contenuto, uno per ogni ContentPlaceHolders della pagina master: `MainContent` e `LoginContent` .

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample1.aspx)]

Si desidera visualizzare il markup predefinito della pagina master per `LoginContent` ContentPlaceHolder per queste pagine. Rimuovere quindi il markup dichiarativo per il `Content2` controllo contenuto. Al termine di questa operazione, il markup delle pagine deve contenere solo un controllo contenuto.

Le pagine ASP.NET della `Administration` cartella sono destinate esclusivamente agli utenti amministrativi. È stato aggiunto un ruolo di amministratore al sistema nell' <a id="_msoanchor_2"></a> esercitazione [*creazione e gestione dei ruoli*](../roles/creating-and-managing-roles-vb.md) . limitare l'accesso a queste due pagine a questo ruolo. A tale scopo, aggiungere un `Web.config` file alla `Administration` cartella e configurarne l' `<authorization>` elemento per ammettere gli utenti nel ruolo Administrators e per negare tutti gli altri.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample2.xml)]

A questo punto la Esplora soluzioni del progetto dovrebbe essere simile a quella mostrata nella figura 1.

[![Sono state aggiunte quattro nuove pagine e un file di Web.config al sito Web](building-an-interface-to-select-one-user-account-from-many-vb/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image1.png)

**Figura 1**: quattro nuove pagine e un `Web.config` file sono stati aggiunti al sito Web ([fare clic per visualizzare l'immagine con dimensioni complete](building-an-interface-to-select-one-user-account-from-many-vb/_static/image3.png))

Infine, aggiornare la mappa del sito ( `Web.sitemap` ) per includere una voce nella `ManageUsers.aspx` pagina. Aggiungere il codice XML seguente dopo `<siteMapNode>` che è stato aggiunto per le esercitazioni sui ruoli.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample3.xml)]

Con la mappa del sito aggiornata, visitare il sito tramite un browser. Come illustrato nella figura 2, la navigazione a sinistra include ora gli elementi per le esercitazioni sull'amministrazione.

[![La mappa del sito include un nodo denominato amministrazione utenti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image4.png)

**Figura 2**: la mappa del sito include un nodo denominato amministrazione utenti ([fare clic per visualizzare l'immagine con dimensioni complete](building-an-interface-to-select-one-user-account-from-many-vb/_static/image6.png))

## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Passaggio 2: elenco di tutti gli account utente in un controllo GridView

L'obiettivo finale di questa esercitazione è creare una griglia filtrabile di paging tramite la quale un amministratore può selezionare un account utente da gestire. Per iniziare, elencare *tutti* gli utenti in GridView. Una volta completata questa operazione, si aggiungeranno le interfacce e le funzionalità di filtro e paging.

Aprire la `ManageUsers.aspx` pagina nella `Administration` cartella e aggiungere un controllo GridView, impostando la proprietà `ID` su `UserAccounts` in un attimo, si scriverà il codice per associare il set di account utente a GridView usando il `Membership` metodo della classe `GetAllUsers` . Come illustrato nelle esercitazioni precedenti, il `GetAllUsers` metodo restituisce un `MembershipUserCollection` oggetto, che è una raccolta di `MembershipUser` oggetti. Ogni `MembershipUser` nella raccolta include proprietà quali `UserName` , `Email` , `IsApproved` e così via.

Per visualizzare le informazioni sull'account utente desiderate in GridView, impostare la `AutoGenerateColumns` proprietà di GridView su false e aggiungere BoundField per le proprietà, `UserName` e `Email` `Comment` e CheckBoxFields per le `IsApproved` `IsLockedOut` proprietà, e `IsOnline` . Questa configurazione può essere applicata tramite il markup dichiarativo del controllo o tramite la finestra di dialogo campi. Nella figura 3 viene illustrata una schermata della finestra di dialogo campi dopo che la casella di controllo genera automaticamente campi è stata deselezionata e i BoundField e CheckBoxFields sono stati aggiunti e configurati.

[![Aggiungere tre BoundField e tre CheckBoxFields a GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image7.png)

**Figura 3**: aggiungere tre BoundField e tre CheckBoxFields al GridView ([fare clic per visualizzare l'immagine con dimensioni complete](building-an-interface-to-select-one-user-account-from-many-vb/_static/image9.png))

Dopo aver configurato GridView, verificare che il markup dichiarativo sia simile al seguente:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample4.aspx)]

Successivamente, è necessario scrivere il codice che associa gli account utente a GridView. Creare un metodo denominato `BindUserAccounts` per eseguire questa attività e quindi chiamarlo dal `Page_Load` gestore eventi nella prima pagina visita.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample5.vb)]

È possibile eseguire il test della pagina tramite un browser. Come illustrato nella figura 4, `UserAccounts` GridView elenca il nome utente, l'indirizzo di posta elettronica e altre informazioni sull'account pertinente per tutti gli utenti del sistema.

[![Gli account utente sono elencati in GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image10.png)

**Figura 4**: gli account utente sono elencati in GridView ([fare clic per visualizzare l'immagine con dimensioni complete](building-an-interface-to-select-one-user-account-from-many-vb/_static/image12.png))

## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Passaggio 3: filtrare i risultati in base alla prima lettera del nome utente

Attualmente `UserAccounts` GridView Mostra *tutti* gli account utente. Per i siti Web con centinaia o migliaia di account utente, è essenziale che l'utente sia in grado di ridurre rapidamente gli account visualizzati. Questa operazione può essere eseguita aggiungendo filtri LinkButton alla pagina. Si aggiungono 27 LinkButton alla pagina: uno denominato tutti insieme a un LinkButton per ogni lettera dell'alfabeto. Se un visitatore fa clic su tutti gli LinkButton, GridView visualizzerà tutti gli utenti. Se si fa clic su una determinata lettera, verranno visualizzati solo gli utenti il cui nome utente inizia con la lettera selezionata.

La prima attività consiste nell'aggiungere i 27 controlli LinkButton. Un'opzione consiste nel creare i 27 LinkButton in modo dichiarativo, uno alla volta. Un approccio più flessibile consiste nell'usare un controllo Repeater con un `ItemTemplate` che esegue il rendering di un LinkButton e quindi associa le opzioni di filtro al Repeater come `String` matrice.

Per iniziare, aggiungere un controllo Repeater alla pagina sopra `UserAccounts` GridView. Impostare la proprietà del ripetitore `ID` per `FilteringUI` configurare i modelli del Repeater in modo che il relativo `ItemTemplate` rendering sia un LinkButton le cui `Text` `CommandName` proprietà e sono associate all'elemento di matrice corrente. Come illustrato nell' <a id="_msoanchor_3"></a> esercitazione [*assegnazione dei ruoli agli utenti*](../roles/assigning-roles-to-users-vb.md) , questa operazione può essere eseguita usando la `Container.DataItem` sintassi di associazione dati. Utilizzare il ripetitore `SeparatorTemplate` per visualizzare una linea verticale tra ogni collegamento.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample6.aspx)]

Per popolare questo Repeater con le opzioni di filtro desiderate, creare un metodo denominato `BindFilteringUI` . Assicurarsi di chiamare questo metodo dal `Page_Load` gestore eventi nel primo caricamento della pagina.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample7.vb)]

Questo metodo specifica le opzioni di filtro come elementi nella `String` matrice `filterOptions` per ogni elemento nella matrice, il ripetitore eseguirà il rendering di un LinkButton con le relative `Text` `CommandName` proprietà e assegnate al valore dell'elemento della matrice.

La figura 5 Mostra la `ManageUsers.aspx` pagina quando viene visualizzata tramite un browser.

[![Il Repeater elenca 27 controlli LinkButton di filtro](building-an-interface-to-select-one-user-account-from-many-vb/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image13.png)

**Figura 5**: il Repeater elenca 27 controlli LinkButton di filtro ([fare clic per visualizzare l'immagine con dimensioni complete](building-an-interface-to-select-one-user-account-from-many-vb/_static/image15.png))

> [!NOTE]
> I nomi utente possono iniziare con qualsiasi carattere, inclusi i numeri e la punteggiatura. Per visualizzare questi account, l'amministratore dovrà utilizzare l'opzione all LinkButton. In alternativa, è possibile aggiungere un LinkButton per restituire tutti gli account utente che iniziano con un numero. Lo lascio come esercizio per il lettore.

Se si fa clic su uno degli LinkButton di filtro, viene generato un postback e viene generato l'evento del ripetitore, ma non viene apportata `ItemCommand` alcuna modifica alla griglia perché è ancora necessario scrivere il codice per filtrare i risultati. La `Membership` classe include un [ `FindUsersByName` Metodo](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) che restituisce gli account utente il cui nome utente corrisponde a un criterio di ricerca specificato. È possibile utilizzare questo metodo per recuperare solo gli account utente i cui nomi utente iniziano con la lettera specificata dal `CommandName` del LinkButton filtrato selezionato.

Per iniziare, aggiornare la `ManageUser.aspx` classe code-behind della pagina in modo che includa una proprietà denominata `UsernameToMatch` questa proprietà rende permanente la stringa del filtro nome utente nei postback:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample8.vb)]

La `UsernameToMatch` proprietà archivia il valore che viene assegnato alla `ViewState` raccolta utilizzando la chiave "UsernameToMatch". Quando il valore di questa proprietà viene letto, verifica se un valore è presente nella `ViewState` raccolta; in caso contrario, restituisce il valore predefinito, una stringa vuota. La `UsernameToMatch` proprietà presenta un modello comune, ovvero rende permanente un valore per visualizzare lo stato in modo che tutte le modifiche apportate alla proprietà vengano mantenute tra i postback. Per ulteriori informazioni su questo modello, vedere [informazioni sullo stato di visualizzazione ASP.NET](https://msdn.microsoftn-us/library/ms972976.aspx).

Aggiornare `BindUserAccounts` quindi il metodo in modo che, invece di chiamare `Membership.GetAllUsers` , chiami `Membership.FindUsersByName` , passando il valore della `UsernameToMatch` Proprietà accodato con il carattere jolly SQL%.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample9.vb)]

Per visualizzare solo gli utenti il cui nome utente inizia con la lettera A, impostare la `UsernameToMatch` proprietà su un oggetto, quindi chiamare `BindUserAccounts` questo risultato in una chiamata a `Membership.FindUsersByName("A%")` , che restituirà tutti gli utenti il cui nome utente inizia con. Analogamente, per restituire *tutti* gli utenti, assegnare una stringa vuota alla `UsernameToMatch` Proprietà in modo che il `BindUserAccounts` Metodo richiami `Membership.FindUsersByName("%")` , restituendo quindi tutti gli account utente.

Creare un gestore eventi per l'evento del ripetitore `ItemCommand` . Questo evento viene generato ogni volta che si fa clic su uno degli LinkButton di filtro; viene passato il valore di LinkButton selezionato `CommandName` tramite l' `RepeaterCommandEventArgs` oggetto. È necessario assegnare il valore appropriato alla `UsernameToMatch` proprietà, quindi chiamare il `BindUserAccounts` metodo. Se `CommandName` è all, assegnare una stringa vuota a in `UsernameToMatch` modo che tutti gli account utente vengano visualizzati. In caso contrario, assegnare il `CommandName` valore a `UsernameToMatch`

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample10.vb)]

Con questo codice, testare la funzionalità di filtro. Quando la pagina viene visitata per la prima volta, vengono visualizzati tutti gli account utente (fare riferimento alla figura 5). Se si fa clic su un LinkButton, viene visualizzato un postback e i risultati vengono filtrati, visualizzando solo gli account utente che iniziano con.

[![Utilizzare gli LinkButton di filtro per visualizzare gli utenti il cui nome utente inizia con una determinata lettera](building-an-interface-to-select-one-user-account-from-many-vb/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image16.png)

**Figura 6**: usare gli LinkButton di filtro per visualizzare gli utenti il cui nome utente inizia con una determinata lettera ([fare clic per visualizzare l'immagine con dimensioni complete](building-an-interface-to-select-one-user-account-from-many-vb/_static/image18.png))

## <a name="step-4-updating-the-gridview-to-use-paging"></a>Passaggio 4: aggiornamento di GridView per l'uso del paging

Il controllo GridView illustrato nelle figure 5 e 6 elenca tutti i record restituiti dal `FindUsersByName` metodo. Se sono presenti centinaia o migliaia di account utente, questo può comportare un sovraccarico di informazioni durante la visualizzazione di tutti gli account (come nel caso in cui si fa clic sul LinkButton tutti o quando inizialmente si visita la pagina). Per aiutare a presentare gli account utente in blocchi più gestibili, configurare GridView per visualizzare 10 account utente alla volta.

Il controllo GridView offre due tipi di paging:

- **Paging predefinito** -facile da implementare, ma inefficiente. In breve, con il paging predefinito, GridView prevede *tutti* i record dalla relativa origine dati. Viene quindi visualizzata solo la pagina di record appropriata.
- **Paging personalizzato** : richiede più lavoro da implementare, ma è più efficiente del paging predefinito perché con il paging personalizzato l'origine dati restituisce solo il set preciso di record da visualizzare.

La differenza di prestazioni tra il paging predefinito e quello personalizzato può essere piuttosto sostanziale durante il paging di migliaia di record. Poiché si sta compilando questa interfaccia presumendo che ci siano centinaia o migliaia di account utente, si userà il paging personalizzato.

> [!NOTE]
> Per una discussione più approfondita sulle differenze tra il paging predefinito e quello personalizzato, nonché i problemi correlati all'implementazione del paging personalizzato, vedere l'articolo relativo al [paging efficiente in grandi quantità di dati](https://asp.net/learn/data-access/tutorial-25-vb.aspx). Per un'analisi della differenza di prestazioni tra il paging predefinito e quello personalizzato, vedere [paging personalizzato in ASP.NET con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).

Per implementare il paging personalizzato, è necessario prima di tutto un meccanismo in base al quale recuperare il subset preciso di record visualizzati dal controllo GridView. La novità è che il `Membership` metodo della classe `FindUsersByName` presenta un overload che consente di specificare l'indice e le dimensioni di pagina e restituisce solo gli account utente che rientrano in tale intervallo di record.

In particolare, questo overload ha la firma seguente: [`FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)`](https://msdn.microsoft.com/library/fa5st8b2.aspx) .

Il parametro *pageIndex* specifica la pagina degli account utente da restituire; *pageSize* indica il numero di record da visualizzare per ogni pagina. Il parametro *totalRecords* è un `ByRef` parametro che restituisce il numero totale di account utente nell'archivio utente.

> [!NOTE]
> I dati restituiti da `FindUsersByName` sono ordinati in base al nome utente; i criteri di ordinamento non possono essere personalizzati.

GridView può essere configurato in modo da utilizzare il paging personalizzato, ma solo quando è associato a un controllo ObjectDataSource. Affinché il controllo ObjectDataSource implementi il paging personalizzato, richiede due metodi: uno a cui viene passato un indice della riga iniziale e il numero massimo di record da visualizzare e restituisce il subset preciso di record che rientrano in tale intervallo; e un metodo che restituisce il numero totale di record di cui viene eseguito il paging. L' `FindUsersByName` Overload accetta un indice e una dimensione di pagina e restituisce il numero totale di record tramite un `ByRef` parametro. Quindi, esiste un'interfaccia non corrispondente.

Un'opzione consiste nel creare una classe proxy che espone l'interfaccia prevista da ObjectDataSource, quindi chiama internamente il `FindUsersByName` metodo. Un'altra opzione, e quella da usare per questo articolo, consiste nel creare un'interfaccia di paging personalizzata e usarla invece dell'interfaccia di paging incorporata di GridView.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Creazione di una prima, precedente, successiva, ultima interfaccia di paging

Verrà ora compilata un'interfaccia di paging con i primi, i precedenti, i successivi e gli ultimi LinkButton. Il primo LinkButton, quando si fa clic su di esso, porta l'utente alla prima pagina di dati, mentre l'operazione precedente lo restituirà alla pagina precedente. Analogamente, Next e Last sposteranno l'utente rispettivamente nella pagina successiva e nell'ultima pagina. Aggiungere i quattro controlli LinkButton sotto `UserAccounts` GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample11.aspx)]

Successivamente, creare un gestore eventi per ogni evento di LinkButton `Click` .

Nella figura 7 vengono illustrati i quattro LinkButton quando vengono visualizzati tramite la visualizzazione di progettazione di Visual Web Developer.

[![Aggiungere LinkButton First, Previous, Next e Last sotto GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image19.png)

**Figura 7**: aggiungere LinkButton First, Previous, Next e Last sotto GridView ([fare clic per visualizzare l'immagine con dimensioni complete](building-an-interface-to-select-one-user-account-from-many-vb/_static/image21.png))

### <a name="keeping-track-of-the-current-page-index"></a>Tenere traccia dell'indice della pagina corrente

Quando un utente visita la pagina per la prima volta `ManageUsers.aspx` o fa clic su uno dei pulsanti di filtro, si desidera visualizzare la prima pagina di dati nel controllo GridView. Quando l'utente fa clic su uno degli LinkButton di navigazione, tuttavia, è necessario aggiornare l'indice della pagina. Per mantenere l'indice di pagina e il numero di record da visualizzare per pagina, aggiungere le due proprietà seguenti alla classe code-behind della pagina:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample12.vb)]

Analogamente alla `UsernameToMatch` proprietà, la `PageIndex` proprietà Salva in modo permanente il relativo valore per visualizzare lo stato. La proprietà di sola lettura `PageSize` restituisce un valore hardcoded, 10. Invito il lettore interessato ad aggiornare questa proprietà per usare lo stesso modello di `PageIndex` e quindi per aumentare la pagina in `ManageUsers.aspx` modo che la persona che visita la pagina possa specificare il numero di account utente da visualizzare per ogni pagina.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Recupero solo dei record della pagina corrente, aggiornamento dell'indice della pagina e abilitazione e disabilitazione degli LinkButton dell'interfaccia di paging

Con l'interfaccia di paging sul posto e `PageIndex` le `PageSize` proprietà e aggiunte, è possibile aggiornare il `BindUserAccounts` metodo in modo che usi l'overload appropriato `FindUsersByName` . Inoltre, è necessario che questo metodo consenta di abilitare o disabilitare l'interfaccia di paging a seconda della pagina visualizzata. Quando si visualizza la prima pagina di dati, è necessario disabilitare il primo e il collegamento precedente; È necessario disabilitare Next e Last per visualizzare l'ultima pagina.

Aggiornare il metodo `BindUserAccounts` con il codice seguente:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample13.vb)]

Si noti che il numero totale di record di cui viene eseguito il paging è determinato dall'ultimo parametro del `FindUsersByName` metodo. Dopo che la pagina specificata degli account utente viene restituita, i quattro LinkButton sono abilitati o disabilitati, a seconda che venga visualizzata la prima o l'ultima pagina di dati.

L'ultimo passaggio consiste nel scrivere il codice per i quattro `Click` gestori eventi di LinkButton. Questi gestori di eventi devono aggiornare la `PageIndex` proprietà e quindi riassociare i dati al GridView tramite una chiamata al `BindUserAccounts` primo, il primo e il successivo gestore eventi sono molto semplici. Il `Click` gestore eventi per l'ultimo LinkButton, tuttavia, è un po' più complesso perché è necessario determinare il numero di record visualizzati per determinare l'ultimo indice di pagina.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample14.vb)]

Nelle figure 8 e 9 è illustrata l'interfaccia di paging personalizzata. Nella figura 8 viene illustrata la `ManageUsers.aspx` pagina durante la visualizzazione della prima pagina di dati per tutti gli account utente. Si noti che vengono visualizzati solo 10 dei 13 account. Se si fa clic sul collegamento successivo o ultimo, viene attivato un postback, viene aggiornato `PageIndex` a 1 e viene associata la seconda pagina degli account utente alla griglia (vedere la figura 9).

[![Vengono visualizzati i primi 10 account utente](building-an-interface-to-select-one-user-account-from-many-vb/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image22.png)

**Figura 8**: vengono visualizzati i primi 10 account utente ([fare clic per visualizzare l'immagine con dimensioni complete](building-an-interface-to-select-one-user-account-from-many-vb/_static/image24.png))

[![Facendo clic sul collegamento successivo viene visualizzata la seconda pagina degli account utente](building-an-interface-to-select-one-user-account-from-many-vb/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image25.png)

**Figura 9**: facendo clic sul collegamento successivo viene visualizzata la seconda pagina di account utente ([fare clic per visualizzare l'immagine con dimensioni complete](building-an-interface-to-select-one-user-account-from-many-vb/_static/image27.png))

## <a name="summary"></a>Riepilogo

Gli amministratori devono spesso selezionare un utente dall'elenco di account. Nelle esercitazioni precedenti è stato esaminato l'uso di un elenco a discesa popolato con gli utenti, ma questo approccio non può essere ridimensionato correttamente. In questa esercitazione è stata illustrata un'alternativa migliore: un'interfaccia filtrabile i cui risultati vengono visualizzati in un GridView di paging. Con questa interfaccia utente, gli amministratori possono individuare e selezionare un account utente in modo rapido ed efficiente tra le migliaia.

Buona programmazione!

### <a name="further-reading"></a>Altre informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Paging personalizzato in ASP.NET con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Paging efficiente in grandi quantità di dati](https://asp.net/learn/data-access/tutorial-25-vb.aspx)
- [Implementazione dello strumento di amministrazione del sito Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è *[Sams Teach Yourself ASP.NET 2,0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott può essere raggiunto in [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o tramite il suo Blog all'indirizzo [http://ScottOnWriting.NET](http://scottonwriting.net/) .

### <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore responsabile di questa esercitazione è Alicja Maziarz. Sei interessato a esaminare i miei prossimi articoli MSDN? In caso affermativo, rilasciare una riga in

> [!div class="step-by-step"]
> [Precedente](unlocking-and-approving-user-accounts-cs.md) 
>  [Avanti](recovering-and-changing-passwords-vb.md)
