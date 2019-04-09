---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Creazione di un MVC 3 Application with Razor and Unobtrusive JavaScript | Microsoft Docs
author: microsoft
description: L'applicazione web di esempio elenco utenti viene illustrato quanto sia semplice creare applicazioni ASP.NET MVC 3 con il motore di visualizzazione Razor. Gli oggetti di applicazione di esempio...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 91c96cc413e63ad2fc160ffbb473c4f3e1ada3e4
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59401062"
---
# <a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="f521c-104">Creazione di un'applicazione MVC 3 con Razor e JavaScript discreto</span><span class="sxs-lookup"><span data-stu-id="f521c-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>

<span data-ttu-id="f521c-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f521c-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f521c-106">L'applicazione web di esempio elenco utenti viene illustrato quanto sia semplice creare applicazioni ASP.NET MVC 3 con il motore di visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="f521c-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="f521c-107">L'applicazione di esempio viene illustrato come usare il nuovo motore di visualizzazione Razor con ASP.NET MVC versione 3 e Visual Studio 2010 per creare un sito Web di elenco di utenti fittizi che include funzionalità quali la creazione, visualizzazione, modifica e l'eliminazione degli utenti.</span><span class="sxs-lookup"><span data-stu-id="f521c-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="f521c-108">Questa esercitazione vengono descritti i passaggi che sono stati eseguiti per compilare l'applicazione ASP.NET MVC 3 esempio di elenco degli utenti.</span><span class="sxs-lookup"><span data-stu-id="f521c-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="f521c-109">Un progetto di Visual Studio con C# e il codice sorgente Visual Basic è disponibile a complemento di questo argomento: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="f521c-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="f521c-110">Se hai domande su questa esercitazione, è possibile pubblicarle per il [forum MVC](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="f521c-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>


## <a name="overview"></a><span data-ttu-id="f521c-111">Panoramica</span><span class="sxs-lookup"><span data-stu-id="f521c-111">Overview</span></span>

<span data-ttu-id="f521c-112">L'applicazione che sarà compilata è un sito Web di elenco semplice di utente.</span><span class="sxs-lookup"><span data-stu-id="f521c-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="f521c-113">Gli utenti possono immettere, visualizzare e aggiornare le informazioni utente.</span><span class="sxs-lookup"><span data-stu-id="f521c-113">Users can enter, view, and update user information.</span></span>

![Sito di esempio](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="f521c-115">È possibile scaricare il progetto completato VB e c# [qui](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span><span class="sxs-lookup"><span data-stu-id="f521c-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="f521c-116">Creazione dell'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="f521c-116">Creating the Web Application</span></span>

<span data-ttu-id="f521c-117">Per avviare l'esercitazione, aprire Visual Studio 2010 e creare un nuovo progetto usando il *applicazione Web ASP.NET MVC 3* modello.</span><span class="sxs-lookup"><span data-stu-id="f521c-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="f521c-118">Denominare l'applicazione &quot;Mvc3Razor&quot;.</span><span class="sxs-lookup"><span data-stu-id="f521c-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

[![N<span data-ttu-id="f521c-119">progetto MVC 3 uova]</span><span class="sxs-lookup"><span data-stu-id="f521c-119">ew MVC 3 project]</span></span>(creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

<span data-ttu-id="f521c-120">Nel **nuovo progetto ASP.NET MVC 3** finestra di dialogo, seleziona **applicazione Internet**, selezionare il motore di visualizzazione Razor e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="f521c-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Finestra di dialogo Nuovo progetto ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="f521c-122">In questa esercitazione non si utilizzerà il provider di appartenenze ASP.NET, pertanto è possibile eliminare tutti i file associati con account di accesso e appartenenza.</span><span class="sxs-lookup"><span data-stu-id="f521c-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="f521c-123">Nelle **Esplora soluzioni**, rimuovere i file e le directory seguenti:</span><span class="sxs-lookup"><span data-stu-id="f521c-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- *<span data-ttu-id="f521c-124">Controllers\AccountController</span><span class="sxs-lookup"><span data-stu-id="f521c-124">Controllers\AccountController</span></span>*
- *<span data-ttu-id="f521c-125">Models\AccountModels</span><span class="sxs-lookup"><span data-stu-id="f521c-125">Models\AccountModels</span></span>*
- *<span data-ttu-id="f521c-126">Views\Shared\\_LogOnPartial</span><span class="sxs-lookup"><span data-stu-id="f521c-126">Views\Shared\\_LogOnPartial</span></span>*
- <span data-ttu-id="f521c-127">*Views\Account* (e tutti i file in questa directory)</span><span class="sxs-lookup"><span data-stu-id="f521c-127">*Views\Account* (and all the files in this directory)</span></span>

![Exp soln](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="f521c-129">Modificare il  <em>\_layout. cshtml</em> file e sostituire il markup all'interno di `<div>` elemento denominato `logindisplay` con il messaggio <em>&quot;</em>account di accesso disabilitato&quot;.</span><span class="sxs-lookup"><span data-stu-id="f521c-129">Edit the <em>\_Layout.cshtml</em> file and replace the markup inside the `<div>` element named `logindisplay` with the message <em>&quot;</em>Login Disabled&quot;.</span></span> <span data-ttu-id="f521c-130">L'esempio seguente illustra il nuovo tag:</span><span class="sxs-lookup"><span data-stu-id="f521c-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="f521c-131">Aggiunta del modello</span><span class="sxs-lookup"><span data-stu-id="f521c-131">Adding the Model</span></span>

<span data-ttu-id="f521c-132">In **Esplora soluzioni**, fare doppio clic sul *modelli* cartella, selezionare **Add**, quindi fare clic su **classe**.</span><span class="sxs-lookup"><span data-stu-id="f521c-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Nuova classe Mdl utente](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="f521c-134">Assegnare alla classe il nome `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="f521c-134">Name the class `UserModel`.</span></span> <span data-ttu-id="f521c-135">Sostituire il contenuto del *UserModel* file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f521c-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="f521c-136">Il `UserModel` classe rappresenta gli utenti.</span><span class="sxs-lookup"><span data-stu-id="f521c-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="f521c-137">Ogni membro della classe è annotato con il [necessari](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) dell'attributo dalle [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="f521c-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="f521c-138">Gli attributi nel [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi forniscono la convalida automatica client e lato server per le applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="f521c-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="f521c-139">Aprire il `HomeController` classi e aggiungere un `using` direttiva in modo da poter accedere il `UserModel` e `Users` classi:</span><span class="sxs-lookup"><span data-stu-id="f521c-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="f521c-140">Subito dopo il `HomeController` dichiarazione, aggiungere il seguente commento e il riferimento a un `Users` classe:</span><span class="sxs-lookup"><span data-stu-id="f521c-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="f521c-141">Il `Users` classe è un archivio dati semplificati, in memoria che verrà usato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="f521c-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="f521c-142">In un'applicazione reale si utilizzerebbe un database per archiviare informazioni utente.</span><span class="sxs-lookup"><span data-stu-id="f521c-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="f521c-143">Le prime righe del `HomeController` file vengono visualizzati nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="f521c-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="f521c-144">Compilare l'applicazione in modo che il modello di utente saranno disponibile per la procedura guidata lo scaffolding nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="f521c-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="f521c-145">Creazione della visualizzazione predefinita</span><span class="sxs-lookup"><span data-stu-id="f521c-145">Creating the Default View</span></span>

<span data-ttu-id="f521c-146">Il passaggio successivo consiste nell'aggiungere un metodo di azione e una visualizzazione per visualizzare gli utenti.</span><span class="sxs-lookup"><span data-stu-id="f521c-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="f521c-147">Eliminare la chiave esistente *Views\Home\Index* file.</span><span class="sxs-lookup"><span data-stu-id="f521c-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="f521c-148">Si creerà una nuova *indice* file da visualizzare agli utenti.</span><span class="sxs-lookup"><span data-stu-id="f521c-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="f521c-149">Nel `HomeController` classe, sostituire il contenuto del `Index` metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f521c-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="f521c-150">Pulsante destro del mouse all'interno di `Index` metodo e quindi fare clic su **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="f521c-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Aggiungi visualizzazione](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="f521c-152">Selezionare il **creare una visualizzazione fortemente tipizzata** opzione.</span><span class="sxs-lookup"><span data-stu-id="f521c-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="f521c-153">Per la **visualizzare i dati classe**, selezionare **Mvc3Razor.Models.UserModel**.</span><span class="sxs-lookup"><span data-stu-id="f521c-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="f521c-154">(Se non viene visualizzata **Mvc3Razor.Models.UserModel** nel **visualizzare dati classe** casella, è necessario compilare il progetto.) Assicurarsi che il motore di visualizzazione è impostato su **Razor**.</span><span class="sxs-lookup"><span data-stu-id="f521c-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="f521c-155">Impostare **visualizzare il contenuto** al **elenco** e quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="f521c-155">Set **View content** to **List** and then click **Add**.</span></span>

![Aggiungi visualizzazione Index](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="f521c-157">La nuova vista esegue automaticamente scaffolding dei dati utente che viene passati per il `Index` vista.</span><span class="sxs-lookup"><span data-stu-id="f521c-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="f521c-158">Esaminare appena generato *Views\Home\Index* file.</span><span class="sxs-lookup"><span data-stu-id="f521c-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="f521c-159">Il **Crea nuovo**, **Edit**, **dettagli**, e **Elimina** i collegamenti non funzionano, ma il resto della pagina è funzionale.</span><span class="sxs-lookup"><span data-stu-id="f521c-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="f521c-160">Eseguire la pagina.</span><span class="sxs-lookup"><span data-stu-id="f521c-160">Run the page.</span></span> <span data-ttu-id="f521c-161">Viene visualizzato un elenco di utenti.</span><span class="sxs-lookup"><span data-stu-id="f521c-161">You see a list of users.</span></span>

![Pagina di indice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="f521c-163">Aprire il *index. cshtml* del file e sostituire il `ActionLink` markup per **modificare**, **dettagli**, e **eliminare** con il codice seguente :</span><span class="sxs-lookup"><span data-stu-id="f521c-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="f521c-164">Il nome utente è utilizzato come ID per trovare il record selezionato nel **Edit**, **dettagli**, e **Elimina** collegamenti.</span><span class="sxs-lookup"><span data-stu-id="f521c-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="f521c-165">Creazione della visualizzazione dei dettagli</span><span class="sxs-lookup"><span data-stu-id="f521c-165">Creating the Details View</span></span>

<span data-ttu-id="f521c-166">Il passaggio successivo consiste nell'aggiungere un `Details` metodo di azione e visualizzazione per visualizzare i dettagli dell'utente.</span><span class="sxs-lookup"><span data-stu-id="f521c-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="f521c-168">Aggiungere il codice seguente `Details` metodo al controller home:</span><span class="sxs-lookup"><span data-stu-id="f521c-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="f521c-169">Pulsante destro del mouse all'interno di `Details` metodo e quindi selezionare <strong>Aggiungi visualizzazione</strong>.</span><span class="sxs-lookup"><span data-stu-id="f521c-169">Right-click inside the `Details` method and then select <strong>Add View</strong>.</span></span> <span data-ttu-id="f521c-170">Verificare che il <strong>visualizzare i dati classe</strong> finestra contiene <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span><span class="sxs-lookup"><span data-stu-id="f521c-170">Verify that the <strong>View data class</strong> box contains <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span></span> <span data-ttu-id="f521c-171">Impostare <strong>visualizzare il contenuto</strong> al <strong>dettagli</strong> e quindi fare clic su <strong>Add</strong>.</span><span class="sxs-lookup"><span data-stu-id="f521c-171">Set <strong>View content</strong> to <strong>Details</strong> and then click <strong>Add</strong>.</span></span>

![Aggiungi visualizzazione dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="f521c-173">Eseguire l'applicazione e selezionare un collegamento details.</span><span class="sxs-lookup"><span data-stu-id="f521c-173">Run the application and select a details link.</span></span> <span data-ttu-id="f521c-174">Lo scaffolding automatico illustra ogni proprietà nel modello.</span><span class="sxs-lookup"><span data-stu-id="f521c-174">The automatic scaffolding shows each property in the model.</span></span>

![Dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="f521c-176">Creazione della visualizzazione di modifica</span><span class="sxs-lookup"><span data-stu-id="f521c-176">Creating the Edit View</span></span>

<span data-ttu-id="f521c-177">Aggiungere il codice seguente `Edit` metodo al controller home.</span><span class="sxs-lookup"><span data-stu-id="f521c-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="f521c-178">Aggiunge una visualizzazione come nei passaggi precedenti, ma imposta **visualizzare il contenuto** al **modificare**.</span><span class="sxs-lookup"><span data-stu-id="f521c-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Aggiungere una visualizzazione di modifica](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="f521c-180">Eseguire l'applicazione e modificare il nome e cognome di uno degli utenti.</span><span class="sxs-lookup"><span data-stu-id="f521c-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="f521c-181">In caso di qualsiasi violazione `DataAnnotation` vincoli che sono stati applicati per il `UserModel` (classe), quando si invia il form, si visualizzeranno gli errori di convalida generati dal codice server.</span><span class="sxs-lookup"><span data-stu-id="f521c-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="f521c-182">Ad esempio, se si modifica il nome &quot;Ann&quot; al &quot;oggetto&quot;, quando si invia il form, viene visualizzato l'errore seguente nel form:</span><span class="sxs-lookup"><span data-stu-id="f521c-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="f521c-183">In questa esercitazione, si sta considerando il nome utente come chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="f521c-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="f521c-184">Pertanto, la proprietà del nome utente non può essere modificata.</span><span class="sxs-lookup"><span data-stu-id="f521c-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="f521c-185">Nel *Edit. cshtml* nel file, subito dopo il `Html.BeginForm` istruzione, impostare il nome utente sia un campo nascosto.</span><span class="sxs-lookup"><span data-stu-id="f521c-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="f521c-186">In questo modo la proprietà deve essere passato nel modello.</span><span class="sxs-lookup"><span data-stu-id="f521c-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="f521c-187">Il frammento di codice seguente viene illustrata la posizione del `Hidden` istruzione:</span><span class="sxs-lookup"><span data-stu-id="f521c-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="f521c-188">Sostituire il `TextBoxFor` e `ValidationMessageFor` markup per il nome utente con un `DisplayFor` chiamare.</span><span class="sxs-lookup"><span data-stu-id="f521c-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="f521c-189">Il `DisplayFor` metodo visualizza la proprietà come elemento di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="f521c-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="f521c-190">Nell'esempio seguente viene illustrato il markup completato.</span><span class="sxs-lookup"><span data-stu-id="f521c-190">The following example shows the completed markup.</span></span> <span data-ttu-id="f521c-191">Originale `TextBoxFor` e `ValidationMessageFor` come commento le chiamate con i caratteri di commento begin ed end commento Razor (`@* *@`)</span><span class="sxs-lookup"><span data-stu-id="f521c-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="f521c-192">Abilitazione della convalida lato Client</span><span class="sxs-lookup"><span data-stu-id="f521c-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="f521c-193">Per abilitare la convalida lato client in ASP.NET MVC 3, è necessario impostare due flag ed è necessario includere tre file JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f521c-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="f521c-194">Aprire l'applicazione *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="f521c-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="f521c-195">Verificare `that ClientValidationEnabled` e `UnobtrusiveJavaScriptEnabled` sono impostati su true nelle impostazioni dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f521c-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="f521c-196">Il frammento seguente dalla radice *Web. config* file Mostra le impostazioni corrette:</span><span class="sxs-lookup"><span data-stu-id="f521c-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="f521c-197">Impostazione `UnobtrusiveJavaScriptEnabled` su true, Abilita unobtrusive Ajax e la convalida del client non intrusivo.</span><span class="sxs-lookup"><span data-stu-id="f521c-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="f521c-198">Quando si usa la convalida discreta, le regole di convalida vengono convertite in attributi HTML5.</span><span class="sxs-lookup"><span data-stu-id="f521c-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="f521c-199">I nomi degli attributi HTML5 può contenere solo lettere minuscole, numeri e trattini.</span><span class="sxs-lookup"><span data-stu-id="f521c-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="f521c-200">Impostazione `ClientValidationEnabled` su true consente la convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="f521c-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="f521c-201">Tramite l'impostazione di queste chiavi nell'applicazione *Web. config* file, si abilita la convalida del client e JavaScript non intrusivo per l'intera applicazione.</span><span class="sxs-lookup"><span data-stu-id="f521c-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="f521c-202">È anche possibile abilitare o disabilitare queste impostazioni nelle singole viste o i metodi del controller utilizzando il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f521c-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="f521c-203">È anche necessario includere numerosi file JavaScript nella visualizzazione sottoposta a rendering.</span><span class="sxs-lookup"><span data-stu-id="f521c-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="f521c-204">È un modo semplice per includere il codice JavaScript in tutte le visualizzazioni per aggiungerli al *Views\Shared\\layout. cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="f521c-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="f521c-205">Sostituire il `<head>` dell'elemento di  *\_layout. cshtml* file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f521c-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="f521c-206">I primi due script jQuery sono ospitati dal contenuto Delivery Network (rete CDN Microsoft Ajax).</span><span class="sxs-lookup"><span data-stu-id="f521c-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="f521c-207">Grazie all'uso della rete CDN Microsoft Ajax, è possibile migliorare significativamente le prestazioni prima richiesta delle applicazioni.</span><span class="sxs-lookup"><span data-stu-id="f521c-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="f521c-208">Eseguire l'applicazione e fare clic su un collegamento di modifica.</span><span class="sxs-lookup"><span data-stu-id="f521c-208">Run the application and click an edit link.</span></span> <span data-ttu-id="f521c-209">Visualizzare il codice sorgente nel browser.</span><span class="sxs-lookup"><span data-stu-id="f521c-209">View the page's source in the browser.</span></span> <span data-ttu-id="f521c-210">L'origine del browser Mostra molti attributi nel formato `data-val` (convalida dei dati).</span><span class="sxs-lookup"><span data-stu-id="f521c-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="f521c-211">Quando la convalida del client e JavaScript non intrusivo è abilitato, i campi di input con una regola di convalida client contengono il `data-val="true"` attributo per attivare la convalida del client non intrusivo.</span><span class="sxs-lookup"><span data-stu-id="f521c-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="f521c-212">Ad esempio, il `City` campo del modello è stata decorata con il [obbligatorio](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attributo, che genera il codice HTML mostrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="f521c-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="f521c-213">Per ogni regola di convalida client, viene aggiunto un attributo che ha il formato `data-val-rulename="message"`.</span><span class="sxs-lookup"><span data-stu-id="f521c-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="f521c-214">Usando il `City` campo, ad esempio illustrato in precedenza, la regola di convalida client richiesta genera il `data-val-required` attributo e il messaggio &quot;City il campo è obbligatorio&quot;.</span><span class="sxs-lookup"><span data-stu-id="f521c-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="f521c-215">Eseguire l'applicazione, modificare uno degli utenti e deselezionare il `City` campo.</span><span class="sxs-lookup"><span data-stu-id="f521c-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="f521c-216">Quando si preme tab fuori dal campo, viene visualizzato un messaggio di errore di convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="f521c-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Città obbligati](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="f521c-218">Analogamente, per ogni parametro nella regola di convalida client, viene aggiunto un attributo che ha il formato `data-val-rulename-paramname=paramvalue`.</span><span class="sxs-lookup"><span data-stu-id="f521c-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="f521c-219">Ad esempio, il `FirstName` proprietà è annotata con il [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attributo e specifica una lunghezza minima pari a 3 e una lunghezza massima di 8.</span><span class="sxs-lookup"><span data-stu-id="f521c-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="f521c-220">La regola di convalida di dati denominata `length` ha il nome del parametro `max` e il valore del parametro 8.</span><span class="sxs-lookup"><span data-stu-id="f521c-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="f521c-221">Di seguito è riportato il codice HTML generato per il `FirstName` campo quando si modifica uno degli utenti:</span><span class="sxs-lookup"><span data-stu-id="f521c-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="f521c-222">Per altre informazioni sulla convalida del client non intrusivi, vedere l'intervento [Unobtrusive Validation di Client in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) nel blog di Brad Wilson.</span><span class="sxs-lookup"><span data-stu-id="f521c-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="f521c-223">Nella versione Beta di ASP.NET MVC 3, è talvolta necessario inviare il modulo per avviare la convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="f521c-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="f521c-224">Ciò potrebbe essere modificato nella versione finale.</span><span class="sxs-lookup"><span data-stu-id="f521c-224">This might be changed for the final release.</span></span>


## <a name="creating-the-create-view"></a><span data-ttu-id="f521c-225">Creazione della visualizzazione Create</span><span class="sxs-lookup"><span data-stu-id="f521c-225">Creating the Create View</span></span>

<span data-ttu-id="f521c-226">Il passaggio successivo consiste nell'aggiungere un `Create` metodo di azione e visualizzazione per consentire all'utente di creare un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="f521c-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="f521c-227">Aggiungere il codice seguente `Create` metodo al controller home:</span><span class="sxs-lookup"><span data-stu-id="f521c-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="f521c-228">Aggiunge una visualizzazione come nei passaggi precedenti, ma imposta **visualizzare il contenuto** al **crea**.</span><span class="sxs-lookup"><span data-stu-id="f521c-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Create View](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="f521c-230">Eseguire l'applicazione, selezionare la **Create** collegare e aggiungere un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="f521c-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="f521c-231">Il `Create` metodo sfrutta automaticamente la convalida lato client e lato server.</span><span class="sxs-lookup"><span data-stu-id="f521c-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="f521c-232">Provare a immettere un nome utente che contiene gli spazi vuoti, ad esempio &quot;Ben X&quot;.</span><span class="sxs-lookup"><span data-stu-id="f521c-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="f521c-233">Quando si esce dal campo nome utente, un errore di convalida lato client (`White space is not allowed`) viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="f521c-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="f521c-234">Aggiungere il metodo Delete</span><span class="sxs-lookup"><span data-stu-id="f521c-234">Add the Delete method</span></span>

<span data-ttu-id="f521c-235">Per completare l'esercitazione, aggiungere il codice seguente `Delete` metodo al controller home:</span><span class="sxs-lookup"><span data-stu-id="f521c-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="f521c-236">Aggiungere un `Delete` vista come nei passaggi precedenti, l'impostazione **visualizzare il contenuto** al **eliminare**.</span><span class="sxs-lookup"><span data-stu-id="f521c-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Elimina visualizzazione](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="f521c-238">Ora disponibile un'applicazione di ASP.NET MVC 3 semplice seppur completamente funzionale con la convalida.</span><span class="sxs-lookup"><span data-stu-id="f521c-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
