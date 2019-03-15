---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form - parte 7 | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET utilizzando Entity Framework. L'applicazione di esempio è...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: 48092838ac0b00137ae0a4e37391c31883afcd09
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57027918"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a><span data-ttu-id="cb503-104">Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form - parte 7</span><span class="sxs-lookup"><span data-stu-id="cb503-104">Getting Started with Entity Framework 4.0 Database First and ASP.NET 4 Web Forms - Part 7</span></span>
====================
<span data-ttu-id="cb503-105">da [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="cb503-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="cb503-106">L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4.0 e Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="cb503-106">The Contoso University sample web application demonstrates how to create ASP.NET Web Forms applications using the Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="cb503-107">Per informazioni sulla serie di esercitazioni, vedere [la prima esercitazione della serie](the-entity-framework-and-aspnet-getting-started-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="cb503-107">For information about the tutorial series, see [the first tutorial in the series](the-entity-framework-and-aspnet-getting-started-part-1.md)</span></span>


## <a name="using-stored-procedures"></a><span data-ttu-id="cb503-108">Utilizzo delle stored procedure</span><span class="sxs-lookup"><span data-stu-id="cb503-108">Using Stored Procedures</span></span>

<span data-ttu-id="cb503-109">Nell'esercitazione precedente è implementato un modello di ereditarietà tabella per gerarchia.</span><span class="sxs-lookup"><span data-stu-id="cb503-109">In the previous tutorial you implemented a table-per-hierarchy inheritance pattern.</span></span> <span data-ttu-id="cb503-110">Questa esercitazione illustrerà come utilizzare le stored procedure per controllare ulteriormente l'accesso al database.</span><span class="sxs-lookup"><span data-stu-id="cb503-110">This tutorial will show you how to use stored procedures to gain more control over database access.</span></span>

<span data-ttu-id="cb503-111">Entity Framework consente di specificare che è necessario utilizzare stored procedure per l'accesso al database.</span><span class="sxs-lookup"><span data-stu-id="cb503-111">The Entity Framework lets you specify that it should use stored procedures for database access.</span></span> <span data-ttu-id="cb503-112">Per qualsiasi tipo di entità, è possibile specificare una stored procedure da utilizzare per la creazione, aggiornamento o l'eliminazione delle entità di quel tipo.</span><span class="sxs-lookup"><span data-stu-id="cb503-112">For any entity type, you can specify a stored procedure to use for creating, updating, or deleting entities of that type.</span></span> <span data-ttu-id="cb503-113">Nel modello di dati è quindi possibile aggiungere riferimenti alle stored procedure che è possibile usare per eseguire attività quali il recupero di set di entità.</span><span class="sxs-lookup"><span data-stu-id="cb503-113">Then in the data model you can add references to stored procedures that you can use to perform tasks such as retrieving sets of entities.</span></span>

<span data-ttu-id="cb503-114">Utilizzo delle stored procedure è un requisito comune per l'accesso al database.</span><span class="sxs-lookup"><span data-stu-id="cb503-114">Using stored procedures is a common requirement for database access.</span></span> <span data-ttu-id="cb503-115">In alcuni casi un amministratore del database può richiedere che l'accesso al database passano attraverso le stored procedure per motivi di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="cb503-115">In some cases a database administrator may require that all database access go through stored procedures for security reasons.</span></span> <span data-ttu-id="cb503-116">In altri casi è possibile compilare una logica di business in alcuni dei processi di Entity Framework viene utilizzato quando si aggiorna il database.</span><span class="sxs-lookup"><span data-stu-id="cb503-116">In other cases you may want to build business logic into some of the processes that the Entity Framework uses when it updates the database.</span></span> <span data-ttu-id="cb503-117">Ad esempio, ogni volta che viene eliminata un'entità è possibile copiarlo in un database di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="cb503-117">For example, whenever an entity is deleted you might want to copy it to an archive database.</span></span> <span data-ttu-id="cb503-118">O ogni volta che viene aggiornata una riga si potrebbe voler scrivere una riga della tabella di registrazione che registra che ha apportato la modifica.</span><span class="sxs-lookup"><span data-stu-id="cb503-118">Or whenever a row is updated you might want to write a row to a logging table that records who made the change.</span></span> <span data-ttu-id="cb503-119">È possibile eseguire questi tipi di attività in una stored procedure che viene chiamata ogni volta che Entity Framework elimina un'entità o aggiorna un'entità.</span><span class="sxs-lookup"><span data-stu-id="cb503-119">You can perform these kinds of tasks in a stored procedure that's called whenever the Entity Framework deletes an entity or updates an entity.</span></span>

<span data-ttu-id="cb503-120">Come nell'esercitazione precedente, si creerà non tutte le nuove pagine.</span><span class="sxs-lookup"><span data-stu-id="cb503-120">As in the previous tutorial, you'll not create any new pages.</span></span> <span data-ttu-id="cb503-121">Al contrario, sarà necessario modificare il modo in cui che Entity Framework accede al database per alcune delle pagine è già stato creato.</span><span class="sxs-lookup"><span data-stu-id="cb503-121">Instead, you'll change the way the Entity Framework accesses the database for some of the pages you already created.</span></span>

<span data-ttu-id="cb503-122">In questa esercitazione si creerà le stored procedure nel database per l'inserimento `Student` e `Instructor` entità.</span><span class="sxs-lookup"><span data-stu-id="cb503-122">In this tutorial you'll create stored procedures in the database for inserting `Student` and `Instructor` entities.</span></span> <span data-ttu-id="cb503-123">È possibile aggiungerli al modello di dati e si specificano che Entity Framework devono usarli per l'aggiunta `Student` e `Instructor` entità al database.</span><span class="sxs-lookup"><span data-stu-id="cb503-123">You'll add them to the data model, and you'll specify that the Entity Framework should use them for adding `Student` and `Instructor` entities to the database.</span></span> <span data-ttu-id="cb503-124">Si creerà inoltre una stored procedure che è possibile usare per recuperare `Course` entità.</span><span class="sxs-lookup"><span data-stu-id="cb503-124">You'll also create a stored procedure that you can use to retrieve `Course` entities.</span></span>

## <a name="creating-stored-procedures-in-the-database"></a><span data-ttu-id="cb503-125">Creazione di Stored procedure nel Database</span><span class="sxs-lookup"><span data-stu-id="cb503-125">Creating Stored Procedures in the Database</span></span>

<span data-ttu-id="cb503-126">(Se si usa la *School. mdf* file dal progetto disponibile per il download in questa esercitazione, è possibile ignorare questa sezione perché le stored procedure esistano già.)</span><span class="sxs-lookup"><span data-stu-id="cb503-126">(If you're using the *School.mdf* file from the project available for download with this tutorial, you can skip this section because the stored procedures already exist.)</span></span>

<span data-ttu-id="cb503-127">Nelle **Esplora Server**, espandere *School. mdf*, fare doppio clic su **Stored Procedures**, selezionare **Aggiungi nuova Stored Procedure**.</span><span class="sxs-lookup"><span data-stu-id="cb503-127">In **Server Explorer**, expand *School.mdf*, right-click **Stored Procedures**, and select **Add New Stored Procedure**.</span></span>

<span data-ttu-id="cb503-128">[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cb503-128">[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)</span></span>

<span data-ttu-id="cb503-129">Copiare le istruzioni SQL seguenti e incollarle nella finestra della stored procedure, sostituendo la stored procedure scheletro.</span><span class="sxs-lookup"><span data-stu-id="cb503-129">Copy the following SQL statements and paste them into the stored procedure window, replacing the skeleton stored procedure.</span></span>

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

<span data-ttu-id="cb503-130">[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="cb503-130">[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)</span></span>

<span data-ttu-id="cb503-131">`Student` le entità hanno quattro proprietà: `PersonID`, `LastName`, `FirstName`, e `EnrollmentDate`.</span><span class="sxs-lookup"><span data-stu-id="cb503-131">`Student` entities have four properties: `PersonID`, `LastName`, `FirstName`, and `EnrollmentDate`.</span></span> <span data-ttu-id="cb503-132">Il database genera automaticamente il valore ID e la stored procedure accetta parametri per le altre tre.</span><span class="sxs-lookup"><span data-stu-id="cb503-132">The database generates the ID value automatically, and the stored procedure accepts parameters for the other three.</span></span> <span data-ttu-id="cb503-133">La stored procedure restituisce il valore della chiave del record della nuova riga in modo che Entity Framework può tenere traccia di che nella versione dell'entità che conserva in memoria.</span><span class="sxs-lookup"><span data-stu-id="cb503-133">The stored procedure returns the value of the new row's record key so that the Entity Framework can keep track of that in the version of the entity it keeps in memory.</span></span>

<span data-ttu-id="cb503-134">Salvare e chiudere la finestra di stored procedure.</span><span class="sxs-lookup"><span data-stu-id="cb503-134">Save and close the stored procedure window.</span></span>

<span data-ttu-id="cb503-135">Creare un `InsertInstructor` stored procedure allo stesso modo, usando le istruzioni SQL seguenti:</span><span class="sxs-lookup"><span data-stu-id="cb503-135">Create an `InsertInstructor` stored procedure in the same manner, using the following SQL statements:</span></span>

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

<span data-ttu-id="cb503-136">Creare `Update` stored procedure per il `Student` e `Instructor` entità anche.</span><span class="sxs-lookup"><span data-stu-id="cb503-136">Create `Update` stored procedures for `Student` and `Instructor` entities also.</span></span> <span data-ttu-id="cb503-137">(Il database esiste già un `DeletePerson` stored procedure che funzionerà per entrambe `Instructor` e `Student` entities.)</span><span class="sxs-lookup"><span data-stu-id="cb503-137">(The database already has a `DeletePerson` stored procedure which will work for both `Instructor` and `Student` entities.)</span></span>

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

<span data-ttu-id="cb503-138">In questa esercitazione verrà eseguito il mapping tutte le tre funzioni: insert, update e delete, per ogni tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="cb503-138">In this tutorial you'll map all three functions -- insert, update, and delete -- for each entity type.</span></span> <span data-ttu-id="cb503-139">Consente a Entity Framework versione 4 è possibile mappare solo uno o due di queste funzioni alle stored procedure senza mapping di altri utenti, con una sola eccezione: se si esegue il mapping della funzione di aggiornamento, ma non della funzione di eliminazione, Entity Framework genererà un'eccezione quando si tentativo di eliminare un'entità.</span><span class="sxs-lookup"><span data-stu-id="cb503-139">The Entity Framework version 4 allows you to map just one or two of these functions to stored procedures without mapping the others, with one exception: if you map the update function but not the delete function, the Entity Framework will throw an exception when you attempt to delete an entity.</span></span> <span data-ttu-id="cb503-140">In Entity Framework versione 3.5, non è stato simile flessibilità nel mapping delle stored procedure: se è stata associata una funzione era necessario eseguire il mapping di tutte e tre.</span><span class="sxs-lookup"><span data-stu-id="cb503-140">In the Entity Framework version 3.5, you did not have this much flexibility in mapping stored procedures: if you mapped one function you were required to map all three.</span></span>

<span data-ttu-id="cb503-141">Per creare una stored procedure che legge piuttosto che aggiorna i dati, crearne uno che seleziona tutti gli elementi `Course` entità, usando le istruzioni SQL seguenti:</span><span class="sxs-lookup"><span data-stu-id="cb503-141">To create a stored procedure that reads rather than updates data, create one that selects all `Course` entities, using the following SQL statements:</span></span>

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a><span data-ttu-id="cb503-142">Aggiunta delle Stored procedure al modello di dati</span><span class="sxs-lookup"><span data-stu-id="cb503-142">Adding the Stored Procedures to the Data Model</span></span>

<span data-ttu-id="cb503-143">Le stored procedure vengono ora definite nel database, ma devono essere aggiunti al modello di dati per renderli disponibili per Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cb503-143">The stored procedures are now defined in the database, but they must be added to the data model to make them available to the Entity Framework.</span></span> <span data-ttu-id="cb503-144">Aprire *SchoolModel*, fare doppio clic nell'area di progettazione e selezionare **Aggiorna modello da Database**.</span><span class="sxs-lookup"><span data-stu-id="cb503-144">Open *SchoolModel.edmx*, right-click the design surface, and select **Update Model from Database**.</span></span> <span data-ttu-id="cb503-145">Nel **Add** scheda della finestra di **Scegli oggetti di Database** finestra di dialogo, espandere **Stored procedure**, selezionare le stored procedure appena create e il `DeletePerson` stored procedure e quindi fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="cb503-145">In the **Add** tab of the **Choose Your Database Objects** dialog box, expand **Stored Procedures**, select the newly created stored procedures and the `DeletePerson` stored procedure, and then click **Finish**.</span></span>

<span data-ttu-id="cb503-146">[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="cb503-146">[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)</span></span>

## <a name="mapping-the-stored-procedures"></a><span data-ttu-id="cb503-147">Mapping delle Stored procedure</span><span class="sxs-lookup"><span data-stu-id="cb503-147">Mapping the Stored Procedures</span></span>

<span data-ttu-id="cb503-148">In Progettazione modelli di dati, fare doppio clic il `Student` entità e selezionare **Mapping Stored Procedure**.</span><span class="sxs-lookup"><span data-stu-id="cb503-148">In the data model designer, right-click the `Student` entity and select **Stored Procedure Mapping**.</span></span>

<span data-ttu-id="cb503-149">[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="cb503-149">[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)</span></span>

<span data-ttu-id="cb503-150">Il **Dettagli Mapping** viene visualizzata la finestra in cui è possibile specificare le stored procedure che Entity Framework devono usare per l'inserimento, aggiornamento ed eliminazione di entità di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="cb503-150">The **Mapping Details** window appears, in which you can specify stored procedures that the Entity Framework should use for inserting, updating, and deleting entities of this type.</span></span>

<span data-ttu-id="cb503-151">[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="cb503-151">[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)</span></span>

<span data-ttu-id="cb503-152">Impostare il **inserire** funzione **InsertStudent**.</span><span class="sxs-lookup"><span data-stu-id="cb503-152">Set the **Insert** function to **InsertStudent**.</span></span> <span data-ttu-id="cb503-153">La finestra Mostra un elenco di parametri delle stored procedure, ognuno dei quali deve essere mappato a una proprietà di entità.</span><span class="sxs-lookup"><span data-stu-id="cb503-153">The window shows a list of stored procedure parameters, each of which must be mapped to an entity property.</span></span> <span data-ttu-id="cb503-154">Due di queste vengono mappate automaticamente perché i nomi sono uguali.</span><span class="sxs-lookup"><span data-stu-id="cb503-154">Two of these are mapped automatically because the names are the same.</span></span> <span data-ttu-id="cb503-155">È presente nessuna proprietà di entità denominata `FirstName`, pertanto è necessario selezionare manualmente `FirstMidName` da un elenco di riepilogo che mostra le proprietà delle entità disponibili.</span><span class="sxs-lookup"><span data-stu-id="cb503-155">There's no entity property named `FirstName`, so you must manually select `FirstMidName` from a drop-down list that shows available entity properties.</span></span> <span data-ttu-id="cb503-156">(Infatti, è stato modificato il nome del `FirstName` proprietà `FirstMidName` nella prima esercitazione.)</span><span class="sxs-lookup"><span data-stu-id="cb503-156">(This is because you changed the name of the `FirstName` property to `FirstMidName` in the first tutorial.)</span></span>

<span data-ttu-id="cb503-157">[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="cb503-157">[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)</span></span>

<span data-ttu-id="cb503-158">Nella stessa **Dettagli Mapping** (finestra), mappa il `Update` funzione per il `UpdateStudent` stored procedure di (assicurarsi di specificare `FirstMidName` come valore del parametro per `FirstName`, come fatto in precedenza per il `Insert` stored procedure) e il `Delete` funzione per il `DeletePerson` stored procedure.</span><span class="sxs-lookup"><span data-stu-id="cb503-158">In the same **Mapping Details** window, map the `Update` function to the `UpdateStudent` stored procedure (make sure you specify `FirstMidName` as the parameter value for `FirstName`, as you did for the `Insert` stored procedure) and the `Delete` function to the `DeletePerson` stored procedure.</span></span>

<span data-ttu-id="cb503-159">[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="cb503-159">[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)</span></span>

<span data-ttu-id="cb503-160">Seguire la stessa procedura per eseguire il mapping di insert, update e delete di stored procedure per instructors (insegnanti) per il `Instructor` entità.</span><span class="sxs-lookup"><span data-stu-id="cb503-160">Follow the same procedure to map the insert, update, and delete stored procedures for instructors to the `Instructor` entity.</span></span>

<span data-ttu-id="cb503-161">[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="cb503-161">[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)</span></span>

<span data-ttu-id="cb503-162">Per le stored procedure che leggono invece di aggiornare i dati, si utilizza il **Browser modello** restituisce digitarlo finestra per eseguire il mapping della stored procedure per l'entità.</span><span class="sxs-lookup"><span data-stu-id="cb503-162">For stored procedures that read rather than update data, you use the **Model Browser** window to map the stored procedure to the entity type it returns.</span></span> <span data-ttu-id="cb503-163">In Progettazione modelli di dati, fare doppio clic su area di progettazione e seleziona **Browser modello**.</span><span class="sxs-lookup"><span data-stu-id="cb503-163">In the data model designer, right-click the design surface and select **Model Browser**.</span></span> <span data-ttu-id="cb503-164">Aprire il **SchoolModel. Store** nodo, quindi aprire il **Stored Procedures** nodo.</span><span class="sxs-lookup"><span data-stu-id="cb503-164">Open the **SchoolModel.Store** node and then open the **Stored Procedures** node.</span></span> <span data-ttu-id="cb503-165">Quindi scegliere il `GetCourses` stored procedure e selezionare **Aggiungi importazione di funzioni**.</span><span class="sxs-lookup"><span data-stu-id="cb503-165">Then right-click the `GetCourses` stored procedure and select **Add Function Import**.</span></span>

<span data-ttu-id="cb503-166">[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="cb503-166">[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)</span></span>

<span data-ttu-id="cb503-167">Nel **Aggiungi importazione di funzioni** nella finestra di dialogo **restituisce una raccolta di** seleziona **entità**e quindi selezionare `Course` come il tipo di entità restituite.</span><span class="sxs-lookup"><span data-stu-id="cb503-167">In the **Add Function Import** dialog box, under **Returns a Collection Of** select **Entities**, and then select `Course` as the entity type returned.</span></span> <span data-ttu-id="cb503-168">Al termine, fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="cb503-168">When you're done, click **OK**.</span></span> <span data-ttu-id="cb503-169">Salvare e chiudere il *edmx* file.</span><span class="sxs-lookup"><span data-stu-id="cb503-169">Save and close the *.edmx* file.</span></span>

<span data-ttu-id="cb503-170">[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="cb503-170">[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)</span></span>

## <a name="using-insert-update-and-delete-stored-procedures"></a><span data-ttu-id="cb503-171">Utilizza inserimento, aggiornamento ed eliminazione di Stored procedure</span><span class="sxs-lookup"><span data-stu-id="cb503-171">Using Insert, Update, and Delete Stored Procedures</span></span>

<span data-ttu-id="cb503-172">Le stored procedure per inserire, aggiornare ed eliminare i dati vengono usati da Entity Framework automaticamente dopo aver aggiunte al modello di dati e li mappati alle entità appropriata.</span><span class="sxs-lookup"><span data-stu-id="cb503-172">Stored procedures to insert, update, and delete data are used by the Entity Framework automatically after you've added them to the data model and mapped them to the appropriate entities.</span></span> <span data-ttu-id="cb503-173">È ora possibile eseguire la *StudentsAdd.aspx* pagina, e ogni volta che si crea un nuovo studente, Entity Framework userà il `InsertStudent` stored procedure per aggiungere la nuova riga per il `Student` tabella.</span><span class="sxs-lookup"><span data-stu-id="cb503-173">You can now run the *StudentsAdd.aspx* page, and every time you create a new student, the Entity Framework will use the `InsertStudent` stored procedure to add the new row to the `Student` table.</span></span>

<span data-ttu-id="cb503-174">[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="cb503-174">[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)</span></span>

<span data-ttu-id="cb503-175">Eseguire la *Students.aspx* pagina e il nuovo studente viene visualizzato nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="cb503-175">Run the *Students.aspx* page and the new student appears in the list.</span></span>

<span data-ttu-id="cb503-176">[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="cb503-176">[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)</span></span>

<span data-ttu-id="cb503-177">Modificare il nome da verificare il corretto funzionamento della funzione di aggiornamento e quindi eliminare lo studente per verificare il corretto funzionamento della funzione di eliminazione.</span><span class="sxs-lookup"><span data-stu-id="cb503-177">Change the name to verify that the update function works, and then delete the student to verify that the delete function works.</span></span>

<span data-ttu-id="cb503-178">[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="cb503-178">[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)</span></span>

## <a name="using-select-stored-procedures"></a><span data-ttu-id="cb503-179">Utilizzo delle Stored procedure Select</span><span class="sxs-lookup"><span data-stu-id="cb503-179">Using Select Stored Procedures</span></span>

<span data-ttu-id="cb503-180">Entity Framework non esegue automaticamente le stored procedure, ad esempio `GetCourses`, e non possono essere utilizzati con il `EntityDataSource` controllo.</span><span class="sxs-lookup"><span data-stu-id="cb503-180">The Entity Framework does not automatically run stored procedures such as `GetCourses`, and you cannot use them with the `EntityDataSource` control.</span></span> <span data-ttu-id="cb503-181">A questo scopo, si chiamarli dal codice.</span><span class="sxs-lookup"><span data-stu-id="cb503-181">To use them, you call them from code.</span></span>

<span data-ttu-id="cb503-182">Aprire il *InstructorsCourses.aspx.cs* file.</span><span class="sxs-lookup"><span data-stu-id="cb503-182">Open the *InstructorsCourses.aspx.cs* file.</span></span> <span data-ttu-id="cb503-183">Il `PopulateDropDownLists` metodo utilizza una query LINQ-a-entità per recuperare tutte le entità course in modo che possa scorrere l'elenco in ciclo e determinare quali un insegnante è assegnato a e quelle che sono assegnate:</span><span class="sxs-lookup"><span data-stu-id="cb503-183">The `PopulateDropDownLists` method uses a LINQ-to-Entities query to retrieve all course entities so that it can loop through the list and determine which ones an instructor is assigned to and which ones are unassigned:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

<span data-ttu-id="cb503-184">Sostituire con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="cb503-184">Replace this with the following code:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

<span data-ttu-id="cb503-185">La pagina Usa ora la `GetCourses` stored procedure per recuperare l'elenco di tutti i corsi.</span><span class="sxs-lookup"><span data-stu-id="cb503-185">The page now uses the `GetCourses` stored procedure to retrieve the list of all courses.</span></span> <span data-ttu-id="cb503-186">Eseguire la pagina per verificarne il funzionamento come in precedenza.</span><span class="sxs-lookup"><span data-stu-id="cb503-186">Run the page to verify that it works as it did before.</span></span>

<span data-ttu-id="cb503-187">(Le proprietà di navigazione di entità recuperata da una stored procedure potrebbero non venire popolate automaticamente con i dati relativi a tali entità, a seconda `ObjectContext` impostazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="cb503-187">(Navigation properties of entities retrieved by a stored procedure might not be automatically populated with the data related to those entities, depending on `ObjectContext` default settings.</span></span> <span data-ttu-id="cb503-188">Per altre informazioni, vedere [caricamento di oggetti correlati](https://msdn.microsoft.com/library/bb896272.aspx) in MSDN Library.)</span><span class="sxs-lookup"><span data-stu-id="cb503-188">For more information, see [Loading Related Objects](https://msdn.microsoft.com/library/bb896272.aspx) in the MSDN Library.)</span></span>

<span data-ttu-id="cb503-189">Nella prossima esercitazione, si apprenderà come usare la funzionalità di Dynamic Data per renderlo più facile da programmare e testare i dati formattazione e convalida le regole.</span><span class="sxs-lookup"><span data-stu-id="cb503-189">In the next tutorial, you'll learn how to use Dynamic Data functionality to make it easier to program and test data formatting and validation rules.</span></span> <span data-ttu-id="cb503-190">Anziché specificare le regole di ogni pagina web, ad esempio le stringhe di formato di dati e o meno un campo è obbligatorio, è possibile specificare regole di questo tipo nei metadati del modello di dati e vengono applicati automaticamente in ogni pagina.</span><span class="sxs-lookup"><span data-stu-id="cb503-190">Instead of specifying on each web page rules such as data format strings and whether or not a field is required, you can specify such rules in data model metadata and they're automatically applied on every page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cb503-191">[Precedente](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="cb503-191">[Previous](the-entity-framework-and-aspnet-getting-started-part-6.md)
[Next](the-entity-framework-and-aspnet-getting-started-part-8.md)</span></span>