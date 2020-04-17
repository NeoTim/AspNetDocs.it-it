---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
title: 'Iterazione #6 – Utilizzare lo sviluppo basato su test (VB) Documenti Microsoft'
author: rick-anderson
description: In questa sesta iterazione, aggiungiamo nuove funzionalità all'applicazione scrivendo prima gli unit test e scrivendo il codice sugli unit test. In questa iterazione,...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: e1fd226f-3f8e-4575-a179-5c75b240333d
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
msc.type: authoredcontent
ms.openlocfilehash: 8464f4cdee673ef75d561e4cf89613fdca2c16af
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542807"
---
# <a name="iteration-6--use-test-driven-development-vb"></a>Iterazione 6 - Usare lo sviluppo basato su test (VB)

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il codice](iteration-6-use-test-driven-development-vb/_static/contactmanager_6_vb1.zip)

> In questa sesta iterazione, aggiungiamo nuove funzionalità all'applicazione scrivendo prima gli unit test e scrivendo il codice sugli unit test. In questa iterazione vengono aggiunti gruppi di contatti.

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

Nell'iterazione precedente dell'applicazione Contact Manager, sono stati creati unit test per fornire una rete di sicurezza per il codice. La motivazione per la creazione degli unit test è stata quella di rendere il codice più resiliente alle modifiche. Con gli unit test in atto, possiamo tranquillamente apportare qualsiasi modifica al nostro codice e sapere immediatamente se abbiamo rotto la funzionalità esistente.

In questa iterazione vengono utilizzati unit test per uno scopo completamente diverso. In questa iterazione vengono utilizzati gli unit test come parte di una filosofia di progettazione dell'applicazione denominata *sviluppo basato su test*. Quando si pratica lo sviluppo basato su test, si scrivono prima i test e quindi il codice sui test.

Più precisamente, quando si pratica lo sviluppo basato su test, sono disponibili tre passaggi da completare durante la creazione del codice (rosso/verde/refactoring):

1. Scrivere uno unit test che ha esito negativo (rosso)Write a unit test that fails (Red)
2. Scrivere codice che supera lo unit test (verde)
3. Effettua refactoring del codice (Refactoring)Refactoring your code (Refactor)

In primo luogo, si scrive lo unit test. Lo unit test deve esprimere l'intenzione per il modo in cui si prevede che il codice si comporti. Quando si crea lo unit test per la prima volta, lo unit test dovrebbe avere esito negativo. Il test dovrebbe avere esito negativo perché non è ancora stato scritto alcun codice dell'applicazione che soddisfi il test.

Successivamente, si scrive codice sufficiente per consentire il superato dello unit test. L'obiettivo è quello di scrivere il codice nel modo più pigro, più sciatto e più veloce possibile. È consigliabile non perdere tempo a pensare all'architettura dell'applicazione. È invece consigliabile concentrarsi sulla scrittura della quantità minima di codice necessaria per soddisfare l'intenzione espressa dallo unit test.

Infine, dopo aver scritto codice sufficiente, è possibile tornare indietro e considerare l'architettura complessiva dell'applicazione. In questo passaggio si riscrive (refactoring) il codice sfruttando i modelli di progettazione software, ad esempio il modello di repository, in modo che il codice sia più gestibile. È possibile riscrivere senza paura il codice in questo passaggio perché il codice è coperto da unit test.

Ci sono molti vantaggi che derivano dalla pratica dello sviluppo basato su test. In primo luogo, lo sviluppo basato su test impone di concentrarsi sul codice che deve essere effettivamente scritto. Poiché si è costantemente concentrati sulla sola scrittura di codice sufficiente per superare un particolare test, è possibile evitare di vagare nelle erbacce e scrivere enormi quantità di codice che non si utilizzerà mai.

In secondo luogo, una metodologia di progettazione "test first" impone di scrivere codice dal punto di vista di come verrà utilizzato il codice. In altre parole, quando si pratica lo sviluppo basato su test, si scrivono costantemente i test dal punto di vista dell'utente. Pertanto, lo sviluppo basato su test può comportare API più pulite e più comprensibili.

Infine, lo sviluppo basato su test impone di scrivere unit test come parte del normale processo di scrittura di un'applicazione. Con l'avvicinarsi della scadenza di un progetto, il test è in genere la prima cosa che esce dalla finestra. Quando si pratica lo sviluppo basato su test, d'altra parte, è più probabile che si sia virtuosi nella scrittura di unit test perché lo sviluppo basato su test rende gli unit test centrali per il processo di creazione di un'applicazione.

> [!NOTE] 
> 
> Per saperne di più sullo sviluppo basato su test, vi consiglio di leggere il libro di Michael Feathers **Working Effectively with Legacy Code**.

In questa iterazione viene aggiunta una nuova funzionalità all'applicazione Contact Manager. Aggiungiamo il supporto per i gruppi di contatti. È possibile utilizzare i gruppi di contatti per organizzare i contatti in categorie quali i gruppi Business e Friend.

Aggiungeremo questa nuova funzionalità all'applicazione seguendo un processo di sviluppo basato su test. Scriveremo prima gli unit test e scriveremo tutto il codice rispetto a questi test.

## <a name="what-gets-tested"></a>Elementi testati

Come illustrato nell'iterazione precedente, in genere non si scrivono unit test per la logica di accesso ai dati o la logica di visualizzazione. Non si scrivono unit test per la logica di accesso ai dati perché l'accesso a un database è un'operazione relativamente lenta. Non si scrivono unit test per la logica di visualizzazione perché l'accesso a una visualizzazione richiede la rotazione di un server Web che è un'operazione relativamente lenta. Non si dovrebbe scrivere uno unit test a meno che il test può essere eseguito più e più volte molto veloce

Poiché lo sviluppo basato su test è guidato da unit test, ci concentriamo inizialmente sulla scrittura di controller e logica di business. Evitiamo di toccare il database o le viste. Non modificheremo il database o creeremo le nostre viste fino alla fine di questa esercitazione. Iniziamo con ciò che può essere testato.

## <a name="creating-user-stories"></a>Creazione di storie utente

Quando si pratica lo sviluppo basato su test, si inizia sempre scrivendo un test. Questo solleva immediatamente la domanda: Come si fa a decidere quale test scrivere per primo? Per rispondere a questa domanda, è necessario scrivere un set di [*storie utente*](http://en.wikipedia.org/wiki/User_stories).

Una user story è una descrizione molto breve (di solito una frase) di un requisito software. Deve essere una descrizione non tecnica di un requisito scritto dal punto di vista dell'utente.

Di seguito è riportato l'insieme di storie utente che descrivono le funzionalità richieste dalla nuova funzionalità Gruppo di contatti:

1. L'utente può visualizzare un elenco di gruppi di contatti.
2. L'utente può creare un nuovo gruppo di contatti.
3. L'utente può eliminare un gruppo di contatti esistente.
4. L'utente può selezionare un gruppo di contatti durante la creazione di un nuovo contatto.
5. L'utente può selezionare un gruppo di contatti quando modifica un contatto esistente.
6. Nella vista Indice viene visualizzato un elenco di gruppi di contatti.
7. Quando un utente fa clic su un gruppo di contatti, viene visualizzato un elenco di contatti corrispondenti.

Si noti che questo elenco di storie utente è completamente comprensibile da un cliente. Non si fa menzione dei dettagli tecnici di attuazione.

Durante il processo di compilazione dell'applicazione, il set di storie utente può diventare più raffinato. È possibile suddividere una User story in più storie (requisiti). Ad esempio, è possibile decidere che la creazione di un nuovo gruppo di contatti debba comportare la convalida. L'invio di un gruppo di contatti senza nome deve restituire un errore di convalida.

Dopo aver creato un elenco di storie utente, è possibile scrivere il primo unit test. Inizieremo creando uno unit test per visualizzare l'elenco dei gruppi di contatti.

## <a name="listing-contact-groups"></a>Elenco dei gruppi di contatti

La nostra prima storia utente è che un utente deve essere in grado di visualizzare un elenco di gruppi di contatti. Dobbiamo esprimere questa storia con un test.

Creare un nuovo unit test facendo clic con il pulsante destro del mouse sulla cartella Controllers nel progetto ContactManager.Tests, selezionando **Add, New Test**e selezionando il modello **Unit Test** (vedere la Figura 1). Denominare il nuovo unit test GroupControllerTest.vb e fare clic sul pulsante **OK.**

[![Aggiunta dello unit test GroupControllerTest](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)

**Figura 01**: Aggiunta dello unit test GroupControllerTest[(Fare clic per visualizzare l'immagine a dimensioni intere)](iteration-6-use-test-driven-development-vb/_static/image2.png)

Il nostro primo unit test è contenuto nel listato 1. Questo test verifica che il metodo Index() del controller Group restituisca un set di gruppi. Il test verifica che una raccolta di gruppi venga restituita nei dati della visualizzazione.

**Listato 1 - Controllers-GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample1.vb)]

Quando si digita per la prima volta il codice nel listato 1 in Visual Studio, si otterrà un sacco di linee ondulate rosse. Non è stata creata la classe GroupController o Group.

A questo punto, possiamo t anche costruire la nostra applicazione in modo che possiamo t eseguire il nostro primo unit test. Questo è buono. Questo conta come un test non riuscito. Pertanto, ora è possibile iniziare a scrivere il codice dell'applicazione. Abbiamo bisogno di scrivere codice sufficiente per eseguire il nostro test.

La classe controller di gruppo nel listato 2 contiene il minimo di codice necessario per superare lo unit test. L'azione Index() restituisce un elenco codificato staticamente di gruppi (la classe Group è definita nel listato 3).

**Listato 2 - Controllers-GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample2.vb)]

**Listato 3 - Modelli-Gruppo.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample3.vb)]

Dopo aver aggiunto le classi GroupController e Group al progetto, il primo unit test viene completato correttamente (vedere la Figura 2). Abbiamo fatto il lavoro minimo necessario per superare il test. È tempo di festeggiare.

[![Successo!](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)

**Figura 02**: Successo! ([Fare clic per visualizzare l'immagine a dimensioni intere](iteration-6-use-test-driven-development-vb/_static/image4.png))

## <a name="creating-contact-groups"></a>Creazione di gruppi di contatti

Ora possiamo passare alla seconda User story. Dobbiamo essere in grado di creare nuovi gruppi di contatti. Dobbiamo esprimere questa intenzione con un test.

Il test nel listato 4 verifica che la chiamata al metodo Create() con un nuovo gruppo aggiunge il gruppo all'elenco dei gruppi restituiti dal metodo Index(). In altre parole, se creo un nuovo gruppo, dovrei essere in grado di ottenere il nuovo gruppo dall'elenco dei gruppi restituiti dal metodo Index().

**Listato 4 - Controllers-GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample4.vb)]

Il test nel listato 4 chiama il metodo Create() del controller di gruppo con un nuovo gruppo di contatti. Successivamente, il test verifica che la chiamata al metodo Index() del controller di gruppo restituisca il nuovo gruppo nei dati di visualizzazione.

Il controller di gruppo modificato nel listato 5 contiene il minimo indispensabile di modifiche necessarie per superare il nuovo test.

**Listato 5 - Controllers-GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample5.vb)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>Il controller di gruppo nel listato 5 ha una nuova azione Create(). Questa azione aggiunge un gruppo a una raccolta di gruppi. Si noti che l'azione Index() è stata modificata per restituire il contenuto della raccolta di gruppi.

Ancora una volta, abbiamo eseguito la quantità minima di lavoro necessaria per superare lo unit test. Dopo aver apportato queste modifiche al controller di gruppo, tutti gli unit test vengono superati.

## <a name="adding-validation"></a>Aggiunta della convalida

Questo requisito non è stato indicato in modo esplicito nella User story. Tuttavia, è ragionevole richiedere che un gruppo abbia un nome. In caso contrario, organizzare i contatti in gruppi non sarebbe molto utile.

Il listato 6 contiene un nuovo test che esprime questa intenzione. Questo test verifica che il tentativo di creare un gruppo senza fornire un nome genera un messaggio di errore di convalida nello stato del modello.

**Listato 6 - Controllers-GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample6.vb)]

Per soddisfare questo test, è necessario aggiungere una proprietà Name alla classe Group (vedere il listato 7). Inoltre, è necessario aggiungere un po 'di logica di convalida per il nostro controller di gruppo s Create() azione (vedere listato 8).

**Listato 7 - Modelli-Gruppo.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample7.vb)]

**Listato 8 - Controllers-GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample8.vb)]

Si noti che l'azione Create() del controller di gruppo contiene ora sia la convalida che la logica del database. Attualmente, il database utilizzato dal controller di gruppo è costituito da nient'altro che una raccolta in memoria.

## <a name="time-to-refactor"></a>Tempo per il refactoring

Il terzo passaggio in Rosso/Verde/Refactoring è la parte Effettua refactoring. A questo punto, è necessario tornare indietro dal codice e considerare come è possibile effettuare il refactoring dell'applicazione per migliorarne la progettazione. La fase di refactoring è la fase in cui riflettre amoglia no sul modo migliore di implementare i principi e i modelli di progettazione software.

Siamo liberi di modificare il nostro codice in qualsiasi modo scegliamo di migliorare la progettazione del codice. Abbiamo una rete di sicurezza di unit test che ci impediscono di rompere le funzionalità esistenti.

In questo momento, il nostro controller di gruppo è un pasticcio dal punto di vista della buona progettazione del software. Il controller di gruppo contiene un pasticcio aggrovigliato di convalida e codice di accesso ai dati. Per evitare di violare il principio di responsabilità unica, è necessario separare queste preoccupazioni in classi diverse.

La classe controller di gruppo di cui è stato esefattore è contenuta nel listato 9.Our refactored Group controller class is contained in Listing 9. Il controller è stato modificato per utilizzare il livello di servizio ContactManager. Si tratta dello stesso livello di servizio utilizzato con il controller di contatto.

Il listato 10 contiene i nuovi metodi aggiunti al livello di servizio ContactManager per supportare la convalida, l'elenco e la creazione di gruppi. L'interfaccia IContactManagerService è stata aggiornata per includere i nuovi metodi.

Il listato 11 contiene una nuova classe FakeContactManagerRepository che implementa l'interfaccia IContactManagerRepository.Listing 11 contains a new FakeContactManagerRepository class that implements the IContactManagerRepository interface. A differenza della classe EntityContactManagerRepository che implementa anche l'interfaccia IContactManagerRepository, la nuova classe FakeContactManagerRepository non comunica con il database. La classe FakeContactManagerRepository utilizza una raccolta in memoria come proxy per il database. Useremo questa classe nei nostri unit test come un livello di repository falso.

**Listato 9 - Controllers-GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample9.vb)]

**Listato 10 - Controllers- ContactManagerService.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample10.vb)]

**Listato 11 - Controllers-FakeContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample11.vb)]

La modifica dell'interfaccia IContactManagerRepository richiede l'utilizzo per implementare i metodi CreateGroup() e ListGroups() nella classe EntityContactManagerRepository. Il modo più pigro e più veloce per farlo è quello di aggiungere metodi stub che assomigliano a questo:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample12.vb)]

Infine, queste modifiche alla progettazione dell'applicazione richiedono alcune modifiche agli unit test. È ora necessario utilizzare il FakeContactManagerRepository quando si eseguono gli unit test. La classe GroupControllerTest aggiornata è contenuta nel listato 12.The updated GroupControllerTest class is contained in Listing 12.

**Listato 12 - Controllers-GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample13.vb)]

Dopo aver apportato tutte queste modifiche, ancora una volta, tutti i nostri unit test passano. Abbiamo completato l'intero ciclo di rosso/verde/Refactoring. Abbiamo implementato le prime due storie utente. Sono ora disponibili unit test di supporto per i requisiti espressi nelle storie utente. L'implementazione del resto delle storie utente comporta la ripetizione dello stesso ciclo di rosso/verde/refactoring.

## <a name="modifying-our-database"></a>Modifica del database

Purtroppo, anche se abbiamo soddisfatto tutti i requisiti espressi dai nostri unit test, il nostro lavoro non è finito. Dobbiamo ancora modificare il nostro database.

È necessario creare una nuova tabella di database Group.We need to create a new Group database table. A tale scopo, seguire questa procedura:

1. Nella finestra Esplora server fare clic con il pulsante destro del mouse sulla cartella Tabelle e selezionare l'opzione di menu **Aggiungi nuova tabella**.
2. Immettere le due colonne descritte di seguito in Progettazione tabelle.
3. Contrassegnare la colonna Id come chiave primaria e la colonna Identity.
4. Salvare la nuova tabella con il nome Gruppi facendo clic sull'icona del dischetto.

<a id="0.12_table01"></a>

| **Nome colonna** | **Tipo di dati** | **Consenti valori NULL** |
| --- | --- | --- |
| ID | INT | False |
| Nome | nvarchar(50) | False |

Successivamente, è necessario eliminare tutti i dati dalla tabella Contatti (in caso contrario, non sarà possibile creare una relazione tra le tabelle Contatti e Gruppi). A tale scopo, seguire questa procedura:

1. Fare clic con il pulsante destro del mouse sulla tabella Contatti e selezionare l'opzione di menu **Mostra dati tabella**.
2. Eliminare tutte le righe.

Successivamente, è necessario definire una relazione tra la tabella di database Gruppi e la tabella di database Contatti esistente. A tale scopo, seguire questa procedura:

1. Fare doppio clic sulla tabella Contacts nella finestra Esplora server per aprire Progettazione tabelle.
2. Aggiungere una nuova colonna Integer alla tabella Contacts denominata GroupId.Add a new integer column to the Contacts table named GroupId.
3. Fare clic sul pulsante Relazione per aprire la finestra di dialogo Relazioni chiave esterna (vedere la Figura 3).
4. Fare clic sul pulsante Aggiungi.
5. Fare clic sul pulsante con i conielli visualizzati accanto al pulsante Specifica tabella e colonne.
6. Nella finestra di dialogo Tabelle e colonne selezionare Gruppi come tabella chiave primaria e ID come colonna chiave primaria. Selezionare Contatti come tabella chiave esterna e GroupId come colonna chiave esterna (vedere la figura 4). Fare clic sul pulsante OK.
7. In **Specifica INSERT e UPDATE**selezionare il valore **Sovrapponi** per Elimina **regola**.
8. Fare clic sul pulsante Chiudi per chiudere la finestra di dialogo Relazioni chiavi esterne.
9. Fare clic sul pulsante Salva per salvare le modifiche apportate alla tabella Contatti.

[![Creazione di una relazione tra tabelle di database](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)

**Figura 03**: Creazione di una relazione di tabella di database ([Fare clic per visualizzare l'immagine a dimensioni intere](iteration-6-use-test-driven-development-vb/_static/image6.png))

[![Specifica delle relazioni tra tabelle](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)

**Figura 04**: Specifica delle relazioni tra tabelle ([Fare clic per visualizzare l'immagine a dimensioni intere)](iteration-6-use-test-driven-development-vb/_static/image8.png)

### <a name="updating-our-data-model"></a>Aggiornamento del modello di datiUpdating our Data Model

Successivamente, è necessario aggiornare il modello di dati per rappresentare la nuova tabella di database. A tale scopo, seguire questa procedura:

1. Fare doppio clic sul file ContactManagerModel.edmx nella cartella Modelli per aprire Entity Designer.
2. Fare clic con il pulsante destro del mouse sull'area Designer e selezionare l'opzione di menu **Aggiorna modello da database**.
3. Nell'Aggiornamento guidato selezionare la tabella Gruppi e fare clic sul pulsante Fine (vedere la figura 5).
4. Fare clic con il pulsante destro del mouse sull'entità Gruppi e selezionare l'opzione di menu **Rinomina**. Modificare il nome dell'entità *Gruppi* in *Gruppo* (singolare).
5. Fare clic con il pulsante destro del mouse sulla proprietà di navigazione Gruppi visualizzata nella parte inferiore dell'entità Contatto. Modificare il nome della proprietà di navigazione *Gruppi* in *Gruppo* (singolare).

[![Aggiornamento di un modello Entity Framework dal databaseUpdating an Entity Framework model from the database](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)

**Figura 05:** aggiornamento di un modello Entity Framework dal[database(Fare clic per visualizzare l'immagine a dimensioni intere)](iteration-6-use-test-driven-development-vb/_static/image10.png)

Dopo aver completato questi passaggi, il modello di dati rappresenterà entrambe le tabelle Contatti e Gruppi. Entity Designer dovrebbe mostrare entrambe le entità (vedere Figura 6).

[![Entity Designer visualizza Gruppo e Contatto](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)

**Figura 06**: Entity Designer che visualizza Gruppo e Contatto[(Fare clic per visualizzare l'immagine](iteration-6-use-test-driven-development-vb/_static/image12.png)a dimensioni intere)

### <a name="creating-our-repository-classes"></a>Creazione delle classi di repositoryCreating our Repository Classes

Successivamente, è necessario implementare la classe di repository. Nel corso di questa iterazione, sono stati aggiunti diversi nuovi metodi all'interfaccia IContactManagerRepository durante la scrittura di codice per soddisfare gli unit test. La versione finale dell'interfaccia IContactManagerRepository è contenuta nel listato 14.

**Listato 14 - Modelli: IContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample14.vb)]

Non è stato effettivamente implementato uno dei metodi correlati all'utilizzo dei gruppi di contatti nella classe EntityContactManagerRepository reale. Attualmente, la classe EntityContactManagerRepository dispone di metodi stub per ognuno dei metodi del gruppo di contatti elencati nell'interfaccia IContactManagerRepository. Ad esempio, il metodo ListGroups() è attualmente simile al seguente:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample15.vb)]

I metodi stub ci hanno permesso di compilare l'applicazione e superare gli unit test. Tuttavia, ora è il momento di implementare effettivamente questi metodi. La versione finale della classe EntityContactManagerRepository è contenuta nel listato 13.

**Listato 13 - Modelli, EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample16.vb)]

### <a name="creating-the-views"></a>Creazione delle viste

ASP.NET'applicazione MVC quando si utilizza il motore di visualizzazione ASP.NET predefinito. Pertanto, non creare viste in risposta a un particolare unit test. Tuttavia, poiché un'applicazione sarebbe inutile senza viste, è possibile completare questa iterazione senza creare e modificare le viste contenute nell'applicazione Contact Manager.

È necessario creare le seguenti nuove visualizzazioni per la gestione dei gruppi di contatti (vedere figura 7):

- Visualizzazioni- Gruppo-Index.aspx - Visualizza l'elenco dei gruppi di contatti
- Views-Group-Delete.aspx - Visualizza il modulo di conferma per l'eliminazione di un gruppo di contatti

[![La vista Indice di gruppo](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)

**Figura 07**: Visualizzazione Indice di gruppo[(fare clic per visualizzare l'immagine a dimensioni intere)](iteration-6-use-test-driven-development-vb/_static/image14.png)

È necessario modificare le seguenti visualizzazioni esistenti in modo che includano i gruppi di contatti:

- Visualizzazioni, Home, Create.aspx
- Viste: Home, Modifica.aspx
- Visualizzazioni, Home, Indice.aspx

È possibile visualizzare le visualizzazioni modificate esaminando l'applicazione di Visual Studio che accompagna questa esercitazione. Ad esempio, Figura 8 illustra la visualizzazione dell'indice di contatto.

[![Visualizzazione Dell'indice contatti](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)

**Figura 08**: Visualizzazione Indice contatti[(fare clic per visualizzare l'immagine a dimensioni intere)](iteration-6-use-test-driven-development-vb/_static/image16.png)

## <a name="summary"></a>Riepilogo

In questa iterazione sono state aggiunte nuove funzionalità all'applicazione Contact Manager seguendo una metodologia di progettazione dell'applicazione di sviluppo basata su test. Abbiamo iniziato creando un set di storie utente. È stato creato un set di unit test che corrisponde ai requisiti espressi dalle storie utente. Infine, è stato scritto codice sufficiente per soddisfare i requisiti espressi dagli unit test.

Dopo aver completato la scrittura di codice sufficiente per soddisfare i requisiti espressi dagli unit test, sono stati aggiornati il database e le visualizzazioni. È stata aggiunta una nuova tabella Groups al database e aggiornato il modello di dati Entity Framework. Abbiamo anche creato e modificato un insieme di viste.

Nell'iterazione successiva -- l'iterazione finale -- riscriviamo la nostra applicazione per sfruttare i vantaggi di Ajax. Sfruttando Ajax, miglioreremo la velocità di risposta e le prestazioni dell'applicazione Contact Manager.

> [!div class="step-by-step"]
> [Successivo](iteration-5-create-unit-tests-vb.md)
> [precedente](iteration-7-add-ajax-functionality-vb.md)
