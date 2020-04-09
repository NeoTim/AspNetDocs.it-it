---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Pagamento e pagamento con PayPal Documenti Microsoft
author: Erikre
description: Questa serie di esercitazioni insegnerà le nozioni di base per la creazione di un'applicazione Web Form ASP.NET utilizzando ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per noi...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 62d00a86c6c5845fb894896df65002c7086d039f
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676055"
---
# <a name="checkout-and-payment-with-paypal"></a>Completamento della transazione e pagamento con PayPal

da parte di [Erik Reitan](https://github.com/Erikre)

[Scarica Wingtip Toys Sample Project (C)](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [Scarica E-book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET utilizzando ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Per accompagnare questa serie di esercitazioni, è disponibile un progetto di Visual Studio 2013 [con codice sorgente C .](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)

Questa esercitazione descrive come modificare l'applicazione di esempio Wingtip Toys per includere l'autorizzazione utente, la registrazione e il pagamento tramite PayPal.This tutorial describes how to modify the Wingtip Toys sample application to include user authorization, registration, and payment using PayPal. Solo gli utenti che hanno effettuato l'accesso avranno l'autorizzazione per l'acquisto di prodotti. La funzionalità di registrazione utente incorporata del modello di progetto Web Form ASP.NET 4.5 include già gran parte di ciò che è necessario. Si aggiungerà la funzionalità PayPal Express Checkout. In questa esercitazione si utilizza l'ambiente di test per sviluppatori PayPal, quindi non verranno trasferiti fondi effettivi. Alla fine dell'esercitazione, testerai l'applicazione selezionando i prodotti da aggiungere al carrello, facendo clic sul pulsante checkout e trasferendo i dati al sito web di test di PayPal. Sul sito web di test PayPal, confermerai le informazioni di spedizione e pagamento e tornerai alla domanda di esempio Locale Wingtip Toys per confermare e completare l'acquisto.

Ci sono diversi processori di pagamento di terze parti esperti che si specializzano in shopping online che affrontano la scalabilità e la sicurezza. ASP.NET sviluppatori di ASP.NET dovrebbero considerare i vantaggi di utilizzare una soluzione di pagamento di terze parti prima di implementare una soluzione di acquisto e acquisto.

> [!NOTE] 
> 
> L'applicazione di esempio Wingtip Toys è stata progettata per mostrare concetti e funzionalità di ASP.NET specifici disponibili per ASP.NET gli sviluppatori Web. Questa applicazione di esempio non è stata ottimizzata per tutte le possibili circostanze in materia di scalabilità e sicurezza.

## <a name="what-youll-learn"></a>Contenuto dell'esercitazione:

- Come limitare l'accesso a pagine specifiche in una cartella.
- Come creare un carrello della spesa noto da un carrello della spesa anonimo.
- Come abilitare SSL per il progetto.
- Come aggiungere un provider OAuth al progetto.
- Come utilizzare PayPal per acquistare prodotti utilizzando l'ambiente di test PayPal.
- Come visualizzare i dettagli da PayPal in un controllo **DetailsView.**
- Come aggiornare il database dell'applicazione Wingtip Toys con i dettagli ottenuti da PayPal.

## <a name="adding-order-tracking"></a>Aggiunta di tracciabilità ordine

In questa esercitazione verranno create due nuove classi per tenere traccia dei dati dall'ordine creato da un utente. Le classi monitoreranno i dati relativi alle informazioni di spedizione, al totale dell'acquisto e alla conferma del pagamento.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Aggiungere le classi del modello Order e OrderDetailAdd the Order and OrderDetail Model Classes

In precedenza in questa serie di esercitazioni è stato definito `Category`lo `Product`schema `CartItem` per le categorie, i prodotti e gli elementi del carrello mediante la creazione delle classi , e nella cartella *Models* . A questo punto si aggiungeranno due nuove classi per definire lo schema per l'ordine dei prodotti e i dettagli dell'ordine.

1. Nella cartella **Modelli** aggiungere una nuova classe denominata *Order.cs*.   
   Il nuovo file di classe viene visualizzato nell'editor.
2. Sostituire il codice predefinito con quello riportato di seguito:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Aggiungere *una* classe OrderDetail.cs alla cartella *Models.Add* an OrderDetail.cs class to the Models folder.
4. Sostituire il codice predefinito con il codice seguente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

Le `Order` `OrderDetail` classi e contengono lo schema per definire le informazioni sull'ordine utilizzate per l'acquisto e la spedizione.

Inoltre, sarà necessario aggiornare la classe di contesto del database che gestisce le classi di entità e che fornisce l'accesso ai dati al database. A tale scopo, si aggiungeranno `OrderDetail` le classi `ProductContext` Order e model appena create alla classe.

1. In **Esplora soluzioni**individuare e aprire il file *di ProductContext.cs.*
2. Aggiungere il codice evidenziato al file *ProductContext.cs* come illustrato di seguito:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Come accennato in precedenza in questa serie `System.Data.Entity` di esercitazioni, il codice nel file di *ProductContext.cs* aggiunge lo spazio dei nomi in modo da avere accesso a tutte le funzionalità di base di Entity Framework. Questa funzionalità include la possibilità di eseguire query, inserire, aggiornare ed eliminare dati utilizzando oggetti fortemente tipizzati. Il codice precedente `ProductContext` nella classe aggiunge l'accesso `OrderDetail` Entity Framework alle classi e e appena aggiunte. `Order`

## <a name="adding-checkout-access"></a>Aggiunta dell'accesso all'estrazione

L'applicazione di esempio Wingtip Toys consente agli utenti anonimi di rivedere e aggiungere prodotti a un carrello acquisti. Tuttavia, quando gli utenti anonimi scelgono di acquistare i prodotti aggiunti al carrello, devono accedere al sito. Una volta effettuato l'accesso, possono accedere alle pagine con restrizioni dell'applicazione Web che gestiscono il processo di estrazione e acquisto. Queste pagine con restrizioni sono contenute nella cartella *Checkout* dell'applicazione.

### <a name="add-a-checkout-folder-and-pages"></a>Aggiungere una cartella e pagine di estrazione

Ora creerai la cartella *Checkout* e le pagine in essa in essa in essa in essa contenuto che il cliente vedrà durante il processo di checkout. Queste pagine verranno aggiornate più avanti in questa esercitazione.

1. Fare clic con il pulsante destro del mouse sul nome del progetto (**Wingtip Toys**) in **Esplora soluzioni** e selezionare **Aggiungi nuova cartella**. 

    ![Checkout e pagamento con PayPal - Nuova cartella](checkout-and-payment-with-paypal/_static/image1.png)
2. Assegnare alla nuova cartella il nome *Estrai*.
3. Fare clic con il pulsante destro del mouse sulla cartella *Checkout,* quindi scegliere **Aggiungi**-&gt;**nuovo elemento**. 

    ![Checkout e pagamento con PayPal - Nuovo articolo](checkout-and-payment-with-paypal/_static/image2.png)
4. La finestra di dialogo **Aggiungi nuovo elemento** viene visualizzata.
5. Selezionare il gruppo di modelli **Web** **di Visual C.**  - &gt; Quindi, dal riquadro centrale, selezionare **Web Form con pagina master**e denominarlo *CheckoutStart.aspx*. 

    ![Checkout e pagamento con PayPal - Finestra di dialogo Aggiungi nuovo elemento](checkout-and-payment-with-paypal/_static/image3.png)
6. Come in precedenza, selezionare il file *Site.Master* come pagina master.
7. Aggiungere le seguenti pagine aggiuntive alla cartella *Checkout* utilizzando la stessa procedura descritta in precedenza:   

    - CheckReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Aggiungere un file Web.config

Aggiungendo un nuovo file *Web.config* alla cartella *Checkout,* sarà possibile limitare l'accesso a tutte le pagine contenute nella cartella.

1. Fare clic con il pulsante destro del mouse sulla cartella *Checkout* e selezionare **Aggiungi**  - &gt; **nuovo elemento**.  
   La finestra di dialogo **Aggiungi nuovo elemento** viene visualizzata.
2. Selezionare il gruppo di modelli **Web** **di Visual C.**  - &gt; Quindi, nel riquadro centrale selezionare File di **configurazione Web**, accettare il nome predefinito *Web.config*, quindi scegliere **Aggiungi**.
3. Sostituire il contenuto XML esistente nel file *Web.config* con il contenuto seguente:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Salvare il file *Web.config*.

Il file *Web.config* specifica che a tutti gli utenti sconosciuti dell'applicazione Web deve essere negato l'accesso alle pagine contenute nella cartella *Checkout.* Tuttavia, se l'utente ha registrato un account ed è connesso, sarà un utente noto e avrà accesso alle pagine nella cartella *Checkout.*

È importante notare che ASP.NET configurazione segue una gerarchia, in cui ogni file *Web.config* applica le impostazioni di configurazione alla cartella in cui si trova e a tutte le directory figlio sottostanti.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Abilitare SSL per il progetto

 Secure Sockets Layer (SSL) è un protocollo definito per consentire ai server Web e ai client Web di comunicare in modo più sicuro tramite l'uso della crittografia. Quando non si usa SSL, i dati inviati tra il client e il server possono essere soggetti allo sniffing dei pacchetti da qualsiasi soggetto con accesso fisico alla rete. Inoltre, numerosi schemi di autenticazione comuni non sono sicuri sul protocollo HTTP normale. In particolare, l'autenticazione di base e l'autenticazione basata su form inviano credenziali non crittografate. Per essere sicuri, questi schemi di autenticazione devono usare il protocollo SSL. 

1. In **Esplora soluzioni**fare clic sul progetto **WingtipToys,** quindi premere **F4** per visualizzare la finestra **Proprietà.**
2. Impostare SSL `true`abilitato **su** .
3. Copiare **l'URL SSL** in modo da poterlo utilizzare in un secondo momento.   
 L'URL SSL `https://localhost:44300/` sarà a meno che non sia stato precedentemente creato siti Web SSL (come illustrato di seguito).   
    ![Proprietà del progetto](checkout-and-payment-with-paypal/_static/image4.png)
4. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **WingtipToys** e **scegliere Proprietà**.
5. Nella scheda a sinistra fare clic su **Web**.
6. Modificare **l'URL del progetto** per utilizzare l'URL **SSL** salvato in precedenza.   
    ![Proprietà del progetto Web](checkout-and-payment-with-paypal/_static/image5.png)
7. Per salvare la pagina, premere **CTRL+S**.
8. Premete **Ctrl e F5** per eseguire l'applicazione. In Visual Studio verrà visualizzata un'opzione che consente di evitare eventuali avvisi SSL.
9. Fare clic su **Sì** per considerare attendibile il certificato SSL di IIS Express e continuare.   
    ![Dettagli del certificato SSL di IIS Express](checkout-and-payment-with-paypal/_static/image6.png)  
  Verrà visualizzato un avviso di sicurezza.
10. Fare clic su **Sì** per installare il certificato per l'host locale.   
    ![Finestra di dialogo Avviso di sicurezza](checkout-and-payment-with-paypal/_static/image7.png)  
  Verrà visualizzata la finestra del browser.

È ora possibile testare facilmente l'applicazione Web in locale utilizzando SSL.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Aggiungere un provider OAuth 2.0

ASP.NET Web Form offre opzioni avanzate per l'appartenenza e l'autenticazione. Questi miglioramenti includono OAuth.These enhancements include OAuth. OAuth è un protocollo aperto che consente l'autorizzazione sicura in un metodo semplice e standard da applicazioni web, mobile e desktop. Il modello Web Form ASP.NET utilizza OAuth per esporre Facebook, Twitter, Google e Microsoft come provider di autenticazione. Anche se questa esercitazione utilizza solo Google come provider di autenticazione, è possibile modificare facilmente il codice per utilizzare uno qualsiasi dei provider. I passaggi per implementare altri provider sono molto simili a quelli che si vedranno in questa esercitazione.

Oltre all'autenticazione, nell'esercitazione verranno utilizzati anche i ruoli per implementare l'autorizzazione. Solo gli utenti aggiunti al ruolo `canEdit` saranno in grado di modificare i dati, ovvero creare, modificare o eliminare i contatti.

> [!NOTE] 
> 
> Le applicazioni Windows Live accettano solo un URL attivo per un sito Web funzionante, pertanto non è possibile utilizzare un URL del sito Web locale per il test degli accessi.

La procedura seguente consente di aggiungere un provider di autenticazione Google.

1. Aprire il file *Di avvio\_dell'app, Startup.Auth.cs.*
2. Rimuovere i caratteri di commento dal metodo `app.UseGoogleAuthentication()` in modo da ottenere il codice seguente: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Passare a [Google Developers Console](https://console.developers.google.com/). Sarà necessario eseguire l'accesso con l'account di posta elettronica per sviluppatori di Google (gmail.com). Se non si dispone di un account Google, selezionare il collegamento **Crea un account** .   
   Verrà quindi visualizzata la pagina **Google Developers Console**.   
    ![Google Developers Console](checkout-and-payment-with-paypal/_static/image8.png)
4. Fare clic sul pulsante **Crea progetto** e immettere un nome e un ID del progetto (è possibile utilizzare i valori predefiniti). Quindi, fare clic sulla **casella di controllo dell'accordo** e sul pulsante **Crea.**  

    ![Google - Nuovo progetto](checkout-and-payment-with-paypal/_static/image9.png)

   In pochi secondi verrà creato il nuovo progetto e il browser visualizzerà la pagina dei nuovi progetti.
5. Nella scheda sinistra fare clic su ** &amp; Autenticazione API**, quindi su **Credenziali**.
6. Fare clic su **Crea nuovo ID client** in **OAuth**.   
   Verrà visualizzata la finestra di dialogo **Create Client ID** .   
    ![Google - creare ID Client](checkout-and-payment-with-paypal/_static/image10.png)
7. Nella finestra di dialogo **Crea ID client** mantenere **l'applicazione Web** predefinita per il tipo di applicazione.
8. Impostare **Origini JavaScript autorizzate** sull'URL SSL usato`https://localhost:44300/` in precedenza in questa esercitazione (a meno che non siano stati creati altri progetti SSL).   
   Questo URL rappresenta l'origine dell'applicazione. Per questo esempio, sarà necessario immettere solo l'URL di test localhost. Tuttavia, è possibile immettere più URL per tenere conto di localhost e produzione.
9. Per **Authorized Redirect URI** immettere le impostazioni seguenti: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Questo valore è l'URI che verrà usato da ASP.NET OAuth per comunicare con il server OAuth di Google. Ricorda l'URL SSL `https://localhost:44300/` che hai utilizzato sopra (a meno che tu non abbia creato altri progetti SSL).
10. Fare clic sul pulsante **Crea ID client.**
11. Nel menu a sinistra della Console per sviluppatori Google, fai clic sulla voce di menu **della schermata Consenso,** quindi imposta il tuo indirizzo email e il nome del prodotto. Una volta completato il modulo, fare clic su **Salva**.
12. Fai clic sulla voce di menu **API,** scorri verso il basso e fai clic sul pulsante **Off** accanto all'API **di Google.**   
    L'accettazione di questa opzione consente di abilitare l'API di Google.
13. È inoltre necessario aggiornare il pacchetto Microsoft.Owin NuGet alla versione 3.0.0.You must also update the **Microsoft.Owin** NuGet package to version 3.0.0.   
    Dal menu **Strumenti** , selezionare **NuGet Gestione pacchetti** e quindi selezionare Gestisci pacchetti **NuGet per soluzione**.  
    Nella finestra **Gestisci pacchetti NuGet** individuare e aggiornare il pacchetto **Microsoft.Owin** alla versione 3.0.0.
14. In Visual Studio `UseGoogleAuthentication` aggiornare il metodo della pagina *Startup.Auth.cs* copiando e incollando l'ID **client** e il **segreto client** nel metodo. I valori **ID client** e **Segreto client** illustrati di seguito sono esempi e non funzioneranno. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Per compilare ed eseguire l'applicazione, premere **CTRL e F5.** Fare clic sul collegamento **Accedi** .
16. In **Utilizzare un altro servizio per accedere,** fai clic su **Google**.  
    ![Accesso](checkout-and-payment-with-paypal/_static/image11.png)
17. Se hai bisogno di inserire le tue credenziali, sarai reindirizzato al sito di Google dove inserisci le tue credenziali.  
    ![Google - accesso](checkout-and-payment-with-paypal/_static/image12.png)
18. Dopo aver immesso le credenziali, verrà richiesto di concedere le autorizzazioni per l'applicazione Web appena creata.  
    ![Account del servizio di progetto predefinito](checkout-and-payment-with-paypal/_static/image13.png)
19. Fare clic su **Accetta**. Ora sarai reindirizzato alla pagina **Register** dell'applicazione **WingtipToys** dove potrai registrare il tuo account Google.  
    ![Registrare con l'Account Google](checkout-and-payment-with-paypal/_static/image14.png)
20. Sarà possibile modificare il nome di registrazione dell'indirizzo di posta elettronica locale usato per l'account Gmail, anche se in generale è preferibile mantenere l'alias di posta elettronica predefinito, ovvero quello usato per l'autenticazione. Fare clic su **Accedi** come mostrato sopra.

### <a name="modifying-login-functionality"></a>Modifica della funzionalità di accesso

Come accennato in precedenza in questa serie di esercitazioni, gran parte della funzionalità di registrazione utente è stata inclusa nel modello Web Form ASP.NET per impostazione predefinita. A questo punto si modificheranno le pagine predefinite `MigrateCart` *Login.aspx* e *Register.aspx* per chiamare il metodo . Il `MigrateCart` metodo associa un utente appena connesso a un carrello acquisti anonimo. Associando l'utente e il carrello, l'applicazione di esempio Wingtip Toys sarà in grado di mantenere il carrello dell'utente tra una visita e l'altra.

1. In **Esplora soluzioni**individuare e aprire la cartella *Account.*
2. Modificare la pagina code-behind denominata *Login.aspx.cs* per includere il codice evidenziato in giallo, in modo che venga visualizzato come segue:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Salvare il file *di Login.aspx.cs.*

Per il momento, è possibile ignorare l'avviso che non esiste alcuna definizione per il `MigrateCart` metodo. Si aggiungerà un po 'più avanti in questo tutorial.

Il *Login.aspx.cs* file code-behind supporta un metodo LogIn.The code-behind file supports a LogIn method. Esaminando la pagina Login.aspx, si noterà che questa pagina include un pulsante `LogIn` "Accedi" che quando si fa clic attiva il gestore nel code-behind.

Quando `Login` viene chiamato il metodo sul *Login.aspx.cs,* viene `usersShoppingCart` creata una nuova istanza del carrello. L'ID del carrello (un GUID) viene `cartId` recuperato e impostato sulla variabile. Quindi, `MigrateCart` viene chiamato il metodo `cartId` , passando sia il e il nome dell'utente connesso a questo metodo. Quando viene eseguita la migrazione del carrello, il GUID utilizzato per identificare il carrello anonimo viene sostituito con il nome utente.

Oltre a modificare *il* Login.aspx.cs file code-behind per eseguire la migrazione del carrello quando l'utente esegue l'accesso, è necessario modificare anche il *file code-behind Register.aspx.cs* per eseguire la migrazione del carrello quando l'utente crea un nuovo account ed esegue l'accesso.

1. Nella cartella *Account* aprire il file code-behind denominato *Register.aspx.cs*.
2. Modificare il file code-behind includendo il codice in giallo, in modo che venga visualizzato come segue:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Salvare il file *Register.aspx.cs.* Ancora una volta, ignorare `MigrateCart` l'avviso relativo al metodo.

Si noti che il `CreateUser_Click` codice utilizzato nel gestore eventi `LogIn` è molto simile al codice utilizzato nel metodo. Quando l'utente si connette o accede al `MigrateCart` sito, verrà effettuata una chiamata al metodo.

## <a name="migrating-the-shopping-cart"></a>Migrazione del carrello

Dopo aver aggiornato il processo di accesso e registrazione, è possibile aggiungere `MigrateCart` il codice per eseguire la migrazione del carrello utilizzando il metodo.

1. In **Esplora soluzioni**individuare la cartella *Logic* e aprire il file di classe *ShoppingCartActions.cs.*
2. Aggiungere il codice evidenziato in giallo al codice esistente nel file *di ShoppingCartActions.cs,* in modo che il codice nel file *di ShoppingCartActions.cs* venga visualizzato come segue:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

Il `MigrateCart` metodo utilizza il cartId esistente per trovare il carrello dell'utente. Successivamente, il codice scorre in ciclo tutti gli `CartId` elementi del carrello e sostituisce la proprietà (come specificato dallo `CartItem` schema) con il nome utente connesso.

### <a name="updating-the-database-connection"></a>Aggiornamento della connessione al database

Se si segue questa esercitazione utilizzando l'applicazione di esempio Wingtip Toys **predefinita,** è necessario ricreare il database delle appartenenze predefinito. Modificando la stringa di connessione predefinita, il database delle appartenenze verrà creato alla successiva esecuzione dell'applicazione.

1. Aprire il file *Web.config* nella radice del progetto.
2. Aggiornare la stringa di connessione predefinita in modo che venga visualizzata come segue:Update the default connection string so that it appears as follows:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>Integrazione di PayPal

PayPal è una piattaforma di fatturazione basata sul web che accetta pagamenti da parte di commercianti online. Questa esercitazione spiega quindi come integrare la funzionalità di check-in rapida di PayPal nell'applicazione. Express Checkout consente ai tuoi clienti di utilizzare PayPal per pagare gli articoli che hanno aggiunto al carrello.

### <a name="create-paypal-test-accounts"></a>Creazione di account di test PayPal

Per utilizzare l'ambiente di test PayPal, è necessario creare e verificare un account di test per sviluppatori. Utilizzerai l'account di test per sviluppatori per creare un account di test dell'acquirente e un account di test del venditore. Le credenziali dell'account di test per sviluppatori consentiranno inoltre all'applicazione di esempio Wingtip Toys di accedere all'ambiente di test PayPal.

1. In un browser, accedi al sito di test per sviluppatori PayPal:   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Se non hai un account sviluppatore PayPal, crea un nuovo account facendo clic su **Iscriviti**e seguendo la procedura di iscrizione. Se si dispone di un account sviluppatore PayPal esistente, accedere facendo clic su **Accedi**. Avrai bisogno del tuo account sviluppatore PayPal per testare l'applicazione di esempio Wingtip Toys più avanti in questa esercitazione.
3. Se ti sei appena registrato per il tuo account sviluppatore PayPal, potrebbe essere necessario verificare il tuo account sviluppatore PayPal con PayPal. Puoi verificare il tuo account seguendo la procedura che PayPal ha inviato al tuo account email. Dopo aver verificato il tuo account sviluppatore PayPal, accedi nuovamente al sito di test per sviluppatori PayPal.
4. Dopo aver effettuato l'accesso al sito per sviluppatori PayPal con il tuo account sviluppatore PayPal, devi creare un account di prova dell'acquirente PayPal se non ne hai già uno. Per creare un account di test dell'acquirente, nel sito PayPal fare clic sulla scheda **Applicazioni** e quindi su **Account sandbox**.   
 Viene visualizzata la pagina **Account di test sandbox.**   

    > [!NOTE] 
    > 
    > Il sito PayPal Developer fornisce già un account di test commerciante.

    ![Checkout e pagamento con PayPal - Conti di prova sandbox](checkout-and-payment-with-paypal/_static/image15.png)
5. Nella pagina Account di test sandbox fare clic su **Crea account.**
6. Nella pagina **Crea account** di prova scegliere l'indirizzo di posta elettronica e la password dell'account di test dell'acquirente a scelta.   

    > [!NOTE] 
    > 
    > Avrete bisogno degli indirizzi e-mail dell'acquirente e la password per testare l'applicazione di esempio Wingtip Toys alla fine di questa esercitazione.

    ![Checkout e pagamento con PayPal - Conti di prova sandbox](checkout-and-payment-with-paypal/_static/image16.png)
7. Crea l'account di test dell'acquirente facendo clic sul pulsante **Crea account.**  
 Viene visualizzata la pagina **Account di test sandbox.** 

    ![Checkout e pagamento con PayPal - Conti PayPal](checkout-and-payment-with-paypal/_static/image17.png)
8. Nella pagina **Account di test sandbox** fare clic sull'account di posta elettronica del **facilitatore.**  
    **Vengono** visualizzate le opzioni Profilo e **Notifica.**
9. Seleziona l'opzione **Profilo,** quindi fai clic su **Credenziali API** per visualizzare le credenziali API per l'account di test del commerciante.
10. Copiare le credenziali dell'API TEST nel blocco note.

Per effettuare chiamate API dall'applicazione di esempio Wingtip Toys all'ambiente di testing PayPal sono necessarie le credenziali DELL'API TEST classica visualizzate (nome utente, password e firma). Le credenziali verranno aggiunte nel passaggio successivo.

### <a name="add-paypal-class-and-api-credentials"></a>Aggiungere le credenziali API e classe PayPalAdd PayPal Class and API Credentials

Inserirai la maggior parte del codice PayPal in un'unica classe. Questa classe contiene i metodi utilizzati per comunicare con PayPal. Inoltre, aggiungerai le tue credenziali PayPal a questa classe.

1. Nell'applicazione di esempio Wingtip Toys in Visual Studio fare clic con il pulsante destro del mouse sulla cartella **Logic** , quindi **scegliere Aggiungi**  - &gt; **nuovo elemento**.   
   La finestra di dialogo **Aggiungi nuovo elemento** viene visualizzata.
2. Nel riquadro **Installato a** sinistra del riquadro Installato di Visual **C,** selezionare **Codice**.
3. Nel riquadro centrale selezionare **Classe**. Denominare questa nuova classe **PayPalFunctions.cs**.
4. Fare clic su **Aggiungi**.  
   Il nuovo file di classe viene visualizzato nell'editor.
5. Sostituire il codice predefinito con il codice seguente:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Aggiungere le credenziali dell'API Commerciante (nome utente, password e firma) visualizzate in precedenza in questa esercitazione in modo da poter effettuare chiamate di funzione all'ambiente di test PayPal.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> In questa applicazione di esempio si aggiungono semplicemente le credenziali a un file di C . Tuttavia, in una soluzione implementata, è consigliabile crittografare le credenziali in un file di configurazione.

La classe NVPAPICaller contiene la maggior parte delle funzionalità Di PayPal. Il codice nella classe fornisce i metodi necessari per effettuare un acquisto di prova dall'ambiente di test PayPal. Per effettuare acquisti vengono utilizzate le seguenti tre funzioni PayPal:

- Funzione `SetExpressCheckout`
- Funzione `GetExpressCheckoutDetails`
- Funzione `DoExpressCheckoutPayment`

Il `ShortcutExpressCheckout` metodo raccoglie le informazioni di acquisto di prova `SetExpressCheckout` e i dettagli del prodotto dal carrello e chiama la funzione PayPal. Il `GetCheckoutDetails` metodo conferma i dettagli `GetExpressCheckoutDetails` dell'acquisto e chiama la funzione PayPal prima di effettuare l'acquisto del test. Il `DoCheckoutPayment` metodo completa l'acquisto di test `DoExpressCheckoutPayment` dall'ambiente di test chiamando la funzione PayPal. Il codice rimanente supporta i metodi e il processo PayPal, ad esempio la codifica di stringhe, la decodifica di stringhe, l'elaborazione di matrici e la determinazione delle credenziali.

> [!NOTE] 
> 
> PayPal consente di includere dettagli di acquisto opzionali in base alle [specifiche API di PayPal.](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout) Estendendo il codice nell'applicazione di esempio Wingtip Toys, è possibile includere dettagli di localizzazione, descrizioni dei prodotti, imposte, un numero di servizio clienti e molti altri campi facoltativi.

Si noti che gli URL return e cancel specificati nel metodo **ShortcutExpressCheckout** utilizzano un numero di porta.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Quando Visual Web Developer esegue un progetto Web utilizzando SSL, in genere la porta 44300 viene utilizzata per il server Web. Come illustrato in precedenza, il numero di porta è 44300. Quando si esegue l'applicazione, è possibile visualizzare un numero di porta diverso. Il numero di porta deve essere impostato correttamente nel codice in modo da poter eseguire correttamente l'applicazione di esempio Wingtip Toys alla fine di questa esercitazione. Nella sezione successiva di questa esercitazione viene illustrato come recuperare il numero di porta host locale e aggiornare la classe PayPal.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Aggiornare il numero di porta LocalHost nella classe PayPal

L'applicazione di esempio Wingtip Toys acquista prodotti accedendo al sito di test PayPal e tornando all'istanza locale dell'applicazione di esempio Wingtip Toys. Per fare in modo che PayPal torni all'URL corretto, è necessario specificare il numero di porta dell'applicazione di esempio in esecuzione localmente nel codice PayPal indicato in precedenza.

1. Fare clic con il pulsante destro del mouse sul nome del progetto (**WingtipToys**) in **Esplora soluzioni** e scegliere **Proprietà**.
2. Nella colonna sinistra, selezionare la scheda **Web.**
3. Recuperare il numero di porta dalla casella **URL progetto.**
4. Se necessario, `returnURL` aggiornare e `cancelURL` nella`NVPAPICaller`classe PayPal ( ) nel file *PayPalFunctions.cs* per utilizzare il numero di porta dell'applicazione Web:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

A questo punto il codice aggiunto corrisponderà alla porta prevista per l'applicazione Web locale. PayPal sarà in grado di tornare all'URL corretto sul computer locale.

### <a name="add-the-paypal-checkout-button"></a>Aggiungere il pulsante PayPal Checkout

Ora che le funzioni PayPal principali sono state aggiunte all'applicazione di esempio, è possibile iniziare ad aggiungere il markup e il codice necessari per chiamare queste funzioni. In primo luogo, è necessario aggiungere il pulsante di checkout che l'utente vedrà sulla pagina del carrello.

1. Aprire il file *ShoppingCart.aspx.*
2. Scorrere fino alla fine del `<!--Checkout Placeholder -->` file e trovare il commento.
3. Sostituire il commento `ImageButton` con un controllo in modo che il contrassegno verso l'alto venga sostituito come segue:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. Nel file *ShoppingCart.aspx.cs,* `UpdateBtn_Click` dopo il gestore eventi vicino `CheckOutBtn_Click` alla fine del file, aggiungere il gestore eventi:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Sempre nel file *ShoppingCart.aspx.cs* aggiungere un `CheckoutBtn`riferimento a , in modo che il pulsante della nuova immagine sia referenziato come segue:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Salvare le modifiche apportate sia al file *ShoppingCart.aspx* che al file *ShoppingCart.aspx.cs.*
7. Dal menu , selezionare **Debug**-&gt;**Build WingtipToys**.  
   Il progetto verrà ricompilato con il controllo **ImageButton** appena aggiunto.

### <a name="send-purchase-details-to-paypal"></a>Invia i dettagli dell'acquisto a PayPal

Quando l'utente fa clic sul pulsante **Checkout** nella pagina del carrello della spesa (*ShoppingCart.aspx*), inizierà il processo di acquisto. Il codice seguente chiama la prima funzione PayPal necessaria per acquistare i prodotti.

1. Dalla cartella *Estrai* aprire il file code-behind denominato *CheckoutStart.aspx.cs*.   
   Assicurarsi di aprire il file code-behind.
2. Sostituire il codice esistente con quello seguente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Quando l'utente dell'applicazione fa clic sul pulsante **Checkout** nella pagina del carrello, il browser passerà alla pagina *CheckoutStart.aspx.* Quando viene caricata la pagina `ShortcutExpressCheckout` *CheckoutStart.aspx,* viene chiamato il metodo . A questo punto, l'utente viene trasferito al sito Web di test PayPal. Sul sito PayPal, l'utente immette le proprie credenziali PayPal, esamina i dettagli di acquisto, accetta l'accordo PayPal e torna all'applicazione di esempio Wingtip Toys in cui il `ShortcutExpressCheckout` metodo viene completato. Al `ShortcutExpressCheckout` termine del metodo, l'utente verrà reindirizzato alla pagina `ShortcutExpressCheckout` *CheckoutReview.aspx* specificata nel metodo. Ciò consente all'utente di esaminare i dettagli dell'ordine dall'applicazione di esempio Wingtip Toys.

### <a name="review-order-details"></a>Revisione dei dettagli dell'ordine

Dopo il ritorno da PayPal, la pagina *CheckoutReview.aspx* dell'applicazione di esempio Wingtip Toys visualizza i dettagli dell'ordine. Questa pagina consente all'utente di esaminare i dettagli dell'ordine prima di acquistare i prodotti. La pagina *CheckoutReview.aspx* deve essere creata come segue:

1. Nella cartella *Checkout* aprire la pagina denominata *CheckoutReview.aspx*.
2. Sostituire il markup esistente con quanto segue:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Aprire la pagina code-behind denominata *CheckoutReview.aspx.cs* e sostituire il codice esistente con quanto segue:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

Il controllo **DetailsView** viene utilizzato per visualizzare i dettagli dell'ordine restituiti da PayPal. Inoltre, il codice precedente salva i dettagli dell'ordine `OrderDetail` nel database Wingtip Toys come oggetto. Quando l'utente fa clic sul pulsante **Ordine di completamento,** vengono reindirizzati alla pagina *CheckoutComplete.aspx.*

> [!NOTE] 
> 
> **mancia**
> 
> Nel markup della pagina *CheckoutReview.aspx,* `<ItemStyle>` si noti che il tag viene utilizzato per modificare lo stile degli elementi all'interno del **controllo DetailsView** nella parte inferiore della pagina. Visualizzando la pagina in **visualizzazione Progettazione** (selezionando **Progettazione** nell'angolo inferiore sinistro di Visual Studio), quindi selezionando il controllo **DetailsView** e selezionando lo **smart tag** (l'icona a freccia nella parte superiore destra del controllo), sarà possibile visualizzare le **attività detailsView**.
> 
> ![Checkout e pagamento con PayPal - Modifica campi](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Selezionando **Modifica campi**, verrà visualizzata la finestra di dialogo **Campi.** In questa finestra di dialogo è possibile controllare facilmente le proprietà visive, ad esempio **ItemStyle**, del **controllo DetailsView.**
> 
> ![Checkout e pagamento con PayPal - Finestra di dialogo Campi](checkout-and-payment-with-paypal/_static/image19.png)

### <a name="complete-purchase"></a>Acquisto completo

*CheckoutComplete.aspx* pagina effettua l'acquisto da PayPal. Come accennato in precedenza, l'utente deve fare clic sul pulsante **Ordine di completamento** prima che l'applicazione passerà alla pagina *CheckoutComplete.aspx.*

1. Nella cartella *Checkout* aprire la pagina *denominata CheckoutComplete.aspx*.
2. Sostituire il markup esistente con quanto segue:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Aprire la pagina code-behind denominata *CheckoutComplete.aspx.cs* e sostituire il codice esistente con quanto segue:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Quando viene caricata la pagina `DoCheckoutPayment` *CheckoutComplete.aspx,* viene chiamato il metodo . Come accennato `DoCheckoutPayment` in precedenza, il metodo completa l'acquisto dall'ambiente di test PayPal. Una volta che PayPal ha completato l'acquisto dell'ordine, `ID` la pagina *CheckoutComplete.aspx* visualizza una transazione di pagamento per l'acquirente.

### <a name="handle-cancel-purchase"></a>Gestire Annulla acquisto

Se l'utente decide di annullare l'acquisto, verrà indirizzato alla pagina *CheckoutCancel.aspx* in cui verrà visualizzato che l'ordine è stato annullato.

1. Aprire la pagina denominata *CheckoutCancel.aspx* nella cartella *Checkout.*
2. Sostituire il markup esistente con quanto segue:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Gestire gli errori di acquisto

Gli errori durante il processo di acquisto verranno gestiti dalla pagina *CheckoutError.aspx.* Il code-behind della pagina *CheckoutStart.aspx,* la pagina *CheckoutReview.aspx* e la pagina *CheckoutComplete.aspx* verranno reindirizzati alla pagina *CheckoutError.aspx* se si verifica un errore.

1. Aprire la pagina denominata *CheckoutError.aspx* nella cartella *Checkout.*
2. Sostituire il markup esistente con quanto segue:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

La pagina *CheckoutError.aspx* viene visualizzata con i dettagli dell'errore quando si verifica un errore durante il processo di estrazione.

## <a name="running-the-application"></a>Esecuzione dell'applicazione

Eseguire l'applicazione per vedere come acquistare i prodotti. Si noti che verrà eseguito nell'ambiente di test PayPal. Non si scambiano denaro reale.

1. Assicurarsi che tutti i file vengono salvati in Visual Studio.
2. Aprire un browser Web [https://developer.paypal.com](https://developer.paypal.com/)e passare a .
3. Accedi con il tuo account sviluppatore PayPal che hai creato in precedenza in questa esercitazione.  
   Per la sandbox per sviluppatori di PayPal, [https://developer.paypal.com](https://developer.paypal.com/) è necessario effettuare l'accesso per testare il checkout espresso. Questo vale solo per i test sandbox di PayPal, non per l'ambiente live di PayPal.
4. In Visual Studio premere F5 per eseguire l'applicazione di esempio Wingtip Toys.In Visual Studio, press **F5** to run the Wingtip Toys sample application.  
   Dopo la ricompilazione del database, il browser verrà aperto e verrà visualizzata la pagina *Default.aspx.*
5. Aggiungere tre prodotti diversi al carrello selezionando la categoria di prodotto, ad esempio "Auto" e quindi facendo clic su **Aggiungi al carrello** accanto a ogni prodotto.  
   Il carrello visualizzerà il prodotto selezionato.
6. Fai clic sul pulsante **PayPal** per effettuare il checkout. 

    ![Checkout e pagamento con PayPal - Carrello](checkout-and-payment-with-paypal/_static/image20.png)

   Per l'estrazione è necessario disporre di un account utente per l'applicazione di esempio Wingtip Toys.
7. Fai clic sul link **Google** a destra della pagina per accedere con un account email gmail.com esistente.  
   Se non si dispone di un account gmail.com, è possibile crearne uno a scopo di test presso [www.gmail.com](https://www.gmail.com/). È inoltre possibile utilizzare un account locale standard facendo clic su "Registra". 

    ![Checkout e pagamento con PayPal - Log in](checkout-and-payment-with-paypal/_static/image21.png)
8. Accedi con il tuo account gmail e la password. 

    ![Checkout e pagamento con PayPal - Accesso Gmail](checkout-and-payment-with-paypal/_static/image22.png)
9. Fare clic sul pulsante **Accedi** per registrare l'account gmail con il nome utente dell'applicazione di esempio Wingtip Toys. 

    ![Checkout e pagamento con PayPal - Registro conto](checkout-and-payment-with-paypal/_static/image23.png)
10. Sul sito di prova PayPal, aggiungi l'indirizzo email e la password **dell'acquirente** creati in precedenza in questa esercitazione, quindi fai clic sul pulsante **Accedi.** 

    ![Checkout e pagamento con PayPal - Sign-In PayPal](checkout-and-payment-with-paypal/_static/image24.png)
11. Accetta le norme payPal e fai clic sul pulsante **Accetto e continua.**  
    Tieni presente che questa pagina viene visualizzata solo la prima volta che utilizzi questo conto PayPal. Si noti ancora una volta che questo è un conto di prova, non viene scambiato denaro reale. 

    ![Checkout e pagamento con PayPal - Politica PayPal](checkout-and-payment-with-paypal/_static/image25.png)
12. Esaminare le informazioni sull'ordine nella pagina di revisione dell'ambiente di test PayPal e fare clic su **Continua**. 

    ![Checkout e pagamento con PayPal - Informazioni sulla revisione](checkout-and-payment-with-paypal/_static/image26.png)
13. Nella pagina *CheckoutReview.aspx* verificare l'importo dell'ordine e visualizzare l'indirizzo di spedizione generato. Quindi, fare clic sul pulsante **Completa ordine.** 

    ![Checkout e pagamento con PayPal - Recensione dell'ordine](checkout-and-payment-with-paypal/_static/image27.png)
14. Viene visualizzata la pagina **CheckoutComplete.aspx** con un ID transazione di pagamento. 

    ![Checkout e pagamento con PayPal - Checkout completato](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Revisione del database

Esaminando i dati aggiornati nel database dell'applicazione di esempio Wingtip Toys dopo l'esecuzione dell'applicazione, è possibile notare che l'applicazione ha registrato correttamente l'acquisto dei prodotti.

È possibile esaminare i dati contenuti nel file di database *Wingtiptoys.mdf* utilizzando la finestra **Esplora database** (finestra**Esplora server** in Visual Studio) come è stato fatto in precedenza in questa serie di esercitazioni.

1. Chiudere la finestra del browser se è ancora aperta.
2. In Visual Studio selezionare l'icona **Mostra tutti i file** nella parte superiore di Esplora **soluzioni** per espandere la cartella **Dati\_app.**
3. Espandere la cartella **\_App Data.**  
 Potrebbe essere necessario selezionare l'icona **Mostra tutti i file** per la cartella.
4. Fare clic con il pulsante destro del mouse sul file di database *Wingtiptoys.mdf* e scegliere **Apri**.  
    Verrà visualizzato **Esplora server.**
5. Espandere la cartella **Tabelle** .
6. Fare clic con il pulsante destro del mouse sulla tabella **Orders**e scegliere **Mostra dati tabella**.  
 Viene visualizzata la tabella **Ordini.**
7. Esaminare la colonna **PaymentTransactionID** per confermare le transazioni riuscite. 

    ![Checkout e pagamento con PayPal - Banca dati delle recensioni](checkout-and-payment-with-paypal/_static/image29.png)
8. Chiudere la finestra della tabella **Ordini.**
9. In Esplora server fare clic con il pulsante destro del mouse sulla tabella **OrderDetails** e scegliere **Mostra dati tabella**.
10. Esaminare `OrderId` `Username` i valori e nella tabella **OrderDetails.** Si noti che `OrderId` `Username` questi valori corrispondono ai valori e inclusi nella tabella **Orders.**
11. Chiudere la finestra della tabella **OrderDetails.**
12. Fare clic con il pulsante destro del mouse sul file di database Wingtip Toys (*Wingtiptoys.mdf*) e scegliere **Chiudi connessione**.
13. Se la finestra **Esplora soluzioni** non viene visualizzata, fare clic su **Esplora soluzioni** nella parte inferiore della finestra **Esplora server** per visualizzare nuovamente **Esplora soluzioni.**

## <a name="summary"></a>Riepilogo

In questa esercitazione sono stati aggiunti schemi di dettaglio dell'ordine e dell'ordine per tenere traccia dell'acquisto di prodotti. È stata inoltre integrata la funzionalità PayPal nell'applicazione di esempio Wingtip Toys.

## <a name="additional-resources"></a>Risorse aggiuntive

[Cenni preliminari sulla configurazione di ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Distribuire un'app Web Form di ASP.NET sicura con appartenenza, OAuth e database SQL al servizio app di AzureDeploy a Secure ASP.NET Web Forms App with Membership, OAuth, and SQL Database to Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - Versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Dichiarazione di non responsabilità

Questa esercitazione contiene codice di esempio. Tale codice di esempio viene fornito "così com'è" senza garanzia di alcun tipo. Di conseguenza, Microsoft non garantisce l'accuratezza, l'integrità o la qualità del codice di esempio. L'utente accetta di utilizzare il codice di esempio a proprio rischio e pericolo. In nessun caso Microsoft sarà responsabile per l'utente in alcun modo per qualsiasi codice di esempio, contenuto, inclusi, ma non limitati a, eventuali errori o omissioni in qualsiasi codice di esempio, contenuto o qualsiasi perdita o danno di qualsiasi tipo sostenuto a seguito dell'uso di qualsiasi codice di esempio. L'utente viene informato e accetta di indennizzare, salvare e mantenere Microsoft innocuo da e contro qualsiasi perdita, reclamo di perdita, infortunio o danno di qualsiasi tipo, comprese, senza limitazioni, quelle causate da o derivanti da materiale che pubblichi, trasmetti, usi o fai affidamento su includendo, ma non limitato, le opinioni espresse in esso contenute.

> [!div class="step-by-step"]
> [Successivo](shopping-cart.md)
> [precedente](membership-and-administration.md)
