---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
title: Autenticazione degli utenti con l'autenticazione basata su form (VB) Documenti Microsoft
author: rick-anderson
description: Informazioni su come usare l'attributo [Authorize] per proteggere con password determinate pagine nell'applicazione MVC. Si apprenderà come utilizzare anche l'amministrazione del sito Web...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 4341f5b1-6fe5-44c5-8b8a-18fa84f80177
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 9e3117af55db2effed20b6421c2322f1c265f1c7
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540818"
---
# <a name="authenticating-users-with-forms-authentication-vb"></a>Autenticazione degli utenti con l'autenticazione basata su form (VB)

da parte [di Microsoft](https://github.com/microsoft)

> Informazioni su come usare l'attributo [Authorize] per proteggere con password determinate pagine nell'applicazione MVC. Informazioni su come utilizzare lo strumento Amministrazione sito Web per creare e gestire utenti e ruoli. Viene inoltre illustrato come configurare la posizione in cui vengono archiviate le informazioni relative all'account utente e ai ruoli.

L'obiettivo di questa esercitazione è spiegare come è possibile utilizzare l'autenticazione basata su form per proteggere con password le visualizzazioni nelle applicazioni MVC ASP.NET. Informazioni su come utilizzare lo strumento Amministrazione sito Web per creare utenti e ruoli. Si apprenderà inoltre come impedire agli utenti non autorizzati di richiamare le azioni del controller. Infine, si apprenderà come configurare la posizione di archiviazione di nomi utente e password.

#### <a name="using-the-web-site-administration-tool"></a>Utilizzo dello strumento Amministrazione sito Web

Prima di fare qualsiasi altra cosa, dovremmo iniziare creando alcuni utenti e ruoli. Il modo più semplice per creare nuovi utenti e ruoli consiste nell'utilizzare lo strumento Amministrazione sito Web di Visual Studio 2008. È possibile avviare questo strumento selezionando l'opzione di menu **Progetto, ASP.NET Configurazione**. In alternativa, è possibile avviare lo strumento Amministrazione sito Web facendo clic sull'icona (in qualche modo spaventoso) del martello che colpisce il mondo visualizzato nella parte superiore della finestra Esplora soluzioni (vedere Figura 1).

**Figura 1 – Avvio dello strumento Amministrazione sito Web**

![clip_image002[4]](authenticating-users-with-forms-authentication-vb/_static/image1.jpg)

Nello strumento Amministrazione sito Web, è possibile creare nuovi utenti e ruoli selezionando la scheda Protezione. Fare clic sul collegamento **Crea utente** per creare un nuovo utente denominato Stefano (vedere la figura 2). Fornire all'utente Stephen la password desiderata, ad esempio *segreta.*

**Figura 2 – Creazione di un nuovo utente**

![clip_image004[4]](authenticating-users-with-forms-authentication-vb/_static/image2.jpg)

È possibile creare nuovi ruoli abilitando innanzitutto i ruoli e definendo uno o più ruoli. Abilitare i ruoli facendo clic sul collegamento **Abilita ruoli.** Successivamente, creare un ruolo denominato *Administrators* facendo clic sul collegamento **Crea o gestisci ruoli** (vedere la figura 3).

**Figura 3 – Creazione di un nuovo ruolo**

![clip_image006[4]](authenticating-users-with-forms-authentication-vb/_static/image3.jpg)

Infine, creare un nuovo utente denominato Sally e associare Sally al ruolo Administrators facendo clic sul collegamento Crea utente e selezionando Amministratori durante la creazione di Sally (vedere la figura 4).

**Figura 4 – Aggiunta di un utente a un ruoloFigure 4 – Adding a user to a role**

![clip_image008[4]](authenticating-users-with-forms-authentication-vb/_static/image4.jpg)

Quando tutto è detto e fatto, si dovrebbe avere due nuovi utenti di nome Stephen e Sally. È inoltre necessario disporre di un nuovo ruolo denominato Administrators.You should also have a new role named Administrators. Sally è un membro del ruolo Administrators e Stephen non lo è.

#### <a name="requiring-authorization"></a>Richiedere l'autorizzazione

È possibile richiedere l'autenticazione di un utente prima che l'utente richiami un'azione del controller aggiungendo l'attributo [Authorize] all'azione. È possibile applicare l'attributo [Authorize] a una singola azione del controller oppure a un'intera classe controller.

Ad esempio, il controller nel listato 1 espone un'azione denominata CompanySecrets(). Poiché questa azione è decorata con l'attributo [Authorize], questa azione non può essere richiamata a meno che un utente non sia autenticato.

**Listato 1 – Controller-HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample1.vb)]

Se si richiama l'azione CompanySecrets() immettendo l'URL /Home/CompanySecrets nella barra degli indirizzi del browser e non si è un utente autenticato, verrà reindirizzato automaticamente alla visualizzazione Account di accesso (vedere la Figura 5).

**Figura 5 – Visualizzazione Account di accesso**

![clip_image010[4]](authenticating-users-with-forms-authentication-vb/_static/image5.jpg)

È possibile utilizzare la vista Login per immettere il nome utente e la password. Se non si è un utente registrato, è possibile fare clic sul collegamento **di registrazione** per passare alla visualizzazione Register (vedere Figura 6). È possibile utilizzare la visualizzazione Registra per creare un nuovo account utente.

**Figura 6 – Visualizzazione Registro**

![clip_image012](authenticating-users-with-forms-authentication-vb/_static/image6.jpg)

Dopo aver eseguito correttamente l'accesso, è possibile visualizzare la visualizzazione CompanySecrets (vedere Figura 7). Per impostazione predefinita, si continuerà ad essere connessi fino a quando non si chiude la finestra del browser.

**Figura 7 – Visualizzazione CompanySecrets**

![clip_image014](authenticating-users-with-forms-authentication-vb/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Autorizzazione in base al nome utente o al ruolo utente

È possibile utilizzare l'attributo [Authorize] per limitare l'accesso a un'azione del controller a un determinato set di utenti o a un particolare set di ruoli utente. Ad esempio, il controller Home modificato nel listato 2 contiene due nuove azioni denominate StephenSecrets() e AdministratorSecrets().

**Listato 2 – Controller-HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample2.vb)]

Solo un utente con il nome utente Stephen può richiamare l'azione StephenSecrets(). Tutti gli altri utenti vengono reindirizzati alla visualizzazione di accesso. La proprietà Users accetta un elenco separato da virgole di nomi di account utente.

Solo gli utenti con il ruolo Administrators possono richiamare l'azione AdministratorSecrets(). Ad esempio, poiché Sally è un membro del gruppo Administrators, può richiamare l'azione AdministratorSecrets(). Tutti gli altri utenti vengono reindirizzati alla visualizzazione di accesso. Il Roles proprietà accetta un elenco separato da virgole di nomi di ruolo.

#### <a name="configuring-authentication"></a>Configurazione dell'autenticazione

A questo punto, ci si potrebbe chiedere dove vengono archiviate le informazioni sull'account utente e sul ruolo. Per impostazione predefinita, le informazioni vengono archiviate in un database SQL Express (RANU)\_denominato ASPNETDB.mdf che si trova nella cartella Dati applicazioni dell'applicazione MVC. Questo database viene generato automaticamente dal framework di ASP.NET quando si inizia a usare l'appartenenza.

Per visualizzare il database ASPNETDB.mdf nella finestra Esplora soluzioni, è innanzitutto necessario selezionare l'opzione di menu Progetto, Mostra tutti i file.

L'utilizzo del database SQL Express predefinito è corretta quando si sviluppa un'applicazione. Molto probabilmente, tuttavia, non si desidera utilizzare il database ASPNETDB.mdf predefinito per un'applicazione di produzione. In tal caso, è possibile modificare la posizione in cui vengono archiviate le informazioni sull'account utente completando i due passaggi seguenti:

1. Aggiungere gli oggetti di database di Application Services al database di produzione - Modificare la stringa di connessione dell'applicazione in modo che punti al database di produzioneAdd the Application Services database objects to your production database - Change your application connection string to point to your production database

Il primo passaggio consiste nell'aggiungere tutti gli oggetti di database necessari (tabelle e stored procedure) al database di produzione. Il modo più semplice per aggiungere questi oggetti a un nuovo database consiste nell'utilizzare il ASP.NET'Installazione guidata di SQL Server (vedere la Figura 8). È possibile avviare questo strumento aprendo il prompt dei comandi di Visual Studio 2008 dal gruppo di programmi di Microsoft Visual Studio 2008 ed eseguendo il comando seguente dal prompt dei comandi:

aspnet\_regsql

**Figura 8 – Installazione guidata di ASP.NET SQL Server**

![clip_image016](authenticating-users-with-forms-authentication-vb/_static/image8.jpg)

L'Installazione guidata di ASP.NET SQL ServerSQL Server consente di selezionare un database di SQL Server nella rete e di installare tutti gli oggetti di database richiesti dai servizi dell'applicazione ASP.NET. Non è necessario che il server di database si trovi nel computer locale.

> [!NOTE]
> Se non si desidera utilizzare l'Installazione guidata di ASP.NETSQL Server, è possibile trovare script SQL per l'aggiunta degli oggetti di database dei servizi dell'applicazione nella cartella seguente:
> 
> 
> C:.

Dopo aver creato gli oggetti di database necessari, è necessario modificare la connessione di database utilizzata dall'applicazione MVC. Modificare la stringa di connessione ApplicationServices nel file di configurazione Web (web.config) in modo che punti al database di produzione. Ad esempio, la connessione modificata nel listato 3 punta a un database denominato MyProductionDB (la stringa di connessione ApplicationServices originale è stata impostata come commento).

**Listato 3 – Web.config**

[!code-xml[Main](authenticating-users-with-forms-authentication-vb/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Configurazione delle autorizzazioni del database

Se si utilizza la protezione integrata per connettersi al database, sarà necessario aggiungere l'account utente di Windows corretto come account di accesso al database. L'account corretto dipende dal fatto che si utilizzi ASP.NET Development Server o Internet Information Services come server Web. L'account utente corretto dipende anche dal sistema operativo.

Se si utilizza il ASP.NET Development Server (il server Web predefinito utilizzato da Visual Studio), l'applicazione viene eseguita nel contesto dell'account utente di Windows. In tal caso, è necessario aggiungere l'account utente di Windows come account di accesso al server di database.

In alternativa, se si utilizza Internet Information Services, è necessario aggiungere l'account ASPNET o l'account NT AUTHORITY/NETWORK SERVICE come account di accesso al server di database. Se si utilizza Windows XP, aggiungere l'account ASPNET come account di accesso al database. Se si utilizza un sistema operativo più recente, ad esempio Windows Vista o Windows Server 2008, aggiungere l'account NT AUTHORITY/NETWORK SERVICE come account di accesso al database.

È possibile aggiungere un nuovo account utente al database utilizzando Microsoft SQL Server Management Studio (vedere la figura 9).

**Figura 9 – Creazione di un nuovo account di accesso di Microsoft SQL Server**

![clip_image018](authenticating-users-with-forms-authentication-vb/_static/image9.jpg)

Dopo aver creato l'account di accesso richiesto, è necessario eseguire il mapping dell'account di accesso a un utente del database con i ruoli del database corretti. Fare doppio clic sull'account di accesso e selezionare la scheda Mapping utenti. Selezionare uno o più ruoli del database dei servizi dell'applicazione. Ad esempio, per autenticare gli utenti, è\_\_necessario abilitare il ruolo del database aspnet Membership BasicAccess. Per creare nuovi utenti, è necessario abilitare il ruolo del database aspnet\_Membership\_FullAccess (vedere la Figura 10).

**Figura 10 - Aggiunta di ruoli del database di Application Services**

![clip_image020](authenticating-users-with-forms-authentication-vb/_static/image10.jpg)

#### <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come utilizzare l'autenticazione basata su form durante la compilazione di un'applicazione MVC ASP.NET. In primo luogo, è stato illustrato come creare nuovi utenti e ruoli sfruttando lo strumento Amministrazione sito Web. Successivamente, si è appreso come utilizzare l'attributo [Authorize] per impedire agli utenti non autorizzati di richiamare le azioni del controller. Infine, è stato illustrato come configurare l'applicazione MVC per archiviare le informazioni su utenti e ruoli in un database di produzione.

> [!div class="step-by-step"]
> [Successivo](preventing-javascript-injection-attacks-cs.md)
> [precedente](authenticating-users-with-windows-authentication-vb.md)
