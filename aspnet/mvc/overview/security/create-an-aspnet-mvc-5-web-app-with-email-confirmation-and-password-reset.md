---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Creare un'app web ASP.NET MVC 5 sicura con accesso, inviare tramite posta elettronica di conferma e reimpostazione della password (c#) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra come compilare un'app web ASP.NET MVC 5 con conferma tramite posta elettronica e reimpostazione della password usando il sistema di appartenenze ASP.NET Identity. È ca...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 5092476c6cf59bea6fab6fa6f169ff11ec4c9c4a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57030278"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="d86c2-104">Creare un'app Web ASP.NET MVC 5 sicura con accesso, messaggi di posta elettronica di conferma e reimpostazione della password (C#)</span><span class="sxs-lookup"><span data-stu-id="d86c2-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>
====================
<span data-ttu-id="d86c2-105">da [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="d86c2-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="d86c2-106">Questa esercitazione illustra come compilare un'app web ASP.NET MVC 5 con conferma tramite posta elettronica e reimpostazione della password usando il sistema di appartenenze ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="d86c2-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span> <span data-ttu-id="d86c2-107">È possibile scaricare l'applicazione completata [qui](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="d86c2-107">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="d86c2-108">Il download contiene gli helper di debug che consentono di testare conferma tramite posta elettronica e SMS senza dover configurare un indirizzo di posta elettronica o il provider SMS.</span><span class="sxs-lookup"><span data-stu-id="d86c2-108">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="d86c2-109">Questa esercitazione è stato scritto dal [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="d86c2-109">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="d86c2-110">Creare un'app ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="d86c2-110">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="d86c2-111">Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) oppure [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="d86c2-111">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="d86c2-112">Installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="d86c2-112">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="d86c2-113">Avviso: È necessario installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva per completare questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="d86c2-113">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="d86c2-114">Creare un nuovo progetto Web ASP.NET e selezionare il modello MVC.</span><span class="sxs-lookup"><span data-stu-id="d86c2-114">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="d86c2-115">Web Form supporta inoltre ASP.NET Identity, pertanto è possibile seguire la procedura in un'applicazione web form.</span><span class="sxs-lookup"><span data-stu-id="d86c2-115">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="d86c2-116">Lasciare l'autenticazione predefinita come **singoli account utente di**.</span><span class="sxs-lookup"><span data-stu-id="d86c2-116">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="d86c2-117">Se vuoi ospitare l'app in Azure, lasciare selezionata la casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="d86c2-117">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="d86c2-118">Più avanti nell'esercitazione verrà distribuita in Azure.</span><span class="sxs-lookup"><span data-stu-id="d86c2-118">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="d86c2-119">È possibile [aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="d86c2-119">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="d86c2-120">Impostare il [progetto per l'uso di SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="d86c2-120">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="d86c2-121">Eseguire l'app, scegliere il **registrare** collegare e registrare un utente.</span><span class="sxs-lookup"><span data-stu-id="d86c2-121">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="d86c2-122">A questo punto, l'unica convalida il messaggio di posta elettronica è con il [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attributo.</span><span class="sxs-lookup"><span data-stu-id="d86c2-122">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="d86c2-123">In Esplora Server, passare a **dati Connections\DefaultConnection\Tables\AspNetUsers**, fare clic e selezionare **Apri definizione tabella**.</span><span class="sxs-lookup"><span data-stu-id="d86c2-123">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="d86c2-124">La figura seguente mostra la `AspNetUsers` dello schema:</span><span class="sxs-lookup"><span data-stu-id="d86c2-124">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="d86c2-125">Fare clic con il pulsante destro sul **AspNetUsers** tabelle e selezionare **Mostra dati tabella**.</span><span class="sxs-lookup"><span data-stu-id="d86c2-125">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="d86c2-126">A questo punto non è stato confermato l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d86c2-126">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="d86c2-127">Fare clic sulla riga e selezionare Elimina.</span><span class="sxs-lookup"><span data-stu-id="d86c2-127">Click on the row and select delete.</span></span> <span data-ttu-id="d86c2-128">Si sarà aggiungere di nuovo questo messaggio di posta elettronica nel passaggio successivo e inviare un messaggio di posta elettronica di conferma.</span><span class="sxs-lookup"><span data-stu-id="d86c2-128">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="d86c2-129">Conferma tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="d86c2-129">Email confirmation</span></span>

<span data-ttu-id="d86c2-130">È consigliabile verificare che il messaggio di posta elettronica di una nuova registrazione utente per verificare che non rappresentano un altro utente (vale a dire non hanno registrato con un altro messaggio di posta elettronica).</span><span class="sxs-lookup"><span data-stu-id="d86c2-130">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="d86c2-131">Si supponga di un forum di discussione, si desidera impedire `"bob@example.com"` tramite la registrazione come `"joe@contoso.com"`.</span><span class="sxs-lookup"><span data-stu-id="d86c2-131">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="d86c2-132">Senza conferma tramite posta elettronica, `"joe@contoso.com"` è stato possibile ottenere l'indirizzo di posta elettronica indesiderato dalla propria app.</span><span class="sxs-lookup"><span data-stu-id="d86c2-132">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="d86c2-133">Si supponga che Bob accidentalmente registrato come `"bib@example.com"` e non l'aveste notato, egli non sarebbe in grado di utilizzare ripristino password perché l'app non ha il suo indirizzo di posta elettronica corretto.</span><span class="sxs-lookup"><span data-stu-id="d86c2-133">Suppose Bob accidently registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="d86c2-134">Conferma tramite posta elettronica fornisce solo una protezione limitata dai Bot e non fornisce protezione da spammer determinato, hanno molti alias di posta elettronica di lavoro che possono usare per registrare.</span><span class="sxs-lookup"><span data-stu-id="d86c2-134">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="d86c2-135">In genere si desidera impedire ai nuovi utenti dalla registrazione di tutti i dati al sito web prima che sono state confermate tramite posta elettronica, un messaggio SMS o un altro meccanismo.</span><span class="sxs-lookup"><span data-stu-id="d86c2-135">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="d86c2-136">Nelle sezioni seguenti, verrà Abilita conferma tramite posta elettronica e modificare il codice per impedire l'accesso fino a quando non è stata confermata la posta elettronica gli utenti appena registrati.</span><span class="sxs-lookup"><span data-stu-id="d86c2-136">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="d86c2-137">Associare SendGrid</span><span class="sxs-lookup"><span data-stu-id="d86c2-137">Hook up SendGrid</span></span>

<span data-ttu-id="d86c2-138">Le istruzioni riportate in questa sezione non sono aggiornate.</span><span class="sxs-lookup"><span data-stu-id="d86c2-138">The instructions in this section are not current.</span></span> <span data-ttu-id="d86c2-139">Visualizzare [provider di posta elettronica SendGrid configurare](/aspnet/core/security/authentication/accconfirm#configure-email-provider) per aggiornare le istruzioni.</span><span class="sxs-lookup"><span data-stu-id="d86c2-139">See [Configure SendGrid email provider](/aspnet/core/security/authentication/accconfirm#configure-email-provider) for updated instructions.</span></span>

<span data-ttu-id="d86c2-140">Sebbene in questa esercitazione illustra solo come aggiungere notifica tramite posta elettronica tramite [SendGrid](http://sendgrid.com/), è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi (vedere [risorse aggiuntive](#addRes)).</span><span class="sxs-lookup"><span data-stu-id="d86c2-140">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="d86c2-141">Nella Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="d86c2-141">In the Package Manager Console, enter the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="d86c2-142">Andare alla [pagina di iscrizione Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) e registrare un account SendGrid gratuito.</span><span class="sxs-lookup"><span data-stu-id="d86c2-142">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for a free SendGrid account.</span></span> <span data-ttu-id="d86c2-143">Configurare SendGrid aggiungendo codice analogo al seguente nella *App_Start/IdentityConfig.cs*:</span><span class="sxs-lookup"><span data-stu-id="d86c2-143">Configure SendGrid by adding code similar to the following in *App_Start/IdentityConfig.cs*:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="d86c2-144">È necessario aggiungere che include quanto segue:</span><span class="sxs-lookup"><span data-stu-id="d86c2-144">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="d86c2-145">Per semplificare questo esempio, le impostazioni dell'app in verrà archiviato il *Web. config* file:</span><span class="sxs-lookup"><span data-stu-id="d86c2-145">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="d86c2-146">Security - store mai i dati sensibili nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="d86c2-146">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="d86c2-147">L'account e le credenziali vengono archiviate in appSetting.</span><span class="sxs-lookup"><span data-stu-id="d86c2-147">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="d86c2-148">In Azure, è possibile archiviare in modo sicuro questi valori sul **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** scheda nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="d86c2-148">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="d86c2-149">Visualizzare [procedure consigliate per la distribuzione di password e altri dati sensibili in ASP.NET e Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="d86c2-149">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>


### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="d86c2-150">Abilitare la conferma tramite posta elettronica nel controller Account</span><span class="sxs-lookup"><span data-stu-id="d86c2-150">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="d86c2-151">Verificare i *Views\Account\ConfirmEmail.cshtml* file ha la sintassi razor corretto.</span><span class="sxs-lookup"><span data-stu-id="d86c2-151">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="d86c2-152">(Il @ carattere nella prima riga potrebbe essere manca.</span><span class="sxs-lookup"><span data-stu-id="d86c2-152">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="d86c2-153">)</span><span class="sxs-lookup"><span data-stu-id="d86c2-153">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="d86c2-154">Eseguire l'app e fare clic sul collegamento di registrazione.</span><span class="sxs-lookup"><span data-stu-id="d86c2-154">Run the app and click the Register link.</span></span> <span data-ttu-id="d86c2-155">Dopo aver inviato il modulo di registrazione, si è connessi.</span><span class="sxs-lookup"><span data-stu-id="d86c2-155">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="d86c2-156">Verificare l'account di posta elettronica e fare clic sul collegamento per confermare l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d86c2-156">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="d86c2-157">Richiedere conferma tramite posta elettronica prima dell'accesso in</span><span class="sxs-lookup"><span data-stu-id="d86c2-157">Require email confirmation before log in</span></span>

<span data-ttu-id="d86c2-158">Attualmente una volta che un utente ha completato il modulo di registrazione, vengono registrate.</span><span class="sxs-lookup"><span data-stu-id="d86c2-158">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="d86c2-159">In genere si vuole verificare la posta elettronica prima archiviandoli.</span><span class="sxs-lookup"><span data-stu-id="d86c2-159">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="d86c2-160">Nella sezione seguente, si modificherà il codice per richiedere agli utenti di nuovo per disporre di un messaggio di posta elettronica confermato prima vengono registrate nel (autenticato).</span><span class="sxs-lookup"><span data-stu-id="d86c2-160">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="d86c2-161">Aggiornamento di `HttpPost Register` metodo con le modifiche evidenziate di seguito:</span><span class="sxs-lookup"><span data-stu-id="d86c2-161">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="d86c2-162">Impostando come commento il `SignInAsync` metodo, l'utente non essere firmata in per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="d86c2-162">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="d86c2-163">Il `TempData["ViewBagLink"] = callbackUrl;` riga può essere usata per [il debug dell'app](#dbg) e registrazione di test senza l'invio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d86c2-163">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="d86c2-164">`ViewBag.Message` Consente di visualizzare le istruzioni di conferma.</span><span class="sxs-lookup"><span data-stu-id="d86c2-164">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="d86c2-165">Il [Scarica esempio](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contiene il codice per test conferma tramite posta elettronica senza configurare posta elettronica e consente inoltre il debug dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d86c2-165">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="d86c2-166">Creare un `Views\Shared\Info.cshtml` file e aggiungere il markup razor seguente:</span><span class="sxs-lookup"><span data-stu-id="d86c2-166">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="d86c2-167">Aggiungere il [attributo Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) per il `Contact` metodo di azione del controller Home.</span><span class="sxs-lookup"><span data-stu-id="d86c2-167">Add the [Authorize attribute](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="d86c2-168">È possibile fare clic sui **contatto** collegamento per verificare gli utenti anonimi non hanno accesso e si dispongono dell'accesso agli utenti autenticati.</span><span class="sxs-lookup"><span data-stu-id="d86c2-168">You can click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="d86c2-169">È necessario aggiornare anche il `HttpPost Login` metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="d86c2-169">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="d86c2-170">Aggiorna il *Views\Shared\Error.cshtml* visualizzazione per visualizzare il messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="d86c2-170">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="d86c2-171">Eliminare gli account nel **AspNetUsers** tabella che contiene l'alias di posta elettronica si desidera eseguire il test.</span><span class="sxs-lookup"><span data-stu-id="d86c2-171">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="d86c2-172">Eseguire l'app e verificare che non è possibile accedere fino a quando non si è verificato l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d86c2-172">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="d86c2-173">Dopo aver verificato l'indirizzo di posta elettronica, scegliere il **contatto** collegamento.</span><span class="sxs-lookup"><span data-stu-id="d86c2-173">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="d86c2-174">Ripristino/reimpostazione della password</span><span class="sxs-lookup"><span data-stu-id="d86c2-174">Password recovery/reset</span></span>

<span data-ttu-id="d86c2-175">Rimuovere i caratteri di commento dal `HttpPost ForgotPassword` metodo di azione nel controller account:</span><span class="sxs-lookup"><span data-stu-id="d86c2-175">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="d86c2-176">Rimuovere i caratteri di commento dal `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) nel *Views\Account\Login.cshtml* file di visualizzazione razor:</span><span class="sxs-lookup"><span data-stu-id="d86c2-176">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="d86c2-177">Nella pagina di accesso mostrerà ora un link per reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="d86c2-177">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="d86c2-178">Inviare di nuovo collegamento di conferma tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="d86c2-178">Resend email confirmation link</span></span>

<span data-ttu-id="d86c2-179">Una volta che un utente crea un nuovo account locale, essi vengono inviati tramite posta elettronica un collegamento di conferma che viene richiesto di usare prima di poter accedere.</span><span class="sxs-lookup"><span data-stu-id="d86c2-179">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="d86c2-180">Se l'utente accidentalmente Elimina il messaggio di posta elettronica di conferma o non arrivi mai il messaggio di posta elettronica, è necessario il collegamento di conferma inviato nuovamente.</span><span class="sxs-lookup"><span data-stu-id="d86c2-180">If the user accidently deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="d86c2-181">Le modifiche al codice seguente viene illustrato come abilitare questa opzione.</span><span class="sxs-lookup"><span data-stu-id="d86c2-181">The following code changes show how to enable this.</span></span>

<span data-ttu-id="d86c2-182">Aggiungere il seguente metodo helper a fondo le *Controllers\AccountController.cs* file:</span><span class="sxs-lookup"><span data-stu-id="d86c2-182">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="d86c2-183">Aggiornare il metodo Register per usare il nuovo helper:</span><span class="sxs-lookup"><span data-stu-id="d86c2-183">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="d86c2-184">Aggiornare il metodo di accesso per inviare di nuovo la password se non è stato confermato l'account utente:</span><span class="sxs-lookup"><span data-stu-id="d86c2-184">Update the Login method to resend the password if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="d86c2-185">Combinare gli account di accesso social e locali</span><span class="sxs-lookup"><span data-stu-id="d86c2-185">Combine social and local login accounts</span></span>

<span data-ttu-id="d86c2-186">È possibile combinare account locali e basati su social network, fare clic sul collegamento nel messaggio.</span><span class="sxs-lookup"><span data-stu-id="d86c2-186">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="d86c2-187">Nella sequenza seguente **RickAndMSFT@gmail.com** viene innanzitutto creato un account di accesso locale, ma è possibile creare l'account come log basati su social network in prima, quindi aggiungere un account di accesso locale.</span><span class="sxs-lookup"><span data-stu-id="d86c2-187">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="d86c2-188">Fare clic sui **Gestisci** collegamento.</span><span class="sxs-lookup"><span data-stu-id="d86c2-188">Click on the **Manage** link.</span></span> <span data-ttu-id="d86c2-189">Si noti il **account di accesso esterni: 0** associate all'account.</span><span class="sxs-lookup"><span data-stu-id="d86c2-189">Note the **External Logins: 0** associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="d86c2-190">Fare clic sul collegamento a un altro registro nel servizio e accettare le richieste di app.</span><span class="sxs-lookup"><span data-stu-id="d86c2-190">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="d86c2-191">I due account sono stati combinati, sarà in grado di accedere con uno di questi account.</span><span class="sxs-lookup"><span data-stu-id="d86c2-191">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="d86c2-192">È possibile che gli utenti per aggiungere gli account locali nel caso in cui i log basati su social network in servizio di autenticazione è inattivo oppure più probabile che si è perso l'accesso al proprio account di social networking.</span><span class="sxs-lookup"><span data-stu-id="d86c2-192">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="d86c2-193">Nell'immagine seguente, Tom è una procedura di accesso basati su social network (che è possibile visualizzare dal **account di accesso esterni: 1** visualizzato sulla pagina).</span><span class="sxs-lookup"><span data-stu-id="d86c2-193">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="d86c2-194">Facendo clic su **Scegli una password** consente di aggiungere un registro locale in associati con lo stesso account.</span><span class="sxs-lookup"><span data-stu-id="d86c2-194">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="d86c2-195">Conferma tramite posta elettronica in modo più approfondito</span><span class="sxs-lookup"><span data-stu-id="d86c2-195">Email confirmation in more depth</span></span>

<span data-ttu-id="d86c2-196">L'esercitazione [conferma Account e recupero della Password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) entra in questo argomento con altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="d86c2-196">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="d86c2-197">Debug dell'app</span><span class="sxs-lookup"><span data-stu-id="d86c2-197">Debugging the app</span></span>

<span data-ttu-id="d86c2-198">Se non si riceve un messaggio di posta elettronica che contiene il collegamento:</span><span class="sxs-lookup"><span data-stu-id="d86c2-198">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="d86c2-199">Controllare la cartella della posta indesiderata o antispam.</span><span class="sxs-lookup"><span data-stu-id="d86c2-199">Check your junk or spam folder.</span></span>
- <span data-ttu-id="d86c2-200">Accedere all'account SendGrid e fare clic sui [collegamento di attività di posta elettronica](https://sendgrid.com/logs/index).</span><span class="sxs-lookup"><span data-stu-id="d86c2-200">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="d86c2-201">Per testare il collegamento per la verifica senza messaggio di posta elettronica, scaricare il [esempio completato](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="d86c2-201">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="d86c2-202">Nella pagina verranno visualizzati il collegamento di conferma e codici di conferma.</span><span class="sxs-lookup"><span data-stu-id="d86c2-202">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="d86c2-203">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d86c2-203">Additional Resources</span></span>

- [<span data-ttu-id="d86c2-204">Risorse consigliate su collegamenti ad ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="d86c2-204">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="d86c2-205">[Conferma dell'account e recupero della Password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) descrivono in maggiore dettaglio nella conferma account e di ripristino della password.</span><span class="sxs-lookup"><span data-stu-id="d86c2-205">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="d86c2-206">[App MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) questa esercitazione illustra come scrivere un'app ASP.NET MVC 5 con Facebook e Google OAuth 2 l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="d86c2-206">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="d86c2-207">Viene inoltre illustrato come aggiungere dati aggiuntivi al database di identità.</span><span class="sxs-lookup"><span data-stu-id="d86c2-207">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="d86c2-208">[Distribuire un'app ASP.NET MVC sicura con appartenenza, OAuth e Database SQL in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="d86c2-208">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="d86c2-209">Questa esercitazione aggiunge una distribuzione di Azure, come proteggere l'app con i ruoli, come usare l'API di appartenenza per aggiungere utenti e ruoli e funzionalità di sicurezza aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="d86c2-209">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="d86c2-210">Creazione di un'app Google per OAuth 2 e ci si connette l'app al progetto</span><span class="sxs-lookup"><span data-stu-id="d86c2-210">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="d86c2-211">Creazione dell'app in Facebook e ci si connette l'app al progetto</span><span class="sxs-lookup"><span data-stu-id="d86c2-211">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="d86c2-212">Configurare SSL nel progetto</span><span class="sxs-lookup"><span data-stu-id="d86c2-212">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)