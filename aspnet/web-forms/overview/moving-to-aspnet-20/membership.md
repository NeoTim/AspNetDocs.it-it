---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: Proprietà Membership . Documenti Microsoft
author: rick-anderson
description: ASP.NET'appartenenza si basa sul successo del modello di autenticazione basata su form da ASP.NET 1.x. ASP.NET l'autenticazione basata su form fornisce un modo pratico per incorp...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: ed48c11cbd483de088239bad7c2452b7fc60a1cf
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543769"
---
# <a name="membership"></a>Appartenenza

da parte [di Microsoft](https://github.com/microsoft)

> ASP.NET'appartenenza si basa sul successo del modello di autenticazione basata su form da ASP.NET 1.x. ASP.NET l'autenticazione basata su form ASP.NET fornisce un modo pratico per incorporare un form di accesso nell'applicazione ASP.NET e convalidare gli utenti in base a un database o a un altro archivio dati.

ASP.NET'appartenenza si basa sul successo del modello di autenticazione basata su form da ASP.NET 1.x. ASP.NET l'autenticazione basata su form ASP.NET fornisce un modo pratico per incorporare un form di accesso nell'applicazione ASP.NET e convalidare gli utenti in base a un database o a un altro archivio dati. I membri della classe FormsAuthentication consentono di gestire i cookie per l'autenticazione, verificare la presenza di un account di accesso valido, disconnettere un utente e così via. Tuttavia, l'implementazione dell'autenticazione basata su form in un'applicazione ASP.NET 1.x può richiedere una discreta quantità di codice.

L'appartenenza a ASP.NET 2.0 è un importante passo oltre l'utilizzo dell'autenticazione basata su form. L'appartenenza è più affidabile se associata all'autenticazione basata su form, ma l'utilizzo dell'autenticazione basata su form non è un requisito. Come si vedrà presto, è possibile utilizzare ASP.NET appartenenza e i controlli di accesso in ASP.NET 2.0 per implementare un potente sistema di appartenenze senza scrivere molto codice.

## <a name="implementing-membership-in-aspnet-20"></a>Implementazione dell'appartenenza a ASP.NET 2.0Implementing Membership in ASP.NET 2.0

L'appartenenza viene implementata seguendo quattro passaggi. Tenere presente che ci sono molti passaggi secondari che sono coinvolti così come la configurazione facoltativa che può essere implementato pure. Questi passaggi hanno lo scopo di illustrare il quadro generale della configurazione dell'appartenenza.

1. Creare il database delle appartenenze (se SQL Server viene utilizzato come archivio di appartenenze).
2. Specificare le opzioni di appartenenza nei file di configurazione delle applicazioni. L'appartenenza è abilitata per impostazione predefinita.
3. Determinare il tipo di archivio di appartenenze che si desidera utilizzare. Sono disponibili le opzioni seguenti: 

    - Microsoft SQL Server (versione 7.0 o successiva)
    - Archivio di Active Directory
    - Provider di appartenenze personalizzato
4. Configurare l'applicazione per l'autenticazione basata su form ASP.NET. Ancora una volta, l'appartenenza è progettata per sfruttare l'autenticazione basata su form, ma l'utilizzo dell'autenticazione basata su form non è un requisito.
5. Definire gli account utente per l'appartenenza e configurare i ruoli, se lo si desidera.

## <a name="creating-the-membership-database"></a>Creazione del database delle appartenenzeCreating the Membership Database

Se si utilizza SQL Server 7.0 o versione successiva come archivio di appartenenze, è possibile utilizzare l'utilità aspnet\_regsql (disponibile più facilmente dal prompt dei comandi di Visual Studio .NET 2005) per configurare il database. L'utilità\_aspnet regsql può essere utilizzata come strumento del prompt dei comandi o tramite una procedura guidata GUI. Il metodo della procedura guidata è il modo più semplice per configurare il database. Per accedere alla procedura guidata, è sufficiente eseguire il comando seguente:

`aspnet_regsql W`

Dopo aver eseguito il comando, verrà visualizzata la ASP.NET'Installazione guidata di SQL Server COME illustrato di seguito.

![](membership/_static/image1.jpg)

**Figura 1**

L'Installazione guidata di sql ASP.NET SQL ServerSQL Server crea il sito Web nell'istanza specificata nella procedura guidata. Tuttavia, ASP.NET utilizzerà la stringa di connessione nel file machine.config per connettersi al database. Per impostazione predefinita, questa stringa di connessione punterà a un'istanza di SQL Server 2005, pertanto se si utilizza un'istanza di SQL Server 2000 o SQL Server 7.0, sarà necessario modificare la stringa di connessione nel file machine.config. La stringa di connessione può trovarsi qui:That connection string can be located here:

[!code-xml[Main](membership/samples/sample1.xml)]

Sfortunatamente, se non si modifica la stringa di connessione, ASP.NET non verrà visualizzato un errore descrittivo. Continuerà a lamentarsi dicendo che non hai creato il database. Nel caso precedente, ho modificato la stringa di connessione in modo che punti all'istanza locale di SQL Server 2000.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Specifica della configurazione e aggiunta di utenti e ruoliSpecifying Configuration and Adding Users and Roles

Il passaggio successivo nella configurazione dell'appartenenza consiste nell'aggiungere le informazioni necessarie al file web.config dell'applicazione. In ASP.NET 1.x, la modifica del file web.config era talvolta difficile a causa dell'uso di lowerCamelCase e della mancanza di Intellisense. Visual Studio .NET 2005 rende l'attività molto più semplice con Intellisense per i file di configurazione, ma ASP.NET 2.0 fa un ulteriore passo avanti fornendo un'interfaccia Web per la modifica dei file di configurazione.

È possibile avviare l'interfaccia Web facendo clic sul pulsante Configurazione ASP.NET sulla barra degli strumenti di Esplora soluzioni, come illustrato di seguito. È inoltre possibile avviare l'interfaccia Web tramite popup che vengono visualizzati quando vengono inseriti i controlli di accesso.

![](membership/_static/image2.jpg)

**Figura 2**

Verrà avviato lo strumento di amministrazione del sito Web ASP.NET illustrato di seguito. L'ASP.NET Amministrazione sito Web è un'interfaccia a quattro schede che semplifica la gestione delle impostazioni dell'applicazione. Sono disponibili le seguenti schede:

- **Home**
- **Sicurezza** Configurare utenti, ruoli e accesso.
- **Applicazione** Configurare le impostazioni dell'applicazione.
- **Fornitore** Configurare e testare il provider di appartenenze alle applicazioni.

Lo strumento Amministrazione sito Web consente di creare facilmente nuovi utenti, creare nuovi ruoli e gestire utenti e ruoli. Questa funzionalità non è disponibile nell'interfaccia di Windows. L'interfaccia di Windows consente di definire facilmente le impostazioni di autorizzazione e di aggiungere, eliminare e gestire provider, funzionalità non presenti nello strumento Amministrazione sito Web.

Per avviare l'interfaccia di Windows, aprire lo snap-in Internet Information Services, fare clic con il pulsante destro del mouse sull'applicazione e scegliere Proprietà. Fare clic sulla scheda ASP.NET e quindi sul pulsante Modifica configurazione. Affinché il pulsante Modifica configurazione sia abilitato, l'applicazione deve essere in esecuzione in ASP.NET 2.0. È possibile configurare la versione ASP.NET anche nella finestra di dialogo ASP.NET.) La finestra di dialogo Impostazioni di configurazione ASP.NET viene visualizzata come illustrato di seguito.

![](membership/_static/image3.jpg)

**Figura 3**

Nella scheda Generale sono elencate le stringhe di connessione e le impostazioni dell'applicazione. Tutte le impostazioni in corsivo vengono definite in un file di configurazione padre (il file machine.config o un file web.config a un livello superiore) e le impostazioni non in corsivo provengono dal file di configurazione delle applicazioni. Se un'impostazione viene aggiunta, rimossa o modificata a livello di applicazione, ASP.NET aggiungerà, rimuoverà o modificherà l'impostazione a livello di applicazione web.config anziché rimuoverla dal file di configurazione da cui viene ereditata.

La scheda Autenticazione è illustrata di seguito. Qui è possibile configurare le impostazioni di appartenenza. Le impostazioni di autenticazione basata su form, i provider di appartenenze e i provider di ruoli possono essere configurati qui.

![](membership/_static/image4.jpg)

**Figura 4**

## <a name="implementing-membership-in-your-application"></a>Implementazione dell'appartenenza all'applicazioneImplementing Membership in Your Application

Il modo più semplice per implementare ASP.NET'appartenenza 2.0 nell'applicazione consiste nell'utilizzare i controlli di accesso forniti. Questo metodo consente di implementare le nozioni di base di ASP.NET 2.0 l'appartenenza senza scrivere alcun codice.

I seguenti controlli di accesso sono disponibili in ASP.NET 2.0:

## <a name="login-control"></a>Controllo di accesso

Il controllo Login fornisce un'interfaccia per l'accesso al sistema di appartenenza. Fornisce un nome utente e password casella di testo e un pulsante di login. Molte altre caratteristiche comuni come un link per registrarsi per le persone che non lo hanno ancora fatto, una casella di controllo che consente all'utente di accedere automaticamente alle visite successive, un link per un promemoria password, ecc. Tutte le funzionalità del controllo Login sono personalizzabili tramite le proprietà del controllo.

In ASP.NET 1.x, gli sviluppatori dovevano scrivere una discreta quantità di codice per eseguire una ricerca quando si utilizza l'autenticazione basata su form. Con ASP.NET 2.0, è possibile convalidare gli utenti senza scrivere alcun codice. ASP.NET farà automaticamente la ricerca dell'utente per te. Se si utilizza il controllo Login senza utilizzare ASP.NETappartenenza, è possibile utilizzare il metodo **OnAuthenticate** per convalidare l'utente.

## <a name="loginview-control"></a>Controllo LoginView

Il controllo LoginView è un controllo basato su modelli che fornisce due modelli per impostazione predefinita; l'oggetto AnonymousTemplate e il LoggedInTemplate. Il modello visualizzato dipende dal fatto che l'utente sia connesso o meno al sistema di appartenenze. Questo controllo viene in genere utilizzato per visualizzare un controllo Login quando un utente non ha ancora effettuato l'accesso e un controllo LoginStatus e/o altri controlli di accesso quando l'utente ha eseguito l'accesso. Se si utilizza la gestione dei ruoli nell'applicazione ASP.NET, il controllo LoginView può visualizzare un modello specifico in base al ruolo degli utenti. Ulteriori informazioni sulla gestione dei ruoli ASP.NET verranno trattate in un secondo momento.

## <a name="passwordrecovery-control"></a>Controllo PasswordRecovery

Il controllo PasswordRecovery consente agli utenti di ricevere un messaggio di posta elettronica con la password corrente o reimpostare la password. Testo non crittografato e password crittografate possono essere recuperati e inviate via e-mail agli utenti. Se viene eseguito l'hashing della password, non è possibile recuperarla. Al contrario, all'utente verrà richiesto di eseguire una reimpostazione della password.

## <a name="loginstatus-control"></a>Controllo LoginStatus

Il controllo LoginStatus viene utilizzato per visualizzare un indicatore di accesso agli utenti che non hanno effettuato l'accesso e un indicatore di disconnessione per gli utenti attualmente connessi. Il Request.IsAuthenticated proprietà viene utilizzata per determinare quale indicatore da visualizzare. L'indicatore visualizzato dal controllo LoginStatus può essere testo (implementato tramite le proprietà **LoginText** e **LogoutText)** o immagini (implementate tramite le proprietà **LoginImageUrl** e **LogoutImageUrl).**

Quando un utente si disconnette tramite il controllo LoginStatus, viene reindirizzato all'URL specificato dalla proprietà **LogoutPageUrl.** Se tale proprietà non è impostata, la pagina corrente viene aggiornata. Poiché il sito è probabilmente protetto dall'autenticazione basata su form, l'aggiornamento della pagina corrente reindirizzerà l'utente alla pagina di accesso del sito.

## <a name="loginname-control"></a>Controllo LoginName

Il controllo LoginName visualizza il nome utente dell'utente attualmente connesso al sito.

## <a name="createuserwizard-control"></a>CreateUserWizard Control

Il controllo CreateUserWizard offre agli utenti un modo pratico per registrarsi al sistema di appartenenze. È possibile aggiungere passaggi (implementati come una raccolta di WizardSteps) tramite l'interfaccia illustrata di seguito.

![](membership/_static/image5.jpg)

**Figura 5**

Il CreateUserWizard è un controllo basato su modelli che deriva dalla Wizard classe e fornisce i modelli seguenti:The CreateUserWizard is a templated control that derives from the Wizard class and provides the following templates:

- **HeaderTemplate** Questo modello controlla l'aspetto dell'intestazione della procedura guidata.
- **Modello Sidebar** Questo modello controlla l'aspetto della barra laterale della procedura guidata.
- **StartNavigationTemplate (modello StartNavigation)** Questo modello controlla l'aspetto della struttura di spostamento della procedura guidata nel passaggio iniziale.
- **Modello StepNavigation** Questo modello controlla l'aspetto dell'area di navigazione quando non si è nel passaggio iniziale o finale.
- **FinishNavigationTemplate (modello FinishNavigation)** Questo modello controlla l'aspetto dell'area di navigazione al passaggio finale.

Inoltre, per ogni passaggio aggiunto alla procedura guidata, ASP.NET verrà creato un modello personalizzato che contiene sia un ContentTemplate e un CustomNavigationTemplate per tale passaggio. Per informazioni dettagliate sulla personalizzazione di CreateUserWizard, vedere la documentazione di VS.NET 2005:

## <a name="changepassword-control"></a>Controllo ChangePassword

Il controllo ChangePassword consente agli utenti di modificare la propria password. Se la proprietà DisplayUserName è true (è false per impostazione predefinita), l'utente può modificare la propria password quando non è connesso. Se l'utente *è* già connesso e la proprietà DisplayUserName è true, l'utente sarà in grado di modificare la password di un altro utente che non è connesso, poiché conosce l'ID utente di tale utente.

Tenere presente che se si desidera che gli utenti siano in grado di modificare le password senza dover effettuare l'accesso, è necessario assicurarsi che la pagina in cui viene visualizzato il controllo ChangePassword consenta l'accesso anonimo. Ovviamente, gli utenti dovranno fornire la loro vecchia password al fine di cambiare la loro password.

## <a name="role-management"></a>Gestione dei ruoli

La gestione dei ruoli consente di assegnare gli utenti a un ruolo specifico e quindi di limitare l'accesso a determinati file o cartelle in base a tale ruolo. La gestione dei ruoli fornisce inoltre un'API in modo che sia possibile determinare a livello di codice il ruolo someones o determinare tutti gli utenti in un determinato ruolo e rispondere di conseguenza.

La gestione dei ruoli non è un requisito nellASP.NETappartenenza, né l'appartenenza è un requisito per utilizzare la gestione dei ruoli. Tuttavia, i due si completano a vicenda bene ed è probabile che gli sviluppatori li useranno in combinazione con l'altro.

Per abilitare la gestione dei ruoli nell'applicazione, apportare la seguente modifica nel file web.config:

[!code-xml[Main](membership/samples/sample2.xml)]

Quando l'attributo **cacheRolesInCookie** è impostato su true, ASP.NET memorizza nella cache un'appartenenza al ruolo users in un cookie sul client. In questo modo le ricerche di ruolo si verificano senza chiamate nel RoleProvider.This allows role lookups to occur without calls into the RoleProvider. Quando si utilizza questo attributo, gli sviluppatori sono invitati a garantire che l'attributo **cookieProtection** sia impostato su All. Questa è l'impostazione predefinita. Ciò garantisce che i dati dei cookie siano crittografati e contribuisce a garantire che i contenuti dei cookie non siano stati alterati. I ruoli possono essere aggiunti utilizzando lo strumento Amministrazione sito Web. Consente di definire facilmente i ruoli, configurare l'accesso a parti del sito in base a tali ruoli e assegnare gli utenti ai ruoli.

![](membership/_static/image6.jpg)

**Figura 6**

Come illustrato in precedenza, è possibile aggiungere nuovi ruoli semplicemente immettendo il nome del ruolo e quindi facendo clic su Aggiungi ruolo. I ruoli esistenti possono essere gestiti o eliminati facendo clic sul collegamento appropriato nell'elenco dei ruoli esistenti.

Quando si gestisce un ruolo, è possibile aggiungere o rimuovere utenti come illustrato di seguito.

![](membership/_static/image7.jpg)

**Figura 7**

Selezionando la casella di controllo L'utente è nel ruolo, è possibile aggiungere facilmente un utente a un ruolo specifico. ASP.NET aggiornerà automaticamente il database delle appartenenze con le voci appropriate. È inoltre necessario configurare le regole di accesso per l'applicazione. ASP.NET 1.x gli sviluppatori hanno &lt;familiarità con questa operazione tramite l'elemento authorization&gt; nel file web.config e tale opzione è ancora disponibile in ASP.NET 2.0. Tuttavia, è più semplice gestire le regole di accesso utilizzando lo strumento Amministrazione sito Web, come illustrato di seguito.

![](membership/_static/image8.jpg)

**Figura 8**

In questo caso, la cartella Amministrazione è evidenziata (difficile da vedere perché lo strumento lo evidenzia in grigio chiaro) e al ruolo Administrators è stato concesso l'accesso. Tutti gli altri utenti vengono negati. È possibile fare clic sull'icona della testa per selezionare una regola e quindi utilizzare i pulsanti Sposta su e Sposta giù per disporre le regole. Come per &lt;l'elemento di autorizzazione&gt; ASP.NET, le regole vengono elaborate nell'ordine in cui vengono visualizzate. In altre parole, se l'ordine delle regole nella ripresa precedente fosse invertito, nessuno avrebbe accesso alla cartella Amministrazione perché la prima regola che ASP.NET avrebbe incontrato sarebbe la regola che nega tutti alla cartella.

ASP.NET 2.0 aggiunge un file web.config alla cartella per la quale si specifica una regola di accesso. Le regole di accesso possono essere modificate tramite il file di configurazione o tramite lo strumento Amministrazione sito Web. In altre parole, lo strumento Amministrazione sito Web è semplicemente un'interfaccia tramite la quale il file di configurazione può essere modificato in un ambiente di facile utilizzo.

## <a name="using-roles-in-code"></a>Utilizzo dei ruoli nel codiceUsing Roles in Code

L'API per la gestione dei ruoli non è stata modificata dalla versione 1.x.The API for role management has not changed since version 1.x. Il **IsInRole** metodo viene utilizzato per determinare se un utente è in un ruolo specifico.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET crea inoltre un RolePrincipal istanza come membro del contesto corrente. L'oggetto RolePrincipal può essere utilizzato per ottenere tutti i ruoli a cui appartiene l'utente nel modo seguente:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Utilizzo di RoleGroups con il controllo LoginViewUsing RoleGroups with the LoginView Control

Ora che si ha una conoscenza della gestione dei ruoli e l'appartenenza, consente di discutere brevemente come il controllo LoginView sfrutta questa funzionalità in ASP.NET 2.0. Come illustrato in precedenza, il controllo LoginView è un controllo basato su modelli che contiene due modelli per impostazione predefinita; l'oggetto AnonymousTemplate e il LoggedInTemplate. Nella finestra di dialogo Attività LoginView è riportato un collegamento (illustrato di seguito) che consente di modificare RoleGroups.In the LoginView Tasks dialog is a link (shown below) that allows you to edit RoleGroups.

![](membership/_static/image9.jpg)

**Figura 9**

Ogni RoleGroup oggetto contiene una matrice di stringhe che definisce i ruoli che RoleGroup si applica a. Per aggiungere un nuovo RoleGroup al controllo LoginView, fare clic sul collegamento Modifica RoleGroups. Nell'immagine precedente, è possibile vedere che è stato aggiunto un nuovo RoleGroup per gli amministratori. Selezionando tale RoleGroup (RoleGroup[0]) dall'elenco a discesa Visualizzazioni, è possibile configurare un modello che verrà visualizzato solo ai membri del ruolo Administrators. Nell'immagine seguente è stato aggiunto un nuovo RoleGroup che si applica ai membri del ruolo Sales e del ruolo Distribution. In questo modo viene aggiunto un secondo RoleGroup all'elenco a discesa Visualizzazioni nella finestra di dialogo Attività LoginView e qualsiasi elemento aggiunto a tale modello sarà visibile da qualsiasi utente nel ruolo Vendite o Distribuzione.

![](membership/_static/image10.jpg)

**Figura 10**

## <a name="overriding-the-existing-membership-provider"></a>Override del provider di appartenenze esistente

Esistono un paio di modi per estendere la funzionalità di ASP.NET l'appartenenza. Prima di tutto, è ovviamente possibile modificare la funzionalità esistente della classe SqlMembershipProvider ereditando da essa ed eseguendo l'override dei relativi metodi. Ad esempio, se si desidera implementare funzionalità personalizzate quando vengono creati gli utenti, è possibile creare una classe personalizzata che eredita da SqlMembershipProvider come segue:For example, if you want to implement your own functionality when users are created, you can create your own class that inherits from SqlMembershipProvider as follows:

[!code-csharp[Main](membership/samples/sample5.cs)]

Se, d'altra parte, si desidera creare un provider personalizzato (per archiviare le informazioni sull'appartenenza in un database di Access, ad esempio), è possibile creare un provider personalizzato.

## <a name="creating-your-own-membership-provider"></a>Creazione di un provider di appartenenze personalizzatoCreating Your Own Membership Provider

Per creare il proprio provider di appartenenze, è innanzitutto necessario creare una classe che eredita dalla classe MembershipProvider.To create your own membership provider, you will first need to create a class that inherits from the MembershipProvider class. Se si utilizza VB.NET, Visual Studio 2005 aggiungerà gli stub per tutti i metodi di cui è necessario eseguire l'override. Se si utilizza C , è fino a voi per aggiungere gli stub.

È necessario eseguire l'override di quanto segue:

- ApplicationName (proprietà)
- ChangePassword (funzione)
- ChangePasswordQuestionAndAnswer (funzione)
- CreateUser (funzione)
- DeleteUser (funzione)
- EnablePasswordReset (proprietà)
- EnablePasswordRetrieval (proprietà)
- FindUsersByEmail (funzione)
- FindUsersByName (funzione)
- GetAllUsers (funzione)
- GetNumberOfUsersOnline (funzione)GetNumberOfUsersOnline function
- GetPassword (funzione)
- GetUser (funzione)
- GetUserNameByEmail (funzione)
- MaxInvalidPasswordAttempts (proprietà)
- MinRequiredNonAlphanumericCharacters (proprietà)
- MinRequiredPasswordLength (proprietà)
- PasswordAttemptWindow (proprietà)
- PasswordFormat (proprietà)
- PasswordStrengthRegularExpression (proprietà)
- RequiresQuestionAndAnswer (proprietà)
- RequiresUniqueEmail (proprietà)
- ResetPassword (funzione)
- Sblocca funzione utente
- UpdateUser (funzione)
- ValidateUser (funzione)

Che è piuttosto un elenco da implementare come uno sviluppatore di C. Potrebbe essere più semplice creare la classe in VB.NET senza alcuna implementazione e quindi utilizzare .NET Reflector o uno strumento simile per convertire il codice in C .

La stringa di connessione e altre proprietà devono essere impostate sui valori predefiniti nel metodo Initialize.The connection string and other properties should be set to their defaults in the Initialize method. Il metodo Initialize viene generato quando il provider viene caricato in fase di esecuzione. Il secondo parametro del metodo Initialize è di tipo System.Collections.Specialized.NameValueCollection ed è un riferimento all'elemento &lt;add&gt; associato al provider personalizzato nel file web.config. Tale voce è simile alla seguente:

[!code-xml[Main](membership/samples/sample6.xml)]

Di seguito è riportato un esempio del Initialize metodo.

[!code-csharp[Main](membership/samples/sample7.cs)]

Per convalidare l'utente quando invia il modulo di accesso, è necessario utilizzare il ValidateUser metodo. Questo metodo viene generato quando l'utente fa clic sul pulsante di accesso nel controllo di accesso. Inserirà il codice che esegue la ricerca dell'utente in questo metodo.

Come si può vedere, scrivere il proprio provider di appartenenze non è difficile e consente di estendere questa potente funzionalità di ASP.NET 2.0.
