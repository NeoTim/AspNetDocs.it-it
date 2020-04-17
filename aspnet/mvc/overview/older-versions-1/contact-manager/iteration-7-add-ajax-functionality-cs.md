---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
title: '#7 di iterazione: aggiunta di funzionalità Ajax (C ) Documenti Microsoft'
author: rick-anderson
description: Nella settima iterazione, miglioriamo la velocità di risposta e le prestazioni dell'applicazione aggiungendo il supporto per Ajax.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f1b0809e-8909-444e-b6bb-a5cd1dea3f72
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
msc.type: authoredcontent
ms.openlocfilehash: 12d7348382bc55af049567922bd6963970a9421b
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542313"
---
# <a name="iteration-7--add-ajax-functionality-c"></a>Iterazione 7 - Aggiungere funzionalità Ajax (C#)

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il codice](iteration-7-add-ajax-functionality-cs/_static/contactmanager_7_cs1.zip)

> Nella settima iterazione, miglioriamo la velocità di risposta e le prestazioni dell'applicazione aggiungendo il supporto per Ajax.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Compilazione di una gestione dei contatti ASP.NET'applicazione MVC (c ')

In questa serie di esercitazioni viene compilata un'intera applicazione di gestione dei contatti dall'inizio alla fine. L'applicazione Contact Manager consente di memorizzare le informazioni di contatto - nomi, numeri di telefono e indirizzi e-mail - per un elenco di persone.

Costruiamo l'applicazione su più iterazioni. Ad ogni iterazione, miglioriamo gradualmente l'applicazione. L'obiettivo di questo approccio a più iterazioni è consentire di comprendere il motivo di ogni modifica.

- Iterazione #1- Crea l'applicazione. Nella prima iterazione, creiamo il Contact Manager nel modo più semplice possibile. Aggiungiamo il supporto per le operazioni di base del database: crea, leggi, aggiorna ed elimina (CRUD).

- Iterazione #2 - Rendere l'applicazione un aspetto gradel. In questa iterazione viene migliorato l'aspetto dell'applicazione modificando la pagina master della visualizzazione MVC ASP.NET predefinita e il foglio di stile CSS.

- #3 iterazione - Aggiungere la convalida del form. Nella terza iterazione, aggiungiamo la convalida di base del form. Impediamo alle persone di inviare un modulo senza compilare i campi modulo obbligatori. Convalidiamo anche gli indirizzi e-mail e i numeri di telefono.

- Iterazione #4 - Rendere l'applicazione liberamente accoppiato. In questa quarta iterazione, sfruttiamo diversi modelli di progettazione software per semplificare la manutenzione e la modifica dell'applicazione Contact Manager. Ad esempio, viene eseguito il refactoring dell'applicazione per usare il modello Repository e il modello di inserimento delle dipendenze.

- #5 di iterazione - Creare unit test. Nella quinta iterazione, seminiamo la nostra applicazione più facile da gestire e modificare aggiungendo unit test. Ci prendiamo gioco delle classi del modello di dati e creiamo unit test per i controller e la logica di convalida.

- #6 di iterazione - Utilizzare lo sviluppo basato su test. In questa sesta iterazione, aggiungiamo nuove funzionalità all'applicazione scrivendo prima gli unit test e scrivendo il codice sugli unit test. In questa iterazione vengono aggiunti gruppi di contatti.

- Iterazione #7- Aggiungere funzionalità Ajax.Iteration #7 - Add Ajax functionality. Nella settima iterazione, miglioriamo la velocità di risposta e le prestazioni dell'applicazione aggiungendo il supporto per Ajax.

## <a name="this-iteration"></a>Questa iterazione

In questa iterazione dell'applicazione Contact Manager, viene eseguito il refactoring dell'applicazione per utilizzare Ajax. Sfruttando Ajax, rendiamo la nostra applicazione più reattiva. Possiamo evitare di eseguire il rendering di un'intera pagina quando è necessario aggiornare solo una determinata area in una pagina.

Verrà eseguito il refactoring della visualizzazione Indice in modo che non sia necessario visualizzare nuovamente l'intera pagina ogni volta che un utente seleziona un nuovo gruppo di contatti. Invece, quando qualcuno fa clic su un gruppo di contatti, aggiorneremo l'elenco dei contatti e lasceremo il resto della pagina da solo.

Cambieremo anche il modo in cui funziona il nostro link di eliminazione. Invece di visualizzare una pagina di conferma separata, verrà visualizzata una finestra di dialogo di conferma JavaScript. Se si conferma l'eliminazione di un contatto, viene eseguita un'operazione HTTP DELETE sul server per eliminare il record del contatto dal database.

Inoltre, si approfitterà di jQuery per aggiungere effetti di animazione alla nostra vista indice. Verrà visualizzata un'animazione quando il nuovo elenco di contatti viene recuperato dal server.

Infine, sfruteremo del ASP.NET il supporto del framework AJAX per la gestione della cronologia del browser. Creeremo punti della cronologia ogni volta che eseguiamo una chiamata Ajax per aggiornare l'elenco dei contatti. In questo modo, i pulsanti avanti e indietro del browser funzioneranno.

## <a name="why-use-ajax"></a>Perché usare Ajax?

L'utilizzo di Ajax ha molti vantaggi. In primo luogo, l'aggiunta di funzionalità Ajax a un'applicazione comporta una migliore esperienza utente. In una normale applicazione Web, l'intera pagina deve essere inviata al server ogni volta che un utente esegue un'azione. Ogni volta che si esegue un'azione, il browser si blocca e l'utente deve attendere che l'intera pagina venga recuperata e visualizzata nuovamente.

Si tratterebbe di un'esperienza inaccettabile nel caso di un'applicazione desktop. Ma, tradizionalmente, abbiamo vissuto con questa cattiva esperienza utente nel caso di un'applicazione web perché non sapevamo che avremmo potuto fare di meglio. Pensavamo fosse una limitazione delle applicazioni web quando, in realtà, era solo una limitazione della nostra immaginazione.

In un'applicazione Ajax, non è necessario arrestare l'esperienza utente solo per aggiornare una pagina. È invece possibile eseguire una richiesta asincrona in background per aggiornare la pagina. L'utente non consente all'utente di attendere durante l'aggiornamento di parte della pagina.

Sfruttando Ajax, è anche possibile migliorare le prestazioni dell'applicazione. Considerare il funzionamento dell'applicazione Contact Manager in questo momento senza funzionalità Ajax. Quando si fa clic su un gruppo di contatti, è necessario visualizzare nuovamente l'intera visualizzazione Dell'indice. L'elenco dei contatti e l'elenco dei gruppi di contatti devono essere recuperati dal server di database. Tutti questi dati devono essere trasmessi attraverso la rete dal server web al browser web.

Dopo aver aggiunto la funzionalità Ajax all'applicazione, tuttavia, è possibile evitare di visualizzare nuovamente l'intera pagina quando un utente fa clic su un gruppo di contatti. Non è più necessario acquisire i gruppi di contatti dal database. Inoltre, non è necessario eseguire il push dell'intera vista Indice attraverso il filo. Sfruttando Ajax, riduciamo la quantità di lavoro che il nostro server di database deve eseguire e riduciamo la quantità di traffico di rete richiesto dalla nostra applicazione.

## <a name="don-t-be-afraid-of-ajax"></a>Non aver paura di Ajax

Alcuni sviluppatori evitano di utilizzare Ajax perché si preoccupano dei browser di livello inferiore. Vogliono assicurarsi che le loro applicazioni web continueranno a funzionare quando si accede da un browser che non supporta JavaScript. Poiché Ajax dipende da JavaScript, alcuni sviluppatori evitano di utilizzare Ajax.

Tuttavia, se si è attenti a come si implementa Ajax, è possibile creare applicazioni che funzionano con i browser di livello superiore e di livello inferiore. La nostra applicazione Contact Manager funzionerà con i browser che supportano JavaScript e i browser che non lo supportano.

Se si utilizza l'applicazione Contact Manager con un browser che supporta JavaScript, si avrà una migliore esperienza utente. Ad esempio, quando si fa clic su un gruppo di contatti, verrà aggiornata solo l'area della pagina che visualizza i contatti.

Se, d'altra parte, si utilizza l'applicazione Contact Manager con un browser che non supporta JavaScript (o che ha JavaScript disabilitato) allora si avrà un'esperienza utente leggermente meno desiderabile. Ad esempio, quando si fa clic su un gruppo di contatti, l'intera visualizzazione Indice deve essere inviata al browser per visualizzare l'elenco di contatti corrispondente.

## <a name="adding-the-required-javascript-files"></a>Aggiunta dei file JavaScript necessari

Dovremo usare tre file JavaScript per aggiungere funzionalità Ajax alla nostra applicazione. Tutti e tre questi file sono inclusi nella cartella Scripts di una nuova applicazione MVC ASP.NET.

Se si prevede di utilizzare Ajax in più pagine nell'applicazione, è opportuno includere i file JavaScript necessari nella pagina master di visualizzazione s dell'applicazione. In questo modo, i file JavaScript verranno inclusi automaticamente in tutte le pagine dell'applicazione.

Aggiungere il seguente JavaScript &lt;&gt; include all'interno del tag head della pagina master di visualizzazione:

[!code-html[Main](iteration-7-add-ajax-functionality-cs/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Refactoring the Index View to use Ajax

Iniziamo modificando la nostra vista Indice in modo che facendo clic su un gruppo di contatti aggiorni solo l'area della vista che visualizza i contatti. La casella rossa in Figura 1 contiene l'area che si desidera aggiornare.

[![Aggiornamento solo dei contatti](iteration-7-add-ajax-functionality-cs/_static/image1.jpg)](iteration-7-add-ajax-functionality-cs/_static/image1.png)

**Figura 01**: Aggiornamento solo[contatti(Fare clic per visualizzare l'immagine a dimensioni intere)](iteration-7-add-ajax-functionality-cs/_static/image2.png)

Il primo passaggio consiste nel separare la parte della visualizzazione che si desidera aggiornare in modo asincrono in un parziale separato (controllo utente di visualizzazione). La sezione della vista Indice che visualizza la tabella dei contatti è stata spostata nel parziale nel listato 1.

**Listato 1 - Visualizzazioni, contatto, ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample2.aspx)]

Si noti che il partial nel listato 1 ha un modello diverso rispetto alla vista Indice. L'attributo *Inherits* &lt;nella&gt; direttiva % % della pagina specifica&lt;che&gt; il partial eredita dalla classe ViewUserControl Group.

La vista aggiornata dell'indice è contenuta nel listato 2.

**Listato 2 - Visualizzazioni, Contatto, Indice.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample3.aspx)]

Ci sono due cose che dovresti notare sulla vista aggiornata nel listato 2. In primo luogo, si noti che tutto il contenuto spostato nel partial viene sostituito con una chiamata a Html.RenderPartial(). Il metodo Html.RenderPartial() viene chiamato quando la vista Index viene richiesta per la prima volta per visualizzare il set iniziale di contatti.

In secondo luogo, si noti che html.ActionLink() utilizzato per visualizzare i gruppi di contatti è stato sostituito con un Ajax.ActionLink(). Ajax.ActionLink() viene chiamato con i seguenti parametri:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample4.aspx)]

Il primo parametro rappresenta il testo da visualizzare per il collegamento, il secondo parametro rappresenta i valori della route e il terzo parametro rappresenta le opzioni Ajax. In questo caso, utilizziamo l'opzione UpdateTargetId &lt;Ajax&gt; per puntare al tag HTML div che vogliamo aggiornare dopo il completamento della richiesta Ajax. Vogliamo aggiornare &lt;il&gt; tag div con il nuovo elenco di contatti.

Il metodo Index() aggiornato del controller di contatto è contenuto nel listato 3.

**Listato 3 - Controllers-ContactController.cs (metodo di indicizzazione)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample5.cs)]

L'azione Index() aggiornata restituisce in modo condizionale una delle due cose. Se l'azione Index() viene richiamata da una richiesta Ajax, il controller restituisce un partial. In caso contrario, l'azione Index() restituisce un'intera visualizzazione.

Si noti che l'azione Index() non deve restituire la maggior quantità di dati quando viene richiamata da una richiesta Ajax. Nel contesto di una richiesta normale, l'azione Indice restituisce un elenco di tutti i gruppi di contatti e del gruppo di contatti selezionato. Nel contesto di una richiesta Ajax, l'azione Index() restituisce solo il gruppo selezionato. Ajax significa meno lavoro sul server di database.

La nostra vista Indice modificata funziona sia nel caso di browser di livello superiore che di browser di livello inferiore. Se si fa clic su un gruppo di contatti e il browser supporta JavaScript, viene aggiornata solo l'area della visualizzazione che contiene l'elenco dei contatti. Se, d'altra parte, il browser non supporta JavaScript, viene aggiornata l'intera visualizzazione.

La visualizzazione aggiornata dell'indice presenta un problema. Quando si fa clic su un gruppo di contatti, il gruppo selezionato non viene evidenziato. Poiché l'elenco dei gruppi viene visualizzato all'esterno dell'area aggiornata durante una richiesta Ajax, il gruppo corretto non viene evidenziato. Questo problema verrà risolto nella sezione successiva.

## <a name="adding-jquery-animation-effects"></a>Aggiunta di effetti di animazione jQuery

Normalmente, quando si fa clic su un collegamento in una pagina web, è possibile utilizzare la barra di avanzamento del browser per rilevare se il browser sta recuperando attivamente il contenuto aggiornato. Quando si esegue una richiesta Ajax, d'altra parte, la barra di avanzamento del browser non mostra alcun avanzamento. Questo può rendere gli utenti nervosi. Come fai a sapere se il browser si è bloccato?

Esistono diversi modi che è possibile indicare a un utente che il lavoro viene eseguito durante l'esecuzione di una richiesta Ajax.There are several ways that you can indicate to a user that work is being performed while performing an Ajax request. Un approccio consiste nel visualizzare una semplice animazione. Ad esempio, è possibile dissolvenza in uscita di un'area quando una richiesta Ajax inizia e dissolvenza nell'area quando la richiesta viene completata.

Utilizzeremo la libreria jQuery inclusa nel framework Microsoft ASP.NET MVC, per creare gli effetti di animazione. La vista aggiornata dell'indice è contenuta nel listato 4.

**Listato 4 - Visualizzazioni, Contatto, Indice.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample6.aspx)]

Si noti che la vista Indice aggiornata contiene tre nuove funzioni JavaScript. Le prime due funzioni utilizzano jQuery per dissolvenza in uscita e dissolvenza nell'elenco dei contatti quando si fa clic su un nuovo gruppo di contatti. La terza funzione visualizza un messaggio di errore quando una richiesta Ajax genera un errore (ad esempio, timeout di rete).

La prima funzione si occupa anche di evidenziare il gruppo selezionato. All'elemento padre (l'elemento LI) dell'elemento su cui è stato fatto clic viene aggiunto un attributo di classe selezionato. Anche in questo caso, jQuery semplifica la selezione dell'elemento corretto e l'aggiunta della classe CSS.

Questi script sono associati ai collegamenti di gruppo con l'aiuto del parametro Ajax.ActionLink() AjaxOptions. La chiamata al metodo Ajax.ActionLink() aggiornata è simile alla seguente:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Aggiunta del supporto della cronologia del browser

Normalmente, quando si fa clic su un collegamento per aggiornare una pagina, la cronologia del browser viene aggiornata. In questo modo, è possibile fare clic sul pulsante Indietro del browser per tornare indietro nel tempo allo stato precedente della pagina. Ad esempio, se si fa clic sul gruppo di contatti Amici e quindi sul gruppo di contatti Business, è possibile fare clic sul pulsante Indietro del browser per tornare allo stato della pagina quando è stato selezionato il gruppo di contatti Amici.

Sfortunatamente, l'esecuzione di una richiesta Ajax non aggiorna automaticamente la cronologia del browser. Se si fa clic su un gruppo di contatti e l'elenco dei contatti corrispondenti viene recuperato con una richiesta Ajax, la cronologia del browser non viene aggiornata. Non è possibile utilizzare il pulsante Indietro del browser per tornare a un gruppo di contatti dopo aver selezionato un nuovo gruppo di contatti.

Se si desidera che gli utenti siano in grado di utilizzare il pulsante Indietro del browser dopo aver eseguito le richieste Ajax, è necessario eseguire un po' più di lavoro. È necessario sfruttare la funzionalità di gestione della cronologia del browser incorporata nella ASP.NET di AJAX Framework.

ASP.NET cronologia del browser AJAX, è necessario eseguire tre operazioni:a :

1. Abilitare cronologia del browser impostando la proprietà enableBrowserHistory su true.
2. Salvare i punti della cronologia quando lo stato di una visualizzazione cambia chiamando il metodo addHistoryPoint().
3. Ricostruire lo stato della visualizzazione quando viene generato l'evento navigate.

La vista aggiornata dell'indice è contenuta nel listato 5.

**Listato 5 - Visualizzazioni, Contatto, Indice.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample8.aspx)]

Nel listato 5, Cronologia browser è abilitata nella funzione pageInit(). La funzione pageInit() viene utilizzata anche per impostare il gestore eventi per l'evento navigate. L'evento navigate viene generato ogni volta che il pulsante Avanti o Indietro del browser determina la modifica dello stato della pagina.

Il metodo beginContactList() viene chiamato quando si fa clic su un gruppo di contatti. Questo metodo crea un nuovo punto della cronologia chiamando il metodo addHistoryPoint(). L'ID del gruppo di contatti selezionato viene aggiunto alla cronologia.

L'ID gruppo viene recuperato da un attributo expando sul collegamento del gruppo di contatti. Il rendering del collegamento viene eseguito con la chiamata seguente a Ajax.ActionLink().

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample9.aspx)]

L'ultimo parametro passato a Ajax.ActionLink() aggiunge un attributo expando denominato groupid al collegamento (minuscolo per compatibilità XHTML).

Quando un utente preme il pulsante Indietro o Avanti del browser, viene generato l'evento navigate e viene chiamato il metodo navigate(). Questo metodo aggiorna i contatti visualizzati nella pagina in modo che corrispondano allo stato della pagina che corrisponde al punto della cronologia del browser passato al metodo navigate.

## <a name="performing-ajax-deletes"></a>Esecuzione di eliminazioni AjaxPerforming Ajax Deletes

Attualmente, al fine di eliminare un contatto, è necessario fare clic sul collegamento Elimina e quindi fare clic sul pulsante Elimina visualizzato nella pagina di conferma di eliminazione (vedere Figura 2). Questo sembra un sacco di richieste di pagina per fare qualcosa di semplice come l'eliminazione di un record di database.

[![La pagina di conferma dell'eliminazione](iteration-7-add-ajax-functionality-cs/_static/image2.jpg)](iteration-7-add-ajax-functionality-cs/_static/image3.png)

**Figura 02**: La pagina di conferma dell'eliminazione[(Fare clic per visualizzare l'immagine a dimensioni intere)](iteration-7-add-ajax-functionality-cs/_static/image4.png)

Si è tentati di ignorare la pagina di conferma dell'eliminazione ed eliminare un contatto direttamente dalla visualizzazione Indice. Si dovrebbe evitare questa tentazione perché prendendo questo approccio apre l'applicazione a falle di sicurezza. In generale, non si desidera eseguire un'operazione HTTP GET quando si richiama un'azione che modifica lo stato dell'applicazione web. Quando si esegue un'eliminazione, si desidera eseguire un HTTP POST, o meglio ancora, un'operazione HTTP DELETE.

Il collegamento Elimina è contenuto nel ContactList parziale. Una versione aggiornata del ContactList parziale è contenuta nel listato 6.

**Listato 6 - Visualizzazioni, contatti, ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample10.aspx)]

Viene eseguito il rendering del collegamento Delete con la seguente chiamata al metodo Ajax.ImageActionLink():

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample11.aspx)]

> [!NOTE] 
> 
> Ajax.ImageActionLink() non è una parte standard del framework MVC ASP.NET. Ajax.ImageActionLink() è un metodo helper personalizzato incluso nel progetto Contact Manager.

Il parametro AjaxOptions ha due proprietà. In primo luogo, il Confirm proprietà viene utilizzata per visualizzare una finestra di dialogo di conferma JavaScript popup. In secondo luogo, il HttpMethod proprietà viene utilizzata per eseguire un'operazione HTTP DELETE.

Il listato 7 contiene una nuova azione AjaxDelete() che è stata aggiunta al controller di contatto.

**Listato 7 - Controllers-ContactController.cs (AjaxDelete)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample12.cs)]

L'azione AjaxDelete() è decorata con un attributo AcceptVerbs. Questo attributo impedisce che l'azione venga richiamata tranne che da qualsiasi operazione HTTP diversa da un'operazione HTTP DELETE. In particolare, non è possibile richiamare questa azione con un HTTP GET.

Dopo aver eliminato il record di database, è necessario visualizzare l'elenco aggiornato dei contatti che non contiene il record eliminato. Il metodo AjaxDelete() restituisce il ContactList parziale e l'elenco aggiornato dei contatti.

## <a name="summary"></a>Riepilogo

In questa iterazione è stata aggiunta la funzionalità Ajax all'applicazione Contact Manager. Abbiamo usato Ajax per migliorare la reattività e le prestazioni della nostra applicazione.

In primo luogo, è stato eseguito il refactoring della visualizzazione indice in modo che facendo clic su un gruppo di contatti non aggiornare l'intera visualizzazione. Al contrario, facendo clic su un gruppo di contatti viene aggiornato solo l'elenco dei contatti.

Successivamente, abbiamo usato gli effetti di animazione jQuery per dissolvenza in uscita e dissolvenza nell'elenco dei contatti. L'aggiunta di animazione a un'applicazione Ajax può essere utilizzata per fornire agli utenti dell'applicazione l'equivalente di un indicatore di stato del browser.

Abbiamo anche aggiunto il supporto della cronologia del browser alla nostra applicazione Ajax. Abbiamo abilitato gli utenti a fare clic sui pulsanti Indietro e Avanti del browser per modificare lo stato della vista Indice.

Infine, è stato creato un collegamento di eliminazione che supporta le operazioni HTTP DELETE. Eseguendo le eliminazioni Ajax, si consente agli utenti di eliminare i record del database senza richiedere all'utente di richiedere una pagina di conferma dell'eliminazione aggiuntiva.

> [!div class="step-by-step"]
> [Successivo](iteration-6-use-test-driven-development-cs.md)
> [precedente](iteration-1-create-the-application-vb.md)
