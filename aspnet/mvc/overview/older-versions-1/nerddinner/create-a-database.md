---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Creare un database | Microsoft Docs
author: rick-anderson
description: Passaggio 2 illustra i passaggi per creare il database contenente tutti i dati di cena e RSVP per l'applicazione NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: d0b87e4a6a27b37d2dbaa6d5b871da477b25586d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541611"
---
# <a name="create-a-database"></a>Creare un database

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 2 di un'esercitazione gratuita [sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come compilare un'applicazione Web piccola ma completa usando ASP.NET MVC 1.
> 
> Passaggio 2 illustra i passaggi per creare il database contenente tutti i dati di cena e RSVP per l'applicazione NerdDinner.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [ALL'archivio musicale MVC.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-2-creating-the-database"></a>Passaggio 2 di NerdDinner: Creazione del databaseNerdDinner Step 2: Creating the Database

Useremo un database per archiviare tutti i dati Dinner e RSVP per la nostra applicazione NerdDinner.

La procedura seguente illustra la creazione del database utilizzando l'edizione gratuita di SQL Server Express (che è possibile installare facilmente utilizzando V2 [dell'Installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)). Tutto il codice che scriveremo funziona sia con SQL Server Express che con l'intero SQL Server.

### <a name="creating-a-new-sql-server-express-database"></a>Creazione di un nuovo database di SQL Server Express

Inizieremo facendo clic con il pulsante destro del mouse sul nostro progetto web, quindi selezionare il comando di menu **Aggiungi-&gt;Nuovo elemento:**

![](create-a-database/_static/image1.png)

Verrà esuntela finestra di dialogo "Aggiungi nuovo elemento" di Visual Studio. Il filtro verrà filtrato in base alla categoria "Dati" e verrà selezionato il modello di elemento "Database di SQL Server":

![](create-a-database/_static/image2.png)

Il database di SQL Server Express verrà denominato e verrà premuto ok. Visual Studio ci chiederà quindi se si desidera\_aggiungere questo file alla nostra directory di dati app (che è una directory già impostata con ACL di sicurezza di lettura e scrittura):

![](create-a-database/_static/image3.png)

Faremo clic su "Sì" e il nostro nuovo database verrà creato e aggiunto al nostro Esplora soluzioni:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Creazione di tabelle all'interno del database

Ora abbiamo un nuovo database vuoto. Aggiungiamo alcune tabelle.

A tale scopo si passerà alla finestra della scheda "Esplora server" all'interno di Visual Studio, che consente di gestire database e server. I database di SQL Server\_Express archiviati nella cartella dati dell'app dell'applicazione verranno visualizzati automaticamente all'interno di Esplora server. Facoltativamente, è possibile utilizzare l'icona "Connetti al database" nella parte superiore della finestra "Esplora server" per aggiungere ulteriori database di SQL Server (sia locali che remoti) all'elenco:

![](create-a-database/_static/image5.png)

Aggiungeremo due tabelle al nostro database NerdDinner: una per memorizzare le nostre cene e l'altra per tenere traccia delle accettazioni RSVP. Possiamo creare nuove tabelle facendo clic con il pulsante destro del mouse sulla cartella "Tabelle" all'interno del nostro database e scegliendo il comando di menu "Aggiungi nuova tabella":

![](create-a-database/_static/image6.png)

Verrà aperta una finestra di progettazione tabelle che consente di configurare lo schema della tabella. Per la nostra tabella "Dinners" aggiungeremo 10 colonne di dati:

![](create-a-database/_static/image7.png)

Vogliamo che la colonna "DinnerID" sia una chiave primaria univoca per la tabella. Possiamo configurarlo facendo clic con il pulsante destro del mouse sulla colonna "DinnerID" e scegliendo la voce di menu "Imposta chiave primaria":

![](create-a-database/_static/image8.png)

Oltre a rendere DinnerID una chiave primaria, vogliamo anche configurarla come una colonna "identity" il cui valore viene incrementato automaticamente man mano che vengono aggiunte nuove righe di dati alla tabella (ovvero la prima riga Dinner inserita avrà un DinnerID pari a 1, la seconda riga inserita avrà un DinnerID pari a 2 e così via).

Possiamo farlo selezionando la colonna "DinnerID" e quindi utilizzare l'editor "Proprietà colonna" per impostare la proprietà "(Identità Is)" nella colonna su "Sì". Useremo le impostazioni predefinite dell'identità standard (inizia da 1 e incremento 1 su ogni nuova riga Dinner):

![](create-a-database/_static/image9.png)

Verrà quindi salvata la tabella digitando Ctrl-S o utilizzando il comando di menu **&gt;File- Salva.** Questo ci chiederà di nominare la tabella. Lo chiameremo "Cena":

![](create-a-database/_static/image10.png)

La nostra nuova tabella Dinners verrà quindi visualizzata all'interno del database in Esplora server.

Ripeteremo quindi i passaggi precedenti e creeremo una tabella "RSVP". Questa tabella con 3 colonne. La colonna RsvpID verrà impostata come chiave primaria e la si renderà anche una colonna Identity:

![](create-a-database/_static/image11.png)

Lo salveremo e gli daremo il nome "RSVP".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Impostazione di una relazione di chiave esterna tra tabelle

Ora abbiamo due tabelle all'interno del nostro database. L'ultimo passaggio di progettazione dello schema consiste nell'impostare una relazione "uno-a-molti" tra queste due tabelle, in modo da poter associare ogni riga Dinner a zero o più righe RSVP applicabili. A tale scopo, configurare la colonna "DinnerID" della tabella RSVP per avere una relazione di chiave esterna con la colonna "DinnerID" nella tabella "Dinners".

A tale scopo, apriremo la tabella RSVP all'interno di Progettazione tabelle facendo doppio clic su di essa in Esplora server. Quindi selezioniamo la colonna "DinnerID" all'interno di esso, fare clic con il pulsante destro del mouse e scegliere il "Relazioni..." comando del menu contestuale:

![](create-a-database/_static/image12.png)

Verrà visualizzata una finestra di dialogo che è possibile utilizzare per impostare le relazioni tra le tabelle:This will bring up a dialog that we can use to setup relationships between tables:

![](create-a-database/_static/image13.png)

Faremo clic sul pulsante "Aggiungi" per aggiungere una nuova relazione alla finestra di dialogo. Una volta aggiunta una relazione, si espanderà il nodo della visualizzazione struttura ad albero "Tabelle e specifiche di colonna" all'interno della griglia delle proprietà a destra della finestra di dialogo e quindi fare clic sul "..." a destra di esso:

![](create-a-database/_static/image14.png)

Facendo clic sul pulsante "..." pulsante verrà visualizzata un'altra finestra di dialogo che ci permette di specificare quali tabelle e colonne sono coinvolte nella relazione, così come ci permettono di nominare la relazione.

La tabella della chiave primaria verrà modificata in "Dinners" e selezioneremo la colonna "DinnerID" all'interno della tabella Dinners come chiave primaria. Il nostro tavolo RSVP sarà la tabella di chiave esterna, e il RSVP. La colonna DinnerID verrà associata come chiave esterna:DinnerID column will be associated as the foreign-key:

![](create-a-database/_static/image15.png)

A questo punto ogni riga nella tabella RSVP verrà associata a una riga nella tabella Dinner. SQL Server manterrà l'integrità referenziale per noi e ci impedirà di aggiungere una nuova riga RSVP se non punta a una riga Dinner valida. Ci impedirà anche di eliminare una riga Dinner se ci sono ancora righe RSVP che fanno riferimento ad essa.

### <a name="adding-data-to-our-tables"></a>Aggiunta di dati alle tabelle

Concludiamo aggiungendo alcuni dati di esempio alla tabella Dinners.Let's finish by adding some sample data to our Dinners table. Possiamo aggiungere dati a una tabella facendo clic con il pulsante destro del mouse su di essa all'interno di Esplora server e scegliendo il comando "Mostra dati tabella":

![](create-a-database/_static/image16.png)

Aggiungeremo alcune righe di dati Dinner che possiamo usare in seguito quando iniziamo a implementare l'applicazione:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>passaggio successivo

Abbiamo finito di creare il nostro database. Creiamo ora le classi del modello che possiamo usare per eseguire query e aggiornarle.

> [!div class="step-by-step"]
> [Successivo](create-a-new-aspnet-mvc-project.md)
> [precedente](build-a-model-with-business-rule-validations.md)
