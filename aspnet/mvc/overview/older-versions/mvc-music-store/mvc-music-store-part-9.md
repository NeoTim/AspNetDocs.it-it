---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: "Parte 9: Registrazione e l'estrazione | Microsoft Docs"
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 9 vengono illustrate la registrazione e l'estrazione.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: c7151351b087439f17457b254cd9e373af21cae3
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59380899"
---
# <a name="part-9-registration-and-checkout"></a><span data-ttu-id="f3930-104">Parte 9: Registrazione e completamento della transazione</span><span class="sxs-lookup"><span data-stu-id="f3930-104">Part 9: Registration and Checkout</span></span>

<span data-ttu-id="f3930-105">by [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="f3930-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="f3930-106">Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.</span><span class="sxs-lookup"><span data-stu-id="f3930-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="f3930-107">Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="f3930-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="f3930-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="f3930-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="f3930-109">Parte 9 vengono illustrate la registrazione e l'estrazione.</span><span class="sxs-lookup"><span data-stu-id="f3930-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="f3930-110">In questa sezione verranno creati un CheckoutController che raccoglierà le informazioni di pagamento e indirizzo dell'acquirente.</span><span class="sxs-lookup"><span data-stu-id="f3930-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="f3930-111">Sarà necessario agli utenti di registrarsi con il nostro sito prima dell'estrazione, in modo che questo controller richiederà l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="f3930-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="f3930-112">Gli utenti si sposterà al processo di estrazione dal carrello acquisti facendo clic sul pulsante "Checkpoint".</span><span class="sxs-lookup"><span data-stu-id="f3930-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="f3930-113">Se l'utente non è connesso, verrà chiesto.</span><span class="sxs-lookup"><span data-stu-id="f3930-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="f3930-114">Dopo l'accesso, all'utente viene quindi visualizzato il pagamento e indirizzo nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f3930-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="f3930-115">Una volta che il modulo compilato e ha inviato l'ordine, verrà visualizzata la schermata di conferma dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="f3930-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="f3930-116">Prova a visualizzare un ordine non esistente o un ordine in cui non appartiene all'utente visualizzerà la visualizzazione di errore.</span><span class="sxs-lookup"><span data-stu-id="f3930-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="f3930-117">Eseguire la migrazione del carrello degli acquisti</span><span class="sxs-lookup"><span data-stu-id="f3930-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="f3930-118">Durante il processo di acquisto è anonimo, quando l'utente fa clic sul pulsante con checkpoint, sarà necessario registrare ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="f3930-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="f3930-119">Gli utenti si aspettano che si manterrà le informazioni relative al carrello acquisti tra visite, pertanto è necessario associare le informazioni relative al carrello acquisti un utente quando vengono completate registrazione o accesso.</span><span class="sxs-lookup"><span data-stu-id="f3930-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="f3930-120">Si tratta in realtà molto semplice, come la classe ShoppingCart già dispone di un metodo quale assocerà tutti gli elementi nel carrello corrente con un nome utente.</span><span class="sxs-lookup"><span data-stu-id="f3930-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="f3930-121">È necessario semplicemente chiamare questo metodo quando un utente ha completato la registrazione o accesso.</span><span class="sxs-lookup"><span data-stu-id="f3930-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="f3930-122">Aprire il **AccountController** classe che abbiamo aggiunto quando si stavamo si configura l'appartenenza e l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="f3930-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="f3930-123">Aggiungere un usando l'istruzione che fa riferimento MvcMusicStore.Models, quindi aggiungere il metodo MigrateShoppingCart seguente:</span><span class="sxs-lookup"><span data-stu-id="f3930-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="f3930-124">Successivamente, modificare l'azione di post di accesso per chiamare MigrateShoppingCart dopo che l'utente è stato convalidato, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="f3930-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="f3930-125">Apportare la stessa modifica al registro Registra azione, subito dopo è stato creato l'account utente:</span><span class="sxs-lookup"><span data-stu-id="f3930-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="f3930-126">Questo è tutto - è ora un carrello acquisti anonimo verrà automaticamente trasferito a un account utente al termine della registrazione o accesso.</span><span class="sxs-lookup"><span data-stu-id="f3930-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="f3930-127">Creazione di CheckoutController</span><span class="sxs-lookup"><span data-stu-id="f3930-127">Creating the CheckoutController</span></span>

<span data-ttu-id="f3930-128">Fare doppio clic sulla cartella controller e aggiungere un nuovo Controller al progetto denominato CheckoutController usando il modello controller vuoto.</span><span class="sxs-lookup"><span data-stu-id="f3930-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="f3930-129">In primo luogo, aggiungere l'attributo Authorize sopra la dichiarazione di classe Controller in modo da richiedere agli utenti di registrarsi prima dell'estrazione:</span><span class="sxs-lookup"><span data-stu-id="f3930-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*<span data-ttu-id="f3930-130">Nota: Come avviene per la modifica che sono state apportate in precedenza al StoreManagerController, ma in tal caso l'attributo Authorize richiesto che l'utente sia in un ruolo di amministratore.</span><span class="sxs-lookup"><span data-stu-id="f3930-130">Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role.</span></span> <span data-ttu-id="f3930-131">Nel Controller di estrazione, si sta richiedendo l'utente di connettersi, ma che non richiedono che si trovino gli amministratori.</span><span class="sxs-lookup"><span data-stu-id="f3930-131">In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.</span></span>*

<span data-ttu-id="f3930-132">Per ragioni di semplicità, Microsoft non sarà a che fare con le informazioni di pagamento in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="f3930-132">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="f3930-133">Al contrario, si consente agli utenti di estrarre usando un codice promozionale.</span><span class="sxs-lookup"><span data-stu-id="f3930-133">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="f3930-134">Si archivierà il codice promozionale tramite una costante denominata codice di promozione.</span><span class="sxs-lookup"><span data-stu-id="f3930-134">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="f3930-135">Come in StoreController, si dichiara un campo per conservare un'istanza della classe MusicStoreEntities, denominata storeDB.</span><span class="sxs-lookup"><span data-stu-id="f3930-135">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="f3930-136">Per rendere utilizza la classe MusicStoreEntities, è necessario aggiungere using istruzione dello spazio dei nomi MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="f3930-136">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="f3930-137">Di seguito è riportata la parte superiore del controller Checkout.</span><span class="sxs-lookup"><span data-stu-id="f3930-137">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="f3930-138">Il CheckoutController avrà le azioni del controller seguente:</span><span class="sxs-lookup"><span data-stu-id="f3930-138">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="f3930-139">**AddressAndPayment (GET method)** verrà visualizzato un form per consentire all'utente di immettere le informazioni.</span><span class="sxs-lookup"><span data-stu-id="f3930-139">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="f3930-140">**AddressAndPayment (metodo POST)** verrà convalidato l'input e di elaborare l'ordine.</span><span class="sxs-lookup"><span data-stu-id="f3930-140">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="f3930-141">**Completa** verranno visualizzati dopo che un utente viene completato il processo di estrazione.</span><span class="sxs-lookup"><span data-stu-id="f3930-141">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="f3930-142">Questa visualizzazione includerà numero d'ordine dell'utente, come conferma.</span><span class="sxs-lookup"><span data-stu-id="f3930-142">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="f3930-143">In primo luogo, è possibile rinominare l'azione Index del controller (che è stata generata durante la creazione del controller) in AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="f3930-143">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="f3930-144">Questa azione del controller Visualizza solo il modulo di estrazione, in modo che non richieda le informazioni sul modello.</span><span class="sxs-lookup"><span data-stu-id="f3930-144">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="f3930-145">Il metodo POST AddressAndPayment seguirà lo stesso modello abbiamo utilizzato la StoreManagerController: verrà effettuato un tentativo accettare l'invio del form e completare l'ordine e verrà nuovamente visualizzato il form in caso di errore.</span><span class="sxs-lookup"><span data-stu-id="f3930-145">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="f3930-146">Dopo la convalida dell'input di modulo soddisfa i requisiti di convalida per un ordine, si controllerà direttamente il valore di modulo di codice di promozione.</span><span class="sxs-lookup"><span data-stu-id="f3930-146">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="f3930-147">Presupponendo che tutto sia corretto, che si salverà le informazioni aggiornate con l'ordine, indicare l'oggetto ShoppingCart per completare il processo dell'ordine e il reindirizzamento all'azione di completamento.</span><span class="sxs-lookup"><span data-stu-id="f3930-147">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="f3930-148">Al completamento del processo di estrazione, gli utenti verranno reindirizzati per l'azione del controller completo.</span><span class="sxs-lookup"><span data-stu-id="f3930-148">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="f3930-149">Questa azione eseguirà un controllo semplice per verificare che l'ordine appartenga effettivamente l'utente ha eseguito l'accesso prima di visualizzare il numero dell'ordine come un messaggio di conferma.</span><span class="sxs-lookup"><span data-stu-id="f3930-149">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*<span data-ttu-id="f3930-150">Nota: Visualizzazione dell'errore è stata creata automaticamente per noi nella cartella /Views/Shared quando abbiamo iniziato con il progetto.</span><span class="sxs-lookup"><span data-stu-id="f3930-150">Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.</span></span>*

<span data-ttu-id="f3930-151">Il codice CheckoutController completo è come segue:</span><span class="sxs-lookup"><span data-stu-id="f3930-151">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="f3930-152">Aggiunta della visualizzazione AddressAndPayment</span><span class="sxs-lookup"><span data-stu-id="f3930-152">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="f3930-153">A questo punto, è possibile creare la vista AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="f3930-153">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="f3930-154">Fare clic su una delle azioni controller AddressAndPayment e aggiungere una visualizzazione denominata AddressAndPayment che è fortemente tipizzata come un ordine e Usa il modello di modifica, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="f3930-154">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="f3930-155">In questa vista renderà usare due delle tecniche di cui è stato esaminato durante la compilazione della visualizzazione StoreManagerEdit:</span><span class="sxs-lookup"><span data-stu-id="f3930-155">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="f3930-156">Si userà Html.EditorForModel() per visualizzare i campi del modulo per il modello di ordine</span><span class="sxs-lookup"><span data-stu-id="f3930-156">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="f3930-157">Si userà le regole di convalida utilizzando una classe Order con gli attributi di convalida</span><span class="sxs-lookup"><span data-stu-id="f3930-157">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="f3930-158">Si inizierà aggiornando il codice del modulo per l'uso Html.EditorForModel(), seguito da una casella di testo aggiuntiva per il codice promozionale.</span><span class="sxs-lookup"><span data-stu-id="f3930-158">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="f3930-159">Seguito è riportato il codice completo per la visualizzazione AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="f3930-159">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="f3930-160">Definizione delle regole di convalida per l'ordine</span><span class="sxs-lookup"><span data-stu-id="f3930-160">Defining validation rules for the Order</span></span>

<span data-ttu-id="f3930-161">Ora che la vista è configurato, si configurerà le regole di convalida per il nostro modello ordine come in precedenza per il modello di Album.</span><span class="sxs-lookup"><span data-stu-id="f3930-161">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="f3930-162">Fare doppio clic sulla cartella modelli e aggiungere una classe denominata dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="f3930-162">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="f3930-163">Oltre agli attributi di convalida che dell'album usati in precedenza, abbiamo inoltre utilizzerà un'espressione regolare per convalidare l'indirizzo di posta elettronica dell'utente.</span><span class="sxs-lookup"><span data-stu-id="f3930-163">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="f3930-164">Tentativo di inviare il modulo con mancanti o informazioni non valide a questo punto verranno visualizzato il messaggio di errore utilizzando la convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="f3930-164">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="f3930-165">Bene, abbiamo eseguito la maggior parte del lavoro per il processo di estrazione. abbiamo appena pochi odds and termina alla fine.</span><span class="sxs-lookup"><span data-stu-id="f3930-165">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="f3930-166">È necessario aggiungere due visualizzazioni semplici ed è necessario occuparsi di consegna delle informazioni carrello durante il processo di accesso.</span><span class="sxs-lookup"><span data-stu-id="f3930-166">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="f3930-167">Aggiunta della visualizzazione completa di estrazione</span><span class="sxs-lookup"><span data-stu-id="f3930-167">Adding the Checkout Complete view</span></span>

<span data-ttu-id="f3930-168">La vista completa di estrazione è piuttosto semplice, poiché è sufficiente visualizzare l'ordine di ID.</span><span class="sxs-lookup"><span data-stu-id="f3930-168">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="f3930-169">Fare doppio clic sull'azione di completamento controller e aggiungere una visualizzazione denominata completa che è fortemente tipizzata come valore int.</span><span class="sxs-lookup"><span data-stu-id="f3930-169">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="f3930-170">A questo punto si aggiornerà il codice di visualizzazione per visualizzare l'ID dell'ordine, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="f3930-170">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="f3930-171">Aggiornamento della vista dell'errore</span><span class="sxs-lookup"><span data-stu-id="f3930-171">Updating The Error view</span></span>

<span data-ttu-id="f3930-172">Il modello predefinito include una visualizzazione di errori nella cartella views condiviso in modo che possa essere usato nuovamente in un' posizione nel sito.</span><span class="sxs-lookup"><span data-stu-id="f3930-172">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="f3930-173">In questa vista di errore contiene un errore molto semplice e non usa il nostro sito Layout, in modo che verrà aggiornata in.</span><span class="sxs-lookup"><span data-stu-id="f3930-173">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="f3930-174">Poiché si tratta di una pagina di errore generico, il contenuto è molto semplice.</span><span class="sxs-lookup"><span data-stu-id="f3930-174">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="f3930-175">Verrà incluso un messaggio e un collegamento per passare alla pagina precedente nella cronologia se l'utente vuole riprovare l'azione.</span><span class="sxs-lookup"><span data-stu-id="f3930-175">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> <span data-ttu-id="f3930-176">[Precedente](mvc-music-store-part-8.md)
> [Successivo](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="f3930-176">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
