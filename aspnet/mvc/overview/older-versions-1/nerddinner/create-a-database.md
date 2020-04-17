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
# <a name="create-a-database"></a><span data-ttu-id="dff70-103">Creare un database</span><span class="sxs-lookup"><span data-stu-id="dff70-103">Create a Database</span></span>

<span data-ttu-id="dff70-104">da parte [di Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="dff70-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="dff70-105">Scarica il PDF</span><span class="sxs-lookup"><span data-stu-id="dff70-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="dff70-106">Questo è il passaggio 2 di un'esercitazione gratuita [sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come compilare un'applicazione Web piccola ma completa usando ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="dff70-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="dff70-107">Passaggio 2 illustra i passaggi per creare il database contenente tutti i dati di cena e RSVP per l'applicazione NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="dff70-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="dff70-108">Se si utilizza ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [ALL'archivio musicale MVC.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="dff70-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="dff70-109">Passaggio 2 di NerdDinner: Creazione del databaseNerdDinner Step 2: Creating the Database</span><span class="sxs-lookup"><span data-stu-id="dff70-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="dff70-110">Useremo un database per archiviare tutti i dati Dinner e RSVP per la nostra applicazione NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="dff70-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="dff70-111">La procedura seguente illustra la creazione del database utilizzando l'edizione gratuita di SQL Server Express (che è possibile installare facilmente utilizzando V2 [dell'Installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="dff70-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="dff70-112">Tutto il codice che scriveremo funziona sia con SQL Server Express che con l'intero SQL Server.</span><span class="sxs-lookup"><span data-stu-id="dff70-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="dff70-113">Creazione di un nuovo database di SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="dff70-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="dff70-114">Inizieremo facendo clic con il pulsante destro del mouse sul nostro progetto web, quindi selezionare il comando di menu **Aggiungi-&gt;Nuovo elemento:**</span><span class="sxs-lookup"><span data-stu-id="dff70-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="dff70-115">Verrà esuntela finestra di dialogo "Aggiungi nuovo elemento" di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dff70-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="dff70-116">Il filtro verrà filtrato in base alla categoria "Dati" e verrà selezionato il modello di elemento "Database di SQL Server":</span><span class="sxs-lookup"><span data-stu-id="dff70-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="dff70-117">Il database di SQL Server Express verrà denominato e verrà premuto ok.</span><span class="sxs-lookup"><span data-stu-id="dff70-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="dff70-118">Visual Studio ci chiederà quindi se si desidera\_aggiungere questo file alla nostra directory di dati app (che è una directory già impostata con ACL di sicurezza di lettura e scrittura):</span><span class="sxs-lookup"><span data-stu-id="dff70-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="dff70-119">Faremo clic su "Sì" e il nostro nuovo database verrà creato e aggiunto al nostro Esplora soluzioni:</span><span class="sxs-lookup"><span data-stu-id="dff70-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="dff70-120">Creazione di tabelle all'interno del database</span><span class="sxs-lookup"><span data-stu-id="dff70-120">Creating Tables within our Database</span></span>

<span data-ttu-id="dff70-121">Ora abbiamo un nuovo database vuoto.</span><span class="sxs-lookup"><span data-stu-id="dff70-121">We now have a new empty database.</span></span> <span data-ttu-id="dff70-122">Aggiungiamo alcune tabelle.</span><span class="sxs-lookup"><span data-stu-id="dff70-122">Let's add some tables to it.</span></span>

<span data-ttu-id="dff70-123">A tale scopo si passerà alla finestra della scheda "Esplora server" all'interno di Visual Studio, che consente di gestire database e server.</span><span class="sxs-lookup"><span data-stu-id="dff70-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="dff70-124">I database di SQL Server\_Express archiviati nella cartella dati dell'app dell'applicazione verranno visualizzati automaticamente all'interno di Esplora server.</span><span class="sxs-lookup"><span data-stu-id="dff70-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="dff70-125">Facoltativamente, è possibile utilizzare l'icona "Connetti al database" nella parte superiore della finestra "Esplora server" per aggiungere ulteriori database di SQL Server (sia locali che remoti) all'elenco:</span><span class="sxs-lookup"><span data-stu-id="dff70-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="dff70-126">Aggiungeremo due tabelle al nostro database NerdDinner: una per memorizzare le nostre cene e l'altra per tenere traccia delle accettazioni RSVP.</span><span class="sxs-lookup"><span data-stu-id="dff70-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="dff70-127">Possiamo creare nuove tabelle facendo clic con il pulsante destro del mouse sulla cartella "Tabelle" all'interno del nostro database e scegliendo il comando di menu "Aggiungi nuova tabella":</span><span class="sxs-lookup"><span data-stu-id="dff70-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="dff70-128">Verrà aperta una finestra di progettazione tabelle che consente di configurare lo schema della tabella.</span><span class="sxs-lookup"><span data-stu-id="dff70-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="dff70-129">Per la nostra tabella "Dinners" aggiungeremo 10 colonne di dati:</span><span class="sxs-lookup"><span data-stu-id="dff70-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="dff70-130">Vogliamo che la colonna "DinnerID" sia una chiave primaria univoca per la tabella.</span><span class="sxs-lookup"><span data-stu-id="dff70-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="dff70-131">Possiamo configurarlo facendo clic con il pulsante destro del mouse sulla colonna "DinnerID" e scegliendo la voce di menu "Imposta chiave primaria":</span><span class="sxs-lookup"><span data-stu-id="dff70-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="dff70-132">Oltre a rendere DinnerID una chiave primaria, vogliamo anche configurarla come una colonna "identity" il cui valore viene incrementato automaticamente man mano che vengono aggiunte nuove righe di dati alla tabella (ovvero la prima riga Dinner inserita avrà un DinnerID pari a 1, la seconda riga inserita avrà un DinnerID pari a 2 e così via).</span><span class="sxs-lookup"><span data-stu-id="dff70-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="dff70-133">Possiamo farlo selezionando la colonna "DinnerID" e quindi utilizzare l'editor "Proprietà colonna" per impostare la proprietà "(Identità Is)" nella colonna su "Sì".</span><span class="sxs-lookup"><span data-stu-id="dff70-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="dff70-134">Useremo le impostazioni predefinite dell'identità standard (inizia da 1 e incremento 1 su ogni nuova riga Dinner):</span><span class="sxs-lookup"><span data-stu-id="dff70-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="dff70-135">Verrà quindi salvata la tabella digitando Ctrl-S o utilizzando il comando di menu **&gt;File- Salva.**</span><span class="sxs-lookup"><span data-stu-id="dff70-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="dff70-136">Questo ci chiederà di nominare la tabella.</span><span class="sxs-lookup"><span data-stu-id="dff70-136">This will prompt us to name the table.</span></span> <span data-ttu-id="dff70-137">Lo chiameremo "Cena":</span><span class="sxs-lookup"><span data-stu-id="dff70-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="dff70-138">La nostra nuova tabella Dinners verrà quindi visualizzata all'interno del database in Esplora server.</span><span class="sxs-lookup"><span data-stu-id="dff70-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="dff70-139">Ripeteremo quindi i passaggi precedenti e creeremo una tabella "RSVP".</span><span class="sxs-lookup"><span data-stu-id="dff70-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="dff70-140">Questa tabella con 3 colonne.</span><span class="sxs-lookup"><span data-stu-id="dff70-140">This table with have 3 columns.</span></span> <span data-ttu-id="dff70-141">La colonna RsvpID verrà impostata come chiave primaria e la si renderà anche una colonna Identity:</span><span class="sxs-lookup"><span data-stu-id="dff70-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="dff70-142">Lo salveremo e gli daremo il nome "RSVP".</span><span class="sxs-lookup"><span data-stu-id="dff70-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="dff70-143">Impostazione di una relazione di chiave esterna tra tabelle</span><span class="sxs-lookup"><span data-stu-id="dff70-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="dff70-144">Ora abbiamo due tabelle all'interno del nostro database.</span><span class="sxs-lookup"><span data-stu-id="dff70-144">We now have two tables within our database.</span></span> <span data-ttu-id="dff70-145">L'ultimo passaggio di progettazione dello schema consiste nell'impostare una relazione "uno-a-molti" tra queste due tabelle, in modo da poter associare ogni riga Dinner a zero o più righe RSVP applicabili.</span><span class="sxs-lookup"><span data-stu-id="dff70-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="dff70-146">A tale scopo, configurare la colonna "DinnerID" della tabella RSVP per avere una relazione di chiave esterna con la colonna "DinnerID" nella tabella "Dinners".</span><span class="sxs-lookup"><span data-stu-id="dff70-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="dff70-147">A tale scopo, apriremo la tabella RSVP all'interno di Progettazione tabelle facendo doppio clic su di essa in Esplora server.</span><span class="sxs-lookup"><span data-stu-id="dff70-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="dff70-148">Quindi selezioniamo la colonna "DinnerID" all'interno di esso, fare clic con il pulsante destro del mouse e scegliere il "Relazioni..." comando del menu contestuale:</span><span class="sxs-lookup"><span data-stu-id="dff70-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationships…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="dff70-149">Verrà visualizzata una finestra di dialogo che è possibile utilizzare per impostare le relazioni tra le tabelle:This will bring up a dialog that we can use to setup relationships between tables:</span><span class="sxs-lookup"><span data-stu-id="dff70-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="dff70-150">Faremo clic sul pulsante "Aggiungi" per aggiungere una nuova relazione alla finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="dff70-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="dff70-151">Una volta aggiunta una relazione, si espanderà il nodo della visualizzazione struttura ad albero "Tabelle e specifiche di colonna" all'interno della griglia delle proprietà a destra della finestra di dialogo e quindi fare clic sul "..." a destra di esso:</span><span class="sxs-lookup"><span data-stu-id="dff70-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="dff70-152">Facendo clic sul pulsante "..." pulsante verrà visualizzata un'altra finestra di dialogo che ci permette di specificare quali tabelle e colonne sono coinvolte nella relazione, così come ci permettono di nominare la relazione.</span><span class="sxs-lookup"><span data-stu-id="dff70-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="dff70-153">La tabella della chiave primaria verrà modificata in "Dinners" e selezioneremo la colonna "DinnerID" all'interno della tabella Dinners come chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="dff70-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="dff70-154">Il nostro tavolo RSVP sarà la tabella di chiave esterna, e il RSVP. La colonna DinnerID verrà associata come chiave esterna:DinnerID column will be associated as the foreign-key:</span><span class="sxs-lookup"><span data-stu-id="dff70-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="dff70-155">A questo punto ogni riga nella tabella RSVP verrà associata a una riga nella tabella Dinner.</span><span class="sxs-lookup"><span data-stu-id="dff70-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="dff70-156">SQL Server manterrà l'integrità referenziale per noi e ci impedirà di aggiungere una nuova riga RSVP se non punta a una riga Dinner valida.</span><span class="sxs-lookup"><span data-stu-id="dff70-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="dff70-157">Ci impedirà anche di eliminare una riga Dinner se ci sono ancora righe RSVP che fanno riferimento ad essa.</span><span class="sxs-lookup"><span data-stu-id="dff70-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="dff70-158">Aggiunta di dati alle tabelle</span><span class="sxs-lookup"><span data-stu-id="dff70-158">Adding Data to our Tables</span></span>

<span data-ttu-id="dff70-159">Concludiamo aggiungendo alcuni dati di esempio alla tabella Dinners.Let's finish by adding some sample data to our Dinners table.</span><span class="sxs-lookup"><span data-stu-id="dff70-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="dff70-160">Possiamo aggiungere dati a una tabella facendo clic con il pulsante destro del mouse su di essa all'interno di Esplora server e scegliendo il comando "Mostra dati tabella":</span><span class="sxs-lookup"><span data-stu-id="dff70-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="dff70-161">Aggiungeremo alcune righe di dati Dinner che possiamo usare in seguito quando iniziamo a implementare l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="dff70-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="dff70-162">passaggio successivo</span><span class="sxs-lookup"><span data-stu-id="dff70-162">Next Step</span></span>

<span data-ttu-id="dff70-163">Abbiamo finito di creare il nostro database.</span><span class="sxs-lookup"><span data-stu-id="dff70-163">We've finished creating our database.</span></span> <span data-ttu-id="dff70-164">Creiamo ora le classi del modello che possiamo usare per eseguire query e aggiornarle.</span><span class="sxs-lookup"><span data-stu-id="dff70-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dff70-165">[Successivo](create-a-new-aspnet-mvc-project.md)
> [precedente](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="dff70-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
