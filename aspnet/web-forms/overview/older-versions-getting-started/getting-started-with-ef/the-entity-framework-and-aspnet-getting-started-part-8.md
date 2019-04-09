---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form - parte 8 | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET utilizzando Entity Framework. L'applicazione di esempio è...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 0fd943eba4c6d80bba5ca6c4d69cbd3a8927513d
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59391511"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a><span data-ttu-id="ad344-104">Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form - parte 8</span><span class="sxs-lookup"><span data-stu-id="ad344-104">Getting Started with Entity Framework 4.0 Database First and ASP.NET 4 Web Forms - Part 8</span></span>

<span data-ttu-id="ad344-105">da [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ad344-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="ad344-106">L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4.0 e Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="ad344-106">The Contoso University sample web application demonstrates how to create ASP.NET Web Forms applications using the Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="ad344-107">Per informazioni sulla serie di esercitazioni, vedere [la prima esercitazione della serie](the-entity-framework-and-aspnet-getting-started-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="ad344-107">For information about the tutorial series, see [the first tutorial in the series](the-entity-framework-and-aspnet-getting-started-part-1.md)</span></span>


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a><span data-ttu-id="ad344-108">Usando la funzionalità di Dynamic Data per formattare e convalidare i dati</span><span class="sxs-lookup"><span data-stu-id="ad344-108">Using Dynamic Data Functionality to Format and Validate Data</span></span>

<span data-ttu-id="ad344-109">Nell'esercitazione precedente è implementato le stored procedure.</span><span class="sxs-lookup"><span data-stu-id="ad344-109">In the previous tutorial you implemented stored procedures.</span></span> <span data-ttu-id="ad344-110">Questa esercitazione verrà illustrato come la funzionalità di Dynamic Data può offrire i vantaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="ad344-110">This tutorial will show you how Dynamic Data functionality can provide the following benefits:</span></span>

- <span data-ttu-id="ad344-111">I campi vengono formattati automaticamente per la visualizzazione in base al tipo di dati.</span><span class="sxs-lookup"><span data-stu-id="ad344-111">Fields are automatically formatted for display based on their data type.</span></span>
- <span data-ttu-id="ad344-112">I campi vengono convalidati automaticamente in base al tipo di dati.</span><span class="sxs-lookup"><span data-stu-id="ad344-112">Fields are automatically validated based on their data type.</span></span>
- <span data-ttu-id="ad344-113">È possibile aggiungere metadati al modello di dati per personalizzare il comportamento di formattazione e convalida.</span><span class="sxs-lookup"><span data-stu-id="ad344-113">You can add metadata to the data model to customize formatting and validation behavior.</span></span> <span data-ttu-id="ad344-114">Quando si esegue questa operazione, è possibile aggiungere le regole di formattazione e convalida un'unica posizione e vengono automaticamente applicati ovunque si accede ai campi utilizzando i controlli Dynamic Data.</span><span class="sxs-lookup"><span data-stu-id="ad344-114">When you do this, you can add the formatting and validation rules in just one place, and they're automatically applied everywhere you access the fields using Dynamic Data controls.</span></span>

<span data-ttu-id="ad344-115">Per visualizzarne il funzionamento, sarà necessario modificare i controlli da usare per visualizzare e modificare campi esistenti *Students.aspx* pagina e si aggiungerà la formattazione e convalida i metadati per i campi nome e la data del `Student` tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="ad344-115">To see how this works, you'll change the controls you use to display and edit fields in the existing *Students.aspx* page, and you'll add formatting and validation metadata to the name and date fields of the `Student` entity type.</span></span>

[![I<span data-ttu-id="ad344-116">mage01]</span><span class="sxs-lookup"><span data-stu-id="ad344-116">mage01]</span></span>(the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a><span data-ttu-id="ad344-117">Utilizzo di markup di DynamicField e controlli di DynamicControl</span><span class="sxs-lookup"><span data-stu-id="ad344-117">Using DynamicField and DynamicControl Controls</span></span>

<span data-ttu-id="ad344-118">Aprire il *Students.aspx* pagina e nella `StudentsGridView` controllo Sostituisci il **nome** e **data di registrazione** `TemplateField` elementi con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="ad344-118">Open the *Students.aspx* page and in the `StudentsGridView` control replace the **Name** and **Enrollment Date** `TemplateField` elements with the following markup:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

<span data-ttu-id="ad344-119">Usa questo markup `DynamicControl` controlla invece di `TextBox` e `Label` controlli in studente assegnare un nome campo modello e viene utilizzato un `DynamicField` controllo per la data di registrazione.</span><span class="sxs-lookup"><span data-stu-id="ad344-119">This markup uses `DynamicControl` controls in place of `TextBox` and `Label` controls in the student name template field, and it uses a `DynamicField` control for the enrollment date.</span></span> <span data-ttu-id="ad344-120">Nessuna stringa di formato viene specificata.</span><span class="sxs-lookup"><span data-stu-id="ad344-120">No format strings are specified.</span></span>

<span data-ttu-id="ad344-121">Aggiungere un `ValidationSummary` controllo dopo il `StudentsGridView` controllo.</span><span class="sxs-lookup"><span data-stu-id="ad344-121">Add a `ValidationSummary` control after the `StudentsGridView` control.</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

<span data-ttu-id="ad344-122">Nel `SearchGridView` controllo sostituire il markup per il **Name** e **data di registrazione** colonne come è stato fatto nel `StudentsGridView` controllare, ad eccezione del fatto omettere il `EditItemTemplate` elemento.</span><span class="sxs-lookup"><span data-stu-id="ad344-122">In the `SearchGridView` control replace the markup for the **Name** and **Enrollment Date** columns as you did in the `StudentsGridView` control, except omit the `EditItemTemplate` element.</span></span> <span data-ttu-id="ad344-123">Il `Columns` dell'elemento di `SearchGridView` controllo ora contiene il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="ad344-123">The `Columns` element of the `SearchGridView` control now contains the following markup:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

<span data-ttu-id="ad344-124">Aprire *Students.aspx.cs* e aggiungere quanto segue `using` istruzione:</span><span class="sxs-lookup"><span data-stu-id="ad344-124">Open *Students.aspx.cs* and add the following `using` statement:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

<span data-ttu-id="ad344-125">Aggiungere un gestore per la pagina `Init` evento:</span><span class="sxs-lookup"><span data-stu-id="ad344-125">Add a handler for the page's `Init` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

<span data-ttu-id="ad344-126">Questo codice specifica che i dati dinamici fornirà la formattazione e convalida in questi controlli con associazione a dati per i campi del `Student` entità.</span><span class="sxs-lookup"><span data-stu-id="ad344-126">This code specifies that Dynamic Data will provide formatting and validation in these data-bound controls for fields of the `Student` entity.</span></span> <span data-ttu-id="ad344-127">Se viene visualizzato un messaggio di errore simile al seguente quando si esegue la pagina, in genere significa che ha dimenticato di chiamare il `EnableDynamicData` metodo `Page_Init`:</span><span class="sxs-lookup"><span data-stu-id="ad344-127">If you get an error message like the following example when you run the page, it typically means you've forgotten to call the `EnableDynamicData` method in `Page_Init`:</span></span>

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

<span data-ttu-id="ad344-128">Eseguire la pagina.</span><span class="sxs-lookup"><span data-stu-id="ad344-128">Run the page.</span></span>

[![I<span data-ttu-id="ad344-129">mage03]</span><span class="sxs-lookup"><span data-stu-id="ad344-129">mage03]</span></span>(the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

<span data-ttu-id="ad344-130">Nel **data di registrazione** colonna, insieme alla data viene visualizzata l'ora poiché il tipo di proprietà `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="ad344-130">In the **Enrollment Date** column, the time is displayed along with the date because the property type is `DateTime`.</span></span> <span data-ttu-id="ad344-131">Correggerai che in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="ad344-131">You'll fix that later.</span></span>

<span data-ttu-id="ad344-132">Per ora, si noti che i dati dinamici offre automaticamente la convalida dei dati di base.</span><span class="sxs-lookup"><span data-stu-id="ad344-132">For now, notice that Dynamic Data automatically provides basic data validation.</span></span> <span data-ttu-id="ad344-133">Ad esempio, fare clic su **Edit**, deselezionare il campo della data, fare clic su **Update**, e vedrai che i dati dinamici automaticamente rende questa un campo obbligatorio in quanto il valore non ammette valori null nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="ad344-133">For example, click **Edit**, clear the date field, click **Update**, and you see that Dynamic Data automatically makes this a required field because the value is not nullable in the data model.</span></span> <span data-ttu-id="ad344-134">Nella pagina viene visualizzato un asterisco dopo il campo e un messaggio di errore nel `ValidationSummary` controllo:</span><span class="sxs-lookup"><span data-stu-id="ad344-134">The page displays an asterisk after the field and an error message in the `ValidationSummary` control:</span></span>

[![I<span data-ttu-id="ad344-135">mage05]</span><span class="sxs-lookup"><span data-stu-id="ad344-135">mage05]</span></span>(the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

<span data-ttu-id="ad344-136">È possibile omettere il `ValidationSummary` controllare, in quanto è anche possibile tenere premuto il puntatore del mouse sull'asterisco per visualizzare il messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="ad344-136">You could omit the `ValidationSummary` control, because you can also hold the mouse pointer over the asterisk to see the error message:</span></span>

[![I<span data-ttu-id="ad344-137">mage06]</span><span class="sxs-lookup"><span data-stu-id="ad344-137">mage06]</span></span>(the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

<span data-ttu-id="ad344-138">I dati dinamici convalida inoltre che i dati immessi nel **data di registrazione** campo è una data valida:</span><span class="sxs-lookup"><span data-stu-id="ad344-138">Dynamic Data will also validate that data entered in the **Enrollment Date** field is a valid date:</span></span>

[![I<span data-ttu-id="ad344-139">mage04]</span><span class="sxs-lookup"><span data-stu-id="ad344-139">mage04]</span></span>(the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

<span data-ttu-id="ad344-140">Come si può notare, si tratta di un messaggio di errore generico.</span><span class="sxs-lookup"><span data-stu-id="ad344-140">As you can see, this is a generic error message.</span></span> <span data-ttu-id="ad344-141">Nella sezione successiva verrà illustrato come personalizzare i messaggi, nonché la convalida e regole di formattazione.</span><span class="sxs-lookup"><span data-stu-id="ad344-141">In the next section you'll see how to customize messages as well as validation and formatting rules.</span></span>

## <a name="adding-metadata-to-the-data-model"></a><span data-ttu-id="ad344-142">Aggiunta di metadati nel modello di dati</span><span class="sxs-lookup"><span data-stu-id="ad344-142">Adding Metadata to the Data Model</span></span>

<span data-ttu-id="ad344-143">In genere, si desidera personalizzare la funzionalità fornita da Dynamic Data.</span><span class="sxs-lookup"><span data-stu-id="ad344-143">Typically, you want to customize the functionality provided by Dynamic Data.</span></span> <span data-ttu-id="ad344-144">Ad esempio, è possibile modificare la modalità di visualizzazione dei dati e il contenuto dei messaggi di errore.</span><span class="sxs-lookup"><span data-stu-id="ad344-144">For example, you might change how data is displayed and the content of error messages.</span></span> <span data-ttu-id="ad344-145">È in genere anche personalizzare le regole di convalida dei dati per offrire più funzionalità rispetto a ciò che fornisce i dati dinamici automaticamente in base i tipi di dati.</span><span class="sxs-lookup"><span data-stu-id="ad344-145">You typically also customize data validation rules to provide more functionality than what Dynamic Data provides automatically based on data types.</span></span> <span data-ttu-id="ad344-146">A tale scopo è creare classi parziali che corrispondono ai tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="ad344-146">To do this, you create partial classes that correspond to entity types.</span></span>

<span data-ttu-id="ad344-147">Nella **Esplora soluzioni**, fare doppio clic il **ContosoUniversity** progetto, selezionare **Aggiungi riferimento**e aggiungere un riferimento a `System.ComponentModel.DataAnnotations`.</span><span class="sxs-lookup"><span data-stu-id="ad344-147">In **Solution Explorer**, right-click the **ContosoUniversity** project, select **Add Reference**, and add a reference to `System.ComponentModel.DataAnnotations`.</span></span>

[![I<span data-ttu-id="ad344-148">mage11]</span><span class="sxs-lookup"><span data-stu-id="ad344-148">mage11]</span></span>(the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

<span data-ttu-id="ad344-149">Nel *DAL* cartella, creare un nuovo file di classe, denominarlo *Student.cs*e sostituire il codice del modello in essa con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="ad344-149">In the *DAL* folder, create a new class file, name it *Student.cs*, and replace the template code in it with the following code.</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

<span data-ttu-id="ad344-150">Questo codice crea una classe parziale per il `Student` entità.</span><span class="sxs-lookup"><span data-stu-id="ad344-150">This code creates a partial class for the `Student` entity.</span></span> <span data-ttu-id="ad344-151">Il `MetadataType` attributo applicato a questa classe parziale identifica la classe che si sta usando per specificare i metadati.</span><span class="sxs-lookup"><span data-stu-id="ad344-151">The `MetadataType` attribute applied to this partial class identifies the class that you're using to specify metadata.</span></span> <span data-ttu-id="ad344-152">La classe di metadati può avere qualsiasi nome, ma usando il nome dell'entità più "Metadati" è una pratica comune.</span><span class="sxs-lookup"><span data-stu-id="ad344-152">The metadata class can have any name, but using the entity name plus "Metadata" is a common practice.</span></span>

<span data-ttu-id="ad344-153">Gli attributi applicati alle proprietà nella classe di metadati specificano la formattazione, convalida, regole e messaggi di errore.</span><span class="sxs-lookup"><span data-stu-id="ad344-153">The attributes applied to properties in the metadata class specify formatting, validation, rules, and error messages.</span></span> <span data-ttu-id="ad344-154">Gli attributi riportati di seguito saranno disponibili i seguenti risultati:</span><span class="sxs-lookup"><span data-stu-id="ad344-154">The attributes shown here will have the following results:</span></span>

- `EnrollmentDate` <span data-ttu-id="ad344-155">verrà visualizzato come una data (senza un'ora).</span><span class="sxs-lookup"><span data-stu-id="ad344-155">will display as a date (without a time).</span></span>
- <span data-ttu-id="ad344-156">Entrambi i campi nome devono essere 25 caratteri o meno in lunghezza, mentre un messaggio di errore personalizzato viene fornito.</span><span class="sxs-lookup"><span data-stu-id="ad344-156">Both name fields must be 25 characters or less in length, and a custom error message is provided.</span></span>
- <span data-ttu-id="ad344-157">Sono necessari entrambi i campi nome e viene fornito un messaggio di errore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="ad344-157">Both name fields are required, and a custom error message is provided.</span></span>

<span data-ttu-id="ad344-158">Eseguire la *Students.aspx* pagina nuovamente e noterete che le date sono ora visualizzate senza tempi di:</span><span class="sxs-lookup"><span data-stu-id="ad344-158">Run the *Students.aspx* page again, and you see that the dates are now displayed without times:</span></span>

[![I<span data-ttu-id="ad344-159">mage08]</span><span class="sxs-lookup"><span data-stu-id="ad344-159">mage08]</span></span>(the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

<span data-ttu-id="ad344-160">Modificare una riga e provare a cancellare i valori nei campi dei nomi.</span><span class="sxs-lookup"><span data-stu-id="ad344-160">Edit a row and try to clear the values in the name fields.</span></span> <span data-ttu-id="ad344-161">Gli asterischi che indica gli errori di campo vengono visualizzati non appena si lascia un campo, prima di fare clic **Update**.</span><span class="sxs-lookup"><span data-stu-id="ad344-161">The asterisks indicating field errors appear as soon as you leave a field, before you click **Update**.</span></span> <span data-ttu-id="ad344-162">Quando fa clic su **Update**, la pagina Visualizza il testo del messaggio di errore specificato.</span><span class="sxs-lookup"><span data-stu-id="ad344-162">When you click **Update**, the page displays the error message text you specified.</span></span>

[![I<span data-ttu-id="ad344-163">mage10]</span><span class="sxs-lookup"><span data-stu-id="ad344-163">mage10]</span></span>(the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

<span data-ttu-id="ad344-164">Provare a immettere nomi più lunghi di 25 caratteri, fare clic su **Update**, e la pagina Visualizza il testo del messaggio di errore specificato.</span><span class="sxs-lookup"><span data-stu-id="ad344-164">Try to enter names that are longer than 25 characters, click **Update**, and the page displays the error message text you specified.</span></span>

[![I<span data-ttu-id="ad344-165">mage09]</span><span class="sxs-lookup"><span data-stu-id="ad344-165">mage09]</span></span>(the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

<span data-ttu-id="ad344-166">Ora che è stato configurato queste regole di convalida e formattazione nei metadati del modello di dati, le regole verranno applicate automaticamente in ogni pagina che consente di visualizzare o consente di apportare modifiche a tali campi, in modo purché utilizzino `DynamicControl` o `DynamicField` controlli.</span><span class="sxs-lookup"><span data-stu-id="ad344-166">Now that you've set up these formatting and validation rules in the data model metadata, the rules will automatically be applied on every page that displays or allows changes to these fields, so long as you use `DynamicControl` or `DynamicField` controls.</span></span> <span data-ttu-id="ad344-167">Ciò riduce la quantità di codice ridondante, è necessario scrivere, che rende la programmazione e le operazioni di testing, e assicura che siano coerenti in tutta l'applicazione convalida e formattazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="ad344-167">This reduces the amount of redundant code you have to write, which makes programming and testing easier, and it ensures that data formatting and validation are consistent throughout an application.</span></span>

## <a name="more-information"></a><span data-ttu-id="ad344-168">Altre informazioni</span><span class="sxs-lookup"><span data-stu-id="ad344-168">More Information</span></span>

<span data-ttu-id="ad344-169">Si conclude così questa serie di esercitazioni sui concetti introduttivi di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ad344-169">This concludes this series of tutorials on Getting Started with the Entity Framework.</span></span> <span data-ttu-id="ad344-170">Per altre risorse che consentono di imparare a usare Entity Framework, continuare [la prima esercitazione della serie di esercitazioni di Entity Framework successiva](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) o i siti seguenti:</span><span class="sxs-lookup"><span data-stu-id="ad344-170">For more resources to help you learn how to use the Entity Framework, continue with [the first tutorial in the next Entity Framework tutorial series](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) or visit the following sites:</span></span>

- [<span data-ttu-id="ad344-171">Domande frequenti su Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ad344-171">Entity Framework FAQ</span></span>](http://www.ef-faq.org/introduction.html)
- [<span data-ttu-id="ad344-172">Blog del Team di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ad344-172">The Entity Framework Team Blog</span></span>](https://blogs.msdn.com/b/adonet/)
- [<span data-ttu-id="ad344-173">Entity Framework in MSDN Library</span><span class="sxs-lookup"><span data-stu-id="ad344-173">Entity Framework in the MSDN Library</span></span>](https://msdn.microsoft.com/library/bb399572.aspx)
- [<span data-ttu-id="ad344-174">Entity Framework in MSDN Data Developer Center</span><span class="sxs-lookup"><span data-stu-id="ad344-174">Entity Framework in the MSDN Data Developer Center</span></span>](https://msdn.microsoft.com/data/ef.aspx)
- [<span data-ttu-id="ad344-175">Cenni preliminari sul controllo Server Web EntityDataSource in MSDN Library</span><span class="sxs-lookup"><span data-stu-id="ad344-175">EntityDataSource Web Server Control Overview in the MSDN Library</span></span>](https://msdn.microsoft.com/library/cc488502.aspx)
- [<span data-ttu-id="ad344-176">Controllo EntityDataSource API reference in MSDN Library</span><span class="sxs-lookup"><span data-stu-id="ad344-176">EntityDataSource control API reference in the MSDN Library</span></span>](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [<span data-ttu-id="ad344-177">Entity Framework forum su MSDN</span><span class="sxs-lookup"><span data-stu-id="ad344-177">Entity Framework Forums on MSDN</span></span>](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [<span data-ttu-id="ad344-178">Blog di Julie</span><span class="sxs-lookup"><span data-stu-id="ad344-178">Julie Lerman's blog</span></span>](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [<span data-ttu-id="ad344-179">Precedente</span><span class="sxs-lookup"><span data-stu-id="ad344-179">Previous</span></span>](the-entity-framework-and-aspnet-getting-started-part-7.md)
