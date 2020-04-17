---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Creazione di un modello con le convalide delle regole di business Documenti Microsoft
author: rick-anderson
description: Passaggio 3 viene illustrato come creare un modello che è possibile utilizzare per eseguire query e aggiornare il database per l'applicazione NerdDinner.Step 3 shows how to create a model that we can use to both query and update the database for our NerdDinner application.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: 1a316e9051cf56cd4f1546336b334ace991c05b3
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541637"
---
# <a name="build-a-model-with-business-rule-validations"></a>Creare un modello con convalide delle regole business

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 3 di un'esercitazione gratuita [sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come compilare un'applicazione Web piccola ma completa usando ASP.NET MVC 1.
> 
> Passaggio 3 viene illustrato come creare un modello che è possibile utilizzare per eseguire query e aggiornare il database per l'applicazione NerdDinner.Step 3 shows how to create a model that we can use to both query and update the database for our NerdDinner application.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [ALL'archivio musicale MVC.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-3-building-the-model"></a>Passaggio 3 di NerdDinner: Creazione del modelloNerdDinner Step 3: Building the Model

In un framework model-view-controller il termine "modello" si riferisce agli oggetti che rappresentano i dati dell'applicazione, nonché alla logica di dominio corrispondente che integra la convalida e le regole di business con esso. Il modello è per molti aspetti il "cuore" di un'applicazione basata su MVC e, come vedremo più avanti, guida fondamentalmente il comportamento di esso.

Il framework di MVC ASP.NET supporta l'utilizzo di qualsiasi tecnologia di accesso ai dati e gli sviluppatori possono scegliere tra un'ampia gamma di opzioni di dati .NET avanzate per implementare i modelli, tra cui: LINQ to Entities, LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM o semplicemente non elaborati ADO.NET DataReader o DataSet.

Per la nostra applicazione NerdDinner useremo LINQ to SQLLINQ to SQL per creare un modello semplice che corrisponde abbastanza strettamente alla progettazione del database e aggiunge alcune regole di convalida e business personalizzate. Implementeremo quindi una classe di repository che consente di astrarre l'implementazione di persistenza dei dati dal resto dell'applicazione e ci consente di eseguire facilmente unit test.

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQLLINQ to SQL è un ORM (object relational mapper) fornito come parte di .NET 3.5.

LINQ to SQLLINQ to SQL fornisce un modo semplice per eseguire il mapping di tabelle di database a classi .NET in cui è possibile eseguire il codice. Per la nostra applicazione NerdDinner useremo per mappare le tabelle Dinners e RSVP all'interno del nostro database alle classi Dinner e RSVP. Le colonne delle tabelle Dinners e RSVP corrisponderanno alle proprietà delle classi Dinner e RSVP. Ogni oggetto Dinner e RSVP rappresenterà una riga separata all'interno delle tabelle Dinners o RSVP nel database.

LINQ to SQLLINQ to SQL consente di evitare di dover costruire manualmente istruzioni SQL per recuperare e aggiornare gli oggetti Dinner e RSVP con i dati del database. Al contrario, definiremo le classi Dinner e RSVP, la modalità di mapping al/dal database e le relazioni tra di esse. LINQ to SQLLINQ to SQL si occuperà quindi di generare la logica di esecuzione SQL appropriata da usare in fase di esecuzione quando le interagiamo e le utilizzeremo.

È possibile usare il supporto del linguaggio LINQ all'interno di VB e C , per scrivere query espressive che recuperano gli oggetti Dinner e RSVP dal database. Questo riduce al minimo la quantità di codice di dati che è necessario scrivere e ci permette di creare applicazioni veramente pulite.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Aggiunta di classi LINQ to SQL al progettoAdding LINQ to SQL Classes to our project

Inizieremo facendo clic con il pulsante destro del mouse sulla cartella "Modelli" all'interno del nostro progetto e selezioniamo il comando di menu **Aggiungi-&gt;Nuovo elemento:**

![](build-a-model-with-business-rule-validations/_static/image1.png)

Verrà eseguito la finestra di dialogo "Aggiungi nuovo elemento". Il filtro verrà eseguito in base alla categoria "Dati" e si selezionerà il modello "Classi LINQ to SQL" al suo interno:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Chiameremo la voce "NerdDinner" e fare clic sul pulsante "Aggiungi". Visual Studio aggiungerà un file NerdDinner.dbml nella directory di modelli e quindi aprirà la finestra di progettazione relazionale oggetti LINQ to SQL:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Creazione di classi del modello di dati con LINQ to SQLCreating Data Model Classes with LINQ to SQL

LINQ to SQLLINQ to SQL consente di creare rapidamente classi del modello di dati dallo schema di database esistente. A tale scopo, apriremo il database NerdDinner in Esplora server e selezioneremo le tabelle che si desidera modellare in esso:

![](build-a-model-with-business-rule-validations/_static/image4.png)

È quindi possibile trascinare le tabelle nell'area di progettazione LINQ to SQLLINQ to SQL. Quando si esegue questa operazione LINQ to SQLLINQ to SQL verranno create automaticamente le classi Dinner e RSVP utilizzando lo schema delle tabelle (con le proprietà della classe che eseguono il mapping alle colonne della tabella di database):

![](build-a-model-with-business-rule-validations/_static/image5.png)

Per impostazione predefinita, la finestra di progettazione LINQ to SQLLINQ to SQL "pluralizza" automaticamente i nomi di tabelle e colonne quando crea classi in base a uno schema di database. Ad esempio: la tabella "Dinners" nel nostro esempio precedente ha generato una classe "Dinner". Questa denominazione di classe consente di rendere i modelli coerenti con le convenzioni di denominazione .NET e in genere trovo che la progettazione risolvere questo problema conveniente (soprattutto quando si aggiungono molte tabelle). Se non si è piaciuti il nome di una classe o di una proprietà generata dalla finestra di progettazione, tuttavia, è sempre possibile eseguirne l'override e modificarlo con qualsiasi nome desiderato. È possibile eseguire questa operazione modificando il nome dell'entità/proprietà in linea all'interno della finestra di progettazione o modificandolo tramite la griglia delle proprietà.

Per impostazione predefinita, la finestra di progettazione LINQ to SQLLINQ to SQL controlla anche le relazioni chiave primaria/chiave esterna delle tabelle e in base a esse crea automaticamente "associazioni di relazioni" predefinite tra le diverse classi del modello che crea. Ad esempio, quando abbiamo trascinato le tabelle Dinners e RSVP nella finestra di progettazione LINQ to SQL è stata dedotta un'associazione di relazione uno-a-molti tra i due in base al fatto che la tabella RSVP aveva una chiave esterna per la tabella Dinners (questo è indicato dalla freccia nella finestra di progettazione):

![](build-a-model-with-business-rule-validations/_static/image6.png)

L'associazione precedente farà in modo che LINQ to SQLLINQ to SQL aggiure una proprietà "Dinner" fortemente tipizzata alla classe RSVP che gli sviluppatori possono utilizzare per accedere alla dinner associata a un determinato RSVP. Verrà inoltre la classe Dinner di una proprietà di raccolta "RSVP" che consente agli sviluppatori di recuperare e aggiornare gli oggetti RSVP associati a un particolare Dinner.

Di seguito è riportato un esempio di intellisense all'interno di Visual Studio quando si crea un nuovo oggetto RSVP e lo si aggiunge a una raccolta RSVP di Dinner. Si noti come LINQ to SQLLINQ to SQL ha aggiunto automaticamente una raccolta "RSSP" nell'oggetto Dinner:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Aggiungendo l'oggetto RSVP alla raccolta RSVP della dinner, viene detto a LINQ to SQL LINQ to SQL di associare una relazione di chiave esterna tra la riga Dinner e RSVP del database:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Se non si è come la finestra di progettazione ha modellato o denominato un'associazione di tabella, è possibile eseguirne l'override. È sufficiente fare clic sulla freccia di associazione all'interno della finestra di progettazione e accedere alle relative proprietà tramite la griglia delle proprietà per rinominarla, eliminarla o modificarla. Per l'applicazione NerdDinner, tuttavia, le regole di associazione predefinite funzionano bene per le classi del modello di dati che stiamo compilando e possiamo solo usare il comportamento predefinito.

### <a name="nerddinnerdatacontext-class"></a>Classe NerdDinnerDataContext

Visual Studio creerà automaticamente classi .NET che rappresentano i modelli e le relazioni di database definiti utilizzando la finestra di progettazione LINQ to SQLLINQ to SQL. Una classe LINQ to SQLLINQ to SQL DataContext viene generata anche per ogni file della finestra di progettazione LINQ to SQLLINQ to SQL aggiunto alla soluzione. Poiché è stato denominato l'elemento di classe LINQ to SQLLINQ to SQL "NerdDinner", la classe DataContext creata verrà denominata "NerdDinnerDataContext". Questa classe NerdDinnerDataContext è il modo principale in cui interageremo con il database.

La nostra classe NerdDinnerDataContext espone due proprietà, "Dinners" e "RSVP" , che rappresentano le due tabelle modellate all'interno del database. È possibile usare C , per scrivere query LINQ su tali proprietà per eseguire query e recuperare gli oggetti Dinner e RSVP dal database.

Il codice seguente illustra come creare un'istanza di un oggetto NerdDinnerDataContext ed eseguire una query LINQ su di esso per ottenere una sequenza di Dinners che si verificano in futuro. Visual Studio fornisce intellisense completo durante la scrittura della query LINQ e gli oggetti da essa restituiti sono fortemente tipizzati e supportano anche intellisense:Visual Studio provides full intellisense when writing the LINQ query, and the objects returned from it are strongly-typed and also support intellisense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Oltre a consentirci di eseguire una query per gli oggetti Dinner e RSVP, un Oggetto NerdDinnerDataContext tiene traccia automaticamente anche di tutte le modifiche apportate successivamente agli oggetti Dinner e RSVP che recuperiamo attraverso di esso. È possibile utilizzare questa funzionalità per salvare facilmente le modifiche nel database, senza dover scrivere codice di aggiornamento SQL esplicito.

Ad esempio, il codice seguente illustra come utilizzare una query LINQ per recuperare un singolo oggetto Dinner dal database, aggiornare due delle proprietà Dinner e quindi salvare nuovamente le modifiche nel database:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

L'oggetto NerdDinnerDataContext nel codice precedente ha automaticamente registrato le modifiche alle proprietà apportate all'oggetto Dinner recuperato da esso. Quando abbiamo chiamato il metodo "SubmitChanges()", eseguirà un'istruzione SQL "UPDATE" appropriata al database per rendere persistenti i valori aggiornati.

### <a name="creating-a-dinnerrepository-class"></a>Creazione di una classe DinnerRepositoryCreating a DinnerRepository Class

Per le applicazioni di piccole dimensioni, a volte è bene fare in modo che I controller lavorino direttamente su una classe LINQ to SQL Linq to SQL DataContext e incorporare query LINQ all'interno dei controller. Man mano che le applicazioni diventano più grandi, tuttavia, questo approccio diventa ingombrante da gestire e testare. Può anche portare a noi la duplicazione delle stesse query LINQ in più posizioni.

Un approccio che può semplificare la manutenzione e il test delle applicazioni consiste nell'utilizzare un modello di "repository". Una classe di repository consente di incapsulare la logica di query e persistenza dei dati e astrae i dettagli di implementazione della persistenza dei dati dall'applicazione. Oltre a rendere più pulito il codice dell'applicazione, l'uso di un modello di repository può semplificare la modifica delle implementazioni di archiviazione dei dati in futuro e può facilitare l'unit test di un'applicazione senza richiedere un database reale.

Per l'applicazione NerdDinner definiremo una classe DinnerRepository con la firma seguente:For our NerdDinner application we'll define a DinnerRepository class with the below signature:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Nota: Più avanti in questo capitolo estrarremo un'interfaccia IDinnerRepository da questa classe e abiliteremo l'inserimento delle dipendenze con esso nei controller. Per cominciare, però, inizieremo semplice e solo lavorare direttamente con la classe DinnerRepository.*

Per implementare questa classe fare clic con il pulsante destro del mouse sulla cartella "Modelli" e scegliere il comando di menu **Aggiungi-&gt;Nuovo elemento.** All'interno della finestra di dialogo "Aggiungi nuovo elemento" selezioneremo il modello "Classe" e denominare il file "DinnerRepository.cs":

![](build-a-model-with-business-rule-validations/_static/image10.png)

È quindi possibile implementare la classe DinnerRepository usando il codice seguente:We can then implement our DinnerRepository class using the code below:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Recupero, aggiornamento, inserimento ed eliminazione tramite la classe DinnerRepositoryRetrieving, Updating, Inserting and Deleting using the DinnerRepository class

Ora che abbiamo creato la nostra classe DinnerRepository, diamo un'occhiata ad alcuni esempi di codice che illustrano le attività comuni che possiamo fare con esso:

#### <a name="querying-examples"></a>Esempi di queryQuerying Examples

Il codice seguente recupera una singola Dinner usando il valore DinnerID:The code below retrieves a single Dinner using the DinnerID value:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

Il codice seguente recupera tutte le cene e i loop imminenti su di essi:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>Esempi di inserimento e aggiornamento

Il codice seguente illustra l'aggiunta di due nuove cene. Le aggiunte/modifiche al repository non vengono salvate nel database finché non viene chiamato il metodo "Save()". LINQ to SQLLINQ to SQL esegue automaticamente il wrapping di tutte le modifiche in una transazione di database, pertanto vengono apportate tutte le modifiche o quando il repository viene salvato:LINQ to SQL SQL automatically wraps all changes in a database transaction – so either all changes happen or none of them do when our repository saves:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

Il codice seguente recupera un oggetto Dinner esistente e modifica due proprietà su di esso. Il commit delle modifiche viene eseguito nel database quando il metodo "Save()" viene chiamato sul nostro repository:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

Il codice seguente recupera una cena e quindi vi aggiunge un RSVP. Questa operazione viene eseguito utilizzando la raccolta RSVP sull'oggetto Dinner creato automaticamente da LINQ to SQL (perché esiste una relazione chiave primaria/chiave esterna tra i due nel database). Questa modifica viene mantenuta nel database come nuova riga della tabella RSVP quando il metodo "Save()" viene chiamato nel repository:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Elimina esempio

Il codice seguente recupera un oggetto Dinner esistente e quindi lo contrassegna per l'eliminazione. Quando il metodo "Save()" viene chiamato sul repository, verrà eseguito il commit dell'eliminazione nel database:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Integrazione della convalida e della logica delle regole di business con le classi modelloIntegrating Validation and Business Rule Logic with Model Classes

L'integrazione della convalida e della logica delle regole business è una parte fondamentale di qualsiasi applicazione che funziona con i dati.

#### <a name="schema-validation"></a>Convalida dello schema

Quando le classi del modello vengono definite tramite la finestra di progettazione LINQ to SQLLINQ to SQL, i tipi di dati delle proprietà nelle classi del modello di dati corrispondono ai tipi di dati della tabella di database. Ad esempio: se la colonna "EventDate" nella tabella Dinners è un "datetime", la classe del modello di dati creata da LINQ to SQL sarà di tipo "DateTime" (che è un tipo di dati .NET incorporato). Ciò significa che si otterranno errori di compilazione se si tenta di assegnare un numero intero o booleano dal codice e verrà generato automaticamente un errore se si tenta di convertire in modo implicito un tipo stringa non valido in fase di esecuzione.

LINQ to SQLLINQ to SQL gestiscerà automaticamente l'escaping dei valori SQL per l'utilizzo di stringhe, che consente di proteggere l'utente da attacchi SQL injection quando viene utilizzato.

#### <a name="validation-and-business-rule-logic"></a>Convalida e logica delle regole di business

La convalida dello schema è utile come primo passaggio, ma raramente è sufficiente. La maggior parte degli scenari reali richiedono la possibilità di specificare una logica di convalida più completa che può estendersi su più proprietà, eseguire codice e spesso avere consapevolezza dello stato di un modello (ad esempio: viene creato /aggiornato/eliminato o all'interno di uno stato specifico di dominio come "archiviato"). Esistono una varietà di modelli e framework diversi che possono essere utilizzati per definire e applicare le regole di convalida alle classi del modello e ci sono diversi framework basati su .NET là fuori che possono essere utilizzati per aiutare con questo. È possibile utilizzare praticamente qualsiasi di essi all'interno di ASP.NET applicazioni MVC.

Ai fini della nostra applicazione NerdDinner, useremo un modello relativamente semplice e diretto in cui esponiamo una proprietà IsValid e un metodo GetRuleViolations() sul nostro oggetto modello Dinner. Il IsValid proprietà restituirà true o false a seconda se le regole di convalida e business sono tutte valide. Il metodo GetRuleViolations() restituirà un elenco di eventuali errori della regola.

Implementeremo IsValid e GetRuleViolations() per il modello Dinner aggiungendo una "classe parziale" al nostro progetto. Le classi parziali possono essere utilizzate per aggiungere metodi/proprietà/eventi alle classi gestite da una finestra di progettazione VS (ad esempio la classe Dinner generata dalla finestra di progettazione LINQ to SQLLINQ to SQL) e impedire allo strumento di interferire con il codice. È possibile aggiungere una nuova classe parziale al progetto facendo clic con il pulsante destro del mouse sulla cartella Modelli e quindi selezionare il comando di menu "Aggiungi nuovo elemento". Possiamo quindi scegliere il modello "Classe" all'interno della finestra di dialogo "Aggiungi nuovo elemento" e denominarlo Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Facendo clic sul pulsante "Aggiungi" aggiungerà un file di Dinner.cs al progetto e aprirlo all'interno dell'IDE. È quindi possibile implementare un framework di applicazione delle regole/convalida di base usando il codice seguente:We can then implement a basic rule/validation enforcement framework using the below code:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Alcune note sul codice di cui sopra:

- La classe Dinner è preceduta da una parola chiave "partial", il che significa che il codice in essa contenuto verrà combinato con la classe generata/gestita dalla finestra di progettazione LINQ to SQLE compilata in un'unica classe.
- La classe RuleViolation è una classe helper che aggiungeremo al progetto che consente di fornire ulteriori dettagli su una violazione della regola.
- Il metodo Dinner.GetRuleViolations() determina la valutazione delle regole di convalida e di business (le implementeremo a breve). Restituisce quindi una sequenza di oggetti RuleViolation che forniscono ulteriori dettagli sugli eventuali errori delle regole.
- La proprietà Dinner.IsValid fornisce una comoda proprietà helper che indica se l'oggetto Dinner dispone di RuleViolations attivi. Può essere controllato in modo proattivo da uno sviluppatore utilizzando il Dinner oggetto in qualsiasi momento (e non genera un'eccezione).
- Il metodo parziale Dinner.OnValidate() è un hook fornito da LINQ to SQLLINQ to SQL che consente di ricevere una notifica ogni volta che l'oggetto Dinner sta per essere reso persistente all'interno del database. L'implementazione OnValidate() precedente garantisce che la Dinner non abbia RuleViolations prima che venga salvata. Se si trova in uno stato non valido, genera un'eccezione, che causerà LINQ to SQLLINQ to SQL interrompere la transazione.

Questo approccio fornisce un framework semplice in cui è possibile integrare la convalida e le regole di business. Per ora aggiungiamo le seguenti regole al nostro metodo GetRuleViolations():

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Stiamo usando la funzionalità di "rendimento di rendimento" di C , per restituire una sequenza di tutte le RuleViolations. I primi sei controlli della regola precedente semplicemente applicare che le proprietà di stringa nel nostro Dinner non può essere null o vuoto. L'ultima regola è un po' più interessante e chiama un metodo helper PhoneValidator.IsValidNumber() che è possibile aggiungere al progetto per verificare che il formato del numero ContactPhone corrisponda al paese della cena.

Possiamo usare . Supporto delle espressioni regolari di NET per implementare il supporto della convalida del telefono. Di seguito è riportata una semplice implementazione PhoneValidator che è possibile aggiungere al progetto che consente di aggiungere controlli di modello Regex specifici per paese:Below is a simple PhoneValidator implementation that we can add to our project that enables us to add country-specific Regex pattern checks:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Gestione della convalida e delle violazioni della logica di businessHandling Validation and Business Logic Violations

Ora che abbiamo aggiunto la convalida sopra e il codice della regola business, ogni volta che tentiamo di creare o aggiornare una cena, le nostre regole della logica di convalida verranno valutate e applicate.

Gli sviluppatori possono scrivere codice come di seguito per determinare in modo proattivo se un Dinner oggetto è valido e recuperare un elenco di tutte le violazioni in esso senza generare eccezioni:Developers can write code like below to proactively determine if a Dinner object is valid, and retrieve a list of all violations in it without raising any exceptions:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Se tentiamo di salvare una Dinner in uno stato non valido, verrà generata un'eccezione quando chiamiamo il metodo Save() su DinnerRepository. Ciò si verifica perché LINQ to SQLLINQ to SQL chiama automaticamente il metodo parziale Dinner.OnValidate() prima di salvare le modifiche della Dinner ed è stato aggiunto codice a Dinner.OnValidate() per generare un'eccezione se esistono violazioni delle regole in Dinner. Possiamo intercettare questa eccezione e recuperare reattivamente un elenco delle violazioni da correggere:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Poiché le nostre regole di convalida e business vengono implementate all'interno del livello del modello e non all'interno del livello dell'interfaccia utente, verranno applicate e utilizzate in tutti gli scenari all'interno dell'applicazione. In seguito è possibile modificare o aggiungere regole di business e fare in modo che tutto il codice che funziona con gli oggetti Dinner li onoriamo.

Avere la flessibilità di modificare le regole di business in un'unica posizione, senza che queste modifiche si increspano in tutta l'applicazione e la logica dell'interfaccia utente, è un segno di un'applicazione ben scritta e un vantaggio che un framework MVC consente di incoraggiare.

### <a name="next-step"></a>passaggio successivo

Ora abbiamo un modello che possiamo usare sia per eseguire query e aggiornare il nostro database.

Aggiungiamo ora alcuni controller e visualizzazioni al progetto che possiamo usare per creare un'esperienza dell'interfaccia utente HTML intorno ad esso.

> [!div class="step-by-step"]
> [Successivo](create-a-database.md)
> [precedente](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
