---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
title: Convalida delle credenziali utente rispetto all'archivio utente di appartenenza (C#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato come convalidare le credenziali di un utente rispetto all'archivio utente di appartenenza utilizzando sia il metodo programmatico che il controllo di accesso....
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 61aa4e08-aa81-4aeb-8ebe-19ba7a65e04c
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
msc.type: authoredcontent
ms.openlocfilehash: fd50f4e63c75cf77eb21629d0c11a859da6b357d
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2020
ms.locfileid: "89045234"
---
# <a name="validating-user-credentials-against-the-membership-user-store-c"></a>Convalida delle credenziali utente rispetto all'archivio utente di appartenenza (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_CS.zip) o [Scarica PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_cs.pdf)

> In questa esercitazione verrà illustrato come convalidare le credenziali di un utente rispetto all'archivio utente di appartenenza utilizzando sia il metodo programmatico che il controllo di accesso. Si esaminerà anche come personalizzare l'aspetto e il comportamento del controllo di accesso.

## <a name="introduction"></a>Introduzione

Nell' <a id="Tutorial05"></a> [esercitazione precedente](creating-user-accounts-cs.md) si è appreso come creare un nuovo account utente nel Framework di appartenenza. Per prima cosa è stata esaminata la creazione di account utente a livello di codice tramite il `Membership` metodo della classe `CreateUser` , quindi è stato esaminato l'utilizzo del controllo Web CreateUserWizard. Tuttavia, la pagina di accesso convalida attualmente le credenziali fornite rispetto a un elenco hardcoded di coppie nome utente e password. È necessario aggiornare la logica della pagina di accesso in modo da convalidare le credenziali rispetto all'archivio utenti del Framework di appartenenza.

Analogamente alla creazione di account utente, è possibile convalidare le credenziali a livello di programmazione o in modo dichiarativo. L'API di appartenenza include un metodo per convalidare le credenziali di un utente a livello di codice rispetto all'archivio utente. ASP.NET viene fornito con il controllo Web login, che esegue il rendering di un'interfaccia utente con caselle di testo per nome utente e password e un pulsante per l'accesso.

In questa esercitazione verrà illustrato come convalidare le credenziali di un utente rispetto all'archivio utente di appartenenza utilizzando sia il metodo programmatico che il controllo di accesso. Si esaminerà anche come personalizzare l'aspetto e il comportamento del controllo di accesso. È possibile iniziare subito.

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Passaggio 1: convalida delle credenziali rispetto all'archivio utenti di appartenenza

Per i siti Web che utilizzano l'autenticazione basata su form, un utente accede al sito Web visitando una pagina di accesso e immettendo le credenziali. Queste credenziali vengono quindi confrontate con l'archivio utente. Se sono validi, all'utente viene concesso un ticket di autenticazione basata su form, ovvero un token di sicurezza che indica l'identità e l'autenticità del visitatore.

Per convalidare un utente nel Framework di appartenenza, usare il `Membership` [ `ValidateUser` Metodo](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)della classe. Il `ValidateUser` metodo accetta due parametri di input, *`username`* e, *`password`* e restituisce un valore booleano che indica se le credenziali sono valide. Analogamente al `CreateUser` Metodo esaminato nell'esercitazione precedente, il `ValidateUser` metodo delega la convalida effettiva al provider di appartenenze configurato.

`SqlMembershipProvider`Consente di convalidare le credenziali fornite ottenendo la password dell'utente specificato tramite il `aspnet_Membership_GetPasswordWithFormat` stored procedure. Ricordare che `SqlMembershipProvider` archivia le password degli utenti usando uno dei tre formati seguenti: Clear, Encrypted o Hashed. Il `aspnet_Membership_GetPasswordWithFormat` stored procedure restituisce la password nel formato non elaborato. Per le password crittografate o con hash, il `SqlMembershipProvider` trasforma il *`password`* valore passato nel `ValidateUser` Metodo nello stato crittografato o con hash equivalente, quindi lo confronta con quello che è stato restituito dal database. Se la password archiviata nel database corrisponde alla password formattata immessa dall'utente, le credenziali sono valide.

Verrà ora aggiornata la pagina di accesso (~/ `Login.aspx` ) in modo da convalidare le credenziali fornite rispetto all'archivio utenti del Framework di appartenenza. Questa pagina di accesso è stata creata nuovamente nell' <a id="Tutorial02"></a> esercitazione [*Panoramica dell'autenticazione basata su form*](../introduction/an-overview-of-forms-authentication-cs.md) , creazione di un'interfaccia con due caselle di testo per nome utente e password, una casella di controllo Remember me e un pulsante di accesso (vedere la figura 1). Il codice convalida le credenziali immesse rispetto a un elenco hardcoded di coppie di nome utente e password (Scott/password, Jisun/password e Sam/password). Nell'esercitazione sulla <a id="Tutorial03"></a> [*configurazione dell'autenticazione basata su form e sugli argomenti avanzati*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) è stato aggiornato il codice della pagina di accesso per archiviare informazioni aggiuntive nella proprietà del ticket di autenticazione basata su form `UserData` .

[![L'interfaccia della pagina di accesso include due caselle di testo, un oggetto CheckBoxList e un pulsante](validating-user-credentials-against-the-membership-user-store-cs/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image1.png)

**Figura 1**: l'interfaccia della pagina di accesso include due caselle di testo, un oggetto CheckBoxList e un pulsante ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-cs/_static/image3.png))

L'interfaccia utente della pagina di accesso può rimanere invariata, ma è necessario sostituire il gestore eventi del pulsante di accesso `Click` con il codice che convalida l'utente rispetto all'archivio utenti del Framework di appartenenza. Aggiornare il gestore eventi in modo che il relativo codice venga visualizzato come segue:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample1.cs)]

Questo codice è molto semplice. Si inizia chiamando il `Membership.ValidateUser` metodo, passando il nome utente e la password specificati. Se il metodo restituisce true, l'utente viene connesso al sito tramite il `FormsAuthentication` metodo RedirectFromLoginPage della classe. Come illustrato nel <a id="Tutorial2"></a> [*Panoramica dell'esercitazione sull'autenticazione basata su form*](../introduction/an-overview-of-forms-authentication-cs.md) , la `FormsAuthentication.RedirectFromLoginPage` Crea il ticket di autenticazione basata su form e quindi reindirizza l'utente alla pagina appropriata. Se le credenziali non sono valide, tuttavia, `InvalidCredentialsMessage` viene visualizzata l'etichetta, che informa l'utente che il nome utente o la password non è corretta.

Questo è tutto ciò che occorre fare.

Per verificare che la pagina di accesso funzioni come previsto, provare a eseguire l'accesso con uno degli account utente creati nell'esercitazione precedente. In alternativa, se non è ancora stato creato un account, procedere e crearne uno dalla `~/Membership/CreatingUserAccounts.aspx` pagina.

> [!NOTE]
> Quando l'utente immette le proprie credenziali e invia il modulo della pagina di accesso, le credenziali, inclusa la password, vengono trasmesse tramite Internet al server Web come *testo normale*. Ciò significa che qualsiasi hacker che analizza il traffico di rete può visualizzare il nome utente e la password. Per evitare questo problema, è fondamentale crittografare il traffico di rete tramite [SSL (Secure Socket Layer)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). In questo modo, le credenziali, nonché il markup HTML dell'intera pagina, verranno crittografate dal momento in cui lasciano il browser fino a quando non vengono ricevute dal server Web.

### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Come il Framework di appartenenza gestisce i tentativi di accesso non validi

Quando un visitatore raggiunge la pagina di accesso e invia le credenziali, il browser esegue una richiesta HTTP alla pagina di accesso. Se le credenziali sono valide, la risposta HTTP include il ticket di autenticazione in un cookie. Un pirata informatico che tenta di suddividere il sito potrebbe quindi creare un programma che invii in modo esauriente le richieste HTTP alla pagina di accesso con un nome utente valido e una supposizione alla password. Se la password è corretta, la pagina di accesso restituirà il cookie del ticket di autenticazione e a questo punto il programma saprà che si è verificato un ostacolo a una coppia nome utente/password valida. Attraverso la forza bruta, un programma di questo tipo potrebbe essere in grado di inciampare sulla password di un utente, soprattutto se la password è debole.

Per evitare attacchi di forza bruta, il Framework di appartenenza blocca un utente se è presente un certo numero di tentativi di accesso non riusciti entro un determinato periodo di tempo. I parametri esatti sono configurabili tramite le due impostazioni di configurazione del provider di appartenenza seguenti:

- `maxInvalidPasswordAttempts` : specifica il numero di tentativi di password non validi consentiti per l'utente entro il periodo di tempo prima che l'account venga bloccato. Il valore predefinito è 5.
- `passwordAttemptWindow` -indica il periodo di tempo in minuti durante il quale il numero specificato di tentativi di accesso non validi provocherà il blocco dell'account. Il valore predefinito è 10.

Se un utente è stato bloccato, non è in grado di eseguire l'accesso finché un amministratore non sblocca l'account. Quando un utente viene bloccato, il `ValidateUser` metodo restituirà *sempre* `false` , anche se vengono fornite credenziali valide. Sebbene questo comportamento riduca la probabilità che un pirata informatico entri nel sito tramite metodi di forza bruta, può causare il blocco di un utente valido che ha semplicemente dimenticato la password o ha accidentalmente il blocco di maiuscole e minuscole.

Sfortunatamente, non esiste alcuno strumento incorporato per sbloccare un account utente. Per sbloccare un account, è possibile modificare direttamente il database: modificare il `IsLockedOut` campo nella `aspnet_Membership` tabella per l'account utente appropriato oppure creare un'interfaccia basata sul Web in cui sono elencati gli account bloccati con le opzioni per sbloccarli. Si esaminerà la creazione di interfacce amministrative per eseguire attività comuni relative a account utente e ruoli in un'esercitazione futura.

> [!NOTE]
> Un aspetto negativo del `ValidateUser` metodo è che, quando le credenziali fornite non sono valide, non fornisce alcuna spiegazione del motivo. Le credenziali potrebbero non essere valide perché non esiste una coppia nome utente/password corrispondente nell'archivio utente oppure perché l'utente non è ancora stato approvato oppure perché l'utente è stato bloccato. Nel passaggio 4 verrà illustrato come visualizzare un messaggio più dettagliato all'utente quando il tentativo di accesso ha esito negativo.

## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Passaggio 2: raccolta delle credenziali tramite il controllo Web di accesso

Il [controllo Web di accesso](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) esegue il rendering di un'interfaccia utente predefinita molto simile a quella creata nell'esercitazione <a id="SKM5"></a> [*Panoramica dell'autenticazione basata su form*](../introduction/an-overview-of-forms-authentication-cs.md) . L'utilizzo del controllo Login consente di evitare di dover creare l'interfaccia per raccogliere le credenziali del visitatore. Inoltre, il controllo di accesso firma automaticamente l'utente (presupponendo che le credenziali inviate siano valide), evitando così di dover scrivere codice.

È ora necessario aggiornare `Login.aspx` , sostituendo l'interfaccia e il codice creati manualmente con un controllo di accesso. Per iniziare, rimuovere il markup esistente e il codice in `Login.aspx` . È possibile eliminarlo o semplicemente impostarlo come commento. Per impostare come commento il markup dichiarativo, racchiuderlo tra i `<%--` `--%>` delimitatori e. È possibile immettere questi delimitatori manualmente oppure, come illustrato nella figura 2, è possibile selezionare il testo da impostare come commento, quindi fare clic sull'icona imposta le righe selezionate come commento sulla barra degli strumenti. Analogamente, è possibile usare l'icona imposta righe selezionate come commento per impostare come commento il codice selezionato nella classe code-behind.

[![Impostare come commento il markup dichiarativo e il codice sorgente esistenti in login. aspx](validating-user-credentials-against-the-membership-user-store-cs/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image4.png)

**Figura 2**: impostare come commento il markup dichiarativo esistente e il codice sorgente in `Login.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-cs/_static/image6.png))

> [!NOTE]
> L'icona Imposta come commento le righe selezionate non è disponibile quando si Visualizza il markup dichiarativo in Visual Studio 2005. Se non si usa Visual Studio 2008, sarà necessario aggiungere manualmente i `<%--` `--%>` delimitatori e.

Trascinare quindi un controllo Login dalla casella degli strumenti nella pagina e impostare la relativa `ID` proprietà su `myLogin` . A questo punto la schermata dovrebbe essere simile alla figura 3. Si noti che l'interfaccia predefinita del controllo Login include i controlli TextBox per il nome utente e la password, la casella di controllo memorizza l'ora successiva e un pulsante Accedi. Sono inoltre presenti `RequiredFieldValidator` controlli per le due caselle di testo.

[![Aggiungere un controllo di accesso alla pagina](validating-user-credentials-against-the-membership-user-store-cs/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image7.png)

**Figura 3**: aggiungere un controllo di accesso alla pagina ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-cs/_static/image9.png))

E abbiamo finito! Quando si fa clic sul pulsante Accedi del controllo di accesso, si verificherà un postback e il controllo Login chiamerà il `Membership.ValidateUser` metodo, passando il nome utente e la password immessi. Se le credenziali non sono valide, il controllo di accesso Visualizza un messaggio che informa tale. Se, tuttavia, le credenziali sono valide, il controllo di accesso crea il ticket di autenticazione basata su form e reindirizza l'utente alla pagina appropriata.

Il controllo Login utilizza quattro fattori per determinare la pagina appropriata a cui reindirizzare l'utente dopo un accesso riuscito:

- Indica se il controllo di accesso si trova nella pagina di accesso, come definito dall' `loginUrl` impostazione nella configurazione dell'autenticazione basata su form. il valore predefinito di questa impostazione è `Login.aspx`
- Presenza di un `ReturnUrl` parametro QueryString
- Valore della [ `DestinationUrl` Proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx) del controllo Login
- Il `defaultUrl` valore specificato nelle impostazioni di configurazione dell'autenticazione basata su form. il valore predefinito di questa impostazione è `Default.aspx`

Nella figura 4 viene illustrata la modalità di utilizzo di questi quattro parametri da parte del controllo Login per ottenere la decisione appropriata per la pagina.

[![Aggiungere un controllo di accesso alla pagina](validating-user-credentials-against-the-membership-user-store-cs/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image10.png)

**Figura 4**: aggiungere un controllo di accesso alla pagina ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-cs/_static/image12.png))

Per testare il controllo degli accessi, visitare il sito tramite un browser ed eseguire l'accesso come utente esistente nel framework delle appartenenze.

L'interfaccia sottoposta a rendering del controllo Login è altamente configurabile. Sono disponibili numerose proprietà che ne influenzano l'aspetto; il controllo di accesso può essere convertito in un modello per un controllo preciso sul layout degli elementi dell'interfaccia utente. Il resto di questo passaggio esamina come personalizzare l'aspetto e il layout.

### <a name="customizing-the-login-controls-appearance"></a>Personalizzazione dell'aspetto del controllo Login

Le impostazioni predefinite delle proprietà del controllo di accesso eseguono il rendering di un'interfaccia utente con un titolo (accesso), una casella di testo e i controlli etichetta per gli input di nome utente e password, una casella di controllo memorizzarmi la volta successiva e un pulsante Accedi. Le caratteristiche di questi elementi sono tutte configurabili tramite le numerose proprietà del controllo di accesso. Inoltre, ulteriori elementi dell'interfaccia utente, ad esempio un collegamento a una pagina per creare un nuovo account utente, possono essere aggiunti impostando una proprietà o due.

Dedicare alcuni istanti a Gussy l'aspetto del controllo di accesso. Poiché la `Login.aspx` pagina dispone già di un testo nella parte superiore della pagina con l'account di accesso, il titolo del controllo di accesso è superfluo. Quindi, cancellare il valore della [ `TitleText` Proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx) per rimuovere il titolo del controllo di accesso.

Nome utente: e password: le etichette a sinistra dei due controlli casella di testo possono essere personalizzate rispettivamente tramite [`UserNameLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) le [ `PasswordLabelText` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx)e. Modificare il nome utente: etichetta in lettura nome utente:. Gli stili Label e TextBox sono configurabili rispettivamente tramite le [`LabelStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx) [ `TextBoxStyle` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx)e.

La proprietà Text della casella di controllo memorizza la prossima volta può essere impostata tramite l'oggetto del controllo di accesso [`RememberMeText property`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx) e lo stato di selezione predefinito è configurabile tramite l'oggetto [`RememberMeSet property`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx) (il valore predefinito è false). Procedere e impostare la `RememberMeSet` proprietà su true in modo che la casella di controllo memorizza ora successiva verrà selezionata per impostazione predefinita.

Il controllo Login offre due proprietà per la regolazione del layout dei controlli dell'interfaccia utente. [`TextLayout property`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx)Indica se le etichette username: e password: vengono visualizzate a sinistra delle caselle di testo corrispondenti (impostazione predefinita) o superiori. [`Orientation property`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx)Indica se gli input di nome utente e password sono posizionati verticalmente (uno sopra l'altro) o orizzontalmente. Queste due proprietà verranno impostate sui valori predefiniti, ma si consiglia di provare a impostare queste due proprietà sui valori non predefiniti per vedere l'effetto risultante.

> [!NOTE]
> Nella sezione successiva, configurazione del layout del controllo di accesso, si osserverà l'uso dei modelli per definire il layout preciso degli elementi dell'interfaccia utente del controllo layout.

Eseguire il wrapping delle impostazioni delle proprietà del controllo di accesso impostando le [`CreateUserText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx) [ `CreateUserUrl` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx) e su non ancora registrate? Creare un account. `~/Membership/CreatingUserAccounts.aspx`rispettivamente e. Viene aggiunto un collegamento ipertestuale all'interfaccia del controllo Login che punta alla pagina creata nell' <a id="SKM6"></a> [esercitazione precedente](creating-user-accounts-cs.md). Le proprietà e e proprietà del controllo Login e [`HelpPageText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx) [ `HelpPageUrl` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx) [`PasswordRecoveryText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) funzionano allo stesso modo, eseguendo il rendering dei collegamenti a una pagina della guida e una pagina di recupero della password. [ `PasswordRecoveryUrl` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx)

Dopo aver apportato queste modifiche alle proprietà, il markup dichiarativo e l'aspetto del controllo di accesso dovrebbero essere simili a quelli illustrati nella figura 5.

[![I valori delle proprietà del controllo di accesso ne determinano l'aspetto](validating-user-credentials-against-the-membership-user-store-cs/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image13.png)

**Figura 5**: i valori delle proprietà del controllo di accesso ne determinano l'aspetto ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-cs/_static/image15.png))

### <a name="configuring-the-login-controls-layout"></a>Configurazione del layout del controllo di accesso

L'interfaccia utente predefinita del controllo Web login dispone l'interfaccia in un HTML `<table>` . Ma cosa accade se è necessario un controllo più preciso sull'output sottoposto a rendering? Forse si vuole sostituire `<table>` con una serie di `<div>` tag. O cosa accade se l'applicazione richiede credenziali aggiuntive per l'autenticazione? Molti siti web finanziari, ad esempio, richiedono che gli utenti forniscano non solo un nome utente e una password, ma anche un numero di identificazione personale (PIN) o altre informazioni di identificazione. Qualunque sia il motivo, è possibile convertire il controllo Login in un modello, da cui è possibile definire in modo esplicito il markup dichiarativo dell'interfaccia.

Per aggiornare il controllo Login per raccogliere credenziali aggiuntive, è necessario eseguire due operazioni:

1. Aggiornare l'interfaccia del controllo Login per includere i controlli Web per raccogliere le credenziali aggiuntive.
2. Eseguire l'override della logica di autenticazione interna del controllo di accesso in modo che un utente venga autenticato solo se il nome utente e la password sono validi e anche le credenziali aggiuntive sono valide.

Per eseguire la prima attività, è necessario convertire il controllo Login in un modello e aggiungere i controlli Web necessari. Come per la seconda attività, la logica di autenticazione del controllo Login può essere sostituita dalla creazione di un gestore eventi per l' [ `Authenticate` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx)del controllo.

Aggiornare il controllo Login in modo da richiedere agli utenti il nome utente, la password e l'indirizzo di posta elettronica e autenticare l'utente solo se l'indirizzo di posta elettronica specificato corrisponde al proprio indirizzo di posta elettronica nel file. Prima di tutto è necessario convertire l'interfaccia del controllo Login in un modello. Dallo smart tag del controllo Login scegliere l'opzione Converti in modello.

[![Convertire il controllo Login in un modello](validating-user-credentials-against-the-membership-user-store-cs/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image16.png)

**Figura 6**: convertire il controllo Login in un modello ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-cs/_static/image18.png))

> [!NOTE]
> Per ripristinare la versione pre-modello del controllo di accesso, fare clic sul collegamento Reimposta dallo smart tag del controllo.

La conversione del controllo Login in un modello consente di aggiungere un oggetto `LayoutTemplate` al markup dichiarativo del controllo con elementi HTML e controlli Web che definiscono l'interfaccia utente. Come illustrato nella figura 7, la conversione del controllo in un modello consente di rimuovere un certo numero di proprietà dall'Finestra Proprietà, ad esempio `TitleText` , `CreateUserUrl` e così via, in quanto questi valori di proprietà vengono ignorati quando si usa un modello.

[![Sono disponibili meno proprietà quando il controllo account di accesso viene convertito in un modello](validating-user-credentials-against-the-membership-user-store-cs/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image19.png)

**Figura 7**: un numero inferiore di proprietà è disponibile quando il controllo di accesso viene convertito in un modello ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-cs/_static/image21.png))

Il markup HTML in `LayoutTemplate` può essere modificato in base alle esigenze. Analogamente, è possibile aggiungere nuovi controlli Web al modello. Tuttavia, è importante che i controlli Web principali del controllo di accesso rimangano nel modello e mantengano i `ID` valori assegnati. In particolare, non rimuovere o rinominare le `UserName` caselle di `Password` testo o, la `RememberMe` casella di controllo, il `LoginButton` pulsante, l' `FailureText` etichetta o i `RequiredFieldValidator` controlli.

Per raccogliere l'indirizzo di posta elettronica del visitatore, è necessario aggiungere una casella di testo al modello. Aggiungere il markup dichiarativo seguente tra la riga della tabella ( `<tr>` ) che contiene la casella di `Password` testo e la riga della tabella che contiene la casella di controllo Remember me next time:

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample2.aspx)]

Dopo l'aggiunta della `Email` casella di testo, visitare la pagina tramite un browser. Come illustrato nella figura 8, l'interfaccia utente del controllo di accesso include ora una terza casella di testo.

[![Il controllo di accesso include ora una casella di testo per l'indirizzo di posta elettronica dell'utente](validating-user-credentials-against-the-membership-user-store-cs/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image22.png)

**Figura 8**: il controllo account di accesso include ora una casella di testo per l'indirizzo di posta elettronica dell'utente ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-cs/_static/image24.png))

A questo punto, il controllo di accesso usa ancora il `Membership.ValidateUser` metodo per convalidare le credenziali fornite. In corrispondenza di, il valore immesso nella `Email` casella di testo non determina se l'utente può eseguire l'accesso. Nel passaggio 3 verrà illustrato come eseguire l'override della logica di autenticazione del controllo di accesso in modo che le credenziali vengano considerate valide solo se il nome utente e la password sono validi e l'indirizzo di posta elettronica fornito corrisponde all'indirizzo di posta elettronica del file.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Passaggio 3: modifica della logica di autenticazione del controllo Login

Quando un visitatore fornisce le proprie credenziali e fa clic sul pulsante Accedi, viene seguito un postback e il controllo degli accessi avanza attraverso il flusso di lavoro di autenticazione. Il flusso di lavoro inizia con la generazione dell' [ `LoggingIn` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx). Qualsiasi gestore eventi associato a questo evento può annullare l'operazione di accesso impostando la `e.Cancel` proprietà su `true` .

Se l'operazione di accesso non viene annullata, il flusso di lavoro avanza generando l' [ `Authenticate` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx). Se è presente un gestore eventi per l' `Authenticate` evento, è responsabile di determinare se le credenziali specificate sono valide o meno. Se non viene specificato alcun gestore eventi, il controllo Login usa il `Membership.ValidateUser` metodo per determinare la validità delle credenziali.

Se le credenziali specificate sono valide, viene creato il ticket di autenticazione basata su form, viene generato l' [ `LoggedIn` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx) e l'utente viene reindirizzato alla pagina appropriata. Se, tuttavia, le credenziali vengono ritenute non valide, viene generato l' [ `LoginError` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx) e viene visualizzato un messaggio che informa l'utente che le credenziali non sono valide. Per impostazione predefinita, in caso di errore, il controllo di accesso imposta semplicemente la `FailureText` proprietà Text del controllo etichetta su un messaggio di errore (il tentativo di accesso non è riuscito. Riprovare. Tuttavia, se la [ `FailureAction` Proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx) del controllo di accesso è impostata su `RedirectToLoginPage` , il controllo di accesso rilascia un oggetto `Response.Redirect` alla pagina di accesso aggiungendo il parametro QueryString `loginfailure=1` , che determina la visualizzazione del messaggio di errore da parte del controllo Login.

La figura 9 offre un diagramma di flusso del flusso di lavoro di autenticazione.

[![Flusso di lavoro di autenticazione del controllo Login](validating-user-credentials-against-the-membership-user-store-cs/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image25.png)

**Figura 9**: flusso di lavoro di autenticazione del controllo Login ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-cs/_static/image27.png))

> [!NOTE]
> Se si sta chiedendo quando si usa l' `FailureAction` opzione di `RedirectToLogin` pagina di, considerare lo scenario seguente. A questo punto, la `Site.master` pagina master ha attualmente il testo, Hello, sconosciuto visualizzato nella colonna sinistra quando viene visitato da un utente anonimo, ma si supponga di voler sostituire il testo con un controllo di accesso. Questo consentirebbe a un utente anonimo di accedere da qualsiasi pagina nel sito, anziché richiedere la visita diretta della pagina di accesso. Tuttavia, se un utente non è stato in grado di accedere tramite il controllo di accesso di cui è stato eseguito il rendering dalla pagina master, potrebbe essere opportuno reindirizzarli alla pagina di accesso ( `Login.aspx` ) perché la pagina probabilmente include istruzioni aggiuntive, collegamenti e altre informazioni, ad esempio collegamenti per creare un nuovo account o recuperare una password persa, che non sono stati aggiunti alla pagina master.

### <a name="creating-theauthenticateevent-handler"></a>Creazione del `Authenticate` gestore eventi

Per inserire la logica di autenticazione personalizzata, è necessario creare un gestore eventi per l'evento del controllo di accesso `Authenticate` . La creazione di un gestore eventi per l'evento genererà `Authenticate` la definizione del gestore eventi seguente:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample3.cs)]

Come si può notare, al `Authenticate` gestore dell'evento viene passato un oggetto di tipo [`AuthenticateEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) come secondo parametro di input. La `AuthenticateEventArgs` classe contiene una proprietà booleana denominata `Authenticated` utilizzata per specificare se le credenziali specificate sono valide. L'attività, quindi, consiste nel scrivere il codice che determina se le credenziali specificate sono valide o meno e per impostare la `e.Authenticate` proprietà di conseguenza.

### <a name="determining-and-validating-the-supplied-credentials"></a>Determinazione e convalida delle credenziali fornite

Utilizzare le [`UserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx) [ `Password` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx) e del controllo di accesso per determinare le credenziali del nome utente e della password immesse dall'utente. Per determinare i valori immessi in tutti i controlli Web aggiuntivi, ad esempio la `Email` casella di testo aggiunta nel passaggio precedente, usare *`LoginControlID`* `.FindControl` (" *`controlID`* ") per ottenere un riferimento a livello di codice al controllo Web nel modello la cui `ID` proprietà è uguale a *`controlID`* . Ad esempio, per ottenere un riferimento alla `Email` casella di testo, usare il codice seguente:

`TextBox EmailTextBox = myLogin.FindControl("Email") as TextBox;`

Per convalidare le credenziali dell'utente, è necessario eseguire due operazioni:

1. Verificare che il nome utente e la password specificati siano validi
2. Verificare che l'indirizzo di posta elettronica immesso corrisponda all'indirizzo di posta elettronica nel file per l'utente che prova ad accedere

Per eseguire il primo controllo, è possibile usare semplicemente il `Membership.ValidateUser` metodo come illustrato nel passaggio 1. Per il secondo controllo, è necessario determinare l'indirizzo di posta elettronica dell'utente in modo da poterlo confrontare con l'indirizzo di posta elettronica immesso nel controllo TextBox. Per ottenere informazioni su un utente specifico, usare il `Membership` [ `GetUser` Metodo](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx)della classe.

Il `GetUser` metodo dispone di un numero di overload. Se utilizzato senza passare alcun parametro, vengono restituite informazioni sull'utente attualmente connesso. Per ottenere informazioni su un determinato utente, chiamare `GetUser` il passaggio del nome utente. In entrambi i casi, `GetUser` restituisce un [ `MembershipUser` oggetto](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)con proprietà quali `UserName` , `Email` , `IsApproved` , `IsOnline` e così via.

Il codice seguente implementa questi due controlli. Se entrambi passano, `e.Authenticate` viene impostato su `true` , in caso contrario viene assegnato `false` .

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample4.cs)]

Con questo codice, provare ad accedere come utente valido, immettendo il nome utente, la password e l'indirizzo di posta elettronica corretti. Riprovare, ma questa volta usare intenzionalmente un indirizzo di posta elettronica non corretto (vedere la figura 10). Infine, provare a usarlo una terza volta usando un nome utente inesistente. Nel primo caso si dovrebbe accedere correttamente al sito, ma negli ultimi due casi dovrebbe essere visualizzato il messaggio di credenziali non valido del controllo di accesso.

[![Tito non può accedere quando si specifica un indirizzo di posta elettronica non corretto](validating-user-credentials-against-the-membership-user-store-cs/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image28.png)

**Figura 10**: Tito non può accedere quando si specifica un indirizzo di posta elettronica non corretto ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-cs/_static/image30.png))

> [!NOTE]
> Come illustrato nella sezione come il Framework di appartenenza gestisce i tentativi di accesso non validi nel passaggio 1, quando `Membership.ValidateUser` viene chiamato il metodo e vengono passate credenziali non valide, tiene traccia del tentativo di accesso non valido e blocca l'utente se supera una determinata soglia di tentativi non validi in un intervallo di tempo specificato. Poiché la logica di autenticazione personalizzata chiama il `ValidateUser` metodo, una password non corretta per un nome utente valido incrementerà il contatore dei tentativi di accesso non valido, ma questo contatore non viene incrementato nel caso in cui il nome utente e la password siano validi, ma l'indirizzo di posta elettronica non è corretto. È probabile che questo comportamento sia appropriato, poiché è improbabile che un pirata informatico conosca il nome utente e la password, ma debba usare tecniche di forza bruta per determinare l'indirizzo di posta elettronica dell'utente.

## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Passaggio 4: miglioramento del messaggio di credenziali non valido per il controllo di accesso

Quando un utente tenta di accedere con credenziali non valide, il controllo di accesso Visualizza un messaggio che informa che il tentativo di accesso non è riuscito. In particolare, il controllo Visualizza il messaggio specificato dalla relativa [ `FailureText` Proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx), che ha un valore predefinito del tentativo di accesso non riuscito. Riprova.

Ricordare che esistono diversi motivi per cui le credenziali di un utente potrebbero non essere valide:

- Il nome utente potrebbe non esistere
- Il nome utente esiste, ma la password non è valida
- Il nome utente e la password sono validi, ma l'utente non è ancora approvato
- Il nome utente e la password sono validi, ma l'utente è bloccato (probabilmente perché ha superato il numero di tentativi di accesso non validi entro l'intervallo di tempo specificato)

E potrebbero esserci altri motivi per usare la logica di autenticazione personalizzata. Ad esempio, con il codice scritto nel passaggio 3, il nome utente e la password potrebbero essere validi, ma l'indirizzo di posta elettronica potrebbe non essere corretto.

Indipendentemente dal motivo per cui le credenziali non sono valide, il controllo di accesso Visualizza lo stesso messaggio di errore. Questa mancanza di feedback può generare confusione per un utente il cui account non è ancora stato approvato o che è stato bloccato. Con un po' di lavoro, tuttavia, possiamo fare in modo che il controllo di accesso visualizzi un messaggio più appropriato.

Ogni volta che un utente tenta di accedere con credenziali non valide, il controllo di accesso genera il proprio `LoginError` evento. Procedere con la creazione di un gestore eventi per questo evento e aggiungere il codice seguente:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample5.cs)]

Il codice precedente inizia impostando la proprietà del controllo Login sul `FailureText` valore predefinito (il tentativo di accesso non è riuscito. Riprovare. Viene quindi verificato se il nome utente specificato è mappato a un account utente esistente. In tal caso, vengono consultate le `MembershipUser` proprietà e dell'oggetto risultante `IsLockedOut` `IsApproved` per determinare se l'account è stato bloccato o non è ancora stato approvato. In entrambi i casi, la `FailureText` proprietà viene aggiornata a un valore corrispondente.

Per testare questo codice, provare intenzionalmente ad accedere come utente esistente, ma usare una password non corretta. Eseguire questa operazione cinque volte in una riga all'interno di un intervallo di tempo di 10 minuti e l'account verrà bloccato. Come illustrato nella figura 11, i tentativi di accesso successivi avranno sempre esito negativo (anche con la password corretta), ma ora visualizzerà il più descrittivo l'account è stato bloccato a causa di un numero eccessivo di tentativi di accesso non validi. Contattare l'amministratore per fare in modo che il proprio account abbia sbloccato il messaggio.

[![Tito ha eseguito un numero eccessivo di tentativi di accesso non validi ed è stato bloccato](validating-user-credentials-against-the-membership-user-store-cs/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image31.png)

**Figura 11**: Tito ha eseguito un numero eccessivo di tentativi di accesso non validi ed è stato bloccato ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-cs/_static/image33.png))

## <a name="summary"></a>Riepilogo

Prima di questa esercitazione, la pagina di accesso convalidava le credenziali fornite rispetto a un elenco hardcoded di coppie nome utente/password. In questa esercitazione è stata aggiornata la pagina per convalidare le credenziali rispetto al framework delle appartenenze. Nel passaggio 1 è stato esaminato l'utilizzo del `Membership.ValidateUser` metodo a livello di codice. Nel passaggio 2 sono state sostituite l'interfaccia utente e il codice creati manualmente con il controllo Login.

Il controllo Login esegue il rendering di un'interfaccia utente di accesso standard e convalida automaticamente le credenziali dell'utente rispetto al framework delle appartenenze. Inoltre, in caso di credenziali valide, il controllo di accesso firma l'utente tramite l'autenticazione basata su form. In breve, un'esperienza utente di accesso completamente funzionale è disponibile semplicemente trascinando il controllo di accesso in una pagina, non è necessario alcun markup o codice dichiarativo aggiuntivo. Il controllo degli accessi è molto personalizzabile, consentendo un livello di controllo accurato sull'interfaccia utente e la logica di autenticazione sottoposte a rendering.

A questo punto i visitatori del sito Web possono creare un nuovo account utente e accedere al sito, ma è ancora necessario esaminare la limitazione dell'accesso alle pagine in base all'utente autenticato. Attualmente, qualsiasi utente, autenticato o anonimo, può visualizzare qualsiasi pagina nel sito. Oltre a controllare l'accesso alle pagine del sito in base all'utente, è possibile che alcune pagine con funzionalità dipendano dall'utente. L'esercitazione successiva illustra come limitare l'accesso e la funzionalità nella pagina in base all'utente connesso.

Buona programmazione!

### <a name="further-reading"></a>Altre informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Visualizzazione di messaggi personalizzati per gli utenti bloccati e non approvati](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Esame dell'appartenenza, dei ruoli e del profilo di ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Procedura: creare una pagina di accesso di ASP.NET](https://msdn.microsoft.com/library/ms178331.aspx)
- [Documentazione tecnica per il controllo degli accessi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [Utilizzo dei controlli Login](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è *[Sams Teach Yourself ASP.NET 2,0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott può essere raggiunto in [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o tramite il suo Blog all'indirizzo [http://ScottOnWriting.NET](http://scottonwriting.net/) .

### <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Teresa Murphy e Michael Olivero. Sei interessato a esaminare i miei prossimi articoli MSDN? In caso affermativo, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com) .

> [!div class="step-by-step"]
> [Precedente](creating-user-accounts-cs.md) 
>  [Avanti](user-based-authorization-cs.md)
