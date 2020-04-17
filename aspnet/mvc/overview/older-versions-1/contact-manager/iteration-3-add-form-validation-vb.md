---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: '#3 iterazione : aggiunta della convalida del form (VB) Documenti Microsoft'
author: rick-anderson
description: Nella terza iterazione, aggiungiamo la convalida di base del form. Impediamo alle persone di inviare un modulo senza compilare i campi modulo obbligatori. Convalidiamo anche emai...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 3ee2f40996873a7af2eaa255edd5f157c3fefb29
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542391"
---
# <a name="iteration-3--add-form-validation-vb"></a>Iterazione 3 - Aggiungere la convalida dei form (VB)

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il codice](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> Nella terza iterazione, aggiungiamo la convalida di base del form. Impediamo alle persone di inviare un modulo senza compilare i campi modulo obbligatori. Convalidiamo anche gli indirizzi e-mail e i numeri di telefono.

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

In questa seconda iterazione dell'applicazione Contact Manager, aggiungiamo la convalida di base del modulo. Impediamo alle persone di inviare un contatto senza immettere valori per i campi modulo obbligatori. Convalidiamo anche i numeri di telefono e gli indirizzi e-mail (vedere figura 1).

[![Finestra di dialogo relativa al nuovo progetto](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)

**Figura 01**: Un modulo con convalida ([Fare clic per visualizzare l'immagine](iteration-3-add-form-validation-vb/_static/image2.png)a dimensioni intere )

In questa iterazione, aggiungiamo la logica di convalida direttamente alle azioni del controller. In generale, questo non è il modo consigliato per aggiungere la convalida a un'applicazione MVC ASP.NET. Un approccio migliore consiste nell'inserire una logica di convalida s dell'applicazione in un [livello di servizio](http://martinfowler.com/eaaCatalog/serviceLayer.html)separato. Nell'iterazione successiva, viene eseguito il refactoring dell'applicazione Contact Manager per rendere l'applicazione più gestibile.

In questa iterazione, per mantenere le cose semplici, scriviamo tutto il codice di convalida a mano. Invece di scrivere il codice di convalida noi stessi, è possibile sfruttare un framework di convalida. Ad esempio, è possibile utilizzare Microsoft Enterprise Library Convalida Application Block (VAB) per implementare la logica di convalida per l'applicazione MVC ASP.NET. Per altre informazioni sul blocco dell'applicazione di convalida, vedere:To learn more about the Validation Application Block, see:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Aggiunta della convalida alla visualizzazione Crea

Let s iniziare aggiungendo la logica di convalida per il crea visualizzazione. Fortunatamente, poiché è stata generata la visualizzazione Crea con Visual Studio, la visualizzazione Crea contiene già tutta la logica dell'interfaccia utente necessaria per visualizzare i messaggi di convalida. La vista Crea è contenuta nel listato 1.

**Listato 1 - "Visualizzazioni" o "Contatto" per la creazione di file aspx**

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

La classe field-validation-error viene utilizzata per definire lo stile dell'output di cui è stato eseguito il rendering dall'helper Html.ValidationMessage(). La classe input-validation-error viene utilizzata per definire lo stile della casella di testo (input) di cui viene eseguito il rendering dall'helper Html.TextBox(). La classe validation-summary-errors viene utilizzata per definire lo stile dell'elenco non ordinato sottoposto a rendering dall'helper Html.ValidationSummary().

> [!NOTE] 
> 
> È possibile modificare le classi dei fogli di stile descritte in questa sezione per personalizzare l'aspetto dei messaggi di errore di convalida.

## <a name="adding-validation-logic-to-the-create-action"></a>Aggiunta della logica di convalida all'azione di creazioneAdding Validation Logic to the Create Action

Al momento, la visualizzazione Crea non visualizza mai i messaggi di errore di convalida perché non è stata scritta la logica per generare messaggi. Per visualizzare i messaggi di errore di convalida, è necessario aggiungere i messaggi di errore a ModelState.

> [!NOTE] 
> 
> Il metodo UpdateModel() aggiunge automaticamente messaggi di errore a ModelState quando si verifica un errore durante l'assegnazione del valore di un campo modulo a una proprietà. Ad esempio, se si tenta di assegnare la stringa "apple" a una proprietà BirthDate che accetta valori DateTime, il metodo UpdateModel() aggiunge un errore a ModelState.

Il metodo Create() modificato nel listato 2 contiene una nuova sezione che convalida le proprietà della classe Contact prima che il nuovo contatto venga inserito nel database.

**Listato 2 - Controller-ContactController.vb (Crea con convalida)**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

La sezione di convalida applica quattro regole di convalida distinte:The validate section enforces four distinct validation rules:

- La proprietà FirstName deve avere una lunghezza maggiore di zero (e non può essere costituita esclusivamente da spazi)
- La proprietà LastName deve avere una lunghezza maggiore di zero (e non può essere costituita esclusivamente da spazi)
- Se la proprietà Phone ha un valore (ha una lunghezza maggiore di 0), la proprietà Phone deve corrispondere a un'espressione regolare.
- Se la proprietà Email ha un valore (ha una lunghezza maggiore di 0), la proprietà Email deve corrispondere a un'espressione regolare.

Quando si verifica una violazione della regola di convalida, viene aggiunto un messaggio di errore a ModelState con l'aiuto del metodo AddModelError(). Quando si aggiunge un messaggio a ModelState, si specifica il nome di una proprietà e il testo di un messaggio di errore di convalida. Questo messaggio di errore viene visualizzato nella visualizzazione dai metodi helper Html.ValidationSummary() e Html.ValidationMessage().

Dopo l'esecuzione delle regole di convalida, viene controllata la proprietà IsValid di ModelState.After the validation rules are executed, the IsValid property of ModelState is checked. La proprietà IsValid restituisce false quando sono stati aggiunti messaggi di errore di convalida a ModelState. Se la convalida non riesce, il modulo Crea viene visualizzato nuovamente con i messaggi di errore.

> [!NOTE] 
> 
> Ho ottenuto le espressioni regolari per la convalida del numero di telefono e l'indirizzo e-mail dal repository delle espressioni regolari a[*http://regexlib.com*](http://regexlib.com)

## <a name="adding-validation-logic-to-the-edit-action"></a>Aggiunta della logica di convalida all'azione di modificaAdding Validation Logic to the Edit Action

L'azione Edit() aggiorna un contatto. L'azione Edit() deve eseguire esattamente la stessa convalida dell'azione Create(). Invece di duplicare lo stesso codice di convalida, è necessario eseguire il refactoring del controller di contatto in modo che entrambe le azioni Create() e Edit() chiamino lo stesso metodo di convalida.

La classe controller Contact modificata è contenuta nel listato 3. Questa classe dispone di un nuovo metodo ValidateContact() che viene chiamato all'interno delle azioni Create() e Edit().

**Listato 3 - Controllers-ContactController.vb**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a>Riepilogo

In questa iterazione è stata aggiunta la convalida di base del modulo all'applicazione Contact Manager. La logica di convalida impedisce agli utenti di inviare un nuovo contatto o modificare un contatto esistente senza fornire valori per il FirstName e LastName proprietà. Inoltre, gli utenti devono fornire numeri di telefono e indirizzi e-mail validi.

In questa iterazione, la logica di convalida è stata aggiunta all'applicazione Contact Manager nel modo più semplice possibile. Tuttavia, mescolando la logica di convalida nella logica del controller creerà problemi per noi a lungo termine. La nostra applicazione sarà più difficile da mantenere e modificare nel tempo.

Nell'iterazione successiva verrà effettuato il refactoring della logica di convalida e della logica di accesso al database dai controller. Approfitteremo di diversi principi di progettazione software per consentirci di creare un'applicazione più liberamente accoppiata e più gestibile.

> [!div class="step-by-step"]
> [Successivo](iteration-2-make-the-application-look-nice-vb.md)
> [precedente](iteration-4-make-the-application-loosely-coupled-vb.md)
