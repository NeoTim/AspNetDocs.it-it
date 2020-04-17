---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Proteggere le applicazioni mediante l'autenticazione e l'autorizzazione Documenti Microsoft
author: rick-anderson
description: Passaggio 9 viene illustrato come aggiungere l'autenticazione e l'autorizzazione per proteggere l'applicazione NerdDinner, in modo che gli utenti devono registrarsi e accedere al sito per creare...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: d96f2763f6e62f9dd599fdb7977a97993d218305
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542573"
---
# <a name="secure-applications-using-authentication-and-authorization"></a>Proteggere le applicazioni tramite l'autenticazione e l'autorizzazione

da parte [di Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 9 di un'esercitazione gratuita [sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come compilare un'applicazione Web piccola ma completa usando ASP.NET MVC 1.
> 
> Passaggio 9 viene illustrato come aggiungere l'autenticazione e l'autorizzazione per proteggere l'applicazione NerdDinner, in modo che gli utenti devono registrarsi e accedere al sito per creare nuove cene e solo l'utente che ospita una cena può modificarla in un secondo momento.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [ALL'archivio musicale MVC.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-9-authentication-and-authorization"></a>Passaggio 9 di NerdDinner: autenticazione e autorizzazioneNerdDinner Step 9: Authentication and Authorization

In questo momento la nostra applicazione NerdDinner concede a chiunque visiti il sito la possibilità di creare e modificare i dettagli di qualsiasi cena. Cambiamo questo in modo che gli utenti devono registrarsi e accedere al sito per creare nuove cene e aggiungere una restrizione in modo che solo l'utente che ospita una cena può modificarlo in un secondo momento.

Per abilitare questa opzione utilizzeremo l'autenticazione e l'autorizzazione per proteggere l'applicazione.

### <a name="understanding-authentication-and-authorization"></a>Informazioni sull'autenticazione e l'autorizzazioneUnderstanding Authentication and Authorization

*L'autenticazione* è il processo di identificazione e convalida dell'identità di un client che accede a un'applicazione. In parole povere, si tratta di identificare "chi" l'utente finale è quando visitano un sito web. ASP.NET supporta più modi per autenticare gli utenti del browser. Per le applicazioni Web Internet, l'approccio di autenticazione più comune utilizzato è denominato "Autenticazione basata su form". L'autenticazione basata su form consente a uno sviluppatore di creare un modulo di accesso HTML all'interno dell'applicazione e quindi di convalidare il nome utente/password che un utente finale invia a un database o a un altro archivio credenziali password. Se la combinazione nome utente/password è corretta, lo sviluppatore può chiedere ASP.NET di emettere un cookie HTTP crittografato per identificare l'utente tra le richieste future. Utilizzeremo l'autenticazione basata su form con l'applicazione NerdDinner.We'll use forms authentication with our NerdDinner application.

*L'autorizzazione* è il processo che consente di determinare se un utente autenticato dispone dell'autorizzazione per accedere a un URL/risorsa specifico o per eseguire un'azione. Ad esempio, all'interno dell'applicazione NerdDinner è consigliabile autorizzare che solo gli utenti che hanno effettuato l'accesso possono accedere al */Dinners/Create* URL e creare nuove cene. Vogliamo anche aggiungere la logica di autorizzazione in modo che solo l'utente che ospita una cena possa modificarla e negare l'accesso in modifica a tutti gli altri utenti.

### <a name="forms-authentication-and-the-accountcontroller"></a>L'autenticazione basata su form e AccountController

Il modello di progetto di Visual Studio predefinito per ASP.NET MVC abilita automaticamente l'autenticazione basata su form quando vengono create nuove applicazioni MVC ASP.NET. Aggiunge automaticamente un'implementazione pre-compilata della pagina di accesso dell'account al progetto, il che rende davvero facile integrare la sicurezza all'interno di un sito.

La pagina master predefinita Site.master visualizza un collegamento "Accedi" in alto a destra del sito quando l'utente che vi accede non è autenticato:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Facendo clic sul collegamento "Accedi" viene utilizzato un utente all'URL */Account/LogOn:*

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

I visitatori che non si sono registrati possono farlo cliccando sul link "Registra" – che li porterà all'URL */Account/Registra* e permetterà loro di inserire i dettagli dell'account:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Facendo clic sul pulsante "Registra" verrà creato un nuovo utente all'interno del sistema di appartenenza ASP.NET e l'autenticazione dell'utente nel sito utilizzando l'autenticazione basata su form.

Quando un utente è connesso, il file Site.master modifica la parte superiore destra della pagina per restituire un "Benvenuto [nome utente]!" messaggio ed esegue il rendering di un collegamento "Disconnetti" anziché "Accedi". Facendo clic sul collegamento "Disconnetti" si disconnette l'utente:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

Le funzionalità di accesso, disconnessione e registrazione precedenti vengono implementate all'interno della classe AccountController aggiunta al progetto da Visual Studio al momento della creazione del progetto. L'interfaccia utente per AccountController viene implementata utilizzando i modelli di visualizzazione all'interno della directory di account:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

La classe AccountController utilizza il sistema di autenticazione basata su form ASP.NET per emettere cookie di autenticazione crittografati e l'API di appartenenza ASP.NET per archiviare e convalidare i nomi utente e le password. L'API di appartenenza ASP.NET è estensibile e consente di utilizzare qualsiasi archivio credenziali password. ASP.NET viene fornito con implementazioni di provider di appartenenze predefinite che archiviano nome utente/password all'interno di un database SQL o in Active Directory.

È possibile configurare il provider di appartenenze che l'applicazione NerdDinner deve utilizzare aprendo &lt;&gt; il file "web.config" nella radice del progetto e cercando la sezione di appartenenza all'interno di esso. Il file web.config predefinito aggiunto al momento della creazione del progetto registra il provider di appartenenze SQL e lo configura per l'utilizzo di una stringa di connessione denominata "ApplicationServices" per specificare il percorso del database.

La stringa di connessione predefinita "ApplicationServices" &lt;(specificata all'interno della sezione connectionStrings&gt; del file web.config) è configurata per l'utilizzo di SQL Express. Fa riferimento a un database SQL Express denominato "ASPNETDB. "MDF" nella directory "App\_Data" dell'applicazione. Se questo database non esiste la prima volta che l'API di appartenenza viene utilizzata all'interno dell'applicazione, ASP.NET creerà automaticamente il database ed eseguirà il provisioning dello schema di database delle appartenenze appropriato al suo interno:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Se invece di utilizzare SQL Express si desidera utilizzare un'istanza completa di SQL Server (o connettersi a un database remoto), tutto ciò che è necessario fare è aggiornare la stringa di connessione "ApplicationServices" all'interno del file web.config e assicurarsi che lo schema di appartenenza appropriato è stato aggiunto al database a cui punta. È possibile eseguire l'utilità "aspnet\_regsql.exe" all'interno della directory di Windows .NET Framework v2.0.50727 per aggiungere lo schema appropriato per l'appartenenza e gli altri ASP.NET servizi dell'applicazione a un database.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autorizzazione dell'URL /Dinners/Create mediante il filtro [Authorize]

Non è stato necessario scrivere codice per abilitare un'implementazione di autenticazione sicura e gestione account per l'applicazione NerdDinner.We didn't have to write any code to enable a secure authentication and account management implementation for the NerdDinner application. Gli utenti possono registrare nuovi account con la nostra applicazione e accedere/disconnettersi dal sito.

Ora possiamo aggiungere la logica di autorizzazione all'applicazione e usare lo stato di autenticazione e il nome utente dei visitatori per controllare cosa possono e non possono fare all'interno del sito. Iniziamo aggiungendo la logica di autorizzazione ai metodi di azione "Crea" della nostra classe DinnersController. In particolare, è necessario che gli utenti che accedono all'URL */Dinners/Create* devono essere connessi. Se non sono connessi, li reindirizzeremo alla pagina di accesso in modo che possano accedere.

L'implementazione di questa logica è piuttosto semplice. Tutto ciò che dobbiamo fare è aggiungere un attributo di filtro [Authorize] ai nostri metodi di azione Create in questo modo:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC supporta la possibilità di creare "filtri di azione" che possono essere utilizzati per implementare la logica riutilizzabile che può essere applicata in modo dichiarativo ai metodi di azione. Il filtro [Authorize] è uno dei filtri azione incorporati forniti da mvcASP.NET e consente a uno sviluppatore di applicare in modo dichiarativo le regole di autorizzazione ai metodi di azione e alle classi controller.

Se applicato senza parametri (come sopra) il filtro [Authorize] impone all'utente che effettua la richiesta del metodo di azione di effettuare l'accesso e il browser verrà reindirizzato automaticamente all'URL di accesso, se non lo è. Quando si esegue questo reindirizzamento, l'URL richiesto in origine viene passato come argomento querystring (ad esempio: /Account/LogOn? UrlRestituito: %2fDinners%2fCreate). AccountController reindirizzerà quindi l'utente all'URL richiesto in origine dopo l'accesso.

Il filtro [Autorizza] supporta facoltativamente la possibilità di specificare una proprietà "Users" o "Roles" che può essere utilizzata per richiedere che l'utente sia connesso e all'interno di un elenco di utenti consentiti o di un membro di un ruolo di sicurezza consentito. Ad esempio, il codice seguente consente solo a due utenti specifici, "scottgu" e "billg", di accedere all'URL /Dinners/Create:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Incorporare nomi utente specifici all'interno del codice tende ad essere piuttosto non gestibile però. Un approccio migliore consiste nel definire "ruoli" di livello superiore rispetto ai quali il codice esegue il controllo e quindi eseguire il mapping degli utenti al ruolo utilizzando un database o un sistema di Active Directory (consentendo l'archiviazione esterna dell'elenco di mapping utenti effettivo dal codice). ASP.NET include un'API di gestione dei ruoli incorporata e un set incorporato di provider di ruoli (inclusi quelli per SQL e Active Directory) che consentono di eseguire questo mapping utente/ruolo. È quindi possibile aggiornare il codice per consentire solo agli utenti all'interno di un ruolo "admin" specifico di accedere all'URL /Dinners/Create:We could then update the code to only allow users within a specific "admin" role to access the /Dinners/Create URL:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Utilizzo della proprietà User.Identity.Name durante la creazione di cene

È possibile recuperare il nome utente dell'utente attualmente connesso di una richiesta utilizzando la proprietà User.Identity.Name esposta nella classe di base Controller.

In precedenza, quando abbiamo implementato la versione HTTP-POST del metodo di azione Create(), eravamo stati codificati la proprietà "HostedBy" della Dinner in una stringa statica. È ora possibile aggiornare questo codice per utilizzare invece la proprietà User.Identity.Name, nonché aggiungere automaticamente un RSVP per l'host che crea la cena:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Poiché è stato aggiunto un attributo [Authorize] al metodo Create(), ASP.NET MVC garantisce che il metodo di azione venga eseguito solo se l'utente che visita l'URL /Dinners/Create è connesso al sito. Di conseguenza, il valore della proprietà User.Identity.Name conterrà sempre un nome utente valido.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Utilizzo della proprietà User.Identity.Name durante la modifica delle cene

Aggiungiamo ora una logica di autorizzazione che limita gli utenti in modo che possano modificare solo le proprietà delle cene che essi stessi ospitano.

Per risolvere questo problema, verrà innanzitutto aggiunto un metodo helper "IsHostedBy(username)" all'oggetto Dinner (all'interno della classe Dinner.cs parziale che abbiamo creato in precedenza). Questo metodo helper restituisce true o false a seconda che un nome utente fornito corrisponda alla proprietà Dinner HostedBy e incapsula la logica necessaria per eseguire un confronto tra stringhe senza distinzione tra maiuscole e minuscole:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Aggiungeremo quindi un attributo [Authorize] ai metodi di azione Edit() all'interno della classe DinnersController. In questo modo gli utenti devono essere connessi per richiedere un URL */Dinners/Edit/[id].*

È quindi possibile aggiungere codice ai metodi Edit che usa il metodo helper Dinner.IsHostedBy(username) per verificare che l'utente connesso corrisponda all'host Dinner. Se l'utente non è l'host, verrà visualizzata una visualizzazione "InvalidOwner" e verrà terminata la richiesta. Il codice per eseguire questa operazione è simile al seguente:The code to do this looks like below:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

È quindi possibile fare clic con il pulsante destro&gt;del mouse sulla directory "Views" e scegliere il comando di menu Aggiungi- Visualizza per creare una nuova vista "InvalidOwner". Lo popoleremo con il seguente messaggio di errore:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

E ora quando un utente tenta di modificare una cena di cui non è proprietario, otterrà un messaggio di errore:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Possiamo ripetere gli stessi passaggi per i metodi di azione Delete() all'interno del nostro controller per bloccare l'autorizzazione per eliminare anche Le cene e garantire che solo l'host di una cena possa eliminarla.

### <a name="showinghiding-edit-and-delete-links"></a>Mostrare/nascondere i collegamenti Modifica ed Elimina collegamenti

Stiamo collegando il metodo di azione Edit e Delete della nostra classe DinnersController dall'URL dei dettagli:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Attualmente stiamo mostrando i link di azione Modifica ed Elimina indipendentemente dal fatto che il visitatore all'URL dei dettagli sia l'host della cena. Cambiamo questo in modo che i link vengono visualizzati solo se l'utente in visita è il proprietario della cena.

Il metodo di azione Details() all'interno di DinnersController recupera un oggetto Dinner e quindi lo passa come oggetto modello al modello di visualizzazione:The Details() action method within our DinnerController retrieves a Dinner object and then passes it as the model object to our view template:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

È possibile aggiornare il modello di visualizzazione per mostrare/nascondere in modo condizionale i collegamenti Modifica ed Elimina utilizzando il metodo helper Dinner.IsHostedBy() come indicato di seguito:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Passaggi successivi

Esaminiamo ora come è possibile abilitare gli utenti autenticati a RSVP per le cene utilizzando AJAX.

> [!div class="step-by-step"]
> [Successivo](implement-efficient-data-paging.md)
> [precedente](use-ajax-to-deliver-dynamic-updates.md)
