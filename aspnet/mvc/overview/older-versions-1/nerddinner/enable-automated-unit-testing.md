---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Abilitazione di unit test automatizzati Documenti Microsoft
author: rick-anderson
description: Passaggio 12 viene illustrato come sviluppare una suite di unit test automatizzati che verificano la funzionalità di NerdDinner e che ci darà la sicurezza di apportare modifiche...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 7fe84efd9e4cc359c19d5ab9e22c579b80207e9c
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541468"
---
# <a name="enable-automated-unit-testing"></a>Abilitare unit test automatici

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 12 di [un'esercitazione gratuita sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come compilare un'applicazione Web piccola ma completa utilizzando ASP.NET MVC 1.
> 
> Passaggio 12 viene illustrato come sviluppare una suite di unit test automatizzati che verificano la funzionalità di NerdDinner e che ci darà la sicurezza di apportare modifiche e miglioramenti all'applicazione in futuro.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [ALL'archivio musicale MVC.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-12-unit-testing"></a>Passaggio 12 di NerdDinner: Unit Test

Sviluppiamo una suite di unit test automatizzati che verificano la nostra funzionalità NerdDinner e che ci daranno la sicurezza di apportare modifiche e miglioramenti all'applicazione in futuro.

### <a name="why-unit-test"></a>Perché Unit Test?

Una mattina, durante l'ingresso nel processo, una mattina avrai un improvviso lampo di ispirazione su un'applicazione su cui stai lavorando. Ti rendi conto che c'è una modifica che puoi implementare che renderà l'applicazione notevolmente migliore. Potrebbe essere un refactoring che pulisce il codice, aggiunge una nuova funzionalità o corregge un bug.

La domanda che ti si pone quando si arriva al computer è – "quanto è sicuro per fare questo miglioramento?" Cosa succede se fare il cambiamento ha effetti collaterali o rompe qualcosa? La modifica potrebbe essere semplice e richiedere solo pochi minuti per l'implementazione, ma cosa succede se sono necessarie ore per testare manualmente tutti gli scenari dell'applicazione? Cosa succede se si dimentica di coprire uno scenario e un'applicazione interrotta entra in produzione? Fare questo miglioramento vale davvero tutto lo sforzo?

Gli unit test automatizzati possono fornire una rete di sicurezza che consente di migliorare continuamente le applicazioni ed evitare di avere paura del codice su cui si sta lavorando. Avere test automatizzati che verificano rapidamente la funzionalità consente di codificare con sicurezza e consentono di apportare miglioramenti che altrimenti non si sarebbe sentito a proprio agio. Contribuiscono inoltre a creare soluzioni più gestibili e che hanno una durata maggiore, il che porta a un ritorno sull'investimento molto più elevato.

Il framework di MVC ASP.NET semplifica e naturale la funzionalità dell'applicazione di unit test. Abilita inoltre un flusso di lavoro TDD (Test Driven Development) che consente lo sviluppo basato su test.

### <a name="nerddinnertests-project"></a>Progetto NerdDinner.Tests

Quando abbiamo creato l'applicazione NerdDinner all'inizio di questa esercitazione, ci è stata richiesta una finestra di dialogo che chiede se si desidera creare un progetto di unit test per andare avanti con il progetto di applicazione:When we created our NerdDinner application at the beginning of this tutorial, we were prompted with a dialog asking whether we wanted to create a unit test project to go with the application project:

![](enable-automated-unit-testing/_static/image1.png)

Abbiamo mantenuto selezionato il pulsante di opzione "Sì, crea un progetto di unit test", che ha comportato l'aggiunta di un progetto "NerdDinner.Tests" alla soluzione:

![](enable-automated-unit-testing/_static/image2.png)

Il progetto NerdDinner.Tests fa riferimento all'assembly del progetto dell'applicazione NerdDinner e consente di aggiungervi facilmente test automatizzati che verificano la funzionalità dell'applicazione.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Creazione di unit test per la classe di modello DinnerCreating Unit Tests for our Dinner Model Class

Aggiungiamo alcuni test al nostro progetto NerdDinner.Tests che verificano la classe Dinner che abbiamo creato quando abbiamo creato il nostro livello di modello.

Inizieremo creando una nuova cartella all'interno del nostro progetto di test chiamata "Modelli" in cui inseriremo i nostri test relativi al modello. Faremo quindi clic con il pulsante destro del mouse sulla cartella e sceglieremo il comando di menu **Aggiungi-&gt;Nuovo test.** Verrà eseguito la finestra di dialogo "Aggiungi nuovo test".

Sceglieremo di creare uno "Unit Test" e denominarlo "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Quando si fa clic sul pulsante "ok" Visual Studio aggiungerà (e aprire) un file di DinnerTest.cs al progetto:

![](enable-automated-unit-testing/_static/image4.png)

Il modello di unit test predefinito di Visual Studio ha un gruppo di codice boiler-plate all'interno di esso che trovo un po 'disordinato. Puliamolo per contenere solo il codice qui sotto:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

L'attributo [TestClass] nella classe DinnerTest precedente lo identifica come una classe che conterrà i test, nonché il codice facoltativo di inizializzazione e teardown dei test. È possibile definire i test al suo interno aggiungendo metodi pubblici che dispongono di un [TestMethod] attributo su di essi.

Di seguito sono riportati i primi di due test che aggiungeremo che esercitano la nostra lezione Dinner. Il primo test verifica che la cena non sia valida se viene creata una nuova cena senza che tutte le proprietà siano impostate correttamente. Il secondo test verifica che la cena sia valida quando una cena ha tutte le proprietà impostate con valori validi:The second test verifies that our Dinner is valid when a Dinner has all properties set with valid values:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Noterete sopra che i nostri nomi di test sono molto espliciti (e un po 'dettagliato). Stiamo facendo questo perché potremmo finire per creare centinaia o migliaia di piccoli test, e vogliamo rendere più facile determinare rapidamente l'intento e il comportamento di ciascuno di essi (soprattutto quando stiamo guardando attraverso un elenco di errori in un test runner). I nomi dei test devono essere denominati in base alla funzionalità che stanno testando. Sopra stiamo usando un\_modello\_di denominazione "Noun Should Verb".

Stiamo strutturando i test utilizzando il modello di test "AAA" – che sta per "Arrange, Act, Assert":

- Organizzare: impostare l'unità sottoposta a test
- Atto: Esercitare l'unità sotto test e catturare i risultati
- Asserzione: verificare il comportamentoAssert: Verify the behavior

Quando scriviamo test vogliamo evitare che i singoli test facciano troppo. Ogni test deve invece verificare un solo concetto (che renderà molto più semplice individuare la causa degli errori). Una buona linea guida consiste nel provare e avere una sola istruzione assert per ogni test. Se si dispone di più di un'istruzione assert in un metodo di test, assicurarsi che vengano tutti utilizzati per testare lo stesso concetto. In caso di dubbio, fare un altro test.

### <a name="running-tests"></a>Esecuzione test

Visual Studio 2008 Professional (e versioni successive) include un test runner incorporato che può essere utilizzato per eseguire progetti di unit test di Visual Studio all'interno dell'IDE. È possibile selezionare il comando di menu **Test-&gt;Esegui-&gt;Tutti i test nella soluzione** (o digitare Ctrl R, A) per eseguire tutti gli unit test. Oppure, in alternativa, è possibile posizionare il cursore all'interno di una classe di test o di un metodo di test specifico e utilizzare il comando di menu **Test-Esecuzione-Test&gt;&gt;nel contesto corrente** (o digitare Ctrl R, T) per eseguire un sottoinsieme degli unit test.

Posizionare il cursore all'interno della classe DinnerTest e digitare "Ctrl R, T" per eseguire i due test appena definiti. Quando si esegue questa operazione verrà visualizzata una finestra "Risultati test" all'interno di Visual Studio e verranno visualizzati i risultati dell'esecuzione dei test elencati al suo interno:When we do this a "Test Results" window will appear within Visual Studio and we'll see the results of our test run listed within it:

![](enable-automated-unit-testing/_static/image5.png)

*Nota: La finestra dei risultati del test VS non mostra la colonna Nome classe per impostazione predefinita. È possibile aggiungerlo facendo clic con il pulsante destro del mouse all'interno della finestra Risultati test e utilizzando il comando di menu Aggiungi/Rimuovi colonne.*

I nostri due test hanno richiesto solo una frazione di secondo per essere eseguiti – e come potete vedere entrambi sono passati. Ora possiamo continuare e aumentarli creando test aggiuntivi che verificano le convalide delle regole specifiche, oltre a coprire i due metodi helper, IsUserHost() e IsUserRegistered(), che abbiamo aggiunto alla classe Dinner. Avere tutti questi test in atto per la classe Dinner renderà molto più facile e sicuro aggiungere nuove regole di business e convalide in futuro. È possibile aggiungere la nuova logica della regola a Dinner e quindi in pochi secondi verificare che non abbia interrotto alcuna delle funzionalità logiche precedenti.

Si noti come l'utilizzo di un nome di test descrittivo semplifica la comprensione rapida di ciò che ogni test sta verificando. È consigliabile utilizzare il comando di menu&gt; **Strumenti-Opzioni,&gt;** aprire la schermata di configurazione Strumenti di test- Esecuzione test e selezionare la casella di controllo "Fare doppio clic su un risultato dello unit test non riuscito o non conclusivo visualizza il punto di errore nel test". In questo modo sarà possibile fare doppio clic su un errore nella finestra dei risultati del test e passare immediatamente all'errore di asserzione.

### <a name="creating-dinnerscontroller-unit-tests"></a>Creazione di unit test DinnersControllerCreating DinnersController Unit Tests

Creiamo ora alcuni unit test che verificano la funzionalità DinnersController.Let's now create some unit tests that verify our DinnersController functionality. Inizieremo facendo clic con il pulsante destro del mouse sulla cartella "Controllers" all'interno del nostro progetto di test e quindi scegliere il **Aggiungi-&gt;Nuovo test** comando di menu. Creeremo uno "Unit Test" e lo chiameremo "DinnersControllerTest.cs".

Creeremo due metodi di test che verificano il metodo di azione Details() su DinnersController. Il primo verificherà che venga restituito un oggetto View quando viene richiesta una cena esistente. Il secondo verificherà che venga restituita una visualizzazione "NotFound" quando viene richiesta una cena inesistente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

Il codice precedente viene compilato pulito. Quando eseguiamo i test, tuttavia, entrambi hanno esito negativo:When we run the tests, though, both fail:

![](enable-automated-unit-testing/_static/image6.png)

Se osserviamo i messaggi di errore, vedremo che il motivo per cui i test non sono riusciti è perché la nostra classe DinnersRepository non è stata in grado di connettersi a un database. L'applicazione NerdDinner utilizza una stringa di connessione a un file\_SQL Server Express locale che si trova nella directory dati app del progetto di applicazione NerdDinner. Poiché il progetto NerdDinner.Tests viene compilato ed eseguito in una directory diversa rispetto al progetto dell'applicazione, il percorso relativo della stringa di connessione non è corretto.

È *possibile* risolvere questo problema copiando il file di database SQL Express nel progetto di test e quindi aggiungervi una stringa di connessione di test appropriata nel file App.config del progetto di test. Questo otterrebbe i test di cui sopra sbloccato e in esecuzione.

Il codice di unit test che utilizza un database reale, tuttavia, comporta una serie di sfide. In particolare:

- Rallenta in modo significativo il tempo di esecuzione degli unit test. Maggiore è il tempo necessario per eseguire i test, minore è la probabilità di eseguirli frequentemente. Idealmente si desidera che gli unit test siano in grado di essere eseguiti in pochi secondi e che sia qualcosa che si fa naturalmente come la compilazione del progetto.
- Complica la logica di impostazione e pulizia all'interno dei test. Si desidera che ogni unit test sia isolato e indipendente dagli altri (senza effetti collaterali o dipendenze). Quando si lavora su un database reale è necessario essere consapevoli dello stato e reimpostarlo tra i test.

Diamo un'occhiata a un modello di progettazione chiamato "iniezione di dipendenza" che può aiutarci a risolvere questi problemi ed evitare la necessità di utilizzare un database reale con i nostri test.

### <a name="dependency-injection"></a>Inserimento di dipendenze

In questo momento DinnersController è strettamente "accoppiato" per la classe DinnerRepository. "Accoppiamento" si riferisce a una situazione in cui una classe si basa in modo esplicito su un'altra classe per funzionare:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Poiché la classe DinnerRepository richiede l'accesso a un database, la dipendenza strettamente accoppiata della classe DinnersController sul DinnerRepository finisce per richiedere di avere un database per i metodi di azione DinnersController da testare.

È possibile aggirare questo problema utilizzando un modello di progettazione denominato "iniezione di dipendenza", ovvero un approccio in cui le dipendenze (ad esempio le classi di repository che forniscono l'accesso ai dati) non vengono più create in modo implicito all'interno di classi che le utilizzano. Al contrario, le dipendenze possono essere passate in modo esplicito alla classe che le utilizza utilizzando gli argomenti del costruttore. Se le dipendenze vengono definite tramite interfacce, abbiamo la flessibilità di passare implementazioni di dipendenza "falsi" per gli scenari di unit test. In questo modo è possibile creare implementazioni di dipendenze specifiche del test che non richiedono effettivamente l'accesso a un database.

Per vedere questo in azione, è possibile implementare inserimento delle dipendenze con il nostro DinnersController.To see this in action, let's implement dependency injection with our DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Estrazione di un'interfaccia IDinnerRepositoryExtracting an IDinnerRepository interface

Our first step will be to create a new IDinnerRepository interface that encapsulates the repository contract our controllers require to retrieve and update Dinners.

È possibile definire manualmente questo contratto di interfaccia facendo clic con il pulsante destro del mouse sulla cartella Modelli, quindi scegliendo il comando di menu **Aggiungi-&gt;Nuovo elemento** e creando una nuova interfaccia denominata IDinnerRepository.cs.

In alternativa è possibile utilizzare gli strumenti di refactoring incorporati in Visual Studio Professional (e versioni successive) per estrarre e creare automaticamente un'interfaccia dalla classe DinnerRepository esistente. Per estrarre questa interfaccia tramite VS, è sufficiente posizionare il cursore nell'editor di testo sulla classe DinnerRepository, quindi fare clic con il pulsante destro del mouse e scegliere il comando di menu **Refactor-&gt;Extract Interface:**

![](enable-automated-unit-testing/_static/image7.png)

Verrà visualizzata la finestra di dialogo "Estrai interfaccia" e verrà richiesto il nome dell'interfaccia da creare. Per impostazione predefinita verrà impostato su IDinnerRepository e verrà selezionato automaticamente tutti i metodi pubblici nella classe DinnerRepository esistente da aggiungere all'interfaccia:

![](enable-automated-unit-testing/_static/image8.png)

Quando si fa clic sul pulsante "ok", Visual Studio aggiungerà una nuova interfaccia IDinnerRepository all'applicazione:When we click the "ok" button, Visual Studio will add a new IDinnerRepository interface to our application:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

E la nostra classe DinnerRepository esistente verrà aggiornata in modo da implementa l'interfaccia:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Aggiornamento di DinnersController per supportare l'inserimento del costruttoreUpdating DinnersController to support constructor injection

Verrà ora aggiornata la classe DinnersController per usare la nuova interfaccia.

Attualmente DinnersController è hardcoded in modo che il suo campo "dinnerRepository" è sempre una classe DinnerRepository:Currently DinnersController is hardcoded such that its "dinnerRepository" field is always a DinnerRepository class:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Lo modificheremo in modo che il campo "dinnerRepository" sia di tipo IDinnerRepository invece di DinnerRepository. Aggiungeremo quindi due costruttori DinnersController pubblici. Uno dei costruttori consente un IDinnerRepository da passare come argomento. L'altro è un costruttore predefinito che usa l'implementazione DinnerRepository esistente:The other is a default constructor that uses our existing DinnerRepository implementation:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Poiché ASP.NET MVC per impostazione predefinita crea classi controller utilizzando i costruttori predefiniti, il nostro DinnersController in fase di esecuzione continuerà a utilizzare il DinnerRepository classe per eseguire l'accesso ai dati.

È ora possibile aggiornare gli unit test, tuttavia, per passare un'implementazione del repository cena "falso" utilizzando il costruttore di parametro. Questo "falso" repository cena non richiederà l'accesso a un database reale, e invece utilizzerà dati di esempio in memoria.

#### <a name="creating-the-fakedinnerrepository-class"></a>Creazione della classe FakeDinnerRepository

Creiamo una classe FakeDinnerRepository.Let's create a FakeDinnerRepository class.

Inizieremo creando una directory "Fakes" all'interno del nostro progetto NerdDinner.Tests e quindi aggiungere una nuova classe FakeDinnerRepository (fare clic con il pulsante destro del mouse sulla cartella e scegliere **Aggiungi-&gt;Nuova classe**):

![](enable-automated-unit-testing/_static/image9.png)

Aggiorneremo il codice in modo che la classe FakeDinnerRepository implementi l'interfaccia IDinnerRepository.We'll update the code so that the FakeDinnerRepository class implements the IDinnerRepository interface. Possiamo quindi fare clic destro su di esso e scegliere il comando del menu di scelta rapida "Implementa interfaccia IDinnerRepository":

![](enable-automated-unit-testing/_static/image10.png)

In questo modo Visual Studio aggiungerà automaticamente tutti i membri dell'interfaccia IDinnerRepository alla classe FakeDinnerRepository con implementazioni "stub out" predefinite:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

È quindi possibile aggiornare l'implementazione di FakeDinnerRepository per lavorare fuori di una raccolta List Dinner in memoria passata come argomento del costruttore:We can then update the FakeDinnerRepository implementation to work off of an in-memory List&lt;Dinner&gt; collection passed ed it as a constructor argument:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Ora abbiamo un'implementazione fittizia di IDinnerRepository che non richiede un database e può invece lavorare fuori un elenco in memoria di Dinner oggetti.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Utilizzo di FakeDinnerRepository con unit testUsing the FakeDinnerRepository with Unit Tests

Torniamo agli unit test DinnersController non riusciti in precedenza perché il database non era disponibile. È possibile aggiornare i metodi di test per utilizzare un FakeDinnerRepository popolato con dati Dinner in memoria di esempio per il DinnersController utilizzando il codice seguente:We can update the test methods to use a FakeDinnerRepository populated with sample in-memory Dinner data to the DinnersController using the code below:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

E ora quando eseguiamo questi test passano entrambi:

![](enable-automated-unit-testing/_static/image11.png)

Meglio di tutti, prendono solo una frazione di secondo per l'esecuzione, e non richiedono alcuna logica di installazione / pulizia complicata. È ora possibile unit test di tutto il codice del metodo di azione DinnersController (inclusi elenco, paging, dettagli, creazione, aggiornamento ed eliminazione) senza mai dover connettersi a un database reale.

| **Argomento collaterale: Framework di inserimento delle dipendenze** |
| --- |
| L'esecuzione dell'inserimento manuale delle dipendenze (come sopra) funziona correttamente, ma diventa più difficile da gestire con l'aumentare del numero di dipendenze e componenti in un'applicazione. Esistono diversi framework di inserimento delle dipendenze per .NET che consentono di fornire una flessibilità ancora maggiore per la gestione delle dipendenze. Questi framework, anche a volte chiamati contenitori "Inversione del controllo" (IoC), forniscono meccanismi che consentono un ulteriore livello di supporto della configurazione per specificare e passare le dipendenze agli oggetti in fase di esecuzione (il più delle volte utilizzando l'inserimento del costruttore). Alcuni dei più popolari Framework OSS Dependency Injection / IOC in .NET includono: AutoFac, Ninject, Spring.NET, StructureMap, e Windsor. ASP.NET MVC espone le API di estendibilità che consentono agli sviluppatori di partecipare alla risoluzione e creazione di istanze dei controller e che consente l'inserimento di dipendenze / Framework IoC per essere integrati in modo pulito all'interno di questo processo. Utilizzando un framework DI/IOC ci consentirebbe anche di rimuovere il costruttore predefinito dal nostro DinnersController – che rimuoverebbe completamente l'accoppiamento tra esso e il DinnerRepository. Non verrà utilizzato un'iniezione di dipendenza / framework IOC con l'applicazione NerdDinner.We won't be using a dependency injection / IOC framework with our NerdDinner application. Ma è qualcosa che potremmo considerare per il futuro se il NerdDinner code-base e le capacità è cresciuto. |

### <a name="creating-edit-action-unit-tests"></a>Creazione di nuovi unit test di azione

Creiamo ora alcuni unit test che verificano la funzionalità di modifica di DinnersController. Inizieremo testando la versione HTTP-GET dell'azione Modifica:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Verrà creato un test che verifica che un oggetto View supportato da un DinnerFormViewModel oggetto viene eseguito il rendering quando viene richiesta una cena valida:We'll create a test that verifies that a View backed by a DinnerFormViewModel object is rendered back when a valid dinner is requested:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Quando si esegue il test, tuttavia, si noterà che ha esito negativo perché un'eccezione di riferimento null viene generata quando il Edit metodo accede alla proprietà User.Identity.Name per eseguire il controllo Dinner.IsHostedBy().

L'oggetto utente nella classe di base Controller incapsula i dettagli sull'utente connesso e viene popolato da ASP.NET MVC quando crea il controller in fase di esecuzione. Poiché stiamo testando il DinnersController all'esterno di un ambiente web-server, il User oggetto non è impostato (da qui l'eccezione di riferimento null).

### <a name="mocking-the-useridentityname-property"></a>Mocking la proprietà User.Identity.Name

Framework di simulazione semplificano i test consentendoci di creare dinamicamente versioni false di oggetti dipendenti che supportano i test. Ad esempio, è possibile usare un framework di simulazione nel test dell'azione Edit per creare dinamicamente un oggetto User che il nostro DinnersController può usare per cercare un nome utente simulato. In questo modo si eviterà la creazione di un riferimento null quando si esegue il test.

Esistono molti framework di simulazione di .NET che possono essere utilizzati con [http://www.mockframeworks.com/](http://www.mockframeworks.com/)ASP.NET MVC (è possibile visualizzare un elenco di essi qui: ). Per testare la nostra applicazione NerdDinner useremo un framework di simulazione open source [http://www.mockframeworks.com/moq](http://www.mockframeworks.com/moq)chiamato "Moq", che può essere scaricato gratuitamente da .

Una volta scaricato, aggiungeremo un riferimento nel nostro progetto NerdDinner.Tests all'assembly Moq.dll:

![](enable-automated-unit-testing/_static/image12.png)

Verrà quindi aggiunto un metodo helper "CreateDinnersControllerAs(username)" alla classe di test che accetta un nome utente come parametro e che quindi "mocks" la proprietà User.Identity.Name nell'istanza DinnersController:We'll then add a "CreateDinnersControllerAs(username)" helper method to our test class that takes a username as a parameter, and then "mocks" the User.Identity.Name property on the DinnersController instance:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Sopra stiamo usando Moq per creare un oggetto Mock che finge un oggetto ControllerContext (che è ciò che ASP.NET MVC passa alle classi Controller per esporre oggetti di runtime come User, Request, Response e Session). Viene chiamato il metodo "SetupGet" su Mock per indicare che la proprietà HttpContext.User.Identity.Name in ControllerContext deve restituire la stringa del nome utente passata al metodo helper.

È possibile simulare qualsiasi numero di controllerDi proprietà e metodi. Per illustrare questo ho anche aggiunto una chiamata SetupGet() per la proprietà Request.IsAuthenticated (che non è effettivamente necessaria per i test riportati di seguito, ma che consente di illustrare come è possibile simulare le proprietà Request). Quando abbiamo finito assegniamo un'istanza di ControllerContext finto per il DinnersController il nostro metodo helper restituisce.

È ora possibile scrivere unit test che utilizzano questo metodo helper per testare gli scenari di modifica che coinvolgono utenti diversi:We can now write unit tests that use this helper method to test Edit scenarios involving different users:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

E ora quando eseguiamo i test che superano:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>Test degli scenari UpdateModel()

Sono stati creati test relativi alla versione HTTP-GET dell'azione Modifica. Creiamo ora alcuni test che verificano la versione HTTP-POST dell'azione Modifica:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

Il nuovo interessante scenario di test che ci permette di supportare con questo metodo di azione è l'utilizzo del metodo helper UpdateModel() nella classe di base Controller. Questo metodo helper viene usato per associare i valori post del form all'istanza dell'oggetto Dinner.We are using this helper method to bind form-post values to our Dinner object instance.

Di seguito sono riportati due test che illustrano come è possibile fornire i valori inviati dal modulo per il metodo helper UpdateModel() da utilizzare. A tale scopo, creare e popolare un oggetto FormCollection e quindi assegnarlo alla proprietà "ValueProvider" nel Controller.

Il primo test verifica che al salvataggio riuscito il browser venga reindirizzato all'azione dei dettagli. Il secondo test verifica che quando viene inviato un input non valido l'azione visualizza nuovamente la visualizzazione di modifica con un messaggio di errore.

[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Test del wrapping

Sono stati illustrati i concetti di base coinvolti nelle classi di controller di unit test. Possiamo usare queste tecniche per creare facilmente centinaia di semplici test che verificano il comportamento della nostra applicazione.

Poiché i test del controller e del modello non richiedono un database reale, sono estremamente veloci e facili da eseguire. Saremo in grado di eseguire centinaia di test automatizzati in pochi secondi e ricevere immediatamente un feedback sul fatto che una modifica che abbiamo apportato abbia rotto qualcosa. Questo ci aiuterà a fornire la fiducia necessaria per migliorare continuamente, refactoring e perfezionare l'applicazione.

Abbiamo trattato il test come ultimo argomento di questo capitolo, ma non perché il test è qualcosa che dovresti fare alla fine di un processo di sviluppo! Al contrario, è consigliabile scrivere test automatizzati il prima possibile nel processo di sviluppo. In questo modo è possibile ottenere un feedback immediato durante lo sviluppo, è possibile pensare in modo ponderato agli scenari dei casi d'uso dell'applicazione e guida l'utente nella progettazione dell'applicazione tenendo presente la creazione di livelli e l'accoppiamento puliti.

Un capitolo successivo del libro verrà illustrato Test Driven Development (TDD) e come utilizzarlo con ASP.NET MVC. TDD è una procedura di codifica iterativa in cui si scrivono prima i test che il codice risultante soddisferà. Con TDD si inizia ogni funzionalità creando un test che verifica la funzionalità che si sta per implementare. Scrivere prima lo unit test aiuta a capire chiaramente la funzionalità e il suo funzionamento. Solo dopo la scrittura del test (e aver verificato che non riesce) si implementano le funzionalità effettive verificate dal test. Poiché hai già dedicato del tempo a pensare al caso d'uso di come dovrebbe funzionare la funzionalità, avrai una migliore comprensione dei requisiti e del modo migliore per implementarli. Al termine dell'implementazione è possibile rieseguire il test e ottenere un feedback immediato sul corretto funzionamento della funzionalità. Parleremo di più TDD nel capitolo 10.

### <a name="next-step"></a>passaggio successivo

Alcuni commenti finale wrap up.

> [!div class="step-by-step"]
> [Successivo](use-ajax-to-implement-mapping-scenarios.md)
> [precedente](nerddinner-wrap-up.md)
