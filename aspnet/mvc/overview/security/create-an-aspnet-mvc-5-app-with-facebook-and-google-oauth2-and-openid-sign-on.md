---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Crea l'app MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on (C Documenti Microsoft
author: Rick-Anderson
description: Questa esercitazione illustra come compilare un'applicazione Web MVC 5 ASP.NET che consente agli utenti di accedere utilizzando OAuth 2.0 con le credenziali di un'autenticazione esterna...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: dd2e55d68ceb5a90134e394c00f3a3a231cb27d6
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676321"
---
# <a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Creare un'app ASP.NET MVC 5 con l'accesso OAuth2 di Facebook, Twitter, LinkedIn e Google (C#)

da parte di [Rick Anderson](https://twitter.com/RickAndMSFT)

> Questa esercitazione illustra come creare un'applicazione Web MVC 5 ASP.NET che consente agli utenti di accedere utilizzando OAuth 2.0 con le credenziali di un provider di autenticazione esterno, ad esempio Facebook, Twitter, LinkedIn, Microsoft o Google.This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft or Google. Per semplicità, questo tutorial si concentra sul lavoro con le credenziali di Facebook e Google.
> 
> L'abilitazione di queste credenziali nei siti Web offre un vantaggio significativo perché milioni di utenti dispongono già di account con questi provider esterni. Questi utenti potrebbero essere più inclini a iscriversi al tuo sito se non devono creare e ricordare un nuovo set di credenziali.
> 
> Vedere anche [ASP.NET'app MVC 5 con SMS e posta elettronica Autenticazione](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)a due fattori .
> 
> L'esercitazione illustra anche come aggiungere i dati del profilo per l'utente e come usare l'API di appartenenza per aggiungere ruoli. Questo tutorial è stato scritto da [Rick](https://blogs.msdn.com/rickAndy) [@RickAndMSFT](https://twitter.com/RickAndMSFT) Anderson ( Si prega di seguirmi su Twitter: ).

<a id="start"></a>
## <a name="getting-started"></a>Introduzione

Iniziare installando ed eseguendo [Visual Studio Express 2013 per Web](https://go.microsoft.com/fwlink/?LinkId=299058) o Visual Studio [2013.](https://go.microsoft.com/fwlink/?LinkId=306566) Installare Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva. Per assistenza con Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! e altro ancora, vedi questo [progetto di esempio](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> È necessario installare Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva per utilizzare Google OAuth 2 e per eseguire il debug in locale senza avvisi SSL.

Fare clic su **Nuovo progetto** nella pagina **iniziale** oppure utilizzare il menu e selezionare **File**, quindi **Nuovo progetto**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Creazione della prima applicazione

Fare clic su **Nuovo progetto**, quindi selezionare **Visual C,** a sinistra, quindi **Web** e selezionare ASP.NET **applicazione Web**. Assegnare al progetto il nome "MvcAuth" e quindi fare clic su **OK**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

Nella finestra di dialogo **Nuovo progetto ASP.NET** fare clic su **MVC**. Se l'autenticazione non è **Account utente singoli**, fare clic sul pulsante Cambia **autenticazione** e selezionare **Singoli account utente**. Selezionando **Host nel cloud,** l'app sarà molto facile da ospitare in Azure.By checking Host in the cloud , the app will be very easy to host in Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Se è stata selezionata **l'opzione Host nel cloud**, completare la finestra di dialogo di configurazione.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)

### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Utilizzare NuGet per eseguire l'aggiornamento al middleware OWIN più recente

Utilizzare gestione pacchetti NuGet per aggiornare il [middleware OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Seleziona **Aggiornamenti** nel menu a sinistra. È possibile fare clic sul pulsante **Aggiorna tutto** oppure cercare solo i pacchetti OWIN (mostrati nell'immagine successiva):

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

Nell'immagine seguente, vengono mostrati solo i pacchetti OWIN:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

Dalla Console di gestione pacchetti (PMC), è possibile immettere il `Update-Package` comando, che aggiornerà tutti i pacchetti.

Premere **F5** o **Ctrl . F5** per eseguire l'applicazione. Nell'immagine seguente, il numero di porta è 1234. Quando si esegue l'applicazione, viene visualizzato un numero di porta diverso.

A seconda delle dimensioni della finestra del browser, potrebbe essere necessario fare clic sull'icona di navigazione per visualizzare i collegamenti **Home**, **Informazioni**, **Contatti**, **Registrati** e **Accedi** .

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Configurazione di SSL nel progetto

Per connetterti a provider di autenticazione come Google e Facebook, dovrai configurare IIS-Express per l'utilizzo di SSL. È importante continuare a utilizzare SSL dopo il login e non tornare a HTTP, il cookie di accesso è segreto come il nome utente e la password, e senza utilizzare SSL si sta inviando in testo non crittografato attraverso la rete. Inoltre, hai già preso il tempo per eseguire l'handshake e proteggere il canale (che è la maggior parte di ciò che rende HTTPS più lento di HTTP) prima che venga eseguita la pipeline MVC, pertanto il reindirizzamento a HTTP dopo aver effettuato l'accesso non renderà la richiesta corrente o le richieste future molto più veloce.

1. In **Esplora soluzioni**fare clic sul progetto **MvcAuth.**
2. Premi il tasto F4 per mostrare le proprietà del progetto. In alternativa, dal menu **Visualizza** è possibile selezionare **Finestra Proprietà**.
3. Impostare **SSL abilitato su** True.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Copiare l'URL SSL `https://localhost:44300/` (che sarà a meno che non sono stati creati altri progetti SSL).
5. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **MvcAuth** e scegliere **Proprietà**.
6. Selezionare la scheda **Web** e quindi incollare l'URL SSL nella casella **URL progetto.** Salvare il file (Ctl-S). Avrai bisogno di questo URL per configurare le app di autenticazione di Facebook e Google.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Aggiungere l'attributo `Home` [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) al controller per richiedere che tutte le richieste utilizzino HTTPS. Un approccio più sicuro consiste nell'aggiungere il filtro [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) all'applicazione. Vedere la &quot;sezione Proteggere l'applicazione&quot; con SSL e l'attributo Authorize nell'esercitazione [Creare un'app MVC ASP.NET con autenticazione e database SQL e distribuire nel servizio app di Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Una parte del controller Home è mostrata di seguito.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Premere CTRL+F5 per eseguire l'applicazione. Se hai installato il certificato in passato, puoi ignorare il resto di questa sezione e passare a Creazione di [un'app Google per OAuth 2 e connettere l'app al progetto,](#goog)in caso contrario, segui le istruzioni per considerare attendibile il certificato autofirmato generato da IIS Express.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Leggere la finestra di dialogo **Avviso** di sicurezza e quindi fare clic su **Sì** se si desidera installare il certificato che rappresenta localhost.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. In Internet Explorer viene visualizzata la pagina *Home* e non viene visualizzato alcun avviso SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome accetta anche il certificato e mostrerà contenuti HTTPS senza alcun avviso. Firefox utilizza il proprio archivio certificati, in modo da visualizzare un avviso. Per la nostra applicazione si può tranquillamente fare clic **Capire i rischi**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Creazione di un'app Google per OAuth 2 e connessione dell'app al progetto

> [!WARNING]
> Per le istruzioni OAuth di Google correnti, consultate [Configurazione dell'autenticazione Google in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).

1. Passare a [Google Developers Console](https://console.developers.google.com/).
2. Se non è stato creato un progetto in precedenza, selezionare **Credenziali** nella scheda sinistra e quindi selezionare **Crea**.
3. Nella scheda sinistra fare clic su **Credenziali**.
4. Fare clic su **Crea credenziali,** quindi su **ID client OAuth**. 

    1. Nella finestra di dialogo **Crea ID client** mantenere **l'applicazione Web** predefinita per il tipo di applicazione.
    2. Imposta le origini **JavaScript autorizzate** sull'URL`https://localhost:44300/` SSL che hai usato sopra (a meno che tu non abbia creato altri progetti SSL)
    3. Impostare **l'URI di reindirizzamento autorizzato** su:Set the Authorized redirect URI to:  
         `https://localhost:44300/signin-google`
5. Fai clic sulla voce di menu della schermata Consenso OAuth, quindi imposta il tuo indirizzo email e il nome del prodotto. Dopo aver compilato il modulo, fare clic su **Salva**.
6. Fai clic sulla voce di menu Libreria, cerca **nell'API Google**, fai clic su di essa, quindi premi Abilita.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   L'immagine seguente mostra le API abilitate.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Dal Gestore API api google API, visitare la scheda **Credenziali** per ottenere l'ID **client**. Scaricare per salvare un file JSON con segreti dell'applicazione. Copiare e incollare **ClientId** `UseGoogleAuthentication` e **ClientSecret** nel metodo presente nel file *di Startup.Auth.cs* nella cartella *App_Start.* I valori **ClientId** e **ClientSecret** illustrati di seguito sono esempi e non funzionano.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Sicurezza: non archiviare mai i dati sensibili nel codice sorgente. L'account e le credenziali vengono aggiunti al codice precedente per mantenere semplice l'esempio. Vedere [Procedure consigliate per la distribuzione di password e altri dati sensibili in ASP.NET e](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)servizio app di Azure.
8. Per compilare ed eseguire l'applicazione, premere **CTRL e F5.** Fare clic sul collegamento **Accedi** .  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. In **Utilizzare un altro servizio per accedere,** fai clic su **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Se si perde uno dei passaggi precedenti si otterrà un errore HTTP 401. Ricontrolla i passaggi precedenti. Se perdi un'impostazione obbligatoria (ad esempio **il nome del prodotto),** aggiungi l'elemento mancante e salva; l'utilizzo dell'autenticazione può richiedere alcuni minuti.
10. Sarai reindirizzato al sito di Google dove inserisci le tue credenziali.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Dopo avere immesso le credenziali, verrà richiesto di concedere le autorizzazioni all'applicazione Web appena creata:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Fare clic su **Accetta**. Verrà ora reindirizzato alla pagina **Register** dell'applicazione MvcAuth in cui è possibile registrare l'account Google. Sarà possibile modificare il nome di registrazione dell'indirizzo di posta elettronica locale usato per l'account Gmail, anche se in generale è preferibile mantenere l'alias di posta elettronica predefinito, ovvero quello usato per l'autenticazione. Fare clic su **Register**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Creazione dell'app in Facebook e connessione dell'app al progetto

> [!WARNING]
> Per le istruzioni di autenticazione OAuth2 di Facebook correnti, vedere [Configurazione dell'autenticazione](/aspnet/core/security/authentication/social/facebook-logins) di Facebook

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Esaminare i dati di appartenenzaExamine the Membership Data

Scegliere **Esplora server**dal menu **Visualizza** .

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Espandere **DefaultConnection (MvcAuth)**, **Tabelle**, fare clic con il pulsante destro del mouse su **AspNetUsers** e **scegliere Mostra dati tabella**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![dati della tabella aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Aggiunta di dati di profilo alla classe utente

In questa sezione si aggiungeranno la data di nascita e la città natale ai dati utente durante la registrazione, come illustrato nell'immagine seguente.

![reg con città natale e Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Aprire il file *Models-IdentityModels.cs* e aggiungere la data di nascita e le proprietà della città principale:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Aprire il file *Models-AccountViewModels.cs* e le proprietà `ExternalLoginConfirmationViewModel`della data di nascita e della città principale in .

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Aprire il file *Controllers-AccountController.cs* e aggiungere il codice `ExternalLoginConfirmation` per la data di nascita e la città natale nel metodo di azione come illustrato:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Aggiungere la data di nascita e la città principale al file *Views-Account/ExternalLoginConfirmation.cshtml:*

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Elimina il database delle appartenenze in modo da poter registrare nuovamente il tuo account Facebook con la tua applicazione e verificare che puoi aggiungere le nuove informazioni sulla data di nascita e sul profilo della città natale.

In **Esplora soluzioni**fare clic sull'icona **Mostra tutti i file** , quindi fare clic con il pulsante destro del mouse su Aggiungi *\_dati,&lt;adaspnet-MvcAuth- dateStamp&gt;.mdf* e scegliere **Elimina**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Dal menu **Strumenti** , fare clic su **NuGet Package Manager**, quindi fare clic su Console di gestione **pacchetti** (PMC). Immettere i seguenti comandi nel PMC.

1. Abilitazione delle migrazioniEnable-Migrations
2. Init di migrazione dei componenti aggiuntiviAdd-Migration Init
3. Database di aggiornamento

Esegui l'applicazione e utilizza FaceBook e Google per accedere e registrare alcuni utenti.

## <a name="examine-the-membership-data"></a>Esaminare i dati di appartenenzaExamine the Membership Data

Scegliere **Esplora server**dal menu **Visualizza** .

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Fare clic con il pulsante destro del mouse su **AspNetUsers** e **scegliere Mostra dati tabella**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

I `HomeTown` `BirthDate` campi e sono mostrati di seguito.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Disconnessione dell'app e accesso con un altro account

Se accedi alla tua app con Facebook e poi ti disconnetti e tenti di accedere di nuovo con un account Facebook diverso (utilizzando lo stesso browser), sarai immediatamente connesso al precedente account Facebook che hai utilizzato. Per utilizzare un altro account, è necessario accedere a Facebook e disconnettersi da Facebook. La stessa regola si applica a qualsiasi altro provider di autenticazione di terze parti. In alternativa, è possibile accedere con un altro account utilizzando un browser diverso.

## <a name="next-steps"></a>Passaggi successivi

Vedere Introduzione ai provider di [sicurezza OAuth di Yahoo e LinkedIn per OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) di Jerrie Pelser per le istruzioni di Yahoo e LinkedIn. Vedere Pulsanti di accesso piuttosto social di Jerrie per ASP.NET MVC 5 per ottenere pulsanti di accesso social di abilitazione.

Seguire il mio tutorial [Creare un'app MVC ASP.NET con autenticazione e DATABASE SQL e distribuire al servizio app di Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), che continua questa esercitazione e mostra quanto segue:Follow my tutorial Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service , which continues this tutorial and shows the following:

1. Come distribuire l'app in Azure.How to deploy your app to Azure.
2. Come proteggere l'app con ruoli.
3. Come proteggere l'app con i filtri [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) e [Authorize.](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx)
4. Come utilizzare l'API di appartenenza per aggiungere utenti e ruoli.

Si prega di lasciare un feedback su come ti è piaciuto questo tutorial e cosa potremmo migliorare. È inoltre possibile richiedere nuovi argomenti in [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Puoi anche chiedere e votare nuove funzionalità da aggiungere ai ASP.NET. Ad esempio, è possibile votare per uno strumento per [creare e gestire utenti e ruoli.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Per una buona spiegazione del funzionamento ASP.NET servizi di autenticazione esterni, vedere Servizi di [autenticazione esterni](https://asp.net/web-api/overview/security/external-authentication-services)di Robert McMurray . L'articolo di Robert entra anche in dettaglio nell'abilitazione dell'autenticazione di Microsoft e Twitter. [Eccellente esercitazione Di EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) di Tom Dykstra viene illustrato come utilizzare Entity Framework.
