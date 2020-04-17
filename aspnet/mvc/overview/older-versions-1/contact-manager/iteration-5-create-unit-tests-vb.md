---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
title: '#5 di iterazione – Creare unit test (VB) Documenti Microsoft'
author: rick-anderson
description: Nella quinta iterazione, seminiamo la nostra applicazione più facile da gestire e modificare aggiungendo unit test. Abbiamo preso in giro le nostre classi del modello di dati e compilare unit test per o ...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: c6e5c036-2265-4fa7-a9eb-47f197bdc262
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
msc.type: authoredcontent
ms.openlocfilehash: cc5425de86e7481894fbea242fa555b5598167f6
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542339"
---
# <a name="iteration-5--create-unit-tests-vb"></a>Iterazione 5 - Creare gli unit test (VB)

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il codice](iteration-5-create-unit-tests-vb/_static/contactmanager_5_vb1.zip)

> Nella quinta iterazione, seminiamo la nostra applicazione più facile da gestire e modificare aggiungendo unit test. Ci prendiamo gioco delle classi del modello di dati e creiamo unit test per i controller e la logica di convalida.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creazione di una gestione dei contatti ASP.NET applicazione MVC (VB)Building a Contact Management ASP.NET MVC Application (VB)

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

Nell'iterazione precedente dell'applicazione Contact Manager, è stato eseguito il refactoring dell'applicazione in modo che sia accoppiata in modo più uniforme. L'applicazione è stata separata in livelli distinti di controller, servizio e repository. Ogni livello interagisce con il livello sottostante tramite interfacce.

È stato eseguito il refactoring dell'applicazione per semplificare la gestione e la modifica dell'applicazione. Ad esempio, se è necessario utilizzare una nuova tecnologia di accesso ai dati, è possibile modificare semplicemente il livello del repository senza toccare il controller o il livello di servizio. Rendendo il Contact Manager liberamente accoppiato, abbiamo reso l'applicazione più resiliente al cambiamento.

Ma cosa succede quando è necessario aggiungere una nuova funzionalità all'applicazione Contact Manager? Oppure, cosa succede quando rileviamo un bug? Una triste, ma ben provata, verità di scrittura di codice è che ogni volta che si tocca il codice si crea il rischio di introdurre nuovi bug.

Ad esempio, un bel giorno, il manager potrebbe chiederti di aggiungere una nuova funzionalità a Contact Manager. Vuole che aggiungi il supporto per i gruppi di contatti. Desidera consentire agli utenti di organizzare i contatti in gruppi quali Amici, Aziende e così via.

Per implementare questa nuova funzionalità, è necessario modificare tutti e tre i livelli dell'applicazione Contact Manager. Sarà necessario aggiungere nuove funzionalità ai controller, al livello di servizio e al repository. Non appena si inizia a modificare il codice, si rischia di interrompere la funzionalità che ha funzionato in precedenza.

Il refactoring dell'applicazione in livelli separati, come abbiamo fatto nell'iterazione precedente, è stata una buona cosa. E 'stata una buona cosa perché ci permette di apportare modifiche a interi livelli senza toccare il resto dell'applicazione. Tuttavia, se si desidera semplificare la gestione e la modifica del codice all'interno di un livello, è necessario creare unit test per il codice.

Utilizzare uno unit test per testare una singola unità di codice. Queste unità di codice sono più piccole di interi livelli dell'applicazione. In genere, si utilizza uno unit test per verificare se un particolare metodo nel codice si comporta nel modo previsto. Ad esempio, è necessario creare uno unit test per il metodo CreateContact() esposto dalla classe ContactManagerService.

Gli unit test per un'applicazione funzionano come una rete di sicurezza. Ogni volta che si modifica il codice in un'applicazione, è possibile eseguire un set di unit test per verificare se la modifica interrompe la funzionalità esistente. Gli unit test rendono il codice sicuro da modificare. Gli unit test rendono tutto il codice nell'applicazione più resiliente alle modifiche.

In questa iterazione vengono aggiunti unit test all'applicazione Contact Manager. In questo modo, nell'iterazione successiva, è possibile aggiungere gruppi di contatti all'applicazione senza preoccuparsi di interrompere le funzionalità esistenti.

> [!NOTE] 
> 
> Esistono diversi framework di unit test, tra cui NUnit, xUnit.net e MbUnit.There are a variety of unit testing frameworks including NUnit, xUnit.net, and MbUnit. In this tutorial, we use the unit testing framework included with Visual Studio. Tuttavia, si potrebbe altrettanto facilmente utilizzare uno di questi quadri alternativi.

## <a name="what-gets-tested"></a>Elementi testati

Nel mondo perfetto, tutto il codice sarebbe coperto da unit test. Nel mondo perfetto, si avrebbe la rete di sicurezza perfetta. Si sarebbe in grado di modificare qualsiasi riga di codice nell'applicazione e sapere immediatamente, eseguendo gli unit test, se la modifica ha interrotto la funzionalità esistente.

Tuttavia, non viviamo in un mondo perfetto. In pratica, quando si scrivono unit test, ci si concentra sulla scrittura di test per la logica di business (ad esempio, la logica di convalida). In particolare, *non* si scrivono unit test per la logica di accesso ai dati o la logica di visualizzazione.

Per essere utile, gli unit test devono essere eseguiti molto rapidamente. È possibile accumulare facilmente centinaia (o addirittura migliaia) di unit test per un'applicazione. Se l'esecuzione degli unit test richiede molto tempo, si eviterà di eseguirli. In altre parole, gli unit test a esecuzione prolungata sono inutili per scopi di codifica giorno per giorno.

Per questo motivo, in genere non si scrivono unit test per il codice che interagisce con un database. L'esecuzione di centinaia di unit test su un database attivo sarebbe troppo lenta. Al contrario, si prende in giro il database e scrivere il codice che interagisce con il database fittizio (si discute di simulazione di un database di seguito).

Per un motivo simile, in genere non si scrivono unit test per le visualizzazioni. Per testare una vista, è necessario creare un server Web. Poiché la rotazione di un server Web è un processo relativamente lento, la creazione di unit test per le visualizzazioni non è consigliata.

Se la visualizzazione contiene logica complessa, è consigliabile spostare la logica in metodi Helper.If your view contains complicated logic then you should consider moving the logic into Helper methods. È possibile scrivere unit test per i metodi Helper che vengono eseguiti senza eseguire il ripinning di un server Web.

> [!NOTE] 
> 
> Mentre la scrittura di test per la logica di accesso ai dati o la logica di visualizzazione non è una buona idea quando si scrivono unit test, questi test possono essere molto utili quando si creano test funzionali o di integrazione.

> [!NOTE] 
> 
> ASP.NET MVC è il motore di visualizzazione Web Form. Mentre il motore di visualizzazione Web Form dipende da un server Web, altri motori di visualizzazione potrebbero non esserlo.

## <a name="using-a-mock-object-framework"></a>Utilizzo di un Framework oggetti MockUsing a Mock Object Framework

Quando si compilano unit test, è quasi sempre necessario sfruttare un framework di Mock Object.When building unit tests, you almost always need to take advantage of a Mock Object framework. Un framework di oggetto fittizio consente di creare simulazioni e stub per le classi nell'applicazione.

Ad esempio, è possibile utilizzare un framework di Mock Object per generare una versione fittizia della classe di repository. In questo modo, è possibile utilizzare la classe di repository fittizia anziché la classe di repository reale negli unit test. L'uso del repository fittizio consente di evitare l'esecuzione di codice del database durante l'esecuzione di uno unit test.

Visual Studio non include un framework di oggetto Mock.Visual Studio does not include a Mock Object framework. Tuttavia, sono disponibili diversi framework di oggetti Mock commerciali e open source per il framework .NET:

1. Moq - Questo framework è disponibile con la licenza BSD open source. È possibile scaricare [https://code.google.com/p/moq/](https://code.google.com/p/moq/)Moq da .
2. Rhino Mocks - Questo framework è disponibile con la licenza BSD open source. È possibile scaricare Rhino [http://ayende.com/projects/rhino-mocks.aspx](http://ayende.com/projects/rhino-mocks.aspx)Mocks da .
3. Typemock Isolator - Questo è un quadro commerciale. È possibile scaricare una [http://www.typemock.com/](http://www.typemock.com/)versione di prova da .

In questo tutorial, ho deciso di utilizzare Moq. Tuttavia, si potrebbe altrettanto facilmente utilizzare Rhino Mocks o Typemock Isolator per creare gli oggetti Mock per l'applicazione Contact Manager.

Prima di poter utilizzare Moq, è necessario completare i seguenti passaggi:

1. .
2. Prima di decomprimere il download, assicurarsi di fare clic con il pulsante destro del mouse sul file e fare clic sul pulsante con etichetta **Sblocca** (vedere Figura 1).
3. Decomprimere il download.
4. Aggiungere un riferimento all'assembly Moq al progetto di test selezionando l'opzione di menu **Progetto, Aggiungi riferimento** per aprire la finestra di dialogo **Aggiungi riferimento.** Nella scheda Sfoglia, individuare la cartella in cui è stato decompresso Moq e selezionare l'assembly Moq.dll. Fare clic sul pulsante **OK** (vedere la figura 2).

[![Sblocco Moq](iteration-5-create-unit-tests-vb/_static/image1.jpg)](iteration-5-create-unit-tests-vb/_static/image1.png)

**Figura 01**: Sblocco Moq(Fare[clic per visualizzare l'immagine a dimensioni intere)](iteration-5-create-unit-tests-vb/_static/image2.png)

[![Riferimenti dopo l'aggiunta di Moq](iteration-5-create-unit-tests-vb/_static/image2.jpg)](iteration-5-create-unit-tests-vb/_static/image3.png)

**Figura 02**: Riferimenti dopo l'aggiunta di Moq([Fare clic per visualizzare l'immagine a dimensioni intere](iteration-5-create-unit-tests-vb/_static/image4.png))

## <a name="creating-unit-tests-for-the-service-layer"></a>Creazione di unit test per il livello di servizioCreating Unit Tests for the Service Layer

Iniziamo creando un set di unit test per il livello di servizio dell'applicazione Contact Manager s. Utilizzeremo questi test per verificare la logica di convalida.

Creare una nuova cartella denominata Models nel progetto ContactManager.Tests.Create a new folder named Models in the ContactManager.Tests project. Successivamente, fare clic con il pulsante destro del mouse sulla cartella Modelli e selezionare **Aggiungi, Nuovo test**. Viene visualizzata la finestra di dialogo **Aggiungi nuovo test** illustrata nella Figura 3. Selezionare il modello **Di unit test** e assegnare al nuovo test il nome ContactManagerServiceTest.vb.Select the Unit Test template and name your new test ContactManagerServiceTest.vb. Fare clic sul pulsante **OK** per aggiungere il nuovo test al progetto di test.

> [!NOTE] 
> 
> In generale, si desidera che la struttura di cartelle del progetto di test corrisponda alla struttura di cartelle del progetto MVC ASP.NET. Ad esempio, si inserisceno i test del controller in una cartella Controllers, si modellano i test in una cartella Models e così via.

[![Modelli: ContactManagerServiceTest.cs](iteration-5-create-unit-tests-vb/_static/image3.jpg)](iteration-5-create-unit-tests-vb/_static/image5.png)

**Figura 03**: Modelli: ContactManagerServiceTest.cs([Fare clic per visualizzare l'immagine](iteration-5-create-unit-tests-vb/_static/image6.png)a dimensioni intere )

Inizialmente, si desidera testare il metodo CreateContact() esposto dalla classe ContactManagerService. Verranno creati i cinque test seguenti:We'll create the following five tests:

- CreateContact() - Verifica che CreateContact() restituisca il valore true quando un contatto valido viene passato al metodo.
- CreateContactRequiredFirstName()- Verifica che un messaggio di errore venga aggiunto allo stato del modello quando un contatto con un nome mancante viene passato al metodo CreateContact().
- CreateContactRequiredLastName() - Verifica che venga aggiunto un messaggio di errore allo stato del modello quando un contatto con un cognome mancante viene passato al metodo CreateContact().
- CreateContactInvalidPhone() - Verifica che un messaggio di errore venga aggiunto allo stato del modello quando un contatto con un numero di telefono non valido viene passato al metodo CreateContact().
- CreateContactInvalidEmail() - Verifica che un messaggio di errore venga aggiunto allo stato del modello quando un contatto con un indirizzo e-mail non valido viene passato al metodo CreateContact().

Il primo test verifica che un contatto valido non generi un errore di convalida. I test rimanenti controllano ciascuna delle regole di convalida.

Il codice per questi test è contenuto nel listato 1.The code for these tests is contained in Listing 1.

**Listato 1 - Modelli, ContactManagerServiceTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample1.vb)]

Poiché si usa la classe Contact nel listato 1, è necessario aggiungere un riferimento a Microsoft Entity Framework al progetto di test. Aggiungere un riferimento all'assembly System.Data.Entity.

Il listato 1 contiene un metodo denominato Initialize() decorato con l'attributo [TestInitialize]. Questo metodo viene chiamato automaticamente prima dell'esecuzione di ciascuno degli unit test (viene chiamato 5 volte a destra prima di ciascuno degli unit test). Il metodo Initialize() crea un repository fittizio con la seguente riga di codice:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample2.vb)]

Questa riga di codice utilizza il framework Moq per generare un repository fittizio dall'interfaccia IContactManagerRepository.This line of code uses the Moq framework to generate a mock repository from the IContactManagerRepository interface. Il repository fittizio viene utilizzato al posto dell'effettivo EntityContactManagerRepository per evitare di accedere al database quando viene eseguito ogni unit test. Il repository fittizio implementa i metodi dell'interfaccia IContactManagerRepository, ma i metodi non eseguono effettivamente alcuna operazione.

> [!NOTE] 
> 
> Quando si usa il framework Moq, esiste una distinzione tra \_mockRepository e \_mockRepository.Object. Il primo fa riferimento alla classe Mock(Of IContactManagerRepository) che contiene metodi per specificare il modo in cui si comporterà il repository fittizio. Quest'ultimo si riferisce al repository fittizio effettivo che implementa l'interfaccia IContactManagerRepository.

Il repository fittizio viene utilizzato nel metodo Initialize() durante la creazione di un'istanza della classe ContactManagerService. Tutti i singoli unit test utilizzano questa istanza della classe ContactManagerService.

Il listato 1 contiene cinque metodi che corrispondono a ciascuno degli unit test. Ognuno di questi metodi è decorato con l'attributo [TestMethod]. Quando si eseguono gli unit test, viene chiamato qualsiasi metodo con questo attributo. In altre parole, qualsiasi metodo decorato con l'attributo [TestMethod] è uno unit test.

Il primo unit test, denominato CreateContact(), verifica che la chiamata a CreateContact() restituisca il valore true quando al metodo viene passata un'istanza valida della classe Contact. Il test crea un'istanza della classe Contact, chiama il metodo CreateContact() e verifica che CreateContact() restituisca il valore true.

I test rimanenti verificano che quando il metodo CreateContact() viene chiamato con un Contact non valido, il metodo restituisce false e il messaggio di errore di convalida previsto viene aggiunto allo stato del modello. Ad esempio, il test CreateContactRequiredFirstName() crea un'istanza della classe Contact con una stringa vuota per la relativa proprietà FirstName. Successivamente, il metodo CreateContact() viene chiamato con l'oggetto Contact non valido. Infine, il test verifica che CreateContact() restituisca false e che lo stato del modello contenga il messaggio di errore di convalida previsto "Nome obbligatorio".

È possibile eseguire gli unit test nel listato 1 selezionando l'opzione di menu **Test, Esegui, Tutti i test nella soluzione (CTRL.R, A)**. I risultati dei test vengono visualizzati nella finestra Risultati test (vedere la Figura 4).

[![Risultati del test](iteration-5-create-unit-tests-vb/_static/image4.jpg)](iteration-5-create-unit-tests-vb/_static/image7.png)

**Figura 04**: Risultati del test ([fare clic per visualizzare l'immagine a dimensioni intere](iteration-5-create-unit-tests-vb/_static/image8.png))

## <a name="creating-unit-tests-for-controllers"></a>Creazione di unit test per i controllerCreating Unit Tests for Controllers

ASP.NET'applicazione MVC controlla il flusso di interazione dell'utente. Durante il test di un controller, si desidera verificare se il controller restituisce il risultato dell'azione corretta e visualizzare i dati. È anche possibile verificare se un controller interagisce con le classi del modello nel modo previsto.

Ad esempio, il listato 2 contiene due unit test per il metodo Create() del controller di contatto. Il primo unit test verifica che quando un contatto valido viene passato al metodo Create(), il metodo Create() reindirizza all'azione Indice. In altre parole, quando viene passato un contatto valido, il metodo Create() deve restituire un valore RedirectToRouteResult che rappresenta l'azione Index.

Non vogliamo testare il livello di servizio ContactManager quando si testa il livello di controller. Pertanto, il livello di servizio viene simulato con il codice seguente nel metodo Initialize:Therefore, we mock the service layer with the following code in the Initialize method:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample3.vb)]

Nello unit test CreateValidContact(), viene simulato il comportamento della chiamata al metodo CreateContact() del livello di servizio con la seguente riga di codice:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample4.vb)]

Questa riga di codice fa in modo che il servizio ContactManager fittizio restituisca il valore true quando viene chiamato il relativo metodo CreateContact(). Prendendo in giro il livello di servizio, è possibile testare il comportamento del controller senza dover eseguire codice nel livello di servizio.

Il secondo unit test verifica che l'azione Create() restituisca la visualizzazione Create quando un contatto non valido viene passato al metodo. Facciamo in modo che il metodo CreateContact() del livello di servizio restituisca il valore false con la seguente riga di codice:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample5.vb)]

Se il metodo Create() si comporta come previsto, deve restituire la vista Create quando il livello di servizio restituisce il valore false. In questo modo, il controller può visualizzare i messaggi di errore di convalida nella visualizzazione Crea e l'utente ha la possibilità di correggere le proprietà Contact non valide.

Se si prevede di compilare unit test per i controller, è necessario restituire nomi di visualizzazione espliciti dalle azioni del controller. Ad esempio, non restituire una visualizzazione simile alla seguente:For example, do not return a view like this:

Ritorno di View()

Al contrario, restituire la visualizzazione in questo modo:Instead, return the view like this:

Visualizzazione di ritorno("Crea")

Se non si è espliciti quando si restituisce una visualizzazione, il ViewResult.ViewName proprietà restituisce una stringa vuota.

**Listato 2 - Controller-ContactControllerTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample6.vb)]

## <a name="summary"></a>Riepilogo

In questa iterazione sono stati creati unit test per l'applicazione Contact Manager. È possibile eseguire questi unit test in qualsiasi momento per verificare che l'applicazione si comporti ancora nel modo previsto. Gli unit test fungono da rete di sicurezza per la nostra applicazione che ci consente di modificare in modo sicuro la nostra applicazione in futuro.

Sono stati creati due set di unit test. In primo luogo, abbiamo testato la nostra logica di convalida creando unit test per il livello di servizio. Successivamente, abbiamo testato la logica di controllo del flusso creando unit test per il livello di controller. Durante il test del livello di servizio, abbiamo isolato i test per il livello di servizio dal livello del repository prendendo in giro il livello del repository. Durante il test del livello di controller, abbiamo isolato i test per il livello di controller simulando il livello di servizio.

Nell'iterazione successiva, viene modificato l'applicazione Contact Manager in modo che supporti i gruppi di contatti. Aggiungeremo questa nuova funzionalità alla nostra applicazione utilizzando un processo di progettazione software chiamato sviluppo basato su test.

> [!div class="step-by-step"]
> [Successivo](iteration-4-make-the-application-loosely-coupled-vb.md)
> [precedente](iteration-6-use-test-driven-development-vb.md)
