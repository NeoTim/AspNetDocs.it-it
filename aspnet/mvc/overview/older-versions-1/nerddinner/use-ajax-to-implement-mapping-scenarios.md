---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Utilizzo di AJAX per implementare gli scenari di mapping Documenti Microsoft
author: rick-anderson
description: Il passaggio 11 mostra come integrare il supporto del mapping AJAX nell'applicazione NerdDinner, consentendo agli utenti che creano, modificano o visualizzano le cene di vedere il...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: f2e2640eb421d5ee8006915f46cbe1090b8d21ad
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542586"
---
# <a name="use-ajax-to-implement-mapping-scenarios"></a>Usare AJAX per implementare scenari di mapping

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 11 di [un'esercitazione gratuita sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come compilare un'applicazione Web piccola ma completa usando ASP.NET MVC 1.
> 
> Il passaggio 11 mostra come integrare il supporto del mapping AJAX nell'applicazione NerdDinner, consentendo agli utenti che creano, modificano o visualizzano le cene di visualizzare graficamente la posizione della cena.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [ALL'archivio musicale MVC.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>Passaggio 11 di NerdDinner: integrazione di una mappa AJAXNerdDinner Step 11: Integrating an AJAX Map

Ora renderemo la nostra applicazione un po 'più visivamente emozionante integrando il supporto di mapping AJAX. Ciò consentirà agli utenti che creano, modificano o visualizzano le cene di vedere graficamente la posizione della cena.

### <a name="creating-a-map-partial-view"></a>Creazione di una vista parziale della mappaCreating a Map Partial View

Useremo la funzionalità di mappatura in diverse posizioni all'interno dell'applicazione. Per mantenere il codice DRY incapsulamo la funzionalità mappa comune all'interno di un singolo modello parziale che è possibile riutilizzare tra più azioni controller e visualizzazioni. Questa visualizzazione parziale verrà denominata "map.ascx" e creata all'interno della directory "Views" .

È possibile creare il map.ascx parziale facendo clic con il pulsante&gt;destro del mouse sulla directory "Views" e scegliendo il comando di menu Add- View. Chiameremo la vista "Map.ascx", la controlliamo come visualizzazione parziale e indicheremo che ci accingiamo a passarla una classe modello "Dinner" fortemente tipizzata:

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Quando clicchiamo sul pulsante "Aggiungi" verrà creato il nostro modello parziale. Aggiorneremo quindi il file Map.ascx per avere il seguente contenuto:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

Il &lt;primo&gt; riferimento allo script punta alla libreria di mapping di Microsoft Virtual Earth 6.2. Il &lt;secondo&gt; riferimento allo script punta a un file map.js che creeremo a breve che incapsula la nostra logica di mapping Javascript comune. L'elemento &lt;div id&gt; "theMap" è il contenitore HTML che Virtual Earth utilizzerà per ospitare la mappa.

Abbiamo quindi un &lt;&gt; blocco di script incorporato che contiene due funzioni JavaScript specifiche di questa visualizzazione. La prima funzione utilizza jQuery per collegare una funzione che viene eseguita quando la pagina è pronta per eseguire lo script sul lato client. Chiama una funzione helper LoadMap() che definirà all'interno del file di script Map.js per caricare il controllo mappa terrestre virtuale. La seconda funzione è un gestore eventi di callback che aggiunge un pin alla mappa che identifica una posizione.

Si noti come si &lt;sta utilizzando&gt; un blocco % % lato server all'interno del blocco di script lato client per incorporare la latitudine e la longitudine della cena che si desidera mappare nel JavaScript. Si tratta di una tecnica utile per l'output di valori dinamici che possono essere utilizzati da script sul lato client (senza richiedere una chiamata AJAX separata al server per recuperare i valori, il che lo rende più veloce). I &lt;blocchi&gt; % % verranno eseguiti quando viene eseguito il rendering della visualizzazione sul server e quindi l'output del codice HTML finirà con i valori JavaScript incorporati (ad esempio: var latitude - 47.64312;).

### <a name="creating-a-mapjs-utility-library"></a>Creazione di una libreria di utilità Map.jsCreating a Map.js utility library

Creiamo ora il file Map.js che possiamo usare per incapsulare la funzionalità JavaScript per la mappa (e implementiamo i metodi LoadMap e LoadPin sopra). Possiamo farlo facendo clic con il pulsante destro del mouse sulla directory&gt;di script all'interno del nostro progetto, quindi scegliere il comando di menu "Aggiungi- Nuovo elemento", selezionare l'elemento JScript e denominarlo "Map.js".

Di seguito è riportato il codice JavaScript che aggiungeremo al file Map.js che interagirà con Virtual Earth per visualizzare la nostra mappa e aggiungere pin di posizione ad esso per le nostre cene:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Integrazione della mappa con i moduli di creazione e modifica

A questo punto il supporto della mappa verrà integrato con gli scenari di creazione e modifica esistenti. La buona notizia è che questo è abbastanza facile da fare, e non ci richiede di cambiare qualsiasi del nostro codice Controller. Poiché le visualizzazioni di creazione e modifica condividono una visualizzazione parziale "DinnerForm" comune per implementare l'interfaccia utente del modulo della cena, è possibile aggiungere la mappa in un'unica posizione e fare in modo che vengano utilizzati dagli scenari di creazione e modifica.

Tutto quello che dobbiamo fare è aprire la visualizzazione parziale "Views" Dinners.DinnerForm.ascx e aggiornarla per includere parziale la nuova mappa. Di seguito è riportato l'aspetto del DinnerForm aggiornato dopo l'aggiunta della mappa (nota: gli elementi del modulo HTML vengono omessi dal frammento di codice riportato di seguito per brevità):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

Il DinnerForm parziale precedente accetta un oggetto di tipo "DinnerFormViewModel" come tipo di modello (perché richiede sia un Dinner oggetto, nonché un SelectList per popolare l'elenco a discesa dei paesi). La nostra mappa parziale ha solo bisogno di un oggetto di tipo "Dinner" come tipo di modello, e quindi quando si esegue il rendering della mappa parziale si passa solo la sottoproprietà Dinner di DinnerFormViewModel ad esso:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

La funzione JavaScript che abbiamo aggiunto al parziale usa jQuery per allegare un evento "blur" alla casella di testo HTML "Address". Probabilmente hai sentito parlare di eventi "focus" che si attivano quando un utente fa clic o schede in una casella di testo. L'opposto è un evento "blur" che viene generato quando un utente esce da una casella di testo. Il gestore eventi precedente cancella i valori di latitudine e longitudine casella di testo quando ciò si verifica e quindi traccia la nuova posizione dell'indirizzo sulla mappa. Un gestore eventi di callback definito all'interno del file map.js aggiornerà quindi le caselle di testo longitudine e latitudine nel form utilizzando i valori restituiti da virtual earth in base all'indirizzo assegnato.

E ora quando eseguiamo di nuovo la nostra applicazione e clicchiamo sulla scheda "Host Dinner" vedremo una mappa predefinita visualizzata insieme ai nostri elementi standard del modulo Dinner:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Quando digitiamo un indirizzo e poi ci schedeamo, la mappa si aggiornerà dinamicamente per visualizzare la posizione e il nostro gestore eventi popolerà le caselle di testo di latitudine/longitudine con i valori di posizione:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Se salviamo la nuova cena e poi la apriamo di nuovo per la modifica, scopriremo che la posizione della mappa viene visualizzata al caricamento della pagina:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Ogni volta che il campo dell'indirizzo viene modificato, la mappa e le coordinate di latitudine/longitudine verranno aggiornate.

Ora che la mappa visualizza la posizione Cena, possiamo anche modificare i campi modulo Latitudine e Longitudine in modo che vengano visualizzati in elementi di testo nascosti (poiché la mappa li aggiorna automaticamente ogni volta che viene immesso un indirizzo). A tale scopo, passeremo dall'utilizzo dell'helper HTML.TextBox() all'utilizzo del metodo helper Html.Hidden():

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

E ora le nostre forme sono un po 'più user-friendly ed evitare di visualizzare la latitudine / longitudine grezza (pur memorizzandoli con ogni cena nel database):

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Integrazione della mappa con la visualizzazione Dettagli

Ora che la mappa è integrata con gli scenari di creazione e modifica, è necessario integrarla anche con lo scenario Dettagli. Tutto quello che dobbiamo fare &lt;è chiamare % Html.RenderPartial("map"); %&gt; all'interno della visualizzazione Dettagli.

Di seguito è riportato il codice sorgente per la visualizzazione Dettagli completa (con l'integrazione della mappa):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

E ora quando un utente passa a un /Dinners/Details/[id] URL vedranno i dettagli sulla cena, la posizione della cena sulla mappa (completo di un push-pin che quando si passa sopra visualizza il titolo della cena e l'indirizzo di esso), e hanno un collegamento AJAX a RSVP per esso:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementazione della ricerca della posizione nel database e nel repositoryImplementing Location Search in our Database and Repository

Per completare l'implementazione AJAX, è possibile aggiungere una mappa alla home page dell'applicazione che consente agli utenti di cercare graficamente le cene vicino a loro.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Inizieremo implementando il supporto all'interno del nostro database e repository di dati livello per eseguire in modo efficiente una ricerca del raggio basata sulla posizione per Dinners. Potremmo usare le nuove [funzionalità geospaziali di SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) per implementare questo, o in alternativa possiamo [http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) usare un approccio di funzione SQL che Gary Dryden discusso in articolo qui: e Rob Conery blogged circa l'utilizzo con LINQ to SQL qui:[http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Per implementare questa tecnica, si aprirà "Esplora Server" all'interno di Visual Studio, selezionare il database NerdDinner e quindi fare clic con il pulsante destro del mouse sul sottonodo "funzioni" sotto di esso e scegliere di creare una nuova "funzione con valori scalari":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Incileremo quindi nella seguente funzione DistanceBetween:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Creeremo quindi una nuova funzione con valori di tabella in SQL Server che chiameremo "NearestDinners":

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Questa funzione di tabella "NearestDinners" utilizza la funzione helper DistanceBetween per restituire tutte le cene entro 100 miglia dalla latitudine e dalla longitudine che forniamo:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Per chiamare questa funzione, apriremo innanzitutto la finestra di progettazione LINQ to SQLLINQ to SQL facendo doppio clic sul file NerdDinner.dbml all'interno della nostra directory di modelli:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Trascinare quindi le funzioni NearestDinners e DistanceBetween nella finestra di progettazione LINQ to SQLLINQ to SQL, che ne determinerà l'aggiunta come metodi nella classe NerdDinnerDataContext LINQ to SQL:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

È quindi possibile esporre un metodo di query "FindByLocation" nella classe DinnerRepository che utilizza la funzione NearestDinner per restituire le cene imminenti che si trovano entro 100 miglia dalla posizione specificata:We can then expose a "FindByLocation" query method on our DinnerRepository class that uses the NearestDinner function to return upcoming Dinners that are within 100 miles of the specified location:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementazione di un metodo di azione di ricerca AJAX basato su JSONImplementing a JSON-based AJAX Search Method

Ora implementeremo un metodo di azione del controller che sfrutta il nuovo metodo di repository FindByLocation() per restituire un elenco di dati Dinner che possono essere utilizzati per popolare una mappa. Questo metodo di azione restituisce i dati Dinner in un formato JSON (JavaScript Object Notation) in modo che possa essere facilmente modificato usando JavaScript sul client.

Per implementare questo, creeremo una nuova classe "SearchController" facendo clic con il&gt;pulsante destro del mouse sulla directory Controllers e scegliendo il comando di menu Aggiungi Controller. Implementeremo quindi un metodo di azione "SearchByLocation" all'interno della nuova classe SearchController come indicato di seguito:We'll then implement a "SearchByLocation" action method within the new SearchController class like below:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

Il metodo di azione SearchByLocation del SearchByLocation di SearchController chiama internamente il metodo FindByLocation su DinnerRepository per ottenere un elenco di cene nelle vicinanze. Anziché restituire il Dinner oggetti direttamente al client, tuttavia, restituisce invece JsonDinner oggetti. La classe JsonDinner espone un sottoinsieme di proprietà Dinner (ad esempio: per motivi di sicurezza non rivela i nomi delle persone che dispongono di RSVP'd per una cena). Include anche una proprietà RSVPCount che non esiste in Dinner e che viene calcolata dinamicamente contando il numero di oggetti RSVP associati a una particolare cena.

Viene quindi utilizzato il metodo helper Json() nella classe di base Controller per restituire la sequenza di cene utilizzando un formato wire basato su JSON. JSON è un formato di testo standard per la rappresentazione di strutture di dati semplici. Di seguito è riportato un esempio dell'aspetto di un elenco in formato JSON di due oggetti JsonDinner restituiti dal metodo di azione:Below is an example of what a JSON-formatted list of two JsonDinner objects looks like when returned from our action method:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Chiamata del metodo AJAX basato su JSON tramite jQueryCalling the JSON-based AJAX method using jQuery

È ora possibile aggiornare la home page dell'applicazione NerdDinner per utilizzare il metodo di azione SearchByLocation del SearchController. A tale scopo, apriremo il modello di visualizzazione /Views/Home/Index.aspx e lo aggiorneremo per avere &lt;una&gt; casella di testo, un pulsante di ricerca, la mappa e un elemento div denominato dinnerList:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

Possiamo quindi aggiungere due funzioni JavaScript alla pagina:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

La prima funzione JavaScript carica la mappa quando la pagina viene caricata per la prima volta. La seconda funzione JavaScript collega un gestore dell'evento click JavaScript sul pulsante di ricerca. Quando il pulsante viene premuto chiama la funzione JavaScript FindDinnersGivenLocation() che aggiungeremo al nostro file Map.js:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Questa funzione FindDinnersGivenLocation() chiama map. Find() sul controllo Virtual Earth per centrarlo sulla posizione immessa. Quando il servizio di mappa di terra virtuale restituisce, la mappa. Find() metodo richiama il callbackUpdateMapDinners metodo di callback è stato passato come argomento finale.

Il metodo callbackUpdateMapDinners() è dove viene eseguito il lavoro reale. Utilizza il metodo helper .post() di jQuery per eseguire una chiamata AJAX al metodo di azione SearchByLocation() del nostro SearchController, passandogli la latitudine e la longitudine della mappa appena centrata. Definisce una funzione inline che verrà chiamata al completamento del metodo di supporto .post() e i risultati della cena in formato JSON restituiti dal metodo di azione SearchByLocation() verranno passati utilizzando una variabile denominata "dinners". Quindi esegue un foreach su ogni cena restituita e utilizza la latitudine e la longitudine della cena e altre proprietà per aggiungere un nuovo pin sulla mappa. Aggiunge anche una voce per la cena all'elenco HTML delle cene a destra della mappa. Quindi collega un evento al passaggio del mouse sia per le puntine che per l'elenco HTML in modo che i dettagli relativi alla cena vengano visualizzati quando un utente passa il mouse su di essi:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

E ora quando eseguiamo l'applicazione e visitiamo la home page ci verrà presentata una mappa. Quando inseriamo il nome di una città la mappa mostrerà le cene imminenti vicino ad essa:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Passando il mouse su una cena mostrerà i dettagli su di esso.

Facendo clic sul titolo Cena sia nella bolla o sul lato destro nell'elenco HTML ci porterà alla cena – che possiamo quindi opzionalmente RSVP per:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>passaggio successivo

Ora abbiamo implementato tutte le funzionalità dell'applicazione della nostra applicazione NerdDinner.We've now implemented all the application functionality of our NerdDinner application. Diamo ora un'occhiata a come possiamo abilitare unit test automatizzati di esso.

> [!div class="step-by-step"]
> [Successivo](use-ajax-to-deliver-dynamic-updates.md)
> [precedente](enable-automated-unit-testing.md)
