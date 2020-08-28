---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Aggiunta di un nuovo campo | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 7018de3eab9d7cced72c76d0b74a79f7c8154f9d
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2020
ms.locfileid: "89045143"
---
# <a name="adding-a-new-field"></a><span data-ttu-id="7c7f4-102">Aggiunta di un nuovo campo</span><span class="sxs-lookup"><span data-stu-id="7c7f4-102">Adding a New Field</span></span>

<span data-ttu-id="7c7f4-103">di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7c7f4-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="7c7f4-104">In questa sezione si utilizzerà Migrazioni Code First di Entity Framework per eseguire la migrazione di alcune modifiche alle classi del modello in modo da applicare la modifica al database.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-104">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="7c7f4-105">Per impostazione predefinita, quando si utilizza Entity Framework Code First per creare automaticamente un database, come in precedenza in questa esercitazione, Code First aggiunge una tabella al database per tenere traccia dell'eventuale sincronizzazione dello schema del database con le classi del modello da cui è stato generato.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-105">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="7c7f4-106">Se non sono sincronizzati, il Entity Framework genera un errore.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-106">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="7c7f4-107">In questo modo è più semplice tenere traccia dei problemi in fase di sviluppo che potrebbero essere individuati (per errori nascosti) in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-107">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="7c7f4-108">Impostazione Migrazioni Code First per le modifiche al modello</span><span class="sxs-lookup"><span data-stu-id="7c7f4-108">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="7c7f4-109">Passare a Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-109">Navigate to Solution Explorer.</span></span> <span data-ttu-id="7c7f4-110">Fare clic con il pulsante destro del mouse sul file *Movies. MDF* e selezionare **Elimina** per rimuovere il database Movies.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-110">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span> <span data-ttu-id="7c7f4-111">Se non viene visualizzato il file *Movies. MDF* , fare clic sull'icona **Mostra tutti i file** mostrata sotto nella struttura rossa.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-111">If you don't see the *Movies.mdf* file, click on the **Show All Files** icon shown below in the red outline.</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="7c7f4-112">Compilare l'applicazione e verificare che non siano presenti errori.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-112">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="7c7f4-113">Dal menu **strumenti** fare clic su **Gestione pacchetti NuGet** e quindi su **console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-113">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Aggiungi Pack Man](adding-a-new-field/_static/image2.png)

<span data-ttu-id="7c7f4-115">Nella finestra **console di gestione pacchetti** al `PM>` prompt dei comandi immettere</span><span class="sxs-lookup"><span data-stu-id="7c7f4-115">In the **Package Manager Console** window at the `PM>` prompt enter</span></span>

<span data-ttu-id="7c7f4-116">Enable-Migrations-ContextTypeName MvcMovie. Models. MovieDBContext</span><span class="sxs-lookup"><span data-stu-id="7c7f4-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span></span>

![](adding-a-new-field/_static/image3.png)

<span data-ttu-id="7c7f4-117">Il comando **Enable-Migrations** (illustrato in precedenza) consente di creare un file *Configuration.cs* in una nuova cartella *Migrations* .</span><span class="sxs-lookup"><span data-stu-id="7c7f4-117">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field/_static/image4.png)

<span data-ttu-id="7c7f4-118">Visual Studio apre il file *Configuration.cs* .</span><span class="sxs-lookup"><span data-stu-id="7c7f4-118">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="7c7f4-119">Sostituire il `Seed` metodo nel file *Configuration.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7c7f4-119">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="7c7f4-120">Posizionare il puntatore del mouse sulla linea rossa ondulata sotto `Movie` e fare clic su `Show Potential Fixes` e quindi su **using** **MvcMovie. Models;**</span><span class="sxs-lookup"><span data-stu-id="7c7f4-120">Hover over the red squiggly line under `Movie` and click `Show Potential Fixes` and then click **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field/_static/image5.png)

<span data-ttu-id="7c7f4-121">Questa operazione consente di aggiungere l'istruzione using seguente:</span><span class="sxs-lookup"><span data-stu-id="7c7f4-121">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> <span data-ttu-id="7c7f4-122">Migrazioni Code First chiama il `Seed` metodo dopo ogni migrazione (ovvero, chiamando **Update-database** nella console di gestione pacchetti) e questo metodo aggiorna le righe che sono già state inserite o le inserisce se non esistono ancora.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-122">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>
> 
> <span data-ttu-id="7c7f4-123">Il metodo [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) nel codice seguente esegue un'operazione "upsert":</span><span class="sxs-lookup"><span data-stu-id="7c7f4-123">The [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method in the following code performs an "upsert" operation:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> <span data-ttu-id="7c7f4-124">Poiché il metodo [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) viene eseguito con ogni migrazione, non è possibile inserire semplicemente i dati perché le righe che si sta tentando di aggiungere saranno già presenti dopo la prima migrazione che crea il database.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-124">Because the [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) method runs with every migration, you can't just insert data, because the rows you are trying to add will already be there after the first migration that creates the database.</span></span> <span data-ttu-id="7c7f4-125">L'operazione "[Upsert](http://en.wikipedia.org/wiki/Upsert)" impedisce gli errori che si verificano se si tenta di inserire una riga già esistente, ma viene eseguito l'override di eventuali modifiche apportate ai dati durante il test dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-125">The "[upsert](http://en.wikipedia.org/wiki/Upsert)" operation prevents errors that would happen if you try to insert a row that already exists, but it overrides any changes to data that you may have made while testing the application.</span></span> <span data-ttu-id="7c7f4-126">Con i dati di test in alcune tabelle potrebbe non essere necessario eseguire questa operazione: in alcuni casi, quando si modificano i dati durante il test si desidera che le modifiche rimangano dopo gli aggiornamenti del database.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-126">With test data in some tables you might not want that to happen: in some cases when you change data while testing you want your changes to remain after database updates.</span></span> <span data-ttu-id="7c7f4-127">In tal caso si desidera eseguire un'operazione di inserimento condizionale: inserire una riga solo se non esiste già.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-127">In that case you want to do a conditional insert operation: insert a row only if it doesn't already exist.</span></span>   
> 
> <span data-ttu-id="7c7f4-128">Il primo parametro passato al metodo [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) specifica la proprietà da utilizzare per verificare se una riga esiste già.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-128">The first parameter passed to the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method specifies the property to use to check if a row already exists.</span></span> <span data-ttu-id="7c7f4-129">Per i dati del film di test da fornire, `Title` è possibile usare la proprietà per questo scopo, perché ogni titolo dell'elenco è univoco:</span><span class="sxs-lookup"><span data-stu-id="7c7f4-129">For the test movie data that you are providing, the `Title` property can be used for this purpose since each title in the list is unique:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> <span data-ttu-id="7c7f4-130">Questo codice presuppone che i titoli siano univoci.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-130">This code assumes that titles are unique.</span></span> <span data-ttu-id="7c7f4-131">Se si aggiunge manualmente un titolo duplicato, alla successiva esecuzione di una migrazione verrà generata l'eccezione seguente.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-131">If you manually add a duplicate title, you'll get the following exception the next time you perform a migration.</span></span>   
> 
> <span data-ttu-id="7c7f4-132">*La sequenza contiene più di un elemento*</span><span class="sxs-lookup"><span data-stu-id="7c7f4-132">*Sequence contains more than one element*</span></span>  
> 
> <span data-ttu-id="7c7f4-133">Per ulteriori informazioni sul metodo [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) , vedere [prestare attenzione al metodo AddOrUpdate di EF 4,3](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/).</span><span class="sxs-lookup"><span data-stu-id="7c7f4-133">For more information about the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method, see [Take care with EF 4.3 AddOrUpdate Method](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)..</span></span>

<span data-ttu-id="7c7f4-134">**Premere CTRL + MAIUSC + B per compilare il progetto.** I passaggi seguenti avranno esito negativo se non si compila a questo punto.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-134">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if you don't build at this point.)</span></span>

<span data-ttu-id="7c7f4-135">Il passaggio successivo consiste nel creare una `DbMigration` classe per la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-135">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="7c7f4-136">Questa migrazione crea un nuovo database, motivo per cui è stato eliminato il file *Movie. MDF* in un passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-136">This migration creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="7c7f4-137">Nella finestra **console di gestione pacchetti** immettere il comando `add-migration Initial` per creare la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-137">In the **Package Manager Console** window, enter the command `add-migration Initial` to create the initial migration.</span></span> <span data-ttu-id="7c7f4-138">Il nome "Initial" è arbitrario e viene usato per assegnare un nome al file di migrazione creato.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-138">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field/_static/image6.png)

<span data-ttu-id="7c7f4-139">Migrazioni Code First crea un altro file di classe nella cartella *Migrations* (con il nome *{datestamp} \_ Initial.cs* ) e questa classe contiene il codice che crea lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-139">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="7c7f4-140">Il nome del file di migrazione è preceduto da un timestamp per facilitare l'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-140">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="7c7f4-141">Esaminare il file \* \_ Initial.cs {datestamp}\* che contiene le istruzioni per creare la `Movies` tabella per il database di film.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-141">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the `Movies` table for the Movie DB.</span></span> <span data-ttu-id="7c7f4-142">Quando si aggiorna il database nelle istruzioni seguenti, questo file *{datestamp} \_ Initial.cs* verrà eseguito e creerà lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-142">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="7c7f4-143">Viene quindi eseguito il metodo **Seed** per popolare il database con i dati di test.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-143">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="7c7f4-144">Nella **console di gestione pacchetti**immettere il comando `update-database` per creare il database ed eseguire il `Seed` metodo.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-144">In the **Package Manager Console**, enter the command `update-database` to create the database and run the `Seed` method.</span></span>

![](adding-a-new-field/_static/image7.png)

<span data-ttu-id="7c7f4-145">Se viene generato un errore che indica che una tabella esiste già e non può essere creata, è probabile che l'applicazione sia stata eseguita dopo l'eliminazione del database e prima dell'esecuzione `update-database` .</span><span class="sxs-lookup"><span data-stu-id="7c7f4-145">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="7c7f4-146">In tal caso, eliminare nuovamente il file *Movies. MDF* e ripetere il `update-database` comando.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-146">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="7c7f4-147">Se viene comunque ricevuto un errore, eliminare la cartella migrazioni e il contenuto, quindi iniziare con le istruzioni nella parte superiore di questa pagina, ovvero eliminare il file *Movies. MDF* , quindi passare a Enable-Migrations.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-147">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span> <span data-ttu-id="7c7f4-148">Se viene comunque visualizzato un errore, aprire Esplora oggetti di SQL Server e rimuovere il database dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-148">If you still get an error, open SQL Server Object Explorer and remove the database from the list.</span></span>

<span data-ttu-id="7c7f4-149">Eseguire l'applicazione e passare all'URL */Movies* .</span><span class="sxs-lookup"><span data-stu-id="7c7f4-149">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="7c7f4-150">Vengono visualizzati i dati di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-150">The seed data is displayed.</span></span>

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="7c7f4-151">Aggiunta di una proprietà Rating al modello Movie</span><span class="sxs-lookup"><span data-stu-id="7c7f4-151">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="7c7f4-152">Per iniziare, aggiungere una nuova `Rating` proprietà alla `Movie` classe esistente.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-152">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="7c7f4-153">Aprire il file *Models\Movie.cs* e aggiungere la `Rating` proprietà come questa:</span><span class="sxs-lookup"><span data-stu-id="7c7f4-153">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="7c7f4-154">La classe completa ha `Movie` ora un aspetto simile al codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7c7f4-154">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

<span data-ttu-id="7c7f4-155">Compilare l'applicazione (CTRL + MAIUSC + B).</span><span class="sxs-lookup"><span data-stu-id="7c7f4-155">Build the application (Ctrl+Shift+B).</span></span>

<span data-ttu-id="7c7f4-156">Poiché è stato aggiunto un nuovo campo alla `Movie` classe, è necessario aggiornare anche l' *elenco bianco* di binding in modo che questa nuova proprietà venga inclusa.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-156">Because you've added a new field to the `Movie` class, you also need to update the binding *white list* so this new property will be included.</span></span> <span data-ttu-id="7c7f4-157">Aggiornare l' `bind` attributo per `Create` i `Edit` metodi di azione e in modo da includere la `Rating` proprietà:</span><span class="sxs-lookup"><span data-stu-id="7c7f4-157">Update the `bind` attribute for `Create` and `Edit` action methods to include the `Rating` property:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

<span data-ttu-id="7c7f4-158">È necessario anche aggiornare i modelli di vista per visualizzare, creare e modificare la nuova proprietà `Rating` nella vista del browser.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-158">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="7c7f4-159">Aprire il file *\Views\Movies\Index.cshtml* e aggiungere un' `<th>Rating</th>` intestazione di colonna immediatamente dopo la colonna **Price** .</span><span class="sxs-lookup"><span data-stu-id="7c7f4-159">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="7c7f4-160">Aggiungere quindi una `<td>` colonna accanto alla fine del modello per eseguire il rendering del `@item.Rating` valore.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-160">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="7c7f4-161">Di seguito è riportato il modello di visualizzazione *index. cshtml* aggiornato:</span><span class="sxs-lookup"><span data-stu-id="7c7f4-161">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

<span data-ttu-id="7c7f4-162">Successivamente, aprire il file *\Views\Movies\Create.cshtml* e aggiungere il `Rating` campo con il markup evidenziato seguente.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-162">Next, open the *\Views\Movies\Create.cshtml* file and add the `Rating` field with the following highlighted markup.</span></span> <span data-ttu-id="7c7f4-163">Viene eseguito il rendering di una casella di testo in modo da poter specificare una classificazione quando viene creato un nuovo film.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-163">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

<span data-ttu-id="7c7f4-164">A questo punto è stato aggiornato il codice dell'applicazione per supportare la nuova `Rating` Proprietà.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-164">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="7c7f4-165">Eseguire l'applicazione e passare all'URL */Movies* .</span><span class="sxs-lookup"><span data-stu-id="7c7f4-165">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="7c7f4-166">Quando si esegue questa operazione, tuttavia, verrà visualizzato uno degli errori seguenti:</span><span class="sxs-lookup"><span data-stu-id="7c7f4-166">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field/_static/image9.png)  
  
<span data-ttu-id="7c7f4-167">Il modello che supporta il contesto ' MovieDBContext ' è stato modificato dopo la creazione del database.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-167">The model backing the 'MovieDBContext' context has changed since the database was created.</span></span> <span data-ttu-id="7c7f4-168">Si consiglia di utilizzare Migrazioni Code First per aggiornare il database di ( https://go.microsoft.com/fwlink/?LinkId=238269) .</span><span class="sxs-lookup"><span data-stu-id="7c7f4-168">Consider using Code First Migrations to update the database (https://go.microsoft.com/fwlink/?LinkId=238269).</span></span>

![](adding-a-new-field/_static/image10.png)

<span data-ttu-id="7c7f4-169">Questo errore viene visualizzato perché la classe del `Movie` modello aggiornata nell'applicazione è ora diversa dallo schema della `Movie` tabella del database esistente.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-169">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="7c7f4-170">Nella tabella del database non è presente una colonna `Rating`.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-170">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="7c7f4-171">Per correggere questo errore, esistono alcuni approcci:</span><span class="sxs-lookup"><span data-stu-id="7c7f4-171">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="7c7f4-172">Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-172">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="7c7f4-173">Questo approccio è molto utile nelle prime fasi del ciclo di sviluppo in una fase attiva di sviluppo di un database di test e consente di migliorare rapidamente lo schema del modello e il database insieme.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-173">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="7c7f4-174">Tuttavia, il lato negativo è che si perdono i dati esistenti nel database, quindi *non* si vuole usare questo approccio in un database di produzione.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-174">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="7c7f4-175">Un modo efficace per sviluppare un'applicazione consiste nell'inizializzare automaticamente un database con dati di test usando un inizializzatore.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-175">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="7c7f4-176">Per altre informazioni sugli inizializzatori di database Entity Framework, vedere l' [esercitazione su MVC/Entity Framework di ASP.NET](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="7c7f4-176">For more information on Entity Framework database initializers, see [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="7c7f4-177">Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-177">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="7c7f4-178">Il vantaggio di questo approccio è che i dati vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-178">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="7c7f4-179">È possibile apportare questa modifica manualmente o creando uno script di modifica del database.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-179">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="7c7f4-180">Usare Migrazioni Code First per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-180">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="7c7f4-181">Per questa esercitazione si userà Migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-181">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="7c7f4-182">Aggiornare il metodo seed in modo da fornire un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-182">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="7c7f4-183">Aprire il file Migrations\Configuration.cs e aggiungere un campo rating a ogni oggetto Movie.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-183">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

<span data-ttu-id="7c7f4-184">Compilare la soluzione, quindi aprire la finestra **console di gestione pacchetti** e immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="7c7f4-184">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration Rating`

<span data-ttu-id="7c7f4-185">Il `add-migration` comando indica al Framework di migrazione di esaminare il modello di film corrente con lo schema del database del film corrente e creare il codice necessario per eseguire la migrazione del database al nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-185">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="7c7f4-186">La *classificazione* dei nomi è arbitraria e viene usata per assegnare un nome al file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-186">The name *Rating* is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="7c7f4-187">È utile usare un nome significativo per il passaggio di migrazione.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-187">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="7c7f4-188">Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMigration` classe derivata e nel `Up` metodo è possibile visualizzare il codice che crea la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-188">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

<span data-ttu-id="7c7f4-189">Compilare la soluzione, quindi immettere il `update-database` comando nella finestra **console di gestione pacchetti** .</span><span class="sxs-lookup"><span data-stu-id="7c7f4-189">Build the solution, and then enter the `update-database` command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="7c7f4-190">Nell'immagine seguente viene illustrato l'output nella finestra della **console di gestione pacchetti** (la *classificazione* di data di inizio in sospeso sarà diversa).</span><span class="sxs-lookup"><span data-stu-id="7c7f4-190">The following image shows the output in the **Package Manager Console** window (The date stamp prepending *Rating* will be different.)</span></span>

![](adding-a-new-field/_static/image11.png)

<span data-ttu-id="7c7f4-191">Eseguire di nuovo l'applicazione e passare all'URL/Movies.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-191">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="7c7f4-192">È possibile visualizzare il nuovo campo rating.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-192">You can see the new Rating field.</span></span>

![](adding-a-new-field/_static/image12.png)

<span data-ttu-id="7c7f4-193">Fare clic sul collegamento **Crea nuovo** per aggiungere un nuovo film.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-193">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="7c7f4-194">Si noti che è possibile aggiungere una classificazione.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-194">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field/_static/image13.png)

<span data-ttu-id="7c7f4-196">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-196">Click **Create**.</span></span> <span data-ttu-id="7c7f4-197">Il nuovo film, incluso il rating, viene ora visualizzato nell'elenco dei film:</span><span class="sxs-lookup"><span data-stu-id="7c7f4-197">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

<span data-ttu-id="7c7f4-199">Ora che il progetto usa le migrazioni, non è necessario eliminare il database quando si aggiunge un nuovo campo o in caso contrario aggiornare lo schema.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-199">Now that the project is using migrations, you won't need to drop the database when you add a new field or otherwise update the schema.</span></span> <span data-ttu-id="7c7f4-200">Nella sezione successiva verranno apportate ulteriori modifiche allo schema e verranno utilizzate le migrazioni per aggiornare il database.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-200">In the next section, we'll make more schema changes and use migrations to update the database.</span></span>

<span data-ttu-id="7c7f4-201">È inoltre necessario aggiungere il `Rating` campo ai modelli di visualizzazione modifica, dettagli ed Elimina.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-201">You should also add the `Rating` field to the Edit, Details, and Delete view templates.</span></span>

<span data-ttu-id="7c7f4-202">È possibile immettere di nuovo il comando "Update-database" nella finestra della **console di gestione pacchetti** e non verrà eseguito alcun codice di migrazione perché lo schema corrisponde al modello.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-202">You could enter the "update-database" command in the **Package Manager Console** window again and no migration code would run, because the schema matches the model.</span></span> <span data-ttu-id="7c7f4-203">Tuttavia, l'esecuzione di "Update-database" eseguirà di `Seed` nuovo il metodo e, se è stato modificato uno dei dati di inizializzazione, le modifiche andranno perse perché il `Seed` Metodo Upsert i dati.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-203">However, running "update-database" will run the `Seed` method again, and if you changed any of the Seed data, the changes will be lost because the `Seed` method upserts data.</span></span> <span data-ttu-id="7c7f4-204">Per altre informazioni sul `Seed` metodo, vedere l'esercitazione più popolare su [ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)di Tom Dykstra.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-204">You can read more about the `Seed` method in Tom Dykstra's popular [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="7c7f4-205">In questa sezione è stato illustrato come è possibile modificare gli oggetti modello e sincronizzare il database con le modifiche.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-205">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="7c7f4-206">È stato inoltre illustrato un modo per popolare un database appena creato con dati di esempio, in modo da poter provare gli scenari.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-206">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="7c7f4-207">Questa è stata una rapida introduzione ai Code First, vedere [creazione di un modello di dati Entity Framework per un'applicazione MVC ASP.NET](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) per un'esercitazione più completa sull'argomento.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-207">This was just a quick introduction to Code First, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) for a more complete tutorial on the subject.</span></span> <span data-ttu-id="7c7f4-208">Si osserverà ora come aggiungere una logica di convalida più completa alle classi del modello e abilitare l'applicazione di alcune regole business.</span><span class="sxs-lookup"><span data-stu-id="7c7f4-208">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7c7f4-209">[Precedente](adding-search.md) 
>  [Avanti](adding-validation.md)</span><span class="sxs-lookup"><span data-stu-id="7c7f4-209">[Previous](adding-search.md)
[Next](adding-validation.md)</span></span>
