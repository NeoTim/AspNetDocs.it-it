---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
title: "#1 di iterazione - Creazione dell'applicazione (VB) Documenti Microsoft"
author: rick-anderson
description: 'Nella prima iterazione, creiamo il Contact Manager nel modo più semplice possibile. Aggiungiamo il supporto per le operazioni di base del database: Crea, Leggi, Aggiorna e D...'
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 5b033582-1646-42c2-b20d-7edc8814e970
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
msc.type: authoredcontent
ms.openlocfilehash: 235f6f8812a2f584de8e07dcf97d5c1712c51776
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542534"
---
# <a name="iteration-1--create-the-application-vb"></a>Iterazione 1 - Creare l'applicazione (VB)

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il codice](iteration-1-create-the-application-vb/_static/contactmanager_1_vb1.zip)

> Nella prima iterazione, creiamo il Contact Manager nel modo più semplice possibile. Aggiungiamo il supporto per le operazioni di base del database: crea, leggi, aggiorna ed elimina (CRUD).

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

In questa prima iterazione, viene compilata l'applicazione di base. L'obiettivo è quello di costruire il Contact Manager nel modo più semplice e veloce possibile. Nelle iterazioni successive, si migliora la progettazione dell'applicazione.

L'applicazione Contact Manager è un'applicazione di base basata su database. È possibile utilizzare l'applicazione per creare nuovi contatti, modificare contatti esistenti ed eliminare contatti.

In questa iterazione vengono completati i passaggi seguenti:In this iteration, we complete the following steps:

1. ASP.NET'applicazione MVC
2. Creare un database per memorizzare i nostri contatti
3. Generare una classe modello per il database con Microsoft Entity Framework
4. Creare un'azione e una visualizzazione del controller che consente di elencare tutti i contatti nel database
5. Creare azioni del controller e una visualizzazione che consente di creare un nuovo contatto nel databaseCreate controller actions and a view that enables us to create a new contact in the database
6. Creare azioni del controller e una visualizzazione che consente di modificare un contatto esistente nel databaseCreate controller actions and a view that enables us to edit an existing contact in the database
7. Creare azioni del controller e una vista che consente di eliminare un contatto esistente nel database

## <a name="software-prerequisites"></a>Prerequisiti software

In ASP.NET applicazioni MVC, è necessario disporre di Visual Studio 2008 o Visual Web Developer 2008 installato nel computer (Visual Web Developer è una versione gratuita di Visual Studio che non include tutte le funzionalità avanzate di Visual Studio). È possibile scaricare la versione di valutazione di Visual Studio 2008 o Visual Web Developer dal seguente indirizzo:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Per ASP.NET applicazioni MVC con Visual Web Developer, è necessario che sia installato Visual Web Developer Service Pack 1. Senza Service Pack 1, non è possibile creare progetti di applicazione Web.

ASP.NET framework MVC. È possibile scaricare il framework mvc ASP.NET dal seguente indirizzo:

[https://www.asp.net/mvc](../../../index.md)

In questa esercitazione viene usato Microsoft Entity Framework per accedere a un database. Entity Framework è incluso in .NET Framework 3.5 Service Pack 1. È possibile scaricare il service pack dal seguente percorso:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

In alternativa all'esecuzione di ciascuno di questi download uno per uno, è possibile sfruttare l'Installazione guidata piattaforma Web (Web PI). È possibile scaricare il PI Web dal seguente indirizzo:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>ASP.NET di progetto MVC

ASP.NET MVC Web Application Project. Avviare Visual Studio e selezionare l'opzione di menu **File, Nuovo progetto**. Viene visualizzata la finestra di dialogo **Nuovo progetto** (vedere la figura 1). Selezionare il tipo di progetto **Web** e il ASP.NET modello **Applicazione Web MVC.** Assegnare al nuovo progetto il nome *ContactManager* e fare clic sul pulsante OK.

Assicurarsi di avere selezionato .NET Framework 3.5 dall'elenco a discesa in alto a destra nella finestra di dialogo **Nuovo progetto.** In caso contrario, il modello applicazione Web MVC ASP.NET non verrà visualizzato.

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image1.jpg)](iteration-1-create-the-application-vb/_static/image1.png)

**Figura 01**: Finestra di dialogo Nuovo progetto[(Fare clic per visualizzare l'immagine a dimensioni intere)](iteration-1-create-the-application-vb/_static/image2.png)

ASP.NET applicazione MVC, viene visualizzata la finestra di dialogo Crea progetto unit **test.** È possibile utilizzare questa finestra di dialogo per indicare che si desidera creare e aggiungere un progetto di unit test alla soluzione quando si crea ASP.NET'applicazione MVC. Anche se non verranno compilati unit test in questa iterazione, è necessario selezionare l'opzione **Sì, creare un progetto** di unit test perché si prevede di aggiungere unit test in un'iterazione successiva. Aggiunta di un progetto di test quando si crea per la prima volta un nuovo progetto mvc ASP.NET è molto più semplice rispetto all'aggiunta di un progetto test dopo la creazione del progetto MVC ASP.NET.

> [!NOTE] 
> 
> Poiché Visual Web Developer non supporta i progetti di test, non viene visualizzato la finestra di dialogo Crea progetto unit test quando si utilizza Visual Web Developer.

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image2.jpg)](iteration-1-create-the-application-vb/_static/image3.png)

**Figura 02**: La finestra di dialogo Crea progetto unit test[(Fare clic per visualizzare l'immagine](iteration-1-create-the-application-vb/_static/image4.png)a dimensioni intere)

ASP.NET'applicazione MVC viene visualizzata nella finestra Esplora soluzioni di Visual Studio (vedere la Figura 3). Se la finestra Esplora soluzioni non è visualizzata, è possibile aprire questa finestra selezionando l'opzione di menu **Visualizza, Esplora soluzioni**. Si noti che la soluzione contiene due progetti: il progetto MVC ASP.NET e il progetto Test. Il progetto MVC ASP.NET è denominato ContactManager e il progetto Test è denominato ContactManager.Tests.

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image3.jpg)](iteration-1-create-the-application-vb/_static/image5.png)

**Figura 03:** finestra Esplora soluzioni ([Fare clic per visualizzare l'immagine](iteration-1-create-the-application-vb/_static/image6.png)a dimensioni intere )

## <a name="deleting-the-project-sample-files"></a>Eliminazione dei file di esempio del progetto

Il modello di progetto MVC ASP.NET include file di esempio per controller e visualizzazioni. Prima di creare una nuova ASP.NET applicazione MVC, è necessario eliminare questi file. È possibile eliminare file e cartelle nella finestra Esplora soluzioni facendo clic con il pulsante destro del mouse su un file o una cartella e selezionando l'opzione di menu **Elimina**.

È necessario eliminare i seguenti file dal progetto MVC ASP.NET:

- Controller di controllo.vb

- <a0>Pagina</a0><a1></a1><a2></a

- Views

Inoltre, è necessario eliminare il file seguente dal progetto Test:

Controller di controllo HomeControllerTest.vb

## <a name="creating-the-database"></a>Creazione del database

L'applicazione Contact Manager è un'applicazione Web basata su database. Usiamo un database per memorizzare le informazioni di contatto.

Il ASP.NET framework MVC con qualsiasi database moderno, inclusi i database Microsoft SQL Server, Oracle, MySQL e IBM DB2. In questa esercitazione viene usato un database di Microsoft SQL Server.In this tutorial, we use a Microsoft SQL Server database. Quando si installa Visual Studio, viene fornita la possibilità di installare Microsoft SQL Server Express, una versione gratuita del database di Microsoft SQL Server.

Creare un nuovo database facendo\_clic con il pulsante destro del mouse sulla cartella App Data nella finestra Esplora soluzioni e selezionando l'opzione di menu **Aggiungi, Nuovo elemento**. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare la categoria **Dati** e il modello di database di **SQL Server** (vedere la figura 4). Assegnare al nuovo database il nome ContactManagerDB.mdf e fare clic sul pulsante OK.

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image4.jpg)](iteration-1-create-the-application-vb/_static/image7.png)

**Figura 04**: Creazione di un nuovo database di Microsoft SQL Server Express ([Fare clic per visualizzare l'immagine](iteration-1-create-the-application-vb/_static/image8.png)a dimensioni intere )

Dopo aver creato il nuovo database,\_il database viene visualizzato nella cartella App Data nella finestra Esplora soluzioni. Fare doppio clic sul file ContactManager.mdf per aprire la finestra Esplora server e connettersi al database.

> [!NOTE] 
> 
> La finestra Esplora server è denominata finestra Esplora database nel caso di Microsoft Visual Web Developer.

È possibile utilizzare la finestra Esplora server per creare nuovi oggetti di database, ad esempio tabelle di database, viste, trigger e stored procedure. Fare clic con il pulsante destro del mouse sulla cartella Tabelle e selezionare l'opzione di menu **Aggiungi nuova tabella**. Verrà visualizzata la finestra di progettazione tabelle database (vedere la figura 5).

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image5.jpg)](iteration-1-create-the-application-vb/_static/image9.png)

**Figura 05**: Progettazione tabelle database ([Fare clic per visualizzare l'immagine a dimensioni intere](iteration-1-create-the-application-vb/_static/image10.png))

È necessario creare una tabella che contiene le colonne seguenti:We need to create a table that contains the following columns:

<a id="0.2_table01"></a>

| **Nome colonna** | **Tipo di dati** | **Consenti valori NULL** |
| --- | --- | --- |
| ID | INT | false |
| FirstName | nvarchar(50) | false |
| LastName | nvarchar(50) | false |
| Telefono | nvarchar(50) | false |
| Email | nvarchar(255) | false |

La prima colonna, la colonna Id, è speciale. È necessario contrassegnare la colonna Id come colonna Identity e come colonna chiave primaria. Si indica che una colonna è una colonna Identity espandendo proprietà colonna (vedere la parte inferiore della figura 6) e scorrere verso il basso fino alla proprietà specifica di identità. Impostare la proprietà **(Identità)** sul valore **Sì**.

Per contrassegnare una colonna come colonna chiave primaria, selezionare la colonna e fare clic sul pulsante con l'icona di una chiave. Dopo che una colonna è contrassegnata come colonna chiave primaria, viene visualizzata un'icona di una chiave accanto alla colonna (vedere la figura 6).

Dopo aver creato la tabella, fare clic sul pulsante Salva (il pulsante con l'icona di un dischetto) per salvare la nuova tabella. Assegnare alla nuova tabella il nome *Contatti*.

Dopo aver creato la tabella di database Contatti, è necessario aggiungere alcuni record alla tabella. Fare clic con il pulsante destro del mouse sulla tabella Contatti nella finestra Esplora server e selezionare l'opzione di menu **Mostra dati tabella**. Immettere uno o più contatti nella griglia visualizzata.

## <a name="creating-the-data-model"></a>Creazione del modello di datiCreating the Data Model

L'applicazione ASP.NET MVC è costituita da modelli, visualizzazioni e controller. Iniziamo creando una classe Model che rappresenta la tabella Contacts creata nella sezione precedente.

In questa esercitazione viene usato Microsoft Entity Framework per generare automaticamente una classe modello dal database.

> [!NOTE] 
> 
> Il framework di MVC ASP.NET non è legato a Microsoft Entity Framework in alcun modo. È possibile utilizzare ASP.NET MVC con tecnologie di accesso al database alternative, tra cui NHibernate, LINQ to SQL o ADO.NET.

Seguire questi passaggi per creare le classi del modello di dati:Follow these steps to create the data model classes:

1. Fare clic con il pulsante destro del mouse sulla cartella Modelli nella finestra Esplora soluzioni e scegliere **Aggiungi, Nuovo elemento**. Viene visualizzata la finestra di dialogo **Aggiungi nuovo elemento** (vedere la figura 6).
2. Selezionare la categoria **Dati** e il modello **ADO.NET Entity Data Model.** Assegnare al modello di dati il nome ContactManagerModel.edmx e fare clic sul pulsante **Aggiungi.Name** your data model *ContactManagerModel.edmx* and click the Add button. Viene visualizzata la procedura guidata Entity Data Model (vedere la Figura 7).
3. Nel passaggio **Scegli contenuto modello** selezionare Genera da **database** (vedere la figura 7).
4. Nel passaggio Scegliere la **connessione dati** selezionare il database ContactManagerDB.mdf e immettere il nome *ContactManagerDBEntities* per le impostazioni di connessione entità (vedere la figura 8).
5. Nel passaggio Scegli oggetti di **database** selezionare la casella di controllo Tabelle con etichetta (vedere la figura 9). Il modello di dati includerà tutte le tabelle contenute nel database (ne esiste solo una, la tabella Contacts). Immettere lo spazio dei nomi *Modelli*. Fare clic sul pulsante Fine per completare la procedura guidata.

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image6.jpg)](iteration-1-create-the-application-vb/_static/image11.png)

**Figura 06**: La finestra di dialogo Aggiungi nuovo elemento[(Fare clic per visualizzare l'immagine](iteration-1-create-the-application-vb/_static/image12.png)a dimensioni intere)

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image7.jpg)](iteration-1-create-the-application-vb/_static/image13.png)

**Figura 07**: Scegliere il contenuto del modello ([Fare clic per visualizzare l'immagine a dimensioni intere](iteration-1-create-the-application-vb/_static/image14.png))

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image8.jpg)](iteration-1-create-the-application-vb/_static/image15.png)

**Figura 08**: Scegliere la connessione dati ([Fare clic per visualizzare l'immagine](iteration-1-create-the-application-vb/_static/image16.png)a dimensioni intere )

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image9.jpg)](iteration-1-create-the-application-vb/_static/image17.png)

**Figura 09**: Scegliere gli oggetti di database ([Fare clic per visualizzare l'immagine](iteration-1-create-the-application-vb/_static/image18.png)a dimensioni intere )

Dopo aver completato la procedura guidata Entity Data Model, verrà visualizzato Entity Data Model Designer. Nella finestra di progettazione viene visualizzata una classe che corrisponde a ogni tabella modellata. Verrà visualizzata una classe denominata Contacts.You should see one class named Contacts.

La procedura guidata Entity Data Model genera nomi di classe in base ai nomi delle tabelle di database. È quasi sempre necessario modificare il nome della classe generata dalla procedura guidata. Fare clic con il pulsante destro del mouse sulla classe Contacts nella finestra di progettazione e selezionare l'opzione di menu **Rinomina**. Modificare il nome della classe da Contatti (plurali) a Contatto (singolare). Dopo aver modificato il nome della classe, la classe dovrebbe essere simile Figura 10.After you change the class name, the class should appear like Figure 10.

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image10.jpg)](iteration-1-create-the-application-vb/_static/image19.png)

**Figura 10**: La classe Contact ([Fare clic per visualizzare l'immagine](iteration-1-create-the-application-vb/_static/image20.png)a dimensioni intere )

A questo punto, è stato creato il modello di database. Possiamo usare la classe Contact per rappresentare un record di contatto specifico nel nostro database.

## <a name="creating-the-home-controller"></a>Creazione del controller principale

Il passo successivo è quello di creare il nostro controller Home. Il controller Home è il controller predefinito richiamato in un'applicazione MVC ASP.NET.

Creare la classe controller Home facendo clic con il pulsante destro del mouse sulla cartella Controllers nella finestra Esplora soluzioni e selezionando l'opzione di menu **Aggiungi, Controller** (vedere Figura 11). Si noti la casella di controllo **con l'etichetta Aggiungi metodi di azione per gli scenari di creazione, aggiornamento e dettagli**. Assicurarsi che questa casella di controllo sia selezionata prima di fare clic sul pulsante **Aggiungi.**

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image11.jpg)](iteration-1-create-the-application-vb/_static/image21.png)

**Figura 11**: Aggiunta del controller Home ([Fare clic per visualizzare l'immagine](iteration-1-create-the-application-vb/_static/image22.png)a dimensioni intere)

Quando si crea il controller Home, si ottiene la classe nel listato 1.

**Listato 1 - Controllers-HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample1.vb)]

## <a name="listing-the-contacts"></a>Elenco dei contatti

Per visualizzare i record nella tabella del database Contatti, è necessario creare un'azione Index() e una vista Indice.

Il controller Home contiene già un'azione Index(). Abbiamo bisogno di modificare questo metodo in modo che assomigli al listato 2.

**Listato 2 - Controllers-HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample2.vb)]

Si noti che la classe controller Home \_nel listato 2 contiene un campo privato denominato entità. Il \_campo entità rappresenta le entità del modello di dati. Usiamo il \_campo entità per comunicare con il database.

Il metodo Index() restituisce una vista che rappresenta tutti i contatti della tabella di database Contacts. Entità \_dell'espressione. ContactSet.ToList() restituisce l'elenco dei contatti come elenco generico.

Ora che è stato creato il controller di indice, è necessario creare la visualizzazione dell'indice. Prima di creare la visualizzazione Indice, compilare l'applicazione selezionando l'opzione di menu **Compila, Compila soluzione**. È sempre necessario compilare il progetto prima di aggiungere una visualizzazione per visualizzare l'elenco delle classi del modello nella finestra di dialogo **Aggiungi visualizzazione.**

Per creare la vista Indice, fare clic con il pulsante destro del mouse sul metodo Index() e selezionare l'opzione di menu **Aggiungi vista** (vedere la figura 12). Selezionando questa opzione di menu si apre la finestra di dialogo **Aggiungi vista** (vedere la figura 13).

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image12.jpg)](iteration-1-create-the-application-vb/_static/image23.png)

**Figura 12**: Aggiunta della visualizzazione Indice ([Fare clic per visualizzare l'immagine a dimensioni intere)](iteration-1-create-the-application-vb/_static/image24.png)

Nella finestra di dialogo **Aggiungi visualizzazione** selezionare la casella di controllo Crea una **visualizzazione fortemente tipizzato.** Selezionare la classe di dati ContactManager.Contact e l'elenco Visualizza contenuto. La selezione di queste opzioni genera una visualizzazione che visualizza un elenco di record Contatto.

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image13.jpg)](iteration-1-create-the-application-vb/_static/image25.png)

**Figura 13**: La finestra di dialogo Aggiungi vista ([Fare clic per visualizzare l'immagine](iteration-1-create-the-application-vb/_static/image26.png)a dimensioni intere )

Quando si fa clic sul pulsante **Aggiungi,** viene generata la vista Indice nel listato 3. Si &lt;noti la&gt; direttiva % di pagina % che viene visualizzata nella parte superiore del file. La visualizzazione indice eredita dalla&lt;classe&lt;ContactManager.Models.Contact di ViewPageIEnumerableManager.Models.Contact.&gt; &gt; In altre parole, la classe Model nella visualizzazione rappresenta un elenco di entità Contact.

Il corpo della visualizzazione Index contiene un ciclo foreach che scorre ognuno dei contatti rappresentati dalla classe Model. Il valore di ogni proprietà della classe Contact viene visualizzato all'interno di una tabella HTML.

**Listato 3 - Visualizzazioni, Home, Index.aspx (non modificato)**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample3.aspx)]

È necessario apportare una modifica alla vista Indice. Poiché non viene creato una visualizzazione Dettagli, è possibile rimuovere il collegamento Dettagli. Individuare e rimuovere il codice seguente dalla visualizzazione Indice:

.id: elemento. L'ID)%&gt;

Dopo aver modificato la visualizzazione Indice, è possibile eseguire l'applicazione Contact Manager. Selezionare l'opzione di menu Debug, Avvia debug o semplicemente premere F5. La prima volta che si esegue l'applicazione, si ottiene la finestra di dialogo in Figura 14. Selezionare l'opzione **Modifica il file Web.config per abilitare il debug** e fare clic sul pulsante OK.

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image14.jpg)](iteration-1-create-the-application-vb/_static/image27.png)

**Figura 14**: Abilitazione del debug ([Fare clic per visualizzare l'immagine](iteration-1-create-the-application-vb/_static/image28.png)a dimensioni intere )

La visualizzazione Indice viene restituita per impostazione predefinita. In questa visualizzazione sono elencati tutti i dati della tabella di database Contatti (vedere la figura 15).

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image15.jpg)](iteration-1-create-the-application-vb/_static/image29.png)

**Figura 15**: Visualizzazione indice ([Fare clic per visualizzare l'immagine a dimensioni intere](iteration-1-create-the-application-vb/_static/image30.png))

Si noti che la vista Indice include un collegamento con l'etichetta Crea nuovo nella parte inferiore della vista. Nella sezione successiva viene illustrato come creare nuovi contatti.

## <a name="creating-new-contacts"></a>Creazione di nuovi contatti

Per consentire agli utenti di creare nuovi contatti, è necessario aggiungere due azioni Create() al controller Home. È necessario creare un'azione Create() che restituisca un modulo HTML per la creazione di un nuovo contatto. È necessario creare una seconda azione Create() che esegua l'inserimento effettivo del database del nuovo contatto.

I nuovi metodi Create() che è necessario aggiungere al controller Home sono contenuti nel listato 4.

**Listato 4 - Controllers-HomeController.vb (con metodi Create)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample4.vb)]

Il primo metodo Create() può essere richiamato con un HTTP GET, mentre il secondo metodo Create() può essere richiamato solo da un HTTP POST. In altre parole, il secondo metodo Create() può essere richiamato solo quando si pubblica un modulo HTML. Il primo metodo Create() restituisce semplicemente una visualizzazione che contiene il modulo HTML per la creazione di un nuovo contatto. Il secondo metodo Create() è molto più interessante: aggiunge il nuovo contatto al database.

Si noti che il secondo metodo Create() è stato modificato per accettare un'istanza della classe Contact. I valori del form inviati dal modulo HTML vengono associati automaticamente a questa classe Contact dal framework MVC ASP.NET. Ogni campo modulo del modulo HTML Create viene assegnato a una proprietà del parametro Contact.

Si noti che il parametro Contact è decorato con un attributo [Bind]. L'attributo [Bind] viene utilizzato per escludere la proprietà Contact Id dall'associazione. Poiché il Id proprietà rappresenta un Identity proprietà, non si desidera impostare il Id proprietà.

Nel corpo del metodo Create(), Entity Framework viene utilizzato per inserire il nuovo contatto nel database. Il nuovo contatto viene aggiunto al set esistente di contatti e viene chiamato il metodo SaveChanges() per reinserire queste modifiche nel database sottostante.

È possibile generare un modulo HTML per la creazione di nuovi contatti facendo clic con il pulsante destro del mouse su uno dei due metodi Create() e selezionando l'opzione di menu **Aggiungi visualizzazione** (vedere la figura 16).

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image16.jpg)](iteration-1-create-the-application-vb/_static/image31.png)

**Figura 16**: Aggiunta della visualizzazione Crea ([Fare clic per visualizzare l'immagine a dimensioni intere](iteration-1-create-the-application-vb/_static/image32.png))

Nella finestra di dialogo **Aggiungi visualizzazione,** selezionare la classe **ContactManager.Contact** e l'opzione **Crea** per visualizzare il contenuto (vedere la figura 17). Quando si fa clic sul pulsante **Aggiungi,** viene generata automaticamente una visualizzazione Crea.

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image17.jpg)](iteration-1-create-the-application-vb/_static/image33.png)

**Figura 17**: Visualizzazione dell'esplosione di una pagina ([Fare clic per visualizzare l'immagine](iteration-1-create-the-application-vb/_static/image34.png)a dimensioni intere)

La visualizzazione Crea contiene campi modulo per ognuna delle proprietà della classe Contact. Il codice per la visualizzazione Crea è incluso nel listato 5.The code for the Create view is included in Listing 5.

**Listato 5 - Visualizzazioni, Home, Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample5.aspx)]

Dopo aver modificato i metodi Create() e aggiunto la vista Crea, è possibile eseguire l'applicazione Gestione contatti e creare nuovi contatti. Fare clic sul collegamento **Crea nuovo** visualizzato nella visualizzazione Indice per passare alla visualizzazione Crea. Verrà visualizzata la visualizzazione in Figura 18.

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image18.jpg)](iteration-1-create-the-application-vb/_static/image35.png)

**Figura 18**: Crea vista ([Fare clic per visualizzare l'immagine](iteration-1-create-the-application-vb/_static/image36.png)a dimensioni intere )

## <a name="editing-contacts"></a>Modifica dei contatti

L'aggiunta della funzionalità per la modifica di un record di contatto è molto simile all'aggiunta della funzionalità per la creazione di nuovi record di contatto. In primo luogo, è necessario aggiungere due nuovi metodi Edit alla classe controller Home.First, we need to add two new Edit methods to the Home controller class. Questi nuovi metodi Edit() sono contenuti nel listato 6.

**Listato 6 - Controllers-HomeController.vb (con metodi di modifica)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample6.vb)]

Il primo metodo Edit() viene richiamato da un'operazione HTTP GET. Un Parametro Id viene passato a questo metodo che rappresenta l'ID del record del contatto in corso di modifica. Entity Framework viene utilizzato per recuperare un contatto che corrisponde all'ID. Viene restituita una visualizzazione che contiene un modulo HTML per la modifica di un record.

Il secondo metodo Edit() esegue l'aggiornamento effettivo al database. Questo metodo accetta un'istanza della classe Contact come parametro. Il framework ASP.NET MVC associa automaticamente i campi del modulo dal form di modifica a questa classe. Si noti che non si include l'attributo [Bind] durante la modifica di un oggetto Contact (è necessario il valore della proprietà Id).

Entity Framework viene utilizzato per salvare il contatto modificato nel database. Il contatto originale deve essere recuperato prima dal database. Successivamente, viene chiamato il metodo ApplyPropertyChanges() di Entity Framework per registrare le modifiche apportate al contatto. Infine, viene chiamato il metodo SaveChanges() di Entity Framework per rendere persistenti le modifiche apportate al database sottostante.

È possibile generare la visualizzazione che contiene il modulo di modifica facendo clic con il pulsante destro del mouse sul metodo Edit() e selezionando l'opzione di menu Aggiungi visualizzazione. Nella finestra di dialogo Aggiungi visualizzazione selezionare la classe **ContactManager.Models.Contact** e il contenuto della visualizzazione **di modifica** (vedere la figura 19).

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image19.jpg)](iteration-1-create-the-application-vb/_static/image37.png)

**Figura 19**: Aggiunta di una vista di modifica ([Fare clic per visualizzare l'immagine](iteration-1-create-the-application-vb/_static/image38.png)a dimensioni intere )

Quando si fa clic sul pulsante Aggiungi, viene generata automaticamente una nuova visualizzazione Di modifica. Il modulo HTML generato contiene campi che corrispondono a ciascuna delle proprietà della classe Contact (vedere listato 7).

**Listato 7 - Visualizzazioni, Home, Modifica.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Eliminazione dei contatti

Se si desidera eliminare i contatti, è necessario aggiungere due azioni Delete() alla classe Controller Home. La prima azione Delete() visualizza un modulo di conferma dell'eliminazione. La seconda azione Delete() esegue l'eliminazione effettiva.

> [!NOTE] 
> 
> Successivamente, in iterazione #7, viene modificato Contact Manager in modo che supporti un'eliminazione Ajax a un passaggio.

I due nuovi metodi Delete() sono contenuti nell'elenco 8.

**Listato 8 - Controllers-HomeController.vb (metodi Delete)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample8.vb)]

Il primo metodo Delete() restituisce un modulo di conferma per l'eliminazione di un record di contatto dal database (vedere Figura20). Il secondo metodo Delete() esegue l'operazione di eliminazione effettiva sul database. Dopo che il contatto originale è stato recuperato dal database, i metodi DeleteObject() e SaveChanges() di Entity Framework vengono chiamati per eseguire l'eliminazione del database.

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image20.jpg)](iteration-1-create-the-application-vb/_static/image39.png)

**Figura 20**: La visualizzazione di conferma dell'eliminazione ([Fare clic per visualizzare l'immagine a dimensioni intere)](iteration-1-create-the-application-vb/_static/image40.png)

È necessario modificare la visualizzazione dell'indice in modo che contenga un collegamento per l'eliminazione dei record dei contatti (vedere Figura 21). È necessario aggiungere il codice seguente alla stessa cella di tabella che contiene il collegamento Modifica:

.id: elemento. L'ID)%&gt;

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image21.jpg)](iteration-1-create-the-application-vb/_static/image41.png)

**Figura 21**: Vista indice con collegamento Modifica ([Fare clic per visualizzare l'immagine a dimensioni intere)](iteration-1-create-the-application-vb/_static/image42.png)

Successivamente, è necessario creare la visualizzazione di conferma dell'eliminazione. Fare clic con il pulsante destro del mouse sul metodo Delete() nella classe Home controller e selezionare l'opzione di menu Aggiungi vista. Viene visualizzata la finestra di dialogo Aggiungi vista (vedere la figura 22).

A differenza delle viste Elenco, Crea e Modifica, la finestra di dialogo Aggiungi vista non contiene un'opzione per creare una vista Elimina. Selezionare invece la classe di dati **ContactManager.Models.Contact** e il contenuto della visualizzazione **vuota.** Selezionando l'opzione Svuota contenuto visualizzazione sarà necessario creare la vista da soli.

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image22.jpg)](iteration-1-create-the-application-vb/_static/image43.png)

**Figura 22**: Aggiunta della visualizzazione di conferma dell'eliminazione ([Fare clic per visualizzare l'immagine a dimensioni intere)](iteration-1-create-the-application-vb/_static/image44.png)

Il contenuto della vista Elimina è contenuto nel listato 9. Questa visualizzazione contiene un modulo che conferma se un particolare contatto deve essere eliminato o meno (vedere figura 21).

**Listato 9 - Visualizzazioni, Home, Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Modifica del nome del controller predefinito

Potrebbe infastidire che il nome della nostra classe controller per l'utilizzo dei contatti è denominato la classe HomeController.It might bother you that the name of our controller class for working with contacts is named the HomeController class. Il controller non deve essere denominato ContactController?

Questo problema è abbastanza facile da risolvere. In primo luogo, è necessario effettuare il refactoring del nome del controller Home. Aprire la classe HomeController nell'editor di codice di Visual Studio, fare clic con il pulsante destro del mouse sul nome della classe e selezionare l'opzione di menu **Rinomina**. Selezionando questa opzione di menu si apre la finestra di dialogo Rinomina.

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image23.jpg)](iteration-1-create-the-application-vb/_static/image45.png)

**Figura 23:** Refactoring del nome di un controller ([Fare clic per visualizzare l'immagine](iteration-1-create-the-application-vb/_static/image46.png)a dimensioni intere)

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image24.jpg)](iteration-1-create-the-application-vb/_static/image47.png)

**Figura 24**: Utilizzo della finestra di dialogo Rinomina ([Fare clic per visualizzare l'immagine a dimensioni intere)](iteration-1-create-the-application-vb/_static/image48.png)

Se si rinomina la classe controller, Visual Studio aggiornerà anche il nome della cartella nella cartella Views. Visual Studio rinominerà la cartella "Views" nella cartella "Views" (Viste) o "Contact".

Dopo aver apportato questa modifica, l'applicazione non diverrà più un controller Home.After you make this change, your application will no longer have a Home controller. Quando si esegue l'applicazione, si otterrà la pagina di errore in Figura 25.

[![Finestra di dialogo relativa al nuovo progetto](iteration-1-create-the-application-vb/_static/image25.jpg)](iteration-1-create-the-application-vb/_static/image49.png)

**Figura 25**: Nessun controller predefinito ([Fare clic per visualizzare l'immagine](iteration-1-create-the-application-vb/_static/image50.png)a dimensioni intere )

È necessario aggiornare la route predefinita nel file Global.asax per utilizzare il controller di contatto anziché il controller Home. Aprire il file Global.asax e modificare il controller predefinito utilizzato dalla route predefinita (vedere il listato 10).

**Listato 10 - Global.asax.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample10.vb)]

Dopo aver apportato queste modifiche, Contact Manager verrà eseguito correttamente. A questo punto, verrà utilizzata la classe controller di contatto come controller predefinito.

## <a name="summary"></a>Riepilogo

In questa prima iterazione, abbiamo creato un'applicazione Contact Manager di base nel modo più veloce possibile. Abbiamo sfruttato Visual Studio per generare automaticamente il codice iniziale per i controller e le visualizzazioni. Abbiamo anche approfittato di Entity Framework per generare automaticamente le classi del modello di database.

Attualmente, possiamo elencare, creare, modificare ed eliminare i record dei contatti con l'applicazione Contact Manager. In altre parole, è possibile eseguire tutte le operazioni di base del database richieste da un'applicazione Web basata su database.

Purtroppo, la nostra applicazione ha alcuni problemi. In primo luogo e ho esitato ad ammettere questo, l'applicazione Contact Manager non è l'applicazione più attraente. Ha bisogno di un po 'di lavoro di progettazione. Nell'iterazione successiva, vedremo come possiamo modificare la pagina master di visualizzazione predefinita e foglio di stile CSS per migliorare l'aspetto dell'applicazione.

In secondo luogo, non abbiamo implementato alcuna convalida del modulo. Ad esempio, non c'è nulla che impedisca di inviare il modulo Crea contatto senza immettere valori per nessuno dei campi del modulo. Inoltre, è possibile inserire numeri di telefono e indirizzi e-mail non validi. Iniziamo ad affrontare il problema della convalida del modulo in #3 di iterazione.

Infine, e soprattutto, l'iterazione corrente dell'applicazione Contact Manager non può essere facilmente modificata o mantenuta. Ad esempio, la logica di accesso al database viene inserita direttamente nelle azioni del controller. Ciò significa che non è possibile modificare il codice di accesso ai dati senza modificare i controller. Nelle iterazioni successive, esploriamo i modelli di progettazione software che possiamo implementare per rendere Contact Manager più resiliente alle modifiche.

> [!div class="step-by-step"]
> [Successivo](iteration-7-add-ajax-functionality-cs.md)
> [precedente](iteration-2-make-the-application-look-nice-vb.md)
