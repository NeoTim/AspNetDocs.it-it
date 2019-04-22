---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
title: Una panoramica dell'autenticazione basata su form (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà convertito dalla discussione semplice all'implementazione; in particolare, si esaminerà l'implementazione di autenticazione basata su form. W l'applicazione web...
ms.author: riande
ms.date: 01/14/2008
ms.assetid: 83267f7d-64d9-41ee-82cf-da91b1bf534d
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 84b1c4c562603eddc5b82500700957bc78f236f4
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59386451"
---
# <a name="an-overview-of-forms-authentication-vb"></a>Una panoramica dell'autenticazione basata su form (VB)

da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_vb.pdf)

> In questa esercitazione verrà convertito dalla discussione semplice all'implementazione; in particolare, si esaminerà l'implementazione di autenticazione basata su form. L'applicazione web che viene avviata la costruzione di questa esercitazione continueranno a essere basati su nelle esercitazioni successive, parliamo di autenticazione basata su form semplice ai ruoli e appartenenza.
> 
> Vedere questo video per altre informazioni su questo argomento: [Utilizza Basic autenticazione basata su form in ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md).


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](security-basics-and-asp-net-support-vb.md) sono illustrate le opzioni di un account utente, l'autorizzazione e autenticazione vari fornite da ASP.NET. In questa esercitazione verrà convertito dalla discussione semplice all'implementazione; in particolare, si esaminerà l'implementazione di autenticazione basata su form. L'applicazione web che viene avviata la costruzione di questa esercitazione continueranno a essere basati su nelle esercitazioni successive, parliamo di autenticazione basata su form semplice ai ruoli e appartenenza.

Questa esercitazione inizia con uno sguardo approfondito sui flussi di lavoro di autenticazione form, un argomento al momento illustrate nell'esercitazione precedente. In seguito, si creerà un sito Web ASP.NET tramite la quale i concetti di autenticazione basata su form della demo. Successivamente, verrà configurato il sito per usare l'autenticazione basata su form, creare una pagina di accesso semplice e informazioni su come determinare, nel codice, se un utente viene autenticato e, in questo caso, il nome utente l'accesso.

Comprendere i form, flusso di lavoro di autenticazione, abilitarlo in un'applicazione web e la creazione di pagine di account di accesso e disconnessione sono i passaggi di tutto fondamentali nella creazione di un'applicazione ASP.NET che supporta gli account utente e autentica gli utenti tramite una pagina web. -Per questo motivo e poiché queste esercitazioni si basano tra loro, è consigliabile usare questa esercitazione completamente prima di procedere a quello successivo, anche se si ha già esperienza di configurazione autenticazione basata su form in progetti precedenti.

## <a name="understanding-the-forms-authentication-workflow"></a>Comprendere il flusso di lavoro di autenticazione form

Quando il runtime ASP.NET elabora una richiesta per una risorsa ASP.NET, ad esempio una pagina ASP.NET o servizio Web ASP.NET, la richiesta genera un numero di eventi durante il ciclo di vita. Sono disponibili gli eventi generati alla fine molto principianti e molto della richiesta, quelli generati quando la richiesta viene autenticata e autorizzato, un evento generato nel caso di un'eccezione non gestita e così via. Per visualizzare un elenco completo degli eventi, vedere la [eventi dell'oggetto HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

*Moduli HTTP* sono le classi gestite il cui codice viene eseguito in risposta a un particolare evento del ciclo di vita di richiesta. ASP.NET viene fornito con un numero di moduli HTTP che eseguono le attività essenziali dietro le quinte. Sono due i moduli HTTP predefiniti che sono particolarmente rilevanti per la discussione:

- **[FormsAuthenticationModule](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)**  -autentica l'utente controllando il ticket di autenticazione form, in genere inclusa nella raccolta di cookie dell'utente. Se non è presente nessun ticket di autenticazione form, l'utente è anonimo.
- **[UrlAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)**  -determina se l'utente corrente è autorizzato ad accedere all'URL richiesto. Questo modulo determina l'autorità consultando le regole di autorizzazione specificate nel file di configurazione dell'applicazione. ASP.NET include anche il [FileAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) che determina l'autorità consultando i file richiesti gli ACL.

FormsAuthenticationModule tenta di autenticare l'utente prima di UrlAuthorizationModule (e FileAuthorizationModule) in esecuzione. Se l'utente che effettua la richiesta non è autorizzato ad accedere alla risorsa richiesta, il modulo di autorizzazione termina la richiesta e restituisce un [HTTP 401 non autorizzato](http://www.checkupdown.com/status/E401.html) dello stato. In scenari di autenticazione di Windows, lo stato HTTP 401 viene restituito al browser. Questo codice di stato fa sì che il browser richiedere all'utente le credenziali tramite una finestra di dialogo modale. Con autenticazione basata su form, tuttavia, lo stato HTTP 401 non autorizzato non viene mai inviato al browser perché FormsAuthenticationModule rileva questo stato e lo modifica in modo da reindirizzare l'utente alla pagina di accesso invece (tramite un [HTTP 302 reindirizzare](http://www.checkupdown.com/status/E302.html) lo stato).

La responsabilità della pagina di accesso consiste nel determinare se le credenziali dell'utente sono valide e, in questo caso, per creare un ticket di autenticazione form e reindirizzerà l'utente torna alla pagina stava tentando di visitare. Il ticket di autenticazione è incluso nelle richieste successive per le pagine nel sito Web, che FormsAuthenticationModule utilizza per identificare l'utente.


[![Il flusso di lavoro di autenticazione form](an-overview-of-forms-authentication-vb/_static/image2.png)](an-overview-of-forms-authentication-vb/_static/image1.png)

**Figura 01**: Il flusso di lavoro di autenticazione form ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image3.png))


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Ricordare il Ticket di autenticazione tra visite di pagina

Dopo l'accesso, il ticket di autenticazione form deve essere inviato al server web per ogni richiesta in modo che l'utente rimane connesso quando esplorano il sito. Questa operazione viene in genere eseguita inserendo il ticket di autenticazione nella raccolta di cookie dell'utente. [I cookie](http://en.wikipedia.org/wiki/HTTP_cookie) sono piccoli file di testo che si trovano sul computer dell'utente e vengono trasmessi nelle intestazioni HTTP per ogni richiesta per il sito Web che ha creato il cookie. Di conseguenza, dopo aver creato ed memorizzato nei cookie del browser il ticket di autenticazione form, ogni visita successivi a tale sito invia il ticket di autenticazione insieme alla richiesta, in tal modo che identifica l'utente.

> [!NOTE]
> L'applicazione web demo usata in ogni esercitazione è disponibile come download. Questa applicazione scaricabile è stata creata con Visual Web Developer 2008 destinato a .NET Framework versione 3.5. Poiché l'applicazione è destinata a .NET 3.5, il relativo file Web. config include gli elementi di configurazione aggiuntive, specifiche 3.5. In breve, se hai ancora a installare .NET 3.5 nel computer, l'applicazione scaricabile dal web non funzionerà senza prima rimuovere il markup specifico della 3.5 da Web. config.


Un aspetto dei cookie è la scadenza, ovvero la data e ora in cui il browser Elimina il cookie. Quando il cookie di autenticazione form scade, l'utente può non è più essere autenticato e diventano pertanto anonimo. Quando si visita a un utente da un terminale pubblico, probabile che si desidera che i ticket di autenticazione scada quando si chiude il browser. Quando si visita da casa, tuttavia, lo stesso utente potrebbe essere necessario il ticket di autenticazione vengano memorizzate tra i riavvii del browser in modo che non hanno per ripetere l'accesso ogni volta che visitano il sito. Spesso, questa decisione viene presa dall'utente sotto forma di un controllo checkbox memorizza dati nella pagina di accesso. Nel passaggio 3 esamineremo come implementare un controllo checkbox memorizza dati nella pagina di accesso. L'esercitazione seguente illustra le impostazioni di timeout ticket di autenticazione in modo dettagliato.

> [!NOTE]
> È possibile che l'agente utente utilizzato per accedere al sito Web potrebbe non supportare i cookie. In tal caso, ASP.NET può utilizzare i ticket di autenticazione form senza cookie. In questa modalità, il ticket di autenticazione è codificato nell'URL. Si esaminerà quando vengono utilizzati i ticket di autenticazione senza cookie e modo in cui vengono creati e gestiti nella prossima esercitazione.


### <a name="the-scope-of-forms-authentication"></a>L'ambito dell'autenticazione basata su form

FormsAuthenticationModule viene gestita il codice che rappresenta parte del runtime di ASP.NET. Prima della versione 7 di Microsoft [Internet Information Services (IIS)](https://www.iis.net/) server web, si è verificato un ostacolo distinto tra pipeline HTTP dell'IIS e la pipeline di ASP.NET runtime. In breve, in IIS 6 e versioni precedenti, FormsAuthenticationModule viene eseguito solo quando una richiesta viene delegata da IIS per il runtime di ASP.NET. Per impostazione predefinita, IIS elabora contenuto statico, come pagine HTML e CSS e file di immagine - e trasferisce le richieste solo al runtime ASP.NET quando viene richiesta una pagina con estensione aspx, asmx e. ashx.

Le pipeline di IIS 7, tuttavia, consente di integrata di IIS e ASP.NET. Con alcune impostazioni di configurazione è possibile configurare IIS 7 per FormsAuthenticationModule per richiamare *tutti* richieste. Con IIS 7, inoltre, è possibile definire regole di autorizzazione URL per i file di qualsiasi tipo. Per altre informazioni, vedere [IIS7 sicurezza e le modifiche tra IIS6](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [sicurezza della piattaforma Web Your](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements), e [Understanding IIS7 URL autorizzazione](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

In breve, nelle versioni precedenti a IIS 7, è possibile utilizzare solo autenticazione basata su form per proteggere le risorse gestite dal runtime di ASP.NET. Analogamente, le regole di autorizzazione URL vengono applicate solo alle risorse gestite dal runtime di ASP.NET. Ma con IIS 7 è possibile integrare il FormsAuthenticationModule e UrlAuthorizationModule nelle pipeline HTTP dell'IIS, estendendo così questa funzionalità a tutte le richieste.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Passaggio 1: Creazione di un sito Web ASP.NET per questa serie di esercitazioni

Per raggiungere un vasto pubblico, con la versione gratuita di Microsoft di Visual Studio 2008, verrà creato il sito Web ASP.NET da compilare in questa serie [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Verrà implementato l'archivio utente SqlMembershipProvider in una [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) database. Se si usa Visual Studio 2005 o un'edizione diversa di Visual Studio 2008 o SQL Server, non preoccuparti: i passaggi sono quasi identici e le eventuali differenze considerevole verranno indicate.

Prima di poter configurare autenticazione basata su form, è innanzitutto necessario un sito Web ASP.NET. Iniziare creando un nuovo file in base al sistema sito Web ASP.NET. A tale scopo, avviare Visual Web Developer e quindi passare al menu File e scegliere Nuovo sito Web, la finestra di dialogo Nuovo sito Web. Scegliere il modello di sito Web ASP.NET, impostare l'elenco di riepilogo a discesa percorso al File System, scegliere una cartella in cui inserire il sito web e impostare la lingua di Visual Basic. Si creerà un nuovo sito web con una pagina ASP.NET default. aspx, un'App\_cartella di dati e un file Web. config.

> [!NOTE]
> Visual Studio supporta due modalità di gestione del progetto: Progetti di siti Web e progetti di applicazione Web. Progetti di siti Web non dispongono di un file di progetto, mentre i progetti applicazione Web imitare l'architettura di progetto in Visual Studio .NET 2002/2003: includono un file di progetto e compilare codice sorgente del progetto in un singolo assembly, il quale viene inserito nella cartella /bin. Visual Studio 2005 inizialmente solo supportati progetti di sito Web, anche se è stato reintrodotto il modello di progetto di applicazione Web con Service Pack 1. Visual Studio 2008 offre entrambi i modelli di progetto. Visual Web Developer 2005 e 2008 edizioni, tuttavia, supportano solo progetti di siti Web. Userà il modello di progetto di sito Web. Se si utilizza un'edizione non Express e si vuole usare il [modello di progetto di applicazione Web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) invece, è possibile eseguire questa operazione, ma tenere presente che potrebbero esserci alcune discrepanze tra ciò che viene visualizzato nella schermata e la procedura è necessario eseguire e il schermate visualizzate e le istruzioni fornite in queste esercitazioni.


[![Creare un nuovo sito di Web File basate sul sistema](an-overview-of-forms-authentication-vb/_static/image5.png)](an-overview-of-forms-authentication-vb/_static/image4.png)

**Figura 02**: Creare un sito Web New File System-Based ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image6.png))


### <a name="adding-a-master-page"></a>Aggiunta di una pagina Master

Successivamente, aggiungere una nuova pagina Master al sito nella directory radice denominato Site. master. [Pagine master](https://msdn.microsoft.com/library/wtxbf3hh.aspx) consentono allo sviluppatore di pagina definire un modello a livello di sito che può essere applicato alle pagine ASP.NET. Il vantaggio principale delle pagine master è che l'aspetto generale del sito può essere definito in un'unica posizione, rendendo in tal modo più semplice aggiornare o modificare il layout del sito.


[![Aggiungere una pagina Master denominato Site. master al sito Web](an-overview-of-forms-authentication-vb/_static/image8.png)](an-overview-of-forms-authentication-vb/_static/image7.png)

**Figura 03**: Aggiungere un site. Master pagina denominato master al sito Web ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image9.png))


Definire il layout delle pagine nell'intero sito nella pagina master. È possibile usare la visualizzazione di progettazione e aggiungere i controlli Web o Layout è necessario oppure è possibile aggiungere manualmente il markup manualmente nella vista origine. Strutturato layout della pagina master per riprodurre il layout utilizzato nella mio *[uso dei dati in ASP.NET 2.0](../../data-access/index.md)* serie di esercitazioni (vedere la figura 4). Nella pagina master viene utilizzata [fogli di stile CSS](http://www.w3schools.com/css/default.asp) per il posizionamento e gli stili con le impostazioni CSS definite nel file Style. CSS (che è incluso nel download associato dell'esercitazione). Sebbene non sia evidente dai tag riportati di seguito, le regole CSS sono definite in modo che la navigazione &lt;div&gt;del contenuto sia sempre posizionato in modo che viene visualizzata a sinistra e abbia una larghezza fissa di 200 pixel.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample1.aspx)]

Una pagina master definisce sia il layout delle pagine statico sia le aree che possono essere modificate tramite le pagine ASP.NET che utilizzano la pagina master. Queste aree con contenuto modificabile sono indicate dal controllo ContentPlaceHolder, che può essere visualizzato all'interno del contenuto &lt;div&gt;. Questa pagina master contiene un solo ContentPlaceHolder (MainContent), ma della pagina master può contenere diversi ContentPlaceHolder.

Con i tag inseriti precedentemente, passando alla visualizzazione progettazione, viene illustrato il layout della pagina master. Tutte le pagine ASP.NET che utilizzano tale pagina master avrà questo layout uniforme, con la possibilità di specificare il markup per l'area MainContent.


[![Nella pagina Master visualizzata mediante la visualizzazione di progettazione](an-overview-of-forms-authentication-vb/_static/image11.png)](an-overview-of-forms-authentication-vb/_static/image10.png)

**Figura 04**: la pagina Master, quando visualizzati tramite la visualizzazione di progettazione ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image12.png))


### <a name="creating-content-pages"></a>Creazione di pagine di contenuto

A questo punto si dispone di una pagina default. aspx nel nostro sito Web, ma non usa la pagina master che appena creato. Sebbene sia possibile modificare il markup dichiarativo di una pagina web per usare una pagina master, se la pagina non contiene alcun contenuto ancora risulta più semplice è sufficiente eliminare la pagina e aggiungerlo nuovamente al progetto, che specifica la pagina master da usare. Pertanto, per iniziare, l'eliminazione di default. aspx dal progetto.

Successivamente, fare doppio clic sul nome del progetto in Esplora soluzioni e scegliere di aggiungere un nuovo modulo Web denominato default. aspx. Questa volta, selezionare la casella di controllo Seleziona pagina master e scegliere la pagina master Site. master dall'elenco.


[![Aggiungere una nuova pagina default. aspx scelta selezionare una pagina Master](an-overview-of-forms-authentication-vb/_static/image14.png)](an-overview-of-forms-authentication-vb/_static/image13.png)

**Figura 05**: Aggiungere un nuovo default. aspx pagina scelta per selezionare una pagina Master ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image15.png))


[![Utilizzare la pagina Master Site. master](an-overview-of-forms-authentication-vb/_static/image17.png)](an-overview-of-forms-authentication-vb/_static/image16.png)

**Figura 06**: Utilizzare la pagina Master Site. master ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image18.png))


> [!NOTE]
> Se si usa il modello di progetto applicazione Web di finestra di dialogo Aggiungi nuovo elemento non include una casella di controllo Seleziona pagina master. In alternativa, è necessario aggiungere un elemento del tipo di Form di contenuto Web. Dopo la scelta dell'opzione di Form di contenuto Web e facendo clic su Aggiungi, Visual Studio verrà visualizzata l'istruzione Select stesso un Master finestra di dialogo illustrata nella figura 6.


Markup dichiarativo della nuova pagina default. aspx include solo un @Page direttiva che specifica il percorso al master pagina file e un controllo contenuto MainContent ContentPlaceHolder della pagina master.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample2.aspx)]

Per il momento, lasciare vuoto default. aspx. Si tornerà a, più avanti in questa esercitazione per aggiungere contenuto.

> [!NOTE]
> Questa pagina master include una sezione di un menu o altre interfacce navigazione. Si creerà un'interfaccia di questo tipo in un'esercitazione futura.


## <a name="step-2-enabling-forms-authentication"></a>Passaggio 2: Abilitare l'autenticazione basata su form

Con il sito Web ASP.NET creato, l'attività successiva consiste nell'abilitare l'autenticazione basata su form. Configurazione dell'autenticazione dell'applicazione viene specificata tramite il [ &lt;authentication&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx) in Web. config. Il &lt;autenticazione&gt; elemento contiene un singolo attributo denominato modalità che specifica il modello di autenticazione usato dall'applicazione. Questo attributo può avere uno dei quattro valori seguenti:

- **Windows** : come illustrato nell'esercitazione precedente, quando un'applicazione utilizza l'autenticazione di Windows è responsabilità del server web per autenticare il visitatore e questa operazione viene in genere eseguita tramite Basic, Digest o integrata di Windows autenticazione.
- **Form**-gli utenti vengono autenticati tramite un modulo in una pagina web.
- **Passport**-gli utenti vengono autenticati usando Microsoft Passport Network.
- **Nessuno**-non viene usato alcun modello di autenticazione, tutti i visitatori sono anonimi.

Per impostazione predefinita, le applicazioni ASP.NET usano l'autenticazione di Windows. Per modificare il tipo di autenticazione per l'autenticazione basata su form, quindi, è necessario modificare il &lt;autenticazione&gt; attributo modalità dell'elemento a un form.

Se il progetto non contiene ancora un file Web. config, aggiungere uno ora facendo clic sul nome del progetto in Esplora soluzioni, scegliendo Aggiungi nuovo elemento e quindi aggiungere un file di configurazione Web.


[![Se il progetto non contiene ancora file Web. config, aggiungerlo ora](an-overview-of-forms-authentication-vb/_static/image20.png)](an-overview-of-forms-authentication-vb/_static/image19.png)

**Figura 07**: Se il progetto viene non ancora includere file Web. config, aggiungere subito ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image21.png))


Successivamente, individuare il &lt;autenticazione&gt; elemento e aggiornamento, è possibile utilizzare l'autenticazione di form. Dopo questa modifica, markup del file Web. config dovrebbe essere simile al seguente:

[!code-xml[Main](an-overview-of-forms-authentication-vb/samples/sample3.xml)]

> [!NOTE]
> Poiché Web. config è un file XML, maiuscole/minuscole sono importante. Assicurarsi di impostare l'attributo mode di form, con una lettera maiuscola F. Se si usano una maiuscole e minuscole diverse, ad esempio moduli, si riceverà un errore di configurazione quando si visita il sito tramite un browser.


Il &lt;authentication&gt; elemento può includere facoltativamente un &lt;forms&gt; elemento figlio che contiene impostazioni specifiche dell'autenticazione form. Per ora, è possibile semplicemente usare le impostazioni di autenticazione form predefinito. Verrà presa in esame il &lt;form&gt; elemento figlio in modo più dettagliato nella prossima esercitazione.

## <a name="step-3-building-the-login-page"></a>Passaggio 3: Creazione della pagina di accesso

Per supportare l'autenticazione basata su form il nostro sito Web richiede una pagina di accesso. Come illustrato nel comprendere la sezione del flusso di lavoro autenticazione form, FormsAuthenticationModule verrà reindirizzata automaticamente l'utente alla pagina di accesso se tentano di accedere a una pagina che non sono autorizzati a visualizzare. Sono inoltre presenti controlli Web ASP.NET che verranno visualizzato un collegamento alla pagina di accesso agli utenti anonimi. Questo discorso fa sorgere una domanda, qual è l'URL della pagina di accesso?

Per impostazione predefinita, il sistema di autenticazione form si aspetta che la pagina di accesso sia denominato Login. aspx e inserito nella directory radice dell'applicazione web. Se si desidera utilizzare un URL della pagina account di accesso diverso, è possibile farlo specificandola nel file Web. config. Si vedrà come eseguire questa operazione nell'esercitazione successiva.

La pagina di accesso ha tre responsabilità:

1. Fornire un'interfaccia che consente il visitatore di immettere le credenziali.
2. Determina se le credenziali inviate sono valide.
3. Accesso dell'utente tramite la creazione di moduli di ticket di autenticazione.

### <a name="creating-the-login-pages-user-interface"></a>Creazione dell'interfaccia utente di pagina di accesso

Attività iniziali con la prima attività. Aggiungere una nuova pagina ASP.NET alla directory radice del sito denominata Login. aspx e associarla alla pagina master Site. master.


[![Aggiungere una nuova pagina ASP.NET denominata Login.](an-overview-of-forms-authentication-vb/_static/image23.png)](an-overview-of-forms-authentication-vb/_static/image22.png)

**Figura 08**: Aggiungere un nuovo ASP.NET pagina denominata Login. aspx ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image24.png))


L'interfaccia della pagina account di accesso tipico è costituito da due caselle di testo - uno per il nome dell'utente, uno per la propria password - e un pulsante per inviare il modulo. Siti Web include spesso una memorizza dati per la casella di controllo che, se selezionata, il ticket di autenticazione risultante viene mantenuto tra i riavvii del browser.

Aggiungere due caselle di testo a Login. aspx e impostare le relative proprietà ID per nome utente e Password, rispettivamente. Impostare anche proprietà TextMode su della Password con Password. Successivamente, aggiungere una casella di controllo, impostare la relativa proprietà ID RememberMe e la relativa proprietà Text da me. tenere presente che In seguito, aggiungere un pulsante denominato LoginButton proprietà il cui testo è impostata su account di accesso. Infine, aggiungere un controllo Web di etichetta e impostarne la proprietà ID per InvalidCredentialsMessage, la relativa proprietà Text per il nome utente o password non è valida. Riprovare più tardi., la relativa proprietà colore di primo piano sul rosso e la relativa proprietà Visible su False.

A questo punto la schermata dovrebbe essere simile allo screenshot nella figura 9 e sintassi dichiarativa della pagina sarà simile al seguente:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample4.aspx)]


[![La pagina di accesso contiene due caselle di testo, una casella di controllo, un pulsante e un'etichetta](an-overview-of-forms-authentication-vb/_static/image26.png)](an-overview-of-forms-authentication-vb/_static/image25.png)

**Figura 09**: L'account di accesso di pagina contiene due caselle di testo, una casella di controllo, un pulsante e un'etichetta ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image27.png))


Infine, creare un gestore eventi per clic del LoginButton evento. Dalla finestra di progettazione, semplicemente fare doppio clic sul controllo Button per creare questo gestore dell'evento.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Determinare se le credenziali fornite non sono validi

È ora necessario implementare attività 2 clic del pulsante gestore eventi: determinare se le credenziali specificate sono valide. Per eseguire questa operazione deve essere un archivio utente che contiene tutte le credenziali degli utenti in modo che è possibile determinare se le credenziali fornite abbinare con credenziali note.

Prima di ASP.NET 2.0, gli sviluppatori erano responsabili dell'implementazione entrambi i propri gli archivi utente e la scrittura del codice per convalidare le credenziali fornite a fronte dell'archivio. La maggior parte degli sviluppatori implementerebbe l'archivio dell'utente in un database, creazione di una tabella denominata gli utenti con le colonne come nome utente, Password, indirizzo di posta elettronica, LastLoginDate e così via. Questa tabella, quindi, sarebbe necessario un record per ogni account utente. Verifica credenziali dell'utente fornito comporta una query sul database per un nome utente corrispondente e assicurarsi che la password nel database corrispondeva con la password fornita.

Con ASP.NET 2.0, gli sviluppatori devono usare uno dei provider di appartenenze per gestire l'archivio utente. In questa serie di esercitazioni si userà il provider SqlMembershipProvider, che usa un database di SQL Server per l'archivio dell'utente. Quando si usa il provider SqlMembershipProvider è necessario implementare uno schema di database specifico che include le tabelle, viste e stored procedure prevede dal provider. Esamineremo come implementare questo schema nel *[creazione dello Schema di appartenenza in SQL Server](../membership/creating-the-membership-schema-in-sql-server-vb.md)* esercitazione. Con il provider di appartenenze posto, la convalida delle credenziali dell'utente è semplice come chiamare il [classe di appartenenza](https://msdn.microsoft.com/library/system.web.security.membership.aspx)del [ValidateUser (*username*, *password*) metodo](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx), che restituisce un valore booleano che indica se la validità del *username* e *password* combinazione. Visualizzare come archivio dell'utente del SqlMembershipProvider non è stata ancora implementata, non utilizziamo l'appartenenza al metodo della classe ValidateUser in questo momento.

Anziché il tempo necessario per compilare una personalizzato utenti tabella di database (che dovrebbe essere obsoleta quando viene implementato il provider SqlMembershipProvider), è possibile invece come hardcoded le credenziali valide nell'account di accesso della pagina stessa. Nella finestra di LoginButton gestore dell'evento Click, aggiungere il codice seguente:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample5.vb)]

Come può notare, sono disponibili tre account utente validi - Scott Jisun e Sam - e tutte e tre hanno la stessa password (password). Il codice consente di scorrere le matrici di utenti e password per trovare una corrispondenza di nome utente e password valida. Se il nome utente e password sono validi, è necessario eseguire l'accesso utente e quindi li reindirizza alla pagina appropriata. Se le credenziali sono valide, quindi è visualizzare l'etichetta InvalidCredentialsMessage.

Quando un utente immette credenziali valide, ho accennato che vengono quindi reindirizzati alla pagina appropriata. Che cos'è la pagina appropriata, tuttavia? È importante ricordare che quando un utente visita una pagina che non sono autorizzati a visualizzare, FormsAuthenticationModule reindirizza automaticamente alla pagina di accesso. In questo modo, include l'URL richiesto nella stringa di query tramite il parametro ReturnUrl. Vale a dire, se un utente ha tentato di visitare ProtectedPage.aspx e non sono autorizzati a tale scopo, FormsAuthenticationModule reindirizzasse per:

Login.aspx?ReturnUrl=ProtectedPage.aspx

Dopo l'accesso correttamente, l'utente deve essere reindirizzato al ProtectedPage.aspx. In alternativa, gli utenti possono visitare la pagina di accesso nel proprio volition. In tal caso, dopo l'accesso dell'utente sono devono essere inviati alla pagina default. aspx della cartella radice.

### <a name="logging-in-the-user"></a>Registrazione dell'utente

Presupponendo che le credenziali specificate siano valide, è necessario creare un ticket di autenticazione form, in tal modo la registrazione dell'utente al sito. Il [classe FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) nel [dotare dello spazio dei nomi](https://msdn.microsoft.com/library/system.web.security.aspx) fornisce vengono formattati diversi metodi per la registrazione in ed effettuando l'accesso degli utenti tramite moduli di sistema di autenticazione. Anche se sono disponibili diversi metodi nella classe FormsAuthentication, i tre che siamo interessati a questo punto sono:

- [GetAuthCookie (*nomeutente*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) -crea un ticket di autenticazione form per il nome fornito *username*. Successivamente, questo metodo crea e restituisce un oggetto HttpCookie che contiene il contenuto del ticket di autenticazione. Se *persistCookie* è True, viene creato un cookie persistente.
- [SetAuthCookie (*nomeutente*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) -chiama il GetAuthCookie (*username*, *persistCookie*) metodo per generare il cookie di autenticazione form. Questo metodo aggiunge quindi il cookie restituito da GetAuthCookie all'insieme di cookie (presupponendo che l'autenticazione basata su cookie basata su form è stata utilizzata; in caso contrario, questo metodo chiama una classe interna che gestisce la logica senza cookie ticket).
- [RedirectFromLoginPage (*nomeutente*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) -questo metodo chiama SetAuthCookie (*username*, *persistCookie*) e quindi reindirizza l'utente alla pagina appropriata.

GetAuthCookie risulta utile quando è necessario modificare il ticket di autenticazione prima della scrittura del cookie per la raccolta di cookie. SetAuthCookie è utile se si desidera creare i form ticket di autenticazione e aggiungerlo all'insieme di cookie, ma non si desidera reindirizzare l'utente alla pagina appropriata. Forse si desidera mantenerli nella pagina di accesso o di inviarli a una pagina alternativa.

Poiché si vuole accedere l'utente e reindirizza alla pagina appropriata, utilizziamo RedirectFromLoginPage. Aggiornamento fare clic su di LoginButton gestore eventi, sostituendo le due righe TODO impostata come commentate con la riga di codice seguente:

FormsAuthentication.RedirectFromLoginPage(UserName.Text, RememberMe.Checked)

Durante la creazione di ticket di autenticazione form di usiamo il ticket di autenticazione form proprietà Text di UserName TextBox *nomeutente* parametro e lo stato selezionato di RememberMe CheckBox per il  *persistCookie* parametro.

Per testare la pagina di accesso, vederla in un browser. Per iniziare, immettere le credenziali non valide, ad esempio un nome utente di e la password errata. Facendo clic sul pulsante di accesso verrà eseguito un postback e verrà visualizzata l'etichetta InvalidCredentialsMessage.


[![L'etichetta InvalidCredentialsMessage è visualizzato quando immettendo credenziali non valide](an-overview-of-forms-authentication-vb/_static/image29.png)](an-overview-of-forms-authentication-vb/_static/image28.png)

**Figura 10**: L'etichetta InvalidCredentialsMessage è visualizzato quando immettendo credenziali non valide ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image30.png))


Successivamente, immettere le credenziali valide e fare clic sul pulsante di accesso. Questa volta si verifica il postback un ticket di autenticazione form viene creata e si verrà reindirizzati automaticamente a default. aspx. A questo punto è connesso al sito Web, anche se non esistono Nessun segnali visivi per indicare che si è attualmente connessi. Nel passaggio 4 che si vedrà come determinare a livello di programmazione se un utente viene registrato in o non, nonché come identificare l'utente visitando la pagina.

Passaggio 5 vengono esaminate le tecniche per la registrazione di un utente dal sito Web.

### <a name="securing-the-login-page"></a>Protezione di pagina di accesso

Quando l'utente immette le proprie credenziali e invia il modulo della pagina account di accesso, le credenziali - tra cui password - vengono trasmessi tramite Internet al server web nello *testo normale*. Ciò significa che qualsiasi utente malintenzionato sniffing del traffico di rete è possibile visualizzare il nome utente e password. Per evitare questo problema, è essenziale per crittografare il traffico di rete mediante [livelli SSL (Secure Socket)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Ciò garantisce che le credenziali (così come markup HTML della pagina intera) vengono crittografati dal momento in cui che gli utenti lasciano il browser fino a quando non vengono ricevuti dal server web.

A meno che il sito Web contiene informazioni riservate, dovrai solo usare SSL nella pagina di accesso e in altre pagine in cui la password dell'utente in caso contrario sarebbe essere inviata via cavo in testo normale. Non devi preoccuparti di moduli di protezione dei ticket di autenticazione perché, per impostazione predefinita, è sia crittografato e firmato digitalmente (per impedire eventuali manomissioni). Nell'esercitazione seguente viene presentata una discussione più approfondita sulla protezione di ticket di autenticazione form.

> [!NOTE]
> Molti siti Web finanziari o medici sono configurati per utilizzare SSL sul *tutti* pagine accessibili agli utenti autenticati. Se si sta creando un sito Web è possibile configurare il sistema di autenticazione form in modo che il ticket di autenticazione form verrà trasmessi solo tramite una connessione sicura. Si esaminerà le varie opzioni di configurazione di autenticazione form nell'esercitazione successiva  *[configurazione dell'autenticazione form e argomenti avanzati](../membership/creating-the-membership-schema-in-sql-server-vb.md)*.


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Passaggio 4: Rilevamento dei visitatori autenticati e determinando la propria identità

A questo punto abbiamo abilitato l'autenticazione basata su form e creato una pagina di accesso rudimentale, ma è ancora esaminare come è possibile determinare se un utente è autenticato o anonimo. In determinate situazioni potrà piacerebbe visualizzare dati diversi o informazioni a seconda del fatto che un utente autenticato o anonimo è sinonimo di visitare la pagina. Inoltre, è spesso necessario conoscere l'identità dell'utente autenticato.

È possibile aumentare la pagina default. aspx esistente per illustrare queste tecniche. In default. aspx aggiungere due controlli di pannello, uno denominato AuthenticatedMessagePanel e AnonymousMessagePanel denominata un altro. Aggiungere un controllo etichetta denominato WelcomeBackMessage nel primo pannello. Nel secondo pannello Aggiungi un controllo collegamento ipertestuale, impostarne la proprietà Text su accesso e la proprietà NavigateUrl a ~ / Login. aspx. A questo punto il markup dichiarativo per default. aspx dovrebbe essere simile al seguente:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample6.aspx)]

Come probabilmente si immaginare a questo punto, l'idea consiste nel visualizzare semplicemente la AuthenticatedMessagePanel visitatori autenticati e semplicemente il AnonymousMessagePanel ai visitatori anonimi. A tale scopo è necessario impostare le proprietà visibili di questi pannelli a seconda del fatto che l'utente è connesso o meno.

Il [Request.IsAuthenticated proprietà](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) restituisce un valore booleano che indica se la richiesta è stata autenticata. Immettere il codice seguente nella pagina\_caricare il codice del gestore eventi:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample7.vb)]

Con questo codice, visitare default. aspx con un browser. Supponendo che hai ancora per l'accesso, verrà visualizzato un collegamento alla pagina di accesso (vedere la figura 11). Fare clic su questo collegamento e accedere al sito. Come abbiamo visto nel passaggio 3, dopo aver immesso le credenziali verrà di nuovo a default. aspx, ma questa volta la pagina Mostra il Bentornato! messaggio (vedere la figura 12).


[![Quando visitare in modo anonimo, un collegamento Log viene visualizzato](an-overview-of-forms-authentication-vb/_static/image32.png)](an-overview-of-forms-authentication-vb/_static/image31.png)

**Figura 11**: Quando si visita in modo anonimo, viene visualizzato un collegamento Log ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image33.png))


[![Gli utenti autenticati sono visualizzati il Bentornato! Messaggio](an-overview-of-forms-authentication-vb/_static/image35.png)](an-overview-of-forms-authentication-vb/_static/image34.png)

**Figura 12**: Gli utenti autenticati sono visualizzati il Bentornato! Messaggio ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image36.png))


È possibile determinare l'identità dell'utente attualmente connesso tramite il [oggetto HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)del [proprietà utente](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx). L'oggetto HttpContext rappresenta le informazioni sulla richiesta corrente ed è la home page per tali oggetti ASP.NET comuni come risposta, richiesta e della sessione, tra gli altri. La proprietà utente rappresenta il contesto di sicurezza della richiesta HTTP corrente e si implementa il [interfaccia IPrincipal](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

L'utente viene impostata da FormsAuthenticationModule. In particolare, quando FormsAuthenticationModule rileva che un ticket di autenticazione form della richiesta in ingresso, crea un nuovo oggetto GenericPrincipal e assegnarlo alla proprietà utente.

Oggetti entità (ad esempio GenericPrincipal) forniscono informazioni sull'identità dell'utente e i ruoli a cui appartengono. L'interfaccia IPrincipal definisce due membri:

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) -un metodo che restituisce un valore booleano che indica se l'entità appartiene al ruolo specificato.
- [Identity](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) -una proprietà che restituisce un oggetto che implementa le [interfaccia IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). L'interfaccia IIdentity definisce tre proprietà: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx), e [nome](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

È possibile determinare il nome del visitatore corrente usando il codice seguente:

Dim currentUsersName As String = User.Identity.Name

Quando tramite autenticazione, basata su form un [FormsIdentity oggetto](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) viene creato per la proprietà Identity dell'oggetto GenericPrincipal. La classe FormsIdentity restituisce sempre i form di stringa per la proprietà AuthenticationType e True per la relativa proprietà IsAuthenticated. La proprietà nome restituisce il nome utente specificato durante la creazione di moduli di ticket di autenticazione. Oltre a queste tre proprietà, FormsIdentity include l'accesso al ticket di autenticazione sottostante tramite le [Ticket proprietà](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). La proprietà Ticket restituisce un oggetto di tipo [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), che dispone di proprietà, ad esempio scadenza, IsPersistent, IssueDate, nome e così via.

Il punto importante da sottolineare qui è che il *nomeutente* il parametro specificato nel FormsAuthentication.GetAuthCookie (*username*, *persistCookie*), SetAuthCookie (*nomeutente*, *persistCookie*) e FormsAuthentication (*username*, *persistCookie*) metodi è lo stesso valore restituito da User.Identity.Name. Inoltre, il ticket di autenticazione creato da questi metodi è disponibile eseguendo il cast di User. Identity a un oggetto FormsIdentity e quindi l'accesso alla proprietà Ticket:

Dim ident come FormsIdentity = CType (User. Identity, FormsIdentity)

Dim authTicket As FormsAuthenticationTicket = ident.Ticket

È possibile fornire un messaggio più personalizzato in default. aspx. Aggiornare la pagina\_caricare il gestore dell'evento in modo che la proprietà di testo dell'etichetta WelcomeBackMessage viene assegnata la stringa iniziale, *username*!

WelcomeBackMessage.Text = "Welcome back, " &amp; User.Identity.Name &amp; "!"

Figura 13 illustra l'effetto di questa modifica (quando si accede come utente Scott l'autorizzazione).


[![Il messaggio di benvenuto include attualmente connesso In Nome dell'utente](an-overview-of-forms-authentication-vb/_static/image38.png)](an-overview-of-forms-authentication-vb/_static/image37.png)

**Figura 13**: Il messaggio di benvenuto include nome dell'utente nel registrati attualmente ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image39.png))


### <a name="using-the-loginview-and-loginname-controls"></a>Usando i controlli di LoginName e LoginView

Visualizzazione contenuto diverso per gli utenti autenticati e anonimi è un requisito comune; quindi, viene visualizzato il nome dell'utente attualmente connesso. Per questo motivo, ASP.NET include due controlli Web che forniscono la stessa funzionalità illustrata nella figura 13, ma senza la necessità di scrivere una singola riga di codice.

Il [controllo LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) è un controllo Web basato su modello che rende più semplice visualizzare dati diversi agli utenti autenticati e anonimi. Il LoginView include due modelli predefiniti:

- AnonymousTemplate - eventuali tag aggiunto a questo modello viene visualizzato solo per i visitatori anonimi.
- LoggedInTemplate - markup di questo modello viene visualizzato solo agli utenti autenticati.

È possibile aggiungere il controllo LoginView pagina master del nostro sito site. master. Anziché aggiungere solo per il controllo LoginView, tuttavia, è possibile aggiungere entrambi un nuovo controllo ContentPlaceHolder e quindi inserire il controllo LoginView all'interno di tale nuovo ContentPlaceHolder. I motivi per questa decisione diventa evidenti a breve.

> [!NOTE]
> Oltre al AnonymousTemplate e LoggedInTemplate, il controllo LoginView può includere i modelli specifici per il ruolo. Modelli specifici per il ruolo mostrano commenti solo agli utenti che appartengono a un ruolo specificato. Verranno esaminate le funzionalità del controllo LoginView basata sui ruoli in un'esercitazione futura.


Iniziare aggiungendo un controllo ContentPlaceHolder denominato LoginContent nella pagina master all'interno di spostamento &lt;div&gt; elemento. È possibile trascinare un controllo ContentPlaceHolder semplicemente dalla casella degli strumenti alla visualizzazione origine, posizionare il tag risulta immediatamente sopra l'attività: Menu si troveranno testo.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample8.aspx)]

Successivamente, aggiungere un controllo LoginView all'interno di LoginContent ContentPlaceHolder. Il contenuto inserito in controlli ContentPlaceHolder della pagina master sono considerati *predefinito contenuto* per il controllo ContentPlaceHolder. Vale a dire, pagine ASP.NET che utilizzano questa pagina master possono specificare il proprio contenuto per ogni controllo ContentPlaceHolder o usare il contenuto predefinito della pagina master.

Nella scheda account di accesso della casella degli strumenti si trovano i LoginView e altri controlli correlati all'accesso.


[![Il controllo LoginView della casella degli strumenti](an-overview-of-forms-authentication-vb/_static/image41.png)](an-overview-of-forms-authentication-vb/_static/image40.png)

**Figura 14**: Il controllo LoginView della casella degli strumenti ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image42.png))


Successivamente, aggiungere due &lt;br /&gt; elementi immediatamente dopo il controllo LoginView, ma comunque entro il controllo ContentPlaceHolder. A questo punto, la navigazione &lt;div&gt; markup dell'elemento dovrebbe essere simile al seguente:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample9.aspx)]

È possibile definire modelli di LoginView dalla finestra di progettazione o markup dichiarativo. Progettazione di Visual Studio, espandere smart tag del LoginView, che sono elencati i modelli configurati in un elenco a discesa. Digitare il testo Hello, estraneo in AnonymousTemplate; Successivamente, aggiungere un controllo collegamento ipertestuale e impostarne le proprietà di testo e Instrumentedlink a Accedi e ~ / Login. aspx, rispettivamente.

Dopo aver configurato il modello AnonymousTemplate, passare a LoggedInTemplate e immettere il testo, "Bentornato,". Quindi trascinare un controllo LoginName dalla casella degli strumenti in LoggedInTemplate posizionandolo immediatamente dopo "Iniziale," testo. Il [controllo LoginName](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), come il nome implica, viene visualizzato il nome dell'utente attualmente connesso. Internamente, la proprietà User.Identity.Name, restituisce semplicemente il controllo LoginName

Dopo aver apportato queste aggiunte ai modelli di LoginView, il markup dovrebbe essere simile al seguente:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample10.aspx)]

Grazie a questa aggiunta alla pagina master Site. master, ogni pagina nel nostro sito Web verrà visualizzato un messaggio diverso a seconda del fatto che l'utente viene autenticato. Figura 15 viene mostrata la pagina di default. aspx quando tramite un browser visitati dall'utente Jisun. Bentornato, Jisun messaggio viene ripetuto due volte: una volta nella sezione di navigazione della pagina master a sinistra (tramite il controllo LoginView appena aggiunto) e una nella finestra di default. aspx area (tramite controlli Panel e a livello di codice per la logica) del contenuto.


[![Il LoginView controllo Visualizza Bentornato, Jisun.](an-overview-of-forms-authentication-vb/_static/image44.png)](an-overview-of-forms-authentication-vb/_static/image43.png)

**Figura 15**: Il LoginView controllo Visualizza Bentornato, Jisun. ([Fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image45.png))


Perché abbiamo aggiunto il LoginView alla pagina master, ma può essere visualizzato in ogni pagina nel sito. Tuttavia, potrebbero esserci pagine web in cui non si desidera visualizzare più questo messaggio. Una tale pagina è la pagina di accesso, perché sembra che un collegamento alla pagina di accesso esiste sul posto. Poiché il controllo LoginView è inserito in un controllo ContentPlaceHolder nella pagina master, è possibile ignorare questo tag predefinito nella nostra pagina contenuta. Aprire Login. aspx e passare alla finestra di progettazione. Poiché non è stato definito in modo esplicito un controllo contenuto in Login. aspx per LoginContent ContentPlaceHolder nella pagina master, la pagina di accesso mostrerà il master markup della pagina predefinita per questo controllo ContentPlaceHolder. È possibile verificarlo tramite la finestra di progettazione - LoginContent ContentPlaceHolder viene illustrato il markup predefinito (il controllo LoginView).


[![La pagina di accesso Mostra il valore predefinito del contenuto per LoginContent ContentPlaceHolder della pagina Master](an-overview-of-forms-authentication-vb/_static/image47.png)](an-overview-of-forms-authentication-vb/_static/image46.png)

**Figura 16**: La pagina di accesso Mostra il contenuto predefinito per the Master Page's LoginContent ContentPlaceHolder ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image48.png))


Per sostituire il markup predefinito per il LoginContent ContentPlaceHolder, semplicemente fare doppio clic sull'area nella finestra di progettazione e scegliere l'opzione Crea contenuto personalizzato dal menu di scelta rapida. (Quando utilizzando Visual Studio 2008 ContentPlaceHolder include uno smart tag che, quando selezionata, offre la stessa opzione.) Verrà aggiunto un nuovo controllo contenuto a markup della pagina e quindi ci permette di definire il contenuto personalizzato per questa pagina. È possibile aggiungere un messaggio personalizzato in questo caso, ad esempio effettuare l'accesso, ma è possibile semplicemente lasciare vuoto questo campo.

> [!NOTE]
> In Visual Studio 2005, la creazione di contenuto personalizzato crea un oggetto vuoto controllo in una pagina ASP.NET del contenuto. In Visual Studio 2008, tuttavia, la creazione di contenuto personalizzato copia il contenuto della pagina master predefinito nel controllo contenuto appena creato. Se si usa Visual Studio 2008, quindi, dopo aver creato il nuovo controllo contenuto assicurarsi di cancellare il contenuto copiato dalla pagina master.


Figura 17 mostra la pagina aspx quando visitati dal browser dopo aver apportato questa modifica. Si noti che non vi sia alcun Hello, estraneo oppure Bentornato, *nomeutente* messaggio nel riquadro di spostamento a sinistra &lt;div&gt; di quanto accade quando si visita default. aspx.


[![La pagina di accesso consente di nascondere il Markup di LoginContent ContentPlaceHolder predefinito](an-overview-of-forms-authentication-vb/_static/image50.png)](an-overview-of-forms-authentication-vb/_static/image49.png)

**Figura 17**: La pagina di accesso consente di nascondere del tipo ContentPlaceHolder predefinito LoginContent ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image51.png))


## <a name="step-5-logging-out"></a>Passaggio 5: La disconnessione

Nel passaggio 3 è stato illustrato la creazione di una pagina di accesso per l'accesso di un utente al sito, ma è ancora stato per istruzioni su come disconnettere un utente. Oltre ai metodi per la registrazione di un utente, la classe FormsAuthentication fornisce anche un [SignOut metodo](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). Il metodo SignOut elimina semplicemente il ticket di autenticazione form, in tal modo l'utente all'esterno del sito di registrazione.

Una collegamento per la disconnessione è una funzionalità comune di questo tipo di offerta che ASP.NET include un controllo appositamente progettato per disconnettere un utente. Il [controllo LoginStatus](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) viene visualizzato un controllo LinkButton account di accesso o Logout LinkButton, a seconda dello stato di autenticazione dell'utente. Viene eseguito il rendering LinkButton un account di accesso per gli utenti anonimi, mentre un Logout LinkButton viene visualizzato agli utenti autenticati. Il testo per l'account di accesso e Logout LinkButtons può essere configurato tramite il LoginStatus proprietà LoginText e LogoutText.

Facendo clic su LinkButton l'account di accesso determina un postback, da cui viene eseguito un reindirizzamento alla pagina di accesso. Facendo clic il Logout LinkButton fa sì che il controllo LoginStatus richiamare il metodo FormsAuthentication.SignOff e quindi reindirizza l'utente a una pagina. La pagina usato per l'accesso utente viene reindirizzato a dipende dalla proprietà LogoutAction, che possono essere assegnate a uno dei tre valori seguenti:

- Aggiornamento: il valore predefinito; l'utente viene reindirizzato alla pagina di che cui sono stati semplice visita. Se la pagina che sono stati semplicemente visitando non consente agli utenti anonimi, quindi FormsAuthenticationModule automaticamente reindirizzerà l'utente alla pagina di accesso.

Si vuole sapere il motivo per cui un reindirizzamento viene eseguito qui. Se l'utente desidera rimanere nella stessa pagina, il motivo per cui la necessità per il reindirizzamento esplicito? Il motivo è che quando si seleziona il Logoff LinkButton, l'utente ha ancora il ticket di autenticazione form nel relativo insieme di cookie. Di conseguenza, la richiesta di postback è una richiesta autenticata. Il controllo LoginStatus chiama il metodo di disconnessione, ma che succede dopo FormsAuthenticationModule ha autenticato l'utente. Pertanto, un reindirizzamento esplicito fa sì che il browser richiedere nuovamente la pagina. Una volta che il browser richiede nuovamente la pagina, il ticket di autenticazione form è stato rimosso e pertanto la richiesta in ingresso è anonima.

- Reindirizzamento - l'utente viene reindirizzato all'URL specificato dalla proprietà LogoutPageUrl del LoginStatus.
- RedirectToLoginPage - l'utente viene reindirizzato alla pagina di accesso.

È possibile aggiungere un controllo LoginStatus alla pagina master e configurarlo per usare l'opzione di reindirizzamento per inviare all'utente a una pagina che visualizza un messaggio di conferma che sono stati firmati. Iniziare creando una pagina nella directory radice denominata Logout. Non dimenticare di associare questa pagina della pagina master Site. master. Successivamente, immettere un messaggio nel markup della pagina che spiega all'utente che è stato disconnesso.

Successivamente, tornare alla pagina master Site. master e aggiungere un controllo LoginStatus di sotto di LoginView in LoginContent ContentPlaceHolder. Impostare proprietà del controllo LoginStatus LogoutAction reindirizzamento e la relativa proprietà LogoutPageUrl a ~/Logout.aspx.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample11.aspx)]

Poiché LoginStatus è all'esterno del controllo LoginView, esso verrà visualizzato per gli utenti anonimi e autenticati, ma che è un problema poiché LoginStatus visualizzerà correttamente un account di accesso o Logout LinkButton. Con l'aggiunta del controllo LoginStatus, il Log In collegamento ipertestuale nel AnonymousTemplate è superfluo, quindi, rimuoverlo.

Figura 18 Mostra default. aspx quando l'utente visita Jisun. Si noti che la colonna a sinistra viene visualizzato il messaggio, Bentornato, Jisun insieme a un collegamento per effettuare la disconnessione. Scegliere il LinkButton per la disconnessione causa un postback Jisun alla disconnessione del sistema e relativa viene quindi reindirizzata alla Logout. Come illustrato nella figura 19, entro l'ora di che jisun raggiunge Logout Anna è già stata effettuata la disconnessione ed è pertanto anonimo. Di conseguenza, la colonna a sinistra mostra il testo di benvenuto, estraneo e un collegamento alla pagina di accesso.


[![Default. aspx Mostra Bentornato, Jisun insieme a un controllo Logout LinkButton](an-overview-of-forms-authentication-vb/_static/image53.png)](an-overview-of-forms-authentication-vb/_static/image52.png)

**Figura 18**: Default. aspx Mostra benvenuto indietro, Jisun lungo con Logout LinkButton ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image54.png))


[![Logout Mostra iniziale, estraneo insieme a un controllo LinkButton account di accesso](an-overview-of-forms-authentication-vb/_static/image56.png)](an-overview-of-forms-authentication-vb/_static/image55.png)

**Figura 19**: Logout Mostra iniziale, estraneo insieme a un account di accesso LinkButton ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-forms-authentication-vb/_static/image57.png))


> [!NOTE]
> Consiglia di personalizzare la pagina Logout per nascondere LoginContent ContentPlaceHolder della pagina master (come abbiamo fatto per Login. aspx nel passaggio 4). Il motivo è che il LinkButton account di accesso di cui è stato eseguito il rendering tramite il controllo LoginStatus (quello di sotto di Hello, estraneo) invia l'utente alla pagina di accesso passando il parametro di stringa di query ReturnUrl all'URL corrente. In breve, se un utente che si è disconnesso fa clic su account di accesso LinkButton di questo LoginStatus e quindi accede, viene reindirizzato al Logout, che è stato facile confondere l'utente.


## <a name="summary"></a>Riepilogo

In questa esercitazione abbiamo iniziato con un esame del flusso di lavoro autenticazione form e quindi abilitata per l'implementazione di autenticazione basata su form in un'applicazione ASP.NET. Autenticazione basata su form è una tecnologia FormsAuthenticationModule, che ha due compiti: identificare gli utenti in base i ticket di autenticazione form e reindirizzando gli utenti non autorizzati alla pagina di accesso.

Classe FormsAuthentication di .NET Framework include metodi per la creazione, l'analisi e la rimozione di ticket di autenticazione form. La proprietà Request.IsAuthenticated e un oggetto utente di fornire ulteriore supporto a livello di codice per determinare se una richiesta è autenticata e informazioni sull'identità dell'utente. Esistono anche i controlli, LoginView, LoginStatus, LoginName Web e offrono agli sviluppatori un modo rapido e senza codice per l'esecuzione di molte attività comuni correlate all'accesso. Esamineremo questi e altri controlli di Web correlati all'accesso più dettagliatamente in esercitazioni future.

Questa esercitazione fornita una panoramica dell'autenticazione basata su form superficiale. È non è stato esaminare le opzioni di configurazione vengono formattati diversi, esaminare la modalità senza cookie form autenticazione tickets lavoro o esplorare come ASP.NET consente di proteggere il contenuto del ticket di autenticazione form. Verranno illustrati questi e altri in argomenti di [prossima esercitazione](forms-authentication-configuration-and-advanced-topics-vb.md).

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Modifiche tra IIS6 e sicurezza di IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Controlli ASP.NET di account di accesso](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Professionisti ASP.NET 2.0 Security, l'appartenenza e la gestione dei ruoli](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Il &lt;autenticazione&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx)
- [Il &lt;form&gt; (elemento) per &lt;autenticazione&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Formazione in video su argomenti contenuti in questa esercitazione

- [Uso dell'autenticazione basata su form di base in ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla.com, ha lavorato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola  *[Sams Teach Yourself ASP.NET 2.0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. È possibile contattarlo Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione includono Alicja Maziarz, John Suru e Teresa Murphy. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Precedente](security-basics-and-asp-net-support-vb.md)
> [Successivo](forms-authentication-configuration-and-advanced-topics-vb.md)
