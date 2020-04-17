---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
title: "#4 di iterazione – Rendere l'applicazione ad accoppiamento libero (VB) Documenti Microsoft"
author: rick-anderson
description: In questa quarta iterazione, sfruttiamo diversi modelli di progettazione software per semplificare la manutenzione e la modifica dell'applicazione Contact Manager. Per...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 92c70297-4430-4e4e-919a-9c2333a8d09a
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
msc.type: authoredcontent
ms.openlocfilehash: 1026e307b7e498a8b1392f2907c08cdcd05a3199
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542768"
---
# <a name="iteration-4--make-the-application-loosely-coupled-vb"></a>Iterazione 4 - Rendere l'applicazione a regime di controllo libero (VB)

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il codice](iteration-4-make-the-application-loosely-coupled-vb/_static/contactmanager_4_vb1.zip)

> In questa quarta iterazione, sfruttiamo diversi modelli di progettazione software per semplificare la manutenzione e la modifica dell'applicazione Contact Manager. Ad esempio, viene eseguito il refactoring dell'applicazione per usare il modello Repository e il modello di inserimento delle dipendenze.

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

In questa quarta iterazione dell'applicazione Contact Manager, viene eseguito il refactoring dell'applicazione per rendere l'applicazione più liberamente accoppiata. Quando un'applicazione è ad accoppiamento ridotto, è possibile modificare il codice in una parte dell'applicazione senza dover modificare il codice in altre parti dell'applicazione. Le applicazioni ad accoppiamento ridotto sono più resilienti al cambiamento.

Attualmente, tutta la logica di convalida e accesso ai dati utilizzata dall'applicazione Contact Manager è contenuta nelle classi controller. È una pessima idea. Ogni volta che è necessario modificare una parte dell'applicazione, si rischia di introdurre bug in un'altra parte dell'applicazione. Ad esempio, se si modifica la logica di convalida, si rischia di introdurre nuovi bug nell'accesso ai dati o nella logica del controller.

> [!NOTE] 
> 
> (SRP), una classe non dovrebbe mai avere più di un motivo per cambiare. La combinazione di controller, convalida e logica di database è una violazione massiccia del principio di responsabilità singola.

Esistono diversi motivi per cui potrebbe essere necessario modificare l'applicazione. Potrebbe essere necessario aggiungere una nuova funzionalità all'applicazione, potrebbe essere necessario correggere un bug nell'applicazione o potrebbe essere necessario modificare la modalità di implementazione di una funzionalità dell'applicazione. Le applicazioni sono raramente statiche. Tendono a crescere e mutare nel tempo.

Si supponga, ad esempio, di decidere di modificare la modalità di implementazione del livello di accesso ai dati. A questo punto, l'applicazione Contact Manager utilizza Microsoft Entity Framework per accedere al database. Tuttavia, è possibile decidere di eseguire la migrazione a una tecnologia di accesso ai dati nuova o alternativa, ad esempio ADO.NET Data Services o NHibernate.However, you might decide to migrate to a new or alternative data access technology such as ADO.NET Data Services or NHibernate. Tuttavia, poiché il codice di accesso ai dati non è isolato dal codice di convalida e dal controller, non è possibile modificare il codice di accesso ai dati nell'applicazione senza modificare altro codice non direttamente correlato all'accesso ai dati.

Quando un'applicazione è ad accoppiamento libero, d'altra parte, è possibile apportare modifiche a una parte di un'applicazione senza toccare altre parti di un'applicazione. Ad esempio, è possibile cambiare le tecnologie di accesso ai dati senza modificare la logica di convalida o controller.

In questa iterazione, sfruttiamo diversi modelli di progettazione software che consentono di eseguire il refactoring dell'applicazione Contact Manager in un'applicazione più liberamente accoppiata. Quando abbiamo finito, il Contact Manager non farà nulla che non ha fatto prima. Tuttavia, saremo in grado di modificare l'applicazione più facilmente in futuro.

> [!NOTE] 
> 
> Il refactoring è il processo di riscrittura di un'applicazione in modo che non perda alcuna funzionalità esistente.

## <a name="using-the-repository-software-design-pattern"></a>Utilizzo del modello di progettazione del software del repository

La nostra prima modifica consiste nell'utilizzare un modello di progettazione software chiamato modello Repository. Utilizzeremo il modello Repository per isolare il codice di accesso ai dati dal resto dell'applicazione.

L'implementazione del modello Repository richiede di completare i due passaggi seguenti:Implementing the Repository pattern requires us to complete the following two steps:

1. Creare un'interfaccia
2. Creare una classe concreta che implementa l'interfacciaCreate a concrete class that implements the interface

In primo luogo, è necessario creare un'interfaccia che descrive tutti i metodi di accesso ai dati che è necessario eseguire. Il IContactManagerRepository interfaccia è contenuta nel listato 1.The IContactManagerRepository interface is contained in Listing 1. Questa interfaccia descrive cinque metodi: CreateContact(), DeleteContact(), EditContact(), GetContact e ListContacts().

**Listato 1 - Modelli: IContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample1.vb)]

Successivamente, è necessario creare una classe concreta che implementa il IContactManagerRepository interfaccia. Poiché si utilizza Microsoft Entity Framework per accedere al database, verrà creata una nuova classe denominata EntityContactManagerRepository.Because we are using the Microsoft Entity Framework to access the database, we'll create a new class named EntityContactManagerRepository. Questa classe è contenuta nel listato 2.This class is contained in Listing 2.

**Listato 2 - Modelli, EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample2.vb)]

Si noti che la classe EntityContactManagerRepository implementa l'interfaccia IContactManagerRepository. La classe implementa tutti e cinque i metodi descritti da tale interfaccia.

Ci si potrebbe chiedere perché abbiamo bisogno di preoccuparsi con un'interfaccia. Perché è necessario creare sia un'interfaccia che una classe che la implementa?

Con un'eccezione, il resto dell'applicazione interagirà con l'interfaccia e non con la classe concreta. Invece di chiamare i metodi esposti dalla classe EntityContactManagerRepository, chiameremo i metodi esposti dall'interfaccia IContactManagerRepository.

In questo modo, è possibile implementare l'interfaccia con una nuova classe senza dover modificare il resto dell'applicazione. Ad esempio, in una data futura, è possibile implementare un DataServicesContactManagerRepository classe che implementa il IContactManagerRepository interfaccia. La classe DataServicesContactManagerRepository potrebbe utilizzare ADO.NET Data Services per accedere a un database anziché a Microsoft Entity Framework.

Se il codice dell'applicazione viene programmato in base all'interfaccia IContactManagerRepository anziché alla classe EntityContactManagerRepository concreta, è possibile passare da classi concrete senza modificare il resto del codice. Ad esempio, è possibile passare dalla classe EntityContactManagerRepository alla classe DataServicesContactManagerRepository senza modificare la logica di accesso ai dati o di convalida.

La programmazione in base alle interfacce (astrazione) anziché alle classi concrete rende l'applicazione più resiliente alle modifiche.

> [!NOTE] 
> 
> È possibile creare rapidamente un'interfaccia da una classe concreta all'interno di Visual Studio selezionando l'opzione di menu Effettua refactoring, Estrai interfaccia. Ad esempio, è possibile creare prima la classe EntityContactManagerRepository e quindi utilizzare Estrai interfaccia per generare automaticamente l'interfaccia IContactManagerRepository.For example, you can create the EntityContactManagerRepository class first and then use Extract Interface to generate the IContactManagerRepository interface automatically.

## <a name="using-the-dependency-injection-software-design-pattern"></a>Utilizzo del modello di progettazione software di inserimento delle dipendenzeUsing the Dependency Injection Software Design Pattern

Ora che è stata eseguita la migrazione del codice di accesso ai dati in una classe Repository separata, è necessario modificare il controller di contatto per utilizzare questa classe. Si sfruttuorà di un modello di progettazione software chiamato Dependency Injection per utilizzare la classe Repository nel controller.

Il controller di contatto modificato è contenuto nel listato 3.

**Listato 3 - Controllers-ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample3.vb)]

Si noti che il controller di contatto nel listato 3 ha due costruttori. Il primo costruttore passa un'istanza concreta dell'interfaccia IContactManagerRepository al secondo costruttore. La classe del controller di contatto utilizza *l'inserimento di dipendenze del costruttore*.

L'unica e unica posizione in cui viene utilizzata la classe EntityContactManagerRepository è nel primo costruttore. Il resto della classe utilizza l'interfaccia IContactManagerRepository anziché la classe EntityContactManagerRepository concreta.

In questo modo è più semplice cambiare implementazioni della classe IContactManagerRepository in futuro. Se si desidera utilizzare la classe DataServicesContactRepository anziché la classe EntityContactManagerRepository, è sufficiente modificare il primo costruttore.

L'inserimento di dipendenze del costruttore rende inoltre la classe controller Contact molto tepossibile. Negli unit test è possibile creare un'istanza del controller di contatto passando un'implementazione fittizia della classe IContactManagerRepository. Questa funzionalità di Dependency Injection sarà molto importante per noi nella prossima iterazione quando si compila unit test per l'applicazione Contact Manager.

> [!NOTE] 
> 
> Se si desidera separare completamente la classe controller di contatto da una particolare implementazione dell'interfaccia IContactManagerRepository, è possibile sfruttare un framework che supporta l'inserimento delle dipendenze, ad esempio StructureMap o Microsoft Entity Framework (MEF). Sfruttando un framework di inserimento delle dipendenze, non è mai necessario fare riferimento a una classe concreta nel codice.

## <a name="creating-a-service-layer"></a>Creazione di un livello di servizioCreating a Service Layer

Potresti aver notato che la logica di convalida è ancora mescolata con la logica del controller nella classe controller modificata nel listato 3. Per lo stesso motivo per cui è consigliabile isolare la logica di accesso ai dati, è consigliabile isolare la logica di convalida.

Per risolvere questo problema, è possibile creare un livello di [servizio](http://martinfowler.com/eaaCatalog/serviceLayer.html)separato . Il livello di servizio è un livello separato che è possibile inserire tra le classi di controller e repository. Il livello di servizio contiene la logica di business, inclusa tutta la logica di convalida.

ContactManagerService è contenuto nel listato 4. Contiene la logica di convalida dalla classe del controller di contatto.

**Listato 4 - Modelli, ContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample4.vb)]

Si noti che il costruttore per il ContactManagerService richiede un ValidationDictionary. The service layer communicates with the controller layer through this ValidationDictionary. Discutereil ValidationDictionary in dettaglio nella sezione seguente quando si discute il modello Decorator.We discuss the ValidationDictionary in detail in the following section when we discuss the Decorator pattern.

Si noti, inoltre, che il ContactManagerService implementa il IContactManagerService interfaccia. Si dovrebbe sempre cercare di programmare contro le interfacce invece di classi concrete. Altre classi nell'applicazione Contact Manager non interagiscono direttamente con la classe ContactManagerService. Al contrario, con un'eccezione, il resto dell'applicazione Contact Manager viene programmato in base all'interfaccia IContactManagerService.

L'interfaccia IContactManagerService è contenuta nel listato 5.

**Listato 5 - Modelli: IContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample5.vb)]

La classe controller Contact modificata è contenuta nel listato 6. Si noti che il controller di contatto non interagisce più con il repository ContactManager. Al contrario, il controller di contatto interagisce con il servizio ContactManager. Ogni livello è isolato il più possibile dagli altri livelli.

**Listato 6 - Controllers-ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample6.vb)]

La nostra applicazione non è più in contrasto con il Principio di Responsabilità Singola (SRP). Il controller di contatto nel listato 6 è stato rimosso da ogni responsabilità diversa dal controllo del flusso di esecuzione dell'applicazione. Tutta la logica di convalida è stata rimossa dal controller di contatto e inserita nel livello di servizio. Tutta la logica del database è stata inserita nel livello del repository.

## <a name="using-the-decorator-pattern"></a>Utilizzo del modello DecoratorUsing the Decorator Pattern

Vogliamo essere in grado di disaccoppiare completamente il nostro livello di servizio dal nostro livello di controller. In linea di principio, dovremmo essere in grado di compilare il livello di servizio in un assembly separato dal livello del controller senza dover aggiungere un riferimento all'applicazione MVC.

Tuttavia, il livello di servizio deve essere in grado di passare i messaggi di errore di convalida al livello del controller. Come possiamo consentire al livello di servizio di comunicare messaggi di errore di convalida senza accantare il controller e il livello di servizio? È possibile sfruttare un modello di progettazione software denominato [modello Decorator](http://en.wikipedia.org/wiki/Decorator_pattern).

Un controller utilizza un ModelStateDictionary denominato ModelState per rappresentare gli errori di convalida. Pertanto, si potrebbe essere tentati di passare ModelState dal livello di controller al livello di servizio. Tuttavia, l'utilizzo di ModelState nel livello di servizio renderebbe il livello di servizio dipendente da una funzionalità del framework MVC ASP.NET. Questo sarebbe male perché, un giorno, si potrebbe desiderare di utilizzare il livello di servizio con un'applicazione WPF anziché un'applicazione MVC ASP.NET. In tal caso, non si desidera fare riferimento al framework MVC ASP.NET per utilizzare la classe ModelStateDictionary.

Il modello Decorator consente di eseguire il wrapping di una classe esistente in una nuova classe per implementare un'interfaccia. Il progetto Contact Manager include la classe ModelStateWrapper contenuta nel listato 7. La classe ModelStateWrapper implementa l'interfaccia nel listato 8.

**Listato 7 - Modelli, Convalida, ModelStateWrapper.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample7.vb)]

**Listato 8 - Modelli, Convalida, IValidationDictionary.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample8.vb)]

Se si esamina da vicino il listato 5, si noterà che il livello di servizio ContactManager utilizza esclusivamente l'interfaccia IValidationDictionary. Il servizio ContactManager non dipende dalla classe ModelStateDictionary. Quando il controller di contatto crea il servizio ContactManager, il controller esegue il wrapping di ModelState in questo modo:When the Contact controller creates the ContactManager service, the controller wraps its ModelState like this:

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample9.vb)]

## <a name="summary"></a>Riepilogo

In questa iterazione, non sono stati aggiunte nuove funzionalità all'applicazione Contact Manager. L'obiettivo di questa iterazione era di eseguire il refactoring dell'applicazione Contact Manager in modo che sia più facile da gestire e modificare.

In primo luogo, abbiamo implementato il modello di progettazione del software Repository. È stata eseguita la migrazione di tutto il codice di accesso ai dati in una classe di repository ContactManager separata.

Abbiamo anche isolato la logica di convalida dalla logica del controller. È stato creato un livello di servizio separato che contiene tutto il codice di convalida. Il livello controller interagisce con il livello di servizio e il livello di servizio interagisce con il livello del repository.

Quando abbiamo creato il livello di servizio, abbiamo sfruttato il modello Decorator per isolare ModelState dal livello di servizio. Nel nostro livello di servizio, abbiamo programmato in base all'interfaccia IValidationDictionary anziché ModelState.In our service layer, we programd against the IValidationDictionary interface instead of ModelState.

Infine, abbiamo sfruttato un modello di progettazione software denominato modello di inserimento delle dipendenze. Questo modello consente di eseguire programmi in base alle interfacce (astrazione) anziché alle classi concrete. L'implementazione del modello di progettazione Dependency Injection rende inoltre il codice più tepotebile. Nell'iterazione successiva, aggiungiamo unit test al progetto.

> [!div class="step-by-step"]
> [Successivo](iteration-3-add-form-validation-vb.md)
> [precedente](iteration-5-create-unit-tests-vb.md)
